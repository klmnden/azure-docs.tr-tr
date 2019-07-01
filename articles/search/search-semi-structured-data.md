---
title: 'Öğretici: Yarı strutured verileri JSON bloblarını - Azure Search dizini oluşturma'
description: Dizin ve Azure Search REST API'lerini ve Postman kullanarak yarı yapılandırılmış Azure JSON bloblarını arama hakkında bilgi edinin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 3184b839087944d8d4335927810ec31d8876866e
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485329"
---
# <a name="rest-tutorial-index-and-search-semi-structured-data-json-blobs-in-azure-search"></a>REST Öğreticisi: Dizin ve yarı yapılandırılmış verileri (JSON blobları) Azure Search'te arama

Azure arama dizin JSON belgeleri ve Azure blob depolama kullanarak dizileri bir [dizin oluşturucu](search-indexer-overview.md) yarı yapılandırılmış verileri okumak nasıl olduğunu bilir. Yarı yapılandırılmış veriler, veriler içindeki içeriği ayıran etiketleri veya işaretleri içerir. Bu tam olarak sıralanması gerekir ve bir alan başına temelinde dizine bir ilişkisel veritabanı şeması gibi bir veri modeli için uyar veri resmi olarak yapılandırılmış, yapılandırılmamış verileri birbirinden ayırır.

Bu öğreticide, [Azure Search REST API'lerini](https://docs.microsoft.com/rest/api/searchservice/) ve aşağıdaki görevleri gerçekleştirmek için bir REST istemcisi:

> [!div class="checklist"]
> * Azure blob kapsayıcısı için Azure Search veri kaynağını yapılandırma
> * Aranabilir içeriğe sahip bir Azure Search dizini oluşturma
> * Yapılandırma ve çalıştırma kapsayıcısını ve Azure blob depolama alanından aranabilir içeriği ayıklamak için dizin oluşturucu
> * Oluşturduğunuz dizini arama

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetleri, araçları ve verileri kullanılır. 

[Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz. 

[Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek verileri depolamak için.

[Postman masaüstü uygulaması](https://www.getpostman.com/) istekleri Azure Search'e göndermek için.

[Deneme json.zip Klinik](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip) Bu öğreticide kullanılan verileri içerir. İndirin ve kendi klasörüne bu dosyanın sıkıştırmasını açın. Veri kaynaklanan [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results), Bu öğretici json'a dönüştürülmüş.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-get-started-postman/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

1. [Azure portalında oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, tıklayın **Blobları**ve ardından **+ kapsayıcı**.

1. [Bir Blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) örnek verileri içerecek şekilde. Genel erişim düzeyi geçerli değerleri birini ayarlayabilirsiniz.

1. Kapsayıcıyı oluşturduktan sonra dosyayı açın ve seçin **karşıya** komut çubuğunda.

   ![Komut çubuğunda karşıya](media/search-semi-structured-data/upload-command-bar.png "komut çubuğunda karşıya yükleme")

1. Örnek dosyalarını içeren klasöre gidin. Bunların tümünü seçin ve ardından **karşıya**.

   ![Dosyaları karşıya yükleme](media/search-semi-structured-data/clinicalupload.png "dosyaları karşıya yükle")

Yükleme tamamlandıktan sonra dosyalar veri kapsayıcısında kendi alt klasöründe görünmelidir.

## <a name="set-up-postman"></a>Postman’i ayarlama

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu aracı bilmiyorsanız bkz [Azure Search REST Postman kullanarak API'lerini keşfedin](search-get-started-postman.md).

Bu öğreticideki her çağrı için istek yöntemi **POST**. Üst bilgi anahtarları "Content-type" ve "api-key"dir. Üst bilgi anahtarlarının değerleri sırayla "application/json" ve "yönetici anahtarınız"dır (yönetici anahtarı, arama birincil anahtarınız için yer tutucudur). Gövde, çağrınızın gerçek içeriklerini yerleştirdiğiniz yerdir. Kullandığınız istemciye bağlı olarak, sorgunuzu oluşturma şeklinizde bazı farklılıklar olabilir. Burada temel bilgiler verilmiştir.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/postmanoverview.png)

Arama hizmetinize üç API çağrısı yaparak veri kaynağı, dizin ve dizin oluşturucu oluşturmak için Postman kullanıyoruz. Veri kaynağı, depolama hesabınıza ve JSON verilerinize yönelik bir işaretçi içerir. Arama hizmetiniz, veriler yüklenirken bağlantı kurar.

Sorgu dizeleri, bir API sürümü ve her çağrının belirtmelisiniz döndürmelidir bir **201 oluşturuldu**. Genel kullanıma sunulan API kullanarak JSON dizileri için sürümü `2019-05-06`.

REST istemcinizden aşağıdaki üç API çağrısını yürütün.

## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

[Veri kaynağı API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)hangi verilerin dizininin oluşturulacağını belirtir bir Azure Search nesnesi oluşturur.

Bu çağrının uç noktası: `https://[service name].search.windows.net/datasources?api-version=2019-05-06`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin. 

Bu çağrı için istek gövdesi, depolama hesabı, depolama hesabı anahtarı ve blob kapsayıcısı adı adını içermelidir. Depolama hesabı anahtarı, Azure portalında depolama hesabınızın **Erişim Anahtarları** bölümünde bulunur. Aşağıdaki resimde konum gösterilmektedir:

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/storagekeys.png)

Değiştirdiğinizden emin olun `[storage account name]`, `[storage account key]`, ve `[blob container name]` çağrıyı yürütmeden önce çağrınızın gövdesinde.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "[blob container name]"}
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
        "name": "[mycontainernamehere]",
        "query": null
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

## <a name="create-an-index"></a>Dizin oluşturma
    
İkinci çağrı [oluşturma dizini API](https://docs.microsoft.com/rest/api/searchservice/create-indexer), Azure Search dizini oluşturma, tüm aranabilir verileri depolar. Dizin, tüm parametreleri ve parametrelerin özniteliklerini belirtir.

Bu çağrının URL’si: `https://[service name].search.windows.net/indexes?api-version=2019-05-06`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

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

Bir dizin oluşturucu veri kaynağına bağlanır, hedef search dizinine verileri alır ve isteğe bağlı olarak veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar. REST API [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Bu çağrının URL’si: `https://[service name].search.windows.net/indexers?api-version=2019-05-06`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

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

İlk belgenin yüklendikten hemen sonra aramayı başlayabilirsiniz. Bu görev için kullanacağınız [ **arama Gezgini** ](search-explorer.md) portalında.

Azure portalında arama hizmeti **genel bakış** sayfasında, oluşturduğunuz dizin Bul **dizinleri** listesi.

Yeni oluşturduğunuz dizin seçtiğinizden emin olun. 

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

Çeşitli yaklaşımlar ve JSON bloblarını dizine ekleme için birden çok seçenek vardır. Bir sonraki adım olarak gözden geçirin ve nelerin senaryonuz için en iyi çalıştığını görmek için çeşitli seçenekler test edin.

> [!div class="nextstepaction"]
> [Azure Search Blob Dizin Oluşturucu kullanarak JSON bloblarını dizinleme](search-howto-index-json-blobs.md)
