---
title: Azure SQL veri ambarı seçeneklerinde grubu kullanarak | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL veri ambarı'nda seçenekleri grubu uygulamak için ipuçları.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: b158048929d3db8672d76027666331448a91a0a8
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861797"
---
# <a name="group-by-options-in-sql-data-warehouse"></a>SQL veri ambarı'nda seçenekleri Gruplandır
Çözümleri geliştirme için Azure SQL veri ambarı'nda seçenekleri grubu uygulamak için ipuçları.

## <a name="what-does-group-by-do"></a>GROUP BY ne yapar?

[GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql) T-SQL yan tümcesi Özet bir satır kümesi için veri toplar. GROUP BY, SQL veri ambarı desteklemediği bazı seçenekleri vardır. Bu seçenekler, geçici çözümler içerir.

Bu seçenekler

* GROUP BY paketi
* GROUPING SETS
* GROUP BY ile küp

## <a name="rollup-and-grouping-sets-options"></a>Döküm ve gruplandırma kümeleri seçenekleri
En basit seçenek burada UNION ALL yerine toplamayı gerçekleştirmek için kullanmaktır güvenmek açık söz dizimi yerine. Sonuç tamamen aynı.

GROUP BY deyimi ile toplama seçeneğini kullanarak aşağıdaki örnekte:
```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Yukarıdaki örnekte, paketi kullanarak aşağıdaki toplamalar ister:

* Ülke ve bölge
* Ülke
* Genel toplam

Toplama değiştirin ve aynı sonuçları döndürmek için UNION ALL kullanın ve gereken toplama açıkça belirtin:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

GROUPING SETS değiştirmek için örnek ilke uygulanır. UNION ALL bölümleri görmek istediğiniz toplama düzeyleri için oluşturmanız yeterlidir.

## <a name="cube-options"></a>Küp seçenekleri
Bir grubu tarafından ile UNION ALL yaklaşımı kullanarak KÜPÜ oluşturmak mümkündür. Sorun kodu hızlıca sıkıcı ve kullanışsız olabilir olmasıdır. Bunu azaltmak için bu yaklaşım daha gelişmiş kullanabilirsiniz.

Yukarıdaki örnekte kullanalım.

İlk adım, 'oluşturmak istiyoruz toplama tüm düzeyleri tanımlar cube' tanımlamaktır. CROSS JOIN iki türetilmiş tablo dikkat edin önemlidir. Bu tüm düzeyleri bizim için oluşturur. Kodun geri kalanını gerçekten biçimlendirme için yoktur.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

CTAS sonuçları gösterir:

![Küp tarafından Grup](media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

İkinci adım, geçiş sonuçlarını depolamak için bir hedef tablo belirtmek için verilmiştir:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Üçüncü adım, bizim toplama işlemini gerçekleştirme sütunları küp üzerinde döngü oluşturmaktır. Sorgu #Cube geçici tablodaki her satır için bir kez çalıştırın ve sonuçları #Results geçici tabloya kaydedin.

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Son olarak, #Results geçici tablodaki okuyarak sonuçları döndürmek

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Kod bölümlere ayırma ve uvozuje konstruktor oluşturma, kod daha yönetilebilir ve sürdürülebilir hale gelir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

