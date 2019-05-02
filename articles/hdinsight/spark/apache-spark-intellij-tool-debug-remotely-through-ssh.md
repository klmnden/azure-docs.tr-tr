---
title: 'Intellij için Azure Araç Seti: Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama '
description: HDInsight uygulamaları uzaktan hata ayıklama için Intellij için Azure araç seti, HDInsight araçları kullanma hakkında adım adım rehberlik SSH üzerinden kümeleri
keywords: Uzaktan hata ayıklama işlemi ıntellij, uzaktan hata ıntellij ssh, ıntellij, hdınsight, ıntellij, hata ayıklama hata ayıklama
ms.service: hdinsight
author: hrasheed
ms.author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/25/2017
ms.openlocfilehash: ef2507a15579ea3d145bfe37df281e2c044d181c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64722329"
---
# <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Apache Spark uygulamaları yerel olarak veya uzaktan Azure araç seti ile HDInsight kümesinde SSH üzerinden Intellij için hata ayıklama

Bu makalede, HDInsight araçları kullanma hakkında adım adım yönergeleri sağlanır. [Intellij için Azure Araç Seti](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) bir HDInsight kümesi üzerinde uzaktan uygulamalarında hata ayıklamak için. Projenizin hatalarını ayıklama için ayrıca görüntüleyebilirsiniz [Intellij için Azure araç seti ile hata ayıklama HDInsight Spark uygulamaları](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.

**Önkoşullar**
* **Intellij için Azure Araç Seti HDInsight Araçları**. Bu araç Intellij için Azure Araç Takımı'nın bir parçasıdır. Daha fazla bilgi için [Intellij için Azure Araç Seti'ni yükleme](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation). Ve **Intellij için Azure Araç Seti**. Bu araç seti, HDInsight kümesi için Apache Spark uygulamaları oluşturmak için kullanın. Daha fazla bilgi için bölümündeki yönergeleri [bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak Intellij için Azure Araç Seti'ni kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).

* **Kullanıcı adı ve parola yönetimi ile HDInsight SSH hizmeti**. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) ve [Ambari erişim için tünel SSH kullanma web kullanıcı Arabirimi, JobHistory, NameNode, Apache Oozie ve diğer web kullanıcı arabirimleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 
## <a name="learn-how-to-perform-local-run-and-debugging"></a>Çalıştırma ve hata ayıklama yerel gerçekleştirme hakkında bilgi edinin
### <a name="scenario-1-create-a-spark-scala-application"></a>Senaryo 1: Bir Scala Spark uygulaması oluşturma 

1. IntelliJ IDEA’yı başlatın ve sonra bir proje oluşturun. **Yeni Proje** iletişim kutusunda aşağıdakileri yapın:

   a. Seçin **Azure Spark HDInsight**. 

   b. Tercihinize bağlı bir Java veya Scala şablonu seçin. Aşağıdaki seçeneklerden birini belirleyebilirsiniz:

   - **Spark proje (Java)**

   - **Spark proje (Scala)**

   - **Spark projesiyle örnekleri (Scala)**

   - **Başarısız görev örnekleri (Önizleme) (Scala) hata ayıklama ile Spark proje**

     Bu örnekte bir **örnekleri (Scala) ile Spark proje** şablonu.

   c. **Derleme aracı** listesinde, gereksiniminize göre aşağıdakilerden birini seçin:

   - Scala projesi oluşturma sihirbazı desteği için **Maven**

   - Scala projesi için bağımlılıkları ve derlemeyi yönetmek için **SBT** 

     ![Hata ayıklama projesi oluşturma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. **İleri**’yi seçin.     
 
1. Sonraki **yeni proje** penceresinde aşağıdakileri yapın:

   ![SDK'sı Spark'ı seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Bir proje adı ve proje konumu girin.

   b. İçinde **proje SDK'sı** aşağı açılan listesinden **Java 1.8** için **2.x Spark** seçin ya da küme **Java 1.7** için **1.x Spark**  kümesi.

   c. İçinde **Spark sürümü** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK'sı ve Scala SDK'sı için doğru sürüm tümleştirilir. Spark kümesi sürüm 2.0 eskiyse, seçin **1.x Spark**. Aksi takdirde seçin **2.x Spark.** Bu örnek, **Spark 2.0.2 (Scala 2.11.8)** kullanır.

   d. **Son**’u seçin.

1. Seçin **src** > **ana** > **scala** kodunuzu projede açmak için. Bu örnekte **SparkCore_wasbloTest** betiği.

### <a name="prerequisite-for-windows"></a>Windows önkoşulları
Bir Windows bilgisayarda yerel Scala Spark uygulaması çalışıyor olsa da açıklandığı gibi özel durum alabilir [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). Windows üzerinde WinUtils.exe eksik olduğu için özel durum oluşur. 

Bu hatayı gidermek için [yürütülebilir dosyayı indir](https://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) gibi bir konuma **C:\WinUtils\bin**. Ardından, ortam değişkenini ekledikten **HADOOP_HOME**, için değişkenin değerini ayarlayıp **C:\WinUtils**.

### <a name="scenario-2-perform-local-run"></a>Senaryo 2: Yerel çalıştırma gerçekleştirin
1. Açık **SparkCore_wasbloTest** komut dosyası, Kod Düzenleyicisi'ni sağ tıklatın ve seçeneğini **'[Spark işi] XXX' çalıştırma** yerel çalıştırma gerçekleştirmek için.
1. Bir kez yerel çalıştırma tamamlandı, geçerli proje gezgininizde kaydetme çıkış dosyası görebilirsiniz **veri** > **__varsayılan__**.

    ![Yerel çalıştırma sonucu](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-result.png)
1. Varsayılan değer otomatik olarak yerel çalıştırma ve yerel gerçekleştirdiğinizde yerel çalışma yapılandırması hata ayıklama araçlarımızı ayarladınız. Yapılandırması **[HDInsight üzerinde Spark] XXX** bölmenin sağ üst köşesinde gördüğünüz **[HDInsight üzerinde Spark] XXX** altında zaten oluşturulmuş **HDInsight üzerinde Apache Spark**. Geçiş **yerel olarak çalıştırma** sekmesi.

    ![Yerel çalıştırma yapılandırma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)
    - [Ortam değişkenlerini](#prerequisite-for-windows): Sistem ortam değişkeni zaten ayarlanmışsa **HADOOP_HOME** için **C:\WinUtils**, bunu otomatik olarak ölçeklendirilebilen algılayan el ile eklemeniz gerekmez.
    - [WinUtils.exe konumu](#prerequisite-for-windows): Sistem ortam değişkeninin ayarlı değil, konumu, düğmesine tıklayarak bulabilirsiniz.
    - Yalnızca iki seçenekten birini seçin ve MacOS ve Linux'ta gerekmez.
1. Yerel çalıştırma ve yerel hata ayıklama işlemini gerçekleştirmeden önce el ile yapılandırma de ayarlayabilirsiniz. Önceki ekran görüntüsünde, artı işaretini seçin (**+**). Ardından **HDInsight üzerinde Apache Spark** seçeneği. İçin bilgi girin **adı**, **ana sınıf adı** kaydetmek için ardından yerel çalıştırma düğmesine tıklayın.

### <a name="scenario-3-perform-local-debugging"></a>Senaryo 3: Yerel hata ayıklama gerçekleştirin
1. Açık **SparkCore_wasbloTest** komut dosyası, kesme noktaları belirleyin.
1. Kod Düzenleyicisi'ni sağ tıklatın ve ardından seçeneğini **hata ayıklama ' [HDInsight üzerinde Spark] XXX'** yerel hata ayıklama gerçekleştirmek için.   



## <a name="learn-how-to-perform-remote-run-and-debugging"></a>Çalıştırma ve hata ayıklama uzak gerçekleştirme hakkında bilgi edinin
### <a name="scenario-1-perform-remote-run"></a>Senaryo 1: Uzak çalışma gerçekleştirme

1. Erişim için **yapılandırmalarını Düzenle** menüsünde, sağ üst köşede bulunan simgeyi seçin. Bu menüden oluşturabilir veya uzaktan hata ayıklama için yapılandırmaları düzenleyin.

   ![Yapılandırmaları düzenle](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

1. İçinde **Çalıştır/hata ayıklama yapılandırmaları** iletişim kutusunda, artı işaretini seçin (**+**). Ardından **HDInsight üzerinde Apache Spark** seçeneği.

   ![Yeni yapılandırma Ekle](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
1. Geçiş **uzaktan çalıştırmak kümesinde** sekmesi. İçin bilgi girin **adı**, **Spark kümesi**, ve **ana sınıf adı**. Ardından **Gelişmiş Yapılandırma (uzak hata ayıklama)**. Sağladığımız araçları ile hata ayıklama desteği **yürütücüler**. **NumExectors**, varsayılan değer 5'tir. Size daha iyi 3'ten daha yüksek ayarlamazsınız.

   ![Hata ayıklama yapılandırmaları Çalıştır](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

1. İçinde **Gelişmiş bir yapılandırma (uzak hata ayıklama)** bölümü, select **etkinleştirme Spark uzaktan hata ayıklama**. SSH kullanıcı adı girin ve bir parola girin veya bir özel anahtar dosyasını kullanın. Uzaktan hata ayıklama gerçekleştirmek istiyorsanız, ayarlamanız gerekir. Uzak çalıştırmaya kullanmak istiyorsanız bunu ayarlamak için gerek yoktur.

   ![Spark ile uzaktan hata ayıklama etkinleştirme](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

1. Yapılandırma artık sağladığınız adla kaydedilir. Yapılandırma ayrıntılarını görüntülemek için yapılandırma adı seçin. Değişiklik yapmak için seçin **yapılandırmalarını Düzenle**. 

1. Yapılandırma ayarları tamamladıktan sonra projeyi uzak kümede çalıştırmak veya uzaktan hata ayıklama gerçekleştirin.
   
   ![Uzaktan Çalıştır düğmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run.png)

1. Tıklayın **Bağlantıyı Kes** gönderimi günlükleri, sol bölmede bulunan görünmüyor düğmesi. Ancak, arka uçta hala çalışıyor.

   ![Uzaktan Çalıştır düğmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/remote-run-result.png)



### <a name="scenario-2-perform-remote-debugging"></a>Senaryo 2: Uzaktan hata ayıklama gerçekleştirin
1. Kesme noktaları ayarlayın ve ardından **uzaktan hata ayıklama** simgesi. Uzak gönderme ile yapılandırılması için SSH kullanıcı adı/parola gereken farktır.

   ![Hata Ayıkla simgesi seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

1. Program yürütme kırılma noktasına ulaştığında, gördüğünüz bir **sürücü** sekmesi ve iki **Yürütücü** sekmelerinde **hata ayıklayıcı** bölmesi. Seçin **sürdürme Program** ardından sonraki kesme noktasına erişene kod çalıştırmaya devam etmek için simge. Doğru geçmeniz gerekirse **Yürütücü** hata ayıklamak için hedef Yürütücü bulmak için sekmesinde. Karşılık gelen üzerinde yürütme günlüklerini görüntüleyebilirsiniz **konsol** sekmesi.

   ![Hata ayıklama sekmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-3-perform-remote-debugging-and-bug-fixing"></a>Senaryo 3: Uzaktan hata ayıklama ve hata düzeltme gerçekleştirme
1. İki kesme noktaları ayarlayın ve ardından **hata ayıklama** uzaktan hata ayıklama işlemi başlatmak için simge.

1. İlk kesme noktasına kod durdurur ve parametre ve değişken bilgi gösterilir **değişkenleri** bölmesi. 

1. Seçin **sürdürme Program** devam etmek için simge. Kod, ikinci bir noktada durdurur. Beklendiği gibi özel durum yakalandı.

   ![Hata durum](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png) 

1. Seçin **sürdürme Program** yeniden simgesi. **HDInsight Spark gönderimi** penceresinde bir "iş çalıştırma başarısız" hatası görüntülenir.

   ![Hata gönderme](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png) 

1. Hata ayıklama özelliği Intellij kullanarak değişken değeri dinamik olarak güncelleştirmek için seçin **hata ayıklama** yeniden. **Değişkenleri** bölmesinde tekrar görünür. 

1. Hedef sağ **hata ayıklama** sekmesine tıklayın ve ardından **değer kümesi**. Ardından, değişken için yeni bir değer girin. Ardından **Enter** değeri kaydetmek için. 

   ![Değer ata](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value.png) 

1. Seçin **sürdürme Program** programı çalıştırmaya devam etmek için simge. Bu kez, hiçbir özel durum yakalandı. Proje özel durumlar başarıyla çalıştığını görebilirsiniz.

   ![Özel durum hata ayıklama](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Sonraki adımlar
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="demo"></a>Tanıtım
* Scala proje (video) oluşturun: [Apache Spark Scala uygulamaları oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [Apache Spark uygulamalarında uzaktan bir HDInsight kümesi üzerinde hata ayıklamak amacıyla Intellij için Azure Araç Seti'ni kullanma](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](../hdinsight-apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için Intellij için Azure Araç Seti'ni kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalar VPN üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçları kullanma](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
