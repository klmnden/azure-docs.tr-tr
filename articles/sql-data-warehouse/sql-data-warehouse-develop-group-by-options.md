---
title: Azure SQL Data Warehouse seçeneklerinde Grup kullanarak | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse seçeneklerinde tarafından Grup uygulamak için ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 0548983df23b158385783ac777b23268b5ac7d01
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="group-by-options-in-sql-data-warehouse"></a>SQL veri ambarı seçeneklerinde göre gruplandırmak
Çözümleri geliştirme için Azure SQL Data Warehouse seçeneklerinde tarafından Grup uygulamak için ipuçları.

## <a name="what-does-group-by-do"></a>GROUP BY ne yapar?

[GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql) T-SQL yan tümcesi Özet bir satır kümesi veri toplar. GROUP BY SQL Data Warehouse desteklemediği bazı seçenekleri vardır. Bu seçenekler geçici çözümler içerir.

Bu seçenekler

* GROUP BY ile dökümü
* GRUPLANDIRMA KÜMELERİ
* GROUP BY ile KÜPÜ

## <a name="rollup-and-grouping-sets-options"></a>Toplama ve gruplandırma kümeleri seçenekleri
En basit seçenek burada UNION ALL yerine toplama gerçekleştirmek için kullanmaktır açık sözdizimi, bağlı olan yerine. Tam olarak aynı sonucudur

GROUP BY deyimi TOPLAMASI seçeneğiyle kullanarak aşağıdaki örnek:
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

Toplama kullanarak, önceki örnekte aşağıdaki toplamalar ister:

* Ülke ve bölgeye
* Ülke
* Genel toplam

Toplama değiştirin ve aynı sonuçları döndürmek için UNION ALL kullanın ve gerekli toplamalar açıkça belirtin:

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

GRUPLANDIRMA KÜMELERİ değiştirmek için örnek ilkesini uygular. Yalnızca görmek istediğiniz toplama düzeyleri için UNION ALL bölümleri oluşturmanız gerekir.

## <a name="cube-options"></a>Küp seçenekleri
Bir grup tarafından ile UNION ALL yaklaşımı kullanarak KÜPÜ oluşturmak mümkündür. Kod sıkıcı ve yönetilmeleri hızlı bir şekilde olabilecek sorunudur. Bunu azaltmak için bu yaklaşım daha gelişmiş kullanabilirsiniz.

Yukarıdaki örnekte kullanalım.

İlk adım, 'biz oluşturmak istediğiniz bir toplama tüm düzeylerini tanımlar cube' tanımlamaktır. CROSS JOIN iki türetilmiş tabloların not almanız önemlidir. Bu tüm düzeyleri bize oluşturur. Kod kalan gerçekten biçimlendirme için yoktur.

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

Aşağıdaki CTAS sonuçlarını gösterir:

![Küp tarafından Grup](media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

Ara Sonuçların depolanacağı bir hedef tablo belirtmek için ikinci adım şöyledir:

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

Üçüncü adım bizim toplama gerçekleştirme sütunları küp üzerinde döngü oluşturmaktır. Sorgu #Cube geçici tablodaki her satır için bir kez çalıştır ve #Results geçici tablosunda Sonuçların depolanacağı

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

Son olarak, #Results geçici tablosundan okuyarak sonuçları geri dönebilirsiniz

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Kod bölümlere ayırma ve döngü yapısı oluşturma, kod daha yönetilebilir ve sürdürülebilir haline gelir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

