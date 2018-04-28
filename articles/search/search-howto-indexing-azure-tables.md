---
title: Azure Table storage Azure Search dizini oluşturma | Microsoft Docs
description: Azure Search Azure Table storage'da depolanan verileri dizin öğrenin
author: chaosrealm
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: 6a065454e274abc9c032b0ac69f42dd72f059443
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="index-azure-table-storage-with-azure-search"></a>Azure Search dizini Azure tablo depolaması
Bu makalede Azure arama için Azure Table storage'da depolanan dizin verileri nasıl kullanılacağı gösterilmektedir.

## <a name="set-up-azure-table-storage-indexing"></a>Azure Table depolama dizin oluşturmayı ayarlama

Bu kaynakları kullanarak Azure Table depolama dizin oluşturucu ayarlayabilirsiniz:

* [Azure Portal](https://ms.portal.azure.com)
* Azure arama [REST API'si](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure arama [.NET SDK'sı](https://aka.ms/search-sdk)

Burada size REST API'sini kullanarak akışını göstermektedir. 

### <a name="step-1-create-a-datasource"></a>1. adım: bir veri kaynağı oluşturma

Dizin, veri ve Azure verilerdeki değişiklikleri verimli bir şekilde tanımlamak arama'yı etkinleştirmek ilkeleri erişmek için gerekli kimlik bilgilerini hangi verilerin bir veri kaynağını belirtir.

Tablo dizin oluşturma işlemi için veri kaynağı aşağıdaki özelliklere sahip olmanız gerekir:

- **ad** arama hizmetinizi içinde veri kaynağı benzersiz adı.
- **tür** olmalıdır `azuretable`.
- **kimlik bilgileri** parametresi depolama hesabı bağlantı dizesi içerir. Bkz: [kimlik bilgilerini belirtmeniz](#Credentials) ayrıntıları bölümü.
- **kapsayıcı** tablo adı ve isteğe bağlı bir sorgu ayarlar.
    - Tablo adı kullanarak belirtin `name` parametresi.
    - İsteğe bağlı olarak kullanarak bir sorgu belirtin `query` parametresi. 

> [!IMPORTANT] 
> Mümkün olduğunda, bir filtre PartitionKey üzerinde daha iyi performans için kullanın. Herhangi bir sorgu performansın büyük tablolar için sonuçta bir tam tablo taraması yapmaz. Bkz: [başarım düşünceleri](#Performance) bölümü.


Bir veri kaynağı oluşturmak için:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Veri kaynağı oluşturma API'si hakkında daha fazla bilgi için bkz [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-to-specify-credentials"></a>Kimlik bilgileri belirtmek için yol ####

İçin şu yöntemlerden birini kullanarak tabloya ait kimlik bilgilerini sağlayın: 

- **Tam erişim depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` giderek bağlantı dizesi Azure portalından alabilirsiniz **depolama hesabı dikey** > **ayarları**  >  **Anahtarları** (için Klasik depolama hesapları için) veya **ayarları** > **erişim anahtarları** (için Azure Resource Manager depolama hesapları).
- **Depolama hesabı paylaşılan erişim imzası bağlantı dizesi**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=t&sp=rl` paylaşılan erişim imzası listesine sahip ve Okuma izinleri kapsayıcıları (Bu durumda tablolar) ve nesne (tablo satırları) gerekir.
-  **Tablo paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<the signature>&se=<the validity end time>&sp=r` paylaşılan erişim imzası tabloda sorgu (okuma) izinleri olmalıdır.

Paylaşılan depolama hakkında daha fazla bilgi için erişim imzalar, bkz: [kullanarak paylaşılan erişim imzaları](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Paylaşılan erişim imzası kimlik bilgileri kullanıyorsanız, veri kaynağı kimlik bilgileri, zaman aşımını önlemek için yenilenen imzalarla düzenli olarak güncelleştirmeniz gerekir. Paylaşılan erişim imzası kimlik bilgilerinin süresi dolar dizin oluşturucu "Bağlantı dizesinde sağlanan kimlik bilgileri geçersiz veya süresi doldu." benzer bir hata iletisiyle başarısız olur.  

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin alanları bir belgede özniteliklerini belirtir ve arama şekil diğer yapıların karşılaşırsınız.

Bir dizin oluşturmak için:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Dizinler oluşturma hakkında daha fazla bilgi için bkz: [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>3. adım: bir dizin oluşturucu yapın
Bir dizin oluşturucu hedef arama dizin ile bir veri kaynağı bağlanır ve veri yenileme otomatikleştirmek için bir zamanlama sağlar. 

Veri kaynağı ve dizin oluşturulduktan sonra Dizin Oluşturucu oluşturmak hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizin oluşturucu, her iki saatte çalışır. (Zamanlama aralığı "PT2H" olarak ayarlanır.) 30 dakikada bir dizin oluşturucu çalıştırmak için "PT30M" aralığını ayarlayın. En kısa desteklenen aralığı beş dakikadır. Zamanlama isteğe bağlıdır; yalnızca zaman bir kez oluşturulduğunda atlanırsa, bir dizin oluşturucu çalışır. Ancak, dilediğiniz zaman isteğe bağlı bir dizin oluşturucu çalıştırabilirsiniz.   

Oluşturma dizin oluşturucu API'si hakkında daha fazla bilgi için bkz: [oluşturma dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="deal-with-different-field-names"></a>Farklı bir alan adları ile Dağıt
Bazı durumlarda, alan adları mevcut dizininizdeki tablonuz özellik adlarını farklıdır. Arama dizininize tablosundan özellik adları alan adlarıyla eşlemek için alan eşlemelerini kullanabilirsiniz. Alan eşlemeleri hakkında daha fazla bilgi için bkz: [Azure Search dizin oluşturucu alan eşlemelerini köprü veri kaynakları ve arama dizinlerini arasındaki farkları](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Tanıtıcı belge anahtarları
Azure Search'te belge anahtarını bir belge benzersiz olarak tanımlar. Tüm arama dizini tam olarak bir anahtar alanı türünün olmalıdır `Edm.String`. Anahtar alanı dizine eklenecek her belge için gereklidir. (Aslında, onu yalnızca gerekli bir alandır.)

Bileşik anahtar tablo satırları olduğu için Azure Search adlı yapay bir alan oluşturur `Key` bölüm anahtarını ve satır anahtar değerlerinin birleşimini olmasıdır. Örneğin, bir satır PartitionKey kullanıcının ise `PK1` ve RowKey `RK1`, sonra `Key` alanın değeri `PK1RK1`.

> [!NOTE]
> `Key` Değeri belge anahtarları, kısa çizgi gibi geçersiz karakterler içeriyor olabilir. Geçersiz karakter kullanarak başa `base64Encode` [alan eşleme işlev](search-indexer-field-mappings.md#base64EncodeFunction). Bunu yaparsanız, API belge anahtarları geçirme gibi arama çağırdığında URL için güvenli Base64 kodlaması de kullanmayı unutmayın.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılama
Bir zamanlamaya göre çalıştırmak için bir tablo dizin oluşturucu ayarladığınızda, bu satırın tarafından belirlendiği şekilde, yalnızca yeni veya güncelleştirilmiş satırları yeniden dizin oluşturur `Timestamp` değeri. Bir değişiklik algılama ilkesi belirtmek zorunda değilsiniz. Artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Belirli belgeleri dizinden kaldırılması gerektiğini göstermek için geçici silme stratejiyi kullanabilirsiniz. Bir satırın silinmesi yerine silindi ve veri kaynağında bir geçici silme algılama İlkesi ayarlama olduğunu belirtmek için bir özellik ekleyin. Örneğin, aşağıdaki İlkesi satır özelliğine sahipse satır silinir göz önünde bulundurur `IsDeleted` değerle `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "<query>" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   

<a name="Performance"></a>
## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Varsayılan olarak Azure arama aşağıdaki sorgu filtresi kullanır: `Timestamp >= HighWaterMarkValue`. Azure tabloları ikincil bir dizin yok olduğundan `Timestamp` alan, bu tür sorgu tam tablo taraması gerektirir ve bu nedenle büyük tablolar için yavaş sahiptir.


Burada, tablo dizin oluşturma performansı artırmak için iki olası yaklaşımlar bulunmaktadır. Bu yaklaşım her ikisi de tablo bölümlerini kullanır: 

- Verilerinizi birkaç bölüm aralığı doğal olarak bölümlenebilir varsa, bir veri kaynağı ve karşılık gelen bir dizin oluşturucu her bölüm aralığı için oluşturun. Her dizin oluşturucu, yalnızca bir belirli bir bölüm aralığı, sorgu daha iyi performans elde edilen işlemek artık sahiptir. Dizine alınması gerekir veri sabit bölümleri, daha iyi az sayıda varsa: her dizin oluşturucu yalnızca bir bölüm tarama yapar. Örneğin, anahtarları içeren bir bölüm aralığı işlemek için bir veri kaynağı oluşturmak için `000` için `100`, böyle bir sorguyu kullanın: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Verilerinizi zamanına göre bölümlendiğinde ise (örneğin, her gün veya hafta yeni bir bölüm oluşturun), aşağıdaki yaklaşımı göz önünde bulundurun: 
    - Bir sorgu biçiminde kullanın: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Kullanarak dizin oluşturucu ilerleme durumunu izleyin [alma dizin oluşturucu durumu API](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)ve düzenli aralıklarla güncelleştirme `<TimeStamp>` son başarılı yüksek su işareti değere göre sorgu koşulu. 
    - Bu yaklaşımda, bir tam yeniden dizin oluşturmaya, tetiklemek gerekiyorsa dizin oluşturucu sıfırlama yanı sıra veri kaynağı sorgusu sıfırlamanız gerekir. 


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya fikir geliştirmeleri için varsa, bunları gönderme sırasında bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
