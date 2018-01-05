---
title: "Cosmos DB veri kaynağı için Azure Search dizini oluşturma | Microsoft Docs"
description: "Bu makale bir Azure Search dizin oluşturucu kişilere Cosmos DB veri kaynağı olarak nasıl oluşturulacağını gösterir."
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
ms.date: 08/10/2017
ms.author: eugenesh
robot: noindex
ms.openlocfilehash: c7c883f683c744415a1b600cea45c1882939e021
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Azure Search'te dizin oluşturucular kullanma Cosmos DB bağlanma

Cosmos DB verilerinizi harika arama deneyimi uygulamak istiyorsanız, bir Azure Search Dizin Oluşturucu veri çekmek için bir Azure Search dizinine kullanabilirsiniz. Bu makalede, Azure Cosmos DB dizin altyapı korumak için herhangi bir kod yazmak zorunda kalmadan Azure Search ile tümleştirmek nasıl gösteriyoruz.

Cosmos DB dizin oluşturucu ayarlamak için bilmeniz gereken bir [Azure Search Hizmeti](search-create-service-portal.md), bir dizin, veri kaynağı ve son olarak dizin oluşturucu oluşturabilirsiniz. Kullanarak bu nesneleri oluşturabilirsiniz [portal](search-import-data-portal.md), [.NET SDK'sı](/dotnet/api/microsoft.azure.search), veya [REST API](/rest/api/searchservice/) tüm .NET olmayan diller için. 

Portal için seçerseniz [verilerini İçeri Aktar Sihirbazı](search-import-data-portal.md) , tüm bu kaynaklar oluşturulmasını size yol gösterir.

> [!NOTE]
> Azure Cosmos DB DocumentDB gelecek nesil ' dir. Ürün adı değiştirilmiş olsa da, söz dizimi önce aynıdır. Belirtmek Lütfen devam `documentdb` bu dizin oluşturucu makalede yönergelerine uygun olarak. 

> [!TIP]
> Başlatabilirsiniz **veri içeri aktarma** bu veri kaynağı için dizin oluşturma basitleştirmek için Cosmos DB panosundan Sihirbazı. Başlamak için sol gezinti bölmesinde **Koleksiyonlar** > **Azure Search Ekle** menüsüne gidin.

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Azure Search dizin oluşturucu kavramları
Ve bu veri kaynaklarına karşı çalışan dizin oluşturucular oluşturulmasını ve yönetimini veri kaynakları (Cosmos DB dahil) azure arama destekler.

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

* **ad**: Cosmos DB veritabanınızı temsil etmek için herhangi bir ad seçin.
* **tür**: olmalıdır `documentdb`.
* **kimlik bilgileri**:
  
  * **connectionString**: gerekli. Azure Cosmos DB veritabanınıza bağlantı bilgileri şu biçimde belirtin:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **kapsayıcı**:
  
  * **ad**: gerekli. Sıralanacak Cosmos DB koleksiyon kimliğini belirtin.
  * **Sorgu**: isteğe bağlı. Azure Search dizin düz bir şemaya rasgele bir JSON belgesi düzleştirmek için bir sorgu belirtebilirsiniz.
* **dataChangeDetectionPolicy**: önerilir. Bkz: [dizin değiştirilmiş belgeleri](#DataChangeDetectionPolicy) bölümü.
* **dataDeletionDetectionPolicy**: isteğe bağlı. Bkz: [silinen belgeler dizin](#DataDeletionDetectionPolicy) bölümü.

### <a name="using-queries-to-shape-indexed-data"></a>Şeklin sorgularını kullanarak veri dizini
İç içe özellikler veya diziler, proje JSON özellikleri düzleştirmek için Cosmos DB sorgusu belirtin ve dizine verileri filtreleyin. 

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
> Bölümlenmiş koleksiyonlar için varsayılan belge Cosmos veritabanı anahtarıdır `_rid` olarak yeniden adlandırıldı özelliği `rid` Azure Search'te. Ayrıca, Cosmos veritabanı `_rid` değerleri Azure Search anahtarlarında geçersiz karakterler içeriyor. Bu nedenle, `_rid` Base64 ile kodlanmış değerlerdir.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>JSON veri türleri ve Azure Search'te veri türleri arasında eşleme
| JSON VERİ TÜRÜ | UYUMLU HEDEF DİZİN ALAN TÜRLERİ |
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
Verimli bir şekilde değiştirilmiş veri öğeleri tanımlamak için bir veri değişikliği algılama ilkesi amacı budur. Şu anda, yalnızca desteklenen ilkedir `High Water Mark` İlkesi'ni kullanarak `_ts` (zaman damgası) özelliğini aşağıdaki gibi belirtilen Cosmos DB tarafından sağlanan:

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
Tebrikler! Cosmos DB için dizin oluşturucu kullanarak Azure Search'te Azure Cosmos DB tümleştirileceğini öğrendiniz.

* Daha fazla nasıl Azure Cosmos DB hakkında bilgi edinmek için [Azure Cosmos DB hizmeti sayfası](https://azure.microsoft.com/services/cosmos-db/).
* Daha fazla nasıl Azure Search hakkında bilgi edinmek için [arama hizmeti sayfası](https://azure.microsoft.com/services/search/).
