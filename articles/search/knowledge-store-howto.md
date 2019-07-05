---
title: Almak için Azure Search (Önizleme) araştırma - bilgi ile çalışmaya nasıl
description: Azure Search işlem hatlarında Azure depolama hesabınızdaki bir Bilgi Bankası deposuna dizin yapay ZEKA tarafından oluşturulan zenginleştirilmiş belgeleri gönderme adımlarını öğrenin. Buradan, görüntülemek, şekillendirmeyi ve Azure Search'te ve diğer uygulamalarda zenginleştirilmiş belgeleri kullanma.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 06/29/2019
ms.author: heidist
ms.openlocfilehash: e50dfcdc5ac2fbe2435066546a340874e1b8f682
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551063"
---
# <a name="how-to-get-started-with-knowledge-mining-in-azure-search"></a>Nasıl bilgi araştırma Azure Search ile çalışmaya başlama

> [!Note]
> Bilgi Bankası preview ve üretim kullanımı için değil amaçlayan deposudur. [2019-05-06-Önizleme REST API sürümü](search-api-preview.md) bu özelliği sağlar. .NET SDK'sı desteği şu anda yoktur.
>
[Bilgi deposunu](knowledge-store-concept-intro.md) AI zenginleştirilmiş belgeleri Azure depolama hesabınıza diğer uygulamalarda aşağı akış bilgi araştırma için dizin oluşturma sırasında oluşturulan kaydeder. Kaydedilen zenginleştirmelerinin, anlamak ve bir Azure Search dizini oluşturma ardışık düzeni geliştirmek için de kullanabilirsiniz. 

Bir Bilgi Bankası deposu tarafından tanımlanan bir *beceri kümesi* ve tarafından oluşturulan bir *dizin oluşturucu*. Bir Bilgi Bankası deposunun fiziksel ifade aracılığıyla belirtilen *projeksiyonlar* depolama veri yapılarını belirler. Bu izlenecek yolda bitiş zamanına göre bu nesnelerin tümü, oluşturulmuş olur ve tüm birbirine nasıl uyduğunu anlarsınız. 

Bu alıştırmada, örnek verileri, hizmetleri ve temel iş akışı oluşturma ve beceri kümesi tanımı indirimlere ilk bilgi deponuza kullanarak öğrenmek için Araçlar ile başlayın.

## <a name="prerequisites"></a>Önkoşullar

Bilgi Bankası, Azure Blob Depolama ve Azure tablo depolama fiziksel depolama ve Azure Search ve nesne oluşturma ve güncelleştirmeler için Bilişsel hizmetler sağlayan birden çok hizmete merkezine deposudur. Konusunda [temel mimari](knowledge-store-concept-intro.md) Bu izlenecek yol için bir önkoşuldur.

Bu hızlı başlangıçta, aşağıdaki hizmetler ve Araçlar kullanılır. 

+ [Postman Masaüstü uygulamasını edinin](https://www.getpostman.com/), Azure Search için HTTP istekleri göndermek için kullanılır.

+ [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek veri ve bilgi depolamak için depolama. Bilgi Bankası deponuz, Azure depolama alanında bulunur.

+ [Bilişsel hizmetler kaynağı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) iskeleti sağlayan geniş erişim için yapay ZEKA zenginleştirmelerinin içinde kullanılan becerileri tam aralığının S0 Kullandıkça Öde katmanında. Bilişsel hizmetler ve Azure Search hizmetiniz aynı bölgede olması gerekir.

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz. 

Örnek JSON belgeleri ve bir Postman koleksiyonu dosya de gereklidir. Bulma ve Tamamlayıcı dosyaları yükleme yönergeleri sağlanır [örnek verileri hazırlama](#prepare-sample-data) bölümü.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

    ![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-get-started-postman/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. Aşağıdaki bölümlerde her HTTP isteği, hizmet adını ve API anahtarını sağlarız.

<a name="prepare-sample-data"></a>

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Bilgi Bankası depolama iyileştirmesini Ardışık düzenin çıkış içerir. İşlem hattı üzerinden ilerledikçe, sonuçta "kullanılabilir" haline gelir "kullanılamıyor" veri girişleri oluşur. Kullanılamayan veri örnekleri, metin veya görüntü özelliklerini çözümlenmesi gereken görüntü dosyaları veya varlıklar, anahtar ifadeleri ve yaklaşım çözümlenebilecek yoğun metin dosyaları içerebilir. 

Bu alıştırmada kaynaklandığı yoğun metin dosyaları (çalışması yasaları bilgileri) kullanan [Caselaw erişim proje](https://case.law/bulk/download/) genel toplu veri sayfasını indirin. 10-belge örnek için GitHub bu alıştırma için dosyamızı. 

Bu görevde, işlem hattı için girdi olarak kullanmak bu belgeleri için bir Azure Blob kapsayıcısı oluşturacaksınız. 

1. İndirin ve ayıklayın [Azure arama örnek verileri](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw) almak için depo [Caselaw veri kümesi](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw). 

1. [Azure portalında oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, tıklayın **Blobları**ve ardından **+ kapsayıcı**.

1. [Bir Blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) örnek verileri içerecek şekilde: 

   1. Kapsayıcı adı `caselaw-test`. 
   
   1. Genel erişim düzeyi geçerli değerleri birine ayarlayın.

1. Kapsayıcıyı oluşturduktan sonra dosyayı açın ve seçin **karşıya** komut çubuğunda.

   ![Komut çubuğunda karşıya](media/search-semi-structured-data/upload-command-bar.png "komut çubuğunda karşıya yükleme")

1. Klasör içeren dizine gidin **caselaw sample.json** örnek dosyası. Dosyayı seçin ve ardından **karşıya**.

1. Azure depolama alanında olmasına karşın, bağlantı dizesi ve kapsayıcı adını alın.  Bu dizelerin her ikisine birden ihtiyacınız [veri kaynağı oluştur](#create-data-source):

   1. Genel bakış sayfasında tıklatın **erişim anahtarlarını** ve kopyalama bir *bağlantı dizesi*. İle başlar `DefaultEndpointsProtocol=https;` ve ile sonucuna `EndpointSuffix=core.windows.net`. Hesap adı ve anahtarı arasında var. 

   1. Kapsayıcı adı olması gereken `caselaw-test` veya ad size atanmış.



## <a name="set-up-postman"></a>Postman’i ayarlama

Postman, istekleri ve JSON belgeleri Azure Search'e göndermek için kullanacağınız istemci uygulamasıdır. Birkaç istekleri bu makaledeki bilgileri kullanarak formüle edilebilir. Ancak, iki (bir beceri kümesi oluşturma, bir dizin oluşturma) en büyük isteklerin bir makaledeki katıştırmak için çok büyük ayrıntılı JSON içerir. 

Tüm istekleri ve JSON belgeleri tam olarak kullanılabilir hale getirmek için bir Postman koleksiyonu dosya oluşturduk. İndirme ve sonra bu dosyayı içeri istemcisi ayarlama ilk göreviniz var.

1. İndirip sıkıştırmasını [Azure arama Postman örnekleri](https://github.com/Azure-Samples/azure-search-postman-samples) depo.

1. Postman'i başlatın ve Caselaw Postman koleksiyonunu içeri aktarın:

   1. Tıklayın **alma** > **dosyaları içeri aktarma** > **seçin dosyaları**. 

   1. \Azure-search-postman-samples-master\azure-search-postman-samples-master\Caselaw klasöre gidin.

   1. Select **Caselaw.postman_collection_v2.json**. Dört görmelisiniz **POST** istekleri koleksiyondaki.

   ![Caselaw tanıtım için postman koleksiyonu](media/knowledge-store-howto/postman-collection.png "Caselaw tanıtım için Postman koleksiyonu")
   

## <a name="create-an-index"></a>Dizin oluşturma
    
İlk istek kullanan [oluşturma dizini API](https://docs.microsoft.com/rest/api/searchservice/create-data-source), Azure Search dizini oluşturma, tüm aranabilir verileri depolar. Bir dizin, tüm alanlar, parametreler ve öznitelikleri belirtir.

Bilgi Bankası araştırma için dizin gerekmeyen gerekmez, ancak bir dizin oluşturucu, bir dizin sağlanmadığı sürece çalışmaz. 

1. URL'deki `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/indexes?api-version=2019-05-06-Preview`, değiştirin `YOUR-AZURE-SEARCH-SERVICE-NAME` arama hizmetinizin adıyla. 

1. Üst bölümünde değiştirin `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı için Azure Search ile.

1. Gövde bölümü içinde bir dizin şemasını JSON belgesidir. Görünürlük için daraltılmış, bir dizinin dış Kabuk aşağıdaki öğelerden oluşur. Alanlar koleksiyonu caselaw veri kümesi alanlara karşılık gelir.

   ```json
   {
      "name": "caselaw",
      "defaultScoringProfile": null,
      "fields": [],
      "scoringProfiles": [],
      "corsOptions": null,
      "suggesters": [],
      "analyzers": [],
      "tokenizers": [],
      "tokenFilters": [],
      "charFilters": [],
      "encryptionKey": null
   }
   ```

1. Genişletin `fields` koleksiyonu. Basit alanlar, oluşan dizin tanımını getirerek içerdiği [karmaşık alanları](search-howto-complex-data-types.md) iç içe geçmiş substructures ve Koleksiyonlar ile.

   Alan tanımı gözden geçirmek için birkaç dakikanızı `casebody` satırlar 302-384 karmaşık alanı. Hiyerarşik gösterimleri gerektiğinde karmaşık bir alan diğer karmaşık alanlar içerebilir dikkat edin. Hiyerarşik yapıları, bu nedenle iç içe veri yapısı Bilgi Bankası deposunda oluşturma, bir beceri kümesi burada ve ayrıca bir projeksiyon olarak gösterildiği gibi bir dizinde modellenebilir.

   ```json
   {
    "name": "casebody",
    "type": "Edm.ComplexType",
    "fields": [
        {
            "name": "status",
            "type": "Edm.String",
            "searchable": true,
            "filterable": true,
            "retrievable": true,
            "sortable": true,
            "facetable": true,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "data",
            "type": "Edm.ComplexType",
            "fields": [
                {
                    "name": "head_matter",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "retrievable": true,
                    "sortable": false,
                    "facetable": false,
                    "key": false,
                    "indexAnalyzer": null,
                    "searchAnalyzer": null,
                    "analyzer": null,
                    "synonymMaps": []
                },
                {
                    "name": "opinions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "author",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": false,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "type",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        }
                    ]
                },
    . . .
   ```

1. Tıklayın **Gönder** ve isteği yürütmek için.  Alması gereken bir **durumu: 201 oluşturuldu** olarak bir yanıt iletisi.

<a name="create-data-source"></a>

## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

İkinci isteği kullanır [veri kaynağı API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source) Azure Blob depolama alanına bağlanmak için. 

1. URL'deki `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/datasources?api-version=2019-05-06-Preview`, değiştirin `YOUR-AZURE-SEARCH-SERVICE-NAME` arama hizmetinizin adıyla. 

1. Üst bölümünde değiştirin `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı için Azure Search ile.

1. Gövde bölümüne JSON belgesi depolama hesabı bağlantı dizesi ve blob kapsayıcı adını içerir. Bağlantı dizesini Azure portalında depolama hesabınızın içinde bulunabilir **erişim anahtarlarını**. 

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-STORAGE-KEY>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<YOUR-BLOB-CONTAINER-NAME>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

1. Tıklayın **Gönder** ve isteği yürütmek için.  Alması gereken bir **durumu: 201 oluşturuldu** olarak bir yanıt iletisi.



<a name="create-skillset"></a>

## <a name="create-a-skillset-and-knowledge-store"></a>Yetenek ve Bilgi Bankası deposu oluşturma

Üçüncü isteği kullanan [beceri kümesi API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset), hangi bilişsel beceriler becerileri birlikte ve en önemlisi, bu kılavuz için - zincir şeklinde nasıl çağrılacağını belirtir bir Azure Search nesne oluşturma nasıl bilgi deposunu belirtmek.

1. URL'deki `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/skillsets?api-version=2019-05-06-Preview`, değiştirin `YOUR-AZURE-SEARCH-SERVICE-NAME` arama hizmetinizin adıyla. 

1. Üst bölümünde değiştirin `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı için Azure Search ile.

1. Gövde bölümüne bir beceri kümesi tanımı JSON belgesidir. Görünürlük için daraltılmış, dış bir beceri kümesi kabuğunu aşağıdaki öğelerden oluşur. `skills` Koleksiyonu bellek içi zenginleştirmelerinin tanımlar ancak `knowledgeStore` çıkış nasıl depolandığını belirtir. `cognitiveServices` Bağlantınızı AI zenginleştirme altyapıları tanımıdır.

   ```json
   {
    "name": "caselaw-ss",
    "description": null,
    "skills": [],
    "cognitiveServices": [],
    "knowledgeStore": []
   }
   ```

1. Genişletin `cognitiveServices` ve `knowledgeStore` böylelikle, bağlantı bilgilerini sağlayabilirsiniz. Örnekte, bu dizeler istek gövdesi sonuna doğru beceri kümesi tanımının sonra yer alır. 

   İçin `cognitiveServices`, S0 katmanında Azure Search ile aynı bölgede bulunan bir kaynak sağlayın. Azure portalında aynı sayfasından cognitiveServices adını ve anahtarını alın. 
   
   İçin `knowledgeStore`, caselaw Blob kapsayıcısı için kullanılan aynı bağlantı dizesini kullanabilirsiniz.

    ```json
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "YOUR-SAME-REGION-S0-COGNITIVE-SERVICES-RESOURCE",
        "key": "YOUR-COGNITIVE-SERVICES-KEY"
    },
    "knowledgeStore": {
        "storageConnectionString": "YOUR-STORAGE-ACCOUNT-CONNECTION-STRING",
    ```

1. Becerileri koleksiyonu özellikle Shaper becerileri satırlarında 85 ve 179, sırasıyla genişletin. Bilgi Bankası araştırma için istediğiniz veri yapılarını çeviren Shaper beceri önemlidir. Beceri yürütmesi sırasında bu yapıları yalnızca bellek içi, ancak sonraki adıma taşırken, daha fazla araştırma için bir Bilgi Bankası mağazası bu Çıktıyı nasıl kaydedilebilir görürsünüz.

   Aşağıdaki kod parçacığı 217 satırından ' dir. 

    ```json
    "name": "Opinions",
    "source": null,
    "sourceContext": "/document/casebody/data/opinions/*",
    "inputs": [
        {
            "name": "Text",
            "source": "/document/casebody/data/opinions/*/text"
        },
        {
            "name": "Author",
            "source": "/document/casebody/data/opinions/*/author"
        },
        {
            "name": "Entities",
            "source": null,
            "sourceContext": "/document/casebody/data/opinions/*/text/pages/*/entities/*",
            "inputs": [
                {
                    "name": "Entity",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/value"
                },
                {
                    "name": "EntityType",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/category"
                }
            ]
        }
    ]
   . . .
   ```

1. Genişletin `projections` öğesinde `knowledgeStore`262 satırda başlayan. Projeksiyonlar bilgi deposu oluşturma belirtin. Projeksiyonlar tabloları nesneleri çiftleri, ancak şu anda yalnızca bir zaman içinde belirtilir. İlk projeksiyonda gördüğünüz gibi `tables` belirtildi ancak `objects` değil. İkinci, onu tersidir.

   Azure depolama tabloları, oluşturduğunuz her tablo için tablo depolamasında oluşturulur ve her nesne bir kapsayıcı, Blob Depolama alanında alır.

   BLOB nesneler genellikle bir zenginleştirme, tam ifade içerir. Tablolar, genellikle belirli amaçlar için düzenleme birleşimler kısmi zenginleştirmelerinin içerir. Bu örnek durumları tablo ve düşünceleri son derece tablo gösterir gösterilmez varlıklar, avukatlar Jüri ve taraflar gibi diğer tablolar gerekmez

    ```json
    "projections": [
        {
            "tables": [
                {
                    "tableName": "Cases",
                    "generatedKeyName": "CaseId",
                    "source": "/document/Case"
                },
                {
                    "tableName": "Opinions",
                    "generatedKeyName": "OpinionId",
                    "source": "/document/Case/OpinionsSnippets/*"
                }
            ],
            "objects": []
        },
        {
            "tables": [],
            "objects": [
                {
                    "storageContainer": "enrichedcases",
                    
                    "source": "/document/CaseFull"
                }
            ]
        }
    ]
    ```

1. Tıklayın **Gönder** ve isteği yürütmek için. Yanıt şu şekilde olmalıdır **201** ve yanıt ilk bölümünü gösteren aşağıdaki örneğe benzer.

    ```json
    {
    "name": "caselaw-ss",
    "description": null,
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
            "name": "SplitSkill#1",
            "description": null,
            "context": "/document/casebody/data/opinions/*/text",
            "defaultLanguageCode": "en",
            "textSplitMode": "pages",
            "maximumPageLength": 5000,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/casebody/data/opinions/*/text
                }
            ],
            "outputs": [
                {
                    "name": "textItems",
                    "targetName": "pages"
                }
            ]
        },
        . . .
    ```

## <a name="create-and-run-an-indexer"></a>Oluşturma ve bir dizin oluşturucuyu çalıştırma

Dördüncü istek kullanan [dizin oluşturucu API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer), bir Azure Search dizin oluşturucu oluşturma. Bir dizin oluşturucu dizinleme işlem hattına yürütme altyapısıdır. Tüm o ana kadar oluşturulmuş tanımlarını, bu adım ile hareket halinde yerleştirilir.

1. URL'deki `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/indexers?api-version=2019-05-06-Preview`, değiştirin `YOUR-AZURE-SEARCH-SERVICE-NAME` arama hizmetinizin adıyla. 

1. Üst bölümünde değiştirin `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı için Azure Search ile.

1. Gövde bölümüne JSON belgesini dizin oluşturucu adını belirtir. Bir veri kaynağı ve dizin, dizin oluşturucu tarafından gereklidir. Bir beceri kümesi, bir dizin oluşturucu için isteğe bağlı, ancak AI zenginleştirme için gerekli değildir.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": { },
        "fieldMappings": [],
        "outputFieldMappings": [ ]
    ```

1. OutputFieldMappings genişletin. Bir veri kaynağındaki alanları ve dizin alanları arasında özel eşleme için kullanılan fieldMappings kullanılmasının outputFieldMappings zenginleştirilmiş alanlarını eşleme için kullanılan, oluşturulur ve çıkış dizini ya da projeksiyon alanlara işlem hattı tarafından doldurulur.

    ```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
            "targetFieldName": "people",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/organizations/*",
            "targetFieldName": "orginizations",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/locations/*",
            "targetFieldName": "locations",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/Case/OpinionsSnippets/*/Entities/*",
            "targetFieldName": "entities",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/keyPhrases/*",
            "targetFieldName": "keyPhrases",
            "mappingFunction": null
        }
    ]
    ```

1. Tıklayın **Gönder** ve isteği yürütmek için. Yanıt şu şekilde olmalıdır **201** ve yanıt gövdesi (konuyu uzatmamak amacıyla kırpılmış) sağladığınız isteği yükü neredeyse aynı görünmelidir.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": { },
        "fieldMappings": [],
        "outputFieldMappings": [ ]
    }
    ```

## <a name="explore-knowledge-store"></a>Bilgi Bankası store'u keşfedin

İlk alındıktan hemen sonra keşfetmeye başlayabilirsiniz. Bu görev için kullanacağınız [ **Depolama Gezgini** ](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer) portalında.

Bilgi deposunu Azure arama için tam olarak ayrılmış olduğunu bilmeniz önemlidir. Azure Search dizini ve veri temsilini ve içeriği, ancak buradan bölümü yolları içeren bilgi mağazası hem de. Dizin, tam metin araması, filtrelenen arama ve Azure arama'yı desteklenen tüm senaryolar için kullanın. Veya, içerikleri analiz etmek için diğer araçlar iliştirme yalnızca bilgi deponuz ile ileri taşıyın.

## <a name="takeaways"></a>Paketler

Artık Azure depolama alanında ilk bilgi deponuza oluşturduğunuz ve Depolama Gezgini zenginleştirmelerinin görüntülemek için kullanılır. Bu, depolanan zenginleştirmelerinin ile çalışmak için temel deneyimidir. 

## <a name="next-steps"></a>Sonraki adımlar

Shaper beceri yeni şekillere birleştirilebilir ayrıntılı veri formları oluşturma ağır işleri yapar. Sonraki adım olarak, nasıl kullanıldıkları hakkındaki ayrıntılar için bu yetenek için başvuru sayfasına gözden geçirin.

> [!div class="nextstepaction"]
> [Shaper beceri başvurusu](cognitive-search-skill-shaper.md)


<!---
## Keep This

How to convert unformatted JSON into an indented JSON document structure that allows you to quickly identify nested structures. Useful for creating an index that includes complex types.

1. Use Visual Studio Code.
2. Open data.jsonl
--->
