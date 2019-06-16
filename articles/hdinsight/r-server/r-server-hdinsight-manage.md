---
title: HDInsight - Azure ML Hizmetleri kümede yönetme
description: Bir Azure HDInsight ML Hizmetleri kümesinde yönetmeyi öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 607f85c10183366e88d597d84090f49fc30aff48
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64687976"
---
# <a name="manage-ml-services-cluster-on-azure-hdinsight"></a>Azure HDInsight kümesinde ML Hizmetleri yönetme

Bu makalede, var olan ML Hizmetleri bir işlem bağlamı, vb. bir ML Hizmetleri kümeye uzaktan bağlanma birden çok eş zamanlı kullanıcılar değiştirme, ekleme gibi görevleri gerçekleştirmek için Azure HDInsight kümesinde yönetme konusunda bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* **Bir HDInsight ML Hizmetleri kümesinde**: Yönergeler için [ML Hizmetleri HDInsight kullanmaya başlama](r-server-get-started.md).

* **Güvenli Kabuk (SSH) istemcisi**: Bir SSH istemcisi uzaktan HDInsight kümesine bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için kullanılır. Daha fazla bilgi için [HDInsight ile SSH kullanma.](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="enable-multiple-concurrent-users"></a>Birden çok eş zamanlı kullanıcı etkinleştirme

RStudio topluluk sürümünün çalıştığı kenar düğümüne daha fazla kullanıcının ekleyerek, HDInsight kümesinde ML Hizmetleri için birden fazla eşzamanlı kullanıcıyı etkinleştirebilirsiniz. Bir HDInsight kümesi oluşturduğunuzda bir HTTP kullanıcısı ve bir SSH kullanıcısı olmak üzere iki kullanıcı sağlamanız gerekir:

![Eşzamanlı kullanıcı 1](./media/r-server-hdinsight-manage/concurrent-users-1.png)

- **Küme oturum açma kullanıcı adı**: Oluşturduğunuz HDInsight kümelerini korumak için kullanılan HDInsight ağ geçidinden kimlik doğrulaması yapmak için kullanılan HTTP kullanıcısı. Bu HTTP kullanıcısı, Apache Ambari UI, Apache Hadoop YARN UI yanı sıra, diğer UI bileşenlerine erişmek için kullanılır.
- **Secure Shell (SSH) kullanıcı adı**: Kümeye Secure Shell üzerinden erişmek için kullanılan SSH kullanıcısı. Bu kullanıcı Linux sisteminde tüm baş düğümler, çalışan düğümleri ve kenar düğümler için kullanılan kullanıcıdır. Bu sayede uzak kümedeki düğümlere erişmek için Secure Shell kullanabilirsiniz.

HDInsight ML Hizmetleri kümede kullanılan R Studio Server topluluk sürümü yalnızca Linux kullanıcı adı ve parola bir oturum açma mekanizması olarak kabul eder. Belirteç iletmeyi desteklemez. Bu nedenle, R Studio ML Hizmetleri kümesinde ilk kez erişmeye çalıştığınızda, iki kez oturum açmanız gerekir.

- İlk HDInsight ağ geçidi üzerinden HTTP kullanıcı kimlik bilgilerini kullanarak oturum açın. 

- Ardından SSH kullanıcısı kimlik bilgilerini kullanarak RStudio oturum açmak için kullanın.
  
Şu anda bir HDInsight kümesi sağlanırken yalnızca bir SSH kullanıcı hesabı oluşturulabilir. Bu nedenle birden fazla kullanıcının HDInsight kümesinde ML Hizmetleri erişim sağlamak için Linux sisteminde ek kullanıcılar oluşturmanız gerekir.

Kümenin kenar düğümünde RStudio çalıştığı için burada birkaç adım vardır:

1. Kenar düğümde oturum açmak için var olan SSH kullanıcısını kullanma
2. Kenar düğümüne daha fazla Linux kullanıcısı ekleme
3. RStudio Topluluk sürümünü oluşturulan kullanıcıyla kullanma

### <a name="step-1-use-the-created-ssh-user-to-sign-in-to-the-edge-node"></a>1\. adım: Kenar düğümde oturum açmak için oluşturulan SSH kullanıcısını kullanma

Konumundaki yönergeleri [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md) kenar düğümüne erişin. HDInsight kümesinde ML Hizmetleri için uç düğüm adresi olan `CLUSTERNAME-ed-ssh.azurehdinsight.net`.

### <a name="step-2-add-more-linux-users-in-edge-node"></a>2\. adım: Kenar düğümüne daha fazla Linux kullanıcısı ekleme

Kenar düğümüne bir kullanıcı eklemek için şu komutları çalıştırın:

    # Add a user 
    sudo useradd <yournewusername> -m

    # Set password for the new user
    sudo passwd <yournewusername>

Aşağıdaki ekran görüntüsünde çıktıları gösterir.

![Eşzamanlı kullanıcı 3](./media/r-server-hdinsight-manage/concurrent-users-2.png)

İstendiğinde "geçerli Kerberos parolası:", tuşuna basarak **Enter** atlayın. `useradd` komutundaki `-m` seçeneği, sistemin RStudio Topluluk sürümü için gerekli olan kullanıcı ana klasörünü oluşturacağını belirtir.

### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>3\. adım: RStudio Topluluk sürümünü oluşturulan kullanıcıyla kullanma

RStudio gelen erişim https://CLUSTERNAME.azurehdinsight.net/rstudio/. Kümeyi oluşturduktan sonra ilk kez oturum açmayı oluşturduğunuz SSH kullanıcısı kimlik bilgilerini tarafından izlenen Küme Yöneticisi kimlik bilgilerini girin. Yalnızca bu ilk oturum açma bilgilerinizi değilse, oluşturduğunuz SSH kullanıcısının kimlik bilgilerini girin.

Özgün kimlik bilgilerini kullanarak da oturum açabilirsiniz (varsayılan olarak, olduğu *sshuser*) eşzamanlı olarak başka bir tarayıcı penceresinden.

Ayrıca yeni eklenen kullanıcıların Linux sisteminde kök ayrıcalıklarına sahip olmadığına ancak uzak HDFS ve WASB depolama alanındaki tüm dosyalara aynı düzeyde erişime sahip olduğuna dikkat edin.

## <a name="connect-remotely-to-microsoft-ml-services"></a>Microsoft ML Hizmetleri uzaktan bağlanma

Sizin masaüstünüzde çalışan uzak bir örneğini ML istemci HDInsight Spark işlem bağlamına erişim ayarlayabilirsiniz. Bunu yapmak için seçenekleri (hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches ve sshProfileScript) belirtin, dizüstü tanımlama izin ver işlem bağlamını masaüstünüzde: Örneğin:

    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- '<clustername>-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- '<sshuser>'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )

Daha fazla bilgi için bkz: "Kullanarak Microsoft Machine Learning sunucusu olarak bir Apache Hadoop istemcisi" bölümünde [RevoScaleR bir Apache Spark işlem bağlamında kullanma](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios)

## <a name="use-a-compute-context"></a>İşlem bağlamı kullanma

Bir işlem bağlamı, hesaplamanın kenar düğümünde yerel olarak yapılmasını veya HDInsight kümesindeki düğümlere dağıtılmasını denetlemenize olanak tanır.

1. RStudio Server veya R konsolundan (bir SSH oturumunda), varsayılan HDInsight depolama alanına örnek verileri yüklemek için aşağıdaki kodu kullanın:

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
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

2. Ardından, bazı veri bilgileri oluşturun ve iki veri kaynağı tanımlayabiliriz.

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

3. Yerel işlem bağlamını kullanarak veriler üzerinde Lojistik regresyon çalıştırın.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Aşağıdaki kod parçacığına benzer satırlarla sona eren bir çıktı görmeniz gerekir:

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

4. Spark bağlamını kullanarak aynı Lojistik regresyonu çalıştırın. Spark bağlamı, işlemi HDInsight kümesindeki tüm çalışan düğümlerine dağıtır.

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


## <a name="distribute-r-code-to-multiple-nodes"></a>R kodunu birden fazla düğüme dağıtma

HDInsight üzerinde ML Hizmetleri ile mevcut R kodunu alabilir ve kümedeki birden fazla düğümde kullanarak çalıştırabilirsiniz `rxExec`. Bu işlev bir parametre tarama veya benzetme işlemi sırasında yararlıdır. `rxExec` kullanımını gösteren bir kod örneği aşağıda verilmiştir:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Yine de Spark bağlamını kullanıyorsanız bu komut çalışan düğümleri için nodename değerini döndürür kodu `(Sys.info()["nodename"])` çalıştırın. Örneğin, dört düğümlü bir kümede, aşağıdaki kod parçacığına benzer bir çıktı beklenir:

    $rxElem1
        nodename
    "wn3-mymlser"

    $rxElem2
        nodename
    "wn0-mymlser"

    $rxElem3
        nodename
    "wn3-mymlser"

    $rxElem4
        nodename
    "wn3-mymlser"

## <a name="access-data-in-apache-hive-and-parquet"></a>Apache Hive ve Parquet verilerine erişim

HDInsight ML Hizmetleri Spark işlem bağlamındaki ScaleR işlevleri tarafından kullanım için Hive ve Parquet içindeki verilere doğrudan erişime izin verir. Bu özellikler, ScaleR tarafından analiz edilmek üzere bir Spart DataFrame’e doğrudan veri yüklemek için Spark SQL kullanarak çalışan RxHiveData ve RxParquetData adlı yeni ScaleR veri kaynağı işlevleriyle kullanılabilir.

Yeni işlevlerin kullanımına ilişkin bazı örnek kodlar aşağıdaki kod ile verilmiştir:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Bu yeni işlevlerin kullanımına ilişkin ek bilgi için ML Hizmetleri aracılığıyla çevrimiçi yardıma bakın `?RxHivedata` ve `?RxParquetData` komutları.  

## <a name="install-additional-r-packages-on-the-cluster"></a>Kümede ek R paketleri yüklemek

### <a name="to-install-r-packages-on-the-edge-node"></a>Kenar düğümüne R paketleri yüklemek için

Kenar düğümüne ek R paketleri yüklemek isterseniz, kullanabileceğiniz `install.packages()` doğrudan içinden R konsolunu bağlandığında kenar düğümüne SSH üzerinden. 

### <a name="to-install-r-packages-on-the-worker-node"></a>Çalışan düğümü üzerinde R paketleri yüklemek için

Kümenin çalışan düğümlerine R paketleri yüklemek için betik eylemi kullanmanız gerekir. Betik Eylemleri, HDInsight kümesinde yapılandırma değişiklikleri yapmak veya ek R paketleri gibi ek yazılımlar yüklemek için kullanılan Bash betikleridir. 

> [!IMPORTANT]  
> Ek R paketleri yüklemek için Betik Eylemleri yalnızca küme oluşturulduktan sonra kullanılabilir. Betik ML Hizmetleri tamamen yapılandırılmış olmasına olduğundan, bu yordamı küme oluşturma sırasında kullanmayın.

1. Bölümündeki adımları [betik eylemi kullanarak kümeleri özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md).

3. İçin **betik eylemini Gönder**, aşağıdaki bilgileri sağlayın:

   * İçin **betik türü**seçin **özel**.

   * İçin **adı**, betik eylemi için bir ad sağlayın.

     * İçin **Bash betiği URI'si**, girin `https://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`. Çalışan düğümüne ek R paketleri yükleyen bir betik budur.

   * Yalnızca için onay kutusunu işaretleyin **çalışan**.

   * **Parametreleri**: Yüklenecek R paketleri. Örneğin, `bitops stringr arules`

   * Onay kutusunu işaretleyin **bu betik eylemi kalıcı**.  

   > [!NOTE]
   > 1. Varsayılan olarak, tüm R paketleri Microsoft MRAN deposunun yüklü olan ML Server sürümüyle tutarlı bir anlık görüntüsünden yüklenir. Paketlerin daha yeni sürümlerini yüklemek istiyorsanız uyumsuzluk riskiyle karşı karşıya kalabilirsiniz. Ancak bu tür yüklemeleri paket listesinin ilk öğesi olarak `useCRAN` belirleyerek mümkün kılabilirsiniz, örneğin `useCRAN bitops, stringr, arules`.  
   > 2. Bazı R paketleri için ek Linux sistem kitaplıkları gerekir. Kolaylık olması için HDInsight ML Hizmetleri en çok 100 en popüler R paketi için gerekli bağımlılıkları olan önceden yüklenmiş olarak sunulur. Ancak, yüklediğiniz R paketleri bunların dışında kitaplıklar gerektirirse, burada kullanılan temel betiği indirmeniz ve adımları ekleyerek sistem kitaplıklarını yüklemeniz gerekir. Ardından, değiştirilmiş betiği Azure depolama hizmetindeki ortak bir blob kapsayıcıya yüklemeniz ve değiştirilmiş betiği kullanarak paketleri yüklemeniz gerekir.
   >    Betik Eylemleri geliştirme hakkında bilgi için bkz. [Betik Eylemi geliştirme](../hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Betik eylemi ekleme](./media/r-server-hdinsight-manage/submitscriptaction.png)

4. Betiği çalıştırmak için **Oluştur**’u seçin. Betik tamamlandıktan sonra R paketleri tüm çalışan düğümlerinde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde ML Services kümesini kullanıma hazır hale getirme](r-server-operationalize.md)
* [HDInsight kümesinde ML Service için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde ML Services kümesi için Azure Depolama seçenekleri](r-server-storage.md)
