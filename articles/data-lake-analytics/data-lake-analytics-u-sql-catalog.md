---
title: "U-SQL Kataloğu ile çalışmaya başlama | Microsoft Docs"
description: "U-SQL kataloğunu kod ve veri paylaşmak için nasıl kullanılacağını öğrenin."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: saveenr
ms.openlocfilehash: b39b5250cc042c393216784128ffc4e2f1288f04
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-u-sql-catalog"></a>U-SQL Kataloğu ile çalışmaya başlama

## <a name="create-a-tvf"></a>Bir TVF oluşturma

Önceki U-SQL komut dosyası aynı kaynak dosyasını okumaya EXTRACT kullanımını yinelenir. U-SQL tablo değerli işlev (TVF), gelecekte tekrar kullanmak için veri yerleştirebilirsiniz.  

Aşağıdaki komut dosyası olarak adlandırılan bir TVF oluşturur `Searchlog()` şema ve varsayılan veritabanı içinde:

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

Aşağıdaki komut dosyası önceki betik tanımlandı TVF kullanmayı gösterir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SerachLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Görünümler oluşturma

Bir tek sorgu ifadesi varsa, TVF yerine, U-SQL görünümü ifade kapsüllemek için kullanabilirsiniz.

Aşağıdaki komut dosyası adlı bir görünüm oluşturur `SearchlogView` şema ve varsayılan veritabanı içinde:

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

Aşağıdaki komut dosyası tanımlı görünüm kullanımını göstermektedir:

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
İlişkisel veritabanı tablolarıyla U-SQL ile bir tablo ile önceden tanımlanmış bir şema oluşturabilir veya şema sorgusu oluşturur bir tablo oluşturmak gibi tablonun (olarak da bilinen CREATE TABLE AS SELECT veya CTAS) doldurur.

Aşağıdaki komut dosyası kullanarak bir veritabanı ve iki tablo oluşturun:

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
Tablolar, veri dosyalarını sorgu aynı şekilde önceki komut dosyasındaki oluşturulanlar gibi sorgulayabilirsiniz. EXTRACT kullanarak bir satır kümesi oluşturmak yerine, şimdi tablo adına başvurabilir.

Tablodan okumak için daha önce kullanılan dönüştürme komut dosyasını değiştirin:

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
 >Şu anda, bir SELECT aynı komut dosyasını bir tabloda tablonun oluşturulduğu çalıştıramazsınız.

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
