---
title: "Azure Blob Depolama Azure Search dizini oluşturma"
description: "Azure Blob Storage dizin ve Azure Search belgeleri metin Al hakkında bilgi edinin"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 2a5968f4-6768-4e16-84d0-8b995592f36a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/22/2017
ms.author: eugenesh
ms.openlocfilehash: 97c1fc602ba27472fed2f11fd634e617ae9c636f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Azure arama ile Azure Blob Storage belgelerde dizin oluşturma
Bu makalede Azure Search dizini belgeler için kullanma gösterilmektedir (PDF gibi Microsoft Office belgelerini ve diğer birçok ortak biçimleri) Azure Blob depolama alanına depolanır. İlk olarak, ayarlama ve blob dizin oluşturucu yapılandırma temellerini açıklar. Ardından, derin keşif davranışı sunar ve karşılaşabileceğiniz olası senaryolar.

## <a name="supported-document-formats"></a>Desteklenen Belge biçimleri
Blob dizin oluşturucu metin aşağıdaki belge biçimlerinden ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>BLOB dizin oluşturmayı ayarlama
Azure Blob Storage bir kullanarak dizin oluşturucu ayarlayabilirsiniz:

* [Azure portal](https://ms.portal.azure.com)
* Azure arama [REST API'si](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure arama [.NET SDK'sı](https://aka.ms/search-sdk)

> [!NOTE]
> Bazı özellikler (örneğin, alan eşlemelerini) henüz portalda kullanılabilir değildir ve programlı olarak kullanılması gerekir.
>
>

Burada, REST API kullanarak akışı gösterilmektedir.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Dizin, veri ve (yeni, değiştirilen veya silinen satır) verilerdeki değişiklikleri verimli bir şekilde tanımlamak için ilkeler erişmek için gerekli kimlik bilgilerini hangi verilerin bir veri kaynağını belirtir. Bir veri kaynağı, aynı arama hizmeti birden çok dizin oluşturucular tarafından kullanılabilir.

BLOB dizini oluşturma için veri kaynağı aşağıdaki gereken özellikleri sahip olmanız gerekir:

* **ad** arama hizmetinizi içinde veri kaynağının benzersiz bir addır.
* **tür** olmalıdır `azureblob`.
* **kimlik bilgileri** depolama hesabı bağlantı dizesi olarak sağlar `credentials.connectionString` parametresi. Bkz: [kimlik bilgilerini belirtmek nasıl](#Credentials) aşağıda Ayrıntılar için.
* **kapsayıcı** depolama hesabınızdaki bir kapsayıcı belirtir. Varsayılan olarak, kapsayıcıdaki tüm blob'lara alınabilir. Yalnızca belirli bir sanal dizin içinde dizini BLOB'lar etmek istiyorsanız, isteğe bağlı kullanarak bu dizini belirtebilirsiniz **sorgu** parametresi.

Bir veri kaynağı oluşturmak için:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Veri kaynağı oluşturma API'si hakkında daha fazla bilgi için bkz [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Kimlik bilgilerini belirtme ####

Şu yöntemlerden birini kullanarak blob kapsayıcısında için kimlik bilgilerini sağlayın:

- **Tam erişim depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. Bağlantı dizesi için depolama hesabı dikey giderek Azure portalından elde edebilirsiniz > Ayarlar > tuşları (Klasik depolama hesapları) ya da ayarlar > erişim anahtarları (için Azure Resource Manager depolama hesapları).
- **Depolama hesabı paylaşılan erişim imzası** (SAS) bağlantı dizesi: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` SAS listesine sahip ve kapsayıcıları ve nesneleri üzerinde okuma (Bu durumda BLOB ').
-  **Kapsayıcı paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` SAS listesine sahip ve okuma kapsayıcısı üzerinde izinleri gerekir.

Paylaşılan depolama hakkında daha fazla bilgi için erişim imzalar, bkz: [kullanarak paylaşılan erişim imzaları](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> SAS kimlik bilgileri kullanıyorsanız, veri kaynağı kimlik bilgileri düzenli aralıklarla tarihlerinden önlemek için yenilenen imzalarla güncelleştirmeniz gerekir. SAS kimlik bilgilerinin süresi dolar dizin oluşturucu için benzer bir hata iletisi ile başarısız olur `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin öznitelikleri, belgedeki alanları belirtir ve arama şekil diğer yapıların karşılaşırsınız.

Bir dizin ile aranabilir oluşturma işte `content` bloblarından ayıklanan metin depolamak için alan:   

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Dizin oluşturma hakkında daha fazla bilgi için bkz: [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>3. adım: bir dizin oluşturucu yapın
Bir dizin oluşturucu hedef arama dizini ile bir veri kaynağına bağlanır ve veri yenileme otomatikleştirmek için bir zamanlama sağlar.

Dizinin ve veri kaynağının oluşturduktan sonra Dizin Oluşturucu oluşturmak hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizin oluşturucu (zamanlama aralığı "PT2H" ayarlanır) iki saatte çalışacaktır. 30 dakikada bir dizin oluşturucu çalıştırmak için "PT30M" aralığını ayarlayın. En kısa desteklenen aralığı 5 dakikadır. Zamanlama atlanırsa, isteğe bağlı -, yalnızca zaman bir kez oluşturulduktan sonra bir dizin oluşturucu çalıştırır. Ancak, bir dizin oluşturucu isteğe bağlı herhangi bir zamanda çalıştırabilirsiniz.   

Oluşturma dizin oluşturucu API'si hakkında daha fazla ayrıntı için kullanıma [oluşturma dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Azure Search BLOB'ları nasıl dizinler

Bağlı olarak [dizin oluşturucu yapılandırma](#PartsOfBlobToIndex), blob dizin oluşturucu yalnızca depolama meta dizin oluşturabilirsiniz (yararlı hakkındaki meta verileri yalnızca ilgilendiğiniz ve BLOB içerik dizinini gerekmez), depolama ve içerik meta verilerini, veya meta veri ve metin içeriği. Varsayılan olarak, dizin oluşturucu meta verilerini ve içeriği ayıklar.

> [!NOTE]
> Varsayılan olarak, BLOB'lar ile yapılandırılmış bir içerik JSON veya CSV gibi tek bir metin öbek dizine alınır. JSON ve CSV BLOB'lar yapılandırılmış şekilde dizin istiyorsanız, bkz: [dizin JSON BLOB '](search-howto-index-json-blobs.md) ve [dizin oluşturma CSV BLOB'ların](search-howto-index-csv-blobs.md) Önizleme özellikleri.
>
> Bileşik veya katıştırılmış bir belge (örneğin, ZIP arşivini veya ekleri içeren katıştırılmış Outlook e-posta içeren bir Word belgesini) de tek bir belge dizine alınır.

* Adlı bir dize alanı belgesinin metinsel içeriği ayıklanan `content`.

> [!NOTE]
> Azure arama sınırlar ne kadar metin fiyatlandırma katmanını bağlı olarak ayıklar: 32.000 karakter ücretsiz katmanı, Basic 64.000 ve standart, standart S2 ve standart S3 katmanları için 4 milyon. Bir uyarı kesilmiş belgeler için dizin oluşturucu durum yanıta dahil edilir.  

* Kullanıcı tanımlı meta veri özelliklerini blob üzerindeki mevcut varsa, aynen ayıklanır.
* Standart blob meta veri özellikleri aşağıdaki alanlara ayıklanır:

  * **meta veri\_depolama\_adı** (Edm.String) - blob dosya adı. Örneğin, bir blob /my-container/my-folder/subfolder/resume.pdf varsa, bu alanın değeri `resume.pdf`.
  * **meta veri\_depolama\_yolu** (Edm.String) - BLOB Depolama hesabı dahil olmak üzere, tam URI. Örneğin, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **meta veri\_depolama\_içerik\_türü** (Edm.String) - içerik blob karşıya yüklemek için kullanılan kod tarafından belirtilen türü. Örneğin, `application/octet-stream`.
  * **meta veri\_depolama\_son\_değiştiren** (Edm.DateTimeOffset) - blob için zaman damgası son değiştirilme tarihi. Azure arama bu zaman damgası her şeyi ilk dizin oluşturma sonrasında yeniden dizin oluşturmaya önlemek için değiştirilmiş BLOB'lar tanımlamak için kullanır.
  * **meta veri\_depolama\_boyutu** (EDM.Int64) - blob bayt cinsinden boyutu.
  * **meta veri\_depolama\_içerik\_md5** (Edm.String) - MD5 karma değeri blob içeriğinin varsa.
* Her belge biçimine özgü meta veriler özellikleri listelenen alanlarına ayıklanan [burada](#ContentSpecificMetadata).

Search dizininizi yukarıdaki tüm özellikler için alanları tanımla - yalnızca uygulamanız için gereksinim duyduğunuz özellikleri yakalama gerekmez.

> [!NOTE]
> Genellikle, alan adları varolan dizininize belge ayıklama sırasında oluşturulan alan adları farklı olacaktır. Kullanabileceğiniz **alan eşlemelerini** search dizininizi içindeki alan adlarının için Azure Search tarafından sağlanan özellik adlarını eşleştirmek için. Alan eşlemeleri aşağıda kullanacağınız örneği görürsünüz.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Belge anahtarları ve alan eşlemelerini tanımlama
Azure Search'te belge anahtarını bir belge benzersiz olarak tanımlar. Tüm arama dizini türü Edm.String tam olarak bir anahtar alanı olması gerekir. (Bunu gerçekte yalnızca gerekli bir alandır) dizine eklenecek her belge için anahtar alanı gereklidir.  

Ayıklanan alanı anahtar alan dizininiz için eşlemelisiniz dikkatlice düşünün. Adaylar:

* **meta veri\_depolama\_adı** - bu kullanışlı bir aday olabilir, ancak farklı klasörlerde aynı adı ve 2) adlı BLOB olabilir olarak 1) adları benzersiz olmayabileceğini Not belge anahtarları, kısa çizgi gibi geçersiz karakterler içeriyor olabilir. Geçersiz karakter kullanarak başa `base64Encode` [alan eşleme işlev](search-indexer-field-mappings.md#base64EncodeFunction) - bunu yaptığınızda, API geçirme gibi arama çağırdığında belge anahtarları kodlanacak unutmayın. (Örneğin, .NET içinde kullanabileceğiniz [UrlTokenEncode yöntemi](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) bu amaç için).
* **meta veri\_depolama\_yolu** - tam yolunu kullanarak benzersizlik sağlar, ancak yol kesinlikle içeriyor `/` olan karakterleri [geçersiz bir belge anahtarında](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Yukarıdaki olarak anahtarlarını kullanarak kodlama seçeneğiniz `base64Encode` [işlevi](search-indexer-field-mappings.md#base64EncodeFunction).
* Yukarıdaki seçeneklerin hiçbiri sizin için çalışıyorsanız, BLOB'lar için özel meta veri özelliği ekleyebilirsiniz. Ancak, bu seçenek tüm blobları bu meta veri özelliği eklemek için blob karşıya yükleme işlemi gerektirir. Bu anahtar gerekli bir özellik olduğundan, bu özelliğe sahip olmayan tüm BLOB'lar dizine başarısız olur.

> [!IMPORTANT]
> Azure Search dizini anahtar alanı için açık bir eşleme varsa, otomatik olarak kullanır `metadata_storage_path` anahtar değerleri (ikinci seçeneği yukarıdaki) anahtar ve base-64 kodlar.
>
>

Bu örnekte, şimdi çekme `metadata_storage_name` alan belge anahtarı olarak. Ayrıca, dizini adlı bir anahtar alanı var varsayalım `key` alanı `fileSize` belge boyutu depolamak için. İstediğiniz gibi işlemleri wire oluştururken veya oluşturucunuz güncelleştirirken aşağıdaki alan eşlemelerini belirtmeniz gerekir:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Bu duruma getirmek için hepsini bir araya, işte nasıl alan eşlemelerini eklemek ve varolan bir dizin oluşturucu için anahtarların 64 tabanlı kodlama etkinleştir:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2016-09-01
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
## <a name="controlling-which-blobs-are-indexed"></a>Hangi BLOB'lar dizine denetleme
Hangi BLOB'lar dizine ve hangi atlanır kontrol edebilirsiniz.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Yalnızca belirli dosya uzantılarına sahip BLOB'lar dizin
Yalnızca belirttiğiniz kullanarak dosya adı uzantılarına sahip BLOB'lar dizin `indexedFileNameExtensions` dizin oluşturucu yapılandırma parametresi. Dosya Uzantıları (ile başında nokta), virgülle ayrılmış listesini içeren bir dize değeridir. Örneğin, yalnızca dizin için. PDF ve. DOCX BLOB'lar, bunu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>BLOB'lar ile belirli dosya uzantılarını dışarıda
Belirli dosya adı uzantılarına sahip BLOB'lar kullanarak dizin dışlayabilirsiniz `excludedFileNameExtensions` yapılandırma parametresi. Dosya Uzantıları (ile başında nokta), virgülle ayrılmış listesini içeren bir dize değeridir. Örneğin, tüm BLOB'lar olanlar dışında dizin. PNG ve. JPEG uzantılar, bunu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Her iki `indexedFileNameExtensions` ve `excludedFileNameExtensions` parametreleri, ilk Azure Search bakar adresindeki `indexedFileNameExtensions`, sonra en `excludedFileNameExtensions`. Bu, aynı dosya uzantısını hem listelerinde varsa, bu dizine almasını dışlanıp olduğunu anlamına gelir.

### <a name="dealing-with-unsupported-content-types"></a>Desteklenmeyen içerik türleriyle ele alma

Blob desteklenmeyen bir içerik türüyle (örneğin, bir görüntü) karşılaştığında hemen varsayılan olarak, blob dizin oluşturucu durdurur. Elbette kullanabilirsiniz `excludedFileNameExtensions` belirli içerik türlerini atlamak için parametre. Ancak, tüm olası içerik türleri önceden bilmeden dizin BLOB'lar için gerekebilir. Desteklenmeyen içerik türü karşılaşıldığında dizin oluşturmaya devam etmek için ayarlama `failOnUnsupportedContentType` yapılandırma parametresi `false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

### <a name="ignoring-parsing-errors"></a>Ayrıştırma hataları yoksayma

Azure arama belge ayıklama mantığı kusursuz değildir ve bazen gibi desteklenen bir içerik türüne belgelerinin ayrıştırmak başarısız olur. DOCX veya. PDF. Bu gibi durumlarda dizin oluşturma işlemi kesintiye, ayarlamak, istemiyorsanız, `maxFailedItems` ve `maxFailedItemsPerBatch` makul bazı değerler yapılandırma parametreleri. Örneğin:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Blob hangi kısımlarının dizine denetleme

BLOB'ları hangi kısımlarının kullanarak dizinlenir denetleyebilirsiniz `dataToExtract` yapılandırma parametresi. Aşağıdaki değerleri alabilir:

* `storageMetadata`-belirtir, yalnızca [standart blob özellikleri ve kullanıcı tanımlı meta veriler](../storage/blobs/storage-properties-metadata.md) dizinlenir.
* `allMetadata`-Depolama meta verilerin belirtir ve [içerik türü belirli meta veriler](#ContentSpecificMetadata) ayıklanan blobundan içerik dizin haline getirilir.
* `contentAndMetadata`-tüm meta veri ve blobundan ayıklanan metinsel içerik dizinlenir belirtir. Varsayılan değer budur.

Örneğin, yalnızca depolama meta veri dizini için kullanın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>BLOB'ları nasıl dizine denetlemek için BLOB meta verileri kullanma

Yukarıda açıklanan yapılandırma parametreleri tüm BLOB'lar için geçerlidir. Bazı durumlarda, denetlemek isteyebilir nasıl *tek tek bloblar* dizinlenir. Aşağıdaki blob meta veri özellikleri ve değerleri ekleyerek bunu yapabilirsiniz:

| Özellik adı | Özellik değeri | Açıklama |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Blob tamamen atlamak için blob dizin oluşturucu bildirir. Meta veri ne içerik ayıklama denenir. Bu, belirli bir blob art arda başarısız olur ve dizin oluşturma işlemi kesintiye uğratır durumunda faydalı olur. |
| AzureSearch_SkipContent |"true" |Bu, eşdeğerdir `"dataToExtract" : "allMetadata"` açıklanan ayarı [yukarıda](#PartsOfBlobToIndex) belirli bir blobu kapsamlı. |

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılama
Bir zamanlamaya göre çalıştırmak için bir blob dizin oluşturucu ayarladığınızda, blob'un tarafından belirlendiği şekilde yalnızca değiştirilen BLOB'lar aşağıdaki yeniden dizinler `LastModified` zaman damgası.

> [!NOTE]
> Bir değişiklik algılama İlkesi belirtmeniz gerekmez – Artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Silme belgeleri desteklemek için bir "geçici silme" yaklaşımı kullanın. BLOB'ları depolayabileceği silerseniz, karşılık gelen belgeler arama dizinden kaldırılmaz. Bunun yerine, aşağıdaki adımları kullanın:  

1. Azure Search mantıksal olarak silinip silinmediğini belirtmek için blob için özel meta veri özellik ekleme
2. Veri kaynağında bir geçici silme algılama ilkesi yapılandırma
3. Dizin Oluşturucu (Dizin Oluşturucu durumu API ile gösterildiği gibi) blob işlediğinde, fiziksel olarak blob silebilirsiniz

Örneğin, bir meta veri özelliği varsa, silinecek blob aşağıdaki ilkesi göz önünde bulundurur `IsDeleted` değerle `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2016-09-01
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

## <a name="indexing-large-datasets"></a>Dizin oluşturma büyük veri kümeleri

BLOB'ları dizin oluşturma zaman alan bir işlem olabilir. BLOB'ları dizin milyonlarca sahip olduğu durumlarda, verilerinizi bölümlendirme ve paralel verileri işlemek için birden çok dizin oluşturucu kullanarak dizin oluşturmayı hızlandırabilir. İşte nasıl bu ayarlayabilirsiniz:

- Verilerinizin birden çok blob kapsayıcıları ya da sanal klasörler bölüm
- Çok sayıda Azure Search veri kaynağı, bir kapsayıcı veya klasör başına ayarlayın. Bir blob klasöre işaret edecek şekilde kullanma `query` parametre:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Her veri kaynağı için karşılık gelen bir dizin oluşturucu yapın. Tüm Dizin oluşturucuların aynı hedef arama dizinine işaret edebilir.  

- Bir arama birimi hizmetinizde bir dizin oluşturucu herhangi bir zamanda çalıştırabilirsiniz. Yukarıda açıklandığı gibi birden çok dizin oluşturucu oluşturma yalnızca gerçekten paralel olarak çalıştırırsanız yararlı olur. Paralel olarak birden çok dizin oluşturucu çalıştırmak için bölümler ve çoğaltmalar uygun sayıda oluşturarak, arama hizmetin ölçeğini genişletin. Arama hizmetinizi 6 arama birimi (örneğin, 2 bölüm x 3 yineleme) varsa, örneğin, daha sonra 6 dizin oluşturucular, dizin oluşturma performansı six-fold bir artış sonuçta çalıştırabilirsiniz. Ölçekleme ve kapasite planlaması hakkında daha fazla bilgi için bkz: [ölçeklendirme sorgu ve iş yüklerini Azure Search'te dizin oluşturma için kaynak düzeylerini](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>İlgili verileri birlikte belgelere dizin

"Dizininizdeki birden fazla kaynaktan belgeleri derlemek" isteyebilirsiniz. Örneğin, BLOB'ları metinden Cosmos DB içinde depolanan diğer meta verileri ile birleştirme isteyebilirsiniz. API ile birlikte çeşitli dizin oluşturucular dizin itme bile birden çok bölümlerinden search belgeleri oluşturmak için de kullanabilirsiniz. 

Bunun çalışması için tüm dizin oluşturucuların ve diğer bileşenleri belge anahtarı kabul etmeniz gerekir. Dış makalede ayrıntılı bir kılavuz için bkz: [Azure Search'te diğer verilerle belgeleri birleştirmeniz ](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Dizin oluşturma düz metin 

Tüm bloblarınızın aynı kodlamada düz metin içeriyorsa, önemli ölçüde dizin oluşturma performansı kullanarak artırabilirsiniz **modu ayrıştırma metin**. Metin modunu ayrıştırma kullanmak üzere ayarlanmış `parsingMode` Yapılandırma özelliği `text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Varsayılan olarak, `UTF-8` kodlama varsayılır. Farklı bir kodlama belirtmek için kullanın `encoding` Yapılandırma özelliği: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>İçerik türe özgü meta veriler özellikleri
Aşağıdaki tabloda her belge biçimi için yapılan işleme özetler ve Azure Search tarafından ayıklanan meta veri özelliklerini açıklar.

| Belge biçimi / içerik türü | İçerik türü belirli meta veriler özellikleri | İşlem ayrıntıları |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Şerit HTML İşaretleme ve ayıklama metin |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Embedded belgeler (görüntüleri hariç) dahil olmak üzere bir metin Ayıkla |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Embedded belgeler dahil olmak üzere bir metin Ayıkla |
| DOC (uygulama/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Embedded belgeler dahil olmak üzere bir metin Ayıkla |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Embedded belgeler dahil olmak üzere bir metin Ayıkla |
| XLS (uygulama/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Embedded belgeler dahil olmak üzere bir metin Ayıkla |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Embedded belgeler dahil olmak üzere bir metin Ayıkla |
| PPT (uygulama/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Embedded belgeler dahil olmak üzere bir metin Ayıkla |
| MSG (uygulama/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Metni ekler dahil olmak üzere, ayıklayın |
| ZIP (uygulama/posta) |`metadata_content_type` |Arşiv tüm belgelerde metin Al |
| XML (uygulama/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |Şerit XML biçimlendirme ve ayıklama metni |
| JSON (uygulama/json) |`metadata_content_type`</br>`metadata_content_encoding` |Metni ayıklayın<br/>Not: JSON blob üzerinden birden çok belge alanlarını ayıklamak gerekiyorsa, bkz. [dizin JSON BLOB'ların](search-howto-index-json-blobs.md) Ayrıntılar için |
| EML (ileti/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Metni ekler dahil olmak üzere, ayıklayın |
| RTF (uygulama/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Metni ayıklayın|
| Düz metin (metin/düz) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Metni ayıklayın|


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya fikir geliştirmeleri için varsa, bize bilmeniz bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
