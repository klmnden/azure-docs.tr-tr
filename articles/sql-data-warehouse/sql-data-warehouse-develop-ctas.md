---
title: Azure SQL Data Warehouse'da tablo AS SELECT (CTAS) oluşturma | Microsoft Docs
description: Açıklama ve örnekleri çözümleri geliştirmek için Azure SQL veri ambarı, CREATE TABLE AS SELECT (CTAS) deyimi.
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
ms.openlocfilehash: ea95a13277927b485bb9da3b75b84cce4337bf88
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280443"
---
# <a name="create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'da tablo AS SELECT (CTAS) oluşturma

Açıklama ve çözümleri geliştirmek için Azure SQL veri ambarı CREATE TABLE AS SELECT (CTAS) T-SQL deyiminde ile kodlama örnekleri.

## <a name="create-table-as-select-ctas"></a>TABLO AS SELECT (CTAS) OLUŞTURMA

[CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) veya CTAS deyimi, en önemli T-SQL özellikleri biridir. CTAS bir SELECT deyiminin çıktı göre yeni bir tablo oluşturur paralel bir işlemdir. CTAS oluşturma ve tek bir komutla bir tabloya veri ekleme basit ve hızlı bir yoludur.

## <a name="selectinto-vs-ctas"></a>SEÇİN... Vs'ye. CTAS

CTAS bir süper dolu sürümü olan [seçin... İÇİNE](/sql/t-sql/queries/select-into-clause-transact-sql) deyimi.

Basit bir SELECT örneği aşağıdadır... İÇİNE:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

SEÇİN... İÇİNE işleminin bir parçası olarak dağıtım yöntemini ya da dizin türünü değiştirmek izin vermez. `[dbo].[FactInternetSales_new]` Kümelenmiş COLUMNSTORE dizini ROUND_ROBIN ve varsayılan tablo yapısı varsayılan dağıtım türü kullanılarak oluşturulur.

Her ikisi de güvendiklerini belirtebilirler CTAS kullanarak tablo verilerini ve bunun yanı sıra tablo yapısı dağıtım türü. CTAS önceki örneğe dönüştürmek için:

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
> Yalnızca dizin değiştirmeye çalışıyorsanız, `CTAS` işlemi ve kaynak tablo karma dağıtılmış olan sonra `CTAS` işlemi aynı dağıtım sütunu ve veri türü sahipseniz en iyi şekilde gerçekleştirir. Bu dağıtım veri taşıma daha verimli olan işlemi sırasında kaçının.

## <a name="using-ctas-to-copy-a-table"></a>CTAS bir tabloyu kopyalama için kullanma

Belki de en yaygın birini kullanır `CTAS` DDL değiştirmek için bir tablonun kopyasını oluşturma. Örneğin, başlangıçta tablonuz olarak oluşturduğunuz, `ROUND_ROBIN` ve artık dağıtılmış bir sütuna, tabloya değiştirmek istiyorsanız `CTAS` nasıl dağıtım sütunu değiştirin. `CTAS` Ayrıca bölümlendirme, dizin oluşturma veya sütun türlerini değiştirmek için kullanılabilir.

Bu tabloda, varsayılan dağıtım türünü kullanarak oluşturduğunuz varsayalım `ROUND_ROBIN` belirterek bir dağıtım sütunu değil `CREATE TABLE`.

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

Artık kümelenmiş Columnstore tablolarının performansını yararlanabilir, böylece bu tabloda yeni bir kopyasını bir kümelenmiş Columnstore diziniyle oluşturmak istiyorsunuz. Ayrıca bu sütunda birleştirme bekleme beri bu tabloda ProductKey dağıtmak istediğiniz ve ürün anahtarı üzerinde birleştirme sırasında veri hareketini önlemek istiyorsanız. Son olarak, aynı zamanda eski bölümleri bırakarak eski verileri hızlı bir şekilde silebilir, böylece OrderDateKey üzerinde bölümleme eklemek istediğiniz. Eski e-tablonuzda yeni bir tabloya kopyalar CTAS deyimi aşağıda verilmiştir.

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

Son olarak yeni tablonuzun değiştirme ve ardından eski tablonuzun tablolarınızı yeniden adlandırabilirsiniz.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="using-ctas-to-work-around-unsupported-features"></a>Desteklenmeyen özellikler etrafında çalışmak için CTAS kullanarak

CTAS, aşağıda listelenen desteklenmeyen özellikler geçici olarak çözmek için de kullanılabilir. Bu yöntem, genellikle yalnızca kodunuzu uyumlu olur ancak bu genellikle daha hızlı bir şekilde SQL veri ambarı üzerinde yürütülür win/win durumda olacak şekilde kanıtlayabilirsiniz. Bu performans, tam olarak paralel tasarımını sonucudur. Geçici bir çözüm ile CTAS çalışılabilmesi senaryolar şunlardır:

* ANSI BİRLEŞTİRMELER güncelleştirmeleri
* ANSI birleştirmeler üzerinde siler
* MERGE deyimi

> [!NOTE]
> Düşünme deneyin "CTAS ilk". Kullanarak sorun giderme `CTAS` sonucunda daha fazla veri yazıyorsunuz bile genellikle iyi bir yaklaşım ise.

## <a name="ansi-join-replacement-for-update-statements"></a>Güncelleştirme ifadeler için ANSI birleştirme değiştirme

İkiden fazla tabloyu birlikte UPDATE veya DELETE gerçekleştirmek için ANSI JOIN söz dizimini kullanarak birleştiren karmaşık bir güncelleştirme sahip bulabilirsiniz.

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

Bir bileşimini kullanabilirsiniz bir `CTAS` ve önceki örnekte değiştirmek için örtük bir birleştirme:

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

Bazen verileri silmek için en iyi yaklaşımdır `CTAS`, özellikle `DELETE` sözdizimi ANSI kullanan deyimleri katılın. SQL veri ambarı ANSI birleşimlerde desteklemiyor olmasıdır `FROM` yan tümcesi bir `DELETE` deyimi. Verileri silme yerine, korumak istediğiniz verileri seçin.

Dönüştürülen bir DELETE deyimi bir örneği aşağıda kullanılabilir:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
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

Birleştirme deyimleri, en az bölümünde CTAS kullanarak değiştirilebilir. Tek bir deyimde INSERT ve UPDATE birleştirebilirsiniz. Tüm silinen kayıtlar kısıtlı `SELECT` sonuçlardan atlamak için deyimi.

Aşağıdaki örnek, bir UPSERT için verilmiştir:

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

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS öneri: Veri türü ve null değer alabilme durumunun çıkış açıkça belirtin.

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

İnstinctively doğru olur ve bu kod için CTAS geçirmelisiniz düşünebilirsiniz. Ancak, burada gizli bir sorun yoktur.

Aşağıdaki kod, aynı sonuç vermez:

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

![CTAS sonuçları](media/sql-data-warehouse-develop-ctas/ctas-results.png)

Bu, veri geçişler için önemlidir. İkinci sorgu tartışmaya daha doğru olsa bile bir sorun yoktur. Veriler için kaynak sistem karşılaştırıldığında farklı olacaktır ve bu soruların geçişin bütünlüğü gidiyor. Bu, nadir durumlarda "yanlış" yanıt gerçekten doğru olanı olduğu biridir!

İki sonuçtan arasında bir uyuşmazlık görüyoruz örtük tür atama nedeniyle nedenidir. İlk örnekte, tablonun sütun tanımı tanımlar. Örtük tür dönüştürme satır eklendiğinde gerçekleşir. İkinci örnekte, örtük tür dönüştürme ifadesi sütunun veri türünü tanımlar gibi bulunur. Ayrıca ilk örnekte, henüz ise ikinci örnekteki sütun null yapılabilir sütun olarak tanımlanmış dikkat edin. Tablonun ilk örnekte oluşturulurken sütun öğesinin açıkça tanımlandı. İkinci örnekte, bu ifade ve varsayılan olarak bırakıldı, bir NULL tanımında neden olur.

Bu sorunları çözmek için açıkça CTAS deyimi seçme bölümünde öğesinin ve tür dönüştürme ayarlamanız gerekir. Oluşturma tablo bölümünde bu özellikleri ayarlanamıyor.

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

* CAST veya CONVERT kullanılan
* IsNull öğesinin değil COALESCE zorlamak için kullanılır
* En dıştaki işlev IsNull olduğu
* IsNull ikinci kısmı olan 0 sabiti

> [!NOTE]
> IsNull ve değil COALESCE kullanılacak öğesinin doğru şekilde ayarlanması önemlidir. COALESCE belirleyici bir işlev değil ve bu nedenle ifadenin sonucu boş değer her zaman olacaktır. IsNull farklıdır. Bu belirleyici bir işlemdir. Bu nedenle zaman IsNull işlevi ikinci bölümü bir sabit veya bir sabit değer ardından sonuç değerini olacaktır NOT NULL.

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

Ancak, tutarı hesaplanan bir ifadedir; Bu kaynak veriler bir parçası değil.

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

Sorgu mükemmel iyi çalışır. Bölüm anahtarı yapmayı denediğinizde sorun gelir. Tablo tanımları eşleşmiyor. Eşleşen tablo tanımları yapmak için CTAS eklemek için değiştirilmesi gereken bir `ISNULL` sütun null atanabilirlik özniteliğine korumak için işlevi.

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

Türü tutarlılık ve öğesinin özellikte CTAS koruma iyi bir mühendislik en iyi uygulama olduğunu görebilirsiniz. Bu, hesaplamalarınızda bütünlüğünü sağlamaya yardımcı olur ve ayrıca bölüm değiştirme mümkün olmasını sağlar.

Başvurmak [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) belgeleri. CTAS, Azure SQL veri ambarı en önemli deyimlerinde biridir. İyice anladığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

