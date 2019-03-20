---
title: JSON Azure Blob storage'da - Azure Search arama Öğreticisi
description: Bu öğreticide, Azure Search kullanarak yarı yapılandırılmış Azure blob verilerini aramayı öğreneceksiniz.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 1c8ce14dd3961eff33a54a14c2bd0b27650d8a50
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201356"
---
# <a name="tutorial-search-semi-structured-data-in-azure-cloud-storage"></a>Öğretici: Azure bulut depolamada yarı yapılandırılmış verileri arama

Azure arama dizin JSON belgeleri ve Azure blob depolama kullanarak dizileri bir [dizin oluşturucu](search-indexer-overview.md) yarı yapılandırılmış verileri okumak nasıl olduğunu bilir. Yarı yapılandırılmış veriler, veriler içindeki içeriği ayıran etiketleri veya işaretleri içerir. Bu tam olarak sıralanması gerekir ve bir alan başına temelinde dizine bir ilişkisel veritabanı şeması gibi bir veri modeli için uyar veri resmi olarak yapılandırılmış, yapılandırılmamış verileri birbirinden ayırır.

Bu öğreticide, [Azure Search REST API'lerini](https://docs.microsoft.com/rest/api/searchservice/) ve aşağıdaki görevleri gerçekleştirmek için bir REST istemcisi:

> [!div class="checklist"]
> * Azure blob kapsayıcısı için Azure Search veri kaynağını yapılandırma
> * Aranabilir içeriğe sahip bir Azure Search dizini oluşturma
> * Yapılandırma ve çalıştırma kapsayıcısını ve Azure blob depolama alanından aranabilir içeriği ayıklamak için dizin oluşturucu
> * Oluşturduğunuz dizini arama

> [!NOTE]
> Bu öğreticide, şu anda Azure Search’te önizleme özelliği olan JSON dizisi desteğinden yararlanılmaktadır. Portalda kullanılma sunulmamıştır. Bu nedenle, API’yi çağırmak için REST istemci aracını ve bu özelliği sağlayan önizleme REST API’sini kullanıyoruz.

## <a name="prerequisites"></a>Önkoşullar

[Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

[Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek verileri içerecek şekilde.

[Postman kullanma](https://www.getpostman.com/) veya isteklerinizi göndermek için başka bir REST istemcisi. Sonraki bölümde bir HTTP isteği Postman içinde ayarlama yönergeleri sağlanır.

## <a name="set-up-postman"></a>Postman’i ayarlama

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu aracı bilmiyorsanız bkz [Azure Search REST Postman kullanarak API'lerini keşfedin](search-fiddler.md).

Bu öğreticideki her çağrı için istek yöntemi "POST" yöntemidir. Üst bilgi anahtarları "Content-type" ve "api-key"dir. Üst bilgi anahtarlarının değerleri sırayla "application/json" ve "yönetici anahtarınız"dır (yönetici anahtarı, arama birincil anahtarınız için yer tutucudur). Gövde, çağrınızın gerçek içeriklerini yerleştirdiğiniz yerdir. Kullandığınız istemciye bağlı olarak, sorgunuzu oluşturma şeklinizde bazı farklılıklar olabilir. Burada temel bilgiler verilmiştir.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/postmanoverview.png)

Bu öğreticide ele alınan REST çağrıları için arama api-key değeriniz gereklidir. Arama hizmetinizin içinde **Anahtarlar** bölümünde api-key değerinizi bulabilirsiniz. Bu api-key, bu öğreticinin sizi yönlendirdiği her API çağrısının üst bilgisinde olmalıdır (önceki ekran görüntüsünde "yönetici anahtarını" bununla değiştirin). Her çağrı için gerekli olduğundan anahtarı saklayın.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/keys.png)

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

1. **[clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** dosyasını indirip kendi klasörüne ayıklayın. Veri kaynaklanan [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results), Bu öğretici json'a dönüştürülmüş.

2. Oturum [Azure portalında](https://portal.azure.com), Azure depolama hesabınıza, açık gidin **veri** kapsayıcı seçeneğine tıklayıp **karşıya**.

3. **Gelişmiş**’e tıklayın, "clinical-trials-json" değerini girin ve sonra indirdiğiniz tüm JSON dosyalarını karşıya yükleyin.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/clinicalupload.png)

Yükleme tamamlandıktan sonra dosyalar veri kapsayıcısında kendi alt klasöründe görünmelidir.

## <a name="connect-your-search-service-to-your-container"></a>Arama hizmetinizi, kapsayıcınıza bağlama

Arama hizmetinize üç API çağrısı yaparak veri kaynağı, dizin ve dizin oluşturucu oluşturmak için Postman kullanıyoruz. Veri kaynağı, depolama hesabınıza ve JSON verilerinize yönelik bir işaretçi içerir. Arama hizmetiniz, veriler yüklenirken bağlantı kurar.

Sorgu dizesi API önizlemesi içermelidir (gibi **api sürümü = 2017-11-11-Preview**) ve her çağrının döndürmelidir bir **201 oluşturuldu**. Genel olarak kullanılabilir olan api sürümü henüz json’ı jsonArray olarak işleme yeteneğine sahip değildir, şu anda yalnızca önizleme api sürümü bu yeteneğe sahiptir.

REST istemcinizden aşağıdaki üç API çağrısını yürütün.

## <a name="create-a-data-source"></a>Veri kaynağı oluşturma

Bir veri kaynağına hangi verilerin dizininin belirten bir Azure Search nesnedir.

Bu çağrının uç noktası: `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin. Bu çağrı için, depolama hesabınızın adı ve depolama hesabı anahtarınız gereklidir. Depolama hesabı anahtarı, Azure portalında depolama hesabınızın **Erişim Anahtarları** bölümünde bulunur. Aşağıdaki resimde konum gösterilmektedir:

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/storagekeys.png)

Çağrıyı yürütmeden önce çağrınızın gövdesinde `[storage account name]` ve `[storage account key]` değerini değiştirdiğinizden emin olun.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Yanıt şöyle görünmelidir:

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

## <a name="create-an-index"></a>Dizin oluşturma
    
İkinci API çağrısı, bir Azure Search dizini oluşturur. Dizin, tüm parametreleri ve parametrelerin özniteliklerini belirtir.

Bu çağrının URL’si: `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

İlk olarak URL’yi değiştirin. Daha sonra aşağıdaki kodu kopyalayıp gövdeye yapıştırın ve sorguyu çalıştırın.

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

Yanıt şöyle görünmelidir:

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

## <a name="create-and-run-an-indexer"></a>Oluşturma ve bir dizin oluşturucuyu çalıştırma

Bir dizin oluşturucu veri kaynağına bağlanır, hedef search dizinine verileri alır ve isteğe bağlı olarak veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar.

Bu çağrının URL’si: `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

İlk olarak URL’yi değiştirin. Daha sonra kopyalayın ve aşağıdaki kodu kopyalayıp gövdeye yapıştırın ve istek gönderin. İstek hemen işlenir. Yanıt geri geldiğinde, tam metin dizin olacaktır aranabilir.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Yanıt şöyle görünmelidir:

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

## <a name="search-your-json-files"></a>JSON dosyalarınızı arama

Dizin sorguları artık uygulayabilirsiniz. Bu görev için kullanacağınız [ **arama Gezgini** ](search-explorer.md) portalında.

  ![Yapılandırılmamış arama](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Kullanıcı tanımlı meta veri araması

Önceden olduğu gibi veriler birçok şekilde sorgulanabilir: tam metin araması, sistem özellikleri veya kullanıcı tanımlı meta veriler. Hem sistem özellikleri hem de kullanıcı tanımlı meta veriler yalnızca hedef dizin oluşturulurken **alınabilir** olarak işaretlendiyse `$select` parametresiyle aranabilir. Dizindeki parametreler oluşturulduktan sonra değiştirilemez. Ancak ek parametreler eklenebilir.

Temel bir sorgu örneği `$select=Gender,metadata_storage_size` olup bu, dönüşü bu iki parametreyle sınırlar.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/lastquery.png)

Daha karmaşık sorgu örneği `$filter=MinimumAge ge 30 and MaximumAge lt 75` olup bu, yalnızca MinimumAge parametrelerinin 30 değerinden büyük veya bu değere eşit ve MaximumAge parametresinin 75’ten küçük olduğu sonuçları döndürür.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/metadatashort.png)

Birkaç sorguyu daha denemek istiyorsanız rahatça bunu yapabilirsiniz. Karşılaştırma işleçlerini (eq, ne, gt, lt, ge, le) değil, Mantıksal işleçleri (and, or, not) kullanabileceğinizi unutmayın. Dize karşılaştırmaları büyük/küçük harfe duyarlıdır.

`$filter` parametresi yalnızca dizininiz oluşturulurken filtrelenebilir olarak işaretlenmiş olan meta verilerle birlikte çalışır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini içeren kaynak grubunu silmektir. Kaynak grubunu silerek içindeki her şeyi kalıcı olarak silebilirsiniz. Portalda kaynak grubu adı, Azure Search hizmetinin Genel Bakış sayfasında bulunur.

## <a name="next-steps"></a>Sonraki adımlar

AI destekli algoritmaları dizin oluşturucu işlem hattına ekleyebilirsiniz. Sonraki adım olarak, aşağıdaki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Blob Depolama’da Belgelerin Dizinini Oluşturma](search-howto-indexing-azure-blob-storage.md)