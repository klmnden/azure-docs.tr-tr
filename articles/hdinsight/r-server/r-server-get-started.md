---
title: HDInsight'ta ML Services ile çalışmaya başlama - Azure
description: HDInsight kümesi üzerinde ML Services içeren bir Apache Spark oluşturma ve küme üzerinde bir R betiği gönderme hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: 465b53e1c5f56c5c05c860ebd69a825141f7e703
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706068"
---
# <a name="get-started-with-ml-services-on-azure-hdinsight"></a>Azure HDInsight'ta ML Services ile çalışmaya başlama

Azure HDInsight, ML Services kümesi oluşturmanızı sağlar. R betikleri kullanmak bu seçenek sayesinde [Apache Spark](https://spark.apache.org/) ve [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) dağıtılmış hesaplamaları çalıştırmak için. Bu makalede, HDInsight kümesi üzerinde bir ML Services kümesi oluşturma ve ardından dağıtılmış R hesaplamaları için Spark kullanmayı gösteren bir R betiği çalıştırma hakkında bilgi alacaksınız.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliğine**: Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Daha fazla bilgi için bkz. [Microsoft Azure ücretsiz denemesini alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Güvenli Kabuk (SSH) istemcisi**: Bir SSH istemcisi uzaktan HDInsight kümesine bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a>Azure portalını kullanarak küme oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Gidin **+ kaynak Oluştur** > **Analytics** > **HDInsight**.

3. **Temel Bilgiler** bölümünden aşağıdaki bilgileri girin:

    * **Küme adı**: HDInsight kümesinin adı.
    * **Abonelik**: Kullanılacak aboneliği seçin.
    * **Küme oturum açma kullanıcı adı** ve **küme oturum açma parolası**: HTTPS üzerinden kümeye erişirken oturum açın. Apache Ambari Web kullanıcı Arabirimi veya REST API gibi hizmetlere erişmek için bu kimlik bilgilerini kullanın.
    * **Güvenli Kabuk (SSH) kullanıcı adı**: SSH üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Varsayılan olarak parola, küme oturum açma parolası ile aynıdır.
    * **Kaynak grubu**: İçinde kümenin oluşturulduğu kaynak grubu.
    * **Konum**: İçinde kümenin oluşturulacağı Azure bölgesi.

        ![Küme temel ayrıntıları](./media/r-server-get-started/clustername.png)

4. **Küme türü**’nü seçin, ardından **Küme yapılandırması** bölümünde aşağıdaki değerleri ayarlayın:

    * **Küme türü**: ML Services

    * **İşletim sistemi**: Linux

    * **Sürüm**: ML Server 9.3 (HDI 3.6). ML Server 9.3 sürüm notlarına [docs.microsoft.com](https://docs.microsoft.com/machine-learning-server/whats-new-in-machine-learning-server) adresinden ulaşabilirsiniz.

    * **ML Server için R Studio topluluk sürümü**: Tarayıcı tabanlı bu IDE, kenar düğümüne varsayılan olarak yüklenir. Yüklememeyi tercih ederseniz, onay kutusunun işaretini kaldırın. Yüklemeyi seçerseniz, RStudio Server oturum açma sayfasına erişim URL'si, oluşturulan kümenizin portal uygulaması dikey penceresinde bulunur.

        ![Küme temel ayrıntıları](./media/r-server-get-started/clustertypeconfig.png)

4. Küme türünü seçtikten sonra __Seç__ düğmesini kullanarak küme türünü ayarlayın. Ardından, __İleri__ düğmesini kullanarak temel yapılandırmayı tamamlayın.

5. **Depolama** bölümünden bir depolama hesabı seçin veya oluşturun. Bu belgedeki adımlar için bu bölümdeki diğer alanları varsayılan değerlerinde bırakın. __İleri__ düğmesini kullanarak depolama yapılandırmasını kaydedin.

    ![HDInsight depolama hesabı ayarlarını belirleme](./media/r-server-get-started/cluster-storage.png)

6. **Özet** bölümünden kümenin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirmek için __Düzenle__ bağlantılarını kullanın. Son olarak, __Oluştur__ düğmesini kullanarak kümeyi oluşturun.

    ![HDInsight depolama hesabı ayarlarını belirleme](./media/r-server-get-started/clustersummary.png)

    > [!NOTE]  
    > Kümenin oluşturulması 20 dakika sürebilir.

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>RStudio Server’a bağlanma

HDInsight kümenizin parçası olarak RStudio Server Community Edition’ı yüklemeyi seçerseniz, aşağıdaki iyi yöntemden birini kullanarak RStudio oturum açma bölümüne erişebilirsiniz:

* **1. Seçenek** - Aşağıdaki URL’ye gidin (burada **CLUSTERNAME**, oluşturduğunuz ML Services kümesinin adıdır):

        https://CLUSTERNAME.azurehdinsight.net/rstudio/

* **2. seçenek** -Azure portalını kullanın.
  Portaldan:
  1. Seçin **tüm hizmetleri** sol menüden.
  2. Altında **ANALYTICS**seçin **HDInsight kümeleri**.
  3. Küme adınızı seçin **HDInsight kümeleri** sayfası.
  4. Gelen **ML Hizmetleri panolar**seçin **R Studio server**. 

     ![HDInsight depolama hesabı ayarlarını belirleme](./media/r-server-get-started/r-studio-server-dashboard.png)

     > [!IMPORTANT]  
     > Kullanılan yöntem ne olursa olsun, ilk kez oturum açtığınızda iki kez kimlik doğrulaması yapmanız gerekir.  Birinci kimlik doğrulaması isteminde *kümenin Yönetici kullanıcı kimliğini* ve *parolasını* belirtin. İkinci kimlik doğrulaması isteminde *SSH kullanıcı kimliğini* ve *parolasını* belirtin. Sonraki oturum açma işlemleri yalnızca SSH kimlik bilgileri gerektirir.

Bağlandıktan sonra ekranınız aşağıdaki ekran görüntüsüne benzemelidir:

![RStudio’ya bağlanma](./media/r-server-get-started/connect-to-r-studio.png)

## <a name="run-a-sample-job"></a>Örnek iş çalıştırma

ScaleR işlevlerini kullanarak bir iş gönderebilirsiniz. Bir işi çalıştırmak için kullanılan örnek komutlar burada verilmiştir:

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
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

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-cluster-edge-node"></a>Küme kenar düğümüne bağlanma

Bu bölümde, SSH kullanarak bir ML Services HDInsight kümesinin kenar düğümüne nasıl bağlanılacağını öğreneceksiniz. SSH kullanımı hakkında bilgi edinmek için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

ML Services küme kenar düğümüne bağlanmak için SSH komutu şöyledir:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

Kümenize yönelik SSH komutunu bulmak için, Azure portalından küme adına tıklayın, **SSH ve Küme girişi**’ne tıklayın, ardından **Ana bilgisayar adı** için kenar düğümünü seçin. Bu seçim, kenar düğümünün SSH Uç Noktası bilgilerini gösterir.

![Kenar düğümünün SSH Uç Noktası görüntüsü](./media/r-server-get-started/sshendpoint.png)

SSH kullanıcı hesabınızın güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir ortak anahtar kullandıysanız eşleşen özel anahtarı belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Bağlantı kurulduktan sonra aşağıdakine benzer bir isteme ulaşırsınız:

    sshuser@ed00-myrclu:~$

<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>R konsolunu kullanma

1. R konsolunu başlatmak için SSH oturumunda aşağıdaki komutu kullanın:  

        R

2. Diğer bilgilere ek olarak, ML Server sürümünü içeren bir çıktı görmeniz gerekir.
    
3. `>` isteminde R kodunu girebilirsiniz. HDInsight üzerinde ML Services, Hadoop ile kolayca etkileşim kurup dağıtılmış hesaplamaları çalıştırmanıza olanak tanıyan paketler içerir. Örneğin, HDInsight kümesinin varsayılan dosya sisteminin kökünü görüntülemek için aşağıdaki komutu kullanın:

        rxHadoopListFiles("/")

4. WASB stil adreslemesini de kullanabilirsiniz.

        rxHadoopListFiles("wasb:///")

5. R konsolundan çıkmak için aşağıdaki komutu kullanın:

        quit()

## <a name="automated-cluster-creation"></a>Otomatik küme oluşturma

SDK’yı ve PowerShell’i kullanarak HDInsight için ML Services kümesi oluşturma işlemini otomatikleştirebilirsiniz.

<!---* To create an ML Server cluster using an Azure Resource Management template, see [Deploy an R Server for HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).--->
* .NET SDK kullanarak ML Services kümesi oluşturmak için bkz. [.NET SDK kullanarak HDInsight’ta Linux tabanlı kümeler oluşturma.](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* Powershell kullanarak ML Services kümesi oluşturmak için [Azure PowerShell kullanarak HDInsight kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md) başlıklı makaleye bakın.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure HDInsight ve bir SSH oturumundan R konsolunu kullanmanın temel adımlarını yeni ML Hizmetleri kümesi oluşturulacağını öğrendiniz. Aşağıdaki makalelerde HDInsight üzerinde ML Services’ı yönetme ve ML Services ile çalışma için diğer yöntemler açıklanmaktadır:

* [Visual Studio için R Araçları’ndan iş gönderme](r-server-submit-jobs-r-tools-vs.md)
* [HDInsight üzerinde ML Services kümesini yönetme](r-server-hdinsight-manage.md)
* [HDInsight üzerinde ML Services kümesini kullanıma hazır hale getirme](r-server-operationalize.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde ML Services kümesi için Azure Depolama seçenekleri](r-server-storage.md)
