---
title: Tasarım kılavuzunu çoğaltılmış tablolar - Azure SQL veri ambarı için | Microsoft Docs
description: Öneriler tasarlamak için Azure SQL veri ambarı şema tabloları çoğaltılmış. 
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/19/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: acea42f7f4ab986e9828000ab7cfc9e302ed92a3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885466"
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Tasarım Kılavuzu, Azure SQL veri ambarı'nda çoğaltılmış tablolar'ı kullanma
Bu makalede, SQL veri ambarı şema çoğaltılmış tablolarda tasarlamaya yönelik öneriler sunar. Veri taşıma ve sorgu karmaşıklığı azaltarak sorgu performansını artırmak için bu önerileri kullanın.

> [!VIDEO https://www.youtube.com/embed/1VS_F37GI9U]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, veri dağıtımı ve SQL veri ambarı'nda veri taşıma kavramlarını bildiğiniz varsayılmıştır.  Daha fazla bilgi için [mimarisi](massively-parallel-processing-mpp-architecture.md) makalesi. 

Tablo Tasarımı işleminin bir parçası olarak, verileriniz ve verileri nasıl sorgulanır hakkında mümkün olduğunca anlayın.  Örneğin, aşağıdaki soruları göz önünde bulundurun:

- Tablo ne kadar büyük?   
- Tablo ne sıklıkla yenilenir?   
- Olgu ve boyut tabloları bir veri ambarı'nda yok mu?   

## <a name="what-is-a-replicated-table"></a>Çoğaltılmış bir tabloda nedir?
Çoğaltılmış bir tabloda, her işlem düğümü üzerinde erişilebilir tablo tam bir kopyasını sahiptir. Tablo çoğaltma JOIN veya toplama önce işlem düğümleri arasında veri aktarımı ihtiyacını ortadan kaldırır. Tabloda birden fazla kopya olduğundan, çoğaltılmış tablolar tablo boyutu sıkıştırılmış 2 GB'tan daha az olduğunda en iyi çalışır.  2 GB sabit bir sınır değil.  Veriler statiktir ve değişmeyen, daha büyük tablolar çoğaltabilirsiniz.

Aşağıdaki diyagramda, her işlem düğümü üzerinde erişilebilir çoğaltılmış bir tabloda gösterilmiştir. SQL veri ambarı'nda, çoğaltılmış tablo, her işlem düğümünde bir dağıtım veritabanı için tam olarak kopyalanır. 

![Çoğaltılan tablo](media/guidance-for-using-replicated-tables/replicated-table.png "çoğaltılan tablo")  

Tablolarda çalıştığı için bir yıldız şeması boyut tablolarındaki çoğaltılır. Boyut tabloları genellikle Boyut tablosuna farklı dağıtılan olgu tabloları için birleştirilir.  Genellikle depolamak ve birden fazla kopyasını korumak için uygun yapan bir boyutu boyutlarıdır. Boyutların müşteri adı ve adresi ve ürün ayrıntıları gibi yavaş değişen açıklayıcı verileri depolayın. Yavaş değişen veriler yapısını çoğaltılmış tablo daha az bakım için yol açar. 

Çoğaltılan kullanmayı tablosundan:

- Disk üzerinde Tablo 2 GB'tan daha az, satır sayısı ne olursa olsun boyutudur. Bir tablonun boyutunu bulmak için kullanabileceğiniz [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) komutu: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- Tablo, aksi takdirde veri taşıma gerektirecek birleştirme kullanılır. Hepsini bir kez deneme tabloya bir karma dağıtılmış tablo gibi aynı sütunda dağıtılmaz tabloları birleştirme, veri taşıma sorguyu tamamlamak için gereklidir.  Tablolardan birinin küçükse, çoğaltılmış bir tabloda göz önünde bulundurun. Çoğu durumda hepsini bir kez deneme tabloları yerine çoğaltılmış tablolar'ı kullanmanızı öneririz. Sorgu planında veri taşıma işlemleri görüntülemek için kullanın [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).  BroadcastMoveOperation çoğaltılmış bir tabloda kullanılarak kaldırılabilir tipik veri taşıma işlemi var.  
 
Çoğaltılmış tablolar değil en iyi sorgu performansını verebilir olduğunda:

- Tabloda, sık sık ekleme, güncelleştirme ve silme işlemleri vardır. Bu veri işleme dili (DML) işlemleri, çoğaltılmış tablo yeniden gerektirir. Yeniden oluşturma, sık sık daha yavaş performans neden olabilir.
- Veri ambarı sık ölçeklendirilir. Veri ambarı ölçeklendirme çoğaltılmış tablo yeniden doğurur işlem düğümlerinin sayısını değiştirir.
- Tablosunda sütun çok sayıda, ancak veri işlemleri genellikle yalnızca az sayıda sütun erişim. Tüm tabloyu çoğaltmak yerine bu senaryoda, tablo dağıtın ve ardından sık erişilen sütunlarda dizin oluşturma daha etkili olabilir. SQL veri ambarı, yalnızca bir sorgu, veri taşıma gerektirdiğinde, istenen sütunlar için veri taşır. 

## <a name="use-replicated-tables-with-simple-query-predicates"></a>Çoğaltılmış tablolar ile basit sorgu koşulları kullanın.
Tabloya karşı çalıştırmayı planladığınız sorgu türleri, dağıtmak veya bir tablo çoğaltma seçmeden önce düşünün. Mümkün olduğunda,

- Çoğaltılmış tablolar eşitlik ve eşitsizlik gibi basit sorgu koşullarda içeren sorgular için kullanın.
- DEĞİL gibi veya dağıtılmış tablolar gibi gibi karmaşık sorgu koşullarla sorgular için kullanın.

İş işlem düğümlerinin tamamında yeniden dağıtıldığında CPU yoğunluklu sorguları en iyi şekilde çalışır. Örneğin, tablonun her satırında üzerinde hesaplamalar çalışan sorguları daha çoğaltılmış tablolar dağıtılmış tablolar üzerinde daha iyi gerçekleştirin. Çoğaltılmış bir tabloda, her işlem düğümünde tam depolandığından, çoğaltılmış bir tabloda CPU-yoğun sorgu karşı tüm tablo her işlem düğümünde çalışır. Ek hesaplama sorgu performansını yavaşlatabilir.

Örneğin, bu sorgu, karmaşık bir koşul vardır.  Çoğaltılmış bir tabloda yerine dağıtılmış bir tablodaki verileri olduğunda daha hızlı çalışır. Bu örnekte, veriler hepsini dağıtılmış olabilir.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Çoğaltılmış tablolar için mevcut hepsini bir kez deneme tabloları Dönüştür
Hepsini bir kez deneme tabloları zaten varsa, bunlar bu makalede ana hatlarıyla belirtilen ölçütleri karşıladığı durumlarda bunları çoğaltılmış tablolar için dönüştürmenizi öneririz. Bunlar veri taşıma gereksinimini ortadan kaldırdığı için çoğaltılmış tablolar hepsini bir kez deneme tabloları performansını iyileştirin.  Hepsini bir kez deneme tablo, veri taşıma birleştirmeler için her zaman gerektirir. 

Bu örnekte [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) DimSalesTerritory tablosu için çoğaltılmış bir tabloda değiştirmek için. Bu örnek, DimSalesTerritory karma dağıtılmış veya hepsini bir kez deneme bağımsız olarak çalışır.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Sorgu performansı örneğin hepsini karşı çoğaltılan 

Tüm tablo her işlem düğümünde zaten varolduğundan çoğaltılmış bir tabloda birleştirme için hiçbir veri taşıma gerektirmez. Boyut tabloları hepsini dağıtılmış ise birleştirme her işlem düğümüne yüklenecek tam olarak Boyut tablosuna kopyalar. Verileri taşımak için sorgu planı BroadcastMoveOperation olarak adlandırılan bir işlem içerir. Bu veri taşıma işlem türü, sorgu performansı yavaşlatır ve çoğaltılmış tablolar'ı kullanarak ortadan kalkar. Sorgu planı adımları görüntülemek için kullanın [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) sistem Katalog görünümü. 

Örneğin, aşağıdaki sorguyu AdventureWorks şemayla içinde `FactInternetSales` tablo karma dağıtılmış. `DimDate` Ve `DimSalesTerritory` daha küçük boyut tabloları tablolarıdır. Bu sorgu, Kuzey Amerika'da 2004 mali yıl için toplam satış döndürür:

```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Yeniden oluşturduğumuz `DimDate` ve `DimSalesTerritory` hepsini bir kez deneme tabloları olarak. Sonuç olarak, sorguyu taşıma işlemlerini birden fazla yayını olan aşağıdaki sorgu planı gösterilmiştir: 
 
![Hepsini bir kez deneme sorgu planı](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Yeniden oluşturduğumuz `DimDate` ve `DimSalesTerritory` olarak çoğaltılmış tablolar ve sorguyu tekrar çalıştırılmıştır. Sonuçta elde edilen sorgu planı çok kısadır ve mu değil sahip tüm yayın taşır.

![Sorgu planı çoğaltılan](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Çoğaltılmış tablolar değiştirmek için performans konuları
SQL veri ambarı, çoğaltılmış bir tabloda bir ana sürüm tablosunun tutarak uygular. Ana sürüm her işlem düğümünde bir dağıtım veritabanı kopyalanır. Bir değişiklik olduğunda, SQL veri ambarı, ilk ana tablo güncelleştirir. Ardından her işlem düğümünde tablolar oluşturur. Her işlem düğümüne yüklenecek tabloyu kopyalama ve sonra dizinleri oluşturma çoğaltılmış bir tabloda yeniden içerir.  Örneğin, bir DW400 çoğaltılmış bir tabloda veri 5 kopya vardır.  Bir ana kopya ve her işlem düğümünde tam bir kopyası.  Tüm verileri dağıtım veritabanlarında depolanır. SQL veri ambarı, veri değişikliği deyimleri daha hızlı ve esnek ölçeklendirme işlemleri desteklemek için bu modeli kullanır. 

Sonra yeniden oluşturulması gereklidir:
- Veri yüklenen veya değiştirildiğinde
- Veri ambarı, farklı bir düzeye ölçeklendirilir
- Tablo tanımı güncelleştirildi

Sonra yeniden oluşturulması gerekmez:
- Duraklatma işlemi
- İşlemi sürdürme

Veri hemen değiştirildikten sonra yeniden oluşmaz. Bunun yerine, yeniden ilk kez bir sorgu tablodan seçtiğinde tetiklenir.  Her bir işlem düğümünde verileri zaman uyumsuz olarak kopyalanırken yeniden harekete geçirilen sorgu tablonun ana sürümünden hemen okur. Veri kopyalama işlemi tamamlanana kadar sonraki sorgular tablonun ana sürümünü kullanmaya devam eder.  Herhangi bir etkinlik başka bir yeniden derleme zorlar çoğaltılmış tablo karşı olursa, veri kopyalama geçersiz kılınır ve sonraki select deyimi yeniden kopyalanacak verileri tetikler. 

### <a name="use-indexes-conservatively"></a>Dizinleri ölçülü kullanın
Çoğaltılmış tablolar için standart bir dizin oluşturma yöntemleri uygulayın. SQL veri ambarı yeniden bir parçası olarak her çoğaltılmış tablo dizin oluşturur. Performans kazancı dizinlerini yeniden oluşturma, maliyetinden ağır yalnızca dizinleri kullanın.  
 
### <a name="batch-data-loads"></a>Toplu veri yüklerini
Çoğaltılmış tablolar veri yükleme yükleri birlikte toplu işleme tarafından yeniden oluşturulması en aza indirmek deneyin. Tüm toplu iş yükleri, select deyimi çalıştırmadan önce gerçekleştirin.

Örneğin, bu yük düzeni dört kaynaktan verileri yükler ve dört yeniden başlatır. 

- 1. kaynaktan yükleyin.
- SELECT deyimi Tetikleyicileri 1 yeniden oluşturun.
- 2 kaynaktan yükleyin.
- SELECT deyimi Tetikleyicileri 2 yeniden oluşturun.
- 3. kaynaktan yükleyin.
- SELECT deyimi Tetikleyicileri 3 yeniden oluşturun.
- 4. kaynaktan yükleyin.
- SELECT deyimi Tetikleyicileri 4 yeniden oluşturun.

Örneğin, bu yük düzeni, dört kaynaktan verileri yükler, ancak yalnızca bir yeniden başlatır.

- 1. kaynaktan yükleyin.
- 2 kaynaktan yükleyin.
- 3. kaynaktan yükleyin.
- 4. kaynaktan yükleyin.
- SELECT deyimi Tetikleyicileri yeniden oluşturun.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Çoğaltılmış bir tabloda bir toplu yükleme işleminden sonra yeniden oluşturun.
Tutarlı sorgu yürütme süreleri sağlamak için toplu yükleme sonrasında çoğaltılmış tablolar yapısını zorlama göz önünde bulundurun. Aksi takdirde, ilk sorgu sorguyu tamamlamak için yine de veri taşıma kullanır. 

Bu sorgu kullanan [sys.pdw_replicated_table_cache_state](/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV'sini değiştirilmiş olan ancak yeniden değil çoğaltılmış tabloları listeleyin.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
Yeniden derleme tetiklemek için yukarıdaki çıktıda her bir tabloda aşağıdaki deyimi çalıştırın. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Sonraki adımlar 
Çoğaltılmış tablo oluşturmak için bu deyimler birini kullanın:

- [Tablo (Azure SQL veri ambarı) oluşturma](/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [Tablo AS SELECT (Azure SQL veri ambarı) oluşturma](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Dağıtılmış tablolar genel bakış için bkz. [dağıtılmış tablolar](sql-data-warehouse-tables-distribute.md).



