---
title: Azure SQL veritabanında bellek içi teknolojiler | Microsoft Docs
description: Azure SQL veritabanında bellek içi teknolojileri analizi iş yükleri ve işlem performansını büyük ölçüde geliştirebilirsiniz.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jodebrui
ms.author: jodebrui
ms.reviewer: ''
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: d850aff8ddb2a8b6cdd68620ae823d582c527581
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229099"
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>SQL veritabanında bellek içi teknolojileri kullanarak performansını iyileştirin

Azure SQL veritabanı'nda Bellek içi teknolojileri kullanarak, çeşitli iş yükleriyle performans iyileştirmeleri elde edebilirsiniz: işlem (çevrimiçi işlem gerçekleştirme (OLTP)), analiz (çevrimiçi analitik işlem (OLAP)) ve (hibrit işlem karma / analitik işleme (HTAP)). Daha verimli sorgu ve işlem nedeniyle, bellek içi teknolojileri de maliyetini azaltmak için yardımcı olur. Genellikle, performans kazanç elde etmek için veritabanı fiyatlandırma katmanını yükseltme gerekmez. Bazı durumlarda bile aktarmanızı fiyatlandırma katmanı, hala performans geliştirmeleri ile bellek içi teknolojileri görerek azaltın.

Bellek içi OLTP performansını önemli ölçüde artırmak için nasıl yardımcı oldu iki örnek aşağıda verilmiştir:

- Bellek içi OLTP kullanarak [70 oranında Dtu artırırken iş yüklerinin çift çekirdek iş çözümleri bağlanabildi](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU anlamına gelir *veritabanı işlem birimi*, ve bir ölçüm kaynak tüketimi içerir.
- Aşağıdaki videoda bir örnek iş yükü ile kaynak tüketimi önemli bir iyileştirme: [Video Azure SQL veritabanı, bellek içi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Daha fazla bilgi için blog gönderisine bakın: [Azure SQL veritabanı Web günlüğü gönderisinde, bellek içi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

Bellek içi teknolojileri tüm Premium elastik havuzlar veritabanları dahil olmak üzere Premium katman veritabanlarında kullanılabilir.

Aşağıdaki videoda, olası performans artışı ile Azure SQL veritabanında bellek içi teknolojiler açıklanmaktadır. Her zaman gördüğünüz performans kazancı iş yüküne ve veri erişimi deseni veritabanının yapısını dahil olmak üzere birçok faktöre bağlı olduğunu unutmayın ve benzeri.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Azure SQL veritabanı, bellek içi teknolojilerin sahiptir:

- *Bellek içi OLTP* işlem artırır ve işlem için gecikme süresini azaltır. Bellek içi OLTP ' yararlanan senaryolar şunlardır: olayları veya önbelleğe alma, veri yükleme ve geçici tablo ve tablo değişkeni senaryoları IOT cihazları, ticaret ve oyun, veri alma gibi yüksek performanslı işlem.
- *Kümelenmiş columnstore dizinleri* (en fazla 10 kez) depolama altyapınızın kapladığı alanı azaltmak ve raporlama ve analiz sorguları için performansı geliştirin. Bu olgu tabloları ile veri reyonlarınızı veritabanınızda daha fazla veri uyacak şekilde genişletebilir ve performansı artırmak için kullanabilirsiniz. Ayrıca, bu birlikte çalışarak geçmiş verileri işletimsel veritabanınız arşivlemek ve 10 kata kadar daha fazla veri sorgulama yapmak için kullanabilirsiniz.
- *Kümelenmemiş columnstore dizinleri* HTAP yardımcı olmak için işletimsel veritabanını çalıştırmak pahalı ayıklama, gerek kalmadan doğrudan sorgulama aracılığıyla işinizi gerçek zamanlı Öngörüler edinmek için dönüştürme ve yükleme (ETL) işlemi ve bekleyin veri ambarı'doldurulmalıdır. Kümelenmemiş columnstore dizinleri, işlemsel iş etkisi azaltırken, OLTP veritabanı üzerinde çok hızlı analiz sorguları yürütülmesine izin.
- Ayrıca, bir columnstore dizini olan bir bellek için iyileştirilmiş tablo birleşimi olabilir. Bu birleşim çok hızlı işlemler gerçekleştirmenize olanak sağlar ve *eşzamanlı olarak* analiz sorguları verilere çok hızlı bir şekilde çalıştırma.

SQL Server ürününün bir parçası 2012 ve 2014'ten itibaren columnstore dizinleri hem de bellek içi OLTP sırasıyla olmuştur. Azure SQL veritabanı ve SQL Server bellek içi teknolojileri aynı uygulamasını paylaşın. SQL Server'da yayınlanmadan önce bundan sonra bu teknolojiler için yeni özellikler Azure SQL veritabanı'nda ilk olarak kullanıma sunulur.

Bu makalede, Azure SQL veritabanı'na özgü bellek içi OLTP ve columnstore dizinleri yönlerini açıklar ve ayrıca örnekleri içerir:
- Depolama ve veri boyut sınırları, bu teknolojiler etkisini göreceksiniz.
- Bu teknolojiler farklı fiyatlandırma katmanları arasında kullanan veritabanlarını hareketini yönetme görürsünüz.
- Azure SQL veritabanında columnstore dizinleri yanı sıra, bellek içi OLTP kullanımını gösteren iki örnek görürsünüz.

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Teknolojileri hakkında ayrıntılı bilgi:

- [Bellek içi OLTP genel bakış ve kullanım senaryoları](https://msdn.microsoft.com/library/mt774593.aspx) (müşteri örnek olay incelemeleri ve kullanmaya başlamak için bilgi başvurular içerir)
- [Bellek içi OLTP için belgeleri](https://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore dizinleri Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx)
- Karma işlemsel / (HTAP), olarak da bilinen analitik işleme [gerçek zamanlı işlem analizi](https://msdn.microsoft.com/library/dn817827.aspx)

Bellek içi OLTP hızlı öncü: [hızlı başlangıç 1: T-SQL performansı daha hızlı bellek içi OLTP teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx) (yardımcı olması için başka bir makalede başlama)

Kapsamlı videoları teknolojileri hakkında:

- [Azure SQL veritabanında bellek içi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (performans avantajlarının bu sonuçları kendiniz yeniden oluşturma adımları ve bir tanıtım içeren)
- [Bellek içi OLTP videolar: Nedir ve ne zaman/nasıl kullanmak için](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Columnstore dizini: Bellek içi analiz videolardan 2016 Ignite.](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Depolama ve veri boyutu

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Bellek içi OLTP için verileri boyutu ve depolama kapasitesi

Bellek içi OLTP, kullanıcı verilerini depolamak için kullanılan bellek için iyileştirilmiş tablolar içerir. Bu tablolar belleğe sığması gerekir. SQL veritabanı hizmeti bellekte doğrudan yönetmek için kullanıcı verileri için bir kota kavramını sahibiz. Bu fikir olarak adlandırılır *bellek içi OLTP depolama alanını*.

Fiyatlandırma katmanı ve her esnek havuzun fiyatlandırma katmanı her desteklenen tek veritabanı, belirli bir miktarda bellek içi OLTP depolama alanı içerir. Bkz: [DTU tabanlı kaynak sınırları - tek veritabanı](sql-database-dtu-resource-limits-single-databases.md), [DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md),[sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

Aşağıdaki öğeler, bellek içi OLTP depolama tavanınızı doğru sayısı:

- Etkin kullanıcı veri satırları bellek için iyileştirilmiş tablolarda ve Tablo değişkenleri. Eski satır sürümleri doğru uç sayılmaz unutmayın.
- Bellek için iyileştirilmiş tablolarda dizinler.
- ALTER TABLE işlemlerin işlemsel yükünü.

Sınırına ulaşırsanız, bir kota aşımı hatayla karşılaştıysanız ve artık ekleyemez veya verileri güncelleştirin. Bu hatayı gidermek için verileri silmek veya havuz veya veritabanı fiyatlandırma katmanı artırın.

Bellek içi OLTP depolama alanı kullanımı izleme ve neredeyse sınırına ulaştığınızda uyarılarını yapılandırma hakkında daha fazla ayrıntı için bkz. [bellek içi izleme depolama](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Elastik havuzlar hakkında

Elastik havuzlar sayesinde, bellek içi OLTP depolama alanı, havuzdaki tüm veritabanları arasında paylaşılır. Bu nedenle, bir veritabanında kullanım büyük olasılıkla diğer veritabanları etkileyebilir. Bu iki Azaltıcı Etkenler şunlardır:

- Yapılandırma bir `Max-eDTU` veya `MaxvCore` bir bütün olarak havuz eDTU veya sanal çekirdek sayısı daha düşük maliyetlidir veritabanları için. Bu maksimum bellek içi OLTP depolama alanı kullanımı, havuzdaki eDTU sayısına karşılık gelen boyutuna herhangi bir veritabanında belirler.
- Yapılandırma bir `Min-eDTU` veya `MinvCore` 0'dan büyük. Bu minimum havuzdaki her bir veritabanına karşılık gelen yapılandırılmış kullanılabilir bellek içi OLTP depolama alanı miktarı olduğunu güvence altına alır. `Min-eDTU` veya `vCore`.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Veri boyutu ve depolama için columnstore dizinleri

Columnstore dizinleri, belleğe sığması için gerekli değildir. Bu nedenle, yalnızca cap dizinleri boyutuna, belgelenen genel veritabanı boyutu, olan [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makaleler.

Kümelenmiş columnstore dizinleri kullandığınızda, aynı zamanda sütunlu sıkıştırma temel tablo depolaması için kullanılır. Bu sıkıştırma veritabanında daha fazla veri sığabilen anlamına gelir, kullanıcı veri depolama ayak izini önemli ölçüde azaltabilir. Ve sıkıştırma ile daha da artırılabilir [sütunlu arşiv sıkıştırma](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). Verilerin niteliğine üzerinde elde edebileceğiniz sıkıştırma bağlıdır, ancak 10 kez sıkıştırma durumdur.

Örneğin, bir maksimum boyut 1 terabayt (TB) sahip bir veritabanı varsa ve 10 kez sıkıştırma columnstore dizinleri kullanarak elde etmek, toplam 10 TB kullanıcı verilerini veritabanında sığabilen.

Kümelenmemiş columnstore dizinleri kullandığınızda, temel tablo hala geleneksel rowstore biçiminde depolanır. Bu nedenle, depolama tasarrufu kümelenmiş columnstore dizinleri ile kadar büyük değil. Ancak, tek bir columnstore dizini ile geleneksel kümelenmemiş dizinler sayısı değiştiriyorsanız, genel bir tablo için depolama ayak izini tasarruf görebilirsiniz.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Fiyatlandırma katmanları arasında bellek içi teknolojileri kullanan veritabanlarını taşıma

Hiçbir zaman uyumsuzlukları veya başka sorunlar için daha yüksek bir fiyatlandırma katmanı, gibi standart Premium sürümüne yükseltme, vardır. Kullanılabilir işlevleri ve kaynakları yalnızca artırın.

Ancak, fiyatlandırma katmanını eski sürüme düşürme veritabanınızı olumsuz yönde etkileyebilir. Veritabanı bellek içi OLTP nesneler içeriyorsa, Premium'dan standart veya temel sürümüne düşürürseniz, özellikle görünen etkisidir. (Bunlar görünür kalır olsa bile), bellek için iyileştirilmiş tablolar indirgeme sonra kullanılamaz. Bir esnek havuzun fiyatlandırma katmanını azaltmayı veya bellek içi teknolojileri ile bir veritabanını elastik havuzun bir standart veya temel taşıma ilgili noktaların aynısı geçerlidir.

### <a name="in-memory-oltp"></a>Bellek içi OLTP

*Temel/standart eski sürüme düşürme*: bellek içi OLTP veritabanlarında standart veya temel katmanında desteklenmiyor. Ayrıca, standart veya temel katmanına herhangi bir bellek içi OLTP nesneleri olan bir veritabanını taşımak mümkün değildir.

Belirli bir veritabanı, bellek içi OLTP destekleyip desteklemediğini anlamak için programlı bir yolu yoktur. Aşağıdaki Transact-SQL sorgusunu yürütebilirsiniz:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Sorgu döndürürse **1**, bellek içi OLTP, bu veritabanında desteklenir.

Standart/temel veritabanına düşürme önce tüm bellek için iyileştirilmiş tablolar ve tablo türleri yanı sıra, tüm yerel olarak derlenen T-SQL modülleri kaldırın. Aşağıdaki sorgularda standart/temel bir veritabanı indirgenen önce kaldırılması gereken tüm nesneleri tanımlar:

```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

*Daha düşük bir Premium katmanı için eski sürüme düşürme*: bellek için iyileştirilmiş tablolardaki verileri veritabanı fiyatlandırma katmanı ile ilişkili olan veya elastik havuzun kullanılabilir bellek içi OLTP depolama içinde sığması gerekir. Fiyatlandırma katmanını daha düşük deneyin veya veritabanını kullanılabilir yeterli bellek içi OLTP depolama alanı sahip olmayan bir havuza taşıma işlemi başarısız olur.

### <a name="columnstore-indexes"></a>Columnstore dizinleri

*Temel veya standart için eski sürüme düşürme*: Columnstore dizinleri, yalnızca Premium fiyatlandırma katmanına ve standart S3 katmanı ve yukarıda ve üzerinde değil, temel katmanı desteklenir. Veritabanınızı desteklenmeyen katmanı veya düzeyini düşürme, columnstore dizininiz kullanılamaz duruma gelir. Sistem, columnstore dizini korur, ancak hiç dizin yararlanır. Daha sonra desteklenen katmanı veya düzeyi geri yükseltirseniz, columnstore dizininiz tekrar havuzlamanızı hemen hazırdır.

Varsa bir **kümelenmiş** columnstore dizini, tüm tablonun kullanım dışı olur sonra düşürme. Bu nedenle tüm bırak öneririz *kümelenmiş* veritabanınızı desteklenmeyen katmanı veya düzeyini düşürme önce columnstore dizinini oluşturur.

*Bir alt desteklenen katmanı veya düzeyinde eski sürüme düşürme*: tüm veritabanının veya elastik havuzdaki kullanılabilir depolama alanı fiyatlandırma katmanı hedef için maksimum veritabanı boyutu içinde sığıyorsa Bu geçişin başarılı olur. Columnstore dizinleri gelen belirli bir etkisi yoktur.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Bellek içi OLTP örnek yükleyin

AdventureWorksLT örnek veritabanı içinde birkaç tıklamayla oluşturabilirsiniz [Azure portalında](https://portal.azure.com/). Ardından, bu bölümdeki adımları nasıl AdventureWorksLT veritabanınızı bellek içi OLTP nesnelerle zenginleştirin ve performans avantajlarının göstermek açıklanmaktadır.

Daha fazla alıyormuş, ancak daha görsel olarak çekici performans gösteri için bellek içi OLTP için bkz:

- Yayın: [içinde-bellek-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Kaynak kodu: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Yükleme adımları

1. İçinde [Azure portalında](https://portal.azure.com/), bir sunucuda Premium veya iş açısından kritik bir veritabanı oluşturun. Ayarlama **kaynak** AdventureWorksLT örnek veritabanı. Ayrıntılı yönergeler için bkz. [ilk Azure SQL veritabanınızı oluşturma](sql-database-get-started-portal.md).

2. SQL Server Management Studio ile veritabanına bağlanma [(SSMS.exe)](https://msdn.microsoft.com/library/mt238290.aspx).

3. Kopyalama [bellek içi OLTP Transact-SQL betiği](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) panonuza. T-SQL betiği, 1. adımda oluşturduğunuz AdventureWorksLT örnek veritabanını gerekli bellek içi nesneleri oluşturur.

4. SSMS ile T-SQL betiğini yapıştırın ve ardından betiği yürütün. `MEMORY_OPTIMIZED = ON` Yan tümcesi CREATE TABLE deyimleri çok önemlidir. Örneğin:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Hata 40536


T-SQL komut dosyasını çalıştırırken hata 40536 alırsanız, veritabanı, bellek içi destekleyip desteklemediğini doğrulamak için aşağıdaki T-SQL betiğini çalıştırın:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Sonucu **0** , bellek içi desteklenmiyor anlamına gelir ve **1** desteklenip desteklenmediğini anlamına gelir. Sorunu tanılamak için veritabanı Premium hizmet katmanında olduğundan emin olun.


#### <a name="about-the-created-memory-optimized-items"></a>Oluşturulan bellek için iyileştirilmiş öğeleri hakkında

**Tabloları**: aşağıdaki bellek için iyileştirilmiş tablolarda örneği içerir:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Bellek için iyileştirilmiş tablolar üzerinden inceleyebilirsiniz **Nesne Gezgini** ssms'de. Sağ **tabloları** > **filtre** > **filtre ayarları** > **bellek için iyileştirilmiş**. Değer 1'e eşittir.


Veya, Katalog görünümleri gibi sorgulayabilirsiniz:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Saklı yordam doğal olarak derlenen**: bir katalog sorguyu görüntüle SalesLT.usp_InsertSalesOrder_inmem inceleyebilirsiniz:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>Örnek OLTP iş yükü çalıştırma

Aşağıdaki iki arasındaki tek fark *saklı yordamlar* olup bellek için iyileştirilmiş tablolar sürümlerini ilk yordam kullanır, ikinci yordam normal diskteki tablolar kullanır:

- SalesLT **.** usp_InsertSalesOrder **_inmem**
- SalesLT **.** usp_InsertSalesOrder **_ondisk**


Bu bölümde, kullanışlı kullanmayı gördüğünüz **ostress.exe** geçmek stresli düzeylerinde iki saklı yordam yürütme için yardımcı program. Ne tamamlanması için iki stres çalıştırmaları sürdüğünü karşılaştırabilirsiniz.


Ostress.exe çalıştırdığınızda, aşağıdakilerin her ikisi için tasarlanmış parametre değerlerinin geçmesini öneririz:

- Çok sayıda eş zamanlı bağlantı çalıştırma kullanarak - n100.
- Yüzlerce kez, her bağlantı döngü göre sahip kullanma - r500.


Ancak, her şeyin çalıştığından emin olmak, - n10 ve - r50 gibi çok daha küçük değerlerle başlatmak isteyebilirsiniz.


### <a name="script-for-ostressexe"></a>Betik ostress.exe için


Bu bölümde, bizim ostress.exe komut satırında katıştırılmış T-SQL betiği görüntüler. Betik daha önce yüklediğiniz T-SQL betiği tarafından oluşturulan öğelerini kullanır.


Aşağıdaki betiği, bellek için iyileştirilmiş aşağıdaki beş satır öğeleri ile bir örnek satış siparişi ekler *tabloları*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Yapmak *_ondisk* sürümü önceki T-SQL betiğinin ostress.exe için her iki tekrarı değiştirin *_inmem* ile alt dize *_ondisk*. Bu değişiklik tablolarına ve depolanmış yordamlarına adlarını etkiler.


### <a name="install-rml-utilities-and-ostress"></a>RML yardımcı programları ve ostress yükleyin


İdeal olarak, bir Azure sanal makinesinde (VM) ostress.exe çalıştırmayı planladığınız. Oluşturmak bir [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) AdventureWorksLT veritabanınızın bulunduğu aynı Azure bölgesinde coğrafi. Ancak, bunun yerine dizüstü bilgisayarınızda ostress.exe çalıştırabilirsiniz.


VM, konak üzerinde veya seçin, yeniden yürütme işaretleme dili (RML) yardımcı programlarını yükleyin. Yardımcı programları ostress.exe içerir.

Daha fazla bilgi için bkz.
- Ostress.exe tartışmalarına [örnek veritabanı için bellek içi OLTP](https://msdn.microsoft.com/library/mt465764.aspx).
- [Örnek veritabanı için bellek içi OLTP](https://msdn.microsoft.com/library/mt465764.aspx).
- [Ostress.exe yüklemek için blog](https://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Çalıştırma *_inmem* ilk iş yükü stres


Kullanabileceğiniz bir *RML Cmd komut istemi* bizim ostress.exe komut satırını çalıştırmak için pencere. Komut satırı parametreleri için ostress doğrudan:

- 100 bağlantı eşzamanlı olarak çalıştırılmasını (-n100).
- Her bağlantınız 50 kez T-SQL betiği çalıştırın (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Önceki ostress.exe komut satırını çalıştırmak için:


1. Veritabanı veri içerikleri, ssms'de, tüm önceki çalıştırmaları tarafından eklenen tüm verileri silmek için aşağıdaki komutu çalıştırarak sıfırlayın:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Önceki ostress.exe komut satırını metnin panonuza kopyalayın.

3. Değiştirin `<placeholders>` parametreleri -S - U -P -d doğru gerçek değerlerle için.

4. Düzenlenen komut satırınızda bir RML Cmd penceresinde çalıştırın.


#### <a name="result-is-a-duration"></a>Bir süre sonucudur


Ostress.exe sona erdiğinde RML Cmd penceresinde çalıştırma süresi, son satırı çıkış olarak yazar. Örneğin, daha kısa bir test çalıştırması yaklaşık 1,5 dakika görüşmelerin Süren:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Sıfırlama, düzenlemek *_ondisk*, sonra yeniden çalıştırın


Sonuçtan sonra *_inmem* çalıştırın, için aşağıdaki adımları gerçekleştirin *_ondisk* çalıştırın:


1. Ssms'de önceki çalışması tarafından eklenen tüm verileri silmek için aşağıdaki komutu çalıştırarak veritabanını sıfırlama:
```
EXECUTE Demo.usp_DemoReset;
```

2. Tüm değiştirmek için ostress.exe komut satırı Düzenle *_inmem* ile *_ondisk*.

3. İkinci kez ostress.exe yeniden çalıştırın ve süresi sonucu yakalayın.

4. Yeniden (depoladığımız ne miktarda test verisi olabilir silme için) veritabanı sıfırlayın.


#### <a name="expected-comparison-results"></a>Beklenen Karşılaştırma sonuçları

Bellek içi testlerimizin tarafından geliştirilmiş performans gösterilmesini **dokuz kez** veritabanı ile aynı Azure bölgesinde Azure sanal makinesinde çalışan ostress ile alıyormuş bu iş yükü için.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Bellek içi analiz örneği yüklemek


Bir columnstore dizini geleneksel b-ağacı dizini karşı kullanırken bu bölümde, GÇ ve istatistikleri sonuçlarını karşılaştırın.


Bir OLTP iş yükü, gerçek zamanlı analizler için kümelenmemiş bir columnstore dizini kullanmak idealdir. Ayrıntılar için bkz [Columnstore dizinleri açıklanan](https://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Columnstore analytics test hazırlama


1. Örnekten yeni bir AdventureWorksLT veritabanı oluşturmak için Azure portalını kullanın.
 - Bu tam adını kullanın.
 - Her Premium Hizmet katmanını seçin.

2. Kopyalama [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) panonuza.
 - T-SQL betiği, 1. adımda oluşturduğunuz AdventureWorksLT örnek veritabanını gerekli bellek içi nesneleri oluşturur.
 - Betik, Boyut tablosuna ve iki olgu tabloları oluşturur. Olgu tabloları 3,5 milyon satır ile doldurulur.
 - Komut dosyasının tamamlanması 15 dakika sürebilir.

3. SSMS ile T-SQL betiğini yapıştırın ve ardından betiği yürütün. **COLUMNSTORE** anahtar sözcüğünü **CREATE INDEX** deyimi, önemli olarak:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Uyumluluk düzeyi 130 için AdventureWorksLT ayarlayın:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Düzeyi 130 bellek içi özellikleri için doğrudan ilgili değildir. Ancak düzeyi 130 genellikle 120 daha hızlı sorgu performans sağlar.


#### <a name="key-tables-and-columnstore-indexes"></a>Anahtar tabloları ve columnstore dizinleri


- dbo. FactResellerSalesXL_CCI sıkıştırma, Gelişmiş bir kümelenmiş columnstore dizini içeren bir tabloda olduğu *veri* düzeyi.

- dbo. FactResellerSalesXL_PageCompressed yalnızca sıkıştırılmış bir eşdeğer normal kümelenmiş dizine sahip olan bir tabloda olduğu *sayfa* düzeyi.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Anahtar sorguları, columnstore dizinini karşılaştırmak için


Vardır [çalıştırabileceğiniz T-SQL sorgusu türlerinden](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) performans iyileştirmeleri öğrenin. 2. adımda T-SQL betiğindeki sorgular bu değer çiftini dikkat edin. Bunlar, yalnızca tek bir satırda farklılık gösterir:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


İçinde FactResellerSalesXL kümelenmiş bir columnstore dizini olan\_CCI tablo.

Aşağıdaki T-SQL betiği alıntı, GÇ ve sorgunun her tablonun süresi istatistikleri yazdırır.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

P2 fiyatlandırma katmanı ile bir veritabanında geleneksel dizin ile karşılaştırıldığında kümelenmiş columnstore dizinini kullanarak, bu sorgu performansı kazancı yaklaşık dokuz kez bekleyebilirsiniz. P15 ile columnstore dizinini kullanarak, yaklaşık 57 kez performans kazancı bekleyebilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

- [1 Hızlı Başlangıç: T-SQL daha hızlı performans için bellek içi OLTP teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx)

- [Mevcut bir Azure SQL uygulamadaki bellek içi OLTP kullanın](sql-database-in-memory-oltp-migration.md)

- [İzleyici bellek içi OLTP depolama alanını](sql-database-in-memory-oltp-monitoring.md) için bellek içi OLTP


## <a name="additional-resources"></a>Ek kaynaklar

#### <a name="deeper-information"></a>Daha ayrıntılı bilgi

- [SQL veritabanında bellek içi OLTP ile 70 oranında DTU düşürürken çekirdek anahtar veritabanının iş yükü nasıl Katlar öğrenin](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [Azure SQL veritabanı Web günlüğü gönderisinde bellek içi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Bellek içi OLTP hakkında bilgi edinin](https://msdn.microsoft.com/library/dn133186.aspx)

- [Columnstore dizinleri hakkında bilgi edinin](https://msdn.microsoft.com/library/gg492088.aspx)

- [Gerçek zamanlı işlem analizi hakkında bilgi edinin](https://msdn.microsoft.com/library/dn817827.aspx)

- Bkz: [yaygın iş yükü düzenleri ve geçiş konuları](https://msdn.microsoft.com/library/dn673538.aspx) (burada bellek içi OLTP yaygın olarak sağlayan önemli ölçüde performans kazanımı iş yükü düzenleri açıklayan)

#### <a name="application-design"></a>Uygulama tasarımı

- [Bellek içi OLTP (bellek içi iyileştirme)](https://msdn.microsoft.com/library/dn133186.aspx)

- [Mevcut bir Azure SQL uygulamadaki bellek içi OLTP kullanın](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Araçlar

- [Azure portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
