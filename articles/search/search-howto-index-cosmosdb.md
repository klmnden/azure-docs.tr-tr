---
title: Bir Azure Cosmos DB veri kaynağı - Azure Search dizini
description: Bir Azure Cosmos DB veri kaynağında gezinen ve Azure Search'te tam metin arama yapılabilir bir dizin verileri alma. Dizin oluşturucular veri alımı Azure Cosmos DB gibi seçili veri kaynakları için otomatik hale getirin.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
robot: noindex
ms.custom: seodec2018
ms.openlocfilehash: d63fdbfd71e812e9b445fb0055cb9aee5876ecc1
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962154"
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Dizin oluşturucuları kullanarak Azure Search ile Cosmos DB'ye bağlanma

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Yapılandırma [Azure Search dizin oluşturucu](search-indexer-overview.md) , bir Azure Cosmos DB koleksiyonu veri kaynağı olarak kullanır.
> * JSON-uyumlu veri türleri ile bir arama dizini oluşturun.
> * İsteğe bağlı ve yinelenen dizin için dizin oluşturucuyu yapılandırın.
> * Temel alınan verilerde yapılan değişiklikler bağlı dizin artımlı olarak yenileyin.

> [!NOTE]
> Azure Cosmos DB, documentdb'nin yeni nesil olur. Ürün adı değiştirilmiş olsa da, `documentdb` söz dizimi Azure Search dizin oluşturucularında hala var için geriye dönük uyumluluk Azure arama API'leri ve portal sayfaları. Dizin oluşturucular yapılandırırken belirttiğinizden emin olun `documentdb` bu makalede anlatılan şekilde bir söz dizimi.

Aşağıdaki videoda, Azure Cosmos DB Program Yöneticisi Manager Andrew Liu nasıl bir Azure Cosmos DB kapsayıcısı için bir Azure Search dizini ekleneceğini gösterir.

>[!VIDEO https://www.youtube.com/embed/OyoYu1Wzk4w]

<a name="supportedAPIs"></a>
## <a name="supported-api-types"></a>Desteklenen API türleri

Azure Cosmos DB çeşitli veri modellerini ve API'lerini destekler; ancak, yalnızca SQL API'si için Azure Search dizin oluşturucu üretim desteği genişletir. MongoDB için Azure Cosmos DB API desteği şu anda genel Önizleme aşamasındadır.  

Ek API'ler için destek, gelecek. İlk desteklemek için hangilerinin belirlememize yardımcı olmak için oyunuzu User Voice web sitesinde dönüştürün:

* [Tablo API veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Graph API veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [Apache Cassandra API'si veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Önkoşullar

Cosmos DB hesabının yanı sıra, olması gerekir. bir [Azure Search Hizmeti](search-create-service-portal.md). 

Cosmos DB hesabınızdaki tüm belgelerin otomatik olarak dizinini koleksiyonu isteyip istemediğinizi seçebilirsiniz. Varsayılan olarak, tüm belgelerin otomatik olarak dizine alınır, ancak otomatik dizin oluşturma devre dışı kapatabilirsiniz. Dizin oluşturmayı devre dışı bırakıldığında, belgelerin yalnızca aracılığıyla erişilebilen kendi kendine bağlantılar veya belge kullanarak sorgular tarafından kimliği Azure arama, Cosmos DB, Azure Search tarafından dizine koleksiyonda açık dizin otomatik gerektirir. 

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Azure Search dizin oluşturucu kavramları

A **veri kaynağı** dizin ve kimlik bilgilerini (örneğin, değiştirilen veya silinen belgeleri koleksiyonunuz içinde) verilerdeki değişiklikleri tanımlamaya yönelik ilkeleri için verileri belirtir. Böylece birden çok dizin oluşturucu tarafından kullanılan veri kaynağı bağımsız bir kaynak olarak tanımlanır.

Bir **dizin oluşturucu** verilerin veri kaynağınızdaki bir hedef search dizinine nasıl aktığını açıklar. Bir dizin oluşturucu için kullanılabilir:

* Veri dizini doldurmak için tek seferlik bir kopyasını gerçekleştirin.
* Dizin bir zamanlamada veri kaynağındaki değişiklikleri ile eşitleyin.
* İsteğe bağlı güncelleştirmeler gerektiği gibi bir dizin için çağırır.

Bir Azure Cosmos DB dizinleyici ayarlamak için bir dizin, veri kaynağı ve son olarak dizin oluşturucuyu oluşturmak gerekir. Bu nesneleri kullanarak oluşturabileceğiniz [portalı](search-import-data-portal.md), [.NET SDK'sı](/dotnet/api/microsoft.azure.search), veya [REST API](/rest/api/searchservice/). 

Bu makalede REST API'SİNİN nasıl kullanılacağı gösterilmektedir. İçin portalı kullanmayı seçerseniz, Cosmos DB veritabanınıza veri içerdiğinden emin olun. [Veri Alma Sihirbazı](search-import-data-portal.md) meta verileri okur ve bir dizin şemasını, ancak çıkarsamak için veri örnekleme yükleri veri Cosmos DB'den de gerçekleştirir. Veri yoksa, Sihirbazı ile bu hata durur. "veri kaynağından hata algılama dizin şeması: Veri kaynağı 'emptycollection' hiçbir veri döndürdüğünden, bir prototip dizini oluşturulamadı. ".

> [!TIP]
> İlgili veri kaynağı için dizin oluşturmayı kolaylaştırmak üzere Azure Cosmos DB panosundan **Verileri içeri aktarma** sihirbazını başlatabilirsiniz. Başlamak için sol gezinti bölmesinde **Koleksiyonlar** > **Azure Search Ekle** menüsüne gidin.

> [!NOTE] 
> Şimdilik, oluşturma düzenleyin veya silemeyeceğiniz **MongoDB** Azure portalı veya .NET SDK kullanarak veri kaynakları. Ancak, **olabilir** MongoDB dizin oluşturucular portalda yürütme geçmişini izleyin.  

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Bir veri kaynağı oluşturmak için bir GÖNDERİ yapın:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCollection", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

İstek gövdesi aşağıdaki alanları içermelidir veri kaynağı tanımını içerir:

* **Ad**: Veritabanınızı temsil etmek için herhangi bir ad seçin.
* **Tür**: Olmalıdır `documentdb`.
* **kimlik bilgileri**:
  
  * **connectionString**: Gereklidir. Azure Cosmos DB veritabanınıza bağlantı bilgisi şu biçimde belirtin: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>` MongoDB koleksiyonu için ekleyin **api türü MongoDb =** bağlantı dizesi: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`
  Uç nokta URL'si bağlantı noktası numaralarını kaçının. Bağlantı noktası numarası dahil, Azure Search, Azure Cosmos DB veritabanının dizinini oluşturmak mümkün olmayacaktır.
* **kapsayıcı**:
  
  * **Ad**: Gereklidir. Sıralanacak veritabanı koleksiyonu kimliği belirtin.
  * **Sorgu**: İsteğe bağlı. Azure Search'ün dizin bir düz şemasına rastgele bir JSON belgesi düzleştirmek için sorgu belirtebilirsiniz. MongoDB koleksiyonlar, sorgular desteklenmez. 
* **dataChangeDetectionPolicy**: Önerilir. Bkz: [değiştirilen belgeler dizin](#DataChangeDetectionPolicy) bölümü.
* **dataDeletionDetectionPolicy**: İsteğe bağlı. Bkz: [silinen belgeler dizin](#DataDeletionDetectionPolicy) bölümü.

### <a name="using-queries-to-shape-indexed-data"></a>Veri şekli sorgularını kullanarak dizini
İç içe özellikler veya dizileri, proje JSON özellikleri düzleştirmek için bir SQL sorgusunu belirtin ve dizine verileri filtreleyin. 

> [!WARNING]
> İçin özel sorgular desteklenmez **MongoDB** koleksiyonları: `container.query` parametresi boş veya belirtilmemiş ayarlanmalıdır. Özel bir sorgu kullanmanız gerekiyorsa, lütfen bize bilmeniz [User Voice](https://feedback.azure.com/forums/263029-azure-search).

Örnek Belge:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Filtre sorgusu:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Düzleştirme sorgusu:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projeksiyon sorgusu:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Düzleştirme sorgu dizisi:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Zaten yoksa, bir hedef Azure Search dizini oluşturma. Kullanarak bir dizin oluşturun [Azure portalı kullanıcı arabirimini](search-create-index-portal.md), [dizin REST API oluşturma](/rest/api/searchservice/create-index) veya [dizin sınıfı](/dotnet/api/microsoft.azure.search.models.index).

Aşağıdaki örnek, bir kimlik ve açıklama alanı ile bir dizin oluşturur:

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Hedef dizin şemasını kaynak JSON belgelerinin şemasını veya kendi özel sorgu projeksiyon çıktısı ile uyumlu olduğundan emin olun.

> [!NOTE]
> Bölümlenmiş koleksiyonlar için Azure Cosmos DB'nin varsayılan belge anahtarını olan `_rid` özelliği için Azure Search otomatik olarak yeniden adlandırır `rid` alan adları bir undescore karakteriyle başlayamaz çünkü. Ayrıca, Azure Cosmos DB `_rid` değerleri, Azure Search anahtarlarında geçersiz karakterler içeremez. Bu nedenle, `_rid` Base64 kodlu değerler.
> 
> Azure Search MongoDB koleksiyonlar için otomatik olarak yeniden adlandırır `_id` özelliğini `doc_id`.  

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>JSON veri türleri ve Azure Search veri türleri arasında eşleme
| JSON veri türü | Uyumlu hedef dizin alan türleri |
| --- | --- |
| Bool |Edm.Boolean, Edm.String |
| Tam sayılar gibi görünen sayı |EDM.Int32, EDM.Int64, Edm.String |
| Görünüm gibi kayan nokta numaraları |Edm.Double, Edm.String |
| String |Edm.String |
| ["A", "b", "c"] Örneğin, ilkel türlerin dizileri |Collection(Edm.String) |
| Tarihler gibi görünen dizeleri |Edm.DateTimeOffset, Edm.String |
| GeoJSON nesneleri, örneğin {"type": "Nokta", "koordinatları": [uzun lat]} |Edm.GeographyPoint |
| Diğer bir JSON nesnesi |Yok |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>3. Adım: Dizin oluşturucu oluşturma

Dizinin ve veri kaynağının oluşturulan dizin oluşturucu oluşturmaya hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizin oluşturucu ("PT2H için" zamanlama aralığı ayarlanır) iki saatte çalışır. 30 dakikada bir dizin oluşturucu çalıştırmak için "PT30M" için aralığı ayarlayın. Kısa desteklenen aralığı 5 dakikadır. Zamanlama atlanırsa isteğe bağlıdır, dizin oluşturucu yalnızca oluştururken bir kez çalışır. Ancak, bir dizin oluşturucu isteğe bağlı herhangi bir zamanda çalıştırabilirsiniz.   

Dizin Oluşturucu Oluşturma API'si hakkında daha fazla ayrıntı için kullanıma [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Dizin Oluşturucu üzerine çalıştırma
Düzenli bir zamanlamaya göre çalıştırmanın yanı sıra, bir dizin oluşturucu, ayrıca isteğe bağlı olarak çağrılabilir:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2017-11-11
    api-key: [Search service admin key]

> [!NOTE]
> Çalıştırma API başarıyla geri döndüğünde, dizin oluşturucu çağrı zamanlandı, ancak gerçek işleme zaman uyumsuz olarak gerçekleşir. 

Portalı veya sonraki açıklayan alma dizin oluşturucu durumu API'si kullanarak dizin oluşturucu durumunu izleyebilirsiniz. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Dizin Oluşturucu Durum alınıyor
Bir dizin oluşturucu durumu ve yürütme geçmişini alabilirsiniz:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2017-11-11
    api-key: [Search service admin key]

Yanıt, genel dizin oluşturucu durumu, son (veya devam eden) dizin oluşturucuyu çağırmayı ve son dizin oluşturucu çağrılarını geçmişini içerir.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Bu nedenle (en son yürütme yanıtta önce gelirse), ters kronolojik sırada saklanıyor 50 en son tamamlanan yürütme, en fazla yürütme geçmişini içerir.

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Değiştirilen belgeler dizin oluşturma
Veri değişiklik algılama ilkesi amacı değiştirilmiş veri öğeleri verimli bir şekilde belirlemektir. Şu anda, yalnızca desteklenen ilkedir `High Water Mark` İlkesi'ni kullanarak `_ts` Azure Cosmos aşağıda belirtilen DB tarafından sağlanan (zaman damgası) özelliği:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Bu ilkeyi kullanan iyi dizin oluşturucu bir performans sağlamak için önerilir. 

Özel sorgu kullanıyorsanız emin `_ts` özelliği sorgu tarafından yansıtılır.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Artımlı ilerleme durumunu ve özel sorgular
Dizin oluşturma sırasında artımlı ilerleme durumunu, dizin oluşturucusu yürütme geçici hatalar ya da yürütme süresi sınırından kesintiye uğrarsa, dizin oluşturucu, tüm koleksiyon sıfırdan yeniden gerek kalmadan yerine sonraki çalıştırıldığında kaldığı yukarı seçim yapabilirsiniz sağlar. Bu büyük koleksiyonlar dizin oluşturulurken özellikle önemlidir. 

Özel bir sorgu kullanarak artımlı ilerleme durumunu etkinleştirmek için sorgu sonuçlarına göre sıralar olun `_ts` sütun. Bu, düzenli onay oluşturucunun hata olması durumunda artımlı ilerleme sağlamak için Azure Search kullanan dönük sağlar.   

Bazı durumlarda, sorgunuzu içeriyor olsa bile bir `ORDER BY [collection alias]._ts` yan tümcesi, Azure Search değil Infer sorgu tarafından sıralanır `_ts`. Azure arama sonuçları kullanılarak sıralanır söyleyebilirsiniz `assumeOrderByHighWaterMarkColumn` yapılandırma özellik. Bu ipucu belirtmek için oluşturma veya dizin şu şekilde güncelleştirin: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Dizin oluşturma belgeleri silindi
Koleksiyondan silinen satır, normalde arama dizini de satırları silmek istediğiniz. Verileri silme algılama ilkesi amacı silinen veri öğeleri verimli bir şekilde belirlemektir. Şu anda, yalnızca desteklenen ilkedir `Soft Delete` İlkesi (silme bir çeşit bayrağıyla işaretlenmiş), olduğu gibi belirtildi:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Özel sorgu kullanıyorsanız, özellik tarafından başvurulan emin `softDeleteColumnName` sorgu tarafından gösterilmiyor.

Aşağıdaki örnek, bir veri kaynağı bir geçici silme ilkesi oluşturur:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Sonraki adımlar
Tebrikler! Azure Search Dizin Oluşturucu kullanarak Azure Cosmos DB tümleştirmeyi öğrendiniz.

* Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB hizmet sayfasında](https://azure.microsoft.com/services/cosmos-db/).
* Azure arama hakkında daha fazla bilgi edinmek için [arama hizmeti sayfasını](https://azure.microsoft.com/services/search/).
