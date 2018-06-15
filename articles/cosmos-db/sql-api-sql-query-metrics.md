---
title: SQL sorgu ölçümleri Azure Cosmos DB SQL API'si | Microsoft Docs
description: İzleme ve Azure Cosmos DB istekleri SQL sorgu performansını hata ayıklama hakkında bilgi edinin.
keywords: SQL söz dizimi, sql sorgusu, sql sorguları, json sorgu dili, veritabanı kavramlarını ve sql sorguları, toplama işlevleri
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: sngun
ms.openlocfilehash: 4ed0008f4b574691387d6e0ee0300b5f05f1ec1b
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34798704"
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a>Sorgu performans Azure Cosmos DB ile ayarlama

Azure Cosmos DB sağlayan bir [SQL veri sorgulama için API](sql-api-sql-query.md), şema veya ikincil dizinler gerektirmeden. Bu makalede, geliştiriciler için aşağıdaki bilgileri sağlar:

* Azure Cosmos veritabanı SQL sorgu yürütme nasıl çalıştığı hakkında üst düzey ayrıntıları
* Sorgu istek ve yanıt üstbilgileri ve istemci SDK seçenekleri hakkında ayrıntılar
* İpuçları ve sorgu performansı için en iyi yöntemler
* Sorgu performansı hata ayıklamak için SQL yürütme istatistikleri kullanmaya nasıl örnekleri

## <a name="about-sql-query-execution"></a>SQL sorgu yürütme hakkında

Azure Cosmos DB'de herhangi büyüyebilir kapsayıcıları veri deposundaki [depolama boyutu veya istek işleme](partition-data.md). Azure Cosmos DB verileri veri artışını işlemek veya sağlanan performansı artırmak için kapsar altında fiziksel bölümleri arasında sorunsuz bir şekilde ölçeklendirir. REST API veya desteklenen birini kullanarak herhangi bir kapsayıcıya SQL sorguları verebilir [SQL SDK'ları](sql-api-sdk-dotnet.md).

Bölümlendirme, kısa bir genel bakış: veri fiziksel bölümleri arasında nasıl bölünür belirler "Şehir" gibi bir bölüm anahtarı tanımlayın. Tek bir bölüm anahtarına ait veri (örneğin, "Şehir" "Seattle" ==) fiziksel bir bölüm içinde depolanır ancak genellikle tek bir fiziksel bölüm birden çok bölüm anahtarlarını sahiptir. Bir bölümü depolama boyutuna ulaştığında, hizmet sorunsuz bir şekilde bölüm iki yeni bölümlere ayırır ve bu bölümler bölüm anahtarı tam olarak böler. Bölümler geçici olduğundan, "Bölüm anahtarı karmaları aralıklarına gösterir bir bölüm anahtarı aralığı" için bir Özet API'leri kullanın. 

Bir sorgu için Azure Cosmos DB verdiğinizde, SDK mantıksal adımları gerçekleştirir:

* Sorgu yürütme planı belirlemek için SQL sorgu ayrıştırılamadı. 
* Sorgu, bir filtre bölüm anahtarı karşı içeriyorsa, ister `SELECT * FROM c WHERE c.city = "Seattle"`, tek bir bölüm yönlendirilir. Sorgu bölüm anahtarı üzerinde bir filtre yok sonra içindeki tüm bölümlerin yürütülür ve sonuçları birleştirilir istemci tarafı.
* Sorgu her bölümü seri içinde yürütülen veya paralel, istemci yapılandırmasına bağlı olarak. Her bölüm içinde sorgu birini yapabilir veya sorgu karmaşıklığına bağlı olarak daha fazla gidiş dönüş sayfa boyutu yapılandırılmış ve koleksiyon verimini sağlandı. Her yürütme sayısını döndürür [istek birimleri](request-units.md) sorgu yürütme ve isteğe bağlı olarak, sorgu yürütme istatistikleri tüketilen. 
* SDK, bölümler sorgu sonuçlarının bir özetini gerçekleştirir. Sorgu bir ORDER BY bölümler içeriyorsa, örneğin, daha sonra bireysel bölümleri sonuçlarından birleştirme-sıralanmış genel olarak sıralanmış olarak sonuçları döndürmek için. Sorgu gibi bir toplama ise `COUNT`, tek tek bölümlerden birinden sayıları genel sayısı üretmek için toplanır.

SDK'ları sorgu yürütmesi için çeşitli seçenekler sağlar. Örneğin, .NET içinde bu seçenekleri mevcuttur `FeedOptions` sınıfı. Aşağıdaki tabloda, bu seçenekler ve bunların nasıl sorgusu yürütme süresi etkisi açıklanmaktadır. 

| Seçenek | Açıklama |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | Ayarlanmalıdır herhangi bir sorgu için true gerektirir arasında birden fazla bölüm yürütülecek. Geliştirme zamanı boyunca bilinçli performans bileşim olmanızı sağlamak için bir açık bayrağı budur. |
| `EnableScanInQuery` | Dizin oluşturma dışında seçtiyseniz true, ancak yine de sorgu bir tarama çalıştırmak istediğiniz ayarlamanız gerekir. Yalnızca geçerli istenen filtre yolu için dizin oluşturma devre dışı bırakılır. | 
| `MaxItemCount` | Sunucuya gidiş dönüş döndürülecek öğe maksimum sayısı. -1 ayarıyla sunucu öğe sayısını yönetme izin verebilirsiniz. Veya yalnızca az sayıda gidiş dönüş başına öğe almak için bu değeri düşürebilirsiniz. 
| `MaxBufferedItemCount` | Bu istemci-tarafı seçenek ve çapraz bölüm ORDER BY gerçekleştirirken bellek tüketimini sınırlamak için kullanılır. Daha yüksek bir değer çapraz bölüm sıralama gecikme süresi azaltılmasına yardımcı olur. |
| `MaxDegreeOfParallelism` | Alır veya Azure Cosmos DB veritabanı hizmeti paralel sorgu yürütme sırasında istemci tarafı çalıştırmak eşzamanlı işlem sayısını ayarlar. Pozitif özellik değerini ayarla değerine eşzamanlı işlem sayısını sınırlar. 0'dan düşük bir değere ayarlanırsa, sistem otomatik olarak çalıştırmak için eşzamanlı işlem sayısını karar verir. |
| `PopulateQueryMetrics` | Yükleme zamanı süre istatistiklerin ayrıntılı günlük sorgu yürütme derleme süresi, dizin döngü süresi ve belge gibi çeşitli aşamaları harcadığı etkinleştirir. Sorgu istatistiklerini çıktısı sorgu performans sorunları tanılamak için Azure desteği ile paylaşabilirsiniz. |
| `RequestContinuation` | Herhangi bir sorgu tarafından döndürülen donuk devamlılık belirteci geçirerek sorgu yürütme devam edebilirsiniz. Devamlılık belirteci sorgu yürütme için gerekli tüm durum yalıtır. |
| `ResponseContinuationTokenLimitInKb` | Sunucu tarafından döndürülen devam belirtecini en büyük boyutunu sınırlandırabilirsiniz. Uygulama ana bilgisayarı yanıt üstbilgi boyutu sınırları varsa bu ayarlamanız gerekebilir. Bu ayar, toplam süre ve sorgu için kullanılan RUs artırabilir.  |

Örneğin, örnek bir sorgu ile bir koleksiyon üzerinde istenen bölüm anahtarı atalım `/city` bölüm anahtarı ve sn'ye 100.000 RU/s ile sağlanan olarak. Bu sorgu kullanarak isteği `CreateDocumentQuery<T>` aşağıdaki gibi .NET içinde:

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

Yukarıda gösterilen SDK kod parçacığını aşağıdaki REST API isteğine karşılık gelir:

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

Her sorgu yürütme sayfası bir REST API için karşılık gelen `POST` ile `Accept: application/query+json` üstbilgi ve gövde SQL sorgusu. Her sorgu bir yapar ya da daha fazla ile sunucuya yuvarlak `x-ms-continuation` belirteci echoed yürütme devam etmek için istemci ile sunucu arasında. FeedOptions yapılandırma seçeneklerinde istek üstbilgileri biçiminde sunucuya geçirilir. Örneğin, `MaxItemCount` karşılık gelen `x-ms-max-item-count`. 

İstek aşağıdaki (okunabilirlik için kesilmiş) yanıtı döndürür:

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

Sorgudan döndürülen anahtar yanıt üstbilgilerini aşağıdakileri içerir:

| Seçenek | Açıklama |
| ------ | ----------- |
| `x-ms-item-count` | Yanıtta döndürülen öğe sayısı. Bu sağlanan üzerinde bağımlı `x-ms-max-item-count`, en büyük yanıt yükü boyutu, sağlanan işleme ve sorgu yürütme süresi içinde sığabilecek öğe sayısı. |  
| `x-ms-continuation:` | Ek sonuçlar mevcutsa, sorgunun yürütülmesi, devam etmek için devamlılık belirteci. | 
| `x-ms-documentdb-query-metrics` | Sorgu istatistiklerini yürütme. Bu, sorgu yürütme çeşitli aşamalarında harcanan zamanın istatistikleri içeren sınırlandırılmış bir dizedir. Döndürülen IF `x-ms-documentdb-populatequerymetrics` ayarlanır `True`. | 
| `x-ms-request-charge` | Sayısı [istek birimleri](request-units.md) sorgu tarafından tüketilen. | 

REST API isteği üstbilgileri ve seçenekleri hakkında daha fazla bilgi için bkz: [REST API kullanarak kaynak sorgulaması](https://docs.microsoft.com/rest/api/cosmos-db/querying-cosmosdb-resources-using-the-rest-api).

## <a name="best-practices-for-query-performance"></a>Sorgu performansı için en iyi yöntemler
Aşağıdaki Azure Cosmos DB sorgu performansı etkileyen en yaygın faktörlerdir. Bu makalede bu konuların her derinlere.

| Faktörü | İpucu | 
| ------ | -----| 
| Sağlanan aktarım hızı | Sorgu RU ölçmek ve sorgularınızı gerekli sağlanan işleme sahip olduğundan emin olun. | 
| Bölümleme ve bölüm anahtarları | Sorguları ayrıcalık tanıma ile düşük gecikme süresi filtre yan tümcesi bölüm anahtar değeri. |
| SDK ve sorgu seçenekleri | Doğrudan bağlantı gibi SDK en iyi uygulamaları izleyin ve istemci tarafı sorgu yürütme seçeneklerini ayarlayın. |
| Ağ gecikmesi | Ağ Yükü ölçüm için hesap ve yakın bölgesinden okumak için çok girişli API'leri kullanın. |
| Dizin Oluşturma İlkesi | Gerekli dizin yolları/ilke sorgu için olduğundan emin olun. |
| Sorgu yürütme ölçümleri | Sorgu ve veri şekilleri, olası yeniden yazdırmaya tanımlamak için sorgu yürütme ölçümleri analiz edin.  |

### <a name="provisioned-throughput"></a>Sağlanan aktarım hızı
Cosmos DB'de her istek birimleri (RU) Saniyedeki cinsinden ifade edilen ayrılmış işleme ile veri kapsayıcı oluşturun. 1 KB belgenin okuma 1. RU ve (sorguları da dahil) her işlemi bunun kapsamına bağlı RUs sabit sayıda normalleştirilmiş. Örneğin, 1000 varsa RU/s kapsayıcısı için sağlanan ve gibi bir sorguya sahip `SELECT * FROM c WHERE c.city = 'Seattle'` 5 RUs kullanır ve ardından (1000 RU/s) gerçekleştirebilirsiniz / (5 RU/sorgu) sorgularını saniyede = 200 sorgu/s. 

200'den fazla saniye başına sorgusu gönderirseniz, hız sınırlaması gelen istekleri 200/s yukarıda hizmetini başlatır. Bu durumda otomatik olarak işleme geri Çekilme/yeniden deneme gerçekleştirerek SDK'lar, bu nedenle bu sorgular için daha yüksek gecikme fark edebilirsiniz. Gerekli değere sağlanan işleme artırma sorgu gecikme süresi ve verimlilik artırır. 

İstek birimleri hakkında daha fazla bilgi için bkz: [istek birimleri](request-units.md).

### <a name="partitioning-and-partition-keys"></a>Bölümleme ve bölüm anahtarları
Azure Cosmos DB ile sorguları aşağıdaki sırayla hızlı/en verimli gelen yavaş/daha az verimli için genellikle gerçekleştirin. 

* Bir tek bölüm anahtarı ve öğe anahtarı alma
* Tek bölüm anahtarı bir filtre yan tümcesi içeren sorgu
* Bir eşitlik veya aralık filtre yan tümcesi herhangi bir özellikte olmadan sorgulama
* Filtre sorgulama

Tüm bölümleri danışmanız gerekir sorguları daha yüksek gecikme gerekir ve daha yüksek RUs kullanmasını sağlayabilirsiniz. Her bölüm karşı tüm özellikleri otomatik dizin oluşturma işlemi olduğundan, sorgu verimli bir şekilde dizinden bu durumda hizmet verilebilir. Bölümler paralellik seçenekleri kullanarak daha hızlı yayılma sorgular yapabilir.

Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB'de bölümleme](partition-data.md).

### <a name="sdk-and-query-options"></a>SDK ve sorgu seçenekleri
Bkz: [performans ipuçları](performance-tips.md) ve [performans testi](performance-testing.md) için en iyi istemci tarafında performans Azure Cosmos DB'den alma. Bu, son SDK'ları kullanarak, platforma özgü yapılandırmaları bağlantıları, atık toplama sıklığını varsayılan sayısı gibi yapılandırma ve doğrudan/TCP gibi basit bağlantı seçenekleri kullanarak içerir. 


#### <a name="max-item-count"></a>En Fazla Öğe Sayısı
Sorgular, değeri için `MaxItemCount` uçtan uca sorgu zamanında önemli bir etkisi olabilir. Her sunucuya gidiş dönüş öğelerin sayısı en fazla döndürülecek `MaxItemCount` (100 öğelerinin varsayılan). Bu daha yüksek bir değere ayarlamak (-1, en fazla ve önerilen), sorgu süresi genel sorgular büyük sonuç kümeleri ile için özellikle istemci ve sunucu arasındaki gidiş dönüş sayısı sınırlayarak geliştirecektir.

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a>Maksimum paralellik derecesi
Sorguları için ayarlama `MaxDegreeOfParallelism` özellikle çapraz bölüm sorgular (olmadan bir filtre bölüm anahtarı değeri) gerçekleştirirseniz, uygulamanız için en iyi yapılandırmaları tanımlamak için. `MaxDegreeOfParallelism`  en fazla sayısını Paralel Görevler, yani, en fazla paralel olarak ziyaret etme bölüm denetler. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

Varsayalım
* D = (istemci makine işlemcide toplam sayısı =) Paralel Görevler varsayılan maksimum sayısı
* P = Paralel Görevler maksimum sayısı kullanıcı tanımlı
* N sorgu yanıtlama ziyaret gerekiyor bölüm sayısı =

Paralel sorgular P. farklı değerler için nasıl davranacaktır etkileri verilmiştir
* (P == 0) = > seri modu
* (P == 1) = > bir görevin maksimum
* (P > 1) = > Min (P, N) Paralel Görevler 
* (P < 1) = > Min (N, D) Paralel Görevler

SDK sürüm notlarında ve uygulanan sınıflar ve yöntemler ayrıntıları görmek için [SQL SDK'ları](sql-api-sdk-dotnet.md)

### <a name="network-latency"></a>Ağ gecikmesi
Bkz: [Azure Cosmos DB genel dağıtım](tutorial-global-distribution-sql-api.md) genel dağıtımları ayarlar ve en yakın bölgeyi bağlanma hakkında. Birden çok gidiş dönüş yapmak veya sorgudan ayarlamak büyük bir sonuç almak gerektiğinde ağ gecikmesi sorgu performansına önemli bir etkisi vardır. 

Sorgu yürütme ölçümleri bölümüne sorgularda sunucusu yürütme süresi almak açıklanmaktadır ( `totalExecutionTimeInMs`), böylece sorgu yürütme harcanan süre ve ağ yolda harcadığı zamanı arasında ayırt edebilir.

### <a name="indexing-policy"></a>Dizin oluşturma ilkesi
Bkz: [dizin oluşturma ilkesini yapılandırma](indexing-policies.md) yolları, tür ve modları ve bunlar sorgu yürütme nasıl etkiler dizin oluşturma için. Varsayılan olarak, dizin oluşturma ilkesini karma dizeleri için dizin oluşturma kullanır etkin olduğu yönelik eşitlik sorguları, ancak aralığı sorguları/order sorgular tarafından için değil. Aralık sorgu dizeleri için ihtiyacınız varsa, tüm dizeleri aralığı dizin türü belirtilmesi önerilir. 

## <a name="query-execution-metrics"></a>Sorgu yürütme ölçümleri
İsteğe bağlı geçirerek sorgu yürütme üzerinde ayrıntılı ölçümler edinebilirsiniz `x-ms-documentdb-populatequerymetrics` üstbilgi (`FeedOptions.PopulateQueryMetrics` .NET SDK'sındaki). Döndürülen değerin `x-ms-documentdb-query-metrics` Gelişmiş sorgu yürütme işlemi sorun giderme yönelik aşağıdaki anahtar-değer çiftleri sahiptir. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| Ölçüm | Birim | Açıklama | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | milisaniye | Sorgu yürütme süresi | 
| `queryCompileTimeInMs` | milisaniye | Sorgu derleme süresi  | 
| `queryLogicalPlanBuildTimeInMs` | milisaniye | Mantıksal sorgu planı oluşturmak için zaman | 
| `queryPhysicalPlanBuildTimeInMs` | milisaniye | Fiziksel sorgu planı oluşturmak için zaman | 
| `queryOptimizationTimeInMs` | milisaniye | Sorgu en iyi duruma getirme için harcanan süre | 
| `VMExecutionTimeInMs` | milisaniye | Sorgu çalışma zamanında harcanan süre | 
| `indexLookupTimeInMs` | milisaniye | Fiziksel dizini katmanda harcanan süre | 
| `documentLoadTimeInMs` | milisaniye | Belgeleri yüklenirken harcanan süre  | 
| `systemFunctionExecuteTimeInMs` | milisaniye | Yürütülen sistem (yerleşik) işlevleri milisaniye cinsinden toplam süre  | 
| `userFunctionExecuteTimeInMs` | milisaniye | Milisaniye cinsinden yürütme kullanıcı tanımlı işlevler için harcanan toplam süre | 
| `retrievedDocumentCount` | sayı | Toplam alınan belge sayısı  | 
| `retrievedDocumentSize` | bayt | Alınan belgeleri bayt olarak toplam boyutu  | 
| `outputDocumentCount` | sayı | Çıktı belge sayısı | 
| `writeOutputTimeInMs` | milisaniye | Milisaniye cinsinden sorgu yürütme süresi | 
| `indexUtilizationRatio` | oranı (< = 1) | Filtre tarafından yüklenen belgelere sayısıyla eşleşen belge sayısı oranı  | 

İstemci SDK'ları dahili olarak sorgu her bölüm içinde hizmet vermek için birden çok sorgu işlemleri kalmasına neden olabilir. İstemci başına toplam sonuç aşarsanız bölümü birden fazla çağrı yaptığında `x-ms-max-item-count`, sorgu bölüm için sağlanan işleme aşarsa veya sorgu yükü sayfa başına en fazla boyutu ulaşırsa veya sorgu ayrılmış sistem ulaşırsa zaman aşımı sınırı. Her kısmi sorgu yürütme döndüren bir `x-ms-documentdb-query-metrics` bu sayfa için. 

Bazı örnek sorgular şunlardır ve sorgu yürütülmesini ölçümleri bazıları yorumlama döndürdü: 

| Sorgu | Örnek ölçümü | Açıklama | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | Alınan belge sayısı üst yan tümce eşleşecek şekilde 100 + 1'dir. Sorgu zaman harcanan çoğunlukla `WriteOutputTime` ve `DocumentLoadTime` tarama olduğundan. | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | RetrievedDocumentCount (TOP yan tümcesi eşleştirmek için daha yüksek 500 + 1) sunulmuştur. | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | Dizin arama olmasından dolayı hakkında 0,9 ms için anahtar bir arama, IndexLookupTime içinde harcanan `/N/?`. | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | Biraz daha fazla (1,7 ms) için harcanan süre içinde IndexLookupTime aralığı tarama bir dizin arama olmasından dolayı `/N/?`. | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | Aynı anda harcanan `DocumentLoadTime` önceki sorgular, ancak daha düşük `WriteOutputTime` yalnızca tek bir özellik yansıtma olduğundan. | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | Yaklaşık 213 ms, geçen `UserDefinedFunctionExecutionTime` UDF her değeri üzerinde yürütme `c.N`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | Yaklaşık 0,6 ms, geçen `IndexLookupTime` üzerinde `/Name/?`. İçinde sorgu yürütme süresi (~ 7 ms) çoğu `SystemFunctionExecutionTime`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | Sorgu, bir tarama gerçekleştirilir, kullandığından `LOWER`, ve 500 2491 alınan belgeleri dışında döndürülür. |


## <a name="next-steps"></a>Sonraki adımlar
* Desteklenen SQL sorgu işleçleri ve anahtar sözcükler hakkında bilgi edinmek için bkz: [SQL sorgusu](sql-api-sql-query.md). 
* İstek birimleri hakkında bilgi edinmek için [istek birimleri](request-units.md).
* Dizin oluşturma ilkesi hakkında bilgi edinmek için [İlkesi dizin oluşturma](indexing-policies.md) 


