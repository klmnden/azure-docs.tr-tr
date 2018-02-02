---
title: "Tasarım Kılavuzu dağıtılmış tablolar - Azure SQL Data Warehouse için | Microsoft Docs"
description: "Azure SQL Data Warehouse karma dağıtılmış ve hepsini tablolarında tasarlamak için öneriler sunar."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 01/18/2018
ms.author: barbkess
ms.openlocfilehash: 3c86b89da796223336e3a0d9dd809ae140d6911e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="guidance-for-designing-distributed-tables-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse dağıtılmış tablolarda tasarlamak için kılavuz

Bu makalede, Azure SQL Data Warehouse dağıtılmış tablolarda tasarlamak için öneriler sağlar. Karma dağıtılmış tabloları büyük olgu tabloları üzerinde sorgu performansını artırmak ve bu makaleyi odağını şunlardır. Hepsini tabloları yükleme hızını artırmak için faydalıdır. Bu tasarım seçenekleri sorgu iyileştirme ve performans yükleme önemli bir etkiye sahiptir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, veri dağıtımı ve SQL Data Warehouse veri taşıma kavramlarına alışık olduğunuz varsayılır.  Daha fazla bilgi için bkz: [mimarisi](massively-parallel-processing-mpp-architecture.md) makalesi. 

Tablo Tasarımı bir parçası olarak, verilerinizi ve verilerin nasıl sorgulanır hakkında mümkün olduğunca anlayın.  Örneğin, aşağıdaki soruları göz önünde bulundurun:

- Tablo büyüklüğü nedir?   
- Tablo ne sıklıkla yenileniyor?   
- Bir veri ambarı olgu ve boyut tabloları var mı?   

## <a name="what-is-a-distributed-table"></a>Dağıtılmış bir tablo nedir?
Dağıtılmış bir tablo tek bir tablo olarak görünür, ancak satırları gerçekte 60 dağıtımları depolanır. Satırları bir karma veya hepsini algoritması ile dağıtılır. 

Başka bir tablo depolama küçük bir tablo tüm işlem düğümü arasında çoğaltma seçenektir. Daha fazla bilgi için bkz: [Tasarım Kılavuzu çoğaltılmış tablolar için](design-guidance-for-replicated-tables.md). Bkz: Dağıtılmış tablolarda hızla üç seçenekten seçmek için [tabloları genel bakış](sql-data-warehouse-tables-overview.md). 


### <a name="hash-distributed"></a>Dağıtılmış karma
Karma dağıtılmış bir tablo, her satır için atamak için belirleyici karma işlevi kullanarak işlem düğümleri arasında tablo satırları dağıtır [dağıtım](massively-parallel-processing-mpp-architecture.md#distributions). 

![Dağıtılmış tablo](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "dağıtılmış tablo")  

Aynı değerlere her zaman aynı dağıtım noktasına karma olduğundan, veri ambarı satır konumlarını yerleşik bilgisine sahiptir. SQL veri ambarı, sorgu performansını artıran veri taşıma sorguları sırasında en aza indirmek için bu bilgiyi kullanır. 

Karma dağıtılmış tabloları yıldız şemasında büyük olgu tabloları için iyi çalışır. Çok fazla sayıda satır olması ve hala yüksek performans elde etmek. Elbette, dağıtılmış sistem sağlamak üzere tasarlanmış performansı elde size yardımcı bazı tasarım konuları vardır. İyi dağıtım sütun seçme bu makalede açıklanan bir tür noktadır. 

Karma dağıtılmış kullanmayı tablosundan:

- 2 GB'den daha fazla disk üzerinde tablo boyutudur.
- Tablo sık ekleme, güncelleştirme ve silme işlemleri vardır. 

### <a name="round-robin-distributed"></a>Dağıtılmış hepsini
Hepsini dağıtılmış tablo tablo satırları tüm dağıtımlar arasında dağıtır. Atama satır dağıtımları için rastgele bir seçimdir. Karma dağıtılmış tablolardan farklı olarak, eşit değerli satırları aynı dağıtım noktasına atanan garanti edilmez. 

Sonuç olarak, sistem, bir sorgu çözebilmek için önce verilerinizi daha iyi düzenlemek için bir veri taşıma işlemini çağırmak bazen gerekir.  Bu ek adım sorgularınızı yavaşlatabilir. Örneğin, genellikle bir hepsini tablo birleştirme satırları reshuffling performans isabet olduğu gerektirir.

Aşağıdaki senaryolarda tablonuzun hepsini dağıtım kullanmayı dikkate alın:

- Basit bir başlangıç noktası olarak bu yana Başlarken varsayılan olduğunda
- Hiçbir belirgin katılma anahtarı ise
- Karma tablo dağıtmak için iyi bir adaydır sütun değilse
- Tablo diğer tablolarla ortak bir birleşim anahtar paylaşmaz varsa
- Join sorgu diğer birleşimlerde'den daha az önemli ise
- Tablo geçici bir hazırlama tablosunda olduğunda

Öğretici [verileri Azure Storage blobundan yüklenirken](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) hepsini hazırlama tabloya veri yükleme, bir örnek verir.


## <a name="choosing-a-distribution-column"></a>Bir dağıtım sütun seçme
Bir karma dağıtılmış tablo hash anahtarı olan bir dağıtım sütunu vardır. Örneğin, aşağıdaki kod, karma dağıtılmış bir tablo ile ProductKey dağıtım sütunu olarak oluşturur.

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

Bu sütundaki değerleri satır nasıl dağıtıldığını belirlediğinden dağıtım sütun seçme bir önemli tasarım bir karardır. En iyi seçenek, çeşitli etmenlere bağlıdır ve genellikle bileşim ilgilidir. Ancak, ilk kez en iyi sütun seçmezseniz, kullanabileceğiniz [oluşturmak tablo AS seçin (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) tablo farklı dağıtım sütunu yeniden oluşturmak için. 

### <a name="choose-a-distribution-column-that-does-not-require-updates"></a>Güncelleştirmeleri gerektirmeyen bir dağıtım sütun seçin
Satır silin ve güncelleştirilmiş değerlerle yeni bir satır ekleyin sürece dağıtım sütun güncelleştirilemiyor. Bu nedenle, statik değerleri içeren bir sütun seçin. 

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Bir dağıtım sütun dağıtır verilerle seçin

En iyi performans için tüm dağıtımların yaklaşık aynı sayıda satır olması gerekir. Bir veya daha fazla dağıtımları satırları aşırı miktarda sahip olduğunuzda, bazı dağıtımları bir paralel sorgu diğerlerinden önce kendi bölümünü tamamlayın. Tüm dağıtımları işleme bitinceye kadar sorgu tamamlayamıyor olduğundan, her sorgu yalnızca yavaş dağıtım hızlı kalır.

- Veri eğme veri dağıtımlar arasında eşit olarak dağıtılmaz anlamına gelir.
- İşleme eğme bazı dağıtımları paralel sorgu çalıştırırken diğerlerinden daha uzun sürede olduğunu anlamına gelir. Veri yayılırsa bu durum oluşabilir.
  
Paralel işleme dengelemek için bir dağıtım sütunu seçin:

- **Çok sayıda benzersiz değerlere sahip.** Sütun bazı yinelenen değerlere sahip olabilir. Ancak, aynı değere sahip olan tüm satırların aynı dağıtım noktasına atandı. 60 dağıtımları olduğundan, en az 60 benzersiz değerler sütunu olmalıdır.  Genellikle benzersiz değerlerin sayısını çok daha büyüktür.
- **Null değerlere sahip değil veya yalnızca birkaç null değerlere sahip.** Sütundaki tüm değerlerin NULL, aşırı örneği için tüm satırların aynı dağıtım noktasına atanır. Sonuç olarak, sorgu işlemi için bir dağıtım eğri ve paralel işlenmesini yararlı değildir. 
- **Bir tarih sütunu değil**. Tüm verileri aynı tarih için aynı dağıtım adlandırıldığını. Birkaç kullanıcı tüm aynı tarihte filtre, 60 dağıtımları yalnızca 1 tüm işlem iş yapın. 

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Veri taşıma en aza indiren bir dağıtım sütun seçin

Doğru sorgu sonuç döndüren sorgular almak için verileri bir işlem düğümden diğerine taşımak. Veri taşıma sık sorguları dağıtılmış tablolarda birleşimler ve toplamalar olduğunda gerçekleşir. Veri taşıma en aza indirmenize yardımcı olan bir dağıtım sütun seçme SQL veri ambarı performansını iyileştirmek için en önemli stratejileri biridir.

Veri taşıma en aza indirmek için bir dağıtım sütunu seçin:

- Kullanılan `JOIN`, `GROUP BY`, `DISTINCT`, `OVER`, ve `HAVING` yan tümceleri. İki büyük olgu tabloları sık birleştirmeler varsa, her iki tabloyu birleştirme sütunları birinde dağıttığınızda, sorgu performansı artırır.  Tablo birleştirme kullanılmadığı zaman sık dosyasındaki bir sütunu tabloda dağıtma göz önünde bulundurun `GROUP BY` yan tümcesi.
- Olan *değil* kullanılan `WHERE` yan tümceleri. Bu, tüm dağıtımları üzerinde çalışmak için sorgu daraltabilirsiniz. 
- Olan *değil* bir tarih sütunu. WHERE yan tümceleri genellikle tarihe göre filtreleyin.  Bu durumda, tüm işlem yalnızca birkaç dağıtımları çalıştırabilir.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>Sütunları hiçbiri iyi dağıtım sütunu olduğunda yapılması gerekenler

Sütunlar hiçbiri dağıtım sütun için yeterli farklı değerleri varsa, bir veya daha fazla değer bileşimi yeni bir sütun oluşturabilirsiniz. Sorgu yürütme sırasında veri taşıma önlemek için sorguları birleştirme sütunu olarak bileşik dağıtım sütunu kullanın.

Bir karma dağıtılmış tablosu tasarlamak sonra sonraki tabloya veri yüklemek için bir adımdır.  Kılavuzu yüklemek için bkz: [yüklemeye genel bakış](sql-data-warehouse-overview-load.md). 

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Dağıtım sütunu olduğunda iyi bir seçimdir anlatma
Veri karması dağıtılmış bir tabloya yüklendikten sonra satırları 60 dağıtımlar arasında eşit şekilde nasıl dağıtılır denetleyin. Dağıtım başına satır % 10 performansı belirgin bir etkisi olmadan farklılık gösterebilir. 

### <a name="determine-if-the-table-has-data-skew"></a>Tablo verileri eğme olup olmadığını belirleyin
Veri eğme kullanmak için bir hızlı şekilde [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql). Aşağıdaki SQL kodu her 60 dağıtımları depolanan tablo satır sayısını döndürür. Dengeli performansı elde etmek için Dağıtılmış tablosundaki satırları eşit tüm dağıtımları yayılır.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

En fazla % 10 veri eğme hangi tabloları tanımlamak için:

1. Gösterilen görünüm dbo.vTableSizes oluşturma [tabloları genel bakış](sql-data-warehouse-tables-overview.md#table-size-queries) makalesi.  
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
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="check-query-plans-for-data-movement"></a>Veri taşıma için sorgu planlarını denetleyin
İyi dağıtım sütun birleşimler ve toplamalar en düşük düzeyde veri taşıma olmasını sağlar. Bu, birleştirmeler yazılması gereken şekilde etkiler. İki karma dağıtılmış tablolarda birleştirme için en düşük düzeyde veri taşıma almak için birleştirme sütunları birini dağıtım sütun olması gerekir.  İki karma dağıtılmış tablo üzerinde aynı veri türünde bir dağıtım sütun katıldığında, katılım veri taşıma gerektirmez. Birleştirmeler ek sütunlar, veri taşıma yansıtılmasını olmadan kullanabilirsiniz.

Veri taşıma sırasında bir birleştirme önlemek için:

- Birleştirme söz konusu tablolar üzerinde dağıtılmış karma olmalıdır **bir** birleşimde yer alan sütun.
- Birleşim sütunların veri türlerini iki tablo arasında eşleşmelidir.
- Sütunları olan bir eşittir işleci katılması gerekir.
- Birleşim türü olamaz bir `CROSS JOIN`.

Sorguları bir veri taşıma yaşıyor görmek için sorgu plana bakabilirsiniz.  


## <a name="resolve-a-distribution-column-problem"></a>Bir dağıtım sütun sorunu giderme
Veri tüm örneklerini eğme çözümlemek gerekli değildir. Veri dağıtma veri eğme ve veri taşıma en aza arasında doğru dengeyi bulma bir konudur. Her zaman veri eğme ve veri taşıma en aza indirmek mümkün değildir. Bazı durumlarda en düşük düzeyde veri hareketini sahip yararı eğme verilere sahip olmak etkisini üstün olabilir.

Veri çözümlenmelidir varsa karar vermek için bir tabloda eğme, sorgular ve veri birimleri hakkında mümkün olduğunca, iş yükü anlamanız gerekir. İçindeki adımları kullanın [sorgu izleme](sql-data-warehouse-manage-monitor.md) eğme sorgu performansı üzerindeki etkisini izlemek için makale. Özellikle, tek tek dağıtımları tamamlamak için büyük sorgular süreyi bakın.

Varolan bir tabloyu dağıtım sütunu değiştirilemiyor olduğundan, veri eğme çözümlemek için tipik tablonun farklı dağıtım sütunu yeniden oluşturmanız yoludur.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>Yeni bir dağıtım sütun içeren tabloyu yeniden oluşturun
Bu örnekte [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) farklı bir karma dağıtım sütunlu bir tablo yeniden oluşturmak için.

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

Dağıtılmış bir tablo oluşturmak için bu deyimleri birini kullanın:

- [TABLOSU (Azure SQL Data Warehouse) oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [TABLE AS SELECT (Azure SQL Data Warehouse oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)


