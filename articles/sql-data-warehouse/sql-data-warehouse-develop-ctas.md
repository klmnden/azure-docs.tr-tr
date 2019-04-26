---
title: Azure SQL Data Warehouse'da tablo AS SELECT (CTAS) oluşturma | Microsoft Docs
description: Açıklama ve çözümleri geliştirmek için Azure SQL veri ambarı CREATE TABLE AS SELECT (CTAS) deyiminde örnekleri.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/26/2019
ms.author: mlee3gsd
ms.reviewer: jrasnick
ms.custom: seoapril2019
ms.openlocfilehash: c8e9f3ccdfaee64f75443f6a4eb89a3df7c48b0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60403790"
---
# <a name="create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'da tablo AS SELECT (CTAS) oluşturma

Bu makalede, Azure SQL veri ambarı CREATE TABLE AS SELECT (CTAS) T-SQL deyiminde çözümleri geliştirmek için açıklanmaktadır. Makale, kod örnekleri de sağlar.

## <a name="create-table-as-select"></a>TABLO AS SELECT OLUŞTURMA

[CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) (CTAS) deyimi, en önemli T-SQL özellikleri biridir. CTAS bir SELECT deyiminin çıktı göre yeni bir tablo oluşturur paralel bir işlemdir. CTAS oluşturma ve tek bir komutla bir tabloya veri ekleme basit ve hızlı bir yoludur.

## <a name="selectinto-vs-ctas"></a>SEÇİN... Vs'ye. CTAS

CTAS, daha fazla özelleştirilebilir bir sürümü olan [seçin... İÇİNE](/sql/t-sql/queries/select-into-clause-transact-sql) deyimi.

Aşağıdaki basit bir SELECT örneğidir... İÇİNE:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

SEÇİN... İÇİNE işleminin bir parçası olarak dağıtım yöntemini ya da dizin türünü değiştirmek izin vermez. Oluşturduğunuz `[dbo].[FactInternetSales_new]` ROUND_ROBIN varsayılan dağıtım türü ve kümelenmiş COLUMNSTORE dizini varsayılan tablo yapısını kullanarak.

CTAS ile diğer yandan, her iki dağıtım tablo verilerini ve bunun yanı sıra tablo yapısı türü belirtebilirsiniz. CTAS önceki örneğe dönüştürmek için:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
   ,CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [!NOTE]
> Yalnızca CTAS işlemi dizin değiştirmeye çalıştığınız ve kaynak tablo karma dağıtılmış olduğundan, aynı dağıtım sütunu ve veri türünü koruyun. Bu geçici dağıtım veri taşıma daha etkilidir işlemi sırasında önler.

## <a name="use-ctas-to-copy-a-table"></a>CTAS bir tabloyu kopyalama için kullanın

Belki de DDL değiştirmek için CTAS en yaygın kullanımlarından biri, bir tablonun kopyasını oluşturuyor. Şimdi deyin başlangıçta tablonuz olarak oluşturduğunuz `ROUND_ROBIN`ve artık bir sütun üzerinde dağıtılan bir tablo için değiştirmek istiyorsanız. CTAS, nasıl dağıtım sütununun değiştiğini ' dir. CTAS, bölümlendirme, dizin oluşturma veya sütun türlerini değiştirmek için de kullanabilirsiniz.

Şimdi deyin bu tablonun varsayılan dağıtım türünü kullanarak oluşturduğunuz `ROUND_ROBIN`, bir dağıtım sütun belirtmeden `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Bu tabloda yeni bir kopyasını oluşturmak istediğiniz artık bir `Clustered Columnstore Index`, kümelenmiş Columnstore tablolarının performansını yararlanabilir. Ayrıca bu tablo üzerinde dağıtmak istediğiniz `ProductKey`, çünkü bu sütunda birleştirme bekleme ve üzerinde birleştirme sırasında veri hareketini önlemek istiyorsanız `ProductKey`. Son olarak, aynı zamanda üzerinde bölümleme eklemek istediğiniz `OrderDateKey`, eski bölümleri bırakarak hızlı bir şekilde eski verileri silebilirsiniz. Eski e-tablonuzda yeni bir tabloya kopyaladığı CTAS deyimi aşağıda verilmiştir.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Son olarak, yeni tablonuzun değiştirme ve ardından eski tablonuzun tablolarınızı yeniden adlandırabilirsiniz.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="use-ctas-to-work-around-unsupported-features"></a>Desteklenmeyen özellikler etrafında çalışmak için CTAS kullanın

CTAS, aşağıda listelenen desteklenmeyen özellikler geçici olarak çözmek için de kullanabilirsiniz. Bu yöntem genellikle yalnızca kodunuzu uyumlu olur, ancak genellikle SQL Data Warehouse'da daha hızlı çalışacaktır, yararlı. Bu performans, tam olarak paralel tasarımını sonucudur. Senaryolar şunlardır:

* ANSI BİRLEŞTİRMELER güncelleştirmeleri
* ANSI birleştirmeler üzerinde siler
* MERGE deyimi

> [!TIP]
> Düşünme deneyin "CTAS ilk." Sonuç olarak daha fazla veri yazıyorsanız bile CTAS kullanarak sorun giderme iyi bir yaklaşım genel olarak geçerlidir.

## <a name="ansi-join-replacement-for-update-statements"></a>Güncelleştirme ifadeler için ANSI birleştirme değiştirme

Karmaşık bir güncelleştirme olduğunu fark edebilirsiniz. Güncelleştirme ikiden fazla tablo, UPDATE veya DELETE gerçekleştirmek için ANSI JOIN söz dizimini kullanarak birlikte birleştirir.

Bu tabloda güncelleştirmeye sahip olduğunu düşünün:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Özgün sorgu şu örnekteki gibi bir şey görünüyordu:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

SQL veri ambarı, ANSI birleşimlerde desteklemiyor `FROM` yan tümcesi bir `UPDATE` deyimi, önceki örnekte değiştirmeden kullanamazlar.

Önceki örnekte değiştirmek için CTAS ve dolaylı bir birleşim bir bileşimini kullanabilirsiniz:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI birleştirme ardılı delete deyimleri

Bazen verileri silmek için en iyi yaklaşım CTAS, özellikle kullanmaktır `DELETE` sözdizimi ANSI kullanan deyimleri katılın. SQL veri ambarı ANSI birleşimlerde desteklemiyor olmasıdır `FROM` yan tümcesi bir `DELETE` deyimi. Verileri silme yerine, korumak istediğiniz verileri seçin.

Dönüştürülen bir örneği verilmiştir `DELETE` deyimi:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you want to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Merge deyimlerinde değiştirin

Merge deyimlerinde bölümünde CTAS kullanarak en az değiştirebilirsiniz. Birleştirebilirsiniz `INSERT` ve `UPDATE` tek bir deyim içinde. Tüm silinen kayıtlar kısıtlı `SELECT` sonuçlardan atlamak için deyimi.

Aşağıdaki örnek için bir `UPSERT`:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];
```

## <a name="explicitly-state-data-type-and-nullability-of-output"></a>Veri türü ve null değer alabilme durumunun çıkış açıkça belirtin.

Kod geçişi sırasında bu tür bir kodlama düzeni arasında çalıştırdığınız bulabilirsiniz:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Bu kod için CTAS geçirmeniz gerekir ve doğru olacaktır düşünebilirsiniz. Ancak, burada gizli bir sorun yoktur.

Aşağıdaki kod, aynı sonucu verecek değil:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

"Sonuç" sütun İleri ifadenin veri türünü ve öğesinin değerleri taşıyan dikkat edin. Dikkatli olmazsanız veri taşıyan İleri türü değerlerinde ince farklarını neden olabilir.

Bu örneği deneyin:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Sonuç için depolanan değeri farklıdır. Sonuç sütunu kalıcı değeri diğer ifadeler kullanıldıkça, hata daha önemli hale gelir.

![Ekran görüntüsü, CTAS sonuçları](media/sql-data-warehouse-develop-ctas/ctas-results.png)

Bu, veri geçişler için önemlidir. İkinci sorgu tartışmaya daha doğru olsa bile bir sorun yoktur. Veriler için kaynak sistem karşılaştırıldığında farklı olacaktır ve bu soruların geçişin bütünlüğü gidiyor. Bu, nadir durumlarda "yanlış" yanıt gerçekten doğru olanı olduğu biridir!

İki sonuçtan arasında bir uyuşmazlık görüyoruz örtük tür atama nedeniyle nedenidir. İlk örnekte, tablonun sütun tanımı tanımlar. Örtük tür dönüştürme satır eklendiğinde gerçekleşir. İkinci örnekte, örtük tür dönüştürme ifadesi sütunun veri türünü tanımlar gibi bulunur.

Ayrıca ilk örnekte, henüz ise ikinci örnekteki sütun null yapılabilir bir sütun olarak tanımlanmış dikkat edin. Tablonun ilk örnekte oluşturulurken sütun öğesinin açıkça tanımlandı. İkinci örnekte, bu ifadeyi bırakıldı ve varsayılan olarak NULL tanımında neden olur.

Bu sorunları çözmek için açıkça CTAS deyimi seçme bölümünde öğesinin ve tür dönüştürme ayarlamanız gerekir. Bu özellikler 'CREATE TABLE' ayarlanamıyor.
Aşağıdaki örnek kod düzeltme gösterilmektedir:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Şunlara dikkat edin:

* CAST kullanabilirsiniz veya DÖNÜŞTÜREBİLİRSİNİZ.
* IsNull, birleşim değil öğesinin zorlamak için kullanın. Aşağıdaki nota bakın.
* IsNull en dıştaki bir işlevdir.
* İkinci IsNull 0 sabiti parçasıdır.

> [!NOTE]
> Öğesinin doğru şekilde ayarlanması, ISNULL ve değil COALESCE kullanmak için önemlidir. COALESCE belirleyici bir işlev değil ve bu nedenle ifadesinin sonucu boş değer her zaman olur. IsNull farklıdır. Bu belirleyici bir işlemdir. Bu nedenle, IsNull işlevi ikinci bölümü, sabit bir değer veya bir sabit değer olduğunda, sonuç değerini olduğu NOT NULL.

Hesaplamalarınızı bütünlüğünü sağlamaya da tabloda bölüm değiştirme için önemlidir. Bu tabloda bir Olgu Tablosu tanımlanan sahip olduğunuzu düşünün:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Ancak, tutarı hesaplanan bir ifadedir. Bu kaynak veriler bir parçası değil.

Bölümlenmiş kümenizi oluşturmak için aşağıdaki kodu kullanmak isteyebilirsiniz:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]
,   [product]
,   [store]
,   [quantity]
,   [price]
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Sorgu mükemmel çalışır. Bölüm anahtarı yapmayı denediğinizde sorun gelir. Tablo tanımları eşleşmiyor. Tablo tanımları eşleşmiyor, eklemek için CTAS değişiklik yapmak için bir `ISNULL` sütun null atanabilirlik özniteliğine korumak için işlevi.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Türü tutarlılık ve öğesinin özellikte CTAS Bakımı bir mühendislik en iyi durumda olduğunu görebilirsiniz. Hesaplamalarınızda bütünlüğünü sağlamaya yardımcı olur ve ayrıca bölüm değiştirme mümkün olmasını sağlar.

CTAS, SQL veri ambarı en önemli deyimlerinde biridir. İyice anladığınızdan emin olun. Bkz: [CTAS belgeleri](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

