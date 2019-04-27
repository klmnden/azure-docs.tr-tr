---
title: U-SQL Python, R ve Azure Data Lake Analytics, Visual Studio Code için C# ile geliştirme
description: Python, R ve C# ile bir Azure veri Gölü'nde işi göndermek için arka plan kod kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 11/22/2017
ms.openlocfilehash: 6c234ad6756f4e65e172bf0ffc0ae5a1d35d109b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814074"
---
# <a name="develop-u-sql-with-python-r-and-c-for-azure-data-lake-analytics-in-visual-studio-code"></a>U-SQL Python, R ve Azure Data Lake Analytics, Visual Studio Code için C# ile geliştirme
Python yazmak için Visual Studio Code (VSCode) kullanmayı öğrenin, R ve C# ile U-SQL arkasındaki kod ve iş göndermek için Azure Data Lake hizmeti. VSCode için Azure Data Lake araçları hakkında daha fazla bilgi için bkz. [Visual Studio Code için Azure Data Lake araçları kullanmak](data-lake-analytics-data-lake-tools-for-vscode.md).

Arka plan kod özel kod yazmadan önce bir klasör veya bir çalışma alanı VSCode içinde açmanız gerekir.


## <a name="prerequisites-for-python-and-r"></a>Python ve R için Önkoşullar
Python ve R uzantıları derlemeleri ADL hesabınız için kaydolun. 
1. Hesabınızı portalda açın.
   - **Genel Bakış**’ı seçin. 
   - Tıklayın **örnek komut dosyası**.
2. Tıklayın **daha fazla**.
3. Seçin **yükleyin U-SQL uzantıları**. 
4. U-SQL Uzantıları yükledikten sonra onay iletisi görüntülenir. 

   ![Python ve R için ortamı ayarlama](./media/data-lake-analytics-data-lake-tools-for-vscode/setup-the-enrionment-for-python-and-r.png)

   > [!Note]
   > Python ve R dil hizmeti ile ilgili en iyi deneyimler için lütfen VSCode Python ve R uzantıyı yükleyin. 

## <a name="develop-python-file"></a>Soubor Pythonu geliştirin
1. Tıklayın **yeni dosya** çalışma alanınızdaki.
2. U-SQL kodunuzu yazın. Bir kod örneği verilmiştir.
    ```U-SQL
    REFERENCE ASSEMBLY [ExtPython];
    @t  = 
        SELECT * FROM 
        (VALUES
            ("D1","T1","A1","@foo Hello World @bar"),
            ("D2","T2","A2","@baz Hello World @beer")
        ) AS 
            D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer("pythonSample.usql.py", pyVersion : "3.5.1");

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();
    ```
    
3. Bir komut dosyasını sağ tıklatın ve ardından **ADL: Python arka plan kod dosyasında oluşturmak**. 
4. **Xxx.usql.py** dosyası kullanarak çalışma klasörünüzde oluşturulur. Python dosyasında kodunuzu yazın. Bir kod örneği verilmiştir.

    ```Python
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ```
5. Sağ **USQL** dosyası tıklayabilirsiniz **derleme betiği** veya **işi Gönder** işi çalıştırmak için.

## <a name="develop-r-file"></a>R dosya geliştirin
1. Tıklayın **yeni dosya** çalışma alanınızdaki.
2. U-SQL dosya içinde kodunuzu yazın. Bir kod örneği verilmiştir.
    ```U-SQL
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT SepalLength double,
                SepalWidth double,
                PetalLength double,
                PetalWidth double,
                Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput =
        REDUCE @ExtendedData
        ON Par
        PRODUCE Par,
                fit double,
                lwr double,
                upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile : "RClusterRun.usql.R", rReturnType : "dataframe", stringsAsFactors : false);
    OUTPUT @RScriptOutput
    TO @OutputFilePredictions
    USING Outputters.Tsv();
    ```
3. Sağ **USQL** dosya ve ardından **ADL: Dosyanın arkasındaki R kodu**. 
4. **Xxx.usql.r** dosyası kullanarak çalışma klasörünüzde oluşturulur. R dosyasında kodunuzu yazın. Bir kod örneği verilmiştir.

    ```R
    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence"))
    ```
5. Sağ **USQL** dosyası tıklayabilirsiniz **derleme betiği** veya **işi Gönder** işi çalıştırmak için.

## <a name="develop-c-file"></a>C# dosyası geliştirin
Tek bir U-SQL betiği ile ilişkili bir C# dosyasını bir arka plan kod dosyasıdır. Özel bir betik UDO, UDA, UDT ve UDF arka plan kod dosyasında tanımlayabilirsiniz. UDO, UDA, UDT ve UDF doğrudan betikte derleme ilk kayıt olmadan kullanılabilir. Arka plan kod dosyası eşleme, U-SQL komut dosyasını aynı klasöre yerleştirin. Xxx.usql adlandırılan bu betik, arka plan kod xxx.usql.cs adlandırılır. Arka plan kod dosyasına el ile silmeniz, arka plan kod özelliği, ilişkili bir U-SQL betiği için devre dışı bırakılır. U-SQL betiği için müşteri kod yazma hakkında daha fazla bilgi için bkz. [yazma ve U-SQL'de özel kod kullanma: Kullanıcı tanımlı işlevleri]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

1. Tıklayın **yeni dosya** çalışma alanınızdaki.
2. U-SQL dosya içinde kodunuzu yazın. Bir kod örneği verilmiştir.
    ```U-SQL
    @a = 
        EXTRACT 
            Iid int,
        Starts DateTime,
        Region string,
        Query string,
        DwellTime int,
        Results string,
        ClickedUrls string 
        FROM @"/Samples/Data/SearchLog.tsv" 
        USING Extractors.Tsv();

    @d =
        SELECT DISTINCT Region 
        FROM @a;

    @d1 = 
        PROCESS @d
        PRODUCE 
            Region string,
        Mkt string
        USING new USQLApplication_codebehind.MyProcessor();

    OUTPUT @d1 
        TO @"/output/SearchLogtest.txt" 
        USING Outputters.Tsv();
    ```
3. Sağ **USQL** dosya ve ardından **ADL: CS arka plan kod dosyasında oluşturmak**. 
4. **Xxx.usql.cs** dosyası kullanarak çalışma klasörünüzde oluşturulur. CS dosyasında kodunuzu yazın. Bir kod örneği verilmiştir.

    ```CS
    namespace USQLApplication_codebehind
    {
        [SqlUserDefinedProcessor]

        public class MyProcessor : IProcessor
        {
            public override IRow Process(IRow input, IUpdatableRow output)
            {
                output.Set(0, input.Get<string>(0));
                output.Set(1, input.Get<string>(0));
                return output.AsReadOnly();
            } 
        }
    }
    ```
5. Sağ **USQL** dosyası tıklayabilirsiniz **derleme betiği** veya **işi Gönder** işi çalıştırmak için.

## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
* [PowerShell kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanma](data-lake-analytics-data-lake-tools-get-started.md)
* [Data Lake kullanma Analytics(U-SQL) Kataloğu](data-lake-analytics-use-u-sql-catalog.md)
