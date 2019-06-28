---
title: 'Öğretici: Kullanım R bir Spark işlem bağlamında Azure HDInsight'
description: Öğretici - R ve Spark ML hizmetleri kullanmaya başlayın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/21/2019
ms.openlocfilehash: 244c62467f187417bbb9f0e54173aad5a7d26d0a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450285"
---
# <a name="tutorial-use-r-in-a-spark-compute-context-in-azure-hdinsight"></a>Öğretici: Kullanım R bir Spark işlem bağlamında Azure HDInsight

Bu öğretici, Azure HDInsight'ın bir ML Hizmetleri kümesinde çalışan Apache Spark, R işlevlerini kullanarak adım adım bir giriş sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yerel depolama alanına örnek verileri indirme
> * Varsayılan depolama alanına veri kopyalama
> * Veri kümesi ayarlayın
> * Veri kaynağı oluşturma
> * Spark için işlem bağlamı oluşturma
> * Bir Doğrusal model Sığdır
> * Bileşik XDF dosyalarını kullanma
> * XDF CSV'ye Dönüştür

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde ML Hizmetleri kümesi. Bkz: [Apache Hadoop kümeleri oluşturma Azure portalını kullanarak](../hdinsight-hadoop-create-linux-clusters-portal.md) seçip **ML Hizmetleri** için **küme türü**.

## <a name="connect-to-rstudio-server"></a>RStudio Server’a bağlanma

Kümenin kenar düğümünde RStudio Server çalışır. Aşağıdaki URL'ye gidin burada `CLUSTERNAME` oluşturduğunuz ML Hizmetleri kümesinin adıdır:

```
https://CLUSTERNAME.azurehdinsight.net/rstudio/
```

İlk kez oturum açtığınızda iki kez kimlik doğrulaması gerekir. Küme Yöneticisi oturum açma ve parola ilk kimlik doğrulaması isteminde sağlarsanız, varsayılan `admin`. İkinci kimlik doğrulaması isteminde SSH kullanıcı adı ve parola sağlarsanız, varsayılan `sshuser`. Sonraki oturum açma işlemleri yalnızca SSH kimlik bilgileri gerektirir.

## <a name="download-sample-data"></a>Örnek veri indirin

*Havayolu 2012 zamanlı veri kümesi* yılı 2012 için ABD içindeki tüm ticari uçuşlar için uçuş varış ve ayrılma ayrıntıları hakkında bilgi içeren 12 virgülle ayrılmış dosyalar oluşur. Büyük bir veri kümesi ile 6 milyonun üzerinde gözlem budur.

1. Birkaç ortamı değişkenlerini başlatın. RStudio Server konsolunda aşağıdaki kodu girin:

    ```R
    bigDataDirRoot <- "/tutorial/data" # root directory on cluster default storage
    localDir <- "/tmp/AirOnTimeCSV2012" # directory on edge node
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012" # location of data
    ```

    Değişkenleri altında ekranın sağ tarafında görünür **ortam** sekmesi.

    ![RStudio](./media/ml-services-tutorial-spark-compute/rstudio.png)

2.  Yerel bir dizin oluşturun ve örnek verileri indirme. RStudio içinde aşağıdaki kodu girin:

    ```R
    # Create local directory
    dir.create(localDir)
    
    # Download data to the tmp folder(local)
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(localDir, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(localDir, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(localDir, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(localDir, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(localDir, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(localDir, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(localDir, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(localDir, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(localDir, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(localDir, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(localDir, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(localDir, "airOT201212.csv"))
    ```

    İndirme hakkında 9 ve yarı bir dakika içinde tamamlanır.

## <a name="copy-data-to-default-storage"></a>Varsayılan depolama alanına veri kopyalama

HDFS konum ile belirtilen `airDataDir` değişkeni. RStudio içinde aşağıdaki kodu girin:

```R
# Set directory in bigDataDirRoot to load the data into
airDataDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

# Create directory (default storage)
rxHadoopMakeDir(airDataDir)

# Copy data from local storage to default storage
rxHadoopCopyFromLocal(localDir, bigDataDirRoot)
    
# Optional. Verify files
rxHadoopListFiles(airDataDir)
```

Adım 10 saniye içinde tamamlanmalıdır.

## <a name="set-up-data-set"></a>Veri kümesi ayarlayın

1. Varsayılan değerleri kullanan bir dosya sistemi nesnesi oluşturun. RStudio içinde aşağıdaki kodu girin:

    ```R
    # Define the HDFS (WASB) file system
    hdfsFS <- RxHdfsFileSystem()
    ```

2. Biz sağlamak için çok kullanışsız değişken adları, orijinal CSV dosyalarına sahip bir `colInfo` daha kolay yönetilebilir hale getirmek için liste. RStudio içinde aşağıdaki kodu girin:

    ```R
    airlineColInfo <- list(
         MONTH = list(newName = "Month", type = "integer"),
        DAY_OF_WEEK = list(newName = "DayOfWeek", type = "factor",
            levels = as.character(1:7),
            newLevels = c("Mon", "Tues", "Wed", "Thur", "Fri", "Sat",
                          "Sun")),
        UNIQUE_CARRIER = list(newName = "UniqueCarrier", type =
                                "factor"),
        ORIGIN = list(newName = "Origin", type = "factor"),
        DEST = list(newName = "Dest", type = "factor"),
        CRS_DEP_TIME = list(newName = "CRSDepTime", type = "integer"),
        DEP_TIME = list(newName = "DepTime", type = "integer"),
        DEP_DELAY = list(newName = "DepDelay", type = "integer"),
        DEP_DELAY_NEW = list(newName = "DepDelayMinutes", type =
                             "integer"),
        DEP_DEL15 = list(newName = "DepDel15", type = "logical"),
        DEP_DELAY_GROUP = list(newName = "DepDelayGroups", type =
                               "factor",
           levels = as.character(-2:12),
           newLevels = c("< -15", "-15 to -1","0 to 14", "15 to 29",
                         "30 to 44", "45 to 59", "60 to 74",
                         "75 to 89", "90 to 104", "105 to 119",
                         "120 to 134", "135 to 149", "150 to 164",
                         "165 to 179", ">= 180")),
        ARR_DELAY = list(newName = "ArrDelay", type = "integer"),
        ARR_DELAY_NEW = list(newName = "ArrDelayMinutes", type =
                             "integer"),  
        ARR_DEL15 = list(newName = "ArrDel15", type = "logical"),
        AIR_TIME = list(newName = "AirTime", type =  "integer"),
        DISTANCE = list(newName = "Distance", type = "integer"),
        DISTANCE_GROUP = list(newName = "DistanceGroup", type =
                             "factor",
         levels = as.character(1:11),
         newLevels = c("< 250", "250-499", "500-749", "750-999",
             "1000-1249", "1250-1499", "1500-1749", "1750-1999",
             "2000-2249", "2250-2499", ">= 2500")))
    
    varNames <- names(airlineColInfo)
    ```

## <a name="create-data-source"></a>Veri kaynağı oluşturma

Bir Spark işlem bağlamında aşağıdaki işlevleri kullanarak veri kaynakları oluşturabilirsiniz:

|İşlev | Açıklama |
|---------|-------------|
|`RxTextData` | Bir virgülle ayrılmış metin veri kaynağı. |
|`RxXdfData` | Verileri XDF verileri dosya biçiminde. RevoScaleR XDF dosyası biçimi dosyaları yerine tek bir dosyayı bileşik kümesindeki verileri depolamak Hadoop için değiştirildi. |
|`RxHiveData` | Bir veri kaynağı yığın nesnesi oluşturur.|
|`RxParquetData` | Parquet veri kaynağı nesnesi oluşturur.|
|`RxOrcData` | Orc veri kaynağı nesnesi oluşturur.|

Oluşturma bir [RxTextData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxtextdata) HDFS'ye kopyalanan dosyaların kullanarak nesne. RStudio içinde aşağıdaki kodu girin:

```R
airDS <- RxTextData( airDataDir,
                        colInfo = airlineColInfo,
                        varsToKeep = varNames,
                        fileSystem = hdfsFS ) 
```

## <a name="create-compute-context-for-spark"></a>Spark için işlem bağlamı oluşturma

Verileri yüklemek ve analizleri çalışan düğümleri üzerinde çalışmak için işlem bağlamı için betiğinizde ayarladığınız [dizüstü](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxspark). Bu bağlamda R işlevleri otomatik olarak iş yükü tüm çalışan düğümleri, işler veya sıra yönetmek için yerleşik bir gereksinim dağıtabilirsiniz. Spark işlem bağlamına yoluyla kurulmuş `RxSpark` veya `rxSparkConnect()` Spark işlem bağlamına ve kullandığı oluşturmak için `rxSparkDisconnect()` yerel işlem bağlamına dönün. RStudio içinde aşağıdaki kodu girin:

```R
# Define the Spark compute context
mySparkCluster <- RxSpark()

# Set the compute context
rxSetComputeContext(mySparkCluster)
```

## <a name="fit-a-linear-model"></a>Bir Doğrusal model Sığdır

1. Kullanım [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) işlevini kullanarak bir Doğrusal model uyacak şekilde, `airDS` veri kaynağı. RStudio içinde aşağıdaki kodu girin:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS,
              cube = TRUE)
    )
    ```
    
    Bu adım 2 ve 3 dakika arası tamamlamanız gerekir.

1. Sonuçlara bakın. RStudio içinde aşağıdaki kodu girin:

    ```R
    summary(delayArr)
    ```

    Aşağıdaki sonuçları görmeniz gerekir:

    ```output
    Call:
    rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS, cube = TRUE)
    
    Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
    Data: airDataXdf (RxXdfData Data Source)
    File name: /tutorial/data/AirOnTimeCSV2012
    Dependent variable(s): ArrDelay
    Total independent variables: 7 
    Number of valid observations: 6005381
    Number of missing observations: 91381 
     
    Coefficients:
                   Estimate Std. Error t value Pr(>|t|)     | Counts
    DayOfWeek=Mon   3.54210    0.03736   94.80 2.22e-16 *** | 901592
    DayOfWeek=Tues  1.80696    0.03835   47.12 2.22e-16 *** | 855805
    DayOfWeek=Wed   2.19424    0.03807   57.64 2.22e-16 *** | 868505
    DayOfWeek=Thur  4.65502    0.03757  123.90 2.22e-16 *** | 891674
    DayOfWeek=Fri   5.64402    0.03747  150.62 2.22e-16 *** | 896495
    DayOfWeek=Sat   0.91008    0.04144   21.96 2.22e-16 *** | 732944
    DayOfWeek=Sun   2.82780    0.03829   73.84 2.22e-16 *** | 858366
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 35.48 on 6005374 degrees of freedom
    Multiple R-squared: 0.001827 (as if intercept included)
    Adjusted R-squared: 0.001826 
    F-statistic:  1832 on 6 and 6005374 DF,  p-value: < 2.2e-16 
    Condition number: 1 
    ```

    Uyarı sonuçları biz veri işleme belirtmek belirtilen dizindeki tüm .csv dosyası kullanarak, altı milyon gözlemler. Biz belirttiğinden de dikkat `cube = TRUE`, hafta (ve ıntercept) her günü için tahmini bir katsayısı sahibiz.

## <a name="use-composite-xdf-files"></a>Bileşik XDF dosyalarını kullanma

Anlatıldığı gibi Hadoop CSV dosyalarını doğrudan R ile analiz edebilirsiniz, ancak verileri daha verimli bir biçimde depolanıyorsa, daha hızlı analiz yapılabilir. R .xdf biçimi, son derece verimlidir, ancak tek tek dosyalar tek bir HDFS blok içinde kalmasını sağlamak için HDFS biraz değiştirilir. (HDFS blok boyutu yükleme başka bir yükleme değişir ancak genellikle 64 MB'ı veya 128 MB'dır.) Kullanırken [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) Hadoop üzerinde belirttiğiniz bir `RxTextData` veri kaynağı gibi `AirDS` inData olarak ve bir `RxXdfData` dosya sistemi veri kaynağıyla bir dizi oluşturmak için ÇıkışDosyası bağımsız değişkeni olarak bir HDFS dosya sistemine ayarlayın Bileşik .xdf dosyaları. `RxXdfData` Nesne sonraki R analizleri veri bağımsız değişken olarak kullanılabilecek.

1. Tanımlayan bir `RxXdfData` nesne. RStudio içinde aşağıdaki kodu girin:

    ```R
    airDataXdfDir <- file.path(bigDataDirRoot,"AirOnTimeXDF2012")
    
    airDataXdf <- RxXdfData( airDataXdfDir,
                            fileSystem = hdfsFS )
    ```

1. Bir blok boyutu 250000 satır kümesi ve tüm verileri okumasını, belirtin. RStudio içinde aşağıdaki kodu girin:

    ```R
    blockSize <- 250000
    numRowsToRead = -1
    ```

1. Kullanarak verileri içeri aktarma `rxImport`. RStudio içinde aşağıdaki kodu girin:

    ```R
    rxImport(inData = airDS,
             outFile = airDataXdf,
             rowsPerRead = blockSize,
             overwrite = TRUE,
             numRows = numRowsToRead )
    ```
    
    Bu adım birkaç dakika içinde tamamlanır.

1. Yeni ve daha hızlı veri kaynağını kullanma aynı bir Doğrusal model yeniden tahmin edin. RStudio içinde aşağıdaki kodu girin:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDataXdf,
              cube = TRUE)
    )
    ```
    
    Adım bir dakika içinde tamamlanır.

1. Sonuçlara bakın. Sonuçları CSV dosyası olduğu gibi aynı olmalıdır. RStudio içinde aşağıdaki kodu girin:

    ```R
    summary(delayArr)
    ```

## <a name="convert-xdf-to-csv"></a>XDF CSV'ye Dönüştür

### <a name="in-a-spark-context"></a>Spark bağlamında

Analizleri çalıştırılırken verimliliğini yararlanın, ancak artık verilerinizi geri CSV'ye Dönüştür istediğiniz XDF için CSV dönüştürdüyseniz kullanarak bunu yapabilirsiniz [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep).

CSV dosyaları bir klasör oluşturmak için öncelikle oluşturma bir `RxTextData` dosya bağımsız değişkeni bir dizin adı kullanarak nesne; bu CSV dosyaları oluşturulacağı klasör temsil eder. Bu dizin, çalıştırdığınızda oluşturulan `rxDataStep`. Ardından, işaret için `RxTextData` nesnesine `outFile` bağımsız değişkeni `rxDataStep`. Oluşturulan her bir CSV, dizin adı göre ve ardından bir sayı olarak adlandırılacaktır.

Bir klasörü HDFS CSV'ye dışarı yazmak istiyoruz varsayalım bizim `airDataXdf` Lojistik regresyon ve tahmin, böylece Kalanlar ve tahmin edilen değerleri yeni CSV dosyaları içeren gerçekleştirdiğimiz sonra bileşik XDF. RStudio içinde aşağıdaki kodu girin:

```R
airDataCsvDir <- file.path(bigDataDirRoot,"AirDataCSV2012")
airDataCsvDS <- RxTextData(airDataCsvDir,fileSystem=hdfsFS)
rxDataStep(inData=airDataXdf, outFile=airDataCsvDS)
```

Bu adım, yaklaşık iki bir buçuk dakika içinde tamamlanır.

Olduğunu fark edersiniz `rxDataStep` giriş bileşik XDF dosyasında her .xdfd dosya için bir CSV kullanıma yazdım. İşlem bağlamı ayarlandığında CSV bileşik XDF HDFS'ye yazmak için varsayılan davranışı budur `RxSpark`.

### <a name="in-a-local-context"></a>Yerel bir bağlamda

Alternatif olarak, işlem bağlamınızın geçiş yapabilir geri `local` işiniz bittiğinde, çözümlemeler gerçekleştirmek ve iki bağımsız değişken içinde yararlanmak `RxTextData` , size biraz daha fazla denetim CSV dosyaları için HDFS yazarken: `createFileSet` ve `rowsPerOutFile`. Zaman `createFileSet` ayarlanır `TRUE`, bir klasör ve CSV dosyaları belirttiğiniz dizine yazılır. Zaman `createFileSet` ayarlanır `FALSE` tek bir CSV dosyasına yazılır. İkinci bağımsız değişkeni `rowsPerOutFile`, kaç satır yazmak için her bir CSV dosyası belirtmek için bir tamsayıya ayarlayın `createFileSet` olduğu `TRUE`.

RStudio içinde aşağıdaki kodu girin:

```R
rxSetComputeContext("local")
airDataCsvRowsDir <- file.path(bigDataDirRoot,"AirDataCSVRows2012")
airDataCsvRowsDS <- RxTextData(airDataCsvRowsDir, fileSystem=hdfsFS, createFileSet=TRUE, rowsPerOutFile=1000000)
rxDataStep(inData=airDataXdf, outFile=airDataCsvRowsDS)
```

Bu adım, yaklaşık 10 dakika içinde tamamlanır.

Kullanırken bir `RxSpark` işlem bağlamında, `createFileSet` varsayılan olarak `TRUE` ve `rowsPerOutFile` hiçbir etkisi olmaz. Bu nedenle tek bir CSV oluşturun veya gerçekleştirmelisiniz dosya başına satır sayısı özelleştirmek istiyorsanız `rxDataStep` içinde bir `local` işlem bağlamını (veri HDFS'deki yine de olabilir).

## <a name="final-steps"></a>Son adımlar

1. Verilerini temizleyin. RStudio içinde aşağıdaki kodu girin:

    ```R
    rxHadoopRemoveDir(airDataDir)
    rxHadoopRemoveDir(airDataXdfDir)
    rxHadoopRemoveDir(airDataCsvDir)
    rxHadoopRemoveDir(airDataCsvRowsDir)
    rxHadoopRemoveDir(bigDataDirRoot)
    ```

1. Uzak Spark uygulamayı durdurun. RStudio içinde aşağıdaki kodu girin:

    ```R
    rxStopEngine(mySparkCluster)
    ```

1. R oturumu kapatın. RStudio içinde aşağıdaki kodu girin:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ML Hizmetleri Azure HDInsight kümesi üzerinde çalışan Apache Spark, R işlevleri kullanmayı öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [İçin işlem bağlamı seçenekleri ML hizmetleri üzerinde HDInsight](r-server-compute-contexts.md)
* [Hadoop üzerinde Spark için R işlevleri](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler-hadoop-functions)
