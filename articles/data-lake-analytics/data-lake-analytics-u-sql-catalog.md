---
title: Azure Data Lake Analytics U-SQL kataloğunda kullanmaya başlama
description: Kod ve veri paylaşmak için U-SQL Kataloğu kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.topic: conceptual
ms.date: 05/09/2017
ms.openlocfilehash: a6faa7037ccbacc0547401dd52bb3b19abd1c474
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60813356"
---
# <a name="get-started-with-the-u-sql-catalog-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics U-SQL kataloğunda kullanmaya başlama

## <a name="create-a-tvf"></a>Bir TVF'si oluşturma

Önceki U-SQL betiği, aynı kaynak dosyasından okumak için ayıklama kullanımını yinelenir. U-SQL tablo değerli işlev ile (TVF), sonra yeniden kullanmak için veri yerleştirebilirsiniz.  

Adlı bir TVF aşağıdaki betiği oluşturur `Searchlog()` varsayılan veritabanı ve şema:

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

Aşağıdaki betiği, önceki betikte tanımlandı TVF kullanma işlemini gösterir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SearchLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Görünümler oluşturma

Tek bir sorgu ifadesi varsa, bir TVF yerine bir U-SQL görünümü ifade kapsamak için kullanabilirsiniz.

Aşağıdaki betiği adlı bir görünüm oluşturur `SearchlogView` varsayılan veritabanı ve şema:

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

Aşağıdaki komut dosyasında tanımlanmış görünüm kullanımını gösterir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a>Tablo oluşturma
İlişkisel veritabanı tabloları ile U-SQL ile bir tablo ile önceden tanımlanmış bir şemaya oluşturabilir veya sorgusundan şema çıkarsar bir tablo oluşturmak gibi tablo (diğer adıyla CREATE TABLE AS SELECT veya CTAS) doldurur.

Aşağıdaki betiği kullanarak, veritabanı ve iki tablo oluşturun:

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a>Sorgu tabloları
Tablolar, veri dosyalarını sorgu aynı şekilde önceki betikte oluşturulanlar gibi sorgulayabilirsiniz. Bir satır kümesi ayıklama kullanarak oluşturmak yerine, artık tablo adına başvurabilir.

Tablodan okumak için daha önce kullandığınız dönüştürme betiği değiştirin:

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    TO "/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 >Şu anda bir SELECT olarak aynı betiği tablosunda tablo oluşturduğunuz çalıştıramazsınız.

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
