---
title: Hdınsight - Azure ML Hizmetleri kümede yönetme | Microsoft Docs
description: Azure Hdınsight ML Hizmetleri kümede yönetmeyi öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: bb3af3b1614c8afc98d2dcf12ecb53fb80b6037a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049753"
---
# <a name="manage-ml-services-cluster-on-azure-hdinsight"></a>Azure Hdınsight kümesinde ML Hizmetleri yönetme

Bu makalede, var olan bir ML hizmetleri kümesine uzaktan bir ML hizmetleri kümesine bağlanma birden çok eşzamanlı kullanıcı ekleme, değiştirme işlem bağlamı, vb. gibi görevleri gerçekleştirmek için Azure hdınsight'ta yönetmeyi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight ML Hizmetleri kümede**: yönergeler için bkz: [hdınsight'ta ML hizmetleri kullanmaya başlama](r-server-get-started.md).

* **Güvenli Kabuk (SSH) istemcisi**: HDInsight kümesine uzaktan bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için bir SSH istemcisi kullanılır. Daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma.](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="enable-multiple-concurrent-users"></a>Birden çok eş zamanlı kullanıcı etkinleştirme

Daha fazla kullanıcı için Rstudio'dan topluluğu sürümü çalıştığı kenar düğümüne ekleyerek, Hdınsight kümesinde ML Hizmetleri için birden çok eşzamanlı kullanıcı etkinleştirebilirsiniz. Bir HDInsight kümesi oluşturduğunuzda bir HTTP kullanıcısı ve bir SSH kullanıcısı olmak üzere iki kullanıcı sağlamanız gerekir:

![Eşzamanlı kullanıcı 1](./media/r-server-hdinsight-manage/concurrent-users-1.png)

- **Küme oturum açma kullanıcı adı**: Oluşturduğunuz HDInsight kümelerini korumak için kullanılan HDInsight ağ geçidinden kimlik doğrulaması yapmak için kullanılan HTTP kullanıcısı. Bu HTTP kullanıcısı Ambari UI, YARN UI ve diğer UI bileşenlerine erişmek için kullanılır.
- **Secure Shell (SSH) kullanıcı adı**: Kümeye Secure Shell üzerinden erişmek için kullanılan SSH kullanıcısı. Bu kullanıcı Linux sisteminde tüm baş düğümler, çalışan düğümleri ve kenar düğümler için kullanılan kullanıcıdır. Bu sayede uzak kümedeki düğümlere erişmek için Secure Shell kullanabilirsiniz.

Hdınsight üzerinde ML Hizmetleri kümedeki kullanılan R Studio Server Community sürümü yalnızca Linux kullanıcı adı ve parola bir oturum açma mekanizması olarak kabul eder. Belirteç iletmeyi desteklemez. Bu nedenle, ilk kez bir ML Hizmetleri kümede R Studio erişmeyi denediğinde, iki kez oturum açmanız gerekir.

- İlk Hdınsight ağ geçidi üzerinden HTTP kullanıcı kimlik bilgilerini kullanarak oturum açın. 

- Ardından Rstudio'dan için oturum açmak için SSH kullanıcı kimlik bilgilerini kullanın.
  
Şu anda bir HDInsight kümesi sağlanırken yalnızca bir SSH kullanıcı hesabı oluşturulabilir. Bu nedenle ML Hizmetleri Hdınsight kümesinde birden çok erişmelerini sağlamak için ek kullanıcılar Linux sistemde oluşturmanız gerekir.

Rstudio'dan kümenin kenar düğümüne üzerinde çalıştığı için burada birkaç adım vardır:

1. Kenar düğümüne oturum açmak için varolan bir SSH kullanıcı kullanın
2. Kenar düğümüne daha fazla Linux kullanıcısı ekleme
3. RStudio Topluluk sürümünü oluşturulan kullanıcıyla kullanma

### <a name="step-1-use-the-created-ssh-user-to-sign-in-to-the-edge-node"></a>1. adım: kenar düğümüne oturum açmak için oluşturulan SSH kullanıcı kullanın.

Bölümündeki yönergeleri izleyin [SSH kullanarak Hdınsight (Hadoop) Bağlan](../hdinsight-hadoop-linux-use-ssh-unix.md) kenar düğümüne erişmek için. Hdınsight kümesinde ML Hizmetleri için edge düğüm adresi `CLUSTERNAME-ed-ssh.azurehdinsight.net`.

### <a name="step-2-add-more-linux-users-in-edge-node"></a>2. Adım: Kenar düğümüne daha fazla Linux kullanıcısı ekleme

Kenar düğümüne bir kullanıcı eklemek için şu komutları çalıştırın:

    # Add a user 
    sudo useradd <yournewusername> -m

    # Set password for the new user
    sudo passwd <yournewusername>

Aşağıdaki ekran görüntüsünde çıkışları gösterir.

![Eşzamanlı kullanıcı 3](./media/r-server-hdinsight-manage/concurrent-users-2.png)

"Geçerli Kerberos parolası:" sorulduğunda **Enter** tuşuna basarak atlayın. `useradd` komutundaki `-m` seçeneği, sistemin RStudio Topluluk sürümü için gerekli olan kullanıcı ana klasörünü oluşturacağını belirtir.

### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>3. Adım: RStudio Topluluk sürümünü oluşturulan kullanıcıyla kullanma

Erişim Rstudio'dan gelen https://CLUSTERNAME.azurehdinsight.net/rstudio/. Kümeyi oluşturduktan sonra ilk kez oturum açmayı, oluşturduğunuz SSH kullanıcı kimlik bilgileri tarafından izlenen küme yönetici kimlik bilgilerini girin. Yalnızca bu ilk oturum açma değilse, oluşturduğunuz SSH kullanıcı için kimlik bilgilerini girin.

Özgün kimlik bilgilerini kullanarak da oturum açabilirsiniz (varsayılan olarak, olmasından *sshuser*) aynı anda başka bir tarayıcı penceresi öğesinden.

Ayrıca yeni eklenen kullanıcıların Linux sisteminde kök ayrıcalıklarına sahip olmadığına ancak uzak HDFS ve WASB depolama alanındaki tüm dosyalara aynı düzeyde erişime sahip olduğuna dikkat edin.

## <a name="connect-remotely-to-microsoft-ml-services"></a>Uzaktan Microsoft ML hizmetlerine bağlanmak

Masaüstünüzde çalıştıran uzak bir örneğini ML istemci erişimi Hdınsight Hadoop Spark işlem bağlamına ayarlayabilirsiniz. Bunu yapmak için (hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches ve sshProfileScript) seçenekleri belirtin zaman RxSpark tanımlama izin ver işlem bağlamı masaüstünüzde: Örneğin:

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

Daha fazla bilgi için "Kullanarak Microsoft Machine Learning sunucu olarak bir Hadoop istemcisi" bölümüne bakın [RevoScaleR bir Spark işlem bağlamında kullanma](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios)

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

2. Ardından, bazı veri bilgisi oluşturun ve iki veri kaynağını tanımlayın.

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

3. Lojistik regresyon yerel işlem bağlamı kullanarak verileri üzerinde çalıştırın.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Aşağıdaki kod parçacığını benzer satırları ile biten bir çıktı görmeniz gerekir:

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

4. Spark bağlamı kullanarak aynı Lojistik regresyon çalıştırın. Spark bağlamı, işlemi HDInsight kümesindeki tüm çalışan düğümlerine dağıtır.

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


   > [!NOTE]
   > Ayrıca, MapReduce kullanarak hesaplamayı küme düğümleri arasında dağıtabilirsiniz. İşlem bağlamı hakkında daha fazla bilgi için bkz: [ML Hizmetleri için içerik seçeneklerini küme Hdınsight üzerinde işlem](r-server-compute-contexts.md).

## <a name="distribute-r-code-to-multiple-nodes"></a>R kodunu birden fazla düğüme dağıtma

Hdınsight üzerinde ML Hizmetleri ile var olan R kodu alabilir ve çalıştırın kümedeki birden çok düğüm arasında kullanarak `rxExec`. Bu işlev bir parametre tarama veya benzetme işlemi sırasında yararlıdır. `rxExec` kullanımını gösteren bir kod örneği aşağıda verilmiştir:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Hala Spark veya MapReduce bağlamını kullanıyorsanız bu komut, üzerinde `(Sys.info()["nodename"])` kodunun çalıştığı çalışan düğümleri için nodename değerini döndürür. Örneğin, dört düğümlü bir küme üzerinde aşağıdaki kod parçacığını benzer bir çıktı almayı beklediğiniz:

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

## <a name="access-data-in-hive-and-parquet"></a>Hive ve Parquet verilerine erişim

Hdınsight ML Hizmetleri Spark işlem bağlamında ScaleR işlevler tarafından doğrudan Hive ve Parquet veri erişimi kullanmak için izin verir. Bu özellikler, ScaleR tarafından analiz edilmek üzere bir Spart DataFrame’e doğrudan veri yüklemek için Spark SQL kullanarak çalışan RxHiveData ve RxParquetData adlı yeni ScaleR veri kaynağı işlevleriyle kullanılabilir.

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


Bu yeni işlevler kullanımı ile ilgili ek bilgi için ML Hizmetleri aracılığıyla çevrimiçi yardıma bakın `?RxHivedata` ve `?RxParquetData` komutları.  

## <a name="install-additional-r-packages-on-the-cluster"></a>Kümede ek R paketleri yüklemek

### <a name="to-install-r-packages-on-the-edge-node"></a>Kenar düğümüne R paketlerini yüklemek için

Kenar düğümüne ek R paketleri yüklemek istiyorsanız, kullanabileceğiniz `install.packages()` doğrudan içinden R konsol bağlandığında SSH aracılığıyla kenar düğümüne. 

### <a name="to-install-r-packages-on-the-worker-node"></a>Çalışan düğümünde R paketlerini yüklemek için

R paketleri kümesinin çalışan düğümlerine yüklemek için bir betik eylemi kullanmanız gerekir. Betik Eylemleri, HDInsight kümesinde yapılandırma değişiklikleri yapmak veya ek R paketleri gibi ek yazılımlar yüklemek için kullanılan Bash betikleridir. 

> [!IMPORTANT]
> Ek R paketleri yüklemek için Betik Eylemleri yalnızca küme oluşturulduktan sonra kullanılabilir. Komut dosyası ML tamamen yapılandırılmakta Services'de alacağından küme oluşturma sırasında bu yordamı kullanmayın.
>
>

1. Bölümündeki adımları izleyin [betik eylemi kullanarak kümelerini özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md).

3. İçin **betik eylemi Gönder**, aşağıdaki bilgileri sağlayın:

   * İçin **komut dosyası türü**seçin **özel**.

   * İçin **adı**, betik eylemi için bir ad sağlayın.

    * İçin **betik URI Bash**, girin `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`. Bu ek R paketleri çalışan düğümünde yükler komut dosyasıdır

   * Yalnızca için onay kutusunu işaretleyin **çalışan**.

   * **Parametreler**: Yüklenecek R paketleri. Örneğin, `bitops stringr arules`

   * İçin bu onay kutusunu işaretleyin **bu betik eylemini Sürdür**.  

   > [!NOTE]
   > 1. Varsayılan olarak, Microsoft MRAN depo yüklendi ML sunucu sürümü ile tutarlı bir anlık tüm R paketleri yüklenir. Paketlerin daha yeni sürümlerini yüklemek istiyorsanız uyumsuzluk riskiyle karşı karşıya kalabilirsiniz. Ancak bu tür yüklemeleri paket listesinin ilk öğesi olarak `useCRAN` belirleyerek mümkün kılabilirsiniz, örneğin `useCRAN bitops, stringr, arules`.  
   > 2. Bazı R paketleri için ek Linux sistem kitaplıkları gerekir. Kolaylık olması için Hdınsight ML Hizmetleri ilk 100 en popüler R paketi tarafından gerekli bağımlılıkları ile önceden yüklenmiş olarak gelir. Ancak, yüklediğiniz R paketleri bunların dışında kitaplıklar gerektirirse, burada kullanılan temel betiği indirmeniz ve adımları ekleyerek sistem kitaplıklarını yüklemeniz gerekir. Ardından, değiştirilmiş betiği Azure depolama hizmetindeki ortak bir blob kapsayıcıya yüklemeniz ve değiştirilmiş betiği kullanarak paketleri yüklemeniz gerekir.
   >    Betik Eylemleri geliştirme hakkında bilgi için bkz. [Betik Eylemi geliştirme](../hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Betik eylemi ekleme](./media/r-server-hdinsight-manage/submitscriptaction.png)

4. Betiği çalıştırmak için **Oluştur**’u seçin. Betik tamamlandıktan sonra R paketleri tüm çalışan düğümlerinde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [ML Hizmetleri Hdınsight kümesinde faaliyete](r-server-operationalize.md)
* [Hdınsight üzerinde ML Service kümesi için içerik seçeneklerini işlem](r-server-compute-contexts.md)
* [Hdınsight kümesinde ML Hizmetleri için Azure depolama seçenekleri](r-server-storage.md)
