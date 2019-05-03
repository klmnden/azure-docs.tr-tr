---
title: Azure Blob Depolama içeriği için tam metin arama - Azure Search dizini
description: Azure Blob Depolama dizin ve Azure Search belgelerden metni Ayıkla hakkında bilgi edinin.
ms.date: 05/02/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: e55d596cfaf34c177f6dc43c27aaac37da87d2f7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024883"
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Azure arama ile Azure Blob Depolama'da belgelerin dizin oluşturma
Bu makalede, belgelerin dizinini oluşturmak için Azure Search kullanma gösterilmektedir (PDF gibi Microsoft Office belge ve diğer birçok ortak biçimleri) Azure Blob Depolama alanında depolanır. İlk olarak ayarlama ve blob dizin oluşturucu yapılandırma temellerini açıklar. Ardından, davranışların bir daha ayrıntılı keşfi sunar ve karşılaşabileceğiniz olası senaryolar.

## <a name="supported-document-formats"></a>Belge biçimleri desteklenir
Blob dizin oluşturucu aşağıdaki belge biçimlerinden metin ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>BLOB dizin oluşturma ayarlama
Bir Azure Blob Depolama Dizin Oluşturucu kullanarak ayarlayabilirsiniz:

* [Azure portal](https://ms.portal.azure.com)
* Azure Search'ü [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search'ü [.NET SDK'sı](https://aka.ms/search-sdk)

> [!NOTE]
> Bazı özellikler (örneğin, alan eşlemelerini) henüz portalda kullanılabilir değildir ve programlı olarak kullanılması gerekir.
>

Burada, REST API kullanarak akışı gösterilmektedir.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Bir veri kaynağı, hangi veri dizini için (yeni, değiştirilen veya Silinen satırlar) verilerdeki değişiklikleri verimli bir şekilde tanımlamak için ilkeler ve veri erişmek için gerekli kimlik bilgilerini belirtir. Bir veri kaynağı, aynı arama hizmetinde birden çok dizin oluşturucular tarafından kullanılabilir.

Veri kaynağı, blob dizin oluşturma işlemi için aşağıdaki gerekli özelliklerde sahip olmanız gerekir:

* **adı** veri kaynağı, arama hizmetinizin içinde benzersiz adıdır.
* **tür** olmalıdır `azureblob`.
* **kimlik bilgileri** olarak depolama hesabı bağlantı dizesi sağlar `credentials.connectionString` parametresi. Bkz: [kimlik bilgilerini belirleme konusunda](#Credentials) altındaki ayrıntılar için.
* **kapsayıcı** depolama hesabınızdaki bir kapsayıcıya belirtir. Varsayılan olarak, kapsayıcıdaki tüm blobları alınabilir. Yalnızca belirli bir sanal dizin içinde dizini bloblara istiyorsanız, isteğe bağlı kullanarak bu dizine belirtebilirsiniz **sorgu** parametresi.

Bir veri kaynağı oluşturmak için:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Veri kaynağı oluşturma API'si hakkında daha fazla bilgi için bkz. [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Kimlik bilgilerini belirtme ####

Aşağıdaki yöntemlerden biriyle blob kapsayıcısında için kimlik bilgileri sağlayabilirsiniz:

- **Tam erişim depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` Depolama hesabı dikey penceresine giderek bağlantı dizesini Azure portalından alabilirsiniz > Ayarlar > tuşları (Klasik depolama hesapları) ya da ayarlar > erişim anahtarları (için Azure Resource Manager depolama hesaplarında).
- **Depolama hesabı paylaşılan erişim imzası** (SAS) bağlantı dizesi: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` SAS listesine sahip ve Okuma izinlerine kapsayıcılar ve nesneler (Bu durumda blobları).
-  **Kapsayıcı paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` SAS listesine sahip ve okuma kapsayıcı üzerindeki izinleri gerekir.

Paylaşılan depolama hakkında daha fazla bilgi için erişim imzaları, bkz: [paylaşılan erişim imzaları kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> SAS kimlik bilgileri kullanıyorsanız, veri kaynağı kimlik bilgileri düzenli aralıklarla yenilenen imzaları ile kendi zaman aşımını önlemek için güncelleştirme gerekir. SAS kimlik bilgilerinin süresi dolar, dizin oluşturucu için benzer bir hata iletisiyle başarısız olur `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Bir belgede, öznitelikler, alanları dizinini belirtir ve arama şekil diğer yapıları karşılaşırsınız.

Aranabilir bir dizin oluşturmak nasıl işte `content` bloblarından ayıklanan metinleri saklamak için alan:   

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Dizin oluşturma hakkında daha fazla bilgi için bkz. [dizin oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>3. Adım: Dizin oluşturucu oluşturma
Bir dizin oluşturucu, bir veri kaynağı ile bir hedef arama dizinine bağlar ve veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar.

Dizinin ve veri kaynağının oluşturulan dizin oluşturucu oluşturmaya hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizin oluşturucu ("PT2H için" zamanlama aralığı ayarlanır) iki saatte çalışır. 30 dakikada bir dizin oluşturucu çalıştırmak için "PT30M" için aralığı ayarlayın. Kısa desteklenen aralığı 5 dakikadır. Zamanlama atlanırsa isteğe bağlıdır, dizin oluşturucu yalnızca oluştururken bir kez çalışır. Ancak, bir dizin oluşturucu isteğe bağlı herhangi bir zamanda çalıştırabilirsiniz.   

Dizin Oluşturucu Oluşturma API'si hakkında daha fazla ayrıntı için kullanıma [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Azure Search BLOB'ları nasıl dizinler?

Yapılandırmanıza bağlı olarak [dizin oluşturucu yapılandırmasını](#PartsOfBlobToIndex), blob dizin oluşturucu yalnızca depolama meta verileri dizinleyebilirsiniz (yararlı hakkındaki meta veriler yalnızca sizin ve BLOB içeriğinin dizinini gerekmez), depolama ve içerik meta verileri veya hem meta veriler ve metin içeriği. Varsayılan olarak, dizin oluşturucu, hem meta veriler hem de içerik ayıklar.

> [!NOTE]
> Varsayılan olarak, JSON veya CSV gibi yapılandırılmış içeriği BLOB'ları, tek bir metin parçası dizine eklenir. Yapılandırılmış bir biçimde JSON ve CSV bloblarını dizine eklemek istiyorsanız bkz [dizin JSON BLOB'ları](search-howto-index-json-blobs.md) ve [dizin CSV BLOB'ları](search-howto-index-csv-blobs.md) daha fazla bilgi için.
>
> Bileşik veya katıştırılmış bir belge (örneğin, bir ZIP arşivi veya Word belgesi ekleri katıştırılmış Outlook e-posta ile), ayrıca tek bir belge olarak dizine alınır.

* Belge metin içeriği adlı bir dize alanı ayıklanan `content`.

> [!NOTE]
> Azure arama, fiyatlandırma katmanına bağlı olarak ayıklar ne kadar metin sınırları: 32.000 karakter ücretsiz katmanı, 64.000 temel ve standart, standart S2 ve standart S3 katmanları için 4 milyon. Bir uyarı kesilmiş belgeler için dizin oluşturucu durumu yanıtına dahil edilir.  

* Kullanıcı tanımlı meta veri özelliklerini blob üzerinde mevcut varsa, aynen ayıklanır.
* Standart blob meta veri özelliklerini şu alanlara ayıklanır:

  * **meta veri\_depolama\_adı** (Edm.String) - blob, dosya adı. Örneğin, bir blob /my-container/my-folder/subfolder/resume.pdf varsa, bu alan değeri `resume.pdf`.
  * **meta veri\_depolama\_yolu** (Edm.String) - BLOB Depolama hesapları dahil olmak üzere, tam URI. Örneğin, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **meta veri\_depolama\_içeriği\_türü** (Edm.String) - içerik blob karşıya yüklemek için kullanılan kod tarafından belirtilen türü. Örneğin, `application/octet-stream`.
  * **meta veri\_depolama\_son\_değiştiren** son değiştirme (Edm.DateTimeOffset) - blob için zaman damgası. Azure arama, her şeyi ilk dizinleme sonra ölçeklemek önlemek için değiştirilen blobları tanımlamak için bu zaman damgası kullanır.
  * **meta veri\_depolama\_boyutu** (EDM.Int64) - blob bayt cinsinden boyutu.
  * **meta veri\_depolama\_içeriği\_md5** (Edm.String) - blob içeriğinin varsa MD5 karma değeri.
* Her belge biçimi için özel meta veri özelliklerini listelenen alanlarına ayıklanan [burada](#ContentSpecificMetadata).

Yukarıdaki özelliklerin tümü için alanları search dizininizi tanımlama - yalnızca uygulamanız için gereken özellikleri yakalama gerekmez.

> [!NOTE]
> Genellikle, alan adları varolan dizininize belge ayıklama sırasında oluşturulan alan adlarından farklı olacaktır. Kullanabileceğiniz **alan eşlemeleri** arama dizininizdeki alan adları için Azure Search tarafından sağlanan özellik adlarını eşlemek için. Alan eşlemelerini aşağıda kullanın örneği görürsünüz.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Belge anahtarları ve alan eşlemelerini tanımlama
Azure Search'te belge anahtarını, bir belgeyi benzersiz olarak tanımlar. Her arama dizini Edm.String türünde tam olarak bir anahtar alan olması gerekir. (Bu gerçekten gerekli olan tek alan) dizine eklenen her belge için anahtar alanı gereklidir.  

Ayıklanan alanı anahtar alan dizininiz için eşlemelisiniz dikkatle göz önünde bulundurmalısınız. Adaylar:

* **meta veri\_depolama\_adı** - bu uygun bir aday olabilir, ancak farklı klasörlerde aynı ada ve (2) adı ile BLOB'ları sahip (1) adları benzersiz olmayabilir, Not karakterleri içerebilir çizgi gibi belge anahtarları geçersiz. İle geçersiz karakterler kullanarak giderebilirsiniz `base64Encode` [alan eşlemesi işlevi](search-indexer-field-mappings.md#base64EncodeFunction) - bunu yaptığınızda, arama gibi bunları geçirmeden API'SİNDE çağırdığında, belge anahtarlar kodlayın unutmayın. (Örneğin,. NET'te kullanabilirsiniz [UrlTokenEncode yöntemi](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) bu amaç için).
* **meta veri\_depolama\_yolu** - tam yolunu kullanarak benzersizlik sağlar, ancak kesinlikle yolunu içeren `/` karakterler [geçersiz bir belge anahtarında](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Yukarıdaki olarak kullanarak anahtarlarını kodlama seçeneğiniz `base64Encode` [işlevi](search-indexer-field-mappings.md#base64EncodeFunction).
* Yukarıdaki seçeneklerin hiçbiri işinize yaramazsa blobları için özel meta veri özelliği ekleyebilirsiniz. Ancak, bu seçenek için tüm blobları bu meta veri özellik eklemek için blob karşıya yükleme işlemi gerektirir. Anahtar gerekli bir özellik olduğundan, bu özelliğe sahip olmayan tüm bloblar dizine başarısız olur.

> [!IMPORTANT]
> Azure Search dizini anahtar alanında açık bir eşleme yok ise, otomatik olarak kullanan `metadata_storage_path` anahtar değerlerini (ikinci seçenek yukarıdaki) anahtar ve base-64 olarak kodlar.
>
>

Bu örnekte, şimdi çekme `metadata_storage_name` alan belge anahtarı olarak. Ayrıca dizininizi sahip adlandırılmış bir anahtar alan varsayalım `key` ve bir alan `fileSize` belge boyutunu depolamak için. İstediğiniz gibi işlemleri wire için oluştururken veya oluşturucunuz güncelleştirirken aşağıdaki alan eşlemelerini belirtin:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Çıkarmasını ister bütün, alan eşlemelerini ekleyin ve var olan bir dizin oluşturucu için anahtarların base-64 kodlamasını etkinleştirmek nasıl şöyledir:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> Alan eşlemeleri hakkında daha fazla bilgi için bkz: [bu makalede](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Hangi BLOB'ları dizine denetleme
Hangi BLOB'ları dizini oluşturulur ve hangi atlanır denetleyebilirsiniz.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Yalnızca belirli dosya uzantılarına sahip bloblarını dizinleme
Yalnızca dosya adı uzantılarını kullanarak belirttiğiniz blob'larla dizinleyebilirsiniz `indexedFileNameExtensions` dizin oluşturucu yapılandırma parametresi. (Sahip, önde gelen bir nokta) dosya uzantılarının virgülle ayrılmış listesini içeren bir dize değeridir. Örneğin, yalnızca dizin için. PDF ve. DOCX BLOB'ları bu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Belirli dosya uzantılarını blob'larla Dışla
Belirli dosya adı uzantıları ile BLOB'ları kullanarak dizine elmadan hariç tutabilirsiniz `excludedFileNameExtensions` yapılandırma parametresi. (Sahip, önde gelen bir nokta) dosya uzantılarının virgülle ayrılmış listesini içeren bir dize değeridir. Örneğin, tüm BLOB'ları ile hariç dizin. PNG ve. JPEG uzantıları, bu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Her iki `indexedFileNameExtensions` ve `excludedFileNameExtensions` parametreleri, ilk Azure Search bakar `indexedFileNameExtensions`, ardından adresindeki `excludedFileNameExtensions`. Bu, her iki listede de aynı dosya uzantısı varsa, dizine elmadan hariç tutulacaktır olduğunu anlamına gelir.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Hangi blob bölümlerinin dizine denetleme

Hangi BLOB bölümlerinin kullanılarak dizinlenir denetleyebilirsiniz `dataToExtract` yapılandırma parametresi. Bunu yapmak için şu değerleri alabilir:

* `storageMetadata` -belirtir, yalnızca [standart blob özelliklerini ve kullanıcı tanımlı meta veriler](../storage/blobs/storage-properties-metadata.md) dizinlenir.
* `allMetadata` -Depolama meta verilerin belirtir ve [içerik türü belirli meta veriler](#ContentSpecificMetadata) ayıklanan blob içeriği dizin bulunur.
* `contentAndMetadata` -tüm meta veriler ve metinsel içeriği blobundan ayıklanan dizini belirtir. Varsayılan değer budur.

Örneğin, yalnızca depolama meta verileri dizine kullanın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Blobların nasıl dizinleneceğini denetlemek için BLOB meta verilerini kullanma

Yukarıda açıklanan yapılandırma parametreleri tüm bloblara uygulanır. Bazı durumlarda, denetlemek isteyebilir nasıl *tek tek bloblar* dizinlenir. Aşağıdaki blob meta veri özelliklerini ve değerlerini ekleyerek bunu yapabilirsiniz:

| Özellik adı | Özellik değeri | Açıklama |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Blob tamamen atlamak için blob dizin oluşturucu bildirir. Meta veriler ya da içerik ayıklama denenir. Belirli bir blob art arda başarısız olur ve dizin oluşturma işlemi kesintiye uğratır bu yararlı olur. |
| AzureSearch_SkipContent |"true" |Bu, eşdeğerdir `"dataToExtract" : "allMetadata"` açıklanan ayarı [yukarıda](#PartsOfBlobToIndex) belirli bir bloba kapsamlı. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Hatalarıyla ilgilenme

Varsayılan olarak, blob dizin oluşturucu, bir blob desteklenmeyen bir içerik türüyle (örneğin, bir görüntü) karşılaştığında hemen durdurur. Elbette kullanabileceğiniz `excludedFileNameExtensions` belirli içerik türlerini atlama parametresi. Ancak, tüm olası içerik türlerini önceden bilmeden dizin blob'lara gerekebilir. Desteklenmeyen bir içerik türü ile karşılaşıldığında dizin oluşturma devam etmek için ayarlama `failOnUnsupportedContentType` yapılandırma parametresi `false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Bazı BLOB'lar için Azure Search içerik türü belirlenemiyor veya bir belgenin işlenemedi. Aksi takdirde içerik türü desteklenen. Bu hata modu yoksayacak şekilde ayarlayın `failOnUnprocessableDocument` yapılandırma parametresi yanlış:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

Azure arama dizini BLOB boyutu sınırlar. Bu sınırlar içinde belirtilmiştir [Azure Search'te hizmet sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity). Büyük boyutlu BLOB'ları, varsayılan olarak hata olarak kabul edilir. Ancak, ayarlarsanız büyük boyutlu blobları depolama meta verilerini hala dizinleyebilirsiniz `indexStorageMetadataOnlyForOversizedDocuments` yapılandırma parametresi true: 

    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }

Hataları işleme, BLOB'ları ayrıştırılırken ya da belgeler için dizin eklerken, herhangi bir noktada görülüyorsa dizin oluşturma devam edebilirsiniz. Belirli bir sayıya hataların yoksayacak şekilde ayarlayın `maxFailedItems` ve `maxFailedItemsPerBatch` istenen değerleri yapılandırma parametreleri. Örneğin:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılama
Bir zamanlamaya göre çalıştırılacak bir blob dizin oluşturucu ' ayarladığınızda, onu yalnızca değiştirilen blobları, blob tarafından belirlendiği reindexes `LastModified` zaman damgası.

> [!NOTE]
> Bir değişiklik algılama İlkesi belirtmeniz gerekmez; Artımlı dizin sizin için otomatik olarak etkinleştirilir.

Belgelerini silmeyi desteklemek için "geçici silme" bir yaklaşım kullanın. Blobları yükseltebilir silerseniz, ilgili belgeleri arama dizinden kaldırılmaz. Bunun yerine, aşağıdaki adımları kullanın:  

1. Azure Search'e mantıksal olarak silinip silinmediğini belirtmek için blob için özel meta veri özellik ekleme
2. Veri kaynağında bir geçici silme algılama ilkesi yapılandırma
3. Dizin Oluşturucu (Dizin Oluşturucu durum API'si tarafından gösterilen şekilde) blob işlediğinde, fiziksel blob silebilirsiniz

Örneğin, bir blobun bir metadata özelliğine sahip silinmesi şu ilkeyi göz önünde bulundurur `IsDeleted` değerle `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Büyük veri kümelerini dizin oluşturma

Bloblarını dizine ekleme, zaman alıcı bir işlem olabilir. BLOB'ları dizin milyonlarca sahip olduğu durumlarda, verilerinizi bölümlemeyi ve paralel verileri işlemek için birden çok dizin oluşturucuları kullanarak dizin oluşturmayı hızlandırabilir. İşte nasıl bunu ayarlayabilirsiniz:

- Verilerinizi birden çok blob kapsayıcıları veya sanal klasörler bölümlere ayırma
- Çeşitli Azure Search veri kaynakları, bir kapsayıcının veya klasörün başına ayarlayın. Blob klasörüne işaret edecek şekilde kullanma `query` parametresi:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Her bir veri kaynağına karşılık gelen bir dizin oluşturucu oluşturun. Tüm Dizin oluşturucuların aynı hedef arama dizinine işaret edebilir.  

- Hizmetinizde bir arama birimi bir dizin oluşturucu, belirli bir zamanda çalıştırabilirsiniz. Yukarıda açıklandığı gibi birden çok dizin oluşturucu oluşturma yalnızca gerçekten paralel olarak çalıştırırsanız yararlı olur. Birden çok dizin oluşturucu paralel olarak çalıştırmak için uygun sayıda bölümleri ve çoğaltmalarını oluşturarak arama hizmetinizi ölçeklendirin. Arama hizmetinizi 6 arama birimleri (örneğin, 2 bölüm x 3 çoğaltmalar) varsa, örneğin, ardından 6 dizin oluşturucular eşzamanlı olarak dizin oluşturma performansı six-fold bir artış výsledek çalıştırabilirsiniz. Ölçekleme ve kapasite planlaması hakkında daha fazla bilgi için bkz: [sorgu ve iş yüklerini Azure Search'te dizin oluşturma için kaynak düzeylerini ölçeklendirme](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>İlgili verileri yanı sıra dizin oluşturma belgeleri

"Belgeleri dizininize birden çok kaynaktan bir araya getirmek" isteyebilirsiniz. Örneğin, Cosmos DB'de depolanan diğer meta veriler BLOB metin birleştirmek isteyebilirsiniz. Birden fazla bölümü arama belgeleri oluşturulacak API dizin birlikte çeşitli oluşturucular anında iletme bile kullanabilirsiniz. 

Bunun işe yaraması için tüm dizin oluşturucuların ve diğer bileşenleri belge anahtarı kabul etmeniz gerekir. Ayrıntılı bir Rehber için bu dış makaleye bakın: [Azure Search'te diğer verilerle belgeleri birleştirmeniz](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Dizin oluşturma düz metin 

Tüm bloblar aynı kodlamada düz metin içeriyorsa, önemli ölçüde dizin oluşturma kullanarak performansı **metni ayrıştırma modu**. Ayrıştırma modu metin kullanmak için ayarlanmış `parsingMode` yapılandırma özelliğini `text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Varsayılan olarak, `UTF-8` kodlama olduğu varsayılır. Farklı bir kodlama belirtmek için kullanın `encoding` yapılandırma özellik: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>İçerik türe özgü meta veriler özellikleri
Aşağıdaki tabloda her belge biçimi için yapılan işleme özetler ve Azure Search tarafından ayıklanan meta veri özelliklerini açıklar.

| Belge biçimi / içerik türü | İçerik türü belirli meta veri özelliklerini | İşleme ayrıntıları |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Şerit HTML biçimlendirmesi ve metni Ayıkla |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Ekli belgelerin (görüntüleri hariç) dahil olmak üzere, metni Ayıkla |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Ekli belgelerin dahil olmak üzere, metni Ayıkla |
| DOC (uygulama/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Ekli belgelerin dahil olmak üzere, metni Ayıkla |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Ekli belgelerin dahil olmak üzere, metni Ayıkla |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Ekli belgelerin dahil olmak üzere, metni Ayıkla |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Ekli belgelerin dahil olmak üzere, metni Ayıkla |
| PPT (uygulama/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Ekli belgelerin dahil olmak üzere, metni Ayıkla |
| MSG (uygulama/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Ekleri dahil olmak üzere, metni Ayıkla |
| ZIP (uygulama/zip'i) |`metadata_content_type` |Arşivdeki tüm belgelerden metni Ayıkla |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |XML işaretlemesini kaldırın ve metni Ayıkla |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Metin ayıklama<br/>NOT: JSON blobu birden çok belge alanlarını ayıklamak ihtiyacınız varsa bkz [dizin JSON blobları](search-howto-index-json-blobs.md) Ayrıntılar için |
| EML (ileti/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Ekleri dahil olmak üzere, metni Ayıkla |
| RTF (uygulama/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Metin ayıklama|
| Düz metin (metin/düz) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Metin ayıklama|


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya geliştirmeleri için fikriniz varsa bize üzerinde bildirin bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
