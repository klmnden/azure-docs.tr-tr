---
title: 'Hızlı Başlangıç: Bir Azure HDInsight R konsolunu kullanmanın ML Hizmetleri kümesinde bir R betiği yürütün.'
description: Bu hızlı başlangıçta, Azure HDInsight R konsolunu kullanmanın bir ML Hizmetleri kümesinde bir R betiği yürütün.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/19/2019
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: 682ee4f44dcdd2619668645fa7a8aa22cb645273
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450929"
---
# <a name="quickstart-execute-an-r-script-on-an-ml-services-cluster-in-azure-hdinsight-using-r-console"></a>Hızlı Başlangıç: Bir Azure HDInsight R konsolunu kullanmanın ML Hizmetleri kümesinde bir R betiği yürütün.

ML Hizmetleri Azure HDInsight üzerinde R betiklerinin dağıtılmış hesaplamaları çalıştırmak için Apache Spark ve Apache Hadoop MapReduce kullanmayı sağlar. ML Hizmetleri işlem bağlamını ayarlayarak çağrıları nasıl yürütülür denetler. Bir küme kenar düğümüne kümeye bağlanın ve, R betikleri çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile çalıştırmanın RevoScaleR uç düğümü sunucusunun çekirdek üzerinde paralel dağıtılmış işlevleri seçeneğiniz vardır. Ayrıca bunları tüm küme düğümlerine RevoScaleR'ın Hadoop Map Reduce kullanarak çalıştırabilirsiniz veya Apache Spark işlem bağlamlarının.

Bu hızlı başlangıçta, dağıtılmış R hesaplamaları için Spark'ı kullanarak gösteren R konsolu ile bir R betiğini çalıştırmayı öğrenin. Hesaplamaları bir kenar düğümünde yerel olarak gerçekleştirmek için bir işlem bağlamı tanımlar ve HDInsight kümesindeki düğümlere dağıtılmasını yeniden.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde ML Hizmetleri kümesi. Bkz: [Apache Hadoop kümeleri oluşturma Azure portalını kullanarak](../hdinsight-hadoop-create-linux-clusters-portal.md) seçip **ML Hizmetleri** için **küme türü**.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="connect-to-r-console"></a>R konsoluna Bağlan

1. SSH kullanarak ML Hizmetleri HDInsight kümesinin kenar düğümüne bağlanın. Değiştirerek aşağıdaki komutu düzenleyin `CLUSTERNAME` , kümenizin adını ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ed-ssh.azurehdinsight.net
    ```

1. R konsolunu başlatmak için SSH oturumunda aşağıdaki komutu kullanın:

    ```
    R
    ```

    Diğer bilgilere ek olarak, ML Server sürümünü içeren bir çıktı görmeniz gerekir.


## <a name="use-a-compute-context"></a>İşlem bağlamı kullanma

1. `>` isteminde R kodunu girebilirsiniz. HDInsight için varsayılan depolama alanına örnek verileri yüklemek için aşağıdaki kodu kullanın:

    ```R
    # Set the HDFS (WASB) location of example data
     bigDataDirRoot <- "/example/data"
    
     # create a local folder for storing data temporarily
     source <- "/tmp/AirOnTimeCSV2012"
     dir.create(source)
    
     # Download data to the tmp folder
     remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
     download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
     download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
     download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
     download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
     download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
     download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
     download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
     download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
     download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
     download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
     download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
     download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))
    
     # Set directory in bigDataDirRoot to load the data into
     inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")
    
     # Make the directory
     rxHadoopMakeDir(inputDir)
    
     # Copy the data from source to input
     rxHadoopCopyFromLocal(source, bigDataDirRoot)
    ```

    Bu adımı tamamlamak için yaklaşık 10 dakika sürebilir.

1. Bazı veri bilgileri oluşturup iki veri kaynağı tanımlayabiliriz. R konsolunda aşağıdaki kodu girin:

    ```R
    # Define the HDFS (WASB) file system
     hdfsFS <- RxHdfsFileSystem()
    
     # Create info list for the airline data
     airlineColInfo <- list(
          DAY_OF_WEEK = list(type = "factor"),
          ORIGIN = list(type = "factor"),
          DEST = list(type = "factor"),
          DEP_TIME = list(type = "integer"),
          ARR_DEL15 = list(type = "logical"))
    
     # get all the column names
     varNames <- names(airlineColInfo)
    
     # Define the text data source in hdfs
     airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)
    
     # Define the text data source in local system
     airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)
    
     # formula to use
     formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"
    ```

1. Kullanarak veriler üzerinde Lojistik regresyon çalıştırın **yerel** işlem bağlamında. R konsolunda aşağıdaki kodu girin:

    ```R
    # Set a local compute context
     rxSetComputeContext("local")
    
     # Run a logistic regression
     system.time(
        modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
     )
    
     # Display a summary
     summary(modelLocal)
    ```

    Hesaplamaları yaklaşık 7 dakika içinde tamamlanır. Aşağıdaki kod parçacığına benzer satırlarla sona eren bir çıktı görmeniz gerekir:

    ```output
    Data: airOnTimeDataLocal (RxTextData Data Source)
     File name: /tmp/AirOnTimeCSV2012
     Dependent variable(s): ARR_DEL15
     Total independent variables: 634 (Including number dropped: 3)
     Number of valid observations: 6005381
     Number of missing observations: 91381
     -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)
    
     Coefficients:
                      Estimate Std. Error z value Pr(>|z|)
      (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
      ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
      ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
      ......
      DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
      DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
      DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
      DEST=BPT         Dropped    Dropped Dropped  Dropped
    
      ---
    
      Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
      Condition number of final variance-covariance matrix: 11904202
      Number of iterations: 7
    ```

1. Kullanarak aynı Lojistik regresyon çalıştırın **Spark** bağlamı. Spark bağlamı, işlemi HDInsight kümesindeki tüm çalışan düğümlerine dağıtır. R konsolunda aşağıdaki kodu girin:

    ```R
    # Define the Spark compute context
     mySparkCluster <- RxSpark()
    
     # Set the compute context
     rxSetComputeContext(mySparkCluster)
    
     # Run a logistic regression
     system.time(  
        modelSpark <- rxLogit(formula, data = airOnTimeData)
     )
    
     # Display a summary
     summary(modelSpark)
    ```

    Hesaplamaları, 5 dakika içinde tamamlanır.

1. R konsolundan çıkmak için aşağıdaki komutu kullanın:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dağıtılmış R hesaplamaları için Spark'ı kullanarak gösterilen R konsolu ile bir R betiğini çalıştırmayı öğrendiniz.  Görüntülenip görüntülenmeyeceğini ve nasıl yürütme kenar düğümüne veya HDInsight kümesi Çekirdekte paralelleştirildi belirtmek için kullanılabilir seçenekleri öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[İçin işlem bağlamı seçenekleri ML hizmetleri üzerinde HDInsight](./r-server-compute-contexts.md)