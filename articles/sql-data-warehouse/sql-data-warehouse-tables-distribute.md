---
title: "SQL veri ambarı tablolarda dağıtma | Microsoft Docs"
description: "Azure SQL Data Warehouse tablolarda dağıtma ile çalışmaya başlama."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: d0e12bf821a81826a20b8db84e76c48fa60ad9b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>SQL veri ambarı tablolarda dağıtma
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Veri türleri][Data Types]
> * [Dağıt][Distribute]
> * [Dizin][Index]
> * [Bölüm][Partition]
> * [İstatistikleri][Statistics]
> * [Geçici][Temporary]
>
>

SQL Veri Ambarı, yüksek düzeyde paralel işleme (MPP) ile dağıtılmış bir veritabanı sistemidir.  Birden fazla düğümde verileri ve işleme özelliğini bölerek, SQL Veri Ambarı tek bir sistemin sağlayabileceğinden çok büyük ölçeklenebilirlik sunabilir.  SQL veri ambarı içindeki verilerinizi dağıtmak nasıl karar verme en iyi performans elde için en önemli faktörler biridir.   En iyi performans anahtarına veri taşıma en aza indirerek ve veri taşıma en aza indirmek için anahtarı doğru Dağıtım stratejisi sırayla seçme.

## <a name="understanding-data-movement"></a>Veri taşıma anlama
MPP sistemde, her tablodan veri birkaç temel veritabanları arasında bölünür.  MPP sistemde en iyileştirilmiş sorgular yalnızca üzerinden diğer veritabanları arasındaki etkileşimi olmadan dağıtılmış veritabanlarını yürütmek için geçirilebilir.  Örneğin, iki tablo, satış ve müşteriler içeren satış verilerini içeren bir veritabanına sahip varsayalım.  Ayrı bir veritabanında, her bir müşteri koyma, satış ve müşteri katılma herhangi bir sorgu içinde her müşteri tablonuz satış tablonuz katılmak için gereken bir sorgu varsa ve müşteri sayıya hem satış hem de müşteri tabloları bölme çözülebilir Veritabanı diğer veritabanlarından olanağıyla ile.  Sipariş numarası ve müşteri verilerinizi müşteri numarasına göre satış verilerinizi bölünmüş, buna karşılık, ardından tüm verilen veritabanı karşılık gelen verilerin her müşteri için sahip olmaz ve bu nedenle, müşteri verileri için satış verilerinizi katılmak istiyorsanız, t yapılandırtmanız gerekir Müşterinizle diğer veritabanlarındaki her bir müşteri için verileri.  Bu ikinci örnekte, iki tablo katılabilir satış verileri için müşteri verileri taşımak için gerçekleşmesini veri taşıma gerekir.  

Veri taşıma her zaman hatalı bir şey değilse, bazen bir sorgu çözmek gerekli değildir.  Ancak bu ek adım önlenebilir doğal olarak sorgunuzu daha hızlı çalışır.  Veri taşıma en yaygın olarak tablolar birleştirilir veya toplamalar gerçekleştirilen ortaya çıkar.  Her ikisi de, gerçekleştirmeniz gereken genellikle bir birleştirme gibi bir senaryo için en iyi duruma olabilir ancak bir toplama gibi diğer senaryo için çözmenize yardımcı olmak için veri taşıma hala gerekir.  Eli daha az iş olduğu çıkışı çıkarılıyor.  Çoğu durumda, büyük olgu tabloları genellikle birleştirilmiş bir sütunda dağıtma çoğu veri taşıma azaltma en etkili yöntemdir.  Veri birleştirme sütunlarda dağıtma, bir toplama söz konusu sütunlarda veri dağıtma daha veri taşıma azaltmak için daha yaygın bir yöntemdir.

## <a name="select-distribution-method"></a>Dağıtım yöntemini seçin
Arka planda, SQL Data Warehouse verilerinizi 60 veritabanlarına böler.  Her bağımsız veritabanı olarak adlandırılır bir **dağıtım**.  Veri her tabloya yüklendiğinde, SQL Data Warehouse verilerinizi 60 bu dağıtımlar arasında bölmek nasıl bilmesi gerekir.  

Dağıtım yöntemi tablo düzeyinde tanımlanır ve şu anda iki seçeneğiniz vardır:

1. **Hepsini bir kez** eşit ancak rastgele veri dağıtın.
2. **Dağıtılmış karma** tek bir sütun değerlerinden karma göre verileri dağıtır

Bir veri dağıtım yöntemi olmayan tanımlarken varsayılan olarak, tablonuz kullanarak dağıtılacak **hepsini** dağıtım yöntemi.  Bununla birlikte, uygulamanızda daha karmaşık hale geldikçe kullanarak düşünmek isteyebilirsiniz **dağıtılmış karma** hangi sırayla iyileştirir veri taşıma en aza indirmek için tabloları sorgu performansı.

### <a name="round-robin-tables"></a>Hepsini bir kez tabloları
Veri dağıtmanın hepsini bir kez yöntemi kullanarak çok nasıl, göründüğü değildir.  Verilerinizi yüklenen gibi her satır yalnızca ileri dağıtım noktasına gönderilir.  Veri dağıtmanın bu yöntem her zaman rastgele veriler çok eşit tüm dağıtımlar arasında dağıtır.  Diğer bir deyişle, var. verilerinizi yerleştirir hepsini bir kez işlemi sırasında yapılan sıralama  Hepsini bir kez dağıtım, bu nedenle rastgele bir karma bazen denir.  Hepsini dağıtılmış tablo ile veri anlamak için gerek yoktur.  Bu nedenle, hepsini tabloları genellikle iyi yükleme hedefleri olun.

Hiçbir dağıtım yöntemi seçilirse, varsayılan olarak, hepsini bir kez dağıtım yöntemi kullanılır.  Ancak, sistem hangi dağıtım garanti edemez anlamına gelir sistem veri rastgele dağıtıldığından hepsini bir kez tablolar kullanmayı, durumdayken her satırın'dır.  Sonuç olarak, sistem, bir sorgu çözebilmek için önce verilerinizi daha iyi düzenlemek için bir veri taşıma işlemini çağırmak bazen gerekir.  Bu ek adım sorgularınızı yavaşlatabilir.

Aşağıdaki senaryolarda tablonuzun hepsini dağıtım kullanmayı dikkate alın:

* Basit bir başlangıç noktası olarak çalışmaya
* Hiçbir belirgin katılma anahtarı ise
* Karma tablo dağıtmak için iyi bir adaydır sütun değilse
* Tablo diğer tablolarla ortak bir birleşim anahtar paylaşmaz varsa
* Join sorgu diğer birleşimlerde'den daha az önemli ise
* Tablo geçici bir hazırlama tablosunda olduğunda

Bu örneklerin her ikisi de hepsini bir kez tablo oluşturacak:

```SQL
-- Round Robin created by default
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
;

-- Explicitly Created Round Robin Table
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Hepsini bir kez olsa da, DDL'de açık olan varsayılan tablo türü bir tablo düzeni amaçları başkalarına açık olmasını sağlamak en iyi yöntem olarak kabul edilir.
>
>

### <a name="hash-distributed-tables"></a>Dağıtılmış tabloları karma
Kullanarak bir **dağıtılmış karma** tablolarınızı dağıtmak için algoritma sorgu zamanında veri taşıma azaltarak pek çok senaryoyla performansı geliştirebilir.  Dağıtılmış karma, seçtiğiniz tek bir sütun üzerinde bir karma algoritması kullanılarak dağıtılan veritabanları arasında bölünmüş tabloların tablolardır.  Dağıtım ne veriler dağıtılan veritabanları arasında nasıl bölünür belirler bir sütundur.  Karma işlevi dağıtım sütun dağıtımları için satır atamak için kullanır.  Karma algoritması ve sonuçta elde edilen dağıtım belirleyici.  Aynı veri türüne sahip aynı değer her zaman aynı dağıtım noktasına sahip olmasıdır.    

Bu örnek kimliği, dağıtılmış bir tablo oluşturacak:

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

## <a name="select-distribution-column"></a>Dağıtım sütun seçin
Seçtiğinizde **karma dağıtmak** bir tablo tek dağıtım sütun seçmeniz gerekir.  Bir dağıtım sütun seçerken, dikkate alınması gereken üç temel Etkenler vardır.  

Olur, tek bir sütun seçin:

1. Güncelleştirilmemiş
2. Veri eşit, veri eğme önleme Dağıt
3. Veri taşıma simge durumuna küçült

### <a name="select-distribution-column-which-will-not-be-updated"></a>Güncelleştirilmez dağıtım sütun seçin
Dağıtım sütunları bu nedenle güncelleştirilebilir, değildir, statik değerleri içeren bir sütun seçin.  Bir sütun güncelleştirilmesi gerekiyorsa, genelde iyi dağıtım aday değildir.  Bir dağıtım sütun güncelleştirme söz konusu ise, bu ilk satırın silinmesi ve yeni satır ekleme yapılabilir.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Veri eşit olarak dağıtmanızı dağıtım sütun seçin
Dağıtılmış bir sistemde yalnızca olabildiğince hızlı şekilde kendi yavaş dağıtım gerçekleştirir, iş dengeli yürütme sistem üzerindeki elde etmek için dağıtımlar arasında eşit olarak bölmek önemlidir.  Dağıtılmış bir sistemde iş bölünmüş şekilde, burada her dağıtım için veri yaşamaktadır temel alır.  Bu, böylece her dağıtım eşit bir iş ve kendi iş kısmını tamamlamak için aynı anda sürer verileri dağıtmak için doğru dağıtım sütun seçmek çok önemli kolaylaştırır.  İş iyi sistem üzerindeki ayrıldığında, verileri dağıtımlar arasında dengelenir.  Veri eşit dengeli değil, bu diyoruz **veri eğme**.  

Veri eşit ayırın ve verileri eğme önlemek için dağıtım sütun seçerken aşağıdakileri göz önünde bulundurun:

1. Çok sayıda farklı değerleri içeren bir sütun seçin.
2. Veri birkaç farklı değerleri olan sütunlarda dağıtma kaçının.
3. Null değerlere yüksek sıklığını sahip sütunlarda veri dağıtma kaçının.
4. Veri tarih sütunlarda dağıtma kaçının.

Her değer 60 dağıtımları 1 için karma olduğundan, hatta dağıtım elde etmek için yüksek oranda benzersiz olan ve birden fazla 60 benzersiz değerler içeren bir sütun seçmek istediğiniz.  Göstermek için bir sütun yalnızca 40 benzersiz değerlere sahip olduğu bir durumu göz önünde bulundurun.  Bu sütun dağıtım anahtarı olarak seçtiyseniz, bu tablo için veri üzerinde 40 dağıtımları en fazla 20 dağıtımları hiçbir veri ve hiçbir işlem yapmak için bırakarak güden.  Buna karşılık, diğer 40 dağıtımları verileri, eşit 60 dağıtımları yayılan, bunu yapmak için daha fazla iş gerekir.  Bu senaryoda, veri eğme örneğidir.

MPP sistemdeki her sorgu adım için tüm dağıtımları kendi paylaşımı iş tamamlamak bekler.  Bir dağıtım diğerlerinden daha fazla iş yapıyor, ardından diğer dağıtımlar kaynak temelde küçülttüğü iyi bir şekilde yalnızca meşgul dağıtım bekleniyor.  İş eşit tüm dağıtımlar arasında yayılır değil, bu diyoruz **işleme eğme**.  İşleme eğme sorgu iş yükü dağıtımlar arasında eşit olarak yayılabilen varsa daha yavaş çalışmasına neden olur.  Veri eğme işleme eğme götürür.

Null değerler tüm aynı dağıtım gideceksiniz gibi yüksek oranda boş değer atanabilir sütun dağıtma kaçının. Tüm veriler belirli bir tarih için aynı dağıtım gideceksiniz çünkü bir tarih sütunu dağıtma işleme eğme da neden olabilir. Birkaç kullanıcı sorguları yürütme tüm filtreleme aynı tarihte 60 dağıtımları yalnızca 1 tüm belirli bir tarihten bu yana iş istediğimizi sonra yalnızca bir dağıtım noktasında olur. Bu senaryoda, sorguları büyük olasılıkla 60 kez veri eşit tüm dağıtımları yayılan, daha yavaş çalışır.

İyi bir adaydır sütun mevcut olduğunda dağıtım yöntemi olarak hepsini kullanarak düşünün.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Veri taşıma en aza indirecek dağıtım sütun seçin
Veri taşıma doğru dağıtım sütun seçerek en aza SQL veri ambarı performansını iyileştirmek için en önemli stratejileri biridir.  Veri taşıma en yaygın olarak tablolar birleştirilir veya toplamalar gerçekleştirilen ortaya çıkar.  Kullanılan sütunlar `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` ve `HAVING` tüm hale getirmek için yan tümceleri **iyi** dağıtım adayları karma.

Diğer yandan, sütunlarında `WHERE` yan tümcesi **değil** hangi dağıtımları işleme neden sorguda katılmak sınırlamak için iyi bir karma sütun adayları eğme olun.  İyi üzerinde dağıtmak için tempting olabilir, ancak bu işleme eğme genellikle neden olabilir bir sütunun bir tarih sütunu bir örnektir.

Genel olarak bakıldığında, bir birleştirme sık söz konusu iki büyük olgu tabloları varsa, her iki tabloyu birleştirme sütunları birinde dağıtarak çoğu performans elde edersiniz.  Hiçbir zaman başka bir büyük olgu tablosu için birleştirilmiş bir tablo varsa, sık içinde sütunları Ara `GROUP BY` yan tümcesi.

Veri taşıma sırasında bir birleştirme önlemek için karşılanması gereken birkaç anahtar ölçütleri vardır:

1. Birleştirme söz konusu tablolar üzerinde dağıtılmış karma olmalıdır **bir** birleşimde yer alan sütun.
2. Birleşim sütunların veri türlerini iki tablo arasında eşleşmelidir.
3. Sütunları olan bir eşittir işleci katılması gerekir.
4. Birleşim türü olamaz bir `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Veri eğme sorunlarını giderme
Tablo verisi karma dağıtım yöntemi kullanılarak dağıtıldığında diğerlerinden orantısız daha fazla veri sağlamak için bazı dağıtımları eğri şansı yoktur. Bir dağıtılmış sorgu sonucunu uzun süre çalışan dağıtım için beklemeniz gerekir çünkü aşırı miktarda verinin eğme sorgu performansını etkileyebilir. Veri eğme derecesini bağlı olarak ele gerekebilir.

### <a name="identifying-skew"></a>Eğme tanımlama
Eğri bir tablo tanımlamak için basit bir yol kullanmaktır `DBCC PDW_SHOWSPACEUSED`.  Her 60 dağıtımları veritabanınızın depolanan tablo satır sayısını görmek için çok hızlı ve kolay bir yolu budur.  En dengeli performans için unutmayın, dağıtılmış, tablosundaki satırları tüm dağıtımları eşit yayılan.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ancak, Azure SQL Data Warehouse dinamik yönetim görünümlerini (DMV) sorgu varsa daha ayrıntılı bir analiz gerçekleştirebilir.  Başlatmak için Görünüm oluşturma [dbo.vTableSizes] [ dbo.vTableSizes] SQL kullanarak görüntülemek [tablo genel bakışı] [ Overview] makalesi.  Görünüm oluşturulduğunda, en fazla % 10 veri eğme hangi tabloları tanımlamak için bu sorguyu çalıştırın.

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

### <a name="resolving-data-skew"></a>Veri eğme çözme
Bir düzeltme garanti etmeye yetecek tüm eğme değil.  Bazı durumlarda, bazı sorgular tabloda performans verileri eğme zarar daha ağır basar.  Veri çözümlenmelidir varsa karar vermek için bir tabloda eğme, sorgular ve veri birimleri hakkında mümkün olduğunca, iş yükü anlamanız gerekir.   Eğme etkisini bakmak için bir yoldur adımlarda kullanmak üzere [sorgu izleme] [ Query Monitoring] eğme sorgu performansı üzerindeki etkisini ve özellikle nasıl etkisini uzun izlemek için makale sorgular tamamlanması Al tek tek dağıtımları.

Veri dağıtma veri eğme en aza indirerek ve veri taşıma en aza arasında doğru dengeyi bulma bir konudur. Bu hedefleri karşıt ve bazen veri veri taşıma azaltmak için eğme tutmak isteyeceksiniz. Dağıtım sütun sık birleşimler ve toplamalar paylaşılan sütunu olduğunda, örneğin, veri taşıma en aza indirme. En düşük düzeyde veri hareketini sahip yararı eğme verilere sahip olmak etkisini üstün olabilir.

Veri eğme çözümlemek için tipik tablonun farklı dağıtım sütunu yeniden oluşturmanız yoludur. Varolan bir tabloda, bir tablo dağıtımını değiştirmek için yol dağıtım sütunu değiştirmek mümkün olduğundan yeniden oluşturmak için [CTAS] [] ile.  İşte nasıl iki örnek veri eğme çözün:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Örnek 1: yeni bir dağıtım sütun içeren tabloyu yeniden oluşturun
Bu örnekte, farklı bir karma dağıtım sütunlu bir tablo yeniden oluşturmak için [CTAS] [] kullanır.

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

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Örnek 2: hepsini dağıtım kullanarak tabloyu yeniden oluşturun
Bu örnek [CTAS] [] hepsini bir karma dağıtım yerine içeren bir tablo yeniden oluşturmak için kullanır. Bu değişiklik bile artan veri taşıma, veri dağıtımı oluşturur.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
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
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Sonraki adımlar
Tablo tasarımı hakkında daha fazla bilgi için bkz: [Dağıt][Distribute], [dizin][Index], [bölüm] [ Partition], [Veri türleri][Data Types], [istatistikleri] [ Statistics] ve [geçici tablolar] [ Temporary] makaleleri.

En iyi yöntemler genel bakış için bkz: [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
