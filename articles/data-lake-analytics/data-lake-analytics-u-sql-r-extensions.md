---
title: U-SQL betikleri Azure Data Lake Analytics, R ile genişletme
description: R kodunu kullanarak Azure Data Lake Analytics U-SQL betiklerini çalıştırma hakkında bilgi edinin
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: 59a52b2aeb83732a608f1fcf5bc4de907d25dfd1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813749"
---
# <a name="extend-u-sql-scripts-with-r-code-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics R kodu ile U-SQL betikleri genişletin

Aşağıdaki örnek, R kodu dağıtmaya ilişkin temel adımları gösterir:
* Kullanım `REFERENCE ASSEMBLY` R uzantıları için U-SQL betiği etkinleştirmek için bildirimi.
* Kullanım `REDUCE` anahtarındaki giriş verileri bölümlere işlemi.
* U-SQL R uzantıları yerleşik Azaltıcı içerir (`Extension.R.Reducer`), R kod üzerinde çalıştığı her köşe için Azaltıcı atanmış. 
* Adanmış kullanımını adlı adlı veri çerçevelerini `inputFromUSQL` ve `outputToUSQL` sırasıyla U-SQL ve r giriş arasında veri iletmek ve tanımlayıcı adları düzeltilen DataFrame çıktı (diğer bir deyişle, kullanıcıların önceden tanımlanmış bu adlar giriş değiştirip edemezsiniz DataFrame çıkış tanımlayıcılar).

## <a name="embedding-r-code-in-the-u-sql-script"></a>U-SQL betiği içinde R kod ekleme

Satır içi R komut parametresi'nı kullanarak U-SQL betiğinizi kod yapabilecekleriniz `Extension.R.Reducer`. Örneğin, R betiği bir dize değişkeni olarak bildirmek ve azaltıcı için bir parametre olarak geçirebilirsiniz.


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

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>R kodu ayrı bir dosyada tutmak ve U-SQL komut dosyası başvurusu

Aşağıdaki örnek, daha karmaşık bir kullanımı gösterilmektedir. Bu durumda, R kodunun, U-SQL betiği bir kaynak dağıtılır.

Bu R kodu ayrı bir dosya olarak kaydedin.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

U-SQL betiği ile kaynak dağıtma deyimi, R betiğini dağıtmak için kullanın.

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

## <a name="how-r-integrates-with-u-sql"></a>R U-SQL ile nasıl tümleştirildiğini

### <a name="datatypes"></a>Veri türleri
* U-SQL dizesi ve sayısal sütunları olarak dönüştürülür-R DataFrame ve U-SQL [desteklenen türleri: `double`, `string`, `bool`, `integer`, `byte`].
* `Factor` Veri türü, U-SQL'de desteklenmiyor.
* `byte[]` bir base64 kodlamalı seri hale getirilmelidir `string`.
* U-SQL dizeleri Etkenler R kodunu, U-SQL R giriş dataframe oluşturduğunuzda veya azaltıcı parametresini ayarlayarak dönüştürülebilir `stringsAsFactors: true`.

### <a name="schemas"></a>Şemalar
* U-SQL veri kümeleri, yinelenen sütun adlarına sahip olamaz.
* U-SQL veri kümeleri sütun adları dize olmalıdır.
* Sütun adları, U-SQL ve R betikleri aynı olmalıdır.
* Salt okunur sütun çıkış dataframe parçası olamaz. Çıkış şeması UDO'ın bir parçası ise salt okunur sütunlar U-SQL tabloda otomatik olarak eklenmiş olduğundan.

### <a name="functional-limitations"></a>İşlev sınırlamaları
* R motoru, aynı işlem içinde iki kez başlatılamaz. 
* Şu anda, U-SQL Birleştirici Udo'lar Azaltıcı Udo'lar kullanılarak oluşturulan bölümlenmiş modelleri kullanarak tahmin için desteklemez. Kullanıcıların kaynak olarak bölümlenmiş modelleri bildirebilir ve bunları kendi R betik kullanın (örnek kodu görmek `ExtR_PredictUsingLMRawStringReducer.usql`)

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
Her köşe sınırlı miktarda bellek atandığını sahiptir. Girdi ve çıktı veri çerçevelerini R kodunu bellekte varolması gerektiğinden, giriş ve çıkış toplam boyutu 500 MB'tan büyük olamaz.

### <a name="sample-code"></a>Örnek Kod
U-SQL Gelişmiş Analiz uzantıları yükledikten sonra daha fazla örnek kod, Data Lake Store hesabınızdaki kullanılabilir. Daha fazla örnek kod yolu: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>U-SQL ile özel R modülleri dağıtma

İlk olarak, özel bir R modülü oluşturmak ve onu zip ve ADL deponuza sıkıştırılmış R Özel Modül dosyası yükleyin. Bu örnekte biz varsayılan ADLS hesabını kullanıyoruz ADLA hesabı için kök magittr_1.5.zip yükleyeceksiniz. ADL deposuna modülü karşıya yükledikten sonra U-SQL betiği ve çağrı kullanabilmesi için kaynak dağıtma kullanma gibi bildirmek `install.packages` yükleyin.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomPackages.txt";

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
* [Azure Data Lake Analytics işleri için U-SQL pencere işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
