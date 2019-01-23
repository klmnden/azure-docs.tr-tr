---
title: JSON bloblarını dizine ekleme gelen tam metin arama - Azure Search için Azure Blob dizin oluşturucu
description: Azure Search Blob Dizin Oluşturucu kullanarak metin içeriği için Azure JSON bloblarını gezinin. Dizin oluşturucular veri alımı Azure Blob Depolama gibi seçili veri kaynakları için otomatik hale getirin.
ms.date: 12/21/2018
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: cafb48f28e38794ce0757d50a5d87432b237e17c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467172"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Azure Search Blob Dizin Oluşturucu ile JSON bloblarını dizine ekleme
Bu makalede bir Azure Search blob dizin oluşturucu JSON BLOB'ları Azure Blob storage'da yapılandırılmış içeriği ayıklamak için yapılandırma gösterilmektedir.

Kullanabileceğiniz [portalı](#json-indexer-portal), [REST API'leri](#json-indexer-rest), veya [.NET SDK'sı](#json-indexer-dotnet) dizin JSON içeriği. Tüm yaklaşımları ortak bir Azure depolama hesabındaki bir blob kapsayıcısında yer alan JSON belgeleri olan. Diğer Azure dışı platformlardan JSON belgelerini dağıtmaya ilişkin yönergeler için bkz. [Azure Search'te verileri içeri](search-what-is-data-import.md).

Azure Blob Depolama alanında JSON bloblarını genellikle tek bir JSON belge ya da bir JSON dizisi cihazlardır. Azure Search blob dizin oluşturucu nasıl belirlediği ayarlara bağlı olarak ya da yapı ayrıştırabilirsiniz **parsingMode** istek parametresi.

> [!IMPORTANT]
> JSON dizin kullanıma sunulacaktır, ancak JsonArray ayrıştırma genel Önizleme aşamasındadır ve üretim ortamlarında kullanılmamalıdır. Daha fazla bilgi için [REST API Sürüm 2017-11-11-Preview =](search-api-2017-11-11-preview.md). 

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Portalı kullanma

JSON belgelerini için en kolay yöntem Sihirbazı'nda kullanmaktır [Azure portalında](https://portal.azure.com/). Azure blob kapsayıcısı meta verilerinde ayrıştırma tarafından [ **verileri içeri aktarma** ](search-import-data-portal.md) Sihirbazı varsayılan bir dizin oluşturmak, kaynak alanları hedef dizin alanlarına eşleme ve tek bir işlemde dizini yükleme. Boyutu ve kaynak verilerin karmaşıklığına bağlı olarak birkaç dakika içinde bir işletimsel tam metin arama dizinini olabilir.

### <a name="1---prepare-source-data"></a>1 - kaynak verileri hazırlama

Bir JSON belge kapsayıcısı ve Blob Depolama ile Azure depolama hesabınız olmalıdır. Tüm bu görevlerin alışkın değilseniz, "Azure Blob hizmeti ve yük örnek veri kümesi" önkoşulları gözden geçirin [bilişsel arama-quickstart](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data).

### <a name="2---start-import-data-wizard"></a>2 - Veri Alma Sihirbazını Başlat

Yapabilecekleriniz [Sihirbazı başlatın](search-import-data-portal.md) Azure arama hizmeti sayfasını veya tıklayarak komut çubuğundan **Azure Search Ekle** içinde **Blob hizmeti** depolama hesabınızın bölümünü Sol gezinti bölmesi.

### <a name="3---set-the-data-source"></a>3 - veri kaynağı ayarla

İçinde **veri kaynağı** sayfasında, kaynak olmalıdır **Azure Blob Depolama**, aşağıdaki özellikleri ile:

+ **Ayıklanacak veri** olmalıdır *içerik ve meta verileri*. Bu seçeneğin belirlenmesi, bir dizin şemasını ve içeri aktarma için alanları eşlemek bir sihirbaz sağlar.
   
+ **Ayrıştırma modu** ayarlanmalıdır *JSON* veya *JSON dizisi*. 

  *JSON* arama sonuçlarında bağımsız bir öğe olarak gösteren, bir tek arama belge olarak her blob korumadaki. 

  *JSON dizisi* BLOB'ları birden çok öğelerin oluşan aranır, bağımsız aramayı belge tek başına geliştirilmiştir için her öğe istediğiniz. Karmaşık BLOB'ları ve tercih etseniz *JSON dizisi* tüm blob tek bir belge alınır.
   
+ **Depolama kapsayıcısı** , depolama hesabı ve kapsayıcı veya kapsayıcıya gideren bir bağlantı dizesi belirtmeniz gerekir. Blob hizmeti portal sayfasında, bağlantı dizeleri alabilirsiniz.

   ![BLOB veri kaynağı tanımını](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - sihirbazında "Bilişsel arama Ekle" sayfasını atlayın

Bilişsel beceriler ekleme JSON belgesi almak için gerekli değildir. Belirli bir gerek olmadığı sürece [Bilişsel hizmetler API'leri ve dönüştürmeler dahil](cognitive-search-concept-intro.md) , dizinleme işlem hattına bu adımı atlayın.

Bu adımı atlamak için sonraki sayfaya tıklayın **hedef dizini Özelleştir**.

### <a name="5---set-index-attributes"></a>5 - dizin öznitelikleri Ayarla

İçinde **dizin** sayfasında, bir veri türü ve dizin öznitelikleri ayarlamaya yönelik onay kutularından oluşan bir serinin alanların listesini görmelisiniz. Sihirbaz, meta verileri ve kaynak verileri örnekleme tarafından dayalı varsayılan bir dizin oluşturabilirsiniz. 

Varsayılan değerleri genellikle çalışılabilir bir çözüm oluşturmak: anahtarı olarak hizmet veya kimliği her belge yanı sıra ve bir sonuç kümesinde alınabilir tam metin araması için iyi adaylar olan alanlar benzersiz olarak tanımlanabilmesi için belge için bir alanı (dize olarak atama) seçin. Blobları için `content` aranabilir içeriği için en iyi aday bir alandır.

Varsayılan değerleri kabul edin veya açıklamasını inceleyin [dizin öznitelikleridir](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) ve [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) geçersiz kılabilir veya başlangıç değerleri desteklemek için. 

Seçimlerinizi gözden geçirmek için bir dakikanızı ayırın. Sihirbazı çalıştırdıktan sonra fiziksel veri yapılarını oluşturulur ve bu alanlar, bırakarak ve tüm nesneleri yeniden düzenlemek mümkün olmayacaktır.

   ![BLOB dizin tanımı](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - dizin oluşturucu oluşturma

Tam olarak belirtilen, sihirbaz arama hizmetinizdeki üç farklı bir nesne oluşturur. Bir veri kaynağı nesnesi ve dizin nesnesi, Azure Search hizmetinizde adlandırılmış kaynaklar olarak kaydedilir. Son adım, bir dizin oluşturucu nesnesini oluşturur. Dizin Oluşturucu adlandırma zamanlayabilir ve Yönet Sihirbazı sırayla oluşturulan dizin ve veri kaynağı nesnesi bağımsız olarak tek başına kaynak olarak mevcut izin verir.

Dizin oluşturucular ile aşina değilseniz bir *dizin oluşturucu* aranabilir içeriği için bir dış veri kaynağında gezinir Azure Search'te bir kaynaktır. Çıkışı **verileri içeri aktarma** sihirbazıdır bir dizin oluşturucu, bir Azure Search dizini aktarır, JSON veri kaynağında gezinir ve aranabilir içeriği ayıklar.

   ![BLOB dizin oluşturucu tanımı](media/search-howto-index-json/import-wizard-json-indexer.png)

Tıklayın **Tamam** Sihirbazı'nı çalıştırın ve tüm nesneleri oluşturmak için. Dizin oluşturma hemen başlar.

Portal sayfalarında veri içeri aktarma izleyebilirsiniz. İlerleme durumu bildirimlerine, dizin oluşturma durumunu ve kaç belgeler karşıya gösterir. Dizin oluşturma tamamlandığında, kullanabileceğiniz [arama Gezgini](search-explorer.md) dizininizi sorgulama için.

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST API'lerini kullanma

JSON bloblarını dizine ekleme benzer bir iş akışında üç bölümlü ortak normal belge ayıklama Azure Search'te tüm dizin oluşturucular için: bir veri kaynağı oluşturun, dizin oluşturma, dizin oluşturucu oluşturma.

Kod tabanlı JSON dizinleme için REST API için API'leri ile kullanabileceğiniz [dizin oluşturucular](https://docs.microsoft.com/rest/api/searchservice/create-indexer), [veri kaynakları](https://docs.microsoft.com/rest/api/searchservice/create-data-source), ve [dizinleri](https://docs.microsoft.com/rest/api/searchservice/create-index). Bir dizin yerinde, JSON belgelerini gönderdiğiniz sırada kabul etmeye hazır olduğunuzdan emin portal sihirbazın farklı olarak, kodu bir yaklaşım gerektirir **dizin oluşturucu oluşturma** isteği.

Azure Blob Depolama alanında JSON bloblarını genellikle tek bir JSON belge ya da bir JSON dizisi cihazlardır. Azure Search blob dizin oluşturucu nasıl belirlediği ayarlara bağlı olarak ya da yapı ayrıştırabilirsiniz **parsingMode** istek parametresi.

| JSON belgesi | parsingMode | Açıklama | Kullanılabilirlik |
|--------------|-------------|--------------|--------------|
| Bir blob başına | `json` | JSON BLOB'ları, tek bir metin parçası ayrıştırır. Her bir JSON blob tek bir Azure Search belge olur. | Hem de genel kullanıma [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API'leri. |
| Birden çok blob başına | `jsonArray` | Burada dizideki her öğe ayrı bir Azure Search belge olur ve blob'daki bir JSON dizisi ayrıştırır.  | Hem de genel kullanıma [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API'leri. |


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

### <a name="step-2-create-a-target-search-index"></a>2. Adım: Bir hedef search dizini oluşturma 

Dizin oluşturucular bir dizin şeması ile eşleştirilmelidir. API (portal yerine) kullanıyorsanız, dizin dizin oluşturucu işlemi belirtebilirsiniz böylece önceden hazırlayın.

Dizini, Azure Search aranabilir içeriği depolar. Bir dizin oluşturmak için bir belge, öznitelikleri ve arama deneyimini şekil diğer yapıları alanları belirten bir şema belirtin. Kaynak olarak aynı alan adlarını ve veri türlerini içeren dizin oluşturma, dizin oluşturucunun açıkça alanlarını eşleme gerek kalmadan işi kaydetme kaynak ve hedef alanlarını eşleşir.

Aşağıdaki örnekte gösterildiği bir [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) isteği. Dizin aranabilir bir sahip `content` bloblarından ayıklanan metinleri saklamak için alan:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="step-3-configure-and-run-the-indexer"></a>3. Adım: Yapılandırma ve dizin oluşturucuyu çalıştırma

Şimdiye kadar veri kaynağı ve dizin tanımlarını parsingMode belirsiz olmuştur. Ancak, adım 3'te dizin oluşturucu yapılandırmasını için JSON blob ayrıştırılır ve bir Azure Search dizini içinde yapılandırılmış içerik nasıl istediğinizi bağlı olarak yol kareninkinden. Seçenekler şunlardır `json` veya `jsonArray`:

+ Ayarlama **parsingMode** için `json` her blob olarak tek bir belge dizini oluşturmak için.

+ Ayarlama **parsingMode** için `jsonArray` JSON dizileri bloblarınızın oluşur ve Azure Search'te ayrı bir belge olmak dizinin her öğesi gerekir. Bir belgenin, arama sonuçlarındaki tek öğe olarak düşünebilirsiniz. Bağımsız bir öğe olarak arama sonuçlarında görünmesini dizideki her öğe istiyorsanız, ardından kullanmak `jsonArray` seçeneği.

Dizin Oluşturucu tanımı içinde isteğe bağlı olarak kullanabileceğiniz **alan eşlemeleri** hedef arama dizininizi doldurmak için kullanılan kaynak JSON belgesinin hangi özellikleri seçmek için. JSON dizileri için alt düzey özelliği olarak, dizi varsa, dizi içinde blob nereye yerleştirileceğini gösteren bir belge kökü ayarlayabilirsiniz.

> [!IMPORTANT]
> Kullanırken `json` veya `jsonArray` ayrıştırma modu, Azure Search içeren JSON veri kaynağındaki tüm blobları varsayar. Aynı veri kaynağındaki JSON ve JSON olmayan bloblar bir karışımını desteklemeniz gerekiyorsa, üzerinde bize [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).


### <a name="how-to-parse-single-json-blobs"></a>Tek bir JSON bloblarını ayrıştırmayı

Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) JSON BLOB'ları, tek bir metin parçası ayrıştırır. Genellikle, JSON belgeleri yapısını korumak isteyebilirsiniz. Örneğin, Azure Blob Depolama alanında aşağıdaki JSON belgesini olduğunu varsayın:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Blob dizin oluşturucu, tek bir Azure Search belgeye JSON belgesini ayrıştırır. Dizin oluşturucunun, dizin "metin", "datePublished" ve "aynı şekilde adlandırılmış ve yazılmış hedef dizin alanları karşı kaynak etiketleri" eşleştirerek yükler.

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

### <a name="how-to-parse-json-arrays"></a>JSON dizileri ayrıştırmayı

Alternatif olarak, JSON dizisi özelliğini seçebilirsiniz. Bu özellik bloblar içerdiğinde yararlıdır bir *JSON nesne dizisi*, ve her öğe ayrı bir Azure Search belge olmasını istiyorsanız. Örneğin, aşağıdaki JSON blob göz önünde bulundurulduğunda, Azure Search dizininizi "id" ve "metin" alanları her üç ayrı belgeler ile doldurabilirsiniz.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Bir JSON dizisi için dizin oluşturucu tanımı aşağıdaki örneğe benzer olmalıdır. ParsingMode parametresinin belirttiği bildirimi `jsonArray` ayrıştırıcı. Doğru ayrıştırıcı belirleme ve doğru verilere sahip giriş JSON bloblarını dizine ekleme için yalnızca iki dizi özgü gereksinimler şunlardır.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Yine, alan eşlemelerini atlanabilir dikkat edin. Dizin ile aynı şekilde adlandırılmış "id" ve "metin" alanları varsayıldığında, blob dizin oluşturucu, bir açık alan eşleme listesi olmadan doğru eşleme çıkarabilir.

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

### <a name="using-field-mappings-to-build-search-documents"></a>Search belgeleri oluşturmak için alan eşlemelerini kullanma

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

Eşlemeleri kaynak alan adlarını kullanarak belirtilen [JSON işaretçisi](https://tools.ietf.org/html/rfc6901) gösterimi. JSON belgenizin kök başvurmak için eğik çizgiyle başlatın ve ardından İleri eğik ayrılmış yol kullanarak (en iç içe geçme düzeyine isteğe bağlı) istenen özellik seçin.

Ayrıca, sıfır tabanlı dizinini kullanarak tek bir dizi öğelerine başvurabilir. Örneğin, yukarıdaki örnekteki "tags" dizinin ilk öğesi seçmek için şunun gibi bir alan eşlemesi kullanın:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Bir kaynak alan adı alan eşleme yolu, JSON'da mevcut olmayan bir özelliğe başvuruyorsa, bu eşlemenin bir hata atlandı. Bu, böylece (Genel kullanım örneği olan) farklı bir şema belgelerle destekliyoruz gerçekleştirilir. Doğrulama olduğundan halletmeniz alan eşleme belirtiminde hatalarını önlemek gerekir.
>
>

### <a name="rest-example-indexer-request-with-field-mappings"></a>REST örnek: Alan eşlemelerini istekle dizin oluşturucu

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

<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>.NET SDK kullanma

.NET SDK'sı, tam olarak REST API ile eşlik vardır. Kavramlar ve iş akışı gereksinimlerini öğrenmek için önceki REST API bölümde gözden geçirmenizi öneririz. Yönetilen kodda bir JSON dizin oluşturucu uygulamak için aşağıdaki .NET API başvuru belgelerine başvurabilirsiniz.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te dizin oluşturucular](search-indexer-overview.md)
+ [Azure arama ile Azure Blob Depolama dizini oluşturma](search-howto-index-json-blobs.md)
+ [Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
+ [Öğretici: Azure Blob depolamadan yarı yapılandırılmış verileri arama](search-semi-structured-data.md)
