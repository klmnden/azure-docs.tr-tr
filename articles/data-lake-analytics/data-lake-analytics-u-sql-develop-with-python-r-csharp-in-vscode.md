---
title: "Azure Data Lake Analytics Visual Studio'da kod için U-SQL Python, R ve CSharp ile geliştirme | Microsoft Docs"
description: "Arka plan kod Python, R ve CSharp ile Azure Data Lake işi göndermek için nasıl kullanılacağını öğrenin."
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: 
editor: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/22/2017
ms.author: jejiang
ms.openlocfilehash: 82f6527388017aadecf761871f5acb25eb100acb
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="develop-u-sql-with-python-r-and-csharp-for-azure-data-lake-analytics-in-visual-studio-code"></a>U-SQL Python, R ve Visual Studio code'da Azure Data Lake Analytics CSharp ile geliştirme
Python yazmak için Visual Studio Code (VSCode) kullanmayı öğrenin, R ve CSharp arkasında U-SQL ile kod ve Azure Data Lake hizmeti göndermek. VSCode için Azure Data Lake araçları hakkında daha fazla bilgi için bkz: [Azure Data Lake araçları kullanmak için Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

Arka plan kodu özel kod yazmadan önce bir klasörü veya bir çalışma alanı içinde VSCode açmanız gerekir.


## <a name="prerequisites-for-python-and-r"></a>Python ve R için Önkoşullar
Python ve, R uzantıları derlemeler ADL hesabınız için kaydolun. 
1. Portal'da hesabınızı açın.
   - Seçin **genel bakış**. 
   - Tıklatın **örnek komut dosyası**.
2. Tıklatın **daha fazla**.
3. Seçin **yükleme U-SQL uzantıları**. 
4. U-SQL uzantıları yüklendikten sonra onay iletisi görüntülenir. 

  ![Python ve R ortamını ayarlama](./media/data-lake-analytics-data-lake-tools-for-vscode/setup-the-enrionment-for-python-and-r.png)

  > [!Note]
  > Python ve R dil hizmeti ile ilgili en iyi deneyimler için lütfen VSCode Python ve R uzantısını yükleyin. 

## <a name="develop-python-file"></a>Python dosyası geliştirin
1. Tıklatın **yeni dosya** çalışma alanınızdaki.
2. U-SQL kodunuzu yazın. Kod örneği verilmiştir.
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
    
3. Bir komut dosyasını sağ tıklatın ve ardından **ADL: arkasında Python kodu dosyası oluştur**. 
4. **Xxx.usql.py** dosyası çalışma klasöründe oluşturulur. Kodunuzu Python dosyasında yazın. Kod örneği verilmiştir.

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
5. Sağ tıklatın, **USQL** tıklayabilirsiniz dosyası **derleme betik** veya **işi Gönder** işi çalıştırmak için.

## <a name="develop-r-file"></a>R dosya geliştirin
1. Tıklatın **yeni dosya** çalışma alanınızdaki.
2. U-SQL dosyasında kodunuzu yazın. Kod örneği verilmiştir.
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
3. Sağ **USQL** dosya ve ardından **ADL: arkasında R kod dosyası oluştur**. 
4. **Xxx.usql.r** dosyası çalışma klasöründe oluşturulur. Kodunuzu R dosyasında yazın. Kod örneği verilmiştir.

    ```R
    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence"))
    ```
5. Sağ tıklatın, **USQL** tıklayabilirsiniz dosyası **derleme betik** veya **işi Gönder** işi çalıştırmak için.

## <a name="develop-c-file"></a>C# dosyasına geliştirin
Tek bir U-SQL betiği ile ilişkili bir C# dosyasına bir arka plan kodu dosyasıdır. UDO, UDA, UDT ve arka plan kod dosyasına UDF için adanmış bir komut dosyası tanımlayabilirsiniz. UDO, UDA, UDT ve UDF doğrudan komut dosyası derleme ilk kaydettirmeden kullanılabilir. Kendi eşleme U-SQL komut dosyası ile aynı klasörde arka plan kod dosyasına yerleştirin. Komut dosyası xxx.usql olarak adlandırılmışsa, arka plan kodu xxx.usql.cs adlandırılır. Arka plan kodu dosyasını el ile silin, arka plan kodu özelliği, ilişkili U-SQL betiği için devre dışı bırakılır. U-SQL betiği için müşteri kod yazma hakkında daha fazla bilgi için bkz: [yazma ve özel kodda kullanarak U-SQL: kullanıcı tanımlı işlevler]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

1. Tıklatın **yeni dosya** çalışma alanınızdaki.
2. U-SQL dosyasında kodunuzu yazın. Kod örneği verilmiştir.
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
3. Sağ **USQL** dosya ve ardından **ADL: arkasında CS kod dosyası oluştur**. 
4. **Xxx.usql.cs** dosyası çalışma klasöründe oluşturulur. Kodunuzu CS dosyasında yazın. Kod örneği verilmiştir.

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
5. Sağ tıklatın, **USQL** tıklayabilirsiniz dosyası **derleme betik** veya **işi Gönder** işi çalıştırmak için.

## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Azure Data Lake Analytics işleri için U-SQL derlemeleri geliştirin](data-lake-analytics-u-sql-develop-assemblies.md)
* [PowerShell kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure Portalı'nı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanın](data-lake-analytics-data-lake-tools-get-started.md)
* [Kullanım Data Lake Analytics(U-SQL) Kataloğu](data-lake-analytics-use-u-sql-catalog.md)
