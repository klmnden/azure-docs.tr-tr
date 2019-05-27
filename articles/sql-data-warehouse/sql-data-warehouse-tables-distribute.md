---
title: Dağıtılmış tablolar Tasarım Kılavuzu - Azure SQL veri ambarı | Microsoft Docs
description: Azure SQL Data warehouse'da dağıtılmış karma dağıtılmış ve hepsini bir kez deneme tabloları tasarlama için öneriler sunar.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: b101a4e19d00d44805c7eb5f44d449a18d756804
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851612"
---
# <a name="guidance-for-designing-distributed-tables-in-azure-sql-data-warehouse"></a>Azure SQL Data warehouse'da dağıtılmış tablolar tasarlama hakkında rehberlik
Azure SQL Data warehouse'da dağıtılmış karma dağıtılmış ve hepsini bir kez deneme tabloları tasarlama için öneriler sunar.

Bu makalede, veri dağıtımı ve SQL veri ambarı'nda veri taşıma kavramlarını bildiğiniz varsayılmıştır.  Daha fazla bilgi için [Azure SQL veri ambarı - yüksek düzeyde paralel işleme (MPP) mimarisi](massively-parallel-processing-mpp-architecture.md). 

## <a name="what-is-a-distributed-table"></a>Dağıtılmış bir tablo nedir?
Tek tablo olarak dağıtılmış bir tablo görünür, ancak satırları 60 dağıtımlar arasında gerçekten depolanır. Satırları, karma veya hepsini bir kez deneme algoritması ile dağıtılır.  

**Tabloları karma dağıtılmış** üzerinde büyük bir olgu tabloları sorgu performansını artırmak ve bu makalenin odak noktası olan. **Hepsini bir kez deneme tabloları** yükleme hızını artırmak için kullanışlıdır. Bu tasarım seçenekleri sorgu iyileştirme ve yükleme performansını büyük bir etkiye sahip.

Başka bir tablo depolama seçeneği, tüm işlem düğümleri arasında küçük bir tabloya çoğaltmak sağlamaktır. Daha fazla bilgi için [tasarım kılavuzunu çoğaltılmış tablolar için](design-guidance-for-replicated-tables.md). Dağıtılmış tablolar hızla üç seçenekten seçmek için bkz [tablolar genel bakış](sql-data-warehouse-tables-overview.md). 

Tablo Tasarımı işleminin bir parçası olarak, verileriniz ve verileri nasıl sorgulanır hakkında mümkün olduğunca anlayın.  Örneğin, aşağıdaki soruları göz önünde bulundurun:

- Tablo ne kadar büyük?   
- Tablo ne sıklıkla yenilenir?   
- Olgu ve boyut tabloları bir veri ambarı'nda yok mu?   


### <a name="hash-distributed"></a>Karma dağıtılmış
Bir karma dağıtılmış tablo, her satır için atamak için belirleyici bir karma işlevi kullanarak işlem düğümleri arasında tablo satırları dağıtır [dağıtım](massively-parallel-processing-mpp-architecture.md#distributions). 

![Dağıtılmış tablo](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "dağıtılmış tablo")  

Her zaman aynı dağıtım noktasına karma aynı değeri olduğundan, veri ambarı satır konumu yerleşik bilgiye sahiptir. SQL veri ambarı, sorgu performansı artıran veri taşıma sırasında sorguları en aza indirmek için bu bilgileri kullanır. 

Tabloları karma dağıtılmış bir yıldız şemasındaki büyük bir olgu tabloları için iyi çalışır. Çok fazla sayıda satır vardır ve hala yüksek bir performansa ulaşın. Elbette, dağıtılmış sisteme sağlamak üzere tasarlanmış performansı elde etmek için yardımcı bazı tasarım konuları mevcuttur. İyi dağıtım sütunu seçilmesi, bu makalede açıklanan bir tür noktadır. 

Bir karma dağıtılmış kullanmayı tablosundan:

- Disk üzerinde Tablo 2 GB'den fazla boyutudur.
- Tabloda, sık sık ekleme, güncelleştirme ve silme işlemleri vardır. 

### <a name="round-robin-distributed"></a>Hepsini dağıtılmış
Hepsini bir kez deneme dağıtılmış tablo tablo satırları tüm dağıtımlar arasında eşit olarak dağıtır. Satır dağıtımlar için atama rastgele belirlenir. Karma dağıtılmış tablo satırları eşit değerlerle aynı dağıtım noktasına atanan garanti edilmez. 

Sonuç olarak, sistem, bir sorgu çözebilmek verilerinizi daha iyi düzenlemek için veri taşıma işlemini çağırmak bazen gerekir.  Bu ek adım sorgularınızı yavaşlatabilir. Örneğin, hepsini bir kez deneme tablo genellikle birleştirme satırları reshuffling bir performans düşüşüne olduğu gerektirir.

Hepsini bir kez deneme dağıtım tablonuzun aşağıdaki senaryolarda kullanmayı dikkate alın:

- Varsayılan olduğundan basit bir başlangıç noktası olarak çalışmaya
- Herhangi bir belirgin katılma anahtar ise
- Karma tablo dağıtmak için iyi aday sütunu değilse
- Tablo, diğer tablolarla ortak bir birleştirme anahtarı paylaşmaz,
- Join sorgu diğer birleşimlerin daha az önemli ise
- Tablo bir geçici hazırlama tablosuna olduğunda

Öğreticiyi [yük New York taksi verilerini Azure SQL veri ambarı](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) veri hepsini bir kez deneme hazırlama tablosuna yükleme, bir örnek verir.


## <a name="choosing-a-distribution-column"></a>Dağıtım sütunu seçme
Bir karma dağıtılmış tablo karma anahtar dağıtım sütunu var. Örneğin, aşağıdaki kod, bir karma dağıtılmış tablo ile ProductKey dağıtım sütunu olarak oluşturur.

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
``` 

Bu sütundaki değerleri satır nasıl dağıtıldığını belirlediğinden dağıtım sütunu seçilmesi bir önemli tasarım kararıdır. En iyi seçenek, çeşitli etkenlere bağlıdır ve genellikle ödünler içerir. Ancak, ilk kez en iyi sütun seçmezseniz, kullanabileceğiniz [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) farklı dağıtım sütunu içeren tabloyu yeniden oluşturmak için. 

### <a name="choose-a-distribution-column-that-does-not-require-updates"></a>Güncelleştirmeleri gerektirmeyen dağıtım sütunu seçin
Sürece, satırın silin ve güncelleştirilmiş değerleri içeren yeni bir satır eklemek için bir dağıtım sütunu update yapılamıyor. Bu nedenle, statik değerlerle bir sütun seçin. 

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Dağıtım sütunu dağıtır verilerle seçin

En iyi performans için tüm dağıtımların yaklaşık aynı sayıda satıra sahip olmalıdır. Bir veya daha fazla dağıtımları orantısız bir satır sayısı olduğunda bazı dağıtımları kendi diğerlerinden önce paralel sorgu bölümünü tamamlayın. Sorgu, tüm dağıtımları işlem tamamlanana kadar tamamlanamıyor olduğundan, her sorgu yalnızca en yavaş dağıtım hızlıdır.

- Veri dengesizliği dağıtımlar arasında verileri eşit olarak dağıtılmaz anlamına gelir.
- İşleme eğriltme bazı dağıtımların paralel sorgu çalıştırırken diğerlerine göre daha uzun anlamına gelir. Veri dengesiz yayılırsa bu durum oluşabilir.
  
Paralel işleme dengelemek için bir dağıtım sütununu seçin:

- **Birçok benzersiz değerlere sahip.** Sütun bazı yinelenen değerlere sahip olabilir. Ancak, aynı değere sahip olan tüm satırların aynı dağıtım noktasına atandı. 60 dağıtım olduğundan, sütunun en az 60 benzersiz değerler içermelidir.  Genellikle benzersiz değerlerin sayısını çok daha büyüktür.
- **Null değerlere sahip değil veya yalnızca birkaç null değerlere sahiptir.** Sütundaki tüm değerleri NULL olduğunda aşırı örneği için tüm satırların aynı dağıtım noktasına atandı. Sonuç olarak, sorgu işleme için bir dağıtım dengesiz ve paralel işlenmesini fayda. 
- **Bir tarih sütunu değil**. Aynı tarih için tüm veri gölünüzdeki aynı dağıtım. Birkaç kullanıcı tümü aynı tarihte uyguladıysanız yalnızca 1 60 dağıtım, tüm işlem iş yapın. 

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Veri taşıma en aza indiren bir dağıtım sütun seçin

Doğru sorgu sonuç döndüren sorgular almak için veri işlem düğümünden diğerine taşıyabilirsiniz. Veri taşıma, yaygın olarak sorgular üzerinde dağıtılmış tablolar birleşimler ve toplamalar olduğunda gerçekleşir. Veri taşıma en aza yardımcı olan dağıtım sütunu seçilmesi, SQL veri ambarınızın performansını iyileştirmek için en önemli stratejiler biridir.

Veri taşıma en aza indirmek için dağıtım sütunu seçin:

- Kullanılan `JOIN`, `GROUP BY`, `DISTINCT`, `OVER`, ve `HAVING` yan tümceleri. İki büyük bir olgu tabloları sık sık birleştirmeler varsa, her iki tablonun birleşim sütunlarında birinde dağıttığınızda, sorgu performansını artırır.  Bir tablo birleştirmelerde kullanıldığında sık içinde bir sütun tablosunda dağıtma göz önünde bulundurun `GROUP BY` yan tümcesi.
- Olan *değil* kullanılan `WHERE` yan tümceleri. Bu, tüm dağıtımları üzerinde çalışmak için sorgu daraltabilirsiniz. 
- Olan *değil* bir tarih sütunu. WHERE yan tümcelerini genellikle tarihe göre filtreleyin.  Bu durumda, tüm işlem yalnızca birkaç dağıtımlarında çalıştırabilirsiniz.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>Sütunların hiçbirinin iyi dağıtım sütunu olduğunda yapılması gerekenler

Sütunlar hiçbiri dağıtım sütunu için yeterli farklı değerlere sahip bir bileşik bir veya daha fazla değer yeni bir sütun oluşturabilirsiniz. Sorgu yürütme işlemi sırasında veri hareketini önlemek için sorgulardaki bir birleştirme sütunu olarak bileşik dağıtım sütunu kullanın.

Bir karma dağıtılmış tablo tasarlama sonra verileri bir tabloya yüklemek için sonraki adım gelir.  Yüklemeyle ilgili Rehber için bkz: [yüklemeye genel bakış](sql-data-warehouse-overview-load.md). 

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Dağıtım sütununuzu iyi bir seçenek olup olmadığını nasıl
Karma dağıtılmış bir tabloya veri yüklendikten sonra satırları 60 dağıtımlar arasında nasıl eşit şekilde dağıtılır denetleyin. Dağıtım başına satır sayısı en fazla %10 belirgin bir performans etkisi olmadan değişebilir. 

### <a name="determine-if-the-table-has-data-skew"></a>Tablo veri dengesizliği olup olmadığını belirleyin
Veri dengesizliği kullanmaktır denetlemek için hızlı bir şekilde [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql). Aşağıdaki SQL kodunu, her 60 dağıtım içinde depolanan tablo satır sayısını döndürür. Dengeli performansı elde etmek için Dağıtılmış tablosundaki satırları eşit dağıtımlar arasında yaymak.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

En fazla % 10 veri dengesizliği tabloları tanımlamak için:

1. Gösterilen görünümü dbo.vTableSizes oluşturma [tablolar genel bakış](sql-data-warehouse-tables-overview.md#table-size-queries) makalesi.  
2. Aşağıdaki sorguyu çalıştırın:

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having (max(row_count * 1.000) - min(row_count * 1.000))/max(row_count * 1.000) >= .10
    )
order by two_part_name, row_count
;
```

### <a name="check-query-plans-for-data-movement"></a>Veri taşıma için sorgu planlarına denetleyin
Birleşimler ve toplamalar çok az veri taşıma için iyi dağıtım sütunu sağlar. Bu, birleştirmeler yazılmalıdır biçimini etkiler. İki karma dağıtılmış tablo üzerindeki bir birleştirme için en düşük düzeyde veri hareketi almak için bir birleşim sütunlarında dağıtım sütunu olması gerekir.  İki karma dağıtılmış tablo aynı veri türü üzerinde bir dağıtım sütunu katıldığında, birleştirme, veri taşıma gerektirmez. Birleştirmeler ek sütunlar, veri taşıma ödemeden kullanabilirsiniz.

Birleştirme sırasında veri hareketini önlemek için:

- Birleştirmeye dahil olan tabloları karma dağıtılmış üzerinde olmalıdır **bir** birleştirme işleminde katılan sütunlar.
- Birleşim sütunlarında veri türleri, her iki tablo arasında eşleşmelidir.
- Sütunları bir eşittir işleciyle katılması gerekir.
- Birleşim türü olamaz bir `CROSS JOIN`.

Sorgular veri taşıma yaşıyorsanız görmek için sorgu planını bakabilirsiniz.  


## <a name="resolve-a-distribution-column-problem"></a>Dağıtım sütunu sorunu
Tüm durumlarda veri dengesizliği çözmek gerekli değildir. Veri dağıtma, veri dengesizliği gösteriyor ve veri taşıma en aza arasında doğru dengeyi bulmak bir konudur. Her zaman veri dengesizliği hem de veri taşıma en aza indirmek mümkün değildir. Bazen veri dengesizliği sahip olmanın etkisi en düşük düzeyde veri hareketini olmasının avantajı gölgede bırakabilir.

Veri çözümlemek, karar vermek için bir tablodaki eğriltme, veri hacimleri ve sorguları hakkında mümkün olduğunca iş yükünüzü anlamanız gerekir. Adımlarda kullandığınız [sorgu izleme](sql-data-warehouse-manage-monitor.md) makalenin eğriltme sorgu performansı üzerindeki etkisini izleyin. Özellikle, bu büyük sorgular tek dağıtımlarında tamamlanması ne kadar sürdüğü bakın.

Var olan bir tabloda dağıtım sütunu değiştirilemez olduğundan, normal şekilde veri dengesizliği çözmek için farklı dağıtım sütunu içeren tabloyu yeniden oluşturmaktır.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>Yeni dağıtım sütunlu tabloyu yeniden oluşturun
Bu örnekte [CREATE TABLE AS SELECT](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?view=aps-pdw-2016-au7) farklı karma dağıtım sütunlu bir tablo yeniden oluşturulacak.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

## <a name="next-steps"></a>Sonraki adımlar

Dağıtılan bir tablo oluşturmak için bu deyimler birini kullanın:

- [Tablo (Azure SQL veri ambarı) oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [Tablo AS SELECT (Azure SQL veri ambarı oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)


