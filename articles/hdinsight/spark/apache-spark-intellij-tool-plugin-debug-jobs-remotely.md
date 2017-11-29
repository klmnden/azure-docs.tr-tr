---
title: "Intellij için Azure Araç Seti: Hdınsight Spark uygulamalarında uzaktan hata ayıklama | Microsoft Docs"
description: "Bilgi nasıl Hdınsight araçları Azure Araç Seti Intellij için Hdınsight kümeleri VPN üzerinden çalışan Spark uygulamalarında uzaktan hata ayıklama için kullanın."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 4a87f1c6ba82edc0a762d9e02542a7756383ed82
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-debug-spark-applications-remotely-in-hdinsight-through-vpn"></a>Intellij için Azure Araç Seti uzaktan VPN aracılığıyla hdınsight'ta Spark uygulamalarında hata ayıklama için kullanın

Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama öneririz. Yönergeler için bkz: [SSH aracılığıyla Intellij için Hdınsight kümesinde Azure araç seti ile Spark uygulamalarında uzaktan hata ayıklama](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Bu makalede, Hdınsight araçları Azure Araç Seti Intellij için Hdınsight Spark kümesi üzerinde Spark iş göndermek ve masaüstü bilgisayarınızdan uzaktan debug için nasıl kullanılacağı hakkında adım adım yönergeler sağlar. Bu görevleri tamamlamak için aşağıdaki üst düzey adımları gerçekleştirmeniz gerekir:

1. Bir siteden siteye veya noktadan siteye Azure sanal ağı oluşturun. Bu belgede yer alan adımlar, siteden siteye ağ kullandığınızı varsayar.
2. Siteden siteye sanal ağın parçası olan Hdınsight'ta Spark kümesi oluşturun.
3. Küme baş düğümüne ve masaüstünüzü arasındaki bağlantıyı doğrulayın.
4. Intellij Idea Scala uygulaması oluşturma ve uzaktan hata ayıklama için yapılandırın.
5. Uygulamada hata ayıklama ve çalıştırın.

## <a name="prerequisites"></a>Ön koşullar
* **Bir Azure aboneliği**. Daha fazla bilgi için bkz: [Azure ücretsiz bir deneme sürümünü edinin](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Hdınsight'ta bir Apache Spark kümesi**. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).
* **Oracle Java Geliştirme Seti**. Şuradan yükleyebilirsiniz [Oracle Web sitesi](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* **Intellij Idea**. Bu makalede sürümünü 2017.1 kullanır. Şuradan yükleyebilirsiniz [JetBrains Web sitesi](https://www.jetbrains.com/idea/download/).
* **Azure Araç Seti Intellij için Hdınsight Araçları**. Intellij için Hdınsight araçları kullanılabilir Intellij için Azure araç seti bir parçası olarak. Azure araç setini yükleme hakkında yönergeler için bkz: [Intellij için Azure Araç Seti yüklemek](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation).
* **Oturum açmak için Azure aboneliğinizin Intellij Idea**. ' Ndaki yönergeleri izleyin [Spark Hdınsight kümesi için uygulamalar oluşturmak üzere Intellij için kullanım Azure Araç Seti](apache-spark-intellij-tool-plugin.md).
* **Özel durum geçici çözüm**. Bir Windows bilgisayarda uzaktan hata ayıklama için Spark Scala uygulaması çalıştırılırken bir özel durum alabilirsiniz. Bu özel durum açıklaması [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) ve Windows'da WinUtils.exe dosyası eksik nedeniyle oluşur. Bu hata olarak çözmek için size gereken [yürütülebilir dosya indirme](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) gibi bir konuma **C:\WinUtils\bin**. Ekleme bir **HADOOP_HOME** ortam değişkeni ve değişkenin değerini ayarlamak **C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>1. adım: Azure sanal ağı oluşturma
Bir Azure sanal ağı oluşturmak için aşağıdaki bağlantılardan birini yönergeleri izleyin ve masaüstü bilgisayar ve sanal ağ arasındaki bağlantıyı doğrulayın:

* [Azure portalını kullanarak siteden siteye VPN bağlantısı olan bir VNet oluşturma](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [PowerShell kullanarak siteden siteye VPN bağlantısı olan bir VNet oluşturma](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [PowerShell kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>2. adım: bir Hdınsight Spark kümesi oluşturma
Bir Apache Spark kümesi de oluşturduğunuz Azure sanal ağı parçası olan Azure Hdınsight'ta oluşturmanızı öneririz. Bulunan bilgileri kullanın [Hdınsight oluşturma Linux tabanlı kümelerde](../hdinsight-hadoop-provision-linux-clusters.md). İsteğe bağlı yapılandırma işleminin bir parçası olarak, önceki adımda oluşturduğunuz Azure sanal ağı seçin.

## <a name="step-3-verify-the-connectivity-between-the-cluster-head-node-and-your-desktop"></a>Adım 3: küme baş düğümüne ve masaüstünüzü arasındaki bağlantıyı doğrulama
1. Baş düğüm IP adresini alın. Ambari UI küme için açın. Küme dikey penceresinden seçin **Pano**.

    ![Ambari Pano seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/launch-ambari-ui.png)
2. Ambari Arabiriminden seçin **ana**.

    ![Ambari konakları seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/ambari-hosts.png)
3. Baş düğümler, çalışan düğümleri ve zookeeper düğümleri listesini bakın. Baş düğümler sahip bir **hn*** öneki. İlk baş düğüm seçin.

    ![Ambari baş düğüm bulunamadı](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/cluster-headnodes.png)
4. Gelen **Özet** bölmesi açılır ve sayfanın kopyalama sonundaki **IP adresi** baş düğümü ve **ana bilgisayar adı**.

    ![Ambari IP adresi Bul](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/headnode-ip-address.png)
5. IP adresi ve ana bilgisayar adını baş düğüm eklemek **ana** bilgisayarda çalıştırmak ve Spark iş uzaktan hata ayıklama için istediğiniz dosya. Bu IP adresi gibi ana bilgisayar adını kullanarak baş düğüm ile iletişim kurmanıza olanak sağlar.

   a. Yükseltilmiş izinleri olan bir not defteri dosyası açın. Gelen **dosya** menüsünde, select **açık**, ana bilgisayarlar dosyasının konumunu bulun. Bir Windows bilgisayarda konumdur **C:\Windows\System32\Drivers\etc\hosts**.

   b. Aşağıdaki bilgileri ekleyin **ana** dosyası:

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. Hdınsight küme tarafından kullanılan Azure sanal ağa bağlı bilgisayardan hostname yanı sıra IP adresini kullanarak baş düğümler ping atabildiğinizi doğrulayın.
7. SSH yönergeleri takip ederek küme baş düğümüne bağlanmak için kullandığınız [SSH kullanarak Hdınsight kümesine bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md). Küme baş düğümünden masaüstü bilgisayarın IP adresini ping işlemi yapın. Bilgisayara atanmış iki IP adresi bağlanabilirliği test edin:

    - Bir ağ bağlantısı
    - Azure sanal ağı için bir tane

8. Diğer baş düğüm için arasındaki adımları yineleyin.

## <a name="step-4-create-a-spark-scala-application-by-using-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>4. adım: Azure araç setindeki Intellij için Hdınsight araçları kullanarak Spark Scala uygulama oluşturma ve uzaktan hata ayıklama için yapılandırma
1. Intellij Idea açın ve yeni bir proje oluşturun. İçinde **yeni proje** iletişim kutusunda, aşağıdakileri yapın:

    ![Intellij Idea yeni proje şablonu seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-hdi-scala-app.png)

    a. Seçin **Hdınsight** > **(Scala) hdınsight'ta Spark**.

    b. Seçin **sonraki**.
2. Sonraki **yeni proje** iletişim kutusunda, aşağıdakileri yapın ve ardından **son**:

    - Bir proje adı ve konum girin.

    - İçinde **proje SDK** aşağı açılan listesinden, **Java 1.8** seçin veya Spark 2.x kümesi için **Java 1.7** Spark 1.x kümesi için.

    - İçinde **Spark sürüm** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK'sı ile Scala SDK'sı için uygun sürüm tümleştirir. Spark kümesi sürüm 2.0 eskiyse, seçin **1.x Spark**. Aksi takdirde seçin **Spark2.x**. Bu örnekte **Spark 2.0.2 (Scala 2.11.8)**.
  
   ![Proje SDK ve Spark sürüm seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-project-details.png)
  
3. Spark proje bir yapı sizin için otomatik olarak oluşturur. Yapı görüntülemek için aşağıdakileri yapın:

    a. Gelen **dosya** menüsünde, select **proje yapısını**.

    b. İçinde **proje yapısını** iletişim kutusunda **yapıları** oluşturulan varsayılan yapı görüntülemek için. Artı işaretini seçerek kendi yapı de oluşturabilirsiniz (**+**).

   ![JAR oluşturma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/default-artifact.png)


4. Kitaplıklarını projenize ekleyin. Kitaplığa eklemek için aşağıdakileri yapın:

    a. Proje ağacında proje adına sağ tıklayın ve ardından **açık modülü ayarları**. 

    b. İçinde **proje yapısını** iletişim kutusunda **kitaplıkları**seçin (**+**) sembol ve ardından **gelen Maven** .

    ![Bir kitaplığı ekleyin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/add-library.png)

    c. İçinde **Uzaktan Yükleme Kitaplığı Maven depodan** iletişim kutusuna, aramak ve aşağıdaki kitaplık ekleyin:

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Kopya `yarn-site.xml` ve `core-site.xml` kümesinden bir küme baş düğümü ve projeye ekleyin. Dosyaları kopyalamak için aşağıdaki komutları kullanın. Kullanabileceğiniz [Cygwin](https://cygwin.com/install.html) çalıştırmak için şunları `scp` kümeden dosyaları kopyalamak için komutları baş düğümler:

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Biz küme baş düğümüne IP adresi ve ana bilgisayar adları için masaüstünde hosts dosyasını zaten eklenmiş olduğundan, biz kullanabilirsiniz `scp` aşağıdaki şekilde komutlar:

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Bu dosyalar projenize eklemek için bunların altında kopyalama **/src** , proje ağacında, örneğin klasör `<your project directory>\src`.
6. Güncelleştirme `core-site.xml` dosya aşağıdaki değişiklikleri yapın:

   a. Şifrelenmiş anahtar değiştirin. `core-site.xml` Dosyası, şifrelenmiş anahtar kümesi ile ilişkili depolama hesabı içerir. İçinde `core-site.xml` varsayılan depolama hesabıyla ilişkili depolamanın anahtarla şifrelenmiş anahtar Değiştir projeye eklenen dosya. Daha fazla bilgi için bkz: [depolama erişim tuşlarınızı yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   b. Aşağıdaki girişlerden kaldırmak `core-site.xml`:

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   c. Dosyayı kaydedin.
7. Uygulamanız için ana sınıf ekleyin. Gelen **Proje Gezgini**, sağ **src**, işaret **yeni**ve ardından **Scala sınıfı**.

    ![Ana sınıf seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code.png)
8. İçinde **yeni Scala Sınıf Oluştur** iletişim kutusunda, bir ad sağlayın, seçin **nesne** içinde **türü** kutusuna ve ardından **Tamam**.

    ![Yeni Scala sınıfı oluşturma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code-object.png)
9. İçinde `MyClusterAppMain.scala` dosya, aşağıdaki kodu yapıştırın. Bu kod Spark bağlamını ve açılır oluşturur bir `executeJob` yönteminden `SparkSample` nesnesi.

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. 8 ve adlı yeni bir Scala nesne eklemek için 9. adımları tekrarlayarak `*SparkSample`. Bu sınıfa aşağıdaki kodu ekleyin. Bu kod (tüm Hdınsight Spark kümeleri kullanılabilir) HVAC.csv verileri okur. Yalnızca bir rakam CSV dosyasındaki yedinci sütunundaki satırları alır ve çıktıyı Yazar **/HVACOut** kümenin varsayılan depolama kapsayıcısını altında.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find the rows which have only one digit in the 7th column in the CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. Adım 8 ve 9 yeni eklemek için sınıfı adı verilen yineleyin `RemoteClusterDebugging`. Bu sınıf uygulamalarında hata ayıklamak için kullanılan Spark test çerçevesi uygular. Aşağıdaki kodu ekleyin `RemoteClusterDebugging` sınıfı:

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     Birkaç dikkat edilecek önemli noktalar şunlardır:

      * İçin `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, Spark derleme JAR belirtilen yolda küme depolama biriminde kullanılabilir olduğundan emin olun.
      * İçin `setJars`, yapı JAR oluşturulduğu konumu belirtin. Genellikle,. `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. İçinde`*RemoteClusterDebugging` sınıfı, sağ `test` anahtar sözcüğü ve ardından **oluşturma RemoteClusterDebugging yapılandırma**.

    ![Bir uzak yapılandırması oluştur](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-remote-config.png)

13. İçinde **oluşturma RemoteClusterDebugging yapılandırma** iletişim kutusu, yapılandırma için bir ad sağlayın ve ardından **Test türü** olarak **Test adı**. Diğer tüm değerler varsayılan ayarları bırakın. Seçin **Uygula**ve ardından **Tamam**.

    ![Yapılandırma ayrıntılarını Ekle](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/provide-config-value.png)
14. Artık görmelisiniz bir **uzaktan çalıştırmak** menü çubuğundaki açılan listede yapılandırmasını.

    ![Uzaktan çalışma aşağı açılan liste](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>5. adım: uygulamayı hata ayıklama modunda çalıştırın.
1. Intellij Idea projenizi açın `SparkSample.scala` ve bir kesme noktası yanına oluşturma `val rdd1`. İçinde **kesme noktası oluşturmak için** açılır menüsünde, select **işlevi executeJob satırında**.

    ![Kesme noktası ekleme](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-breakpoint.png)
2. Uygulamayı çalıştırmak için seçin **hata ayıklama çalıştırmak** düğmesine **uzaktan çalıştırmak** yapılandırma aşağı açılan listesi.

    ![Hata ayıklama Çalıştır düğmesini seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-run-mode.png)
3. Program yürütme kesme noktasına ulaştığında, gördüğünüz bir **hata ayıklayıcı** alt bölmedeki sekmesi.

    ![Hata ayıklayıcı sekmesini görüntüleyin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch.png)
4. Bir izleme eklemek için seçin (**+**) simgesi.

    ![Seçin + simgesi](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable.png)

    Bu örnekte, uygulamanın önce değişkeni ihlal `rdd1` oluşturuldu. Bu izleme kullanarak ilk beş satırları değişkeninde görebiliriz `rdd`. Seçin **girin**.

    ![Hata ayıklama modunda programı çalıştırın](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable-value.png)

    Çalışma zamanında, veri ve hata ayıklama terabayt sorgu önceki resimde gördüğünüz olduğunu nasıl uygulama ilerler. Örneğin, önceki görüntüde gösterildiği çıktıda çıkış ilk satırın üstbilgi olduğunu görebilirsiniz. Bu Çıkışta bağlı olarak, uygulama kodunuzun gerekiyorsa, üstbilgi satırını atlar değiştirebilirsiniz.
5. Artık seçebilirsiniz **Sürdür Program** çalıştırmak uygulamanız ile devam etmek için simge.

    ![Resume programı seçin.](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-continue-run.png)
6. Uygulama başarıyla tamamlanırsa, aşağıdakine benzer bir çıktı görmeniz gerekir:

    ![Konsol çıktısı](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-complete.png)

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
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Hdınsight araçları Azure araç setini Eclipse için Spark uygulamaları oluşturmak için kullanın](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Hdınsight'ta Spark kümesinde ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [Hdınsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Hdınsight'ta bir Apache Spark kümesinde çalışan izleme ve hata ayıklama işleri](apache-spark-job-debugging.md)
