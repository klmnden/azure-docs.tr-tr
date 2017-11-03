---
title: "Dizin oluşturucular kullanarak Azure Search'te Azure SQL veritabanına bağlanma | Microsoft Docs"
description: "Dizin oluşturucular kullanarak Azure Search dizini için Azure SQL veritabanından veri çekme öğrenin."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: e9bbf352-dfff-4872-9b17-b1351aae519f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/13/2017
ms.author: eugenesh
ms.openlocfilehash: 49f614fdf3ba84de238139387ea97ee62077b072
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Dizin oluşturucuları kullanarak Azure Search'te Azure SQL veritabanına bağlanma

Sorgulayabileceğiniz önce bir [Azure Search dizini](search-what-is-an-index.md), verilerinizle doldurmanız gerekir. Verileri bir Azure SQL veritabanında yaşıyorsa bir **Azure SQL veritabanı için Azure Search dizin oluşturucusunu** (veya **Azure SQL dizin oluşturucu** kısaca) yazmak için kodu daha az ve daha az anlamına gelir dizin oluşturma işlemi otomatik hale getirebilirsiniz önem verdiğiniz altyapısı.

Bu makalede kullanmanın mekanizması kapsayan [dizin oluşturucular](search-indexer-overview.md), ancak aynı zamanda yalnızca Azure SQL veritabanları (örneğin, tümleşik değişiklik izleme) ile kullanılabilen özellikleri açıklar. 

Azure SQL veritabanları ek olarak, Azure Search'te dizin oluşturucular için sağlar [Azure Cosmos DB](search-howto-index-documentdb.md), [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md), ve [Azure tablo depolaması](search-howto-indexing-azure-tables.md). Diğer veri kaynakları için destek istemek için geri bildirim sağlayan [Azure Search geri bildirim Forumunda](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Dizin oluşturucular ve veri kaynakları

A **veri kaynağı** hangi verileri dizini, veri erişimi ve verimli bir şekilde (yeni, değiştirilen veya silinen satır) verilerde yapılan değişiklikler tanımlayan ilkeler için kimlik bilgilerini belirtir. Böylece birden çok dizin oluşturucu tarafından kullanılan bağımsız bir kaynak olarak tanımlanır.

Bir **dizin oluşturucu** hedeflenen arama dizini ile tek bir veri kaynağına bağlanan bir kaynaktır. Bir dizin oluşturucu aşağıdaki yollarla kullanılır:

* Bir dizin doldurmak için veri tek seferlik bir kopyasını gerçekleştirin.
* Bir zamanlamada veri kaynağındaki değişikliklerle dizin güncelleştirin.
* Bir dizini gerektiği şekilde güncelleştirmek için isteğe bağlı çalıştırın.

Tek bir dizin oluşturucu yalnızca bir tablo veya Görünüm kullanabilir, ancak birden çok arama dizinlerini doldurmak istiyorsanız birden çok dizin oluşturucu oluşturabilirsiniz. Kavramları hakkında daha fazla bilgi için bkz: [dizin oluşturucu işlemleri: normal iş akışı](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

Ayarlama ve bir Azure SQL Dizin Oluşturucu kullanarak yapılandırma:

* İçeri aktarma veri Sihirbazı'nda [Azure portalı](https://portal.azure.com)
* Azure arama [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Azure arama [REST API'si](https://docs.microsoft.com/en-us/rest/api/searchservice/indexer-operations)

Bu makalede, REST API oluşturmak için kullanacağız **dizin oluşturucular** ve **veri kaynakları**.

## <a name="when-to-use-azure-sql-indexer"></a>Azure SQL dizin oluşturucusunu kullanma zamanı
Verilerinizde ilgili çeşitli etkenlere bağlı olarak Azure SQL dizin oluşturucu kullanımını olabilir veya uygun olmayabilir. Verilerinizi aşağıdaki gereksinimleri uyuyorsa, Azure SQL dizin oluşturucu kullanabilirsiniz.

| Ölçütler | Ayrıntılar |
|----------|---------|
| Tek bir tablo veya Görünüm veri kaynağı | Veriler arasında birden çok tablo dağılmış, verileri tek bir görünüm oluşturabilirsiniz. Bir görünümü kullanıyorsanız, dizin artımlı değişiklikler ile yenilemek için SQL Server'ın tümleşik değişiklik algılama kullanmanız mümkün olmayacaktır. Daha fazla bilgi için bkz: [yakalama değiştirilen ve silinen satır](#CaptureChangedRows) aşağıda. |
| Veri türlerinin uyumlu olduğundan | Bir Azure Search dizini çoğu ancak tüm SQL türleri desteklenir. Bir listesi için bkz: [veri türlerini eşleştirme](#TypeMapping). |
| Gerçek zamanlı veri eşitleme gerekli değil | Bir dizin oluşturucu tablonuz en fazla beş dakikada yeniden dizin oluşturabilirsiniz. Veri değişikliklerini sık sık ve değişiklikleri dizinde saniye ya da tek dakikalar içinde yansıtılması gerekiyorsa kullanmanızı öneririz [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) veya [.NET SDK'sı](search-import-data-dotnet.md) güncelleştirilmiş satırları doğrudan göndermek için. |
| Artımlı dizin mümkündür | Büyük veri kümesi ve bir zamanlamaya göre dizin oluşturucuyu çalıştırmak için plan varsa, Azure Search verimli bir şekilde yeni, değiştirilen veya silinen satır belirlemek mümkün olması gerekir. İsteğe bağlı (zamanlamada değil) dizin oluşturma veya 100. 000'den az satır dizinini olmayan Artımlı dizin oluşturma işlemi yalnızca izin verilir. Daha fazla bilgi için bkz: [yakalama değiştirilen ve silinen satır](#CaptureChangedRows) aşağıda. |

## <a name="create-an-azure-sql-indexer"></a>Bir Azure SQL dizin oluşturucu yapın

1. Veri kaynağı oluşturun:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }
   ```

   Bağlantı dizesinden alabilirsiniz [Azure portal](https://portal.azure.com); kullanın `ADO.NET connection string` seçeneği.

2. Zaten yoksa, hedef Azure Search dizini oluşturma. Kullanarak bir dizinin oluşturabilirsiniz [portal](https://portal.azure.com) veya [oluşturma dizini API](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Hedef dizin şeması kaynak tablosu şeması ile uyumlu olduğundan emin olun - bkz [veri türleri SQL ve Azure search arasında eşleme](#TypeMapping).

3. Dizin Oluşturucu, bir ad verip ve veri kaynak ve hedef dizini başvuran oluşturun:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Bu yolla oluşturulan bir dizin oluşturucu bir zamanlama yok. Ne zaman oluşturulduktan sonra otomatik olarak çalıştırılır. Yeniden miktar adresindeki kullanarak istediğiniz zaman çalıştırabilirsiniz bir **dizin** isteği:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2016-09-01
    api-key: admin-key

Dizin Oluşturucu davranışı, toplu iş boyutu ve bir dizinleyici yürütme başarısız olmadan önce kaç tane belgeleri atlanabilir gibi çeşitli yönlerini özelleştirebilirsiniz. Daha fazla bilgi için bkz: [dizin oluşturucu API oluşturma](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Azure hizmetlerinin veritabanınıza bağlanmasına izin ver gerekebilir. Bkz: [Azure bağlanma](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) Bunu yapmak yönergeler için.

Dizin Oluşturucu durumu ve yürütme geçmişi (dizine öğeleri, hataları, vb. sayısı) izlemek için kullanın bir **dizin oluşturucu durumu** isteği:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2016-09-01
    api-key: admin-key

Yanıtı şuna benzer görünmelidir:

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Yürütme geçmişi (son yürütme yanıtta önce geliyorsa) böylece ters kronolojik sırada sıralanır en son tamamlanan yürütmeleri en çok 50 içerir.
Yanıt hakkında ek bilgiler bulunabilir [dizin oluşturucu durumunu Al](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Dizin oluşturucular bir zamanlamaya göre Çalıştır
Düzenli bir zamanlamaya göre çalıştırmak için dizin oluşturucu de düzenleyebilirsiniz. Bunu yapmak için ekleyin **zamanlama** oluştururken veya güncelleştirirken dizin oluşturucu özelliği. Aşağıdaki örnek, dizin oluşturucusunu güncelleştirmek için bir PUT İsteği gösterir:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Aralığı** parametresi gereklidir. Aralık iki ardışık dizin oluşturucu yürütmeleri başlangıcı arasındaki süreyi ifade eder. İzin verilen en küçük aralık 5 dakikadır; en uzun bir gündür. Bir XSD "dayTimeDuration" değeri biçimlendirilmelidir (sınırlı alt kümesi bir [ISO 8601 süre](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değeri). Bu desen: `P(nD)(T(nH)(nM))`. Örnekler: `PT15M` her 15 dakikada için `PT2H` için her 2 saatte bir.

İsteğe bağlı **startTime** zamanlanmış yürütmeleri zaman başlamak gösterir. Belirtilmezse, geçerli UTC saati kullanılır. Bu süre geçmişte – dizin oluşturucu startTime sürekli olarak çalışan sanki durumda ilk yürütme zamanlandı olabilir.  

Bir dizin oluşturucu yalnızca bir yürütme aynı anda çalıştırabilirsiniz. Bir dizin oluşturucu yürütülmesinin zamanlandığı çalışıyorsa, yürütme sonraki zamanlanan saatine kadar ertelenir.

Şimdi bu daha somut yapmak için bir örneği göz önünde bulundurun. Varsayalım biz yapılandırılmış aşağıdaki saatlik zamanlaması:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Şunlar olur:

1. İlk dizin oluşturucusu yürütme sırasında veya 1 Mart 2015 12: 00'da çevresinde başlatır UTC.
2. Bu yürütme 20 dakika (veya her zaman değerinden 1 saat) alır varsayalım.
3. İkinci yürütme sırasında veya 1 Mart 2015 1: 00'da çevresinde başlatır
4. Şimdi yaklaşık 2: 10'da tamamlandıktan böylece bu yürütme bir saatten – Örneğin, 70 dakika – aldığını varsayalım.
5. Saat 2:00 ' da artık başlatmak üçüncü yürütme için olur. Ancak, çünkü 1 da ikinci yürütülmesinden hala, çalışıyor üçüncü yürütme atlanır. 3 sabah üçüncü yürütülmesine başlar

Eklemek, değiştirmek veya varolan bir dizin oluşturucu için bir zamanlama kullanarak silme bir **PUT dizin oluşturucu** isteği.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Yeni, değiştirilen ve silinen satır yakalama

Azure Search kullanan **Artımlı dizin** tüm tabloyu yeniden dizin veya bir dizin oluşturucu her çalıştığında görüntülemek zorunda kalmamak için. Azure arama, iki Artımlı dizin desteklemek için algılama ilkeleri değiştirme sağlar. 

### <a name="sql-integrated-change-tracking-policy"></a>SQL ile tümleştirilen değişiklik izleme ilkesinin
SQL veritabanınız destekliyorsa [değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), kullanmanızı öneririz **SQL tümleşik değişiklik izleme İlkesi**. Bu en verimli bir ilkedir. Ayrıca, bir açık "geçici silme" sütunu tablonuza eklemek zorunda kalmadan silinen satır tanımlamak Azure Search sağlar.

#### <a name="requirements"></a>Gereksinimler 

+ Veritabanı sürüm gereksinimleri:
  * SQL Server 2012 SP3 ve daha sonra Azure Vm'lerinde SQL Server kullanıyorsanız.
  * Azure SQL veritabanı kullanıyorsanız, Azure SQL Database V12.
+ Tabloları yalnızca (Görünüm). 
+ Veritabanı üzerindeki [etkinleştir değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) tablo. 
+ Birleşik birincil anahtar (bir birincil anahtar içeren birden fazla sütun) tablo üzerinde.  

#### <a name="usage"></a>Kullanım

Bu ilkeyi kullanmak için oluştur veya veri kaynağınızı şöyle güncelleştir:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Silinen satır zaman SQL tümleşik değişiklik izleme ilkesinin, kullanarak belirtme ayrı veri silme algılama İlkesi - bu ilkeyi tanımlamak için yerleşik destek yok. Ancak, silmeleri algılanan "automagically" olarak arama dizininize belge anahtarında SQL tablosundaki birincil anahtar ile aynı olmalıdır. 

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Yüksek su işareti değişiklik algılama İlkesi

Bu değişiklik algılama İlkesi sürümü veya bir satır en son güncelleştirildiği zaman yakalama bir "yüksek su işareti" sütun kullanır. Bir görünüm kullanıyorsanız, yüksek su işareti İlkesi kullanmanız gerekir. Yüksek su işareti sütun aşağıdaki gereksinimleri karşılaması gerekir.

#### <a name="requirements"></a>Gereksinimler 

* Tüm ekler sütunu için bir değer belirtin.
* Bir öğe için tüm güncelleştirmeleri de sütunun değeri değiştirin.
* Bu sütunun değeri her INSERT veya update artırır.
* Aşağıdaki sorguları yeri ve ORDER BY yan tümceleri yürütülebilir verimli bir şekilde:`WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Kullanmanızı öneririz [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) yüksek su işareti sütun için veri türü. Başka bir veri türü kullanılırsa, değişiklik izleme eşzamanlı olarak bir dizin oluşturucu sorgusu yürütme işlemleri varlığında tüm değişiklikleri yakalamak için kesin değildir. Kullanırken **rowversion** salt okunur çoğaltmaları bir yapılandırmada birincil kopyada dizin oluşturucu işaret etmelidir. Yalnızca birincil kopya veri eşitleme senaryoları için kullanılabilir.

#### <a name="usage"></a>Kullanım

Bir yüksek su işareti İlkesi kullanmak, oluşturmak veya bu gibi veri kaynağınız güncelleştirmek için:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a rowversion or last_updated column name]"
      }
    }

> [!WARNING]
> Kaynak tablosu yüksek su işareti sütunu dizin yoksa, SQL Oluşturucu tarafından kullanılan sorguları zaman aşımı olabilir. Özellikle, `ORDER BY [High Water Mark Column]` yan tümcesi tablo satır sayısını içerdiğinde verimli bir şekilde çalıştırmak için bir dizin gerektirir.
>
>

Zaman aşımı hatalarla karşılaşırsanız, kullanabileceğiniz `queryTimeout` sorgu zaman aşımı varsayılan 5 dakikalık zaman aşımından daha yüksek bir değere ayarlamak için dizin oluşturucu yapılandırma ayarı. Örneğin, zaman aşımı süresi 10 dakika olarak ayarlamak için oluşturmak veya dizin oluşturucu aşağıdaki yapılandırma ile güncelleştir:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Ayrıca devre dışı bırakabilirsiniz `ORDER BY [High Water Mark Column]` yan tümcesi. Ancak, dizin oluşturucusu yürütme bir hata nedeniyle kesintiye uğrarsa, daha sonra - çalıştırıyorsa, dizin oluşturucu zaten neredeyse tüm satırları kesintiye uğradı tarafından işlendiği olsa bile tüm satırları yeniden işlemek dizin oluşturucu olduğundan bu önerilmez. Devre dışı bırakmak için `ORDER BY` yan tümcesi, kullanım `disableOrderByHighWaterMarkColumn` dizin oluşturucu tanımında ayarı:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Yazılım silme sütun silme algılama İlkesi
Kaynak tablosundan silinen satır, büyük olasılıkla bu satır arama dizini de silmek istiyor. SQL tümleşik değişiklik izleme ilkesinin kullanırsanız, bu sizin için dikkate. Ancak, yüksek su işareti değişiklik izleme ilkesinin silinen satırlarla yardımcı değil. Ne yapmanız gerekir?

Satırları tablodan fiziksel olarak kaldırılırsa, Azure Search artık mevcut kayıtları varlığını anlamak için hiçbir yolu yoktur.  Ancak, mantıksal olarak tablosundan kaldırmadan satırları silmek için "soft-Sil" tekniği kullanabilirsiniz. Bir sütun bu sütun kullanarak silinmiş olarak tablo veya Görünüm ve işareti satırları ekleyin.

Soft-delete teknik kullanırken, geçici silme ilkesi şu şekilde oluşturulurken veya veri kaynağı güncelleştirilirken belirtebilirsiniz:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]"
        }
    }

**SoftDeleteMarkerValue** bir dize olması gerekir – gerçek değer dize gösterimini kullanın. Örneğin, burada silinen satır işaretlenir 1 değeri bir tamsayı sütunu varsa, kullanmak `"1"`. Burada silinen satır işaretlenir Boolean true değeri ile bir BIT sütun varsa, `"True"`.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>SQL ve Azure Search veri türleri arasında eşleme
| SQL veri türü | Hedef dizini izin alan türleri | Notlar |
| --- | --- | --- |
| bit |Edm.Boolean, Edm.String | |
| int, smallint, tinyint |EDM.Int32, EDM.Int64, Edm.String | |
| bigint |EDM.Int64, Edm.String | |
| Gerçek, kayan nokta |Edm.Double, Edm.String | |
| küçük para, para ondalık sayısal |Edm.String |Azure arama Bu duyarlık kaybeder çünkü Edm.Double ondalık türlerini dönüştürmeyi desteklemez |
| karakter, n karakter, değişken karakter, n değişken karakter |Edm.String<br/>Collection(Edm.String) |Bir SQL dizesi dize bir JSON dizisini temsil ediyorsa Collection(Edm.String) alanını doldurmak için kullanılabilir:`["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, date, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| coğrafi konum |Edm.GeographyPoint |Yalnızca Coğrafya türünün örneklerini noktası (varsayılan değer olan) SRID 4326 ile desteklenir |
| rowVersion |Yok |Satır sürümü sütunları arama dizinine depolanamaz ancak değişiklik izleme için kullanılabilir |
| zaman, timespan, ikili, değişken ikili, resim, xml, geometri, CLR Türleri |Yok |Desteklenmiyor |

## <a name="configuration-settings"></a>Yapılandırma ayarları
SQL dizin oluşturucu birkaç yapılandırma ayarlarını gösterir:

| Ayar | Veri türü | Amaç | Varsayılan değer |
| --- | --- | --- | --- |
| queryTimeout |Dize |SQL sorgusu yürütme için zaman aşımı ayarlar |5 dakika ("00: 05:00") |
| disableOrderByHighWaterMarkColumn |bool |ORDER BY yan tümcesi atlamak için yüksek su işareti ilke tarafından kullanılan SQL sorgusu neden olur. Bkz: [yüksek su işareti İlkesi](#HighWaterMarkPolicy) |False |

Bu ayarları kullanılan `parameters.configuration` dizin oluşturucu tanımında nesne. Örneğin, sorgu zaman aşımı 10 dakika olarak ayarlamak için oluşturmak veya dizin oluşturucu aşağıdaki yapılandırma ile güncelleştir:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>SSS

**S: Azure SQL dizin oluşturucu Iaas VM'ler için Azure'da çalışan SQL veritabanları ile kullanabilirim?**

Evet. Ancak, veritabanınıza bağlanmak için arama hizmetinize izin vermeniz gerekiyor. Daha fazla bilgi için bkz: [bir Azure VM üzerinde SQL Server bir Azure Search dizin oluşturucu arasında bağlantı yapılandırma](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**S: şirket içi çalışan SQL veritabanları ile Azure SQL dizin oluşturucu kullanabilir miyim?**

Doğrudan yönetilemez. Biz önermez ve bunun nedenle veritabanlarınızı Internet trafiği açmanızı gerektirecek şekilde doğrudan bir bağlantı desteklemez. Müşteriler, Azure Data Factory gibi köprüsü teknolojilerini kullanarak bu senaryo ile başarılı olduğunu. Daha fazla bilgi için bkz: [veri Azure Data Factory kullanarak Azure Search dizini için anında](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**S: Azure SQL dizin oluşturucu Iaas Azure üzerinde çalışan SQL Server dışındaki veritabanlarıyla kullanabilirim?**

Hayır. SQL Server dışındaki tüm veritabanları ile dizinleyici test henüz çünkü bu senaryo, desteklemiyoruz.  

**S: bir zamanlamaya göre çalışan birden çok dizin oluşturucu oluşturma?**

Evet. Ancak, yalnızca bir dizin oluşturucu bir düğümde aynı anda çalışıyor olabilir. Eşzamanlı olarak çalışan birden çok dizin oluşturucu ihtiyacınız varsa, birden fazla arama birimi, arama hizmetinize ölçeklendirme göz önünde bulundurun.

**S: bir dizin oluşturucu etkileyen my sorgu iş yükü çalıştıran mu?**

Evet. Dizin Oluşturucu, arama hizmetinize düğümlerin birinde çalışır ve bu düğümün kaynakları dizin oluşturma ve sorgu trafiğinin ve diğer API isteklerine hizmet veren arasında paylaşılır. Dizin oluşturma ve sorgu yoğun iş yükleri çalıştırın ve 503 hatalarıyla veya artan yanıt süreleri yüksek oranda karşılaşırsanız göz önünde bulundurun [arama hizmetinizi ölçeklendirme](search-capacity-planning.md).

**S: bir ikincil çoğaltma kullanabilirim bir [yük devretme kümesi](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) veri kaynağı olarak?**

Duruma göre değişir. Tam bir tablo veya görünüm için dizin, bir ikincil çoğaltma kullanabilirsiniz. 

Artımlı dizin oluşturma için Azure Search desteklediği iki algılama ilkeleri değiştirme: tümleşik SQL izleme ve yüksek su işareti değiştirin.

Salt okunur çoğaltmalar üzerinde tümleşik değişiklik izleme SQL veritabanı desteklemez. Bu nedenle, yüksek su işareti İlkesi kullanmanız gerekir. 

Standart Bizim önerimiz rowversion veri türü için yüksek su işareti sütun kullanmaktır. Ancak, rowversion kullanarak SQL veritabanının üzerinde dayanır `MIN_ACTIVE_ROWVERSION` salt okunur çoğaltmaları desteklenmeyen işlev. Bu nedenle, rowversion kullanıyorsanız, bir birincil çoğaltmaya dizin oluşturucu işaret etmelidir.

Bir salt okunur kopyada rowversion kullanmayı denerseniz, aşağıdaki hatayı görürsünüz: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update the datasource and specify a connection to the primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**S: yüksek su işareti değişiklik izleme için bir alternatif, rowversion olmayan sütun kullanabilir miyim?**

Bu önerilmez. Yalnızca **rowversion** için güvenilir veri eşitlenmesine izin verir. Ancak, uygulama mantığınızın bağlı olarak, güvenli olabilir varsa:

+ Dizin Oluşturucu çalıştırıldığında, bekleyen işlem yoktur dizine tablosundaki emin olun (örneğin, tüm tablo güncelleştirmeleri toplu iş olarak bir zamanlamaya göre gerçekleşir ve Azure Search dizin oluşturucu zamanlaması tablo güncelleştirme ile çakışan önlemek için ayarlanır Zamanlama).  

+ Tüm eksik satırları seçmek için tam bir arat düzenli aralıklarla bunu. 
