---
title: "SQL veri ambarı seçeneklerinde grupla | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse seçeneklerinde tarafından Grup uygulamak için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: da71cb834c13da5d0f5690f471efc6c696163f30
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a>SQL veri ambarı seçeneklerinde göre gruplandırmak
[GROUP BY] [ GROUP BY] yan tümcesi Özet bir satır kümesi için veri toplama için kullanılır. Ayrıca, geçici doğrudan Azure SQL Data Warehouse tarafından desteklenmiyor olarak çalışması için gereken kendi işlevselliğini genişleten birkaç seçenek vardır.

Bu seçenekler

* GROUP BY ile dökümü
* GRUPLANDIRMA KÜMELERİ
* GROUP BY ile KÜPÜ

## <a name="rollup-and-grouping-sets-options"></a>Toplama ve gruplandırma kümeleri seçenekleri
Burada en basit seçenek kullanmaktır `UNION ALL` yerine toplama gerçekleştirmek için açık sözdizimi, bağlı olan yerine. Tam olarak aynı sonucudur

Aşağıda, bir gruba deyimi kullanılarak örneğidir `ROLLUP` seçeneği:

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

Toplama kullanarak aşağıdaki toplamalar istediniz:

* Ülke ve bölgeye
* Ülke
* Genel toplam

Bu kullanması gerekecektir değiştirmek için `UNION ALL`; aynı sonuçları döndürmek için açıkça gerekli toplamalar belirtme:

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

GRUPLANDIRMA yapmamız gereken ayarlar tüm için aynı asıl adı benimsemeyi ancak yalnızca görmeyi istiyoruz toplama düzeyleri için UNION ALL bölümler oluşturma

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

CTAS sonuçlarını aşağıda görebilirsiniz:

![][1]

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

Son olarak biz #Results geçici tablosundan okuyarak sonuçlar döndürebilir

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Kod bölümlere ayırma ve döngü yapısı oluşturma kodu daha yönetilebilir ve sürdürülebilir haline gelir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
