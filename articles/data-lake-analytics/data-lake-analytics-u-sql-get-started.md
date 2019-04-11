---
title: Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama
description: Azure Data Lake Analytics U-SQL dili ile ilgili temel bilgileri öğrenin.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/23/2017
ms.openlocfilehash: 9de5c7228944bd0448d9dfa833ef223140ccf0e8
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59469616"
---
# <a name="get-started-with-u-sql-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics U-SQL ile çalışmaya başlama
U-SQL sorgu dili gibi kesinlik temelli izin için C# ile birleştiren bir dil olan verileri dilediğiniz ölçekte işleyin. U-SQL'nin ölçeklenebilir, dağıtılmış sorgu özelliği, Azure SQL veritabanı gibi ilişkisel depoları arasında veri verimli bir şekilde çözümleyebilirsiniz. U-SQL ile yapılandırılmamış verileri okuma sırasında şema uygulama ve Özel mantık ve UDF'ler eklemeden işleyebilir. Ayrıca, U-SQL, uygun ölçekte yürütmek nasıl üzerinde ayrıntılı denetim sağlar genişletilebilirlik içerir. 

## <a name="learning-resources"></a>Öğrenme kaynakları

* [U-SQL öğretici](https://aka.ms/usqltutorial) U-SQL dili çoğunu destekli bir kılavuz sağlar. Bu belge, U-SQL öğrenmek isteyen tüm geliştiriciler için okuma önerilir.
* İlgili ayrıntılı bilgi için **U-SQL dili sözdizimini**, bkz: [U-SQL dil başvurusu](https://go.microsoft.com/fwlink/p/?LinkId=691348).
* Anlamak için **U-SQL tasarımı felsefesi**, Visual Studio blog gönderisini inceleyin [Karşınızda U-SQL – büyük veri işleme kolay oluşturan bir dille](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Önkoşullar

U-SQL örnekleri bu belgenin üzerinden geçmeden önce okuma ve tamamlamak [Öğreticisi: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md). Bu öğretici, Visual Studio için Azure Data Lake araçları ile U-SQL kullanarak mekanizması açıklanmaktadır.

## <a name="your-first-u-sql-script"></a>İlk U-SQL betiğiniz

Aşağıdaki U-SQL betiğini basittir ve U-SQL dili birçok yönden keşfetmemizi sağlar.

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

Bu betik, herhangi bir dönüştürme adımı yok. Adlı bir kaynak dosyasından okur `SearchLog.tsv`, onu schematizes ve satır kümesi geri SearchLog-ilk-u-sql.csv adlı bir dosyaya yazar.

Soru işareti yazın verinin yanında fark `Duration` alan. Anlamına `Duration` alan null olabilir.

### <a name="key-concepts"></a>Önemli kavramlar
* **Satır kümesi değişkenleri**: Bir satır üretir her sorgu ifadesi, bir değişkene atanabilir. U-SQL T-SQL değişken adlandırma desenini izler (`@searchlog`, örneğin) bir komut.
* **AYIKLAMAK** anahtar sözcüğü bir dosyadan verileri okur ve okuma sırasında şema tanımlar. `Extractors.Tsv` Yerleşik bir U-SQL ayıklayıcısı için sekmesinde ayrılmış değer dosyaları olur. Özel ayıklayıcı geliştirebilirsiniz.
* **Çıkış** veri satır kümesinden bir dosyaya yazar. `Outputters.Csv()` bir virgülle ayrılmış değer dosyası oluşturmak için yerleşik bir U-SQL outputter olur. Özel çıktı geliştirebilirsiniz.

### <a name="file-paths"></a>Dosya yolları

AYIKLAMA ve çıkış deyimleri, dosya yolları kullanır. Dosya yolu mutlak veya göreli olabilir:

Adlı bir Data Lake Store dosyasında aşağıdaki bu mutlak dosya yolu başvurduğu `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Bu aşağıdaki dosya yolu ile başlayan `"/"`. Varsayılan Data Lake Store hesabındaki bir dosyayı ifade eder:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Skaler değişkenleri kullanma

Betik, bakım daha kolay hale getirmek için skalar değişkenler de kullanabilirsiniz. Önceki U-SQL betiği olarak da yazılabilir:

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

WHERE yan tümcesi kullanan bir [C# Boolean ifadesi](/dotnet/csharp/language-reference/operators/index). C# ifade dili kendi ifadeler ve İşlevler yapmak için kullanabilirsiniz. Ayrıca, mantıksal bağlaçlar (Equal) ve disjunctions (ORs) birleştirerek daha karmaşık filtreleme bile gerçekleştirebilirsiniz.

Aşağıdaki betiği DateTime.Parse() yöntemi ve bir birlikte kullanır.

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
 >İkinci sorgu, iki filtre bileşimini oluşturan ilk satır sonucu üzerinde çalışıyor. Bir değişken adı da kullanabilirsiniz ve adları sözcüksel olarak belirlenir.

## <a name="aggregate-rowsets"></a>Toplu satır kümeleri
U-SQL, tanıdık ORDER BY, GROUP BY ve toplama sağlar.

Aşağıdaki sorgu, bölge başına toplam süre bulur ve üst beş süreleri düzende görüntüler.

U-SQL satır kümeleri sonraki sorgu için bunların sırası korumaz. Bu nedenle, bir çıkış sıralamak için çıkış deyime ORDER BY eklemeniz gerekir:

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

U-SQL ORDER BY yan tümcesi, SELECT ifadesinde FETCH yan tümcesini gerektirir.

U-SQL HAVING tümcesine çıkış HAVING koşulu karşılayan gruplarına kısıtlamak için kullanılabilir:

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

Gelişmiş toplama senaryoları için U-SQL başvuru belgelerine bakın [toplama, analiz ve başvuru işlevleri](/u-sql/built-in-functions)

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
