---
title: "Visual Studio - Azure Hdınsight R araçları işlerden gönderme | Microsoft Docs"
description: "Hdınsight kümesi için yerel Visual Studio makinenizden R işleri gönderin."
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: 1a82ba7790f739768156a8bee33a74d7130e24e1
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="submit-jobs-from-r-tools-for-visual-studio"></a>Visual Studio için R araçları işlerden gönderme

[Visual Studio için R Araçları](https://www.visualstudio.com/vs/rtvs/) (RTVS) Community (ücretsiz) için ücretsiz, açık kaynaklı bir uzantısı olduğundan Professional ve Enterprise sürümleri hem [Visual Studio 2017](https://www.visualstudio.com/downloads/), ve [Visual Studio 2015 güncelleştirme 3](http://go.microsoft.com/fwlink/?LinkId=691129) ya da daha yüksek.

RTVS araçları gibi sunarak R akışınızı geliştirir [R etkileşimli pencere](https://docs.microsoft.com/visualstudio/rtvs/interactive-repl) (REPL) IntelliSense (kod tamamlama) [çizim görselleştirme](https://docs.microsoft.com/visualstudio/rtvs/visualizing-data) ggplot2 ve ggviz, gibiRkitaplıklarıaracılığıyla[R kodda hata ayıklama](https://docs.microsoft.com/visualstudio/rtvs/debugging)ve daha fazlası.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

1. Yükleme [R araçları Visual Studio için](https://docs.microsoft.com/visualstudio/rtvs/installation).

    ![Visual Studio 2017 içinde RTVS yükleme](./media/r-server-submit-jobs-r-tools-vs/install-r-tools-for-vs.png)

2. Seçin *veri bilimi ve analitik uygulamaları* iş yükü, ardından **R dil desteği**, **R geliştirmesi için çalışma zamanı desteği**, ve  **Microsoft R istemci** seçenekleri.

3. SSH kimlik doğrulaması için ortak ve özel anahtarları olması gerekir.
<!-- {TODO tbd, no such file yet}[use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) -->

4. Yükleme [R Server](https://msdn.microsoft.com/microsoft-r/rserver-install-windows) makinenizde. R Server sağlar [ `RevoScaleR` ](https://msdn.microsoft.com/microsoft-r/scaler/scaler) ve `RxSpark` işlevleri.

5. Yükleme [PuTTY](http://www.putty.org/) çalıştırmak için bir işlem bağlamı sağlamak için `RevoScaleR` Hdınsight kümenize yerel istemcinizden işlevleri.

6. R araçları için çalışma alanınız için yeni bir düzen sağlar, Visual Studio ortamınıza veri bilimi ayarları uygulamak için seçeneğiniz vardır.
    1. Geçerli Visual Studio ayarlarınızı kaydetmek için kullanın **Araçlar > içeri ve dışarı aktarma ayarları** komutunu ve ardından **verme seçili ortam ayarlarını** ve bir dosya adı belirtin. Bu ayarları geri yüklemek için aynı komutunu kullanın ve seçin **seçili ortam ayarları**.

    2. Git **R Araçları** menü öğesi, ardından **veri bilimi ayarları...** .

        ![Veri bilimi ayarları...](./media/r-server-submit-jobs-r-tools-vs/data-science-settings.png)

    > [!NOTE]
    > 1. adımda yaklaşımı kullanarak, aynı zamanda kaydedebilir ve kişiselleştirilmiş veri Bilimcisi düzeninizi geri yinelenen yerine **veri bilimi ayarları** komutu.

## <a name="execute-local-r-methods"></a>Yerel bir R yöntem yürütülemez

1. Oluştur, [R Server Hdınsight kümesi](r-server-get-started.md).
2. Yükleme [RTVS uzantısı](https://docs.microsoft.com/visualstudio/rtvs/installation).
3. Karşıdan [samples ZIP dosyası](https://github.com/Microsoft/RTVS-docs/archive/master.zip).
4. Açık `examples/Examples.sln` Visual Studio'da Çözüm başlatmak için.
5. Açık `1-Getting Started with R.R` dosyasını `A first look at R` Çözüm klasörü.
6. Dosya en üstte, her satır, bir seferde bir R etkileşimli penceresine göndermek Ctrl + Enter tuşlarına başlatılıyor. Paketleri yükleme gibi bazı satırlar biraz zaman alabilir.
    * Alternatif olarak, R dosyasında (Ctrl + A) tüm satırları seçin, sonra tüm (Ctrl + Enter) yürütmek veya araç çubuğundaki yürütme etkileşimli simgesini seçin.
        ![Etkileşimli yürütme](./media/r-server-submit-jobs-r-tools-vs/execute-interactive.png)

7. Tüm satırları betiği çalıştırdıktan sonra aşağıdakine benzer bir çıktı görmeniz gerekir:

    ![Veri bilimi ayarları...](./media/r-server-submit-jobs-r-tools-vs/workspace.png)

## <a name="submit-jobs-to-an-hdinsight-r-cluster"></a>Bir Hdınsight R kümeye iş göndermek

Bir Microsoft R Server/Microsoft R istemciden PuTTY ile donatılmış bir Windows bilgisayar kullanarak, dağıtılmış çalışacak bir işlem bağlamı oluşturabilirsiniz `RevoScaleR` Hdınsight kümenize yerel istemcinizden işlevleri. Kullanım `RxSpark` , kullanıcı adı, Hadoop küme kenar düğümünü, SSH anahtarları ve benzeri belirtme işlem içeriği oluşturulamadı.

1. Edge düğümün ana bilgisayar adını bulun, Azure üzerinde Hdınsight R küme bölmesini açın ve ardından seçin için **güvenli Kabuk (SSH)** genel bakış bölmesinde üst menüsünde.

    ![Secure Shell (SSH)](./media/r-server-submit-jobs-r-tools-vs/ssh.png)

2. Kopya **kenar düğümü ana bilgisayar adı** değeri.

    ![Uç düğüm ana bilgisayar adı](./media/r-server-submit-jobs-r-tools-vs/edge-node.png)

3. Visual Studio, ortamınıza uyum sağlaması için Kurulum değişkenlerin değerleri değiştirerek R etkileşimli penceresinde aşağıdaki kodu yapıştırın.

    ```R
    # Setup variables that connect the compute context to your HDInsight cluster
    mySshHostname <- 'r-cluster-ed-ssh.azurehdinsight.net ' # HDI secure shell hostname
    mySshUsername <- 'sshuser' # HDI SSH username
    mySshClientDir <- "C:\\Program Files (x86)\\PuTTY"
    mySshSwitches <- '-i C:\\Users\\azureuser\\r.ppk' # Path to your private ssh key
    myHdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep = "/")
    myShareDir <- paste("/var/RevoShare", mySshUsername, sep = "/")
    mySshProfileScript <- "/usr/lib64/microsoft-r/3.3/hadoop/RevoHadoopEnvVars.site"

    # Create the Spark Cluster compute context
    mySparkCluster <- RxSpark(
          sshUsername = mySshUsername,
      sshHostname = mySshHostname,
      sshSwitches = mySshSwitches,
      sshProfileScript = mySshProfileScript,
      consoleOutput = TRUE,
      hdfsShareDir = myHdfsShareDir,
      shareDir = myShareDir,
      sshClientDir = mySshClientDir
    )
    
    # Set the current compute context as the Spark compute context defined above
    rxSetComputeContext(mySparkCluster)
    ```
    
    ![Spark bağlam ayarlama](./media/r-server-submit-jobs-r-tools-vs/spark-context.png)

4. R etkileşimli penceresinde aşağıdaki komutları çalıştırın:

    ```R
    rxHadoopCommand("version") # should return version information
    rxHadoopMakeDir("/user/RevoShare/newUser") # creates a new folder in your storage account
    rxHadoopCopy("/example/data/people.json", "/user/RevoShare/newUser") # copies file to new folder
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ![Başarılı rx komutu yürütme](./media/r-server-submit-jobs-r-tools-vs/rx-commands.png)

5. Doğrulayın `rxHadoopCopy` başarıyla kopyalanan `people.json` dosyasına örnek veri klasöründen yeni oluşturulan `/user/RevoShare/newUser` klasörü:

    1. Azure'da, Hdınsight R küme bölmesinden seçin **depolama hesapları** sol taraftaki menüden.

        ![Depolama hesapları](./media/r-server-submit-jobs-r-tools-vs/storage-accounts.png)

    2. Kapsayıcı/dizin adını not yapmadan kümeniz için varsayılan depolama hesabı seçin.

    3. Seçin **kapsayıcıları** depolama hesabı bölmesi sol menüsünden.

        ![Kapsayıcılar](./media/r-server-submit-jobs-r-tools-vs/containers.png)

    4. Kümenizin kapsayıcı adı seçin, Gözat **kullanıcı** klasörü (tıklatın gerekebilir *daha fazla yük* listesinin altındaki) seçeneğini belirleyip *RevoShare*, sonra **newUser**. `people.json` Dosya görüntülenmesi `newUser` klasör.

        ![Kopyalanan dosya](./media/r-server-submit-jobs-r-tools-vs/copied-file.png)

6. Geçerli Spark bağlamı kullanarak tamamladıktan sonra durdurmanız gerekir. Birden çok bağlamları aynı anda çalıştırılamaz.

    ```R
    rxStopEngine(mySparkCluster)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde R Server için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [ScaleR ve SparkR birleştirme](../hdinsight-hadoop-r-scaler-sparkr.md) uçak uçuş gecikme tahminleri ilişkin bir örnek sağlar.
<!-- * You can also submit R jobs with the [R Studio Server](hdinsight-submit-jobs-from-r-studio-server.md) -->
