---
title: Azure SQL veritabanında bellek içi örneği | Microsoft Docs
description: Azure SQL veritabanında bellek içi OLTP ve columnstore örnek teknolojilerle çalışın.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 2aa98c3958f1dffeb8adbad5e91a11f397d4a9fd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035771"
---
# <a name="in-memory-sample"></a>Bellek içi örnek

Azure SQL veritabanında bellek içi teknolojileri, uygulamanızın performansını sağlar ve potansiyel olarak veritabanı maliyetini azaltın. Azure SQL veritabanında bellek içi teknolojileri kullanarak, çeşitli iş yükleriyle performans iyileştirmeleri elde edebilirsiniz.

Bu makalede, Azure SQL veritabanında columnstore dizinleri yanı sıra, bellek içi OLTP kullanımını gösteren iki örnek görürsünüz.

Daha fazla bilgi için bkz.
- [Bellek içi OLTP genel bakış ve kullanım senaryoları](https://msdn.microsoft.com/library/mt774593.aspx) (müşteri örnek olay incelemeleri ve kullanmaya başlamak için bilgi başvurular içerir)
- [Bellek içi OLTP için belgeleri](https://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore dizinleri Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx)
- Karma işlemsel / (HTAP), olarak da bilinen analitik işleme [gerçek zamanlı işlem analizi](https://msdn.microsoft.com/library/dn817827.aspx)

<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Bellek içi OLTP örnek yükleyin

AdventureWorksLT örnek veritabanı içinde birkaç tıklamayla oluşturabilirsiniz [Azure portalında](https://portal.azure.com/). Ardından, bu bölümdeki adımları nasıl AdventureWorksLT veritabanınızı bellek içi OLTP nesnelerle zenginleştirin ve performans avantajlarının göstermek açıklanmaktadır.

Daha fazla alıyormuş, ancak daha görsel olarak çekici performans gösteri için bellek içi OLTP için bkz:

- Yayın: [içinde-bellek-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Kaynak kodu: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Yükleme adımları

1. İçinde [Azure portalında](https://portal.azure.com/), bir sunucuda Premium veya iş açısından kritik bir veritabanı oluşturun. Ayarlama **kaynak** AdventureWorksLT örnek veritabanı. Ayrıntılı yönergeler için bkz. [ilk Azure SQL veritabanınızı oluşturma](sql-database-single-database-get-started.md).

2. SQL Server Management Studio ile veritabanına bağlanma [(SSMS.exe)](https://msdn.microsoft.com/library/mt238290.aspx).

3. Kopyalama [bellek içi OLTP Transact-SQL betiği](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) panonuza. T-SQL betiği, 1. adımda oluşturduğunuz AdventureWorksLT örnek veritabanını gerekli bellek içi nesneleri oluşturur.

4. SSMS ile T-SQL betiğini yapıştırın ve ardından betiği yürütün. `MEMORY_OPTIMIZED = ON` Yan tümcesi CREATE TABLE deyimleri çok önemlidir. Örneğin:


```sql
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Hata 40536


T-SQL komut dosyasını çalıştırırken hata 40536 alırsanız, veritabanı, bellek içi destekleyip desteklemediğini doğrulamak için aşağıdaki T-SQL betiğini çalıştırın:


```sql
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Sonucu **0** , bellek içi desteklenmiyor anlamına gelir ve **1** desteklenip desteklenmediğini anlamına gelir. Sorunu tanılamak için veritabanı Premium hizmet katmanında olduğundan emin olun.


#### <a name="about-the-created-memory-optimized-items"></a>Oluşturulan bellek için iyileştirilmiş öğeleri hakkında

**Tabloları**: Örnek aşağıdaki bellek için iyileştirilmiş tablolarda içerir:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Bellek için iyileştirilmiş tablolar üzerinden inceleyebilirsiniz **Nesne Gezgini** ssms'de. Sağ **tabloları** > **filtre** > **filtre ayarları** > **bellek için iyileştirilmiş**. Değer 1'e eşittir.


Veya, Katalog görünümleri gibi sorgulayabilirsiniz:


```sql
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Saklı yordam doğal olarak derlenen**: Bir katalog sorguyu görüntüle SalesLT.usp_InsertSalesOrder_inmem inceleyebilirsiniz:


```sql
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


```sql
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


### <a name="install-rml-utilities-and-ostress"></a>RML yardımcı programlarını yükleyin ve `ostress`


İdeal olarak, bir Azure sanal makinesinde (VM) ostress.exe çalıştırmayı planladığınız. Oluşturmak bir [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) AdventureWorksLT veritabanınızın bulunduğu aynı Azure bölgesinde coğrafi. Ancak, bunun yerine dizüstü bilgisayarınızda ostress.exe çalıştırabilirsiniz.


VM, konak üzerinde veya seçin, yeniden yürütme işaretleme dili (RML) yardımcı programlarını yükleyin. Yardımcı programları ostress.exe içerir.

Daha fazla bilgi için bkz.
- Ostress.exe tartışmalarına [örnek veritabanı için bellek içi OLTP](https://msdn.microsoft.com/library/mt465764.aspx).
- [Örnek veritabanı için bellek içi OLTP](https://msdn.microsoft.com/library/mt465764.aspx).
- [Ostress.exe yüklemek için blog](https://blogs.msdn.com/b/psssql/archive/20../../cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(https://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(https://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Çalıştırma *_inmem* ilk iş yükü stres


Kullanabileceğiniz bir *RML Cmd komut istemi* bizim ostress.exe komut satırını çalıştırmak için pencere. Komut satırı parametreleri doğrudan `ostress` için:

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


Zaman `ostress.exe` RML Cmd penceresinde çalıştırma süresi, son satırı çıkış olarak yazar bittiğinde. Örneğin, daha kısa bir test çalıştırması yaklaşık 1,5 dakika görüşmelerin Süren:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Sıfırlama, düzenlemek *_ondisk*, sonra yeniden çalıştırın


Sonuçtan sonra *_inmem* çalıştırın, için aşağıdaki adımları gerçekleştirin *_ondisk* çalıştırın:


1. Ssms'de önceki çalışması tarafından eklenen tüm verileri silmek için aşağıdaki komutu çalıştırarak veritabanını sıfırlama:
   ```sql
   EXECUTE Demo.usp_DemoReset;
   ```

2. Tüm değiştirmek için ostress.exe komut satırı Düzenle *_inmem* ile *_ondisk*.

3. İkinci kez ostress.exe yeniden çalıştırın ve süresi sonucu yakalayın.

4. Yeniden (depoladığımız ne miktarda test verisi olabilir silme için) veritabanı sıfırlayın.


#### <a name="expected-comparison-results"></a>Beklenen Karşılaştırma sonuçları

Bellek içi testlerimizin tarafından geliştirilmiş performans gösterilmesini **dokuz kez** alıyormuş bu iş yükü ile `ostress` veritabanı ile aynı Azure bölgesinde Azure sanal makinesinde çalışan.

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


```sql
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

- [Hızlı Başlangıç 1: T-SQL daha hızlı performans için bellek içi OLTP teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx)

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
