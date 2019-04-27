---
title: JSON bloblarını dizine ekleme gelen tam metin arama - Azure Search için Azure Blob dizin oluşturucu
description: Azure Search Blob Dizin Oluşturucu kullanarak metin içeriği için Azure JSON bloblarını gezinin. Dizin oluşturucular veri alımı Azure Blob Depolama gibi seçili veri kaynakları için otomatik hale getirin.
ms.date: 04/11/2019
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 6db86d3e5aba1a2e43e69e71df8cc516fb14581f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871658"
---
# <a name="how-to-index-json-blobs-using-azure-search-blob-indexer"></a>Azure Search Blob Dizin Oluşturucu kullanarak JSON bloblarını dizinleme
Bu makalede bir Azure Search blob yapılandırma işlemi gösterilmektedir [dizin oluşturucu](search-indexer-overview.md) JSON belgeleri olarak Azure Blob depolama alanından yapılandırılmış içeriği ayıklamak ve Azure Search aranabilir hale getirin. Bu iş akışı, bir Azure Search dizini oluşturur ve JSON bloblarından ayıklanan mevcut metinle yükler. 

Kullanabileceğiniz [portalı](#json-indexer-portal), [REST API'leri](#json-indexer-rest), veya [.NET SDK'sı](#json-indexer-dotnet) dizin JSON içeriği. JSON belgeleri bir Azure depolama hesabındaki bir blob kapsayıcısında bulunan tüm yaklaşımları ortak olur. Diğer Azure dışı platformlardan JSON belgelerini dağıtmaya ilişkin yönergeler için bkz. [Azure Search'te verileri içeri](search-what-is-data-import.md).

Azure Blob Depolama alanında JSON bloblarını genellikle tek bir JSON belge ya da JSON varlıklar koleksiyonu cihazlardır. JSON koleksiyonlar için blob olabilir bir **dizi** doğru biçimlendirilmiş JSON öğelerinin. Blobları bir yeni satır ile ayırarak birden çok bağımsız JSON varlıkların da oluşan. Azure Search blob dizin oluşturucu nasıl belirlediği ayarlara bağlı olarak tüm bu yapı, ayrıştırabilir **parsingMode** istek parametresi.

> [!IMPORTANT]
> `json` ve `jsonArray` ayrıştırma modları genel olarak kullanılabilir, ancak `jsonLines` ayrıştırma modu genel Önizleme aşamasındadır ve üretim ortamlarında kullanılmamalıdır. Daha fazla bilgi için [REST API Sürüm 2017-11-11-Preview =](search-api-2017-11-11-preview.md). 

> [!NOTE]
> Dizin Oluşturucu yapılandırma önerileri izleyin [bire çok dizin](search-howto-index-one-to-many-blobs.md) birden çok arama belgeden bir Azure blob çıktı olarak.

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Portalı kullanma

JSON belgelerini için en kolay yöntem Sihirbazı'nda kullanmaktır [Azure portalında](https://portal.azure.com/). Azure blob kapsayıcısı meta verilerinde ayrıştırma tarafından [ **verileri içeri aktarma** ](search-import-data-portal.md) Sihirbazı varsayılan bir dizin oluşturmak, kaynak alanları hedef dizin alanlarına eşleme ve tek bir işlemde dizini yükleme. Boyutu ve kaynak verilerin karmaşıklığına bağlı olarak birkaç dakika içinde bir işletimsel tam metin arama dizinini olabilir.

Azure Search, hem de Azure depolama, tercihen aynı bölgede aynı Azure aboneliği kullanmanızı öneririz.

### <a name="1---prepare-source-data"></a>1 - kaynak verileri hazırlama

1. [Azure portalında oturum açın](https://portal.azure.com/).

1. [Bir Blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) verilerinizi içermesi için. Genel erişim düzeyi geçerli değerleri için ayarlanabilir.

Depolama hesabı adı, kapsayıcı adı ve verilerinizi almak için bir erişim anahtarı gerekir **verileri içeri aktarma** Sihirbazı.

### <a name="2---start-import-data-wizard"></a>2 - Veri Alma Sihirbazını Başlat

Azure Search hizmetinizin genel bakış sayfasından [Sihirbazı başlatın](search-import-data-portal.md) komut çubuğunda veya tıklayarak **Azure Search Ekle** içinde **Blob hizmeti** bölümünü, Depolama hesabı sol gezinti bölmesindeki.

   ![İçeri aktarma Portalı'nda veri komut](./media/search-import-data-portal/import-data-cmd2.png "veri içeri aktarma Sihirbazını Başlat")

### <a name="3---set-the-data-source"></a>3 - veri kaynağı ayarla

İçinde **veri kaynağı** sayfasında, kaynak olmalıdır **Azure Blob Depolama**, aşağıdaki özellikleri ile:

+ **Ayıklanacak veri** olmalıdır *içerik ve meta verileri*. Bu seçeneğin belirlenmesi, bir dizin şemasını ve içeri aktarma için alanları eşlemek bir sihirbaz sağlar.
   
+ **Ayrıştırma modu** ayarlanmalıdır *JSON*, *JSON dizisi* veya *JSON satırları*. 

  *JSON* arama sonuçlarında bağımsız bir öğe olarak gösteren, bir tek arama belge olarak her blob korumadaki. 

  *JSON dizisi* doğru biçimlendirilmiş JSON - doğru biçimlendirilmiş JSON verilerini içeren BLOB nesnelerinin bir dizisi için karşılık gelen veya bir nesne dizisi olan bir özelliğe sahiptir ve istediğiniz her öğe bir bağımsız, bağımsız aramayı belge geliştirilmiştir. Karmaşık BLOB'ları ve tercih etseniz *JSON dizisi* tüm blob tek bir belge alınır.

  *JSON satır* yeni-satır ile ayırarak birden çok JSON varlık blobları oluşan aranır, her varlık bir tek başına bağımsız arama belgesi olarak geliştirilmiştir istediğiniz. Karmaşık BLOB'ları ve tercih etseniz *JSON satırları* tek bir belge modu, ardından tüm blob ayrıştırma içe alınan.
   
+ **Depolama kapsayıcısı** , depolama hesabı ve kapsayıcı veya kapsayıcıya gideren bir bağlantı dizesi belirtmeniz gerekir. Blob hizmeti portal sayfasında, bağlantı dizeleri alabilirsiniz.

   ![BLOB veri kaynağı tanımını](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - sihirbazında "Bilişsel arama Ekle" sayfasını atlayın

Bilişsel beceriler ekleme JSON belgesi almak için gerekli değildir. Belirli bir gerek olmadığı sürece [Bilişsel hizmetler API'leri ve dönüştürmeler dahil](cognitive-search-concept-intro.md) , dizinleme işlem hattına bu adımı atlayın.

İlk adımı atlamak için bir sonraki sayfasına gidin.

   ![Bilişsel arama için sonraki sayfa düğmesi](media/search-get-started-portal/next-button-add-cog-search.png)

Bu sayfadan dizini özelleştirme, İleri atlayabilirsiniz.

   ![Bilişsel beceri adımını atlama](media/search-get-started-portal/skip-cog-skill-step.png)

### <a name="5---set-index-attributes"></a>5 - dizin öznitelikleri Ayarla

İçinde **dizin** sayfasında, bir veri türü ve dizin öznitelikleri ayarlamaya yönelik onay kutularından oluşan bir serinin alanların listesini görmelisiniz. Sihirbaz, meta veriler kaynak veri örnekleme tarafından temel bir alanlar listesi oluşturabilirsiniz. 

Toplu öznitelikler özniteliği sütun üst kısmındaki onay kutusuna tıklayarak seçimi. Seçin **alınabilir** ve **aranabilir** bir istemci uygulaması ve tam metin arama işleme tabi döndürülmesi gereken her alan için. Tamsayı tam metin olmadığını fark edeceksiniz veya belirsiz aranabilir (sayılar verbatim değerlendirilir ve genellikle filtreleri kullanışlıdır).

Açıklamasını inceleyin [dizin öznitelikleridir](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) ve [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) daha fazla bilgi için. 

Seçimlerinizi gözden geçirmek için bir dakikanızı ayırın. Sihirbazı çalıştırdıktan sonra fiziksel veri yapılarını oluşturulur ve bu alanlar, bırakarak ve tüm nesneleri yeniden düzenlemek mümkün olmayacaktır.

   ![BLOB dizin tanımı](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - dizin oluşturucu oluşturma

Tam olarak belirtilen, sihirbaz arama hizmetinizdeki üç farklı bir nesne oluşturur. Bir veri kaynağı nesnesi ve dizin nesnesi, Azure Search hizmetinizde adlandırılmış kaynaklar olarak kaydedilir. Son adım, bir dizin oluşturucu nesnesini oluşturur. Dizin Oluşturucu adlandırma zamanlayabilir ve Yönet Sihirbazı sırayla oluşturulan dizin ve veri kaynağı nesnesi bağımsız olarak tek başına kaynak olarak mevcut izin verir.

Dizin oluşturucular ile aşina değilseniz bir *dizin oluşturucu* aranabilir içeriği için bir dış veri kaynağında gezinir Azure Search'te bir kaynaktır. Çıkışı **verileri içeri aktarma** sihirbazıdır bir dizin oluşturucu, bir Azure Search dizini aktarır, JSON veri kaynağında gezinir ve aranabilir içeriği ayıklar.

   ![BLOB dizin oluşturucu tanımı](media/search-howto-index-json/import-wizard-json-indexer.png)

Tıklayın **Tamam** Sihirbazı'nı çalıştırın ve tüm nesneleri oluşturmak için. Dizin oluşturma hemen başlar.

Portal sayfalarında veri içeri aktarma izleyebilirsiniz. İlerleme durumu bildirimlerine, dizin oluşturma durumunu ve kaç belgeler karşıya gösterir. 

Dizin oluşturma tamamlandığında, kullanabileceğiniz [arama Gezgini](search-explorer.md) dizininizi sorgulama için.

> [!NOTE]
> Beklediğiniz verileri görmüyorsanız, daha fazla öznitelik ayarlama hakkında daha fazla alan gerekebilir. Yalnızca oluşturuldu ve 5. adımında dizin öznitelikleri için yaptığınız seçimleri değiştirme Sihirbazı yeniden adım dizin oluşturucu ve dizini silin. 

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST API'lerini kullanma

Azure Search'te tüm dizin oluşturucular için üç bölümü iş akışı ortak aşağıdaki JSON bloblarını dizine için REST API kullanabilirsiniz: bir veri kaynağı oluşturun, dizin oluşturma, dizin oluşturucu oluşturma. Blob depolamadan/depolamaya veri ayıklama, dizin oluşturucu oluşturma isteği gönderirseniz oluşur. Bu istek tamamlandıktan sonra sorgulanabilir bir dizine sahip. 

Gözden geçirebilirsiniz [REST örnek kod](#rest-example) sonunda, bu bölümde, tüm üç nesne oluşturma işlemi gösterilmektedir. Bu bölüm hakkında ayrıntılar da içerir. [JSON ayrıştırma modları](#parsing-modes), [tek bloblar](#parsing-single-blobs), [JSON dizileri](#parsing-arrays), ve [iç içe dizi](#nested-json-arrays).

Kod tabanlı JSON dizin oluşturma için kullanmak [Postman](search-fiddler.md) ve bu nesneler oluşturmak için REST API:

+ [Dizin](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [Veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source)
+ [Dizin Oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

İşlemlerin sırasını oluşturmak ve bu sırada nesnelerin çağrı gerektirir. Portal iş akışı farklı olarak, JSON belgelerini gönderilen aracılığıyla kabul etmek için kullanılabilir dizin kodu yaklaşımı gerektirir **dizin oluşturucu oluşturma** isteği.

Azure Blob Depolama alanında JSON bloblarını genellikle tek bir JSON belge ya da bir JSON "array" cihazlardır. Azure Search blob dizin oluşturucu nasıl belirlediği ayarlara bağlı olarak ya da yapı ayrıştırabilirsiniz **parsingMode** istek parametresi.

| JSON belgesi | parsingMode | Açıklama | Kullanılabilirlik |
|--------------|-------------|--------------|--------------|
| Bir blob başına | `json` | JSON BLOB'ları, tek bir metin parçası ayrıştırır. Her bir JSON blob tek bir Azure Search belge olur. | Hem de genel kullanıma [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK. |
| Birden çok blob başına | `jsonArray` | Burada dizideki her öğe ayrı bir Azure Search belge olur ve blob'daki bir JSON dizisi ayrıştırır.  | Hem de önizlemeye sunulan [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK. |
| Birden çok blob başına | `jsonLines` | Burada her varlık ayrı bir Azure Search belge olur bir satır ile ayırarak birden çok JSON varlık (bir "array") içeren bir blob ayrıştırır. | Hem de önizlemeye sunulan [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API ve [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK. |

### <a name="1---assemble-inputs-for-the-request"></a>1 - giriş istek için bir araya getirin

Her istek için hizmet adını ve yönetici anahtarını Azure arama (POST üstbilgisinde) ve depolama hesabı adı ve blob depolama anahtarı sağlamanız gerekir. Kullanabileceğiniz [Postman](search-fiddler.md) Azure Search için HTTP istekleri göndermek için.

Bir istek yapıştırabilmek aşağıdaki dört değerleri Not Defteri'ne kopyalayın:

+ Azure arama hizmeti adı
+ Azure arama yönetici anahtarı
+ Azure depolama hesabı adı
+ Azure depolama hesabı anahtarı

Bu değerleri portalda bulabilirsiniz:

1. Azure Search portal sayfalarında arama hizmeti URL'si genel bakış sayfasından kopyalayın.

2. Sol gezinti bölmesinden **anahtarları** ve ardından (bunlar eşdeğerdir) ya da birincil veya ikincil anahtarı kopyalayın.

3. Depolama hesabınız için portal sayfalarına geçin. Sol gezinti bölmesindeki altında **ayarları**, tıklayın **erişim anahtarlarını**. Bu sayfa hesap adı ve anahtarı sağlar. Depolama hesabı adı ve anahtarlarından birini Not Defteri'ne kopyalayın.

### <a name="2---create-a-data-source"></a>2 - bir veri kaynağı oluşturma

Bu adım, dizin oluşturucu tarafından kullanılan veri kaynağı bağlantı bilgileri sağlar. Veri kaynağı bağlantı bilgilerini devam eden bir Azure Search adlandırılmış bir nesnedir. Veri kaynağı türü, `azureblob`, hangi veri ayıklama davranışları Dizin Oluşturucu tarafından çağrılan belirler. 

Hizmet adı, yönetici anahtarı, depolama hesabı için geçerli değerler yerine ve önemli yer tutucuları hesap.

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="3---create-a-target-search-index"></a>3 - bir hedef arama dizini oluşturma 

Dizin oluşturucular bir dizin şeması ile eşleştirilmelidir. API (portal yerine) kullanıyorsanız, dizin dizin oluşturucu işlemi belirtebilirsiniz böylece önceden hazırlayın.

Dizini, Azure Search aranabilir içeriği depolar. Bir dizin oluşturmak için bir belge, öznitelikleri ve arama deneyimini şekil diğer yapıları alanları belirten bir şema belirtin. Kaynak olarak aynı alan adlarını ve veri türlerini içeren dizin oluşturma, dizin oluşturucunun açıkça alanlarını eşleme gerek kalmadan işi kaydetme kaynak ve hedef alanlarını eşleşir.

Aşağıdaki örnekte gösterildiği bir [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) isteği. Dizin aranabilir bir sahip `content` bloblarından ayıklanan metinleri saklamak için alan:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="4---configure-and-run-the-indexer"></a>4 - yapılandırın ve dizin oluşturucuyu çalıştırma

Dizin ile bir veri kaynağı ve dizin oluşturucu olduğu gibi aynı zamanda adlandırılmış bir nesne oluşturun ve Azure Search Hizmeti üzerinde yeniden kullanın. Bir dizin oluşturucu oluşturmak için tam olarak belirtilen bir istek gibi görünebilir:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Dizin Oluşturucu, istek gövdesinde yapılandırmadır. Bir veri kaynağı ve Azure arama'yı zaten var olan boş hedef dizin gerektirir. 

Zamanlama ve parametreler isteğe bağlıdır. Bunları atlarsanız, dizin oluşturucu hemen kullanarak çalışır `json` ayrıştırma modu.

Bu belirli bir dizin oluşturucu, alan eşlemelerini içermez. Dizin Oluşturucu tanımı içinde bırakabilirsiniz **alan eşlemeleri** kaynak JSON belge özelliklerini hedef arama dizininizin alanlarıyla eşleşiyorsa. 


### <a name="rest-example"></a>REST örneği

Bu bölüm, nesneleri oluşturmak için kullanılan tüm isteklerin yeniden anımsamak yöneliktir. Bu makalenin önceki bölümlerinde bileşen parçalarına tartışma için bkz.

### <a name="data-source-request"></a>Veri kaynağı isteği

Tüm dizin oluşturucular, var olan veri bağlantı bilgilerini sağlayan bir veri kaynağı nesnesi gerektirir. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }  


### <a name="index-request"></a>Dizin isteği

Tüm dizin oluşturucular veri alan bir hedef dizin gerektirir. İstek gövdesi, arama yapılabilir bir dizin istenen davranışları desteklemek için öznitelikli alanlar, oluşan dizin şemasını tanımlar. Dizin Oluşturucu çalıştırdığınızda bu dizini boş olmamalıdır. 

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="indexer-request"></a>Dizin Oluşturucu isteği

Bu istek tam olarak belirtilen bir dizin oluşturucuyu gösterir. Önceki örneklerde atlanmış alan eşlemeleri içerir. Bu "zamanlama", "parameters" geri çağırma ve kullanılabilir bir varsayılan var olduğu sürece "fieldMappings" isteğe bağlıdır. "Zamanlama" atlama hemen çalıştırmak için dizin oluşturucuyu neden olur. "ParsingMode" atlama "json" varsayılan dizin neden olur.

Azure Search'te dizin oluşturucuyu oluşturma, veri alma işlemi tetikler. Bir sağlamışsanız hemen ve bundan sonra bir zamanlamaya göre çalıştırır.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

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

<a name="parsing-modes"></a>

## <a name="parsing-modes"></a>Ayrıştırma modları

JSON bloblarını birden çok form varsayabilirsiniz. **ParsingMode** JSON dizin oluşturucu parametresi JSON blob içeriği nasıl ayrıştırılır ve bir Azure Search dizini içinde yapılandırılmış belirler:

| parsingMode | Açıklama |
|-------------|-------------|
| `json`  | Her blob tek bir belge dizini. Bu varsayılan değerdir. |
| `jsonArray` | JSON dizileri bloblarınızın oluşur ve Azure Search ayrı bir belge olmak dizinin her öğesi ihtiyacınız varsa bu modu seçin. |
|`jsonLines` | Bu mod, yeni satırla ayrılan, birden çok JSON varlık bloblarınızın oluşur ve her varlık, Azure Search'te ayrı bir belge olmak gerekirse seçin. |

Bir belgenin, arama sonuçlarındaki tek öğe olarak düşünebilirsiniz. Bağımsız bir öğe olarak arama sonuçlarında görünmesini dizideki her öğe istiyorsanız, ardından kullanmak `jsonArray` veya `jsonLines` seçeneklerinden uygun olanını.

Dizin Oluşturucu tanımı içinde isteğe bağlı olarak kullanabileceğiniz [alan eşlemeleri](search-indexer-field-mappings.md) hedef arama dizininizi doldurmak için kullanılan kaynak JSON belgesinin hangi özellikleri seçmek için. İçin `jsonArray` modu, alt düzey özelliği olarak dizi varsa, ayrıştırma, dizi içinde blob nereye yerleştirileceğini gösteren bir belge kökü ayarlayabilirsiniz.

> [!IMPORTANT]
> Kullanırken `json`, `jsonArray` veya `jsonLines` ayrıştırma modu, Azure Search içeren JSON veri kaynağındaki tüm blobları varsayar. Aynı veri kaynağındaki JSON ve JSON olmayan bloblar bir karışımını desteklemeniz gerekiyorsa, üzerinde bize [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).


<a name="parsing-single-blobs"></a>

## <a name="parse-single-json-blobs"></a>Tek bir JSON bloblarını ayrıştırılamıyor

Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) JSON BLOB'ları, tek bir metin parçası ayrıştırır. Genellikle, JSON belgeleri yapısını korumak isteyebilirsiniz. Örneğin, Azure Blob Depolama alanında aşağıdaki JSON belgesini olduğunu varsayın:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Blob dizin oluşturucu, tek bir Azure Search belgeye JSON belgesini ayrıştırır. Dizin oluşturucunun, dizin "metin", "datePublished" ve "aynı şekilde adlandırılmış ve yazılmış hedef dizin alanları karşı kaynak etiketleri" eşleştirerek yükler.

Alan eşlemelerini belirtildiği gibi gerekli değildir. Dizin ile "metin" verilen "datePublished ve"etiketleri"alanları, blob dizin oluşturucu tanım Çıkarsama doğru eşleme olmadan bir alan istekteki eşleme.

<a name="parsing-arrays"></a>

## <a name="parse-json-arrays"></a>JSON dizileri ayrıştırılamıyor

Alternatif olarak, JSON dizisi seçeneğini kullanabilirsiniz. Blobları içerdiğinde, bu seçenek kullanışlıdır bir *doğru biçimlendirilmiş JSON nesne dizisi*, ve her öğe ayrı bir Azure Search belge olmasını istiyorsunuz. Örneğin, aşağıdaki JSON blob göz önünde bulundurulduğunda, Azure Search dizininizi "id" ve "metin" alanları her üç ayrı belgeler ile doldurabilirsiniz.  

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

## <a name="parse-nested-arrays"></a>İç içe geçen diziler ayrıştırılamıyor
JSON'öğeleri iç içe geçmiş dizi için belirtebileceğiniz bir `documentRoot` çok düzeyli yapısı belirtmek için. Örneğin, BLOB'ları şöyle görünür:

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

## <a name="parse-blobs-separated-by-newlines"></a>BLOB'ları karakterleriyle ayrılan ayrıştırılamıyor

Bir yeni satır ile ayırarak birden çok JSON varlık, blob içerir ve her öğe ayrı bir Azure Search belge olmasını istediğiniz, JSON satırları seçeneğini tercih edebilirsiniz. Örneğin, aşağıdaki blob verilen (olduğu üç farklı JSON varlıklar), Azure Search dizininizi "id" ve "metin" alanları her üç ayrı belgeler ile doldurabilirsiniz.

    { "id" : "1", "text" : "example 1" }
    { "id" : "2", "text" : "example 2" }
    { "id" : "3", "text" : "example 3" }

JSON satırlar için dizin oluşturucu tanımı aşağıdaki örneğe benzer olmalıdır. ParsingMode parametresinin belirttiği bildirimi `jsonLines` ayrıştırıcı. 

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonLines" } }
    }

Alan eşlemelerini olabileceğini belirtilmemişse, benzer şekilde yeniden fark `jsonArray` ayrıştırma modu.

## <a name="add-field-mappings"></a>Alan eşlemelerini ekleyin

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

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te dizin oluşturucular](search-indexer-overview.md)
+ [Azure arama ile Azure Blob Depolama dizini oluşturma](search-howto-index-json-blobs.md)
+ [Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
+ [Öğretici: Azure Blob depolamadan yarı yapılandırılmış verileri arama](search-semi-structured-data.md)
