---
title: "Intellij için Azure Araç Seti: SSH üzerinden uzaktan hata ayıklama Spark uygulamaları | Microsoft Docs"
description: "SSH Hdınsight araçları Azure Araç Seti Intellij için Hdınsight uygulamaları uzaktan hata ayıklamak için nasıl kullanılacağı hakkında adım adım yönergeler kümeleri"
keywords: "ıntellij, debugging ıntellij, ssh, uzak ıntellij, hdınsight uzaktan hata ayıklama, ıntellij, hata ayıklama hata ayıklama"
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 11/25/2017
ms.author: jejiang
ms.openlocfilehash: 87cda776195dc93a35c6e978b18e823bf54c9ffb
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Spark uygulamalarında yerel olarak veya uzaktan Hdınsight kümesinde Azure araç seti ile SSH aracılığıyla Intellij için hata ayıklama

Bu makalede, Hdınsight araçları Azure Araç Seti Intellij için Hdınsight kümesi üzerinde uzaktan uygulamalarında hata ayıklamak için nasıl kullanılacağı hakkında adım adım yönergeler sağlar. Projenizin hatalarını ayıklamak için de görüntüleyebilirsiniz [Intellij için Hdınsight Spark hata ayıklama uygulamaları Azure araç seti ile](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.

**Önkoşullar**
* **Azure Araç Seti Intellij için Hdınsight Araçları**. Bu araç Intellij için Azure araç seti bir parçasıdır. Daha fazla bilgi için bkz: [Intellij için Azure Araç Seti yüklemek](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation). Ve **Intellij için Azure Araç Seti**. Bu araç seti, Spark Hdınsight kümesi için uygulamalar oluşturmak için kullanın. Daha fazla bilgi için ' ndaki yönergeleri izleyin [Spark Hdınsight kümesi için uygulamalar oluşturmak üzere Intellij için kullanım Azure Araç Seti](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).

* **Hdınsight SSH hizmeti kullanıcı adı ve parola yönetimi ile**. Daha fazla bilgi için bkz: [SSH kullanarak Hdınsight (Hadoop) Bağlan](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) ve [Ambari erişmek için tünel SSH kullanma web kullanıcı Arabirimi, kaynak, iş, Oozie ve diğer web Uı'lar](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 
## <a name="learn-how-to-perform-local-run-and-debugging"></a>Yerel gerçekleştirmek çalıştırın ve hata ayıklama öğrenin
### <a name="scenario-1-create-a-spark-scala-application"></a>Senaryo 1: bir Spark Scala uygulaması oluşturma 

1. Intellij Idea başlatın ve bir proje oluşturun. İçinde **yeni proje** iletişim kutusunda, aşağıdakileri yapın:

   a. Seçin **Hdınsight**. 

   b. Tercihinize bağlı bir Java veya Scala şablonu seçin. Aşağıdaki seçenekler arasından seçin:

      - **(Scala) hdınsight'ta Spark**

      - **(Java) hdınsight'ta Spark**

      - **Spark Hdınsight örneği (Scala)**

      Bu örnekte bir **Spark Hdınsight örneği (Scala)** şablonu.

   c. İçinde **oluşturma aracını** listesinde, gereksiniminize göre aşağıdakilerden birini seçin:

      - **Maven**, Scala Proje Oluşturma Sihirbazı'nı desteği

      -  **SBT**, bağımlılıkları yönetme ve Scala projeyi oluşturmak için 

      ![Hata ayıklama projesi oluşturma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Seçin **sonraki**.     
 
2. Sonraki **yeni proje** penceresinde aşağıdakileri yapın:

   ![SDK Spark seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Bir proje adı ve proje konumu girin.

   b. İçinde **proje SDK** aşağı açılan listesinden, **Java 1.8** için **2.x Spark** seçin ya da küme **Java 1.7** için **1.x Spark** küme.

   c. İçinde **Spark sürüm** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK ve Scala SDK için doğru sürümü tümleştirir. Spark kümesi sürüm 2.0 eskiyse, seçin **1.x Spark**. Aksi takdirde seçin **2.x Spark.** Bu örnekte **Spark 2.0.2 (Scala 2.11.8)**.

   d. **Son**’u seçin.

3. Seçin **src** > **ana** > **scala** projede kodunuzu açmak için. Bu örnekte **SparkCore_wasbloTest** komut dosyası.

### <a name="prerequisite-for-windows"></a>Windows için önkoşul
Bir Windows bilgisayarda yerel Spark Scala uygulama çalıştırıyorsanız, ancak açıklandığı gibi bir özel durum alabilirsiniz [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). WinUtils.exe Windows eksik olduğundan özel durum oluşur. 

Bu hatayı gidermek için [yürütülebilir dosya indirme](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) gibi bir konuma **C:\WinUtils\bin**. Ardından, ortam değişkenine ekleyin **HADOOP_HOME**ve değişkenin değerini ayarlamak **C:\WinUtils**.

### <a name="scenario-2-perform-local-run"></a>Senaryo 2: yerel çalıştırma gerçekleştirin
1. Açık **SparkCore_wasbloTest** komut dosyası, komut dosyası Düzenleyicisi'ni sağ tıklatın ve seçeneğini **'[Spark iş] XXX' Çalıştır** yerel çalıştırma gerçekleştirmek için.
2. Bir kez yerel çalıştırma, tamamlandı, geçerli proje Gezgini Kaydet çıktı dosyası görebilirsiniz **veri** > **__varsayılan__**.

    ![Yerel çalıştırma sonucu](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-result.png)
3. Araçlarımız otomatik olarak yerel çalıştırma ve yerel gerçekleştirdiğinizde yerel çalışma yapılandırması hata ayıklama varsayılan ayarladınız. Yapılandırması'nı açmak **[Spark iş] XXX** bölmenin sağ üst köşedeki üzerinde gördüğünüz **[Spark iş] XXX** altında zaten oluşturulmuş **Azure Hdınsight Spark işi**. Geçiş **yerel olarak çalıştırma** sekmesi.

    ![Yerel çalışma yapılandırma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)
    - [Ortam değişkenleri](#prerequisite-for-windows): Sistem ortam değişkeni zaten ayarladıysanız **HADOOP_HOME** için **C:\WinUtils**, onu otomatik algılayan el ile eklemeniz gerekmez.
    - [WinUtils.exe konumu](#prerequisite-for-windows): Sistem ortam değişkenini ayarlamadıysanız, düğmesine tıklayarak konumu bulabilirsiniz.
    - Yalnızca iki seçenekten birini seçin ve MacOS ve Linux gerekmez.
4. Yerel çalışma ve yerel hata ayıklama yapmadan önce el ile yapılandırma de ayarlayabilirsiniz. Önceki ekran görüntüsünde, artı işaretini seçin (**+**). Ardından **Azure Hdınsight Spark işi** seçeneği. İçin bilgileri girin **adı**, **ana sınıf adı** kaydetmek için ardından yerel çalıştırma düğmesine tıklayın.

### <a name="scenario-3-perform-local-debugging"></a>Senaryo 3: yerel hata ayıklama gerçekleştirmek
1. Açık **SparkCore_wasbloTest** komut dosyası, kümesi kesme noktaları.
2. Komut Dosyası Düzenleyicisi'ni sağ tıklatın ve ardından istediğiniz seçeneği belirleyin **'[Spark iş] XXX' hata ayıklama** yerel hata ayıklama gerçekleştirmek için.   



## <a name="learn-how-to-perform-remote-run-and-debugging"></a>Uzaktan gerçekleştirmek çalıştırın ve hata ayıklama öğrenin
### <a name="scenario-1-perform-remote-run"></a>Senaryo 1: Uzak çalışma gerçekleştirme

1. Erişim için **yapılandırmalarını Düzenle** menüsünde sağ üst köşedeki simgeyi seçin. Bu menüden oluşturabilir veya uzaktan hata ayıklama için yapılandırmaları düzenleyebilirsiniz.

   ![Yapılandırmaları Düzenle](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

2. İçinde **Çalıştır/hata ayıklama yapılandırmaları** iletişim kutusunda, artı işaretini seçin (**+**). Ardından **Azure Hdınsight Spark işi** seçeneği.

   ![Yeni yapılandırma ekleyin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
3. Geçiş **uzaktan çalıştırmak kümedeki** sekmesi. İçin bilgileri girin **adı**, **Spark kümesi**, ve **ana sınıf adı**. Ardından **Gelişmiş Yapılandırma**. Araçlarımız ile hata ayıklama desteği **yürütücüler**. **NumExectors**, varsayılan değer 5'tir. Daha iyi 3'ten daha yüksek ayarlamazsınız.

   ![Hata ayıklama yapılandırmaları çalıştırma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

4. İçinde **Spark gönderimi Gelişmiş Yapılandırma** iletişim kutusunda **etkinleştirmek Spark uzaktan hata ayıklama**. SSH kullanıcı adı girin ve ardından bir parola girin veya özel bir anahtar dosyası kullanın. Yapılandırmayı kaydetmek için seçin **Tamam**. Uzaktan hata ayıklama gerçekleştirmek istiyorsanız, bunu ayarlamanız gerekir. Yalnızca uzaktan çalıştırmak kullanmak istiyorsanız, bunu ayarlamak için gerek yoktur.

   ![Spark uzaktan hata ayıklama etkinleştirme](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

5. Yapılandırma, belirttiğiniz adla şimdi kaydedilir. Yapılandırma ayrıntılarını görüntülemek için yapılandırma adı seçin. Değişiklik yapmak için seçin **yapılandırmalarını Düzenle**. 

6. Yapılandırma ayarları tamamladıktan sonra projeyi uzak küme karşı çalıştırın veya uzaktan hata ayıklama gerçekleştirin.
   
   ![Uzak Çalıştır düğmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run.png)

7. Tıklatın **Bağlantıyı Kes** gönderme günlükleri sol panelde gösterilmiyor düğmesi. Ancak, arka uçta hala çalışıyor.

   ![Uzak Çalıştır düğmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/remote-run-result.png)



### <a name="scenario-2-perform-remote-debugging"></a>Senaryo 2: uzaktan hata ayıklama gerçekleştirin
1. Kesme noktaları ayarlayın ve ardından **uzaktan hata ayıklama** simgesi. SSH kullanıcı adı/parola gereken yapılandırılması ile uzaktan gönderimi farktır.

   ![Hata ayıklama simgesini seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

2. Program yürütme kesme noktası ulaştığında, gördüğünüz bir **sürücü** sekmesi ve iki **Yürütücü** sekmelerinde **hata ayıklayıcı** bölmesi. Seçin **Sürdür Program** sonraki kesme ulaştığında kod çalıştırmaya devam etmek için simge. Doğru geçiş yapmanız **Yürütücü** hata ayıklamak için hedef Yürütücü bulmak için sekmesini. Karşılık gelen üzerinde yürütme günlüklerini görüntüleyebilirsiniz **konsol** sekmesi.

   ![Hata ayıklama sekmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-3-perform-remote-debugging-and-bug-fixing"></a>Senaryo 3: uzaktan hata ayıklama ve hata düzeltme gerçekleştirme
1. İki kesme noktaları ayarlayın ve ardından **hata ayıklama** uzaktan hata ayıklama işlemini başlatmak için simge.

2. Kod ilk kesme noktasına durdurur ve parametre ve değişken bilgileri gösterilir **değişkenleri** bölmesi. 

3. Seçin **Sürdür Program** devam etmek için simge. Kod ikinci bir noktada durdurur. Beklendiği gibi özel durum yakalandı.

   ![Hata durum](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png) 

4. Seçin **Sürdür Program** yeniden simgesi. **Hdınsight Spark gönderme** penceresi bir "işi çalıştırma başarısız oldu" hatasını görüntüler.

   ![Hata gönderme](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png) 

5. Değişken değeri yetenek hata ayıklama Intellij kullanarak dinamik olarak güncelleştirmek için seçin **hata ayıklama** yeniden. **Değişkenleri** yeniden bölmesinde görünür. 

6. Hedef sağ **hata ayıklama** sekmesini tıklatın ve ardından **değeri ayarlanmış**. Ardından, değişken için yeni bir değer girin. Ardından **Enter** değeri kaydetmek için. 

   ![Değerini ayarla](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value.png) 

7. Seçin **Sürdür Program** program çalıştırmaya devam etmek için simge. Bu süre, hiçbir özel durum yakalandı. Proje başarıyla tüm özel durumlar çalışır görebilirsiniz.

   ![Özel durum olmadan hata ayıklama](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Sonraki adımlar
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="demo"></a>Tanıtım
* Scala proje oluştur (video): [Spark Scala uygulamaları oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [uzaktan bir Hdınsight kümesi üzerinde Spark uygulamalarında hata ayıklamasını Intellij için kullanım Azure Araç Seti](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla Hdınsight'ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için hdınsight'ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [Spark akış: Gerçek zamanlı akış uygulamaları oluşturmak için hdınsight'ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](../hdinsight-apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Intellij için Azure Araç Seti Spark Hdınsight kümesi için uygulamalar oluşturmak için kullanın](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Hdınsight araçları Azure araç setini Eclipse için Spark uygulamaları oluşturmak için kullanın](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [Hdınsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
