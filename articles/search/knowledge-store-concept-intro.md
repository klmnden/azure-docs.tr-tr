---
title: Bilgi Bankası Store giriş ve genel bakış - Azure Search
description: Burada, görüntüleyebilir, yeniden şekillendirmek ve zenginleştirilmiş belgeleri Azure Search'te ve diğer uygulamalarda kullanmak için Azure depolama zenginleştirilmiş belgeleri gönderin.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: overview
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 3000016de934aaa3faab96821f9747ea4b571ef7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027001"
---
# <a name="what-is-knowledge-store-in-azure-search"></a>Bilgi Bankası Store Azure Search nedir?

Bilgi Bankası Store zenginleştirilmiş belgeleri ve bir yapay ZEKA tabanlı dizini oluşturma ardışık düzeni tarafından oluşturulan meta veri kaydeder, şu anda genel önizlemede Azure Search, isteğe bağlı bir özellik olan [(bilişsel arama)](cognitive-search-concept-intro.md). Bilgi Bankası Store, işlem hattının bir parçası yapılandırdığınız Azure depolama hesabı tarafından desteklenir. Etkin olduğunda, arama hizmeti zenginleştirilmiş her belgenin gösterimini önbelleğe almak için bu depolama hesabı kullanır. 

Bilişsel arama daha önce kullanmadıysanız, uzmanlık becerileri zenginleştirmelerinin bir dizi aracılığıyla bir belge taşımak için kullanılabilir biliyorsunuzdur. Bir Azure Search dizini sonucu olabilir veya (yeni bu önizlemeye) Computers.csv'deki bilgi deposu.

Verileri bir aşağı akış uygulaması tüketim yapılandırılması için mekanizmanızı projeksiyonlardır. Kullanabileceğiniz [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) Azure depolama veya Azure Depolama'ya bağlanan herhangi bir uygulama için oluşturulan belgeleri zenginleştirilmiş tüketim için yeni olanaklar sunar açılır. Veri bilimi işlem hatları ve özel analiz bazı örnekler verilmiştir.

![İşlem hattı diyagramdaki bilgi Store](./media/knowledge-store-concept-intro/annotationstore_sans_internalcache.png "bilgi Store, işlem hattı diyagramı")

Bilgi Bankası Store kullanmak için ekleme bir `knowledgeStore` öğesi için bir dizin oluşturma işlem hattında tarafınızdaki işlemleri tanımlayan bir beceri kümesi. Yürütme sırasında Azure Search, Azure depolama hesabınızdaki bir alanı oluşturur ve tanımları ve işlem hattı tarafından oluşturulan içeriği ile doldurur.

## <a name="benefits-of-knowledge-store"></a>Bilgi Bankası Store avantajları

Bir bilgi depolamak, yapısı, içerik ve gerçek içerik - BLOB'ları, analiz, yapılmıştır görüntü dosyaları gibi yapılandırılmamış ve yarı yapılandırılmış veri dosyalarından öğrendiğiniz kayıtlar veya hatta yeni formlarına yeniden şekillendirilebilir veri yapılandırılmış sağlar. İçinde bir [adım adım](knowledge-store-howto.md) Bu önizleme için yazılan, birinci elden nasıl yoğun bir JSON belgesi substructures yeni yapılara yeniden oluşturulur, içine ayrıldıktan ve aksi takdirde aşağı yönde için kullanılabilir duruma görebilirsiniz işlemler, makine öğrenimi ve veri bilimi iş yükleri ister.

Yapay ZEKA tabanlı bir dizin oluşturma ardışık ne üretebilir görmek kullanışlı olsa da, Bilgi Bankası Store gerçek gücünü, verileri şekillendirmek yeteneğidir. Temel bir beceri kümesi ile başlatın ve ardından yeni yapılar, Azure Search yanı sıra diğer uygulamalarda tüketilebilir ardından birleştirebilirsiniz yapısı, artan düzeyleri eklemek için gezinilen.

Numaralandırılan, Bilgi Bankası Store avantajları şunları içerir:

+ Zenginleştirilmiş belgelerinde kullanma [analiz ve Raporlama araçları](#tools-and-apps) arama dışında. Power Query ile Power BI ilgi çekici bir seçimdir, ancak herhangi bir aracı veya Azure depolama alanına bağlanan bir uygulama, oluşturduğunuz bir Bilgi Bankası deposundan çekebilirsiniz.

+ Yapay ZEKA dizin bir işlem hattı adımları ve beceri kümesi tanımları hata ayıklama sırasında daraltın. Bilgi deposunu yapay ZEKA dizin bir işlem hattında bir beceri kümesi tanımının ürün gösterir. Sonuçlar, tam olarak zenginleştirmelerinin nasıl göründüğünü görebilirsiniz çünkü daha iyi bir beceri kümesi tasarlamak için kullanabilirsiniz. Kullanabileceğiniz [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) bilgi deposunun içeriğini görüntülemek için Azure depolama alanında.

+ Yeni formlarına şekillendirin. Yeniden şekillendiren becerilerini içinde kod oluşturulmuş, ancak bir beceri kümesi artık bu olanağı sağlayan noktasıdır. [Shaper beceri](cognitive-search-skill-shaper.md) Azure Search'te bu görevi uyum sağlayacak şekilde genişletilmiştir. Yeniden şekillendirme, ilişki korurken kullanım amacınızı verileri ile eşleşen bir projeksiyon tanımlamanızı sağlar.

> [!Note]
> Bilişsel hizmetler kullanarak yapay ZEKA tabanlı dizin ile tanıdık olmayan? Azure arama, ayıklamak ve görüntü dosyaları, varlık tanıma ve anahtar ifade ayıklama metin dosyalarından optik karakter tanıma (OCR) ve daha fazlasını kullanarak kaynak verileri zenginleştirmek için Bilişsel hizmetler görüntü ve dil özellikleri ile tümleştirilir. Daha fazla bilgi için [bilişsel arama nedir?](cognitive-search-concept-intro.md).

## <a name="create-a-knowledge-store"></a>Bilgi Bankası deposu oluşturma

Bilgi mağaza beceri kümesi tanımı'nın bir parçasıdır. Bu önizleme sürümünde oluşturma REST API'sini gerektirir kullanarak `api-version=2019-05-06-Preview` veya **verileri içeri aktarma** portalındaki Sihirbazı.

Aşağıdaki JSON belirtir bir `knowledgeStore`, hangi (gösterilmemiştir) bir dizin oluşturucu tarafından çağrılan bir beceri kümesi bir parçasıdır. Projeksiyonlar içinde belirtimi `knowledgeStore` Azure depolama tabloları veya nesneler oluşturulup oluşturulmayacağını belirler.

Zaten yapay ZEKA tabanlı dizin oluşturma ile ilgili bilgi sahibi olduğunuz, beceri kümesi tanımının oluşturulması, kuruluş ve madde temini zenginleştirilmiş her belgenin belirler.

```json
{
  "name": "my-new-skillset",
  "description": 
  "Example showing knowledgeStore placement, supported in api-version=2019-05-06-Preview. You need at least one skill, most likely a Shaper skill if you are modulating data structures.",
  "skills":
  [
    {
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document/content/phrases/*",
    "inputs": [
        {
        "name": "text",
        "source": "/document/content/phrases/*"
        },
        {
        "name": "sentiment",
        "source": "/document/content/phrases/*/sentiment"
        }
    ],
    "outputs": [
        {
        "name": "output",
        "targetName": "analyzedText"
        }
    ]
    },
  ],
  "cognitiveServices": 
    {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "description": "mycogsvcs resource in West US 2",
    "key": "<your key goes here>"
    },
  "knowledgeStore": { 
    "storageConnectionString": "<your connection string goes here>", 
    "projections": [ 
        { 
            "tables": [  
            { "tableName": "Reviews", "generatedKeyName": "ReviewId", "source": "/document/Review" , "sourceContext": null, "inputs": []}, 
            { "tableName": "Sentences", "generatedKeyName": "SentenceId", "source": "/document/Review/Sentences/*", "sourceContext": null, "inputs": []}, 
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/Review/Sentences/*/KeyPhrases", "sourceContext": null, "inputs": []}, 
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/Review/Sentences/*/Entities/*" ,"sourceContext": null, "inputs": []} 

            ], 
            "objects": [ 
                { 
                "storageContainer": "Reviews", 
                "format": "json", 
                "source": "/document/Review", 
                "key": "/document/Review/Id" 
                } 
            ]      
        }    
    ]     
    } 
}
```

## <a name="components-backing-a-knowledge-store"></a>Yedekleme bilgi deposu bileşenleri

Bir Bilgi Bankası deposu oluşturmak için aşağıdaki hizmetleri ve yapıtları gerekir.

### <a name="1---source-data"></a>1 - kaynak verileri

Azure Search Dizin Oluşturucu tarafından desteklenen Azure veri kaynağı veriler veya belgeler zenginleştirmek için istediğiniz mevcut olması gerekir: 

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

* [Azure Cosmos DB](search-howto-index-cosmosdb.md)

* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)

[Azure tablo depolama](search-howto-indexing-azure-tables.md) bilgi deposundaki giden veriler için kullanılabilir ancak bir kaynak olarak yapay ZEKA tabanlı bir dizin oluşturma ardışık gelen veriler için kullanılamaz.

### <a name="2---azure-search-service"></a>2 - azure Search Hizmeti

Azure Search hizmeti ve veri zenginleştirme için kullanılan nesneleri oluşturmanız ve yapılandırmanız için REST API de gerekir. Bilgi deposunu oluşturmak için REST API `api-version=2019-05-06-Preview`.

Azure Search dizin oluşturucu özelliği sağlar ve dizin oluşturucular sürecinin tamamını-Azure Depolama'da kalıcı, zenginleştirilmiş belgeler výsledek uca, sürücü için kullanılır. Dizin oluşturucular veri kaynağı, dizin ve tümü oluşturmak ve bir Bilgi Bankası deposunun doldurulması için gerekli olan bir beceri kümesi - kullanın.

| Object | REST API | Açıklama |
|--------|----------|-------------|
| veri kaynağı | [Veri Kaynağı Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeleri oluşturmak için kullanılan kaynak verileri sağlayan Azure harici veri kaynağı tanımlayan bir kaynaktır.  |
| Beceri kümesi | [Beceri kümesi oluşturma (api sürümü 2019-05-06 =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak kullanımını koordine [yerleşik yetenekler](cognitive-search-predefined-skills.md) ve [özel bilişsel beceriler](cognitive-search-custom-skill-interface.md) dizin oluşturma sırasında bir zenginleştirme hattında kullanılan. |
| index | [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Azure Search dizini ifade şema. Kaynak veri alanları veya alanları (örneğin, bir alan için kuruluş adlarını varlık tanıma tarafından oluşturulan) zenginleştirme aşaması sırasında üretilen dizin alanları eşleyin. |
| dizin oluşturucu | [Create Indexer (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak dizin oluşturma sırasında kullanılan bileşenleri tanımlama: bir veri kaynağı, bir beceri kümesi, kaynak ve aracı veri yapılarını alan ilişkilendirme hedef dizin ve dizin de dahil olmak üzere. Dizin Oluşturucu veri alımı ve zenginleştirme tetikleyicisi çalışıyor. Çıktı, uzmanlık becerileri ile zenginleştirilmiş kaynak verilerle doldurulmuş dizin şemasını temel arama dizinidir.  |

### <a name="3---cognitive-services"></a>3 - bilişsel hizmetler

Bir beceri kümesi içinde belirtilen zenginleştirmelerinin Bilişsel hizmetler görüntü işleme ve dil özellikleri temel alır. Bilişsel hizmetler işlevselliği becerilerinizi dizin oluşturma sırasında kullanılır. Bir beceri kümesi becerileri bileşimiyse ve becerileri özel görüntü işleme ve dil özelliklerine bağlıdır. Bilişsel hizmetler tümleştirmek için yaparız [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md) bir beceri kümesi için.

### <a name="4---storage-account"></a>4 - depolama hesabı

Azure depolama hesabınız kapsamında Azure Search, bir Blob kapsayıcısı ya da bir beceri kümesi nasıl yapılandırdığınıza bağlı olarak tabloları oluşturur. Verilerinizi Azure Blob veya tablo depolama alanından geliyorsa, zaten ayarlanır. Aksi takdirde, bir Azure depolama hesabı oluşturmanız gerekecektir. Yapay ZEKA tabanlı dizini oluşturma ardışık düzeni tarafından oluşturulan zenginleştirilmiş belgeleri, tablo ve Azure storage'da içerir.

Depolama hesabı, beceri kümesi içinde belirtilir. İçinde `api-version=2019-05-06-Preview`, hesap bilgilerini sağlayabilir, böylece bir beceri kümesi tanımı bir Bilgi Bankası deposu tanımı içerir.

<a name="tools-and-apps"></a>

### <a name="5---access-and-consume"></a>5 - erişme ve kullanma

Zenginleştirmelerinin depolamada varolduğunda, herhangi bir aracı veya Azure Blob veya tablo depolama alanına bağlanan teknolojisini keşfedin, analiz etmek veya içeriği kullanmak için kullanılabilir. Bir başlangıç liste aşağıda verilmiştir:

+ [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) zenginleştirilmiş belge yapısı ve içeriği görüntülemek için. Bu bilgi depo içeriğini görüntülemek için temel aracı olarak düşünün.

+ [Power BI ile Power Query](https://support.office.com/article/connect-to-microsoft-azure-blob-storage-power-query-f8165faa-4589-47b1-86b6-7015b330d13e) doğal dil sorguları veya kullanım raporlama ve analiz araçları sayısal veri varsa.

+ [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) daha ayrıntılı işleme için.

+ Azure arama dizini kullanılarak dizine içerik üzerinde tam metin araması için [bilişsel arama](cognitive-search-concept-intro.md).

## <a name="document-persistence"></a>Belge kalıcılığı

Depolama hesabında zenginleştirmelerinin tablolarına, Azure tablo depolama veya Azure Blob depolamadaki nesnelere olarak ifade edilebilir. Geri çağırırsanız, bir kez depolanır; zenginleştirmelerinin, bir kaynak olarak diğer veritabanları ve araçları ile veri yükleme için kullanılabilir

+ Tablo depolama, tablo biçiminde veri şeması uyumlu bir gösterimini istediğinizde yararlıdır. Tablo depolama şekillendirmeyi veya yeni yollarla öğeleri yeniden birleştirmek istiyorsanız, gerekli ayrıntı düzeyi sağlar.

+ BLOB Depolama, her bir belgeyi her şeyi içeren bir JSON gösterimi oluşturur. İfadeler, çeşitli almak için bir beceri kümesi içinde her iki depolama seçenekleri kullanabilirsiniz.

+ Azure Search dizin içeriğini devam ettirir. Senaryonuz arama-ilgili olmayan, örneğin hedefiniz analizi başka bir araç ise varsa işlem hattını oluşturan dizini silebilirsiniz. Ancak ayrıca dizini korumak ve yerleşik bir aracı gibi [arama Gezgini](search-explorer.md) içeriğinizi ile etkileşim kurmak için üçüncü bir orta (arkasında, Depolama Gezgini ve analiz uygulamanızı) olarak.

Belge içeriklerini birlikte zenginleştirilmiş belgeleri zenginleştirmelerinin üretilen beceri kümesi sürüm meta verileri içerir.  

## <a name="inside-a-knowledge-store"></a>Bilgi Bankası depo içinde

Bilgi Bankası deponun bir ek açıklama önbellek ve projeksiyonları oluşur. *Önbellek* becerileri sonuçlarını önbelleğe alınması ve değişiklikleri izlemek için hizmet tarafından dahili olarak kullanılır. A *projeksiyon* şema ve kullanım amacınız eşleşen zenginleştirmelerinin yapısını tanımlar. Bilgi deposunu başına bir önbellek, ancak birden çok projeksiyonlar yoktur. 

Önbellek her zaman bir blob kapsayıcısı, ancak projeksiyonlar tablo veya nesne geliştirilmiştir:

+ Bir nesne olarak Blob Depolama, projeksiyon nesneleri veya veri bilimi işlem hattı gibi senaryolar için JSON hiyerarşik gösterimleri içinde olan, bir kapsayıcıya kaydedildiği projeksiyon eşlenir.

+ Bir tablo olarak tablo depolama birimine projeksiyon eşler. Bir tablo gösterimi ilişkileri veri analizi veya dışarı aktarma gibi senaryolar için machine learning için veri çerçevesi olarak korur. Ardından zenginleştirilmiş projeksiyonlar diğer veri depolarına kolayca içe olabilir. 

Kuruluşunuzdaki çeşitli constituencies uyum sağlamak için bir Bilgi Bankası deposunda birden fazla projeksiyonlar oluşturabilirsiniz. Bir geliştirici becerilerinizi tarafından şeklinde ayrıntılı veya modüler veri yapılarını veri bilimcilerine veya analist isteyebilirsiniz zenginleştirilmiş bir belgenin tam JSON gösterimini erişimi gerekebilir.

Zenginleştirme sürecinin hedeflerinden ayrıca bir modeli eğitmek için kullanılan bir veri kümesi oluşturmak için ise, örneğin, nesne deposuna verileri yansıtmayla verileri, veri bilimi işlem hattını kullanma yollarından biri olur. Alternatif olarak, zenginleştirilmiş belgelere göre hızlı bir Power BI panosu oluşturmak istiyorsanız tablosal projeksiyon iyi çalışır.

<!---
## Data lifecycle and billing

Each time you run the indexer, the cache in Azure storage is updated if the skillset definition or underlying source data has changed. As input documents are edited or deleted, changes are propagated through the annotation cache to the projections, ensuring that your projected data is a current representation of your inputs at the end of the indexer run. 

Generally speaking, pipeline processing can be an all-or-nothing operation, but Azure Search can process incremental changes, which saves you time and money.

If a document is new or updated, all skills are run. If only the skillset changes, reprocessing is scoped to just those skills and documents affected by your edit.

### Changes to a skillset
Suppose that you have a pipeline composed of multiple skills, operating over a large body of static data (for example, scanned documents), that takes 8 hours and costs $200 to create the knowledge store. Now suppose you need to tweak one of the skills in the skillset. Rather than starting over, Azure Search can determine which skill is affected, and reprocess only that skill. Cached data and projections that are unaffected by the change remain intact in the knowledge store.

### Changes in the data
Scenarios can vary considerably, but let's suppose instead of static data, you have volatile data that changes between indexer invocations. Given no changes to the skillset, you are charged for processing the delta of new and modified document. The timestamp information varies by data source, but for illustration, in a Blob container, Azure Search looks at the `lastmodified` date to determine which blobs need to be ingested.

> [!Note]
> While you can edit the data in the projections, any edits will be overwritten on the next pipeline invocation, assuming the document in source data is updated. 

### Deletions

Although Azure Search creates and updates structures and content in Azure storage, it does not delete them. Projections and cached documents continue to exist even when the skillset is deleted. As the owner of the storage account, you should delete a projection if it is no longer needed. 

### Tips for development

+ Start small with a representative sample of your data as you make significant changes to skillset composition. As your design finalizes, you can slowly add more data during later-stage development, and then roll in the entire data set when you are comfortable with the pipeline composition.

+ Retain control over indexer invocation. Indexers can run on a schedule, which is helpful for solutions that are rolled into production, but less helpful if you are actively developing your pipeline. During development, avoid schedules so that you don’t lose track of cache or projection state. Once your solution is in production and skillset composition is static, you can put the indexer on a schedule to pick up routine changes in the external source data. 

-->

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Öğrenme amacıyla ücretsiz hizmeti öneririz, ancak bu ücretsiz işlem sayısı, abonelik başına günde 20 belgelere sınırlı olduğunu unutmayın.

Birden çok hizmeti kullanırken, tüm hizmetlerinizi aynı bölgede en iyi performans ve maliyetleri en aza indirmek için oluşturun. Gelen veri ya da aynı bölgede başka bir hizmete gider giden veriler için bant genişliği için ücretlendirilmez.

**1. adım: [Bir Azure Search kaynağı oluşturun](search-create-service-portal.md)** 

**2. adım: [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)** 

**3. adım: [Bilişsel hizmetler kaynağı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)** 

**4. adım: [Portal ile çalışmaya başlama](cognitive-search-quickstart-blob.md) - veya - [REST ve Postman kullanarak örnek verilerle çalışmaya başlama](knowledge-store-howto.md)** 

REST kullanarak `api-version=2019-05-06-Preview` Store bilgi içeren bir yapay ZEKA tabanlı bir işlem hattı oluşturmak için. En yeni önizleme sürümünde API standartlarındaki şu nesneyi sağlar `knowledgeStore` tanımı.

## <a name="takeaways"></a>Paketler

Bilgi Bankası Store avantajları dahil olmak üzere çeşitli sunar ancak arama dışında senaryolarda zenginleştirilmiş belgelerin kullanımı etkinleştirme ile sınırlı değildir, maliyet denetimleri ve zenginleştirme işleminizin kaymaları yönetme. Bu özellikler yalnızca standartlarındaki şu bir depolama hesabı ekleme ve güncelleştirilmiş ifade dili makalesinde açıklanan şekilde kullanarak tarafından kullanılacak tüm kullanılabilir [bilgi Store ile çalışmaya başlama konusunda](knowledge-store-howto.md). 

## <a name="next-steps"></a>Sonraki adımlar

Zenginleştirilmiş belgeleri oluşturmak için en kolay yaklaşım aracılığıyladır **verileri içeri aktarma** Sihirbazı.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Portal Kılavuzu'nda bilişsel aramayı deneme](cognitive-search-quickstart-blob.md)
