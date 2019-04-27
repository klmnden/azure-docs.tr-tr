---
title: Bir Azure Cosmos DB veri kaynağı - Azure Search dizini
description: Bir Azure Cosmos DB veri kaynağında gezinen ve Azure Search'te tam metin arama yapılabilir bir dizin verileri alma. Dizin oluşturucular veri alımı Azure Cosmos DB gibi seçili veri kaynakları için otomatik hale getirin.
ms.date: 02/28/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 019945c48342238a1caa7611bdff6d06fd1e2bd9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871732"
---
# <a name="how-to-index-cosmos-db-using-an-azure-search-indexer"></a>Azure Search Dizin Oluşturucu kullanarak Cosmos DB dizinleme

Bu makalede bir Azure Cosmos DB yapılandırma işlemi gösterilmektedir [dizin oluşturucu](search-indexer-overview.md) içeriği ayıklama ve Azure Search aranabilir hale getirin. Bu iş akışı, bir Azure Search dizini oluşturur ve Azure Cosmos DB'den ayıklanan mevcut metinle yükler. 

Terimleri kafanızı karıştırabilir olduğundan, hatalarının ayıklanabileceğini belirtmekte yarar [Azure Cosmos DB dizinleme](https://docs.microsoft.com/azure/cosmos-db/index-overview) ve [Azure arama dizini oluşturma](search-what-is-an-index.md) farklı işlemler, her bir hizmete benzersiz. Azure Search başlamadan önce dizin oluşturma, Azure Cosmos DB veritabanınıza gerekir zaten var ve verileri içerir.

Kullanabileceğiniz [portalı](#cosmos-indexer-portal), Cosmos içeriği dizini oluşturmak için REST API'leri veya .NET SDK'sı. Azure Search'te Cosmos DB dizinleyici gezinebileceği [Azure Cosmos öğeleri](https://docs.microsoft.com/azure/cosmos-db/databases-containers-items#azure-cosmos-items) bu protokolleri aracılığıyla erişilebilir:

* [SQL API'Sİ](https://docs.microsoft.com/azure/cosmos-db/sql-api-query-reference) 
* [MongoDB API'si](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction) (genel Önizleme modundadır bu API için Azure arama desteği)  

> [!Note]
> Uservoice ek API desteği için var olan öğeler içeriyor. Cosmos görmek için desteklenen Azure Search'te istediğiniz API'ları için oy çevirebilirsiniz: [Tablo API'si](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab), [Graph API](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4), [Apache Cassandra API'si](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu).
>

<a name="cosmos-indexer-portal"></a>

## <a name="use-the-portal"></a>Portalı kullanma

Azure Cosmos öğeleri dizinini oluşturmak için en kolay yöntem Sihirbazı'nda kullanmaktır [Azure portalında](https://portal.azure.com/). Veri örnekleme ve kapsayıcı meta verilerini okuma [ **verileri içeri aktarma** ](search-import-data-portal.md) Sihirbazı'nda Azure Search varsayılan bir dizin oluşturmak, kaynak alanları hedef dizin alanlarına eşleme ve tek bir dizinde yükleme işlem. Boyutu ve kaynak verilerin karmaşıklığına bağlı olarak birkaç dakika içinde bir işletimsel tam metin arama dizinini olabilir.

Azure Search, hem de Azure Cosmos DB, tercihen aynı bölgede aynı Azure aboneliği kullanmanızı öneririz.

### <a name="1---prepare-source-data"></a>1 - kaynak verileri hazırlama

SQL API veya MongoDB API'si ve bir JSON belge kapsayıcısı için eşlenmiş bir Azure Cosmos veritabanı bir Cosmos hesabı olmalıdır. 

Cosmos DB veritabanınıza veri içerdiğinden emin olun. [Veri Alma Sihirbazı](search-import-data-portal.md) meta verileri okur ve bir dizin şemasını, ancak çıkarsamak için veri örnekleme yükleri veri Cosmos DB'den de gerçekleştirir. Veri yoksa, Sihirbazı ile bu hata durur. "veri kaynağından hata algılama dizin şeması: Veri kaynağı 'emptycollection' hiçbir veri döndürdüğünden bir prototip dizini oluşturulamadı".

### <a name="2---start-import-data-wizard"></a>2 - Veri Alma Sihirbazını Başlat

Yapabilecekleriniz [Sihirbazı](search-import-data-portal.md) Azure arama hizmeti sayfasını veya tıklayarak komut çubuğundan **Azure Search Ekle** içinde **ayarları** bölümü depolama hesabınızın sol Gezinti Bölmesi.

   ![İçeri aktarma Portalı'nda veri komut](./media/search-import-data-portal/import-data-cmd2.png "veri içeri aktarma Sihirbazını Başlat")

### <a name="3---set-the-data-source"></a>3 - veri kaynağı ayarla

> [!NOTE] 
> Şu anda, oluşturma düzenleyin veya silemeyeceğiniz **MongoDB** Azure portalı veya .NET SDK kullanarak veri kaynakları. Ancak, **olabilir** MongoDB dizin oluşturucular portalda yürütme geçmişini izleyin.

İçinde **veri kaynağı** sayfasında, kaynak olmalıdır **Cosmos DB**, aşağıdaki özellikleri ile:

+ **Adı** veri kaynağı nesnesinin adıdır. Oluşturulduktan sonra diğer iş yükleri için seçebilirsiniz.

+ **Cosmos DB hesabı** ile Cosmos DB, birincil veya ikincil bağlantı dizesi olmalıdır bir `AccountEndpoint` ve `AccountKey`. SQL API veya Mongo DB API olarak veri içerip içermeyeceğini hesap belirler.

+ **Veritabanı** hesabından var olan bir veritabanıdır. 

+ **Koleksiyon** belgelerin bir kapsayıcıdır. Belgeleri, sırada içeri aktarma başarılı olması mevcut olması gerekir. 

+ **Sorgu** boş bırakılabilir Aksi takdirde, tüm belgelerin istiyorsanız, bir belge alt kümesini seçen bir sorgu girebilirsiniz. 

   ![Cosmos DB veri kaynağı tanımını](media/search-howto-index-cosmosdb/cosmosdb-datasource.png "Cosmos DB veri kaynağı tanımı")

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - sihirbazında "Bilişsel arama Ekle" sayfasını atlayın

Bilişsel beceriler ekleme belge alma işlemi için gerekli değildir. Belirli bir gerek olmadığı sürece [Bilişsel hizmetler API'leri ve dönüştürmeler dahil](cognitive-search-concept-intro.md) , dizinleme işlem hattına bu adımı atlayın.

İlk adımı atlamak için bir sonraki sayfasına gidin.

   ![Bilişsel arama için sonraki sayfa düğmesi](media/search-get-started-portal/next-button-add-cog-search.png)

Bu sayfadan dizini özelleştirme, İleri atlayabilirsiniz.

   ![Bilişsel beceri adımını atlama](media/search-get-started-portal/skip-cog-skill-step.png)

### <a name="5---set-index-attributes"></a>5 - dizin öznitelikleri Ayarla

İçinde **dizin** sayfasında, bir veri türü ve dizin öznitelikleri ayarlamaya yönelik onay kutularından oluşan bir serinin alanların listesini görmelisiniz. Sihirbaz, meta veriler kaynak veri örnekleme tarafından temel bir alanlar listesi oluşturabilirsiniz. 

Toplu öznitelikler özniteliği sütun üst kısmındaki onay kutusuna tıklayarak seçimi. Seçin **alınabilir** ve **aranabilir** bir istemci uygulaması ve tam metin arama işleme tabi döndürülmesi gereken her alan için. Tamsayı tam metin olmadığını fark edeceksiniz veya belirsiz aranabilir (sayılar verbatim değerlendirilir ve genellikle filtreleri kullanışlıdır).

Açıklamasını inceleyin [dizin öznitelikleridir](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) ve [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) daha fazla bilgi için. 

Seçimlerinizi gözden geçirmek için bir dakikanızı ayırın. Sihirbazı çalıştırdıktan sonra fiziksel veri yapılarını oluşturulur ve bu alanlar, bırakarak ve tüm nesneleri yeniden düzenlemek mümkün olmayacaktır.

   ![Cosmos DB dizin tanımı](media/search-howto-index-cosmosdb/cosmosdb-index-schema.png "Cosmos DB dizin tanımı")

### <a name="6---create-indexer"></a>6 - dizin oluşturucu oluşturma

Tam olarak belirtilen, sihirbaz arama hizmetinizdeki üç farklı bir nesne oluşturur. Bir veri kaynağı nesnesi ve dizin nesnesi, Azure Search hizmetinizde adlandırılmış kaynaklar olarak kaydedilir. Son adım, bir dizin oluşturucu nesnesini oluşturur. Dizin Oluşturucu adlandırma zamanlayabilir ve Yönet Sihirbazı sırayla oluşturulan dizin ve veri kaynağı nesnesi bağımsız olarak tek başına kaynak olarak mevcut izin verir.

Dizin oluşturucular ile aşina değilseniz bir *dizin oluşturucu* aranabilir içeriği için bir dış veri kaynağında gezinir Azure Search'te bir kaynaktır. Çıkışı **verileri içeri aktarma** sihirbazıdır bir dizin oluşturucu, bir Azure Search dizini aktarır, Cosmos DB veri kaynağında gezinir ve aranabilir içeriği ayıklar.

Aşağıdaki ekran görüntüsünde, varsayılan dizin oluşturucu yapılandırmasını gösterir. Geçebilirsiniz **kez** bir kez dizin oluşturucuyu çalıştırmak istiyorsanız. Tıklayın **Gönder** Sihirbazı'nı çalıştırın ve tüm nesneleri oluşturmak için. Dizin oluşturma hemen başlar.

   ![Cosmos DB dizin oluşturucu tanımı](media/search-howto-index-cosmosdb/cosmosdb-indexer.png "Cosmos DB dizin oluşturucu tanımı")

Portal sayfalarında veri içeri aktarma izleyebilirsiniz. İlerleme durumu bildirimlerine, dizin oluşturma durumunu ve kaç belgeler karşıya gösterir. 

Dizin oluşturma tamamlandığında, kullanabileceğiniz [arama Gezgini](search-explorer.md) dizininizi sorgulama için.

> [!NOTE]
> Beklediğiniz verileri görmüyorsanız, daha fazla öznitelik ayarlama hakkında daha fazla alan gerekebilir. Yalnızca oluşturuldu ve 5. adımında dizin öznitelikleri için yaptığınız seçimleri değiştirme Sihirbazı yeniden adım dizin oluşturucu ve dizini silin. 

<a name="cosmosdb-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST API'lerini kullanma

Azure Search'te tüm dizin oluşturucular için üç bölümü iş akışı ortak aşağıdaki REST API ile dizin Azure Cosmos DB verilere kullanabilirsiniz: bir veri kaynağı oluşturun, dizin oluşturma, dizin oluşturucu oluşturma. Dizin Oluşturucu oluşturma isteği gönderdiğinizde veri ayıklama Cosmos depolamadan oluşur. Bu istek tamamlandıktan sonra sorgulanabilir bir dizine sahip. 

MongoDB değerlendiriyorsanız veri kaynağını oluşturmak için REST API'sini kullanmanız gerekir.

Cosmos DB hesabınızdaki tüm belgelerin otomatik olarak dizinini koleksiyonu isteyip istemediğinizi seçebilirsiniz. Varsayılan olarak, tüm belgelerin otomatik olarak dizine alınır, ancak otomatik dizin oluşturma devre dışı kapatabilirsiniz. Dizin oluşturmayı devre dışı bırakıldığında, belgelerin yalnızca aracılığıyla erişilebilen kendi kendine bağlantılar veya belge kullanarak sorgular tarafından kimliği Azure arama, Cosmos DB, Azure Search tarafından dizine koleksiyonda açık dizin otomatik gerektirir. 

> [!NOTE]
> Azure Cosmos DB, documentdb'nin yeni nesil olur. Ürün adı değiştirilmiş olsa da, `documentdb` söz dizimi Azure Search dizin oluşturucularında hala var için geriye dönük uyumluluk Azure arama API'leri ve portal sayfaları. Dizin oluşturucular yapılandırırken belirttiğinizden emin olun `documentdb` bu makalede anlatılan şekilde bir söz dizimi.


### <a name="1---assemble-inputs-for-the-request"></a>1 - giriş istek için bir araya getirin

Her istek için hizmet adını ve yönetici anahtarını Azure arama (POST üstbilgisinde) ve depolama hesabı adı ve blob depolama anahtarı sağlamanız gerekir. Kullanabileceğiniz [Postman](search-fiddler.md) Azure Search için HTTP istekleri göndermek için.

Bir istek yapıştırabilmek aşağıdaki dört değerleri Not Defteri'ne kopyalayın:

+ Azure arama hizmeti adı
+ Azure arama yönetici anahtarı
+ Cosmos DB bağlantı dizesi

Bu değerleri portalda bulabilirsiniz:

1. Azure Search portal sayfalarında arama hizmeti URL'si genel bakış sayfasından kopyalayın.

2. Sol gezinti bölmesinden **anahtarları** ve ardından (bunlar eşdeğerdir) ya da birincil veya ikincil anahtarı kopyalayın.

3. Portal sayfalarına Cosmos depolama hesabınız için geçin. Sol gezinti bölmesindeki altında **ayarları**, tıklayın **anahtarları**. Bu sayfa bir URI bağlantı dizeleri iki kümesi sağlar ve iki anahtarlarını ayarlar. Bağlantı dizelerini birini Not Defteri'ne kopyalayın.

### <a name="2---create-a-data-source"></a>2 - bir veri kaynağı oluşturma

A **veri kaynağı** dizin ve kimlik bilgilerini (örneğin, değiştirilen veya silinen belgeleri koleksiyonunuz içinde) verilerdeki değişiklikleri tanımlamaya yönelik ilkeleri için verileri belirtir. Böylece birden çok dizin oluşturucu tarafından kullanılan veri kaynağı bağımsız bir kaynak olarak tanımlanır.

Bir veri kaynağı oluşturmak için bir POST isteği düzenleme:

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

| Alan   | Açıklama |
|---------|-------------|
| **Adı** | Gereklidir. Veri kaynağı nesnesinin temsil etmek için herhangi bir ad seçin. |
|**type**| Gereklidir. Olmalıdır `documentdb`. |
|**Kimlik bilgileri** | Gereklidir. Bir Cosmos DB bağlantı dizesi olmalıdır.<br/>SQL koleksiyonlar için bağlantı dizesi bu biçimdedir: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`<br/>MongoDB koleksiyonu için ekleyin **api türü MongoDb =** bağlantı dizesi:<br/>`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`<br/>Uç nokta URL'si bağlantı noktası numaralarını kaçının. Bağlantı noktası numarası dahil, Azure Search, Azure Cosmos DB veritabanının dizinini oluşturmak mümkün olmayacaktır.|
| **Kapsayıcı** | Aşağıdaki öğeleri içerir: <br/>**Ad**: Gereklidir. Sıralanacak veritabanı koleksiyonu kimliği belirtin.<br/>**Sorgu**: İsteğe bağlı. Azure Search'ün dizin bir düz şemasına rastgele bir JSON belgesi düzleştirmek için sorgu belirtebilirsiniz.<br/>MongoDB koleksiyonlar, sorgular desteklenmez. |
| **dataChangeDetectionPolicy** | Önerilir. Bkz: [değiştirilen belgeler dizin](#DataChangeDetectionPolicy) bölümü.|
|**dataDeletionDetectionPolicy** | İsteğe bağlı. Bkz: [silinen belgeler dizin](#DataDeletionDetectionPolicy) bölümü.|

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


### <a name="3---create-a-target-search-index"></a>3 - bir hedef arama dizini oluşturma 

[Hedef Azure Search dizini oluşturma](/rest/api/searchservice/create-index) zaten yoksa. Aşağıdaki örnek, bir kimlik ve açıklama alanı ile bir dizin oluşturur:

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

### <a name="4---configure-and-run-the-indexer"></a>4 - yapılandırın ve dizin oluşturucuyu çalıştırma

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

## <a name="use-net"></a>.NET’i kullanma

.NET SDK'sı, tam olarak REST API ile eşlik vardır. Kavramlar ve iş akışı gereksinimlerini öğrenmek için önceki REST API bölümde gözden geçirmenizi öneririz. Yönetilen kodda bir JSON dizin oluşturucu uygulamak için aşağıdaki .NET API başvuru belgelerine başvurabilirsiniz.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

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

## <a name="watch-this-video"></a>Bu videoyu izleyin

Biraz daha eski bu 7 dakikalık videoda, Azure Cosmos DB Program Yöneticisi Manager Andrew Liu nasıl bir Azure Cosmos DB kapsayıcısı için bir Azure Search dizini ekleneceğini gösterir. Portal sayfalarına videoda gösterilen güncel olmayan, ancak bilgilerin hala geçerlidir.

>[!VIDEO https://www.youtube.com/embed/OyoYu1Wzk4w]

## <a name="NextSteps"></a>Sonraki adımlar

Tebrikler! Azure Search Dizin Oluşturucu kullanarak Azure Cosmos DB tümleştirmeyi öğrendiniz.

* Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB hizmet sayfasında](https://azure.microsoft.com/services/cosmos-db/).
* Azure arama hakkında daha fazla bilgi edinmek için [arama hizmeti sayfasını](https://azure.microsoft.com/services/search/).
