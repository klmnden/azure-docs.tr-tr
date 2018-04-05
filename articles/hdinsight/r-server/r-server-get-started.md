---
title: HDInsight'ta R Server ile çalışmaya başlama - Azure | Microsoft Docs
description: HDInsight kümesi üzerinde R Server içeren bir Apache Spark oluşturma ve küme üzerinde bir R betiği gönderme hakkında bilgi edinin.
services: HDInsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: f4265ce7370542d8253222a5e268ea9cde7fd36e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="get-started-with-r-server-cluster-on-azure-hdinsight"></a>Azure HDInsight üzerinde R Server kümesini kullanmaya başlama

Azure HDInsight, HDInsight kümenizle tümleştirilecek bir R Server seçeneği içerir. Bu seçenek, R betiklerinin dağıtılmış hesaplamaları çalıştırmak için Spark ve MapReduce kullanmasına olanak tanır. Bu makalede, HDInsight kümesinde R Server oluşturmayı öğreneceksiniz. Ardından dağıtılmış R hesaplamalarında Spark kullanımını gösteren R betiğini çalıştırmayı öğreneceksiniz.


## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**: Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Daha fazla bilgi için bkz. [Microsoft Azure ücretsiz denemesini alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Güvenli Kabuk (SSH) istemcisi**: HDInsight kümesine uzaktan bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için bir SSH istemcisi kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a>Azure portalını kullanarak küme oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Kaynak oluştur** > **Veri ve Analiz** > **HDInsight** seçeneğine tıklayın.

3. **Temel Bilgiler** bölümünden aşağıdaki bilgileri girin:

    * **Küme Adı**: HDInsight kümesinin adı.
    * **Abonelik**: Kullanılacak abonelik.
    * **Küme oturumu kullanıcı adı** ve **Küme oturumu parolası**: HTTPS üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Ambari Web kullanıcı arabirimi veya REST API gibi hizmetlere erişmek için bu kimlik bilgilerini kullanın.
    * **Güvenli Kabuk (SSH) kullanıcı adı**: SSH üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Varsayılan olarak parola, küme oturum açma parolası ile aynıdır.
    * **Kaynak Grubu**: Kümenin oluşturulduğu kaynak grubu.
    * **Konum**: Kümenin oluşturulacağı Azure bölgesi.

        ![Küme temel ayrıntıları](./media/r-server-get-started/clustername.png)

4. **Küme türü**’nü seçin, ardından **Küme yapılandırması** bölümünde aşağıdaki değerleri ayarlayın:

    * **Küme Türü**: R Server

    * **İşletim Sistemi**: Linux

    * **Sürüm**: R Server 9.1 (HDI 3.6). Kullanılabilir R Server sürümlerinin sürüm notları [docs.microsoft.com](https://docs.microsoft.com/machine-learning-server/whats-new-in-r-server#r-server-91) sitesinde bulunabilir.

    * **R Server için R Studio topluluk sürümü**: Tarayıcı tabanlı bu IDE, kenar düğümüne varsayılan olarak yüklenir. Yüklememeyi tercih ederseniz, onay kutusunun işaretini kaldırın. Yüklemeyi seçerseniz, RStudio Server oturum açma sayfasına erişim URL'si, oluşturulan kümenizin portal uygulaması dikey penceresinde bulunur.

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

* **1. Seçenek** - Aşağıdaki URL’ye gidin (burada **CLUSTERNAME**, oluşturduğunuz R Server kümesinin adıdır):

        https://CLUSTERNAME.azurehdinsight.net/rstudio/

* **2. Seçenek** - Azure portalında, **Hızlı bağlantılar** bölümünde **R Server Panoları**’na tıklayarak R Server kümesini açın.

     ![HDInsight depolama hesabı ayarlarını belirleme](./media/r-server-get-started/dashboard-quick-links.png)

    **Küme Panoları**’nda **R Studio Server** seçeneğine tıklayın.

    ![HDInsight depolama hesabı ayarlarını belirleme](./media/r-server-get-started/r-studio-server-dashboard.png)

   > [!IMPORTANT]
   > Kullanılan yöntem ne olursa olsun, ilk kez oturum açtığınızda iki kez kimlik doğrulaması yapmanız gerekir.  Birinci kimlik doğrulaması isteminde *kümenin Yönetici kullanıcı kimliğini* ve *parolasını* belirtin. İkinci kimlik doğrulaması isteminde *SSH kullanıcı kimliğini* ve *parolasını* belirtin. Sonraki oturumlarda yalnızca SSH kimlik bilgileri gerekli olacaktır.

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

Bu bölümde, SSH kullanarak bir R Server HDInsight kümesinin kenar düğümüne nasıl bağlanılacağını öğreneceksiniz. SSH kullanımı hakkında bilgi edinmek için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

R Server küme kenar düğümüne bağlanmak için SSH komutu şöyledir:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

Kümenize yönelik SSH komutunu bulmak için, Azure portalından küme adına tıklayın, **SSH ve Küme girişi**’ne tıklayın, ardından **Ana bilgisayar adı** için kenar düğümünü seçin. Bu seçim, kenar düğümünün SSH Uç Noktası bilgilerini gösterir.

![Kenar düğümünün SSH Uç Noktası görüntüsü](./media/r-server-get-started/sshendpoint.png)

SSH kullanıcı hesabınızın güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir ortak anahtar kullandıysanız eşleşen özel anahtarı belirtmek için `-i` parametresini kullanmanız gerekebilir. Örnek:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Bağlantı kurulduktan sonra aşağıdakine benzer bir isteme ulaşırsınız:

    sshuser@ed00-myrclu:~$

<a name="use-r-console"></a>
## <a name="use-the-r-server-console"></a>R Server konsolunu kullanma

1. R konsolunu başlatmak için SSH oturumunda aşağıdaki komutu kullanın:  

        R

2. Diğer bilgilere ek olarak, R Server sürümünü içeren bir çıktı görmeniz gerekir.
    
3. `>` isteminde R kodunu girebilirsiniz. HDInsight üzerinde R Server, Hadoop ile kolayca etkileşim kurup dağıtılmış hesaplamaları çalıştırmanıza olanak tanıyan paketler içerir. Örneğin, HDInsight kümesinin varsayılan dosya sisteminin kökünü görüntülemek için aşağıdaki komutu kullanın:

        rxHadoopListFiles("/")

4. WASB stil adreslemesini de kullanabilirsiniz.

        rxHadoopListFiles("wasb:///")

5. R konsolundan çıkmak için aşağıdaki komutu kullanın:

        quit()

## <a name="automated-cluster-creation"></a>Otomatik küme oluşturma

Azure Resource Manager şablonlarını, SDK’yı ve PowerShell’i kullanarak HDInsight için R Server kümesi oluşturma işlemini otomatikleştirebilirsiniz.

* Azure Kaynak Yönetimi şablonu kullanarak R Server kümesi oluşturmak için bkz. [HDInsight için R Server kümesi dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* .NET SDK kullanarak R Server kümesi oluşturmak için bkz. [.NET SDK kullanarak HDInsight’ta Linux tabanlı kümeler oluşturma.](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* Powershell kullanarak R Server kümesi oluşturmak için [Azure PowerShell kullanarak HDInsight kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md) başlıklı makaleye bakın.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure HDInsight üzerinde yeni bir R Server kümesinin nasıl oluşturulacağını ve bir SSH oturumundan R konsolunu kullanmanın temel adımlarını öğrendiniz. Aşağıdaki makalelerde HDInsight üzerinde R Server’ı yönetme ve R Server ile çalışma için diğer yöntemler açıklanmaktadır:

* [Visual Studio için R Araçları’ndan iş gönderme](r-server-submit-jobs-r-tools-vs.md)
* [HDInsight üzerinde R Server kümesini yönetme](r-server-hdinsight-manage.md)
* [HDInsight üzerinde R Server kümesini kullanıma hazır hale getirme](r-server-operationalize.md)
* [HDInsight üzerinde R Server kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde R Server kümesi için Azure Depolama seçenekleri](r-server-storage.md)
