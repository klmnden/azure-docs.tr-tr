---
title: Visual Studio - Azure HDInsight için R Araçları'ndan iş gönderme
description: Bir HDInsight kümesi, yerel Visual Studio makinenizde R işlerini gönderin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: d977d5a25db0cbe641179bce860e9f67c60f29ab
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340810"
---
# <a name="submit-jobs-from-r-tools-for-visual-studio"></a>Visual Studio için R Araçları’ndan iş gönderme

[Visual Studio için R Araçları](https://www.visualstudio.com/vs/rtvs/) (RTVS) olan ücretsiz, açık kaynaklı bir uzantı için Community (ücretsiz), Professional ve Enterprise sürümleri hem [Visual Studio 2017](https://www.visualstudio.com/downloads/), ve [Visual Studio 2015 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=691129)veya üzeri. RTVS için kullanılabilir değil [Visual Studio 2019](https://docs.microsoft.com/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects?view=vs-2019).

Araçlar sunarak RTVS R akışınızı geliştirir [R etkileşimli penceresi](https://docs.microsoft.com/visualstudio/rtvs/interactive-repl) (REPL), IntelliSense (kod tamamlama) [çizim görselleştirme](https://docs.microsoft.com/visualstudio/rtvs/visualizing-data) ggplot2 ve ggviz, gibiRkitaplıklarıaracılığıyla[R kodu hata ayıklama](https://docs.microsoft.com/visualstudio/rtvs/debugging)ve daha fazlası.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

1. Yükleme [Visual Studio için R Araçları](https://docs.microsoft.com/visualstudio/rtvs/installation).

    ![Visual Studio 2017'de RTVS yükleme](./media/r-server-submit-jobs-r-tools-vs/install-r-tools-for-vs.png)

2. Seçin *veri bilimi ve analitik uygulamalar* iş yükü, ardından **R dili desteğini**, **R geliştirme için çalışma zamanı desteği**, ve  **Microsoft R Client** seçenekleri.

3. SSH kimlik doğrulaması için ortak ve özel anahtarları olması gerekir.
   <!-- {TODO tbd, no such file yet}[use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) -->

4. Yükleme [ML Server](https://msdn.microsoft.com/microsoft-r/rserver-install-windows) makinenizde. ML Server sağlar [ `RevoScaleR` ](https://msdn.microsoft.com/microsoft-r/scaler/scaler) ve `RxSpark` işlevleri.

5. Yükleme [PuTTY](https://www.putty.org/) çalıştırmak için bir işlem bağlam sağlamak için `RevoScaleR` HDInsight kümenize yerel istemcinizden işlevleri.

6. R araçları için çalışma alanınız için yeni bir düzen sağlayan Visual Studio ortamınıza veri bilimi ayarları uygulamak için bir seçenekleri var.
   1. Geçerli Visual Studio ayarlarınızı kaydetmek için kullanın **Araçlar > içeri ve dışarı aktarma ayarları** komutunu ve ardından **seçili ortam ayarlarını dışarı aktar** ve bir dosya adı belirtin. Bu ayarları geri yüklemek için aynı komutu kullanın ve seçin **seçili ortam ayarlarını içeri aktarma**.

   2. Git **R Araçları** menü öğesi, ardından **veri bilimi ayarları...** .

       ![Veri bilimi ayarları...](./media/r-server-submit-jobs-r-tools-vs/data-science-settings.png)

      > [!NOTE]  
      > 1\. adımda yaklaşımı kullanarak da kaydedebilir ve kişiselleştirilmiş bir veri Bilimcisi düzeninizi geri yinelenen yerine **veri bilimi ayarları** komutu.

## <a name="execute-local-r-methods"></a>Yerel R yöntemleri çalıştırma

1. ML Hizmetleri HDInsight kümenizi oluşturun.
2. Yükleme [RTVS uzantısı](https://docs.microsoft.com/visualstudio/rtvs/installation).
3. İndirme [örnekleri zip dosyası](https://github.com/Microsoft/RTVS-docs/archive/master.zip).
4. Açık `examples/Examples.sln` çözümünü Visual Studio'da başlatın.
5. Açık `1-Getting Started with R.R` dosyası `A first look at R` Çözüm klasörü.
6. Her satır, teker teker R etkileşimli pencereye göndermek Ctrl + Enter tuşlarına basın dosyasının en üstüne başlatılıyor. Paketleri yüklendikçe bazı satırları biraz sürebilir.
    * Alternatif olarak, tüm satırların R dosyasını (Ctrl + A) seçin, ardından tüm (Ctrl + Enter) yürütmek veya araç çubuğunda yürütme etkileşimli simgesini seçin.
        ![Etkileşimli Yürüt](./media/r-server-submit-jobs-r-tools-vs/execute-interactive.png)

7. Tüm satırları betiği çalıştırdıktan sonra şuna benzer bir çıktı görmeniz gerekir:

    ![Veri bilimi ayarları...](./media/r-server-submit-jobs-r-tools-vs/workspace.png)

## <a name="submit-jobs-to-an-hdinsight-ml-services-cluster"></a>ML Hizmetleri HDInsight kümesine işlerini gönderme

PuTTY ile donatılmış bir Windows bilgisayarında Microsoft ML Server/Microsoft R Client'ı kullanarak dağıtılmış çalıştıracak bir işlem bağlamı oluşturabilirsiniz `RevoScaleR` HDInsight kümenize yerel istemcinizden işlevleri. Kullanım `RxSpark` , kullanıcı adı, Apache Hadoop küme kenar düğümüne, SSH anahtarları ve benzeri belirterek, bir işlem bağlamı oluşturma.

1. HDInsight üzerinde ML Hizmetleri edge düğüm adresi `CLUSTERNAME-ed-ssh.azurehdinsight.net` burada `CLUSTERNAME` ML Hizmetleri kümenizin adıdır.

1. Visual Studio, ortamınızla eşleşecek şekilde Kurulum değişkenlerin değerleri değiştirmeden R etkileşimli pencerede aşağıdaki kodu yapıştırın.

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
    
    ![Spark bağlamı ayarlama](./media/r-server-submit-jobs-r-tools-vs/spark-context.png)

1. R etkileşimli pencerede aşağıdaki komutları yürütün:

    ```R
    rxHadoopCommand("version") # should return version information
    rxHadoopMakeDir("/user/RevoShare/newUser") # creates a new folder in your storage account
    rxHadoopCopy("/example/data/people.json", "/user/RevoShare/newUser") # copies file to new folder
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ![Başarılı rx komut yürütme](./media/r-server-submit-jobs-r-tools-vs/rx-commands.png)

1. Doğrulayın `rxHadoopCopy` başarıyla kopyalandı `people.json` için yeni oluşturulan örnek veri klasörünü dosyasından `/user/RevoShare/newUser` klasörü:

    1. Azure'da, ML Hizmetleri HDInsight küme bölmesinden seçin **depolama hesapları** sol taraftaki menüden.

        ![Depolama hesapları](./media/r-server-submit-jobs-r-tools-vs/storage-accounts.png)

    2. Kapsayıcı/dizin adı not yapmadan kümeniz için varsayılan depolama hesabını seçin.

    3. Seçin **kapsayıcıları** , depolama hesabı bölmesi sol taraftaki menüsünden.

        ![Kapsayıcılar](./media/r-server-submit-jobs-r-tools-vs/containers.png)

    4. Göz atın, kümenizin kapsayıcı adı seçin **kullanıcı** klasöre ('ye tıklamanız gerekebilir *daha fazla Yükle* listenin altındaki), ardından *RevoShare*, ardından **newUser**. `people.json` Dosya görüntüleneceğine `newUser` klasör.

        ![Kopyalanan dosya](./media/r-server-submit-jobs-r-tools-vs/copied-file.png)

1. Geçerli bir Apache Spark bağlamını kullanarak tamamladıktan sonra durdurmanız gerekir. Birden fazla bağlamı aynı anda çalıştıramazsınız.

    ```R
    rxStopEngine(mySparkCluster)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [İçin işlem bağlamı seçenekleri ML hizmetleri üzerinde HDInsight](r-server-compute-contexts.md)
* [ScaleR ve SparkR birleştirme](../hdinsight-hadoop-r-scaler-sparkr.md) Havayolu uçuşlarda rötar tahmini bir örnek sağlar.

