---
title: "U-SQL betikleri Azure Data Lake Analytics R ile genişletme | Microsoft Docs"
description: "U-SQL betikleri R kodu çalıştırma öğrenin"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: d479af515566f497d9611e75426f6acb8f8276d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Öğretici: U-SQL R ile genişletme ile çalışmaya başlama

Aşağıdaki örnek R kodu dağıtmak için temel adımlar gösterilmektedir:
* Kullanım `REFERENCE ASSEMBLY` R uzantıları için U-SQL betiği etkinleştirme bildirimi.
* Kullanım` REDUCE` bir anahtar üzerindeki giriş verisi bölümlemek için işlemi.
* U-SQL R uzantıları yerleşik reducer içerir (`Extension.R.Reducer`), R kodu üzerinde çalıştığı için reducer atanan her köşe. 
* Ayrılmış kullanımını adlı adlı veri çerçevelerini `inputFromUSQL` ve `outputToUSQL `sırasıyla U-SQL ve r giriş arasında veri iletmek ve tanımlayıcı adları düzeltilen DataFrame çıkış (diğer bir deyişle, kullanıcılar bu önceden tanımlanmış adları giriş değiştirip edemezsiniz DataFrame tanımlayıcıları çıktı).

## <a name="embedding-r-code-in-the-u-sql-script"></a>U-SQL betiği R kodu katıştırma

Satır içi R kodu, U-SQL betiği komut parametresini kullanarak yapabilirsiniz `Extension.R.Reducer`. Örneğin, bir dize değişkeni olarak R betiği bildirme ve Reducer için parametre olarak geçirin.


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>R kodu ayrı bir dosyaya tutmak ve U-SQL betiği başvuru

Aşağıdaki örnek, daha karmaşık bir kullanımı gösterilmektedir. Bu durumda, R kodu U-SQL komut dosyası bir kaynak olarak dağıtılır.

Bu R kodu ayrı bir dosya olarak kaydedin.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

U-SQL betiği bu R betiği dağıtmak kaynağı deyim ile dağıtmak için kullanın.

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a>R U-SQL ile nasıl tümleşik çalıştığını

### <a name="datatypes"></a>Veri türleri
* U-SQL dizesi ve sayısal sütunlarından olarak dönüştürülür-R DataFrame ve U-SQL arasında [desteklenen türleri: `double`, `string`, `bool`, `integer`, `byte`].
* `Factor` U-SQL veri türü desteklenmiyor.
* `byte[]`bir base64 ile kodlanmış serileştirilmesi gereken `string`.
* U-SQL dizeleri faktörleri R kodda U-SQL oluşturduktan sonra R giriş dataframe veya reducer parametresini ayarlayarak dönüştürülebilir `stringsAsFactors: true`.

### <a name="schemas"></a>Şemalar
* U-SQL veri kümeleri yinelenen sütun adlarına sahip olamaz.
* U-SQL veri kümeleri sütun adlarının dize olması gerekir.
* Sütun adları U-SQL ve R betiklerini aynı olması gerekir.
* ReadOnly sütunu çıktı dataframe parçası olamaz. Çıktı şeması UDO parçasıysa, salt okunur sütunlar U-SQL tabloda otomatik olarak eklenmiş çünkü.

### <a name="functional-limitations"></a>İşlev sınırlamaları
* R altyapısı iki kere aynı işlemde örneği oluşturulamıyor. 
* Şu anda, U-SQL için tahmini Reducer Udo'lar kullanılarak oluşturulan bölümlenmiş modelleri kullanarak Birleştirici Udo'lar desteklemiyor. Kullanıcılar kaynak olarak bölümlenmiş modelleri bildirme ve bunların R betiği kullanın (örnek kodu görmek `ExtR_PredictUsingLMRawStringReducer.usql`)

### <a name="r-versions"></a>R sürümleri
Yalnızca R 3.2.2 desteklenir.

### <a name="standard-r-modules"></a>Standart R modülleri

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a>Giriş ve çıkış boyutu sınırlamaları
Her köşe atanmış bellek sınırlı miktarda sahiptir. Giriş ve çıkış DataFrames R kodu bellekte varolması gerektiğinden, giriş ve çıkış toplam boyutu 500 MB aşamaz.

### <a name="sample-code"></a>Örnek kod
U-SQL Advanced Analytics extensions yüklendikten sonra daha fazla örnek kod, Data Lake Store hesabınızdaki kullanılabilir. Daha fazla örnek kod yoludur: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>U-SQL ile özel R modülleri dağıtma

İlk olarak, bir R özel modülü oluşturun ve onu zip ve ADL deponuza daraltılmış R özel modülü dosyayı karşıya yükleme. Örnekte, biz kullanıyoruz ADLA hesabı için varsayılan ADLS hesabı kökünde magittr_1.5.zip yükleyeceksiniz. ADL deposuna modül karşıya yükledikten sonra U-SQL komut dosyasını ve arama kullanabilmesi için dağıtmak kaynağı kullanmak gibi bildirme `install.packages` yükleyin.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Data Lake Analytics işleri için U-SQL penceresi işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
