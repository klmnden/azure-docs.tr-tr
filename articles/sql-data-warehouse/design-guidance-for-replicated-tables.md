---
title: "Tasarım çoğaltılmış tablolar - Azure SQL Data Warehouse için Kılavuzu | Microsoft Docs"
description: "Çoğaltılmış tablolar, Azure SQL Data Warehouse şemasında tasarlama için öneriler."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/23/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 413a9df6d224e53ba42313f6dc5e740710d418e3
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'da çoğaltılmış tablolar kullanmaya yönelik kılavuz tasarım
Bu makalede SQL Data Warehouse şemanızı çoğaltılmış tablolarda tasarlamak için öneriler sağlar. Veri taşıma ve sorgu karmaşıklık azaltarak sorgu performansını artırmak için bu önerileri kullanın.

> [!NOTE]
> Çoğaltılmış tablo özelliği şu anda genel önizlemede değil. Bazı davranışları farklılık gösterebilir.
> 

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, veri dağıtımı ve SQL Data Warehouse veri taşıma kavramlarına alışık olduğunuz varsayılır.  Daha fazla bilgi için bkz: [mimarisi](massively-parallel-processing-mpp-architecture.md) makalesi. 

Tablo Tasarımı bir parçası olarak, verilerinizi ve verilerin nasıl sorgulanır hakkında mümkün olduğunca anlayın.  Örneğin, aşağıdaki soruları göz önünde bulundurun:

- Tablo büyüklüğü nedir?   
- Tablo ne sıklıkla yenileniyor?   
- Bir veri ambarı olgu ve boyut tabloları var mı?   

## <a name="what-is-a-replicated-table"></a>Çoğaltılmış tablosu nedir?
Yinelenen tablo her işlem düğümü üzerinde erişilebilir tablo tam bir kopyasını sahiptir. Tablo çoğaltma JOIN veya toplama önce işlem düğümleri arasında veri aktarmak için gereksinimini ortadan kaldırır. Tablosunda birden çok kopya bulunduğundan tablo boyutunu sıkıştırılmış 2 GB'tan daha az olduğunda çoğaltılmış tablolarda en iyi çalışır.

Aşağıdaki diyagram, her işlem düğümü üzerinde erişilebilir olan bir çoğaltılmış tablo gösterir. SQL veri ambarı'nda çoğaltılmış tablo her işlem düğümü üzerinde bir dağıtım veritabanı tam olarak kopyalanır. 

![Yinelenmiş tablo](media/guidance-for-using-replicated-tables/replicated-table.png "yinelenmiş tablosu")  

İyi bir yıldız şemasının küçük boyut tabloları için tabloları iş çoğaltılan. Boyut tabloları genellikle depolamak ve birden çok kopyasını korumak için uygun yapan bir boyutu var. Boyutların müşteri adı ve adres ve ürün ayrıntıları gibi yavaş değişir açıklayıcı verileri depolar. Verileri yavaş değişen yapısı daha az çoğaltılmış tablo yeniden için yol gösterir. 

Çoğaltılan kullanmayı tablosundan:

- 2 GB'den az, satır sayısından bağımsız olarak disk üzerindeki tablo boyutudur. Bir tablonun boyutunu bulmak için kullanabileceğiniz [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) komutu: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- Tablo, aksi halde veri taşıma gerektirecek birleştirme kullanılır. Örneğin, birleşme sütunları aynı dağıtım sütun olmadığında bir birleştirme karma dağıtılmış tablolarda veri taşıma gerektirir. Karma dağıtılmış tablolardan birini küçükse, çoğaltılmış bir tablo göz önünde bulundurun. Bir birleştirme hepsini tabloda veri taşıma gerektirir. Çoğu durumda hepsini tablolar yerine çoğaltılmış tablolar kullanmanızı öneririz. 


Varolan bir dönüştürme dağıtılmış bir çoğaltılmış tabloya göz önünde bulundurun tablosundan:

- Sorgu, veri tüm işlem düğümlerine yayını kullanımı veri taşıma işlemleri planlar. BroadcastMoveOperation pahalıdır ve sorgu performansı yavaşlatır. Sorgu planında veri taşıma işlemleri görüntülemek için kullanın [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).
 
Çoğaltılmış tablolarda en iyi sorgu performansını değil verim zaman:

- Tablo sık ekleme, güncelleştirme ve silme işlemleri vardır. Bu veri işleme dili (DML) işlemleri yeniden çoğaltılmış tablo oluşturulmasını gerektirir. Yeniden derleme sık performans neden olabilir.
- Veri ambarı sık ölçeklendirilir. Veri ambarı ölçeklendirme yeniden oluşturur, işlem düğümleri sayısını değiştirir.
- Tablo için çok sayıda sütun olsa da, veri işlemleri genellikle az sayıda sütun erişim. Tüm tabloyu çoğaltmak yerine bu senaryoda, karma değerini daha fazla etkili tablo dağıtın ve ardından sık erişilen sütunlarda dizin oluşturma olabilir. Bir sorgu veri taşıma gerektirdiğinde, SQL Data Warehouse istenen sütunlarda yalnızca verileri taşır. 



## <a name="use-replicated-tables-with-simple-query-predicates"></a>Çoğaltılmış tablolar ile basit sorgu koşulları kullanın
Dağıtmak veya bir tablo çoğaltmak seçmeden önce tabloda çalıştırmayı planladığınız sorgu türleri düşünün. Mümkün olduğunda,

- Eşitlik veya eşitsizlik gibi basit sorgu koşulları içeren sorgular için çoğaltılmış tablolarda kullanın.
- DEĞİL gibi veya benzer gibi karmaşık bir sorgu koşulları içeren sorgular için Dağıtılmış tabloları kullanın.

CPU-yoğun sorguları en iyi iş tüm işlem düğümleri arasında dağıtıldığında gerçekleştirin. Örneğin, tablonun her satırında hesaplamaları çalıştırmak sorguları dağıtılmış tablolarda çoğaltılmış tablolar daha iyi gerçekleştirin. Çoğaltılmış bir tabloda, her işlem düğümü üzerinde tam kaydedildiği, çoğaltılmış bir tabloda bir CPU-yoğun sorgu karşı tüm tablo her işlem düğümünde çalışır. Ek hesaplama sorgu performansı düşürebilir.

Örneğin, bu sorgu, bir karmaşık koşuluna sahip.  Sağlayıcı bir çoğaltılmış tablo yerine dağıtılmış bir tablo olduğunda daha hızlı çalışır. Bu örnekte, hepsini dağıtılmış veya tedarikçi karma dağıtılmış olabilir.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Çoğaltılmış tablolar için varolan hepsini tabloları Dönüştür
Hepsini tablolar zaten varsa, bu makalede açıklanan ölçütlerle karşılıyorsa bunları çoğaltılmış tablolar dönüştürme öneririz. Veri taşıma gereksinimini ortadan kaldırdığı çoğaltılmış tablolar hepsini tablolar üzerindeki performansı.  Hepsini tablo, veri taşıma birleştirmeler için her zaman gerektirir. 

Bu örnekte [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) DimSalesTerritory tablo çoğaltılmış bir tabloya değiştirmek için. Bu örnek DimSalesTerritory karma dağıtılmış veya hepsini bağımsız olarak çalışır.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Sorgu performansı hepsini karşı örneğin çoğaltılan 

Tüm tablo her işlem düğümü üzerinde zaten varolduğundan çoğaltılmış tablo birleşimler için tüm veri hareketlerini gerektirmez. Boyut tabloları dağıtılmış hepsini varsa, bir birleştirme her işlem düğümü tam olarak Boyut tablosuna kopyalar. Verileri taşımak için sorgu planı BroadcastMoveOperation adlı bir işlem içerir. Bu tür veri taşıma işlemi sorgu performansını yavaşlatır ve çoğaltılmış tablolar kullanarak ortadan kalkar. Sorgu planı adımları görüntülemek için kullanın [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) sistem Katalog görünümü. 

Örneğin, sorgudaki aşağıdaki AdventureWorks şemayla ` FactInternetSales` tablo karma dağıtılmış. `DimDate` Ve `DimSalesTerritory` tablolardır daha küçük boyut tabloları. Bu sorgu, Kuzey Amerika'da mali yılın 2004 toplam satış döndürür:
 
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
Yeniden oluşturduğumuz `DimDate` ve `DimSalesTerritory` hepsini tabloları olarak. Sonuç olarak, sorguyu taşıma işlemlerini birden fazla yayını olan aşağıdaki sorgu planı gösterilmiştir: 
 
![Hepsini sorgu planı](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Yeniden oluşturduğumuz `DimDate` ve `DimSalesTerritory` olarak çoğaltılmış tablolar ve sorguyu tekrar çalıştırılmıştır. Sonuçta elde edilen sorgu planı daha kısadır ve mu değil sahip herhangi yayını taşır.

![Sorgu planı çoğaltılan](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Çoğaltılmış tabloları değiştirmek için başarım düşünceleri
SQL veri ambarı ana sürüm tablosunun tutarak çoğaltılmış tablo uygular. Her işlem düğümü üzerinde bir dağıtım veritabanı için ana sürüm kopyalar. Bir değişiklik olduğunda, SQL Data Warehouse ilk ana tablo güncelleştirir. Ardından her işlem düğümünde tabloların yeniden gerektirir. Bir yeniden oluşturma çoğaltılmış bir tablonun her işlem düğümü Tablo kopyalama ve dizinleri yeniden oluşturma içerir.

Sonra yeniden gereklidir:
- Veri yüklenen ya da değiştirilmiş
- Veri ambarı farklı bir ölçeklendirilir [hizmet düzeyi](performance-tiers.md#service-levels)
- Tablo tanımı güncelleştirildi

Yeniden sonra gerekli değildir:
- Duraklatma işlemi
- Sürdürme işlemi

Veri hemen değiştirildikten sonra yeniden gerçekleşmez. Bunun yerine, yeniden tablodan bir sorgu seçer ilk kez tetiklenir.  Tablodaki ilk select deyimi içinde çoğaltılmış tablosunu yeniden adımlardır.  Yeniden oluşturma sorgu içinde yapıldığından, ilk select deyiminde etkisini tablo boyutuna bağlı olarak önemli olabilir.  Birden çok çoğaltılmış tablolar yeniden gereken söz konusuysa, her kopya seri olarak deyimi içindeki adımları olarak yeniden oluşturulur.  Verileri korumak için özel bir kilit çoğaltılmış tablo yeniden sırasında tutarlılık tabloda alınır.  Kilit yeniden süresince tabloya tüm erişimi engeller. 

### <a name="use-indexes-conservatively"></a>Dizinleri ölçülü kullanın
Standart dizin oluşturma yöntemleri çoğaltılmış tablolar için geçerlidir. SQL veri ambarı her çoğaltılmış tablosu dizini yeniden oluşturma bir parçası olarak yeniden oluşturur. Performans kazancı dizinleri yeniden oluşturma, maliyetinden ağır yalnızca dizinler kullanın.  
 
### <a name="batch-data-loads"></a>Toplu veri yükler
Verileri çoğaltılmış tablolara yüklenirken yükleri birlikte toplu işleme tarafından yeniden küçültmeyi deneyin. Select deyimi çalıştırmadan önce tüm toplu iş yükleri gerçekleştirin.

Örneğin, bu yük düzeni dört kaynaktan verileri yükler ve dört yeniden çalıştırır. 

- 1 kaynağından yükleyin.
- SELECT deyimi Tetikleyicileri 1 yeniden oluşturun.
- 2 kaynağından yükleyin.
- SELECT deyimi Tetikleyicileri 2 yeniden oluşturun.
- 3 kaynağından yükleyin.
- SELECT deyimi Tetikleyicileri 3 yeniden oluşturun.
- 4 kaynağından yükleyin.
- SELECT deyimi Tetikleyicileri 4 yeniden oluşturun.

Örneğin, bu yük düzeni dört kaynaktan verileri yükler, ancak yalnızca bir yeniden çağırır.

- 1 kaynağından yükleyin.
- 2 kaynağından yükleyin.
- 3 kaynağından yükleyin.
- 4 kaynağından yükleyin.
- SELECT deyimi Tetikleyicileri yeniden oluşturun.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Toplu yükleme sonrasında çoğaltılmış tablo yeniden oluşturma
Tutarlı bir sorgu yürütme süreleri sağlamak için toplu yükleme sonrasında çoğaltılmış tablolar yenilenmesini zorlama öneririz. Aksi durumda, ilk sorgu tabloları yenilemek, dizinleri yeniden oluşturma içeren beklemeniz gerekir. Boyut ve etkilenen çoğaltılmış tablolar sayısına bağlı olarak, performans etkisi önemli olabilir.  

Bu sorgu kullanan [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV değiştirildi, ancak değil yeniden çoğaltılmış tablolarda listelenmektedir.

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
 
Bir yeniden oluşturma zorlamak için her bir tabloda yukarıdaki çıktıda aşağıdaki deyimini çalıştırın. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Sonraki adımlar 
Çoğaltılmış bir tablo oluşturmak için bu deyimleri birini kullanın:

- [TABLOSU (Azure SQL Data Warehouse) oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [TABLE AS SELECT (Azure SQL Data Warehouse) oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Dağıtılmış tabloları genel bakış için bkz: [dağıtılmış tabloları](sql-data-warehouse-tables-distribute.md).



