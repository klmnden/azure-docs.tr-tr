---
title: "Azure SQL veritabanı bellek içi teknolojileri | Microsoft Docs"
description: "Azure SQL veritabanı bellek içi teknolojileri analytics iş yükleri ve işlem performansını önemli ölçüde artırır."
services: sql-database
author: jodebrui
manager: craigg
ms.service: sql-database
ms.custom: develop databases
ms.topic: article
ms.date: 11/16/2017
ms.author: jodebrui
ms.openlocfilehash: 107df78f0ec6ce924785f5027958ee66f2a86c7c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>SQL veritabanı'nda Bellek içi teknolojileri kullanılarak performansı en iyi duruma getirme

Azure SQL veritabanı'nda Bellek içi teknolojilerini kullanarak, performans iyileştirmeleri çeşitli iş yükleri ile elde edebilirsiniz: işlem (çevrimiçi işlem işleme (OLTP)), analytics (çevrimiçi analitik işlem (OLAP)) ve karma (karma işlem/analitik işleme (HTAP)). Daha verimli sorgu ve işlem nedeniyle, bellek içi teknolojileri de maliyetini azaltmak için yardımcı olur. Genellikle performans artışı elde etmek için veritabanının fiyatlandırma katmanı yükseltme gerekmez. Bazı durumlarda, hatta yazdıramayabilirsiniz yine de performans iyileştirmeleri bellek içi teknolojileriyle görüyorsanız sırasında fiyatlandırma katmanı azaltın.

Aşağıda, bellek içi OLTP performansı önemli ölçüde artırmak için nasıl Yardım iki örnek verilmiştir:

- Bellek içi OLTP kullanarak [çekirdek işletme çözümleri % 70 Dtu'lar arttırırken, iş yükü çift mümkün](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU anlamına gelir *veritabanı işleme birimi*, ve kaynak tüketimi mesurement içerir.
- Aşağıdaki videoda bir örnek iş yükü kaynak tüketimi önemli gelişme gösterilmektedir: [Azure SQL veritabanı Video, bellek içi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Daha fazla bilgi için blog gönderisine bakın: [bellek içi OLTP Azure SQL veritabanı Blog gönderisine içinde](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

Bellek içi teknolojileri Premium katmanındaki Premium esnek havuzlarını veritabanları dahil olmak üzere tüm veritabanlarında kullanılabilir.

Aşağıdaki video Azure SQL veritabanında bellek içi teknolojileriyle olası performans artışı açıklanmaktadır. Her zaman gördüğünüz performans kazancı iş yükü ve verileri, veritabanı, erişim desenini yapısını dahil olmak üzere birçok faktöre bağlıdır unutmayın ve benzeri.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Azure SQL veritabanı bellek içi teknolojilerin sahiptir:

- *Bellek içi OLTP* verimliliğini artırır ve işlem için gecikme süresini azaltır. Bellek içi OLTP yararlanan senaryolar şunlardır: yüksek verimlilik işlem ticaret ve oyun, veri alımı olayları veya önbelleğe alma, veri yükü ve geçici bir tablo ve tablo değişkeni senaryoları IOT cihazları gibi işleme.
- *Kümelenmiş columnstore dizinleri* (en fazla 10 kez), depolama ayak izini azaltmak ve raporlama ve analiz sorguları performansını. Bu olgu tabloları ile veri reyonlarını daha fazla veri veritabanınızda sığacak ve performansı artırmak için kullanabilirsiniz. Ayrıca, bu geçmiş verileriyle işlemsel veritabanında arşivlemek ve en fazla 10 kez daha fazla veri sorgulayabilmesi için kullanabilirsiniz.
- *Kümelenmemiş columnstore dizinleri* HTAP yardımcı olmak için işletimsel veritabanının pahalı bir ayıklama çalıştırmaya gerek doğrudan sorgulama aracılığıyla işletmenizin gerçek zamanlı Öngörüler elde size dönüştürme ve yükleme (ETL) işlemi ve bekleyin veri ambarı'doldurulmalıdır. Kümelenmemiş columnstore dizinleri OLTP veritabanı üzerinde işlem iş yükü üzerindeki etkiyi azaltırken analitik sorguları çok hızlı yürütülmesi izin verin.
- Ayrıca, bir columnstore dizini olan bellek için iyileştirilmiş tablo birleşimi olabilir. Bu birleşim çok hızlı işlemler gerçekleştirmenizi sağlar ve *eşzamanlı olarak* analitik sorguları aynı verileri çok hızlı bir şekilde çalıştırın.

SQL Server ürün parçası 2012 ve 2014, bu yana columnstore dizinleri ve bellek içi OLTP sırasıyla olmuştur. Azure SQL Database ve SQL Server bellek içi teknolojilerin aynı uygulaması paylaşır. SQL Server'da yayımlanmadan önce ileride, bu teknolojiler için yeni özellikler Azure SQL veritabanı'nda ilk olarak yayımlanmıştır.

Bu konuda, Azure SQL veritabanına özel bellek içi OLTP ve columnstore dizinleri yönlerini açıklar ve ayrıca örnekleri içerir:
- Bu teknolojiler etkisini depolama ve veri boyutu sınırları görürsünüz.
- Bu teknolojiler farklı fiyatlandırma katmanları arasında kullanan veritabanları hareketini yönetme görürsünüz.
- Azure SQL veritabanında columnstore dizinleri yanı sıra, bellek içi OLTP kullanımını gösteren iki örnek görürsünüz.

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Teknolojileri hakkında ayrıntılı bilgi:

- [Bellek içi OLTP genel bakış ve kullanım senaryoları](https://msdn.microsoft.com/library/mt774593.aspx) (müşteri örnek olay incelemeleri ve başlamak için bilgi başvurular içerir)
- [Bellek içi OLTP için belgeler](http://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore dizinleri Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx)
- Karma işlem / (HTAP), olarak da bilinen analitik işleme [işletimsel gerçek zamanlı analiz](https://msdn.microsoft.com/library/dn817827.aspx)

Bellek içi OLTP hızlı öncü: [hızlı başlangıç 1: bellek içi OLTP teknolojileri daha hızlı T-SQL performansı](http://msdn.microsoft.com/library/mt694156.aspx) (yardımcı olması için başka bir makaleye başlama)

Teknolojileri hakkında ayrıntılı videolar:

- [Azure SQL veritabanında bellek içi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (performans avantajı ve bu sonuçları kendinize yeniden oluşturma adımları gösterimini içeren)
- [Bellek içi OLTP videolar: Nedir ve ne zaman/nasıl kullanmak için](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Columnstore dizini: Bellek içi Analytics videoların 2016 göz atın.](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Depolama ve veri boyutu

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Veri boyutu ve depolama cap bellek içi OLTP için

Bellek içi OLTP kullanıcı verilerini depolamak için kullanılan bellek için iyileştirilmiş tablolar içerir. Bu tablolar belleğe sığması için gereklidir. SQL veritabanı hizmetinin bellekte doğrudan yönetmek için biz kullanıcı verileri için bir kota kavramı vardır. Bu fikir olarak adlandırılır *bellek içi OLTP depolama*.

Belirli bir miktarda bellek içi OLTP depolama fiyatlandırma katmanı ve fiyatlandırma katmanı her esnek havuz her desteklenen tek başına veritabanı içerir. Yazma zaman, her 125 veritabanı işlem birimleri (Dtu'lar) veya esnek veritabanı işlem birimleri (Edtu'lar) için depolama gigabayt alırsınız. Daha fazla bilgi için bkz: [kaynak sınırları](sql-database-resource-limits.md).

Aşağıdaki öğeler, bellek içi OLTP depolama cap doğru sayısı:

- Bellek için iyileştirilmiş tablolar ve Tablo değişkenlerinin etkin kullanıcı veri satır. Eski satır sürümlerini doğru ucun sayılmaz unutmayın.
- Bellek için iyileştirilmiş tablolardaki dizinler.
- ALTER TABLE işlemlerin işlemsel yükünü.

Ucun isabet, kota hata iletisi ve artık ekleyemez veya verileri güncelleştirin. Bu hata etkisini azaltmak için verileri silmek veya havuz veya veritabanı fiyatlandırma katmanı artırın.

Bellek içi OLTP depolama alanı kullanımı izleme ve neredeyse ucun isabet olduğunda uyarıları yapılandırma hakkında daha fazla bilgi için bkz [monitör bellek içi depolama](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Esnek havuzları hakkında

Esnek havuzları ile bellek içi OLTP depolama havuzundaki tüm veritabanları arasında paylaşılır. Bu nedenle, bir veritabanında kullanım büyük olasılıkla diğer veritabanlarına etkileyebilir. Bu iki Azaltıcı Etkenler şunlardır:

- Bir en çok-bir bütün olarak havuz eDTU sayısı daha düşük olan eDTU veritabanları için yapılandırın. Bu maksimum bellek içi OLTP depolama alanı kullanımı herhangi bir veritabanı için eDTU sayısı karşılık gelen bir boyut havuzunda caps.
- 0'dan büyük bir Min-eDTU yapılandırın. Bu en az havuzdaki her veritabanı için yapılandırılmış en az eDTU karşılık gelen kullanılabilir bellek içi OLTP depolama alanı miktarı olduğunu güvence altına alır.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Veri boyutu ve columnstore dizinleri için depolama

Columnstore dizinleri belleğe sığması için gerekli değildir. Bu nedenle, yalnızca dizinleri boyutuna belgelenen en fazla genel veritabanı boyutu sınırıdır [SQL Database hizmet katmanlarına](sql-database-service-tiers.md) makalesi.

Kümelenmiş columnstore dizinleri kullandığınızda, sütunlu sıkıştırma temel tablo depolaması için kullanılır. Bu sıkıştırma depolama ayak izini veritabanında daha fazla veri sığabilecek anlamına gelir, verilerinizin kullanıcı önemli ölçüde azaltabilir. Sıkıştırma daha fazla ile artırılabilir [sütunlu arşiv sıkıştırma](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). Elde edebilirsiniz sıkıştırma veri yapısına bağlıdır, ancak 10 kez sıkıştırma seyrek değil.

Örneğin, 1 terabayt (TB) boyut sınırı olan bir veritabanı varsa ve columnstore dizinleri kullanarak 10 kez sıkıştırma elde size kullanıcı verilerini 10 TB toplam veritabanında uygun olamaz.

Kümelenmemiş columnstore dizinleri kullandığınızda, temel tablo hala geleneksel rowstore biçiminde depolanır. Bu nedenle, depolama tasarrufları kümelenmiş columnstore dizinleriyle kadar büyük değil. Ancak, bir tek columnstore dizini ile geleneksel kümelenmemiş dizin sayısı değiştiriyorsanız, genel bir tablo için depolama ayak izini tasarruf görebilirsiniz.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Fiyatlandırma katmanı arasındaki bellek içi teknolojileri kullanan veritabanlarını taşıma

Hiçbir zaman uyumsuzlukları ya da diğer sorunlar için daha yüksek bir fiyatlandırma katmanı, gibi standart Premium'a yükseltme, vardır. Kullanılabilir işlevler ve kaynakları yalnızca artırın.

Ancak, fiyatlandırma katmanı eski sürüme düşürmeyi veritabanınızı olumsuz yönde etkileyebilir. Veritabanı bellek içi OLTP nesneler içerdiğinde, Premium'dan standart ya da temel düşürmek olduğunda özellikle belirgin bir etkisidir. (Bunların görünür kalmasını olsa bile), sonra indirgeme bellek için iyileştirilmiş tablolar ve columnstore dizinleri kullanılamaz. Bir esnek havuzun fiyatlandırma katmanı düşürmeyi veya bir veritabanı bellek içi teknolojilerle birlikte, standart veya temel esnek havuz taşıma ilgili noktaların aynısı geçerlidir.

### <a name="in-memory-oltp"></a>Bellek içi OLTP

*Basic/standart eski sürüme düşürmeyi*: bellek içi OLTP veritabanlarında standart ya da temel katmanındaki desteklenmiyor. Ayrıca, tüm standart veya temel katmanı bellek içi OLTP nesnelere olan bir veritabanında taşınmasını mümkün değildir.

Verilen bir veritabanı bellek içi OLTP destekleyip desteklemediğini anlamak için programlı bir yolu yoktur. Aşağıdaki Transact-SQL sorgusu çalıştırabilirsiniz:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Sorgu döndürürse **1**, bellek içi OLTP bu veritabanında desteklenir.

Standart/temel veritabanına düşürmek önce tüm bellek için iyileştirilmiş tablolar ve tablo türleri yanı sıra, tüm yerel koda derlenmiş T-SQL modülleri kaldırın. Aşağıdaki sorgularda bir veritabanı standart/Basic alt sürüme önce kaldırılması gereken tüm nesneleri tanımlayın:

```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

*Daha düşük bir Premium katmanına eski sürüme düşürmeyi*: bellek için iyileştirilmiş tablolardaki verileri veritabanı fiyatlandırma katmanı ile ilişkili olduğundan veya esnek Havuzda kullanılabilir bellek içi OLTP depolama içinde sığması gerekir. Fiyatlandırma katmanı düşürmek deneyin veya yeterli kullanılabilir bellek içi OLTP depolama yok bir havuza veritabanı taşıma işlemi başarısız olur.

### <a name="columnstore-indexes"></a>Columnstore dizinleri

*Temel veya standart eski sürüme düşürmeyi*: Columnstore dizinleri yalnızca Premium fiyatlandırma katmanı ve standart ya da Basic katmanları üzerinde desteklenir. Veritabanınıza standart ya da temel düşürmek, columnstore dizini kullanılamaz duruma gelir. Columnstore dizini sistem korur ancak hiç dizini yararlanır. Daha sonra geri Premium yükseltirseniz, columnstore dizini yeniden işlevden hemen hazırdır.

Varsa bir **kümelenmiş** columnstore dizini, tüm tablo olur kullanılamaz katmanı indirgeme sonra. Bu nedenle tüm bırakma öneririz *kümelenmiş* veritabanınızı Premium katmanı aşağıda düşürmek önce columnstore dizinini oluşturur.

*Daha düşük bir Premium katmanına eski sürüme düşürmeyi*: tüm veritabanını esnek Havuzda kullanılabilir depolama alanı veya fiyatlandırma katmanı hedef için en büyük veritabanı boyutu içinde uyuyorsa bu indirgeme başarılı olur. Columnstore dizinleri gelen belirli üzerinde etkisi yoktur.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Bellek içi OLTP örneği yükleme

AdventureWorksLT örnek veritabanı içinde birkaç tıklama ile oluşturabileceğiniz [Azure portal](https://portal.azure.com/). Ardından, bu bölümdeki adımları nasıl AdventureWorksLT veritabanınızı bellek içi OLTP nesnelerle zenginleştirmek ve performans avantajı göstermek açıklanmaktadır.

Daha fazla simplistic, ancak daha görsel olarak çekici performans gösteri için bellek içi OLTP için bkz:

- Yayın: [içinde-bellek-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Kaynak kodu: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Yükleme adımları

1. İçinde [Azure portal](https://portal.azure.com/), bir sunucu üzerinde bir Premium veritabanı oluşturun. Ayarlama **kaynak** AdventureWorksLT örnek veritabanı. Ayrıntılı yönergeler için bkz: [ilk Azure SQL veritabanınızı oluşturma](sql-database-get-started-portal.md).

2. SQL Server Management Studio ile veritabanına bağlanmak [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopya [bellek içi OLTP Transact-SQL betiği](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) panonuza. T-SQL komut dosyası, 1. adımda oluşturduğunuz AdventureWorksLT örnek veritabanı gerekli bellek içi nesneleri oluşturur.

4. SSMS T-SQL betiğini yapıştırın ve betiği yürütün. `MEMORY_OPTIMIZED = ON` Yan tümcesi CREATE TABLE deyimleri önemli. Örneğin:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Hata 40536


T-SQL komut dosyasını çalıştırdığınızda 40536 hata alırsanız, veritabanı bellek içi destekleyip desteklemediğini doğrulamak için aşağıdaki T-SQL betiğini çalıştırın:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Sonucunu **0** , bellek içi desteklenmez, anlamına gelir ve **1** desteklenip desteklenmediğini gösterir. Sorunu tanılamak için veritabanının Premium Hizmet katmanını olduğundan emin olun.


#### <a name="about-the-created-memory-optimized-items"></a>Oluşturulan bellek için iyileştirilmiş öğeleri hakkında

**Tablolar**: örnek aşağıdaki bellek için iyileştirilmiş tablolarda içerir:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Bellek için iyileştirilmiş tablolar aracılığıyla inceleyebilirsiniz **Object Explorer** SSMS içinde. Sağ **tabloları** > **filtre** > **filtre ayarları** > **bellek için iyileştirilmiş**. Değer 1'e eşittir.


Veya, Katalog görünümleri gibi sorgulayabilirsiniz:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Saklı yordam yerel koda derlenmiş**: SalesLT.usp_InsertSalesOrder_inmem Katalog görünümü sorgu ile inceleyebilirsiniz:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>Örnek OLTP iş yükü çalıştırın

Aşağıdaki iki arasındaki tek fark *saklı yordamlar* olan bellek için iyileştirilmiş tablolar sürümlerini ilk yordamı kullanır, ikinci yordam normal disk üzerinde tabloları kullanır:

- SalesLT**.**usp_InsertSalesOrder**_inmem**
- SalesLT**.**usp_InsertSalesOrder**_ondisk**


Bu bölümde, kullanışlı kullanılması hakkında bilgi **ostress.exe** gerilimli düzeylerinde iki saklı yordamı yürütmek için yardımcı programı. Tamamlamak iki stres çalıştırmaları için gereken süreyi karşılaştırabilirsiniz.


Ostress.exe çalıştırdığınızda, her iki aşağıdakiler için tasarlanmış parametre değerlerinin geçmesini öneririz:

- Çok sayıda eşzamanlı bağlantı çalıştırmak kullanarak - n100.
- Yüzlerce kez, her bağlantı döngü göre sahip kullanma - r500.


Bununla birlikte, her şeyin çalıştığından emin olmak - n10 ve - r50 gibi çok daha küçük değerlerle Başlat isteyebilirsiniz.


### <a name="script-for-ostressexe"></a>Ostress.exe için komut dosyası


Bu bölümde bizim ostress.exe komut satırında katıştırılmış T-SQL betiği görüntüler. Komut dosyasını daha önce yüklediğiniz T-SQL betiği tarafından oluşturulan öğeleri kullanır.


Aşağıdaki komut dosyası bellek için iyileştirilmiş aşağıdaki beş satır öğelerini örnek satış siparişle ekler *tabloları*:

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


Yapmak için *_ondisk* ostress.exe için yukarıdaki T-SQL komut dosyası sürümünü, her iki oluşumları değiştirmek *_inmem* ile alt dize *_ondisk*. Bu değişiklik, tabloları ve saklı yordamlar adları etkiler.


### <a name="install-rml-utilities-and-ostress"></a>RML yardımcı programları ve ostress yükleyin


İdeal olarak, Azure sanal makine (VM) ostress.exe çalıştırmayı planladığınız. Oluşturacak bir [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) AdventureWorksLT veritabanınızın bulunduğu aynı Azure coğrafi bölgede. Ancak bunun yerine dizüstü bilgisayarınızda ostress.exe çalıştırabilirsiniz.


VM veya ne olursa olsun, ana bilgisayar seçin, yeniden yürütme işaretleme dili (RML) yardımcı programlarını yükleyin. Yardımcı programlar ostress.exe içerir.

Daha fazla bilgi için bkz.
- Ostress.exe tartışmada [örnek veritabanı bellek içi OLTP için](http://msdn.microsoft.com/library/mt465764.aspx).
- [Örnek veritabanı için bellek içi OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- [Ostress.exe yüklemek için blog](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Çalıştırma *_inmem* ilk iş yükü stres


Kullanabileceğiniz bir *RML Cmd komut istemi* bizim ostress.exe komut satırını çalıştırmak için penceresi. Komut satırı parametreleri için ostress doğrudan:

- 100 bağlantılara eşzamanlı olarak çalıştırın (-n100).
- Her bağlantınız 50 kez T-SQL betiği çalıştırın (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Önceki ostress.exe komut satırını çalıştırmak için:


1. Veritabanı veri içeriği SSMS, tüm önceki çalışmalarını tarafından eklenen tüm verileri silmek için aşağıdaki komutu çalıştırarak sıfırlama:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Önceki ostress.exe komut satırını metnin panonuza kopyalayın.

3. Değiştir `<placeholders>` parametreleri -S - U -P -d doğru gerçek değerlerle için.

4. Düzenlenen komut satırında bir RML Cmd penceresinde çalıştırın.


#### <a name="result-is-a-duration"></a>Bir süre oluşur


Ostress.exe sona erdiğinde, RML Cmd penceresinde çalışma süresi da son satırı çıktı olarak yazar. Örneğin, daha kısa bir test çalıştırması yaklaşık 1,5 dakika devam:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Sıfırlama, düzenlemek *_ondisk*, sonra yeniden çalıştırın


Sonuç elde ettikten sonra *_inmem* çalıştırmak, için aşağıdaki adımları gerçekleştirin *_ondisk* çalıştırın:


1. Veritabanı önceki çalışması tarafından eklenen tüm verileri silmek için SSMS aşağıdaki komutu çalıştırarak sıfırlama:
```
EXECUTE Demo.usp_DemoReset;
```

2. Tüm değiştirmek için ostress.exe komut satırı düzenleyin *_inmem* ile *_ondisk*.

3. Ostress.exe ikinci kez yeniden çalıştırın ve süre sonuç yakalayın.

4. Yeniden (sorumlu bir şekilde ne test verilerinin büyük bir miktarını olabilir silme) veritabanı sıfırlayın.


#### <a name="expected-comparison-results"></a>Beklenen Karşılaştırma sonuçları

Bellek içi testlerimizde tarafından geliştirilmiş performans göstermiştir **dokuz kez** veritabanı ile aynı Azure bölgesinde bir Azure VM üzerinde çalışan ostress ile simplistic bu iş yükü için.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Bellek içi analizi örneği yükleme


Bu bölümde, bir columnstore dizini bir geleneksel b-ağacı dizini karşı kullanırken GÇ ve İstatistikler sonuçlarını karşılaştırın.


Bir OLTP iş yükü için gerçek zamanlı analiz almak için kümelenmemiş bir columnstore dizini kullanmak idealdir. Ayrıntılar için bkz [Columnstore dizinleri açıklanan](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Columnstore analytics test hazırlama


1. Yeni bir AdventureWorksLT veritabanı örnekten oluşturmak için Azure portalını kullanın.
 - Bu tam ad kullanın.
 - Tüm Premium Hizmet katmanını seçin.

2. Kopya [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) panonuza.
 - T-SQL komut dosyası, 1. adımda oluşturduğunuz AdventureWorksLT örnek veritabanı gerekli bellek içi nesneleri oluşturur.
 - Komut dosyası Boyut tablosuna ve iki olgu tabloları oluşturur. Olgu tabloları 3.5 milyon satır her doldurulur.
 - Komut dosyasının tamamlanması 15 dakika sürebilir.

3. SSMS T-SQL betiğini yapıştırın ve betiği yürütün. **COLUMNSTORE** anahtar sözcük **CREATE INDEX** açıklamadır önemli olarak:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT 130 uyumluluk düzeyi ayarlayın:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Düzeyi 130 doğrudan bellek içi özelliklerle ilgili değildir. Ancak düzeyi 130 genellikle 120'den daha hızlı sorgu performansı sağlar.


#### <a name="key-tables-and-columnstore-indexes"></a>Anahtar tablolar ve columnstore dizinleri


- dbo. FactResellerSalesXL_CCI sıkıştırma, Gelişmiş bir kümelenmiş columnstore dizini içeren bir tablo olduğundan *veri* düzeyi.

- dbo. FactResellerSalesXL_PageCompressed yalnızca sıkıştırılmış bir eşdeğer normal kümelenmiş dizin içeren bir tablo olduğundan *sayfa* düzeyi.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Columnstore dizinini karşılaştırmak için anahtar sorguları


Vardır [çalıştırabileceğiniz birkaç T-SQL sorgu türleri](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) performans iyileştirmeleri görmek için. Adım 2'de T-SQL komut dosyası, bu sorguları çiftinin dikkat edin. Bunlar yalnızca tek bir satırda farklılık gösterir:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


FactResellerSalesXL kümelenmiş columnstore dizini olan\_CCI tablo.

Aşağıdaki T-SQL komut dosyası Alıntısı istatistikleri GÇ ve her tablo için sorgu SÜREDİR yazdırır.


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

P2 fiyatlandırma katmanı ile bir veritabanında, geleneksel dizin ile karşılaştırıldığında kümelenmiş columnstore dizini kullanarak bu sorgu performansı kazancı yaklaşık dokuz kez bekleyebilirsiniz. P15 ile columnstore dizinini kullanarak hakkında 57 kez performans kazancı bekleyebilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

- [1 Hızlı Başlangıç: Daha hızlı bir T-SQL performans için bellek içi OLTP teknolojileri](http://msdn.microsoft.com/library/mt694156.aspx)

- [Mevcut Azure SQL uygulamada kullanmak bellek içi OLTP](sql-database-in-memory-oltp-migration.md)

- [İzleyici bellek içi OLTP depolama](sql-database-in-memory-oltp-monitoring.md) bellek içi OLTP için


## <a name="additional-resources"></a>Ek kaynaklar

#### <a name="deeper-information"></a>Daha ayrıntılı bilgi

- [Bellek içi OLTP SQL veritabanında ile % 70 tarafından DTU düşürürken çekirdek anahtar veritabanının iş yükü nasıl Katlar öğrenin](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [Bellek içi OLTP de Azure SQL veritabanı Blog Gönderisi](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Bellek içi OLTP hakkında bilgi edinin](http://msdn.microsoft.com/library/dn133186.aspx)

- [Columnstore dizinleri hakkında bilgi edinin](https://msdn.microsoft.com/library/gg492088.aspx)

- [Gerçek zamanlı işletimsel analytics hakkında bilgi edinin](http://msdn.microsoft.com/library/dn817827.aspx)

- Bkz: [yaygın iş yükü desenleri ve geçiş konuları](http://msdn.microsoft.com/library/dn673538.aspx) (burada bellek içi OLTP sık sağlar önemli performans artışları yükünün desenleri açıklayan)

#### <a name="application-design"></a>Uygulama tasarımı

- [Bellek içi OLTP (bellek içi iyileştirme)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Mevcut Azure SQL uygulamada kullanmak bellek içi OLTP](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Araçlar

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Veri Araçları (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)
