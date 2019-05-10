---
title: Nasıl bilgi Store ile (Önizleme) - Azure Search kullanmaya başlayın
description: Azure Search işlem hatlarında Azure depolama hesabınızdaki bir Bilgi Bankası deposuna dizin yapay ZEKA tarafından oluşturulan zenginleştirilmiş belgeleri gönderme adımlarını öğrenin. Buradan, görüntülemek, şekillendirmeyi ve Azure Search'te ve diğer uygulamalarda zenginleştirilmiş belgeleri kullanma.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 05/08/2019
ms.author: heidist
ms.openlocfilehash: d9006e3fcfc9691b9f3eec4b86c545fd3fea9f8a
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65471751"
---
# <a name="how-to-get-started-with-knowledge-store"></a>Nasıl bilgi Store ile çalışmaya başlama

[Bilgi Bankası Store](knowledge-store-concept-intro.md) Azure Search'te, diğer uygulamalarda bilgi araştırma için dizin oluşturma bir işlem hattı oluşturdunuz AI zenginleştirmelerinin kaydeder yeni bir önizleme özelliğidir. Kaydedilen zenginleştirmelerinin, anlamak ve bir Azure Search dizini oluşturma ardışık düzeni geliştirmek için de kullanabilirsiniz.

Bilgi deposunu bir beceri kümesi tarafından tanımlanır. Normal Azure arama tam metin araması senaryolar için yapay ZEKA zenginleştirmelerinin içerik daha aranabilir yapmak için bir beceri kümesi amacı sunuyor. Bilgi Bankası araştırma senaryoları için bir beceri kümesi rolü oluşturma, doldurma ve analiz için birden çok veri yapılarını depolamak veya diğer uygulamalar ve işlemler modelleme.

Bu alıştırmada, örnek verileri, hizmetleri ve temel iş akışı oluşturma ve beceri kümesi tanımı indirimlere ilk bilgi deponuza kullanarak öğrenmek için Araçlar ile başlayın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetleri, araçları ve verileri kullanılır. 

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek verileri depolamak için. Bilgi Bankası deponuz, Azure depolama alanında bulunur. 

+ [Bilişsel hizmetler kaynağı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) iskeleti sağlayan geniş erişim için yapay ZEKA zenginleştirmelerinin içinde kullanılan becerileri tam aralığının S0 Kullandıkça Öde katmanında. Bu kaynak ve Azure Search hizmetiniz aynı bölgede olması gerekir.

+ [Postman masaüstü uygulaması](https://www.getpostman.com/) istekleri Azure Search'e göndermek için.

+ [Postman koleksiyonu](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/Caselaw) bir veri kaynağı, dizin, beceri kümesi ve dizin oluşturucu oluşturmak için hazırlanan isteklerle. Birkaç nesne tanımları, bu makaledeki dahil etmek için çok uzun olabilir. Dizin ve beceri kümesi tanımları tamamen görmek için bu koleksiyonu almanız gerekir.

+ [Örnek verileri Caselaw](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw) kaynaklanan [Caselaw erişim proje](https://case.law/bulk/download/) genel toplu veri sayfasını indirin. Özellikle, ilk indirme (Arkansas) ilk 10 belgeleri alıştırma kullanır. 10-belge örnek için GitHub bu alıştırma için dosyamızı.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

    ![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

1. [Azure portalında oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, tıklayın **Blobları**ve ardından **+ kapsayıcı**.

1. [Bir Blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) örnek verileri içerecek şekilde. Kapsayıcı adı "caselaw-test" kullanın. Genel erişim düzeyi geçerli değerleri birini ayarlayabilirsiniz.

1. Kapsayıcıyı oluşturduktan sonra dosyayı açın ve seçin **karşıya** komut çubuğunda.

   ![Komut çubuğunda karşıya](media/search-semi-structured-data/upload-command-bar.png "komut çubuğunda karşıya yükleme")

1. Klasör içeren dizine gidin **caselaw sample.json** örnek dosyası. Dosyayı seçin ve ardından **karşıya**.


## <a name="set-up-postman"></a>Postman’i ayarlama

Postman'i başlatın ve Caselaw Postman koleksiyonunu içeri aktarın. Alternatif olarak, bir dizi HTTP isteği ayarlayın. Bu aracı bilmiyorsanız bkz [Azure Search REST Postman kullanarak API'lerini keşfedin](search-fiddler.md).

+ Bu kılavuzda her çağrı için istek yöntemi **PUT** veya **POST**.
+ İstek üstbilgilerini (2) şunları içerir: "Content-type" Ayarla "application/json", "api-key", "Yönetici anahtarını" için ayarlanmış (Yönetici anahtarı bir arama birincil anahtarınız için yer tutucudur) sırasıyla. 
+ İstek gövdesi, çağrınızın gerçek içeriklerini yerleştirdiğiniz ' dir. 

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/postmanoverview.png)

Bu sırayla bir veri kaynağı, dizin, bir beceri kümesi ve bir dizin oluşturucu - oluşturma, arama hizmetinize dört API çağrıları gerçekleştirmek için Postman kullanıyoruz. Veri kaynağı, Azure depolama hesabı ve JSON verilerini bir işaretçi içerir. Arama hizmetiniz, veriler içeri aktarılırken bağlantı kurar.

[Bir beceri kümesi oluşturma](#create-skillset) odak noktası, bu izlenecek yol: zenginleştirme adımları ve bir Bilgi Bankası deposunda kalıcı verileri nasıl belirtir.

URL uç noktasını bir API sürümü ve her çağrının döndürmelidir belirtmelisiniz bir **201 oluşturuldu**. Önizleme api-bilgi deposu desteği ile bir beceri kümesi oluşturmak için Version `2019-05-06-Preview` (büyük-küçük harfe duyarlı).

Şu API çağrıları, REST istemcinizden yürütün.

## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

[Veri kaynağı API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source) hangi verilerin dizininin oluşturulacağını belirtir bir Azure Search nesnesi oluşturur.

Bu çağrının uç noktası `https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview` 

1. `[service name]` değerini, arama hizmetinizin adıyla değiştirin. 

2. Bu çağrı için istek gövdesi, depolama hesabı bağlantı dizesi ve blob kapsayıcı adı içermelidir. Bağlantının Azure portalında depolama hesabınızın içinde bulunabilir **erişim anahtarlarını**. 

   Çağrıyı yürütmeden önce bağlantı dizesi ve blob kapsayıcı adı istek gövdesinde değiştirdiğinizden emin olun.

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

3. İsteği gönderin. Yanıt şu şekilde olmalıdır **201** ve yanıt gövdesinin sağladığınız isteği yükü neredeyse aynı görünmelidir.

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your storage key>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<your blob container name>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

## <a name="create-an-index"></a>Dizin oluşturma
    
İkinci çağrı [oluşturma dizini API](https://docs.microsoft.com/rest/api/searchservice/create-data-source), Azure Search dizini oluşturma, tüm aranabilir verileri depolar. Bir dizin, tüm alanlar, parametreler ve öznitelikleri belirtir.

Bilgi Bankası araştırma için dizin gerekmeyen gerekmez, ancak bir dizin oluşturucu, bir dizin sağlanmadığı sürece çalışmaz. 

Bu çağrının URL'si `https://[service name].search.windows.net/indexes?api-version=2019-05-06-Preview`

1. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

2. Dizin tanımını istek gövdesine Postman koleksiyonu Create Index istekte kopyalayın. Dizin tanımını birkaç yüz, burada yazdırmak için çok uzun satırdır. 

   Bir dizinin dış Kabuk aşağıdaki öğelerden oluşur. 

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

3. `fields` Koleksiyonu, toplu dizin tanımını içerir. Basit alanlar içerir [karmaşık alanları](search-howto-complex-data-types.md) iç içe geçmiş substructures ve Koleksiyonlar ile.

   Alan tanımı gözden `casebody` 302-384'on satır. Hiyerarşik gösterimleri gerektiğinde karmaşık bir alan diğer karmaşık alanlar içerebilir dikkat edin.

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

4. İsteği gönderin. 

   Yanıt şu şekilde olmalıdır **201** ve ilk birkaç alanlarını gösteren aşağıdaki örneğe benzer:

    ```json
    {
        "name": "caselaw",
        "defaultScoringProfile": null,
        "fields": [
            {
                "name": "id",
                "type": "Edm.String",
                "searchable": true,
                "filterable": true,
                "retrievable": true,
                "sortable": true,
                "facetable": true,
                "key": true,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
            {
                "name": "name",
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
      . . .
    ```

<a name="create-skillset"></a>

## <a name="create-a-skillset-and-knowledge-store"></a>Yetenek ve Bilgi Bankası deposu oluşturma

[Beceri kümesi API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset) hangi bilişsel beceriler becerileri birlikte ve en önemlisi, bu kılavuz için - zincir şeklinde nasıl çağrılacağını belirten bir Azure Search nesnesi yaratır nasıl bilgi deposunu belirtmek.

Bu çağrının uç noktası `https://[service name].search.windows.net/skillsets?api-version=2019-05-06-Preview`

1. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

2. Beceri kümesi tanımı beceri kümesi oluşturma isteğinde istek gövdesi Postman koleksiyonuna kopyalayın. Beceri kümesi tanımı birkaç yüz satırları, burada yazdırmak için çok uzun ancak bu kılavuzun odağı olacaktır.

   Bir beceri kümesi, dış Kabuk aşağıdaki öğelerden oluşur. `skills` Koleksiyonu bellek içi zenginleştirmelerinin tanımlar ancak `knowledgeStore` çıkış nasıl depolandığını belirtir. `cognitiveServices` Bağlantınızı AI zenginleştirme altyapıları tanımıdır.

   ```json
   {
    "name": "caselaw-ss",
    "description": null,
    "skills": [],
    "cognitiveServices": [],
    "knowledgeStore": []
   }
   ```

3. İlk olarak ayarlamak `cognitiveServices` ve `knowledgeStore` anahtar ve bağlantı dizesi. Örnekte, bu dizeler istek gövdesi sonuna doğru beceri kümesi tanımının sonra yer alır. Azure Search ile aynı bölgede bulunan S0 katmanı, sağlanan bir Bilişsel hizmetler kaynağı kullanın.

    ```json
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "YOUR-SAME-REGION-S0-COGNITIVE-SERVICES-RESOURCE",
        "key": "YOUR-COGNITIVE-SERVICES-KEY"
    },
    "knowledgeStore": {
        "storageConnectionString": "YOUR-STORAGE-ACCOUNT-CONNECTION-STRING",
    ```

3. Beceriler koleksiyon özellikle Shaper becerileri satırlarında 85 ve 170, sırasıyla gözden geçirin. Bilgi Bankası araştırma için istediğiniz veri yapılarını çeviren Shaper beceri önemlidir. Beceri yürütmesi sırasında bu yapıları yalnızca bellek içi, ancak sonraki adıma taşırken, daha fazla araştırma için bir Bilgi Bankası mağazası bu Çıktıyı nasıl kaydedilebilir görürsünüz.

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

3. Gözden geçirme `projections` öğesinde `knowledgeStore`262 satırda başlayan. Projeksiyonlar bilgi deposu oluşturma belirtin. Projeksiyonlar tabloları nesneleri çiftleri, ancak şu anda yalnızca bir zaman içinde belirtilir. İlk projeksiyonda gördüğünüz gibi `tables` belirtildi ancak `objects` değil. İkinci, onu tersidir.

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

5. İsteği gönderin. Yanıt şu şekilde olmalıdır **201** ve yanıt ilk bölümünü gösteren aşağıdaki örneğe benzer.

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

[Dizin oluşturucu API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer) oluşturur ve hemen bir dizin oluşturucu yürütür. Tüm o ana kadar oluşturulmuş tanımlarını, bu adım ile hareket halinde yerleştirilir. Hizmeti olmadığından bir dizin oluşturucu hemen çalıştırılır. Mevcut sonra var olan bir dizin oluşturucu bir POST çağrısına bir güncelleştirme bir işlemdir.

Bu çağrının uç noktası `https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview`

1. `[service name]` değerini, arama hizmetinizin adıyla değiştirin. 

2. Bu çağrı için istek gövdesi, dizin oluşturucu adını belirtir. Bir veri kaynağı ve dizin, dizin oluşturucu tarafından gereklidir. Bir beceri kümesi, bir dizin oluşturucu için isteğe bağlı, ancak AI zenginleştirme için gerekli değildir.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": {
            "batchSize": 1,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonLines"
            }
        },
        "fieldMappings": [],
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
    }
    ```

3. İsteği gönderin. Yanıt şu şekilde olmalıdır **201** ve yanıt gövdesi (konuyu uzatmamak amacıyla kırpılmış) sağladığınız isteği yükü neredeyse aynı görünmelidir.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": {
            "batchSize": 1,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonLines"
            }
        },
        "fieldMappings": [],
        "outputFieldMappings": [
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
                "targetFieldName": "people",
                "mappingFunction": null
            }
        ]
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