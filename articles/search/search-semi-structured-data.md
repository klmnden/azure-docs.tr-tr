---
title: "Azure bulut depolama alanında arama yarı yapılandırılmış veriler"
description: "Azure Search kullanarak yarı yapılandırılmış blob veri arama."
author: roygara
manager: timlt
ms.service: search
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: a80ae99c2ada00885019ee93e4ef36821340d3a5
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="part-2-search-semi-structured-data-in-cloud-storage"></a>2. Kısım: Bulut depolama alanında arama yarı yapılandırılmış veriler

İki bölümlü öğretici serisinde Azure arama kullanarak yarı yapılandırılmış ve yapılandırılmamış verileri arama öğrenin. [Bölüm 1](../storage/blobs/storage-unstructured-search.md) yapılandırılmamış veriler üzerinde arama gitti, ancak aynı zamanda depolama hesabı oluşturma gibi Bu öğretici için önemli Önkoşullar dahil. 

Kısım 2'de Azure blob'larda depolanan JSON gibi yarı yapılandırılmış veriler için odak geçer. Yarı yapılandırılmış veri etiketleri veya içerik veri içinde ayrı işaretler içerir. Wholistically dizine gerekir yapılandırılmamış verileri gibi bir alan başına temelinde gezinilebilen bir ilişkisel veritabanı şeması, bir veri modeli aynılarını resmi olarak yapılandırılmış veri arasındaki fark böler.

Kısım 2'de bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir Azure blob kapsayıcısı için bir Azure Search veri kaynağını yapılandırma
> * Oluşturma ve bir Azure Search dizini ve kapsayıcı gezinme ve aranabilir içeriği ayıklamak için dizin oluşturucu doldurma
> * Yeni oluşturduğunuz dizin arama

> [!NOTE]
> Bu öğreticide şu anda Azure Search'te bir önizleme özelliğidir JSON dizisi desteği kullanır. Portalda kullanılabilir değildir. Bu nedenle, biz bu özellik ve API'yi çağırmak için bir REST istemci aracı sağlayan REST API Önizleme kullanıyorsunuz.

## <a name="prerequisites"></a>Önkoşullar

* Tamamlanmasından [önceki öğretici](../storage/blobs/storage-unstructured-search.md) önceki öğreticide oluşturulan depolama hesabı ve arama hizmeti sağlayan.

* REST istemcisi ve bir HTTP isteği oluşturmak nasıl bir anlayış yüklemesi. Bu öğreticinin amaçları doğrultusunda kullanıyoruz [Postman](https://www.getpostman.com/). Farklı bir REST istemcisi zaten belirli bir rahat kullanırsanız çekinmeyin.

## <a name="set-up-postman"></a>Postman ayarlayın

Postman başlatın ve bir HTTP isteği ayarlamak. Bu araçla bilginiz yoksa bkz [Azure Search REST Fiddler veya Postman kullanarak API'lerini keşfedin](search-fiddler.md) daha fazla bilgi için.

"POST" Bu öğreticide her çağrı isteği metodu değil "Content-type" ve "api anahtarını." üstbilgisi tuşlar olan "Application/json" ve "Yönetici anahtarınızı" başlığı anahtarların değerlerdir (Yönetici anahtarı için bir arama birincil anahtarınızı yer tutucu) sırasıyla. Gövde aramanız gerçek içeriği nereye ' dir. Kullanmakta olduğunuz istemcinin bağlı olarak nasıl, sorgunuzu üzerinde bazı farklılıklar olabilir, ancak temel olanlardır.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/postmanoverview.png)

Bu öğreticide kapsamdaki REST çağrısı için arama API anahtarı gereklidir. Api anahtarınızı altında bulabilirsiniz **anahtarları** arama hizmetinizi içinde. Bu API anahtarı, Bu öğretici yapmanızı yönlendirir her API çağrısı (Değiştir "Yönetici anahtarı" önceki ekran görüntüsünde onunla) üstbilgisindeki olmalıdır. Her çağrı için gerekli olduğundan anahtarı korur.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Örnek veri indirin

Bir örnek veri kümesi için hazırlandı. **Karşıdan [denemeler json.zip Klinik](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)**  ve kendi klasörüne sıkıştırmasını açın.

Aşağıdaki örnekte yer alan örnek metin başlangıçta olan JSON dosyalarıdır dosyaları elde [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Biz bunları JSON olarak size kolaylık olması için dönüştürdüğünüz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="upload-the-sample-data"></a>Örnek veri yükleme

Azure portalında oluşturulan depolama hesabına gidin [önceki Öğreticisi](../storage/blobs/storage-unstructured-search.md). Ardından açın **veri** kapsayıcı ve tıklatın **karşıya**.

Tıklatın **Gelişmiş**"Klinik-Denemeler-json zaman" girin ve ardından tüm indirdiğiniz JSON dosyalarını yükleyin.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/clinicalupload.png)

Yükleme tamamlandığında, dosyaları, kendi alt veri kapsayıcısı içinde görünmesi gerekir.

## <a name="connect-your-search-service-to-your-container"></a>Arama hizmetinizi, kapsayıcıya Bağlan

Bir veri kaynağı, bir dizin ve bir dizin oluşturucu oluşturmak için üç API çağrıları, arama hizmetinize yapmak için Postman kullanıyoruz. Veri kaynağı, depolama hesabınız ve JSON verileriniz için bir işaretçi içerir. Arama hizmetinizi verileri yüklenirken bağlantı kurar.

Sorgu dizesi içermelidir **API sürümü 2016-09-01-Önizleme =** ve her çağrı döndürmelidir bir **201 oluşturuldu**. Genel olarak kullanılabilir API sürümü henüz yalnızca önizleme API sürümü bir jsonArray, şu anda json işlemek için yeteneğine sahip değil.

Aşağıdaki üç API çağrıları REST istemcinizden yürütün.

### <a name="create-a-datasource"></a>Bir veri kaynağı oluşturma

Bir veri kaynağı dizini için hangi verilerin belirtir.

Uç noktası bu çağrının `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Değiştir `[service name]` arama hizmetinizi adı.

Bu çağrı için depolama hesabınız ve depolama hesabı anahtarınızı adı gerekir. Depolama hesabı anahtarı depolama hesabınızın içinde Azure portalında bulunabilir **erişim tuşları**. Konumu aşağıdaki görüntüde görüntülenir:

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/storagekeys.png)

Değiştirdiğinizden emin olun `[storage account name]` ve `[storage account key]` çağrı yürütmeden önce aramanız gövdesinde.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Yanıt gibi görünmelidir:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

### <a name="create-an-index"></a>Dizin oluşturma
    
İkinci API çağrısı bir dizin oluşturur. Bir dizin tüm parametrelerini ve onların öznitelikleri belirtir.

Bu çağrı için URL `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Değiştir `[service name]` arama hizmetinizi adı.

İlk URL değiştirin. Daha sonra kopyalayın ve, gövde aşağıdaki kodu yapıştırın ve sorgu çalıştırın.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Yanıt gibi görünmelidir:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

### <a name="create-an-indexer"></a>Bir dizin oluşturucu yapın

Bir dizin oluşturucu hedef arama dizini veri kaynağına bağlanır ve isteğe bağlı olarak veri yenileme otomatikleştirmek için bir zamanlama sağlar.

Bu çağrı için URL `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Değiştir `[service name]` arama hizmetinizi adı.

İlk URL değiştirin. Daha sonra kopyalayın ve, gövde aşağıdaki kodu yapıştırın ve sorgu çalıştırın.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Yanıt gibi görünmelidir:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>JSON dosyalarınızın arama

Arama hizmetinizi veri kapsayıcıya bağlı, dosyalarınızı aramaya başlayabilirsiniz.

Azure Portalı'nı açın ve arama hizmetinize geri gidin. Önceki öğreticide yaptığınız gibi.

  ![Yapılandırılmamış arama](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Kullanıcı tanımlı meta veri arama

Önce verileri çeşitli yollarla sorgulanabilir gibi: tam metin araması, Sistem özellikleri veya kullanıcı tanımlı meta veriler. Sistem özellikleri ve kullanıcı tanımlı meta veriler yalnızca arama yapılır ile `$select` olarak işaretlenen, parametre **alınabilir** hedef dizin oluşturulması sırasında. Oluşturulduktan sonra dizini parametrelerinde değiştirilebilir değil. Ancak, ek parametreler eklenebilir.

Temel sorgu örneğidir `$select=Gender,metadata_storage_size`, geri dönmek için bu iki parametre sayısını sınırlandırır.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/lastquery.png)

Daha karmaşık bir sorgu örneği olacaktır `$filter=MinimumAge ge 30 and MaximumAge lt 75`, burada MinimumAge parametreleri 30 eşit veya daha büyük ve MaximumAge değerinden 75 yalnızca sonuçları döndürür.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/metadatashort.png)

Denemeler ve birkaç daha fazla sorgu kendiniz denemek istiyorsanız, bunu yapmak çekinmeyin. Mantıksal işleçler kullanabileceğinizi biliyor (ve veya değil) ve Karşılaştırma işleçleri (eq, ne, gt, lt, ge, le). Dize karşılaştırmaları büyük/küçük harfe duyarlıdır.

`$filter` Parametresi yalnızca çalışır, dizin oluşturma sırasında filtrelenebilir olarak işaretlenen meta verilerle.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure arama, nasıl gibi kullanarak yarı yapılandırılmış veri arama hakkında öğrenilen:

> [!div class="checklist"]
> * REST API kullanarak Azure Search hizmeti oluşturma
> * Azure Search hizmeti, kapsayıcı aramak için kullanın

Arama hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure Blob depolama alanındaki belgelere dizin](search-howto-indexing-azure-blob-storage.md)