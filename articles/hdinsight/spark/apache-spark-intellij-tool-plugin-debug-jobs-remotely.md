---
title: 'Intellij için Azure Araç Seti: HDInsight Spark uygulamalarında uzaktan hata ayıklama '
description: Bilgi nasıl HDInsight araçları Intellij için Azure Araç Seti VPN aracılığıyla HDInsight kümelerinde çalıştırma Spark uygulamalarında uzaktan hata ayıklamak için kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.openlocfilehash: 30d52f1ac6a68a3202de59a0b4cab8edfb7ed042
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124347"
---
# <a name="use-azure-toolkit-for-intellij-to-debug-apache-spark-applications-remotely-in-hdinsight-through-vpn"></a>Apache Spark uygulamalarında uzaktan HDInsight VPN aracılığıyla hata ayıklama Intellij için Azure Araç Seti'ni kullanma

Hata ayıklama öneririz [Apache Spark](https://spark.apache.org/) SSH üzerinden uzaktan uygulamalar. Yönergeler için [SSH üzerinden Intellij için Azure araç seti ile bir HDInsight kümesi üzerinde Apache Spark uygulamaları uzaktan hata ayıklama](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Bu makalede bir HDInsight Spark kümesi üzerinde bir Spark işi göndermek ve masaüstü bilgisayardan uzaktan ayıklama için Intellij için Azure araç seti, HDInsight araçları kullanma hakkında adım adım rehberlik sunar. Bu görevleri tamamlamak için aşağıdaki üst düzey adımları gerçekleştirmeniz gerekir:

1. Bir siteden siteye veya noktadan siteye Azure sanal ağı oluşturun. Bu belgede yer alan adımlar, siteden siteye ağ kullandığınızı varsayar.
1. Siteden siteye sanal ağın parçası olan HDInsight Spark kümesi oluşturma.
1. Küme baş düğümü ve Masaüstü arasındaki bağlantıyı doğrulayın.
1. Intellij Idea Scala uygulama oluşturun ve uzaktan hata ayıklama için yapılandırın.
1. Çalıştırın ve uygulamanın hatasını ayıklama.

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Daha fazla bilgi için [Azure ücretsiz deneme](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **HDInsight, Apache Spark kümesi**. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* **Oracle Java development Kit'i**. Buradan yükleyebilirsiniz [Oracle Web sitesi](https://aka.ms/azure-jdks).
* **IntelliJ IDEA**. Bu makalede, sürüm 2017.1 kullanır. Buradan yükleyebilirsiniz [JetBrains Web sitesi](https://www.jetbrains.com/idea/download/).
* **Intellij için Azure Araç Seti HDInsight Araçları**. Intellij için HDInsight araçları kullanılabilir Intellij için Azure Araç Seti parçası olarak. Azure araç setini yükleme yönergeleri için bkz: [Intellij için Azure Araç Seti'ni yükleme](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation).
* **Azure aboneliğinizde oturum Intellij Idea '**. Bölümündeki yönergeleri [bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak Intellij için Azure Araç Seti'ni kullanma](apache-spark-intellij-tool-plugin.md).
* **Özel durum geçici çözüm**. Bir Windows bilgisayarda uzaktan hata ayıklama Scala Spark uygulaması çalıştırılırken özel durum alabilir. Bu özel durumun açıklaması [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) ve Windows eksik WinUtils.exe dosyasında nedeniyle oluşur. Bu hatayı çözmek için şunları yapmanız gerekir [yürütülebilir dosyayı indir](https://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) gibi bir konuma **C:\WinUtils\bin**. Ekleme bir **HADOOP_HOME** ortam değişkeni ve değeri bir değişkene ayarlayın **C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>1. Adım: Bir Azure sanal ağı oluşturma
Bir Azure sanal ağı oluşturmak için aşağıdaki bağlantılardan yönergeleri izleyin ve ardından, masaüstü bilgisayarınızda ve sanal ağ arasındaki bağlantıyı doğrulayın:

* [Azure portalını kullanarak siteden siteye VPN bağlantısı bir VNet oluşturma](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [PowerShell kullanarak siteden siteye VPN bağlantısı bir VNet oluşturma](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [PowerShell kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>2. Adım: HDInsight Spark kümesi oluşturma
Ayrıca, oluşturduğunuz Azure sanal ağının parçası olan Azure HDInsight Apache Spark kümesi, oluşturmanızı öneririz. Bulunan bilgileri kullanın [oluşturma Linux tabanlı HDInsight kümelerinde](../hdinsight-hadoop-provision-linux-clusters.md). İsteğe bağlı yapılandırma işleminin bir parçası olarak, önceki adımda oluşturduğunuz Azure sanal ağı seçin.

## <a name="step-3-verify-the-connectivity-between-the-cluster-head-node-and-your-desktop"></a>3. Adım: Küme baş düğümü ve Masaüstü arasındaki bağlantıyı doğrulayın
1. Baş düğümün IP adresini alın. Ambari UI, küme için açın. Küme dikey penceresinden seçin **Pano**.

    ![Ambari panoyu seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/launch-ambari-ui.png)
1. Ambari Arabiriminden seçin **konakları**.

    ![Ambari konakları seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/ambari-hosts.png)
1. Baş düğümler, çalışan düğümlerine ve zookeeper düğümleri listesini görürsünüz. Baş düğüme sahip bir **hn*** öneki. İlk baş düğümü seçin.

    ![Ambari baş düğümü bulun](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/cluster-headnodes.png)
1. Gelen **özeti** altındaki açılır ve sayfanın kopyalama bölmesinde **IP adresi** baş düğümün ve **Hostname**.

    ![Ambari IP adresi Bul](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/headnode-ip-address.png)
1. IP adresi ve ana bilgisayar adını baş düğümüne ekleyin **konakları** bilgisayarda çalıştırmak ve Spark işi uzaktan hata ayıklamak için istediğiniz dosya. Bu ana bilgisayar adının yanı sıra IP adresini kullanarak baş düğüm ile iletişim kurmanıza olanak sağlar.

   a. Yükseltilmiş izinlerle bir not defteri dosyası açın. Gelen **dosya** menüsünde **açık**ve ardından konakları dosyasının konumunu bulun. Bir Windows bilgisayarda konumdur **C:\Windows\System32\Drivers\etc\hosts**.

   b. Aşağıdaki bilgileri ekleyin **konakları** dosyası:

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
1. HDInsight kümesi tarafından kullanılan Azure sanal ağına bağlı bilgisayardan, ana bilgisayar adının yanı sıra IP adresini kullanarak baş düğümlerine ping atabildiğinizi doğrulayın.
1. ' Ndaki yönergeleri takip ederek küme baş düğümüne bağlanmak için SSH kullanın [SSH kullanarak HDInsight kümesine bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md). Küme baş düğümünden masaüstü bilgisayarın IP adresine ping komutu. Bilgisayara atanır. iki IP adresini bağlantıyı test edin:

    - Bir ağ bağlantısı
    - Bir Azure sanal ağı

1. Adımları diğer baş düğüm için yineleyin.

## <a name="step-4-create-an-apache-spark-scala-application-by-using-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>4. Adım: Azure araç takımı Intellij için HDInsight araçlarını kullanarak Apache Spark Scala uygulama oluşturma ve uzaktan hata ayıklama için yapılandırma
1. Intellij Idea'ı açın ve yeni bir proje oluşturun. **Yeni Proje** iletişim kutusunda aşağıdakileri yapın:

    ![Intellij Idea içinde yeni proje şablonu seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-hdi-scala-app.png)

    a. **HDInsight** > **HDInsight’ta Spark (Scala)** seçeneğini belirleyin.

    b. **İleri**’yi seçin.
1. Sonraki **yeni proje** iletişim kutusunda, aşağıdakileri yapın ve ardından **son**:

    - Bir proje adı ve konum girin.

    - **Proje SDK’sı** açılır listesinde, Spark 2.x kümesi için **Java 1.8**’i seçin veya Spark 1.x kümesi için **Java 1.7**’yi seçin.

    - İçinde **Spark sürümü** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK'sı ve Scala SDK'sı için doğru sürüm tümleştirilir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, **Spark 2.0.2 (Scala 2.11.8)** kullanır.
  
   ![Proje SDK'sı ve Spark sürümünü seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-project-details.png)
  
1. Spark projesi bir yapıt sizin için otomatik olarak oluşturur. Yapıtı görüntülemek için aşağıdakileri yapın:

    a. **Dosya** menüsünden **Proje Yapısı**’nı seçin.

    b. İçinde **Proje yapısı** iletişim kutusunda **Yapıtları** oluşturulan varsayılan yapıtı görüntülemek için. Artı işaretini seçerek kendi yapıt oluşturabilirsiniz (**+**).

   ![JAR oluşturma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/default-artifact.png)


1. Kitaplıklarını projenize ekleyin. Bir kitaplığı eklemek için aşağıdakileri yapın:

    a. Proje ağacında proje adına sağ tıklayın ve ardından **modül ayarlarını Aç**. 

    b. İçinde **Proje yapısı** iletişim kutusunda **kitaplıkları**seçin (**+**) simgesi ve ardından **gelen Maven** .

    ![Bir kitaplığı Ekle](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/add-library.png)

    c. İçinde **indirme kitaplığı Maven deposundan** iletişim kutusunda, aramak ve aşağıdaki kitaplıklarını ekleyin:

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
1. Kopyalama `yarn-site.xml` ve `core-site.xml` kümeden baş düğüm ve bunları projeye ekleyin. Dosyaları kopyalamak için aşağıdaki komutları kullanın. Kullanabileceğiniz [Cygwin](https://cygwin.com/install.html) çalıştırmak için şunları `scp` kümeden dosyaları kopyalamak için komutları baş düğümlerine:

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Biz küme baş düğümüne IP adresi ve ana bilgisayar adları için masaüstünde hosts dosyasını zaten eklenmiş olduğundan, kullanabiliriz `scp` aşağıdaki şekilde komutları:

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Bu dosyaları projenize eklemek için bunları altında kopyalayın **/src** , proje ağacında, örneğin klasör `<your project directory>\src`.
1. Güncelleştirme `core-site.xml` dosyasını aşağıdaki değişiklikleri yapın:

   a. Şifrelenmiş anahtarı değiştirin. `core-site.xml` Dosya şifrelenmiş anahtar kümeyle ilişkili depolama hesabına içerir. İçinde `core-site.xml` şifrelenmiş anahtarı varsayılan depolama hesabı ile ilişkili gerçek depolama anahtarı ile değiştirin, projeye eklenen dosya. Daha fazla bilgi için [depolama erişim anahtarlarınızı yönetme](../../storage/common/storage-account-manage.md#access-keys).

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
1. Uygulamanız için ana sınıf ekleyin. Gelen **Proje Gezgini**, sağ **src**, işaret **yeni**ve ardından **Scala sınıfı**.

    ![Ana sınıfı seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code.png)
1. İçinde **yeni Scala Sınıf Oluştur** iletişim kutusunda, bir ad belirtin, seçin **nesne** içinde **tür** kutusuna ve ardından **Tamam**.

    ![Yeni Scala sınıfı oluşturma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code-object.png)
1. İçinde `MyClusterAppMain.scala` dosyasında, aşağıdaki kodu yapıştırın. Bu kod Spark bağlamını ve açılır oluşturur bir `executeJob` yönteminden `SparkSample` nesne.

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

1. 8. ve 9 adlı yeni bir Scala nesne eklemek için adımları yineleyin `*SparkSample`. Bu sınıfa aşağıdaki kodu ekleyin. Bu kod (tüm HDInsight Spark kümeleri, kullanılabilir) HVAC.csv verileri okur. Yalnızca CSV dosyasındaki yedinci sütunda bir basamak içeren satırları alır ve ardından çıktıyı Yazar **/HVACOut** kümenin varsayılan depolama kapsayıcısını altında.

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
1. Adım 8 ve 9 yeni bir sınıf adlı yineleyin `RemoteClusterDebugging`. Bu sınıf, uygulamalarında hata ayıklamak için kullanılan Spark test çerçevesi uygular. Aşağıdaki kodu ekleyin `RemoteClusterDebugging` sınıfı:

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

     Birkaç dikkat edilecek önemli noktalar vardır:

      * İçin `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, Spark derleme JAR küme depolama alanına ve belirtilen yolda kullanılabilir olduğundan emin olun.
      * İçin `setJars`, yapıt JAR oluşturulduğu konumu belirtin. Genellikle, olan `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
1. İçinde`*RemoteClusterDebugging` sınıfı, sağ `test` anahtar sözcüğü ve ardından **oluşturma RemoteClusterDebugging yapılandırma**.

    ![Uzak bir yapılandırma oluşturun](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-remote-config.png)

1. İçinde **oluşturma RemoteClusterDebugging yapılandırma** iletişim kutusu, yapılandırma için bir ad belirtin ve ardından **Test türü** olarak **Test adı**. Diğer değerleri varsayılan ayarları bırakın. **Uygula**’yı ve sonra **Tamam**’ı seçin.

    ![Yapılandırma ayrıntılarını ekleme](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/provide-config-value.png)
1. Görmelisiniz bir **uzak çalıştırmaya** yapılandırma menü çubuğundaki aşağı açılan listesi.

    ![Uzak çalışma aşağı açılan listesi](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>5. Adım: Uygulamayı hata ayıklama modunda çalıştırın.
1. Intellij Idea projenizi açın `SparkSample.scala` ve bir kesme noktası oluşturun `val rdd1`. İçinde **kesme noktası oluşturmak için** açılır menüsünde, select **işlevi executeJob satırında**.

    ![Kesme noktası ekleme](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-breakpoint.png)
1. Uygulamayı çalıştırmak için seçin **hata ayıklama çalıştırma** düğmesinin yanındaki **uzaktan çalıştırmak** yapılandırma açılan listesi.

    ![Hata ayıklama Çalıştır düğmesini seçin.](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-run-mode.png)
1. Program yürütme bir kesme noktasına ulaşıldığında, gördüğünüz bir **hata ayıklayıcı** alt bölmede sekme.

    ![Hata ayıklayıcı sekmesini görüntüleyin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch.png)
1. Bir izleme eklemek için seçin (**+**) simgesi.

    ![Seçin + simgesi](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable.png)

    Bu örnekte, uygulamanın değişkeni önce kesildi `rdd1` oluşturuldu. Bu izleme kullanarak ilk beş satır değişkeninde görebiliriz `rdd`. **Enter** tuşunu seçin.

    ![Programın hata ayıklama modunda çalıştırın.](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable-value.png)

    Çalışma zamanında, verileri ve hata ayıklama terabaytlarca sorgu önceki görüntüde gördüğünüz olduğunu nasıl, ilerlemektedir. Örneğin, önceki görüntüde verilen çıktısında çıkış ilk satırı üst bilgi olduğunu görebilirsiniz. Bu çıktı bağlı olarak, uygulama kodunuz, gerekirse, üst bilgi satırı atlamak için değiştirebilirsiniz.
1. Artık seçebilirsiniz **sürdürme Program** , uygulamanızla birlikte ilerlemek için simge.

    ![Sürdürme Program seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-continue-run.png)
1. Uygulama başarıyla tamamlanırsa şuna benzer bir çıktı görmeniz gerekir:

    ![Konsol çıktısı](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-complete.png)

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
* [Apache Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçları kullanma](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight, Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight'ın bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
