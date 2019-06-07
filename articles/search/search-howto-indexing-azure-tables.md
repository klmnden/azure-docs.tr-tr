---
title: Azure tablo depolaması için tam metin arama - Azure Search dizini içeriği
description: Bir Azure Search Dizin Oluşturucu ile Azure tablo depolamada depolanan veriler hakkında bilgi edinin.
ms.date: 05/02/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: bca7c1b9ffe7ac0ab82f4287bba201a78fbf726a
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755077"
---
# <a name="index-azure-table-storage-with-azure-search"></a>Azure Search dizini Azure tablo depolama
Bu makalede, Azure Search için Azure tablo depolamada depolanan dizin verilerini nasıl kullanılacağını gösterir.

## <a name="set-up-azure-table-storage-indexing"></a>Azure tablo depolama dizin oluşturma ayarlayın

Bu kaynakları kullanarak Azure tablo depolama dizin oluşturucu ayarlayabilirsiniz:

* [Azure portal](https://ms.portal.azure.com)
* Azure Search'ü [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search'ü [.NET SDK'sı](https://aka.ms/search-sdk)

Burada biz REST API kullanarak akışı gösterilmektedir. 

### <a name="step-1-create-a-datasource"></a>1. adım: Veritabanı oluşturma

Bir veri kaynağı, hangi verilerin dizin için veriler ve verilerdeki değişikliklerin etkili bir şekilde tanımlamak Azure arama'yı etkinleştirmek ilkeleri erişmek için gerekli kimlik bilgilerini belirtir.

Tablo dizin oluşturma işlemi için veri kaynağı aşağıdaki özelliklere sahip olmanız gerekir:

- **adı** veri kaynağı, arama hizmetinizin içinde benzersiz adıdır.
- **tür** olmalıdır `azuretable`.
- **kimlik bilgileri** parametresi depolama hesabı bağlantı dizesi içerir. Bkz: [kimlik bilgilerini belirtmeniz](#Credentials) ayrıntıları bölümü.
- **kapsayıcı** tablo adını ve isteğe bağlı bir sorgu ayarlar.
    - Tablo adını kullanarak belirtin `name` parametresi.
    - İsteğe bağlı olarak, bir sorgu kullanarak belirtmeniz `query` parametresi. 

> [!IMPORTANT] 
> Bir filtre, mümkün olduğunda PartitionKey üzerinde daha iyi performans için kullanın. Herhangi bir sorgu büyük tablolar için kötü performans elde edilen bir tam tablo tarama yapar. Bkz: [performansla ilgili önemli noktalar](#Performance) bölümü.


Bir veri kaynağı oluşturmak için:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Veri kaynağı oluşturma API'si hakkında daha fazla bilgi için bkz. [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-to-specify-credentials"></a>Kimlik bilgilerini belirtmek için yol ####

Bu şekilde tablosu için kimlik bilgileri sağlayabilirsiniz: 

- **Tam erişim depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` Giderek bağlantı dizesini Azure portalından alabilirsiniz **depolama hesabı dikey** > **ayarları** > **anahtarları** (Klasik için Depolama hesapları için) veya **ayarları** > **erişim anahtarları** (için Azure Resource Manager depolama hesaplarında).
- **Depolama hesabı, paylaşılan erişim imzası bağlantı dizesi**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=t&sp=rl` Paylaşılan erişim imzası, listesine sahip ve kapsayıcı (Bu durumda tablolar) ve nesne (tablo satırları) izinlerini okuyun.
-  **Tablo paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<the signature>&se=<the validity end time>&sp=r` Paylaşılan erişim imzası tablosunda (okuma) sorgu izinleri olmalıdır.

Paylaşılan depolama hakkında daha fazla bilgi için erişim imzaları, bkz: [paylaşılan erişim imzaları kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Paylaşılan erişim imzası kimlik bilgileri kullanıyorsanız veri kaynağı kimlik bilgileri düzenli aralıklarla yenilenen imzaları ile kendi zaman aşımını önlemek için güncelleştirme gerekir. Paylaşılan erişim imzası kimlik bilgilerinin süresi dolar, dizin oluşturucu "Bağlantı dizesinde sağlanan kimlik bilgileri geçersiz veya süresi dolmuş." benzer bir hata iletisiyle başarısız olur  

### <a name="step-2-create-an-index"></a>2. adım: Dizin oluşturma
Dizin alanları bir belgede, öznitelikleri belirtir ve arama şekil diğer yapıları karşılaşırsınız.

Bir dizin oluşturmak için:

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Dizinler oluşturma ile ilgili daha fazla bilgi için bkz: [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>3. adım: Dizin oluşturucu oluşturma
Bir dizin oluşturucu, bir veri kaynağı ile bir hedef arama dizinine bağlar ve veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar. 

Veri kaynağı ve dizin oluşturulduktan sonra Dizin Oluşturucu oluşturmaya hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizin oluşturucu her iki saatte bir çalışır. ("PT2H için" zamanlama aralığı ayarlanır.) 30 dakikada bir dizin oluşturucu çalıştırmak için "PT30M" için aralığı ayarlayın. Desteklenen en kısa aralık beş dakikadır. Zamanlama isteğe bağlıdır; yalnızca ne zaman oluşturulduktan sonra atlanırsa, dizin oluşturucu çalıştırır. Ancak, herhangi bir zamanda isteğe bağlı bir dizin oluşturucu çalıştırabilirsiniz.   

Dizin Oluşturucu Oluşturma API'si hakkında daha fazla bilgi için bkz. [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Dizin Oluşturucu zamanlamaları tanımlama hakkında daha fazla bilgi için bkz. [dizin oluşturucular için Azure Search zamanlama](search-howto-schedule-indexers.md).

## <a name="deal-with-different-field-names"></a>Farklı alan adları ile Dağıt
Bazı durumlarda, mevcut dizininizi alan adlarını tablonuzdaki özelliği adlarından farklıdır. Arama dizininizdeki tablosundan özellik adları alan adlarıyla eşlemek için alan eşlemelerini kullanabilirsiniz. Alan eşlemeleri hakkında daha fazla bilgi için bkz: [Azure Search dizin oluşturucu alan eşlemelerini köprü veri kaynakları ve arama dizinleri arasındaki farklar](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Tanıtıcı belge anahtarları
Azure Search'te belge anahtarını, bir belgeyi benzersiz olarak tanımlar. Her arama dizini türünde tam olarak bir anahtar alan olmalıdır `Edm.String`. Dizine eklenecek her belge için anahtar alanı gereklidir. (Aslında, yalnızca gerekli bir alandır.)

Tablo satırları bir bileşik anahtarı olduğundan, Azure Search adlı yapay bir alan oluşturur `Key` bölüm anahtarını ve satır anahtarı değerlerinin bir birleşimini olmasıdır. Örneğin, bir satır PartitionKey's ise `PK1` ve olup RowKey `RK1`, ardından `Key` alanın değeri `PK1RK1`.

> [!NOTE]
> `Key` Değer tire gibi belge anahtarları geçersiz karakterler içeriyor olabilir. İle geçersiz karakterler kullanarak giderebilirsiniz `base64Encode` [alan eşlemesi işlevi](search-indexer-field-mappings.md#base64EncodeFunction). Bunu yaparsanız, ayrıca belge anahtarları API'SİNDE geçirme gibi arama çağırdığında URL güvenli Base64 kodlaması kullandığınızdan emin olun.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılama
Bir zamanlamaya göre çalıştırılacak bir tablo Dizin Oluşturucu ' ayarladığınızda, bu satırın tarafından belirlendiği yalnızca yeni veya güncelleştirilmiş satırları yeniden dizin oluşturur `Timestamp` değeri. Bir değişiklik algılama İlkesi belirtmeniz gerekmez. Artımlı dizin sizin için otomatik olarak etkinleştirilir.

Bazı belgeler dizinden kaldırılması gerektiğini belirtmek için bir geçici silme stratejiyi kullanabilirsiniz. Satır silme yerine silindi ve veri kaynağı bir geçici silme algılama İlkesi ayarlama olduğunu belirtmek için bir özellik ekleyin. Örneğin, şu ilkeyi satır bir özelliği varsa, bir satır silinmeden göz önünde bulundurur `IsDeleted` değerle `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2019-05-06
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

Varsayılan olarak, Azure Search aşağıdaki sorgu filtresi kullanır: `Timestamp >= HighWaterMarkValue`. Azure tabloları ikincil dizin yok çünkü `Timestamp` alan, bu tür bir sorgu tam tablo taraması gerektiriyor ve bu nedenle büyük tablolar için yavaştır.


Tablo dizin oluşturma performansı artırmak için iki olası yaklaşımlar aşağıda verilmiştir. Bu yaklaşımların her ikisi de tablo bölümleri kullanır: 

- Verilerinizi birkaç bölüm aralığı doğal olarak bölümlenebilir, bir veri kaynağı ve her bölüm aralığı için karşılık gelen bir dizin oluşturucu oluşturun. Her dizin oluşturucu, yalnızca bir bölüme aralığı, sorguyu daha iyi performans elde edilen işlemek artık sahiptir. Sıralanacak gereken verileri sabit bölümler, daha az sayıda varsa: her dizin oluşturucu yalnızca bir bölüm tarama yapar. Örneğin, anahtarları içeren bir bölüm aralığı işlemek için bir veri kaynağını oluşturmak için `000` için `100`, bu gibi bir sorguda kullanın: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Verilerinizi zamanına göre bölümlendiğinde ise (örneğin, her gün veya hafta yeni bir bölüm oluşturun), aşağıdaki yaklaşımı göz önünde bulundurun: 
    - Bir sorgu formun: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Kullanarak dizin oluşturucu ilerlemesini izlemek [alma dizin oluşturucu durum API'si](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)ve düzenli olarak güncelleştirirsiniz `<TimeStamp>` son başarılı yüksek su işareti değerine göre sorgu koşulu. 
    - Bu yaklaşımda, bir tam ölçeklemek, tetiklemek gerekirse dizin oluşturucu sıfırlama yanı sıra veri kaynağı sorgu sıfırlamanız gerekir. 


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya geliştirmeleri için fikriniz varsa, bunları gönderme sırasında bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
