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
ms.date: 08/24/2017
ms.author: jejiang
ms.openlocfilehash: cebbe2a0e28d49c93d0ebf12cc04b3d201dcec97
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>SSH aracılığıyla Intellij için Hdınsight kümesinde Azure araç seti ile Spark uygulamalarında hata ayıklama

Bu makalede, Hdınsight araçları Azure Araç Seti Intellij için Hdınsight kümesi üzerinde uzaktan uygulamalarında hata ayıklamak için nasıl kullanılacağı hakkında adım adım yönergeler sağlar. Projenizin hatalarını ayıklamak için de görüntüleyebilirsiniz [Intellij için Hdınsight Spark hata ayıklama uygulamaları Azure araç seti ile](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.

**Önkoşullar**

* **Azure Araç Seti Intellij için Hdınsight Araçları**. Bu araç Intellij için Azure araç seti bir parçasıdır. Daha fazla bilgi için bkz: [Intellij için Azure Araç Seti yüklemek](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).
* **Intellij için Azure Araç Seti**. Bu araç seti, Spark Hdınsight kümesi için uygulamalar oluşturmak için kullanın. Daha fazla bilgi için ' ndaki yönergeleri izleyin [Spark Hdınsight kümesi için uygulamalar oluşturmak üzere Intellij için kullanım Azure Araç Seti](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **Hdınsight SSH hizmeti kullanıcı adı ve parola yönetimi ile**. Daha fazla bilgi için bkz: [SSH kullanarak Hdınsight (Hadoop) Bağlan](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) ve [Ambari erişmek için tünel SSH kullanma web kullanıcı Arabirimi, kaynak, iş, Oozie ve diğer web Uı'lar](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Spark Scala uygulama oluşturma ve uzaktan hata ayıklama için yapılandırma

1. Intellij Idea başlatın ve bir proje oluşturun. İçinde **yeni proje** iletişim kutusunda, aşağıdakileri yapın:

   a. Seçin **Hdınsight**. 

   b. Tercihinize bağlı bir Java veya Scala şablonu seçin. Aşağıdaki seçenekler arasından seçin:

      - **(Scala) hdınsight'ta Spark**

      - **(Java) hdınsight'ta Spark**

      - **Spark Hdınsight kümesi çalışma örneği (Scala)**

      Bu örnekte bir **Spark Hdınsight küme çalıştırmak örneği (Scala)** şablonu.

   c. İçinde **oluşturma aracını** listesinde, gereksiniminize göre aşağıdakilerden birini seçin:

      - **Maven**, Scala Proje Oluşturma Sihirbazı'nı desteği

      -  **SBT**, bağımlılıkları yönetme ve Scala projeyi oluşturmak için 

      ![Hata ayıklama projesi oluşturma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Seçin **sonraki**.     
 
3. Sonraki **yeni proje** penceresinde aşağıdakileri yapın:

   ![SDK Spark seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Bir proje adı ve proje konumu girin.

   b. İçinde **proje SDK** aşağı açılan listesinden, **Java 1.8** için **2.x Spark** seçin ya da küme **Java 1.7** için **1.x Spark** küme.

   c. İçinde **Spark sürüm** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK ve Scala SDK için doğru sürümü tümleştirir. Spark kümesi sürüm 2.0 eskiyse, seçin **1.x Spark**. Aksi takdirde seçin **2.x Spark.** Bu örnekte **Spark 2.0.2 (Scala 2.11.8)**.

   d. **Son**’u seçin.

4. Seçin **src** > **ana** > **scala** projede kodunuzu açmak için. Bu örnekte **SparkCore_wasbloTest** komut dosyası.

5. Erişim için **yapılandırmalarını Düzenle** menüsünde sağ üst köşedeki simgeyi seçin. Bu menüden oluşturabilir veya uzaktan hata ayıklama için yapılandırmaları düzenleyebilirsiniz.

   ![Yapılandırmaları Düzenle](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

6. İçinde **Çalıştır/hata ayıklama yapılandırmaları** iletişim kutusunda, artı işaretini seçin (**+**). Ardından **Spark işi Gönder** seçeneği.

   ![Yeni yapılandırma ekleyin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
7. İçin bilgileri girin **adı**, **Spark kümesi**, ve **ana sınıf adı**. Ardından **Gelişmiş Yapılandırma**. 

   ![Hata ayıklama yapılandırmaları çalıştırma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

8. İçinde **Spark gönderimi Gelişmiş Yapılandırma** iletişim kutusunda **etkinleştirmek Spark uzaktan hata ayıklama**. SSH kullanıcı adı girin ve ardından bir parola girin veya özel bir anahtar dosyası kullanın. Yapılandırmayı kaydetmek için seçin **Tamam**.

   ![Spark uzaktan hata ayıklama etkinleştirme](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

9. Yapılandırma, belirttiğiniz adla şimdi kaydedilir. Yapılandırma ayrıntılarını görüntülemek için yapılandırma adı seçin. Değişiklik yapmak için seçin **yapılandırmalarını Düzenle**. 

10. Yapılandırma ayarları tamamladıktan sonra projeyi uzak küme karşı çalıştırın veya uzaktan hata ayıklama gerçekleştirin.

## <a name="learn-how-to-perform-remote-debugging"></a>Uzaktan hata ayıklama gerçekleştirmek öğrenin
### <a name="scenario-1-perform-remote-run"></a>Senaryo 1: Uzak çalışma gerçekleştirme

Bu bölümde, sürücüler ve yürütücüler hata ayıklamak nasıl gösteriyoruz.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. Kesme noktaları ayarlayın ve ardından **hata ayıklama** simgesi.

   ![Hata ayıklama simgesini seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

2. Program yürütme kesme noktası ulaştığında, gördüğünüz bir **sürücü** sekmesi ve iki **Yürütücü** sekmelerinde **hata ayıklayıcı** bölmesi. Seçin **Sürdür Program** simgesini, ardından sonraki kesme ulaştığında ve karşılık gelen üzerinde odaklanır kodu çalıştırmaya devam etmek için **Yürütücü** sekmesi. Karşılık gelen üzerinde yürütme günlüklerini gözden geçirebilirsiniz **konsol** sekmesi.

   ![Hata ayıklama sekmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>Senaryo 2: uzaktan hata ayıklama ve hata düzeltme gerçekleştirme
Bu bölümde, değişken değeri için basit bir düzeltme Intellij hata ayıklama özelliği kullanılarak dinamik olarak güncelleştirmek nasıl gösteriyoruz. Aşağıdaki kod örneğinde, hedef dosya zaten var olduğundan özel durum oluşur.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a>Uzaktan hata ayıklama ve hata düzeltme gerçekleştirmek için
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
