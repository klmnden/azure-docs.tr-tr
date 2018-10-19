---
title: Azure Search blob dizin oluşturucu ile JSON bloblarını dizine ekleme
description: Azure Search blob dizin oluşturucu ile JSON bloblarını dizine ekleme
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.openlocfilehash: a4689093508c3287e60da9d4668393e71211fbdd
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405711"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Azure Search blob dizin oluşturucu ile JSON bloblarını dizine ekleme
Bu makalede bir Azure Search blob dizin oluşturucu JSON BLOB'ları Azure Blob storage'da yapılandırılmış içeriği ayıklamak için yapılandırma gösterilmektedir.

Azure Blob Depolama alanında JSON bloblarını genellikle tek bir JSON belge ya da bir JSON dizisi cihazlardır. Azure Search blob dizin oluşturucu nasıl belirlediği ayarlara bağlı olarak ya da yapı ayrıştırabilirsiniz **parsingMode** istek parametresi.

| JSON belgesi | parsingMode | Açıklama | Kullanılabilirlik |
|--------------|-------------|--------------|--------------|
| Bir blob başına | `json` | JSON BLOB'ları, tek bir metin parçası ayrıştırır. Her bir JSON blob tek bir Azure Search belge olur. | Hem de genel kullanıma [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API'leri. |
| Birden çok blob başına | `jsonArray` | Burada dizideki her öğe ayrı bir Azure Search belge olur ve blob'daki bir JSON dizisi ayrıştırır.  | Önizleme aşamasında içinde [REST API-version =`2017-11-11-Preview` ](search-api-2017-11-11-preview.md) ve [.NET SDK Preview](https://aka.ms/search-sdk-preview). |

> [!Note]
> Önizleme API'leri, test ve değerlendirme içindir ve üretim ortamlarında kullanılmamalıdır.
>

## <a name="setting-up-json-indexing"></a>JSON dizin oluşturma ayarlama
JSON bloblarını dizine ekleme, üç bölümü iş akışı ortak normal belge ayıklama Azure Search'te tüm dizin oluşturucular için benzer.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

İlk adım, dizin oluşturucu tarafından kullanılan veri kaynağı bağlantı bilgilerini sağlamaktır. Veri kaynağı türü, belirtilen olarak burada `azureblob`, hangi veri ayıklama davranışları Dizin Oluşturucu tarafından çağrılan belirler. JSON blob dizin oluşturma işlemi için veri kaynağı tanımını hem JSON belgeleri ve diziler için aynıdır. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>2. adım: hedef arama dizini oluşturma 

Dizin oluşturucular bir dizin şeması ile eşleştirilmelidir. API (portal yerine) kullanıyorsanız, dizin dizin oluşturucu işlemi belirtebilirsiniz böylece önceden hazırlayın. 

> [!Note]
> Dizin Oluşturucular, portal üzerinden sunulur **alma** sınırlı sayıda genel kullanıma açık dizin oluşturucular için eylem. Genellikle, iş akışını içeri aktar genellikle meta veri kaynağındaki bağlı ön bir dizin oluşturabilirsiniz. Daha fazla bilgi için [verileri içeri aktarma Azure Search'e portalda](search-import-data-portal.md).

### <a name="step-3-configure-and-run-the-indexer"></a>Adım 3: Yapılandırma ve dizin oluşturucuyu çalıştırma

Şimdiye kadar veri kaynağı ve dizin tanımlarını parsingMode belirsiz olmuştur. Ancak, adım 3'te dizin oluşturucu yapılandırmasını için JSON blob ayrıştırılır ve bir Azure Search dizini içinde yapılandırılmış içerik nasıl istediğinizi bağlı olarak yol kareninkinden.

Dizin oluşturucu çağrılırken, aşağıdakileri yapın:

+ Ayarlama **parsingMode** parametresi `json` (her blob olarak tek bir belge dizini oluşturmak için) veya `jsonArray` (JSON dizileri içeren, blobları ve ayrı bir belge olarak kabul edilmesi için bir dizideki her öğe ihtiyacınız varsa).

+ İsteğe bağlı olarak, **alan eşlemeleri** hedef arama dizininizi doldurmak için kullanılan kaynak JSON belgesinin hangi özellikleri seçmek için. JSON dizileri için alt düzey özelliği olarak, dizi varsa, dizi içinde blob nereye yerleştirileceğini gösteren bir belge kökü ayarlayabilirsiniz.

> [!IMPORTANT]
> Kullanırken `json` veya `jsonArray` ayrıştırma modu, Azure Search içeren JSON veri kaynağındaki tüm blobları varsayar. Aynı veri kaynağındaki JSON ve JSON olmayan bloblar bir karışımını desteklemeniz gerekiyorsa, üzerinde bize [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).


## <a name="how-to-parse-single-json-blobs"></a>Tek bir JSON bloblarını ayrıştırmayı

Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) JSON BLOB'ları, tek bir metin parçası ayrıştırır. Genellikle, JSON belgeleri yapısını korumak isteyebilirsiniz. Örneğin, Azure Blob Depolama alanında aşağıdaki JSON belgesini olduğunu varsayın:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

### <a name="indexer-definition-for-single-json-blobs"></a>Tek JSON BLOB'ları için dizin oluşturucu tanımı

Azure Search blob dizin oluşturucu kullanarak, önceki örneğe benzer bir JSON belgesini tek bir Azure Search belgeye ayrıştırılır. Dizin oluşturucunun, dizin "metin", "datePublished" ve "aynı şekilde adlandırılmış ve yazılmış hedef alanlara göre kaynak etiketleri" eşleştirerek yükler.

Yapılandırma, bir dizin oluşturucu işlemi gövdesinde sağlanır. Veri kaynağı nesnesi, önceden tanımlanmış, geri çağırma veri kaynağı türü ve bağlantı bilgilerini belirtir. Ayrıca, hedef dizin hizmetinizde boş bir kapsayıcı olarak da bulunmalıdır. Zamanlama ve parametreler isteğe bağlıdır, ancak bunları atlarsanız, dizin oluşturucu hemen kullanarak çalışır `json` ayrıştırma modu.

Tam olarak belirtilen bir istek gibi görünebilir:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Alan eşlemelerini belirtildiği gibi gerekli değildir. Dizin ile "metin" verilen "datePublished ve"etiketleri"alanları, blob dizin oluşturucu tanım Çıkarsama doğru eşleme olmadan bir alan istekteki eşleme.

## <a name="how-to-parse-json-arrays-preview"></a>Nasıl ayrıştıracağını JSON dizileri (Önizleme)

Alternatif olarak, JSON dizisi önizleme özelliği için seçebilirsiniz. Bu özellik bloblar içerdiğinde yararlıdır bir *JSON nesne dizisi*, ve her öğe ayrı bir Azure Search belge olmasını istiyorsanız. Örneğin, aşağıdaki JSON blob göz önünde bulundurulduğunda, Azure Search dizininizi "id" ve "metin" alanları her üç ayrı belgeler ile doldurabilirsiniz.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

### <a name="indexer-definition-for-a-json-array"></a>Bir JSON dizisi için dizin oluşturucu tanımı

Bir JSON dizisi için dizin oluşturucu istek API Önizleme kullanır ve `jsonArray` ayrıştırıcı. JSON bloblarını dizine ekleme için yalnızca iki dizi özgü gereksinimler şunlardır.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Yine, alan eşlemelerini gerekli olmadığına dikkat edin. Dizin "id" ve "metin" alanları ile verilen, blob dizin oluşturucu listesini alan eşleme olmadan doğru eşleme çıkarabilir.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>İç içe geçmiş JSON dizileri
Ne bir JSON nesne dizisi, ancak bu diziyi dizine eklemek istediğiniz yere belgenin içinde iç içe yerleştirilmiş? Hangi özelliğini kullanarak dizi içeren çekme `documentRoot` yapılandırma özellik. Örneğin, BLOB'ları şöyle görünür:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

İçinde yer alan dizi dizini oluşturmak için bu yapılandırmayı kullanan `level2` özelliği:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="using-field-mappings-to-build-search-documents"></a>Search belgeleri oluşturmak için alan eşlemelerini kullanma

Kaynak ve hedef alanlarını mükemmel bir şekilde hizalanmış değil, açık alan alanını ilişkileri için istek gövdesinde bir alan eşleme bölümüne tanımlayabilirsiniz.

Yalnızca basit veri türleri, dize dizilerini ve GeoJSON noktaları desteklediğinden şu anda, Azure Search rastgele JSON belgelerinin doğrudan dizin oluşturulamıyor. Ancak, kullanabileceğiniz **alan eşlemeleri** JSON belgenizi bölümlerini seçin ve "onlara arama belgesinin en üst düzey alanlarına Yükselt". Alan eşlemelerini temelleri hakkında bilgi edinmek için [alan eşlemeleri Azure Search dizin oluşturucularında](search-indexer-field-mappings.md).

Bizim örnek JSON belgesi hakkında yeniden değerlendirme:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Şu alanlara sahip bir arama dizini varsayar: `text` türü `Edm.String`, `date` türü `Edm.DateTimeOffset`, ve `tags` türü `Collection(Edm.String)`. Kaynakta "datePublished" arasında bir tutarsızlık dikkat edin ve `date` dizinindeki alan. JSON istediğiniz şekle eşlemek için aşağıdaki alan eşlemelerini kullanın:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Eşlemeleri kaynak alan adlarını kullanarak belirtilen [JSON işaretçisi](http://tools.ietf.org/html/rfc6901) gösterimi. JSON belgenizin kök başvurmak için eğik çizgiyle başlatın ve ardından İleri eğik ayrılmış yol kullanarak (en iç içe geçme düzeyine isteğe bağlı) istenen özellik seçin.

Ayrıca, sıfır tabanlı dizinini kullanarak tek bir dizi öğelerine başvurabilir. Örneğin, yukarıdaki örnekteki "tags" dizinin ilk öğesi seçmek için şunun gibi bir alan eşlemesi kullanın:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Bir kaynak alan adı alan eşleme yolu, JSON'da mevcut olmayan bir özelliğe başvuruyorsa, bu eşlemenin bir hata atlandı. Bu, böylece (Genel kullanım örneği olan) farklı bir şema belgelerle destekliyoruz gerçekleştirilir. Doğrulama olduğundan halletmeniz alan eşleme belirtiminde hatalarını önlemek gerekir.
>
>

## <a name="example-indexer-request-with-field-mappings"></a>Örnek: Dizin Oluşturucu istekle alan eşlemeleri

Aşağıdaki örnek, alan eşlemelerini de dahil olmak üzere tam olarak belirtilen dizin oluşturucu yüktür:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya geliştirmeleri için fikriniz varsa Bize Ulaşın bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te dizin oluşturucular](search-indexer-overview.md)
+ [Azure arama ile Azure Blob Depolama dizini oluşturma](search-howto-index-json-blobs.md)
+ [Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
+ [Öğretici: Azure Blob depolamadan yarı yapılandırılmış verileri arama ](search-semi-structured-data.md)
