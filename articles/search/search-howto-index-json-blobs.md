---
title: Dizin oluşturma JSON BLOB'ları ile Azure Search blob dizin oluşturucu
description: Dizin oluşturma JSON BLOB'ları ile Azure Search blob dizin oluşturucu
author: chaosrealm
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: eugenesh
ms.openlocfilehash: 752df29200a5e020ccf10f511ae2f02c0d72bd48
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Dizin oluşturma JSON BLOB'ları ile Azure Search blob dizin oluşturucu
Bu makalede, JSON BLOB'ları Azure Blob storage'da yapılandırılmış içeriği ayıklamak için bir Azure Search blob dizin oluşturucu yapılandırma gösterilmektedir.

JSON BLOB'ları Azure Blob depolamada genellikle tek bir JSON belgesi veya JSON dizisi yayımlanır. Azure Search blob oluşturucuda nasıl ayarladığınıza bağlı olarak ya da yapım ayrıştıramıyor **parsingMode** istek parametresi.

| JSON belgesi | parsingMode | Açıklama | Kullanılabilirlik |
|--------------|-------------|--------------|--------------|
| Her blob | `json` | JSON BLOB'ları, tek bir metin öbek ayrıştırır. Her bir JSON blob tek bir Azure Search belge olur. | Hem de genel olarak kullanılabilir [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API'leri. |
| Blob başına birden çok | `jsonArray` | Dizideki her öğe ayrı bir Azure Search belge burada hale blob, JSON dizisinde ayrıştırır.  | Önizleme, içinde [REST API sürümü =`2017-11-11-Preview` ](search-api-2017-11-11-preview.md) ve [.NET SDK Preview](https://aka.ms/search-sdk-preview). |

> [!Note]
> Önizleme API'leri sınama ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır.
>

## <a name="setting-up-json-indexing"></a>JSON dizin oluşturmayı ayarlama
JSON BLOB'ları dizin oluşturma üç bölümlük iş akışı ortak Azure Search'te tüm dizin oluşturucuların için normal belge ayıklama benzer.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

İlk adım, dizin oluşturucu tarafından kullanılan veri kaynağı bağlantı bilgilerini sağlamaktır. Veri kaynağı türü, belirtilen burada olarak `azureblob`, hangi veri ayıklama davranışları Oluşturucu tarafından çağrılan belirler. Dizin oluşturma için JSON blob veri kaynağı tanımını JSON belgeleri ve diziler için aynı numarasıdır. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Adım 2: hedef arama dizini oluşturma 

Dizin oluşturucular dizin şeması ile eşleştirilmelidir. API (portal yerine) kullanıyorsanız, dizin oluşturucu işlemi belirtmek için bir dizin önceden hazırlayın. 

> [!Note]
> Dizin oluşturucular Portalı aracılığıyla sunulur **alma** sınırlı sayıda genel olarak kullanılabilir dizin oluşturucular için eylem. Genellikle, içeri aktarma iş akışı genellikle meta veri kaynağındaki bağlı bir ön dizin oluşturabilirsiniz. Daha fazla bilgi için bkz: [veri içeri aktarma Azure Search portalda](search-import-data-portal.md).

### <a name="step-3-configure-and-run-the-indexer"></a>3. adım: Yapılandırma ve dizin oluşturucu çalıştırma

Şimdiye kadar veri kaynağı ve dizin tanımlarında parsingMode belirsiz olmuştur. Ancak, adım 3'te dizin oluşturucu yapılandırması için JSON blob ayrıştırılır ve Azure Search dizini içinde yapılandırılmış içerik nasıl istediğinizi bağlı olarak yol kareninkinden.

Dizin oluşturucu çağrılırken, aşağıdakileri yapın:

+ Ayarlama **parsingMode** parametresi `json` (tek bir belge olarak her bir blob dizini oluşturmak için) veya `jsonArray` (bloblarınızın JSON dizileri içerir ve ayrı bir belge olarak kabul edilmesi için bir dizinin her bir öğesine ihtiyacınız varsa).

+ İsteğe bağlı olarak kullanmak **alan eşlemelerini** hedef search dizininizi doldurmak için kullanılan kaynak JSON belgesi hangi özelliklerini seçin. JSON diziler için bir alt düzey özelliği olarak, dizi varsa, dizi içinde blob yerleştirildiği belirten bir belge kökü ayarlayabilirsiniz.

> [!IMPORTANT]
> Kullandığınızda `json` veya `jsonArray` modu ayrıştırma, Azure Search, veri kaynağındaki tüm BLOB'lar JSON içeren varsayar. Üzerinde bir karışımını JSON ve JSON olmayan BLOB aynı veri kaynağına desteklemeniz gerekiyorsa,'ın bize bildirin [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).


## <a name="how-to-parse-single-json-blobs"></a>Tek JSON BLOB'ları ayrıştırmayı

Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) JSON BLOB'ları tek bir metin öbek ayrıştırır. Genellikle, JSON belgelerinin yapısını korumak istiyorsunuz. Örneğin, Azure Blob depolama alanına aşağıdaki JSON belgesi olduğunu varsayın:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

### <a name="indexer-definition-for-single-json-blobs"></a>Tek JSON BLOB'ları için dizin oluşturucu tanımı

Önceki örneğe benzer bir JSON belgesi, Azure Search blob dizin oluşturucusunu kullanarak, tek bir Azure Search belgeye ayrıştırılır. Dizin Oluşturucu, "metin", "datePublished" ve "aynı adlı ve belirlenmiş hedef alan karşı kaynağından etiketler" eşleştirerek dizin yükler.

Yapılandırma, bir dizin oluşturucu işlemi gövdesinde sağlanır. Geri çağırma önceden tanımlanmış veri kaynağı nesnesinin veri kaynağı türü ve bağlantı bilgilerini belirtir. Ayrıca, hedef dizin hizmetinizde boş bir kapsayıcı olarak da bulunmalıdır. Zamanlama ve parametreler isteğe bağlıdır, ancak bunları atlarsanız, kullanarak dizin oluşturucu hemen çalışır `json` ayrıştırma modu.

Tam olarak belirtilen bir istek şu şekilde görünebilir:

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

Alan eşlemelerini belirtildiği gibi gerekli değildir. Dizin "metin" ile verilen "datePublished ve"etiketler"alanları, blob dizin oluşturucu Infer doğru eşleme alan istekte eşleme olmadan.

## <a name="how-to-parse-json-arrays-preview"></a>Nasıl yapılır parse JSON diziler (Önizleme)

Alternatif olarak, JSON dizisi önizleme özelliği için tercih edebilirsiniz. BLOB'ları içerdiğinde bu özellik yararlıdır bir *dizi JSON nesnesi*, ve her öğe ayrı bir Azure Search belge olmasını istiyor. Örneğin, aşağıdaki JSON blob verildiğinde, Azure Search dizininizi üç ayrı belgelerle, her "id" ve "metin" alanları ile doldurabilirsiniz.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

### <a name="indexer-definition-for-a-json-array"></a>Bir JSON dizisi için dizin oluşturucu tanımı

Bir JSON dizisi için dizin oluşturucu isteği API Önizleme kullanır ve `jsonArray` ayrıştırıcı. JSON BLOB'ları dizin oluşturma için yalnızca iki dizi özgü gereksinimler şunlardır.

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

Yeniden alan eşlemelerini gerekli olmadığına dikkat edin. Dizin "id" ve "metin" alanları ile verilen, blob dizin oluşturucu alan eşleme listesi olmadan doğru eşleme çıkarımını.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>İç içe JSON diziler
Ne JSON nesnelerinin bir dizisi, ancak bu dizi dizini oluşturmak istediğiniz yere belge içinde iç içe yerleştirilmiş? Hangi özelliği kullanarak dizi içerir çekme `documentRoot` Yapılandırma özelliği. Örneğin, BLOB'ları şöyle görünür:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

içinde yer alan dizi dizini oluşturmak için bu yapılandırmayı kullanmak `level2` özelliği:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="using-field-mappings-to-build-search-documents"></a>Search belgeleri oluşturmak için alan eşlemelerini kullanma

Kaynak ve hedef alanlarını mükemmel hizalanmaz oluştururken açık alan alanını ilişkileri için istek gövdesinde bir alanın eşleme bölümüne tanımlayabilirsiniz.

Yalnızca ilkel veri türleri, dize dizileri ve GeoJSON noktaları desteklediğinden, şu anda, Azure Search rastgele JSON belgelerinin doğrudan dizin oluşturulamıyor. Ancak, kullanabileceğiniz **alan eşlemelerini** JSON belgenizi bölümlerini seçin ve "bunları arama belge üst düzey alanlarına Yükselt". Alan eşlemeleri temel kavramları hakkında bilgi edinmek için [alan eşlemelerini Azure Search'te dizin oluşturucular içinde](search-indexer-field-mappings.md).

Bizim örnek JSON belgeyi yeniden ziyaret etme:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Bir arama dizinini aşağıdaki alanlarla varsayalım: `text` türü `Edm.String`, `date` türü `Edm.DateTimeOffset`, ve `tags` türü `Collection(Edm.String)`. "DatePublished" arasında farklılık kaynak dikkat edin ve `date` dizinindeki alan. İstenen şekle, JSON eşlemek için aşağıdaki alan eşlemelerini kullanın:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Eşlemeleri kaynak alan adlarını kullanarak belirtilen [JSON işaretçi](http://tools.ietf.org/html/rfc6901) gösterimi. JSON belgesi kökündeki başvurmak için eğik çizgiyle başlayın, ardından İleri eğik ayrılmış yolu kullanarak ve istenen özelliğine (iç içe geçme düzeyi rastgele) seçin.

Sıfır tabanlı dizini kullanılarak tek tek dizi öğeleri de başvurabilir. Örneğin, yukarıdaki örnekteki "etiketler" dizinin ilk öğesi seçmek için bu gibi bir alan eşleme kullanın:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Bir kaynak alan adı bir alan eşleme yolunda JSON içinde mevcut olmayan bir özelliğe başvuruyor, bu eşlemeyi hatasız atlanır. Bu, böylece (ortak kullanım örneği olan) farklı bir şema belgelerle destekliyoruz gerçekleştirilir. Hiçbir doğrulama olduğundan ilgilenebilmek alan eşleme belirtimi yazım hatalarını önlemek için gerekir.
>
>

## <a name="example-indexer-request-with-field-mappings"></a>Örnek: Dizin Oluşturucu istekle alan eşlemeleri

Aşağıdaki örnek, alan eşlemelerini içeren bir tam olarak belirtilen dizin oluşturucu yükü verilmiştir:

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
Özellik istekleri veya fikir geliştirmeleri için varsa, bize üzerinde ulaşmak bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te dizin oluşturucular](search-indexer-overview.md)
+ [Azure Blob Depolama Azure Search dizini oluşturma](search-howto-index-json-blobs.md)
+ [Dizin oluşturma CSV BLOB'lar ile Azure Search blob dizin oluşturucu](search-howto-index-csv-blobs.md)
+ [Öğretici: Azure Blob depolama biriminden yarı yapılandırılmış veri arama ](search-semi-structured-data.md)