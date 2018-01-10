---
title: "Bir Azure Cosmos DB SQL API veri kaynağı için Azure arama dizini oluşturma | Microsoft Docs"
description: "Bu makale bir Azure Cosmos DB (SQL API) veri kaynağına sahip bir Azure Search dizin oluşturucu oluşturulacağını gösterir."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 01/08/2018
ms.author: eugenesh
robot: noindex
ms.openlocfilehash: e449f13adcd1a3651e1cac852b23f21d0227038a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Azure Search'te dizin oluşturucular kullanma Cosmos DB bağlanma

[Azure Cosmos DB](../cosmos-db/introduction.md) Microsoft'un Genel dağıtılmış birden çok model veritabanıdır. İle [SQL API](../cosmos-db/sql-api-introduction.md), Azure Cosmos DB Şeması daha az JSON verileri üzerinde zengin ve tanıdık SQL sorgu özellikleri tutarlı düşük gecikme süreleriyle sağlar. Azure arama SQL API ile sorunsuz şekilde entegre olur. Bir Azure Search dizini kullanarak doğrudan içine JSON belgelerini çekebilir bir [Azure Search dizin oluşturucusunu](search-indexer-overview.md), özellikle Azure Cosmos DB SQL API için tasarlanmıştır. 

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Azure Search Azure Cosmos DB SQL API'si veritabanı veri kaynağı olarak kullanmak üzere yapılandırın. İsteğe bağlı olarak, bir alt kümesini seçmek için bir sorgu belirtin.
> * Arama dizini JSON uyumlu veri türleri ile oluşturun.
> * Bir dizin oluşturucu isteğe bağlı ve yinelenen dizin oluşturma için yapılandırın.
> * Temel alınan verilerde yapılan değişiklikler bağlı dizin artımlı olarak yenileyin.

> [!NOTE]
> Azure Cosmos DB SQL DocumentDB yeni nesil bir API'dir. Ürün adı değiştirilmiş olsa da, `documentdb` Azure Search'te dizin oluşturucular sözdiziminde hala var için geriye dönük uyumluluk Azure arama API'leri ve portal sayfaları. Dizin oluşturucular yapılandırırken belirttiğinizden emin olun `documentdb` bu makalede belirtildiği gibi sözdizimi.

<a name="supportedAPIs"></a>

## <a name="supported-api-types"></a>Desteklenen API türleri

Çeşitli veri modelleri ve API'ları Azure Cosmos DB desteklemesine rağmen dizin oluşturucu desteği yalnızca SQL API genişletir. 

Ek API'leri için destek gelecek. İlk desteklemek için hangilerinin belirlememize yardımcı olması için kullanıcı ses web sitesinde dönüştürün:

* [Tablo API veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Grafik API'si veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [MongoDB API veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/18861421-documentdb-indexer-should-be-able-to-index-mongodb)
* [Apache Cassandra API veri kaynağı desteği](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Önkoşullar

Bir Azure Cosmos DB dizin oluşturucu ayarlamak için bilmeniz gereken bir [Azure Search Hizmeti](search-create-service-portal.md), bir dizin, veri kaynağı ve son olarak dizin oluşturucu oluşturabilirsiniz. Kullanarak bu nesneleri oluşturabilirsiniz [portal](search-import-data-portal.md), [.NET SDK'sı](/dotnet/api/microsoft.azure.search), veya [REST API](/rest/api/searchservice/) tüm .NET olmayan diller için. 

Portal için seçerseniz [verilerini İçeri Aktar Sihirbazı](search-import-data-portal.md) dizini de dahil olmak üzere tüm bu kaynakların oluşturulmasını size yol gösterir.

> [!TIP]
> İlgili veri kaynağı için dizin oluşturmayı kolaylaştırmak üzere Azure Cosmos DB panosundan **Verileri içeri aktarma** sihirbazını başlatabilirsiniz. Başlamak için sol gezinti bölmesinde **Koleksiyonlar** > **Azure Search Ekle** menüsüne gidin.

<a name="Concepts"></a>

## <a name="azure-search-indexer-concepts"></a>Azure Search dizin oluşturucu kavramları
Azure oluşturulmasını ve yönetimini veri kaynakları (Azure Cosmos DB SQL API dahil) arama destekler ve bu veri kaynaklarına karşı çalışan dizin oluşturucular.

A **veri kaynağı** verileri dizini, kimlik bilgileri ve (örneğin, değiştirilen veya silinen belgeleri koleksiyonunuzu içinde) verilerde yapılan değişiklikler tanımlamak için ilkeler belirtir. Böylece birden çok dizin oluşturucu tarafından kullanılan veri kaynağı bağımsız bir kaynak olarak tanımlanır.

Bir **dizin oluşturucu** verileri veri kaynağından bir hedef search dizinine nasıl akacağını açıklar. Bir dizin oluşturucu için kullanılabilir:

* Bir dizin doldurmak için veri tek seferlik bir kopyasını gerçekleştirin.
* Bir zamanlamada veri kaynağındaki değişikliklerle dizin eşitleme. Zamanlaması dizin oluşturucu tanımının bir parçası değildir.
* İsteğe bağlı güncelleştirmeler gerektiği gibi bir dizine çağırır.

<a name="CreateDataSource"></a>

## <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Bir veri kaynağı oluşturmak için bir POST yapın:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

İstek gövdesini aşağıdaki alanları içermelidir veri kaynağı tanımını içerir:

* **ad**: veritabanınızı temsil etmek için herhangi bir ad seçin.
* **tür**: olmalıdır `documentdb`.
* **kimlik bilgileri**:
  
  * **connectionString**: gerekli. Azure Cosmos DB veritabanınıza bağlantı bilgileri şu biçimde belirtin:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **kapsayıcı**:
  
  * **ad**: gerekli. Sıralanacak veritabanı koleksiyon kimliğini belirtin.
  * **Sorgu**: isteğe bağlı. Azure Search dizin düz bir şemaya rasgele bir JSON belgesi düzleştirmek için bir sorgu belirtebilirsiniz.
* **dataChangeDetectionPolicy**: önerilir. Bkz: [dizin değiştirilmiş belgeleri](#DataChangeDetectionPolicy) bölümü.
* **dataDeletionDetectionPolicy**: isteğe bağlı. Bkz: [silinen belgeler dizin](#DataDeletionDetectionPolicy) bölümü.

### <a name="using-queries-to-shape-indexed-data"></a>Şeklin sorgularını kullanarak veri dizini
İç içe özellikler veya diziler, proje JSON özellikleri düzleştirmek için bir SQL sorgusu belirtin ve dizine verileri filtreleyin. 

Örneğin belge:

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
Zaten yoksa, bir hedef Azure Search dizini oluşturma. Kullanarak bir dizinin oluşturabilirsiniz [Azure portal UI](search-create-index-portal.md), [oluşturma dizini REST API](/rest/api/searchservice/create-index) veya [dizin sınıfı](/dotnet/api/microsoft.azure.search.models.index).

Aşağıdaki örnek, bir kimlik ve açıklama alanı ile bir dizin oluşturur:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
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

Hedef dizin şeması kaynak JSON belgelerinin şemasını veya özel sorgu projeksiyon çıktısını ile uyumlu olduğundan emin olun.

> [!NOTE]
> Bölümlenmiş koleksiyonlar için varsayılan belge Azure Cosmos veritabanı anahtarıdır `_rid` olarak yeniden adlandırıldı özelliği `rid` Azure Search'te. Ayrıca, Azure Cosmos veritabanı `_rid` değerleri Azure Search anahtarlarında geçersiz karakterler içeriyor. Bu nedenle, `_rid` Base64 ile kodlanmış değerlerdir.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>JSON veri türleri ve Azure Search'te veri türleri arasında eşleme
| JSON veri türü | Uyumlu hedef dizin alan türleri |
| --- | --- |
| bool |Edm.Boolean, Edm.String |
| Tamsayıları gibi ara numaraları |EDM.Int32, EDM.Int64, Edm.String |
| Bu görünümlü kayan nokta sayıları |Edm.Double, Edm.String |
| Dize |Edm.String |
| ["A", "b", "c"] örneğin ilkel türlerin dizileri |Collection(Edm.String) |
| Tarihleri gibi görünen dizeleri |Edm.DateTimeOffset, Edm.String |
| GeoJSON nesneleri, örneğin {"tür": "Point", "coordinates": [uzun lat]} |Edm.GeographyPoint |
| Diğer JSON nesneleri |Yok |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>3. adım: bir dizin oluşturucu yapın

Dizinin ve veri kaynağının oluşturduktan sonra Dizin Oluşturucu oluşturmak hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizin oluşturucu (zamanlama aralığı "PT2H" ayarlanır) iki saatte çalışır. 30 dakikada bir dizin oluşturucu çalıştırmak için "PT30M" aralığını ayarlayın. En kısa desteklenen aralığı 5 dakikadır. Zamanlama atlanırsa, isteğe bağlı -, yalnızca zaman bir kez oluşturulduktan sonra bir dizin oluşturucu çalıştırır. Ancak, bir dizin oluşturucu isteğe bağlı herhangi bir zamanda çalıştırabilirsiniz.   

Oluşturma dizin oluşturucu API'si hakkında daha fazla ayrıntı için kullanıma [oluşturma dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Dizin Oluşturucu isteğe bağlı çalıştırma
Düzenli bir zamanlamaya göre çalıştırmanın yanı sıra, bir dizin oluşturucu, ayrıca isteğe bağlı olarak çağrılabilir:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Çalıştır API başarıyla geri döndüğünde, dizin oluşturucu çağırma zamanlandı, ancak gerçek işlemini zaman uyumsuz olarak gerçekleşir. 

Dizin Oluşturucu durumunu portalında veya sonraki açıklamak alma dizin oluşturucu durumu API kullanarak izleyebilirsiniz. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Dizin Oluşturucu durumunu alma
Bir dizin oluşturucu durumunu ve yürütme geçmişini alabilirsiniz:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

Yanıt, genel dizin oluşturucu durumu, son (veya devam eden) dizin oluşturucu çağırma ve son dizin oluşturucu çağrılarını geçmişini içerir.

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

Yürütme geçmişi (son yürütme yanıtta önce geliyorsa şekilde), ters kronolojik sırada sıralanır 50 en son tamamlanan yürütmeleri kadar içerir.

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Değiştirilmiş belgeleri dizin oluşturma
Verimli bir şekilde değiştirilmiş veri öğeleri tanımlamak için bir veri değişikliği algılama ilkesi amacı budur. Şu anda, yalnızca desteklenen ilkedir `High Water Mark` İlkesi'ni kullanarak `_ts` Azure Cosmos gibi belirtilen DB tarafından sağlanan (zaman damgası) özelliği:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Bu ilkeyi kullanarak iyi dizin oluşturucu performans sağlamak için önerilir. 

Özel bir sorgu kullanıyorsanız, emin olun `_ts` özelliği sorgunun tahmini.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Artımlı ilerleme ve özel sorgular
Dizin oluşturma sırasında artımlı ilerleme dizin oluşturucusu yürütme geçici hataları veya yürütme süresi sınırını kesintiye uğrarsa, dizin oluşturucu, tüm koleksiyon en baştan yeniden dizin gerek kalmadan yerine İleri çalıştırıldığında kaldığı yerden yukarı seçim yapabilirsiniz sağlar. Bu büyük topluluklara dizin oluştururken özellikle önemlidir. 

Özel bir sorgu kullanarak artımlı ilerleme etkinleştirmek için sorgu sonuçlarına göre sıralar olun `_ts` sütun. Bu, Dönemsel onay hataları varlığında artımlı ilerleme sağlamak için Azure Search kullanan dönük sağlar.   

Bazı durumlarda, sorgunuzu içeriyor olsa bile bir `ORDER BY [collection alias]._ts` yan tümcesi, Azure Search değil Infer sorgu bazında `_ts`. Azure arama sonuçları kullanarak sıralanır anlayabilirsiniz `assumeOrderByHighWaterMarkColumn` Yapılandırma özelliği. Bu ipucu belirtmek için oluşturmak veya oluşturucunuz şu şekilde güncelleştirin: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Dizin oluşturma belgeleri silindi
Koleksiyondan silinen satır, normalde bu satır arama dizini de silmek istiyor. Silinen veri öğeleri verimli bir şekilde tanımlamak için bir veri silme algılama ilkesi amacı budur. Şu anda, yalnızca desteklenen ilkedir `Soft Delete` İlkesi (silme bazı sıralama bayrağıyla işaretlenmiş), olduğu gibi belirtildi:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Özel bir sorgu kullanıyorsanız, özellik tarafından başvurulan emin olun `softDeleteColumnName` sorgu tarafından yansıtılır.

Aşağıdaki örnek, bir veri kaynağı ile bir geçici silme ilkesi oluşturur:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
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
Tebrikler! Gezinme ve bir SQL veri modeli belgeleri karşıya yüklemek için bir dizin oluşturucu kullanarak Azure Search'te Azure Cosmos DB tümleştirileceğini öğrendiniz.

* Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB hizmeti sayfası](https://azure.microsoft.com/services/cosmos-db/).
* Azure Search hakkında daha fazla bilgi için bkz: [arama hizmeti sayfası](https://azure.microsoft.com/services/search/).
