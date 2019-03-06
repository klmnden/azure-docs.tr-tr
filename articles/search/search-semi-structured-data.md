---
title: JSON Azure Blob storage'da - Azure Search arama Öğreticisi
description: Bu öğreticide, Azure Search kullanarak yarı yapılandırılmış Azure blob verilerini aramayı öğreneceksiniz.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 44c818cba760fb5cd7d496fd45ea321ef38248f3
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57445101"
---
# <a name="tutorial-search-semi-structured-data-in-azure-cloud-storage"></a>Öğretici: Azure bulut depolamada yarı yapılandırılmış verileri arama

İki bölümden oluşan öğretici serisinde, Azure Search kullanarak yarı yapılandırılmış ve yapılandırılmamış verilerin nasıl aranacağını öğrenirsiniz. [1. Bölüm](../storage/blobs/storage-unstructured-search.md)’de yapılandırılmamış veriler üzerinde arama yapma adımları verilmiş olup bu öğretici için depolama hesabı oluşturma gibi önemli önkoşullara da yer verilmiştir. 

2. Bölüm’de, Azure bloblarında depolanan JSON gibi yarı yapılandırılmış verilere odaklanılmaktadır. Yarı yapılandırılmış veriler, veriler içindeki içeriği ayıran etiketleri veya işaretleri içerir. Bu tam olarak sıralanması gerekir ve bir alan başına temelinde gezinilebilen ilişkisel veritabanı şeması gibi bir veri modeli için uyar veri resmi olarak yapılandırılmış, yapılandırılmamış verileri birbirinden ayırır.

2. Bölüm’de aşağıdakileri öğreneceksiniz:

> [!div class="checklist"]
> * Azure blob kapsayıcısı için Azure Search veri kaynağını yapılandırma
> * Kapsayıcıda gezinmek ve aranabilir içeriği ayıklamak için Azure Search dizini ve dizinleyicisi oluşturma ve doldurma
> * Oluşturduğunuz dizini arama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Önceki öğreticide oluşturulan arama hizmetini ve depolama hesabını sağlayan [önceki öğreticinin](../storage/blobs/storage-unstructured-search.md) tamamlanması.

* REST istemcisinin yüklenmesi ve HTTP isteğinin nasıl oluşturulduğunun anlaşılması. Bu öğreticinin amaçları doğrultusunda [Postman](https://www.getpostman.com/) kullanıyoruz. Belirli bir REST istemcisinden memnun değilseniz farklı bir REST istemcisi kullanabilirsiniz.

> [!NOTE]
> Bu öğreticide, şu anda Azure Search’te önizleme özelliği olan JSON dizisi desteğinden yararlanılmaktadır. Portalda kullanılma sunulmamıştır. Bu nedenle, API’yi çağırmak için REST istemci aracını ve bu özelliği sağlayan önizleme REST API’sini kullanıyoruz.

## <a name="set-up-postman"></a>Postman’i ayarlama

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu aracı bilmiyorsanız daha fazla bilgi için bkz. [Fiddler veya Postman kullanarak Azure Search REST API’lerini bulma](search-fiddler.md).

Bu öğreticideki her çağrı için istek yöntemi "POST" yöntemidir. Üst bilgi anahtarları "Content-type" ve "api-key"dir. Üst bilgi anahtarlarının değerleri sırayla "application/json" ve "yönetici anahtarınız"dır (yönetici anahtarı, arama birincil anahtarınız için yer tutucudur). Gövde, çağrınızın gerçek içeriklerini yerleştirdiğiniz yerdir. Kullandığınız istemciye bağlı olarak, sorgunuzu oluşturma şeklinizde bazı farklılıklar olabilir. Burada temel bilgiler verilmiştir.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/postmanoverview.png)

Bu öğreticide ele alınan REST çağrıları için arama api-key değeriniz gereklidir. Arama hizmetinizin içinde **Anahtarlar** bölümünde api-key değerinizi bulabilirsiniz. Bu api-key, bu öğreticinin sizi yönlendirdiği her API çağrısının üst bilgisinde olmalıdır (önceki ekran görüntüsünde "yönetici anahtarını" bununla değiştirin). Her çağrı için gerekli olduğundan anahtarı saklayın.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Örnek verileri indirme

Sizin için bir örnek veri kümesi hazırlandı. **[clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** dosyasını indirip kendi klasörüne ayıklayın.

Örnekte, özgün olarak [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) adresinden alınan metin dosyaları olan örnek JSON dosyaları yer alır. Size kolaylık sağlaması için bunları JSON’a dönüştürdük.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="upload-the-sample-data"></a>Örnek verileri karşıya yükleme

Azure portalında, [önceki öğreticide](../storage/blobs/storage-unstructured-search.md) oluşturduğunuz depolama hesabına geri dönün. Ardından **veri** kapsayıcısını açın ve **Karşıya yükle**’ye tıklayın.

**Gelişmiş**’e tıklayın, "clinical-trials-json" değerini girin ve sonra indirdiğiniz tüm JSON dosyalarını karşıya yükleyin.

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/clinicalupload.png)

Yükleme tamamlandıktan sonra dosyalar veri kapsayıcısında kendi alt klasöründe görünmelidir.

## <a name="connect-your-search-service-to-your-container"></a>Arama hizmetinizi, kapsayıcınıza bağlama

Arama hizmetinize üç API çağrısı yaparak veri kaynağı, dizin ve dizin oluşturucu oluşturmak için Postman kullanıyoruz. Veri kaynağı, depolama hesabınıza ve JSON verilerinize yönelik bir işaretçi içerir. Arama hizmetiniz, veriler yüklenirken bağlantı kurar.

Sorgu dizesi, **api-version=2016-09-01-Preview** değerini içermeli ve her bir çağrı, **201 - Oluşturuldu** değerini döndürmelidir. Genel olarak kullanılabilir olan api sürümü henüz json’ı jsonArray olarak işleme yeteneğine sahip değildir, şu anda yalnızca önizleme api sürümü bu yeteneğe sahiptir.

REST istemcinizden aşağıdaki üç API çağrısını yürütün.

### <a name="create-a-datasource"></a>Veritabanı oluşturma

Veri kaynağı, hangi verilerin dizininin oluşturulacağını belirtir.

Bu çağrının uç noktası: `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

Bu çağrı için, depolama hesabınızın adı ve depolama hesabı anahtarınız gereklidir. Depolama hesabı anahtarı, Azure portalında depolama hesabınızın **Erişim Anahtarları** bölümünde bulunur. Aşağıdaki resimde konum gösterilmektedir:

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

### <a name="create-an-index"></a>Dizin oluşturma
    
İkinci API çağrısı bir dizin oluşturur. Dizin, tüm parametreleri ve parametrelerin özniteliklerini belirtir.

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

### <a name="create-an-indexer"></a>Dizin oluşturucu oluşturma

Dizin oluşturucu, veri kaynağını hedef arama dizinine bağlar ve isteğe bağlı olarak veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar.

Bu çağrının URL’si: `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

İlk olarak URL’yi değiştirin. Daha sonra aşağıdaki kodu kopyalayıp gövdeye yapıştırın ve sorguyu çalıştırın.

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

Arama hizmetiniz, veri kapsayıcınıza bağlandığından artık dosyalarınızı aramaya başlayabilirsiniz.

Azure portalını açın ve arama hizmetinize geri dönün. Önceki öğreticide yaptığınız gibi.

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