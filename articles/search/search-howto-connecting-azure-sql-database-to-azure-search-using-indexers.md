---
title: Bağlanmak ve Azure SQL veritabanı dizin oluşturucuları - Azure Search kullanarak içerik dizini
description: Azure Search'te tam metin arama için dizin oluşturucuları kullanarak Azure SQL veritabanı'nda veri gezinme hakkında bilgi edinin. Bu makale, bağlantılar, dizin oluşturucu yapılandırmasını ve veri alımı kapsar.
ms.date: 05/02/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: c23933e7f379a438d436fd99c5fea7899c5891ef
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025350"
---
# <a name="connect-to-and-index-azure-sql-database-content-using-azure-search-indexers"></a>Bağlanmak ve Azure SQL veritabanı kullanarak Azure Search dizin oluşturucularında içerik dizini

Sorgu önce bir [Azure Search dizini](search-what-is-an-index.md), kendi verilerinizle doldurabileceğiniz gerekir. Verileri bir Azure SQL veritabanı'nda yaşıyorsa bir **Azure SQL veritabanı için Azure Search dizin oluşturucu** (veya **Azure SQL dizin oluşturucu** kısaca) kod yazmak için daha az ve daha az anlamına gelir dizin oluşturma işlemi, otomatik hale getirebilirsiniz önem verdiğiniz için altyapı.

Mekanizması kullanarak bu makalede ele alınmaktadır [dizin oluşturucular](search-indexer-overview.md), ancak aynı zamanda yalnızca Azure SQL veritabanları (örneğin, tümleşik değişiklik izleme) ile kullanılabilen özellikleri açıklar. 

Ek olarak Azure SQL veritabanları, Azure Search dizin oluşturucular için sağlar. [Azure Cosmos DB](search-howto-index-cosmosdb.md), [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md), ve [Azure tablo depolaması](search-howto-indexing-azure-tables.md). Diğer veri kaynakları için destek isteme hakkında geri bildirim sağlamanız için [Azure Search geri bildirim Forumu](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Dizin oluşturucular ve veri kaynakları

A **veri kaynağı** hangi veri dizini, veri erişimi ve verimli bir şekilde (yeni, değiştirilen veya Silinen satırlar) verilerdeki değişiklikleri tanımlayan ilkeler için kimlik bilgilerini belirtir. Birden çok dizin oluşturucular tarafından kullanılabilir, böylece bunu bağımsız bir kaynak olarak tanımlanır.

Bir **dizin oluşturucu** tek bir veri kaynağını hedef arama dizini ile bağlanan bir kaynaktır. Bir dizin oluşturucu, aşağıdaki şekillerde kullanılır:

* Veri dizini doldurmak için tek seferlik bir kopyasını gerçekleştirin.
* Dizin, veri kaynağındaki bir zamanlamaya göre değişikliklerle güncelleştirin.
* Bir dizini gerektiği şekilde güncelleştirmek için isteğe bağlı olarak çalıştırın.

Tek bir dizin oluşturucu, yalnızca bir tablo veya Görünüm kullanabilir, ancak birden çok arama dizinleri doldurmak istiyorsanız, birden çok dizin oluşturucu oluşturabilirsiniz. Kavramları hakkında daha fazla bilgi için bkz. [dizinleyici işlemleri: Tipik iş akışı](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

Ayarlayabilir ve bir Azure SQL Dizin Oluşturucu kullanarak yapılandırın:

* Veri Alma Sihirbazı içinde [Azure portalı](https://portal.azure.com)
* Azure Search'ü [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Azure Search'ü [REST API](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)

Bu makalede, REST API oluşturmak için kullanacağız **dizin oluşturucular** ve **veri kaynakları**.

## <a name="when-to-use-azure-sql-indexer"></a>Azure SQL dizin oluşturucu kullanma zamanı
Verilerinize ilişkin çeşitli etkenlere bağlı olarak Azure SQL dizin oluşturucu kullanımını olabilir veya uygun olmayabilir. Verilerinizi aşağıdaki gereksinimlerine en uygun Azure SQL dizin oluşturucu kullanabilirsiniz.

| Ölçütler | Ayrıntılar |
|----------|---------|
| Veri kaynağı bir tek bir tablo veya Görünüm | Veriler, birden çok tabloda dağılmış, verilerin tek bir görünümünü oluşturabilirsiniz. Bir görünüm kullanırsanız, dizin artımlı değişiklikler ile yenilemek için SQL Server'ın tümleşik değişiklik algılama kullanmanız mümkün olmayacaktır. Daha fazla bilgi için [yakalama değiştirilen ve silinen satırları](#CaptureChangedRows) aşağıda. |
| Uyumlu veri türleri | Çoğu SQL türleri, Azure Search dizini içinde desteklenir. Bir liste için bkz. [veri türlerini eşleme](#TypeMapping). |
| Gerçek zamanlı veri eşitleme, gerekli değildir. | Bir dizin oluşturucu en fazla beş dakikada tablonuzu yeniden. Verileriniz sık sık değişiyorsa ve değişiklikleri dizinde saniye ya da tek dakikalar içinde yansıtılması gerekiyorsa kullanmanızı öneririz [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) veya [.NET SDK'sı](search-import-data-dotnet.md) güncelleştirilen satırların doğrudan göndermek için. |
| Artımlı dizin mümkündür | Büyük veri kümesi ve dizin oluşturucu bir zamanlamaya göre çalıştırılacak planı varsa, Azure Search verimli bir şekilde yeni, değiştirilen veya silinen satırları belirlemek mümkün olması gerekir. İsteğe bağlı olarak (zamanlamaya göre değil) dizini oluşturma veya 100. 000'den daha az satır dizinini artımlı olmayan dizin yalnızca izin verilir. Daha fazla bilgi için [yakalama değiştirilen ve silinen satırları](#CaptureChangedRows) aşağıda. |

> [!NOTE] 
> Azure arama, yalnızca SQL Server kimlik doğrulamasını destekler. Azure Active Directory parola kimlik doğrulaması için destek gerekiyorsa, lütfen bu için oy [UserVoice öneri](https://feedback.azure.com/forums/263029-azure-search/suggestions/33595465-support-azure-active-directory-password-authentica).

## <a name="create-an-azure-sql-indexer"></a>Azure SQL dizin oluşturucu oluşturma

1. Veri kaynağı oluşturun:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }
   ```

   Bağlantı dizesinden alabilirsiniz [Azure portalında](https://portal.azure.com); kullanma `ADO.NET connection string` seçeneği.

2. Zaten yoksa, hedef Azure Search dizini oluşturma. Kullanarak bir dizin oluşturun [portalı](https://portal.azure.com) veya [oluşturma dizini API](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Hedef dizin şemasını kaynak tablonun şeması ile uyumlu olduğundan emin olun - bkz [SQL ve Azure Search'ü arasında veri türlerini eşleme](#TypeMapping).

3. Dizin Oluşturucu, bir ad verin ve başvuru verileri kaynak ve hedef dizin oluşturma:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Bu şekilde oluşturulan bir dizin oluşturucu, bir zamanlamaya sahip değil. Ne zaman oluşturulduktan sonra otomatik olarak çalıştırılır. Yeniden kullanarak istediğiniz zaman çalıştırabilirsiniz bir **dizin oluşturucuyu çalıştırma** isteği:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2019-05-06
    api-key: admin-key

Dizin Oluşturucu davranışı, toplu iş boyutu ve bir dizin oluşturucu yürütme başarısız olmadan önce kaç belgeleri atlanabilir gibi çeşitli yönlerini özelleştirebilirsiniz. Daha fazla bilgi için [dizin oluşturucu API oluşturma](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Veritabanınıza bağlanmak Azure hizmetlerinin izin gerekebilir. Bkz: [Azure bağlanma](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) bunu nasıl yapacağınız hakkında yönergeler için.

Dizin Oluşturucu durumu ve yürütme geçmişini (dizinlenmiş öğeleri, hatalar vb. sayısı) izlemek için kullanmak bir **dizin oluşturucu durumu** isteği:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2019-05-06
    api-key: admin-key

Yanıt, aşağıdakine benzer görünmelidir:

    {
        "\@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
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

Yürütme geçmişi, 50 (en son yürütme yanıtta önce gelirse), ters kronolojik sırada sıralanır en son tamamlanan yürütmelerin içerir.
Yanıt hakkında ek bilgiler bulunabilir [dizin oluşturucu durumunu Al](https://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Dizin oluşturucular bir zamanlamaya göre çalıştırma
Düzenli bir zamanlamaya göre çalıştırmak için dizin oluşturucu da düzenleyebilirsiniz. Bunu yapmak için ekleme **zamanlama** oluştururken veya dizin oluşturucunun güncelleştirme özelliği. Aşağıdaki örnekte, dizin oluşturucu güncelleştirmek için PUT İsteği gösterilmektedir:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Aralığı** parametresi gereklidir. İki ardışık dizin oluşturucusu yürütme başlangıcı arasındaki zaman aralığını gösterir. İzin verilen en küçük aralığı 5 dakikadır; uzun bir gündür. Bir XSD "dayTimeDuration" değeri biçimlendirilmelidir (sınırlı bir alt kümesini bir [ISO 8601 süre](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değeri). Bu desen: `P(nD)(T(nH)(nM))`. Örnekler: `PT15M` her 15 dakika boyunca `PT2H` her 2 saatte için.

İsteğe bağlı **startTime** zamanlanan yürütme ne zaman başlamak gösterir. Atlanırsa, geçerli UTC zamanı kullanılır. Bu süre geçmişte – dizinleyici startTime sürekli olarak çalışıyorsa durum ilk yürütme zamanlandı olabilir.  

Bir dizin oluşturucu yalnızca tek bir yürütme, aynı anda çalıştırabilirsiniz. Bir dizin oluşturucu, yürütme zamanlandığında çalışıyorsa, yürütme sonraki zamanlanan saatine kadar ertelenir.

Bu daha somut anlaşılması için bir örnek düşünelim. Varsayalım biz yapılandırılmış aşağıdaki saatlik zamanlaması:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Şunlar olur:

1. 1 Mart 2015 12: 00'da geçici bir çözüm veya bu ilk dizin oluşturucu yürütmeyi başlatır UTC.
2. Bu yürütme 20 dakika (veya dilediğiniz zaman küçüktür 1 saat) geçen varsayılır.
3. İkinci yürütme sırasında veya 1 Mart 2015'e 1: 00'da geçici olarak başlatır.
4. Şimdi yaklaşık 2: 10'da tamamlanabilmesi bu yürütme bir saatten – örneğin 70 dakika – aldığını varsayalım.
5. Saat 2:00 ' da artık üçüncü yürütme başlatmak bu olur. Ancak, çünkü 1'de ikinci yürütülmesinden yine de çalıştırmak, üçüncü yürütme atlandı. Üçüncü yürütme 3'da başlar.

Ekleme, değiştirme veya var olan bir dizin oluşturucu için bir zamanlama kullanılarak bir **PUT dizin oluşturucu** isteği.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Yeni, değiştirilen ve silinen satırları yakalama

Azure Search kullanan **Artımlı dizin** tablonun tamamını yeniden dizin oluşturma veya bir dizin oluşturucu her çalıştırıldığında görüntülemek olmamasına özen gösterin. Azure arama, iki Artımlı dizin desteklemek için algılama ilkeleri değiştirme sağlar. 

### <a name="sql-integrated-change-tracking-policy"></a>SQL ile tümleştirilen değişiklik izleme ilkesi
SQL veritabanınız destekliyorsa [değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), kullanmanızı öneririz **SQL tümleşik değişiklik izleme İlkesi**. Bu en verimli bir ilkedir. Ayrıca, bir açık "geçici silme" sütun tablonuza eklemek gerek kalmadan silinen satırları tanımlamak Azure Search sağlar.

#### <a name="requirements"></a>Gereksinimler 

+ Veritabanı sürümü gereksinimleri:
  * SQL Server 2012 SP3 ve daha sonra Azure Vm'lerinde SQL Server kullanıyorsanız.
  * Azure SQL veritabanı, Azure SQL veritabanı kullanıyorsanız, V12.
+ Tabloları yalnızca (Görünüm yok). 
+ Veritabanında [etkin değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) tablosu için. 
+ Bileşik birincil anahtar (bir birincil anahtar içeren birden çok sütundan) tablosunda.  

#### <a name="usage"></a>Kullanım

Bu ilkeyi kullanmak, oluşturmak veya bu gibi veri kaynağını güncelleştirmek için:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Silinmiş satırları zaman tümleşik SQL değişiklik izleme ilkesi, kullanarak ayrı veri silme algılama İlkesi belirlemezseniz - Bu ilkeyi tanımlamak için yerleşik destek sunmaktadır. Ancak, silmeleri algılanan "automagically" olarak arama dizininize belge anahtarını SQL tablosundaki birincil anahtar ile aynı olmalıdır. 

> [!NOTE]  
> Kullanırken [TRUNCATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/truncate-table-transact-sql) çok sayıda satırı SQL tablosundan kaldırmak için dizin oluşturucu olması gerekir [sıfırlama](https://docs.microsoft.com/rest/api/searchservice/reset-indexer) değişiklik izleme satır silme seçmek için durumu sıfırlanır.

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Yüksek su işareti değişiklik algılama İlkesi

Bu değişiklik algılama ilkesi sürüm veya bir satır son güncelleştirildiği an yakalama "yüksek su işareti" sütun kullanır. Bir görünüm kullanıyorsanız, yüksek su işareti İlkesi kullanmanız gerekir. Yüksek su işareti sütunu aşağıdaki gereksinimleri karşılaması gerekir.

#### <a name="requirements"></a>Gereksinimler 

* Tüm eklemeleri, sütun için bir değer belirtin.
* Bir öğe için tüm güncelleştirmeleri de sütununun değerini değiştirir.
* Bu sütunun değerini, her ekleme veya güncelleştirme ile artırır.
* Aşağıdaki sorgular nerede ve ORDER BY yan tümcesi verimli bir şekilde çalıştırılabilir: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Kullanmanızı öneririz [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) yüksek su işareti sütunu için veri türü. Herhangi bir veri türü kullanılması durumunda değişiklik izleme işlemleri aynı anda bir dizin oluşturucu sorgu yürütülürken saklanacaktır tüm değişiklikleri yakalamak için garanti edilmez. Kullanırken **rowversion** salt okunur çoğaltmalar bir yapılandırmada birincil çoğaltma üzerinde dizin oluşturucu işaret etmelidir. Yalnızca birincil kopya veri eşitleme senaryoları için kullanılabilir.

#### <a name="usage"></a>Kullanım

Yüksek su işareti ilkesi oluşturun veya bu gibi veri kaynağını güncelleştirmek için:

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
> Kaynak tablosu yüksek su işareti sütunu dizin yoksa, SQL Dizin Oluşturucu tarafından kullanılan sorguları zaman aşımına uğrayabilir. Özellikle, `ORDER BY [High Water Mark Column]` yan tümcesi tablo fazla satır içerdiğinde verimli bir şekilde çalıştırmak için bir dizin gerektirir.
>
>

Zaman aşımı hatalarla karşılaşırsanız, kullanabileceğiniz `queryTimeout` sorgu zaman aşımı varsayılan 5 dakikalık zaman aşımı daha yüksek bir değere ayarlamak için dizin oluşturucuyu yapılandırma ayarı. Örneğin, zaman aşımı 10 dakika olarak ayarlamak için oluşturma veya dizin oluşturucu aşağıdaki yapılandırma ile güncelleştir:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Ayrıca devre dışı bırakabilirsiniz `ORDER BY [High Water Mark Column]` yan tümcesi. Ancak, dizin oluşturucu yürütmeyi bir hata nedeniyle kesintiye uğrarsa, daha sonra - çalıştırıyorsa dizin oluşturucuyu, zaten neredeyse tüm satırları yarıda kesildi zaman işlediği bile tüm satırları yeniden işlemek dizin oluşturucu olduğundan bu önerilmez. Devre dışı bırakmak için `ORDER BY` yan tümcesi, kullanım `disableOrderByHighWaterMarkColumn` dizin oluşturucu tanımı ayarlama:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Geçici silme sütunu silmeyi algılama İlkesi
Kaynak tablosundan silinen satır, büyük olasılıkla arama dizini de satırları silmek istediğiniz. Tümleşik SQL değişiklik izleme ilkesi kullanıyorsanız, bunu sizin için dikkate. Bununla birlikte, yüksek su işareti değişiklik izleme ilkesi ile silinen satırları yardımcı olmaz. Ne yapmanız gerekir?

Tablodan satırları fiziksel olarak kaldırılırsa, Azure Search, artık mevcut kayıtlarının çıkarsamak için bir yolu yoktur.  Ancak, mantıksal olarak tablodan kaldırmadan satırları silmek için "geçici silme" tekniği kullanabilirsiniz. Bir sütun, tablo veya Görünüm ve işareti satırları bu sütun kullanıldığında silindi olarak ekleyin.

Geçici silme tekniği kullanarak, geçici silme ilkesi şu şekilde oluştururken veya veri kaynağı güncelleştiriliyor belirtebilirsiniz:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]"
        }
    }

**SoftDeleteMarkerValue** bir dize olması gerekir: Gerçek, değerin dize gösterimini kullanın. Örneğin, burada silinen satırlar işaretlenmiş 1 değeri bir tamsayı sütunu varsa, kullanmak `"1"`. Burada silinen satırlar işaretlenir Boolean true değeriyle bir BIT sütununuz varsa, dize sabit değeri kullanın `True` veya `true`, böyle olması fark etmez.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>SQL ve Azure Search veri türleri arasında eşleme
| SQL veri türü | Hedef dizin izin alan türleri | Notlar |
| --- | --- | --- |
| bit |Edm.Boolean, Edm.String | |
| int, smallint, tinyint |EDM.Int32, EDM.Int64, Edm.String | |
| bigint |EDM.Int64, Edm.String | |
| Gerçek, float |Edm.Double, Edm.String | |
| küçük para, para ondalık sayısal |Edm.String |Azure arama, bu kesinlik kaybı olur çünkü ondalık türleri Edm.Double dönüştürmeyi desteklemez |
| karakter, n karakter, değişken karakter, n değişken karakter |Edm.String<br/>Collection(Edm.String) |Bir SQL dize, dize JSON dizisini temsil ediyorsa Collection(Edm.String) alanını doldurmak için kullanılabilir: `["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, date, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| Coğrafi konum |Edm.GeographyPoint |Yalnızca Coğrafya türünün örneğini (varsayılan değer olan) SRID 4326 NOKTASIYLA desteklenir |
| rowVersion |Yok |Satır sürümü sütunları arama dizinini depolanamaz, ancak değişiklik izleme için kullanılabilir |
| zaman, zaman aralığı, ikili, varbinary, resim, xml, geometri, CLR Türleri |Yok |Desteklenmiyor |

## <a name="configuration-settings"></a>Yapılandırma ayarları
SQL dizin oluşturucu, çeşitli yapılandırma ayarlarını kullanıma sunar:

| Ayar | Veri türü | Amaç | Varsayılan değer |
| --- | --- | --- | --- |
| queryTimeout |string |SQL sorgusu yürütme zaman aşımı süresi ayarlar |5 dakika ("00: 05:00") |
| disableOrderByHighWaterMarkColumn |bool |ORDER BY yan tümcesi atlamak için yüksek su işareti ilke tarafından kullanılan SQL sorgusunun neden olur. Bkz: [yüksek su işareti İlkesi](#HighWaterMarkPolicy) |false |

Bu ayarlar kullanılan `parameters.configuration` dizin oluşturucu tanımı nesnesi. Örneğin, sorgu zaman aşımı 10 dakika olarak ayarlamak için oluşturma veya dizin oluşturucu aşağıdaki yapılandırma ile güncelleştir:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>SSS

**S: Azure'daki Iaas vm'lerinde çalışan SQL veritabanları ile Azure SQL dizin oluşturucu kullanabilir miyim?**

Evet. Ancak, veritabanınıza bağlanmak için arama hizmetinize izin vermeniz gerekir. Daha fazla bilgi için [bir Azure sanal makinesinde SQL Server için Azure Search dizin oluşturucu arasında bağlantı yapılandırma](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**S: Azure SQL dizin oluşturucu şirket içinde çalışan SQL veritabanları ile kullanabilir miyim?**

Doğrudan yönetilemez. Biz önerilmez ve bunu yapmanız bu nedenle veritabanlarınızı Internet trafiğini açmanız gerekir gibi doğrudan bir bağlantı destekler. Müşteriler, Azure Data Factory gibi köprüsü teknolojilerini kullanarak bu senaryo ile başarılı olduğunda. Daha fazla bilgi için [Azure Data Factory kullanarak Azure Search dizini için veri gönderme](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**S: Iaas, Azure üzerinde çalışan SQL Server dışındaki veritabanları ile Azure SQL dizin oluşturucu kullanabilir miyim?**

Hayır. Dizin Oluşturucu SQL Server dışındaki herhangi bir veritabanına ile test henüz çünkü bu senaryo desteklemiyoruz.  

**S: Bir zamanlamaya göre çalışan birden çok dizin oluşturucular oluşturabilir miyim?**

Evet. Ancak, yalnızca bir dizin oluşturucu bir düğümde aynı anda çalışabilir. Eşzamanlı olarak çalışan birden çok dizin oluşturucu gerekiyorsa, birden fazla arama birimi, arama hizmetinizin'kurmak ölçeklendirmeyi düşünün.

**S: Bir dizin oluşturucu çalışan sorgu iş yükümün etkiliyor mu?**

Evet. Dizin Oluşturucu, arama hizmetinizdeki düğümlerinden biri üzerinde çalışır ve o düğümün kaynak dizin oluşturma ve sorgu trafiği ve diğer API isteklerinin hizmet arasında paylaşılır. Dizin oluşturma ve sorgu kullanımı yoğun iş yüklerini çalıştırmak ve 503 hatalarını veya artan yanıt süreleri, yüksek oranda karşılaşırsanız, göz önünde bulundurun [arama hizmetinizi ölçeklendirme](search-capacity-planning.md).

**S: Bir ikincil çoğaltma kullanabileceğim bir [yük devretme kümesi](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) veri kaynağı olarak?**

Duruma göre değişir. Tam bir tablo veya Görünüm dizin oluşturma işlemi için ikincil bir çoğaltmaya kullanabilirsiniz. 

Azure Search, artan dizin oluşturma için iki değişiklik algılama ilkeleri destekler: SQL değişiklik izleme ve yüksek su işareti ile Tümleştirildi.

Salt okunur çoğaltmalarda SQL veritabanı, tümleşik değişiklik izleme desteklemez. Bu nedenle, yüksek su işareti İlkesi kullanmanız gerekir. 

Standart önerimizde rowversion veri türü için yüksek su işareti sütunu kullanmaktır. Ancak, rowversion kullanarak SQL veritabanı'nda kişinin kullanır `MIN_ACTIVE_ROWVERSION` işlevin salt okunur çoğaltmalarda desteklenmiyor. Bu nedenle, rowversion kullanıyorsanız, dizin oluşturucu bir birincil çoğaltmaya işaret etmelidir.

Salt okunur bir çoğaltma üzerinde rowversion kullanmayı denerseniz, aşağıdaki hatayı görürsünüz: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update the datasource and specify a connection to the primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**S: Yüksek su işareti değişiklik izleme için bir alternatif, rowversion olmayan sütun kullanabilir miyim?**

Bu işlem önerilmez. Yalnızca **rowversion** için güvenilir veri eşitlenmesine izin verir. Ancak, uygulama mantığınızın bağlı olarak, güvenli olabilir:

+ Dizin Oluşturucu çalıştırıldığında olduğunu dizini tablosunda hiçbir bekleyen hareket emin olun (örneğin, tüm tablo güncelleştirmeleri toplu işlem olarak bir zamanlamaya göre gerçekleşir ve tablo güncelleştirme ile çakışan önlemek için Azure Search dizin oluşturucu zamanlaması Ayarla Zamanlama).  

+ Tüm eksik satırları seçmek için bir tam reindex düzenli aralıklarla bunu. 