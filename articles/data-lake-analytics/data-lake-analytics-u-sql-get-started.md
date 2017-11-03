---
title: "U-SQL dili ile çalışmaya başlama | Microsoft Docs"
description: "U-SQL dili temellerini öğrenin."
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
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 38c4e1b9bd24ef0b8a81f6154620f3f98d3b5ac1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-u-sql"></a>U-SQL ile çalışmaya başlama
Bildirim temelli SQL kesinlik temelli izin için C# ile birleştiren bir dil olan U-SQL işlem herhangi ölçeğinde veri. U-SQL'nin ölçeklenebilir, dağıtılmış sorgu özelliği, Azure SQL veritabanı gibi ilişkisel depoları arasında veri verimli bir şekilde çözümleyebilirsiniz. U-SQL ile yapılandırılmamış veriler üzerinde okuma şema uygulama ve Özel mantık ve UDF'ler ekleme işleyebilir. Ayrıca, U-SQL ölçekte yürütmek nasıl üzerinde ayrıntılı denetim sağlar genişletilebilirlik içerir. 

## <a name="learning-resources"></a>Öğrenme kaynakları

* [U-SQL öğretici](http://aka.ms/usqltutorial) çoğu U-SQL dili için bir yol sağlar. Bu belge, U-SQL öğrenmek isteyen tüm geliştiriciler için okuma önerilir.
* Hakkında ayrıntılı bilgi için **U-SQL dili sözdizimi**, bkz: [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/p/?LinkId=691348).
* Anlamak için **U-SQL tasarımı felsefesi**, Visual Studio blog gönderisine bakın [Tanıtımı U-SQL – büyük veri işleme kolaylaştırır bir dil](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Ön koşullar

Bu belgedeki U-SQL örnekleri üzerinden geçmeden önce okuyun ve tamamlamak [öğretici: Visual Studio için Data Lake Araçları'nı kullanarak geliştirme U-SQL betikleri](data-lake-analytics-data-lake-tools-get-started.md). Bu öğretici, U-SQL Visual Studio için Azure Data Lake araçları ile kullanarak mekanizması açıklanmaktadır.

## <a name="your-first-u-sql-script"></a>İlk U-SQL betiğiniz

Aşağıdaki U-SQL betiği basittir ve U-SQL dili birçok nokta keşfetmemizi sağlar.

```
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

OUTPUT @searchlog   
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Bu betik, herhangi bir dönüştürme adım sahip değil. Adlı kaynak dosyadan okur `SearchLog.tsv`, onu schematizes ve satır geri SearchLog-first-u-sql.csv adlı bir dosyaya yazar.

Soru işareti yazın veri yanındaki fark `Duration` alan. Bu anlamına gelir `Duration` alanı null olabilir.

### <a name="key-concepts"></a>Önemli kavramlar
* **Satır kümesi değişkenleri**: satır üreten her sorgu deyimi bir değişkene atanabilir. U-SQL T-SQL değişken adlandırma deseni izler (`@searchlog`, örneğin) komut dosyası.
* **AYIKLAMAK** anahtar sözcüğü bir dosyadan veri okuyan ve şema üzerinde okuma tanımlar. `Extractors.Tsv`Sekme ayrılmış değer dosyaları için yerleşik bir U-SQL Ayıklayıcısı ' dir. Özel ayıklayıcıları geliştirebilirsiniz.
* **Çıkış** verileri bir satır kümesinden bir dosyaya yazar. `Outputters.Csv()`bir virgülle ayrılmış değer dosyası oluşturmak için yerleşik bir U-SQL outputter olur. Özel outputters geliştirebilirsiniz.

### <a name="file-paths"></a>Dosya yolları

EXTRACT ve çıktı deyimleri dosya yolları kullanın. Dosya yolları mutlak veya göreli olabilir:

Bir Data Lake Store adlı bir dosyada aşağıdaki bu mutlak dosya yolu başvurduğu `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Bu aşağıdaki dosya yolu ile başlayan `"/"`. Varsayılan Data Lake Store hesabındaki bir dosyayı ifade eder:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Skaler değişkenleri kullanma

Betik bakım daha kolay hale getirmek için skaler değişkenleri de kullanabilirsiniz. Önceki U-SQL komut dosyası olarak da yazılabilir:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Satır kümeleri dönüştürme

Kullanım **seçin** satır kümeleri dönüştürmek için:

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

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

WHERE yan tümcesi kullanan bir [C# Boole ifadesi](https://msdn.microsoft.com/library/6a71f45d.aspx). C# ifade dili kendi ifadeleri ve işlevleri yapmak için kullanabilirsiniz. Daha karmaşık mantıksal bağlaçlar (kaldırmalı) ve disjunctions (ORs) birleştirerek filtreleme bile gerçekleştirebilirsiniz.

Aşağıdaki komut dosyası DateTime.Parse() yöntemi ve bir birlikte kullanır.

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

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >İkinci sorguyu iki filtre bileşimi oluşturur ilk satır sonucu üzerinde çalışıyor. Bir değişken adı yeniden kullanabilir ve adlarını sözcüksel olarak kapsamındaki.

## <a name="aggregate-rowsets"></a>Toplu satır kümeleri
U-SQL bilindik ORDER BY, GROUP BY ve toplama sağlar.

Aşağıdaki sorgu bölge başına toplam süre bulur ve ardından üst sırada beş süreleri görüntüler.

U-SQL satır kümeleri sıralarına sonraki sorgu için korumaz. Bu nedenle, bir çıktı sipariş için çıktı ifadesine ORDER BY eklemeniz gerekir:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

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

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U-SQL ORDER BY yan tümcesi, SELECT ifadesinde FETCH yan tümcesini kullanarak gerektirir.

U-SQL sahip yan tümcesi, çıkış HAVING koşulu karşılıyor gruplarına kısıtlamak için kullanılabilir:

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

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Gelişmiş toplama senaryoları için U-SQL başvuru belgelerine bakın [toplama, çözümleme ve başvuru işlevleri](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
