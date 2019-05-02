---
title: 'Eclipse için Azure Araç Seti: HDInsight Spark Scala uygulamaları oluşturma '
description: Spark Scala içinde yazılmış uygulamalar geliştirmek ve bunları doğrudan Eclipse IDE içinden bir HDInsight Spark kümesine göndermek için Eclipse için Azure Araç Seti HDInsight araçlarını kullanın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/30/2017
ms.author: hrasheed
ms.openlocfilehash: 1ae585322316a9c215fc32cc2f8ffba2f332ff61
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704860"
---
# <a name="use-azure-toolkit-for-eclipse-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Seti'ni kullanma

HDInsight araçları için Azure Araç Seti'ni kullanma [Eclipse](https://www.eclipse.org/) geliştirmek için [Apache Spark](https://spark.apache.org/) yazılan uygulamalar [Scala](https://www.scala-lang.org/) ve bunları bir Azure HDInsight Spark kümesine gönderin doğrudan Eclipse'teki IDE. HDInsight araçları eklentisi birkaç farklı yolla kullanabilirsiniz:

* Geliştirme ve bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulaması göndermek için.
* Azure HDInsight Spark kümesi kaynaklarınıza erişmek için.
* Geliştirmek ve yerel olarak Scala Spark uygulamasını çalıştırmak için.

> [!IMPORTANT]  
> Oluşturma ve uygulamalar yalnızca Linux'ta bir HDInsight Spark kümesine göndermek için bu aracı kullanabilirsiniz.
> 
> 

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Eclipse IDE çalışma zamanı için kullanılan oracle Java Development Kit 8 sürümünü. Buradan indirebileceğiniz [Oracle Web sitesi](https://aka.ms/azure-jdks).
* Eclipse IDE. Bu makalede, Eclipse Neon kullanılır. Buradan yükleyebilirsiniz [Eclipse Web sitesi](https://www.eclipse.org/downloads/).



## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-the-scala-plug-in"></a>Eclipse ve Scala eklentisi için Azure araç setindeki HDInsight araçlarını yükleme

### <a name="install-azure-toolkit-for-eclipse"></a>Eclipse için Azure araç setini yükleme
Eclipse için HDInsight araçları, Eclipse için Azure Araç Seti parçası olarak. Yükleme yönergeleri için bkz. [Eclipse için Azure Araç Seti'ni yükleme](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-installation).

### <a name="install-the-scala-plug-in"></a>Scala eklentisini yükleme
Eclipse açtığınızda, HDInsight Aracı eklentisi Scala yüklü olup olmadığını otomatik olarak algılar. Seçin **Tamam** devam etmek ve Eclipse marketten eklentiyi yüklemek için yönergeleri izleyin.

![Eklenti Scala otomatik olarak yüklenmesini](./media/apache-spark-eclipse-tool-plugin/auto-install-scala.png)

Kullanıcı şunları yapabilir ya da [Azure aboneliği için oturum açın](#sign-in-to-your-azure-subscription), veya [bir HDInsight kümesini bağlarsınız](#link-a-cluster) Ambari kullanarak kullanıcı adı/parola veya etki alanına katılmış başlatmak için kimlik bilgisi. 

## <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın
1. Eclipse IDE'yi Başlat ve Azure Gezgini açın. Üzerinde **penceresi** menüsünde **görünümü göster**ve ardından **diğer**. Açılan iletişim kutusunda Genişlet **Azure**seçin **Azure Gezgini**ve ardından **Tamam**.

   ![Görünümü Göster iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/view-explorer-1.png)
1. Sağ **Azure** düğümüne tıklayın ve ardından **oturum**.
1. İçinde **Azure oturum açma** iletişim kutusunda, kimlik doğrulama yöntemini seçin, **oturum**, Azure kimlik bilgilerinizi girin.
   
   ![Azure oturum açma iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/view-explorer-2.png)
1. Oturum açtıktan sonra **abonelikleri seçin** iletişim kutusunda, kimlik bilgileri ile ilişkilendirilen tüm Azure abonelikleri listelenir. Tıklayın **seçin** iletişim kutusunu kapatın.

   ![Abonelikleri iletişim kutusunu seçin](./media/apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
1. Üzerinde **Azure Gezgini** sekmesinde, genişletme **HDInsight** HDInsight Spark kümelerinde aboneliğiniz kapsamındaki görmek için.
   
   ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-eclipse-tool-plugin/view-explorer-3.png)
1. Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görmek için bir küme adı düğümü daha da genişletebilirsiniz.
   
   ![Kaynakları görmek için küme adını genişleterek](./media/apache-spark-eclipse-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Küme bağlantı
Ambari yönetilen kullanıcı adı kullanarak, normal bir küme bağlayabilirsiniz. Benzer şekilde, bir etki alanına katılmış HDInsight kümesi için etki alanı ve kullanıcı adı, aşağıdaki gibi kullanarak bağlayabilirsiniz user1@contoso.com.

1. Seçin **küme bağlantı** gelen **Azure Gezgini**.

   ![bağlantı kümesi bağlam menüsü](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

1. Girin **küme adı**, **kullanıcı adı** ve **parola**, ardından kümesini bağlamak için Tamam düğmesine tıklayın. İsteğe bağlı olarak, depolama hesabı, depolama anahtarını girin ve sonra soldaki ağaç görünümünde çalışmak Depolama Gezgini'ni depolama kapsayıcısı seçin
   
   ![bağlantı kümesi iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/link-cluster-dialog.png)
   
   > [!NOTE]  
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlantılı depolama anahtarı, kullanıcı adı ve parola kullanın.
   > ![Depolama Gezgini'nde Eclipse](./media/apache-spark-eclipse-tool-plugin/storage-explorer-in-Eclipse.png)

1. Bağlı küme gördüğünüz **HDInsight** giriş bilgileri doğru olması durumunda Tamam düğmesine tıklandıktan sonra düğüm. Artık bağlantılı bu kümeye bir uygulama gönderebilir.

   ![bağlı küme](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

1. Ayrıca, bir kümeden kesebilir **Azure Gezgini**.
   
   ![bağlantısız küme](./media/apache-spark-intellij-tool-plugin/unlink.png)


## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Spark Scala proje için bir HDInsight Spark kümesi ayarlama

1. Eclipse IDE çalışma alanında **dosya**seçin **yeni**ve ardından **proje**. 
1. Yeni Proje Sihirbazı'nda genişletin **HDInsight**seçin **(Scala) HDInsight üzerinde Spark**ve ardından **sonraki**.

   ![Proje (Scala) HDInsight üzerinde Spark'ı seçme](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
1. Scala Proje Oluşturma Sihirbazı'nı, Scala eklentisi yüklü olup olmadığını otomatik olarak algılar. Seçin **Tamam** eklenti Scala indirilmeye devam ve Eclipse'i yeniden başlatmanız yönergeleri izleyin.

   ![Scala denetimi](./media/apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
1. İçinde **yeni HDInsight Scala proje** iletişim kutusunda aşağıdaki değerleri sağlayın ve ardından **sonraki**:
   * Proje için bir ad girin.
   * İçinde **JRE** alanında olduğundan emin olun **JRE yürütme ortamı kullanmak** ayarlanır **JavaSE 1.7** veya üzeri.
   * İçinde **Spark Kitaplığı** alanında seçebilirsiniz **kullanım Spark SDK'sını yapılandırmak için Maven** seçeneği.  Aracımızı Spark SDK'sı ve Scala SDK'sı için doğru sürüm tümleştirir. Ayrıca seçebilirsiniz **Spark SDK el ile eklemeniz** seçeneği, indirin ve Spark SDK'sı tarafından el ile ekleyin.

   ![Yeni HDInsight Scala proje iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
1. Sonraki iletişim kutusunda seçin **son**. 
   
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Scala uygulaması için bir HDInsight Spark kümesi oluşturma

1. Eclipse IDE'yi paket Gezgini'nde, daha önce oluşturduğunuz proje genişletin, sağ **src**, işaret **yeni**ve ardından **diğer**.
1. İçinde **sihirbaz seçin** iletişim kutusunda **Scala sihirbazları**seçin **Scala nesne**ve ardından **sonraki**.
   
   ![Bir sihirbaz iletişim kutusu seçin](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
1. İçinde **yeni dosya oluştur** iletişim kutusunda, nesne için bir ad girin ve ardından **son**.
   
   ![Yeni dosya iletişim kutusu oluşturma](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
1. Metin Düzenleyicisi'nde aşağıdaki kodu yapıştırın:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
1. Bir HDInsight Spark kümesinde uygulamayı çalıştırın:
   
   a. Paket Gezgini'nde proje adına sağ tıklayın ve ardından **gönderin, HDInsight için Spark uygulaması**.        
   b. İçinde **Spark gönderimi** iletişim kutusunda aşağıdaki değerleri sağlayın ve ardından **Gönder**:
      
   * İçin **küme adı**, uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.
   * Eclipse projeden bir yapı seçin veya bir sabit sürücüye seçin. Varsayılan değer, paketi Gezgini'nden sağ öğesi bağlıdır.
   * İçinde **ana sınıf adı** aşağı açılan listesinde, Gönderme Sihirbazı'nı projenizden tüm nesne adlarını görüntüler. Çalıştırmak istediğiniz girin veya seçin. Bir sabit diskten bir yapıt seçtiyseniz, ana sınıf adını el ile girmeniz gerekir. 
   * Bu örnek uygulama kodunda herhangi bir komut satırı bağımsız değişkenleri gerektirmez veya jar dosyaları dışındaki veya dosyalara başvurmak için diğer metin kutuları boş bırakabilirsiniz.
        
     ![Spark gönderim iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
1. **Spark gönderimi** sekmesinde ilerleme durumunu görüntüleme başlamalıdır. Kırmızı düğmeye seçerek uygulama durdurabilirsiniz **Spark gönderimi** penceresi. Ayrıca, bu belirli uygulama (mavi kutu görüntü olarak gösterilir) dünyanın dört bir yanındaki simgeyi seçerek çalıştırmak için günlükleri görüntüleyebilirsiniz.
      
   ![Spark gönderimi penceresi](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)


## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Erişim ve Azure Araç Seti Eclipse için HDInsight araçlarını kullanarak HDInsight Spark kümeleri yönetme
İş çıktısı erişim dahil olmak üzere, HDInsight araçları kullanarak çeşitli işlemler gerçekleştirebilirsiniz.

### <a name="access-the-job-view"></a>İş görünümü erişim
1. Azure Gezgini'nde **HDInsight**Spark küme adını genişletin ve ardından **işleri**. 

   ![İş görünümü düğümü](./media/apache-spark-eclipse-tool-plugin/job-view-node.png)

1. Seçin **işleri** düğümü. Java sürümü daha düşükse **1.8**, HDInsight araçları otomatik olarak yüklemeniz anımsatıcı **E (fx) clipse** eklenti. Seçin **Tamam** devam etmek ve ardından Eclipse Market'ten yüklemeniz ve Eclipse yeniden başlatmak için sihirbazı izleyin. 

   ![E (fx) clipse yükleyin](./media/apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

1. İş görünümünden **işleri** düğümü. Sağ bölmede, **Spark iş görünümü** sekmesi, küme üzerinde çalıştırılan tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

   ![Uygulama ayrıntıları](./media/apache-spark-eclipse-tool-plugin/view-job-logs.png)

   Ardından bu eylemlerden birini gerçekleştirebilirsiniz:

   * İş grafiğinin gelin. Çalışan iş ile ilgili temel bilgileri gösterir. İş Grafiği'ni seçin ve aşamaları ve her işin oluşturduğu bilgileri görebilirsiniz.

     ![İş aşaması ayrıntıları](./media/apache-spark-eclipse-tool-plugin/Job-graph-stage-info.png)

   * Seçin **günlük** günlükleri de dahil olmak üzere, sık görüntülemek için sekmesinde kullanılan **sürücü Stderr**, **sürücü Stdout**, ve **dizin bilgisi**.

     ![Günlük Ayrıntıları](./media/apache-spark-eclipse-tool-plugin/Job-log-info.png)

   * Spark geçmiş UI ve Apache Hadoop YARN kullanıcı Arabiriminde (uygulama düzeyinde) pencerenin üst kısmındaki köprüler seçerek açın.

### <a name="access-the-storage-container-for-the-cluster"></a>Küme için depolama kapsayıcısını erişim
1. Azure Gezgini'nde **HDInsight** kök düğümü kullanılabilir olan bir HDInsight Spark kümeleri listesini görmek için.
1. Depolama hesabı ve kümenin varsayılan depolama kapsayıcısını görmek için küme adını genişletin.
   
   ![Depolama hesabı ve varsayılan depolama kapsayıcısı](./media/apache-spark-eclipse-tool-plugin/view-explorer-5.png)
1. Kümeyle ilişkili depolama kapsayıcısı adı seçin. Sağ bölmede **HVACOut** klasör. Birini açın **bölümü -** uygulama çıktısını görmek için dosyalara.

### <a name="access-the-spark-history-server"></a>Erişim Spark geçmiş sunucusu
1. Azure Gezgini Spark küme adınızı sağ tıklayın ve ardından **açık Spark geçmiş UI**. İstendiğinde, küme için yönetici kimlik bilgilerini girin. Bu küme hazırlama sırasında belirttiğiniz.
1. Spark geçmiş sunucusu Panoda, uygulama adı uygulamanın yalnızca çalışması sona aramak için kullanın. Önceki kodda, uygulama adı kullanarak ayarladığınız `val conf = new SparkConf().setAppName("MyClusterApp")`. Bu nedenle, Spark uygulamanızın adı olan **MyClusterApp**.

### <a name="start-the-apache-ambari-portal"></a>Apache Ambari portalını başlatma
1. Azure Gezgini Spark küme adınızı sağ tıklayın ve ardından **açık küme yönetim portalı (Ambari)**. 
1. İstendiğinde, küme için yönetici kimlik bilgilerini girin. Bu küme hazırlama sırasında belirttiğiniz.

### <a name="manage-azure-subscriptions"></a>Azure aboneliklerini yönetme
Varsayılan olarak, Eclipse için Azure Araç Seti aracında HDInsight Spark kümeleri, Azure aboneliklerinden gelen listeler. Gerekirse, kümeye erişmek istediğiniz abonelikleri belirtebilirsiniz. 

1. Azure Gezgini içinde sağ **Azure** kök düğümünü ve ardından **Aboneliklerini Yönet**. 
1. İletişim kutusunda, erişim ve ardından istemiyorsanız abonelik için onay kutularını temizleyin **Kapat**. Belirleyebilirsiniz **oturum kapatma** dışında Azure aboneliğinizde oturum açmak istiyorsanız.

## <a name="run-a-spark-scala-application-locally"></a>Spark Scala uygulamayı yerel olarak çalıştırma
Spark Scala uygulamaları yerel iş istasyonunuzda çalıştırmak için HDInsight araçları Eclipse için Azure Araç Seti kullanabilirsiniz. Genellikle, bu uygulamalar bir depolama kapsayıcısı gibi küme kaynaklarında erişim gerekmez ve çalıştırabilir ve bunları yerel olarak test edin.

### <a name="prerequisite"></a>Önkoşul
Bir Windows bilgisayarda yerel Scala Spark uygulaması çalışıyor olsa da açıklandığı gibi özel durum alabilir [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). Bu özel durum oluşur **WinUtils.exe** Windows içinde eksik. 

Bu hatayı çözmek için gerekli [yürütülebilir dosyayı indir](https://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) gibi bir konuma **C:\WinUtils\bin**ve ardından ortam değişkenini ekledikten **HADOOP_HOME** ve değerini ayarlama değişkene **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Yerel bir Scala Spark uygulamasını çalıştırma
1. Eclipse'i başlatın ve bir proje oluşturun. İçinde **yeni proje** iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **sonraki**.
   
   * Sol bölmede **HDInsight**’ı seçin.
   * Sağ bölmede seçin **Spark HDInsight yerel çalıştırma örneği (Scala)**.

   ![Yeni Proje iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
   
1. Proje ayrıntılarını sağlamak için önceki bölümdeki 3 ile 6 arasındaki adımları izleyin [kurulum için bir HDInsight Spark kümesinde bir Spark Scala projesi](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster).

1. Şablon örnek bir kod ekler (**LogQuery**) altında **src** bilgisayarınızda yerel olarak çalıştırabileceğiniz klasör.
   
   ![LogQuery konumu](./media/apache-spark-eclipse-tool-plugin/local-app.png)
   
1. Sağ **LogQuery** uygulama noktasına **Çalıştır**ve ardından **1 Scala uygulama**. Bu görünür gibi çıkış **konsol** sekmesinde:
   
   ![Spark uygulaması yerel çalıştırma sonucu](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="known-problems"></a>Bilinen sorunlar
Ne zaman bir küme bağlantı, ben depolama kimlik bilgileri vermenizi Öner.

![Etkileşimli oturum açma](./media/apache-spark-eclipse-tool-plugin/link-cluster-with-storage-credential-eclipse.png)

İşleri göndermek için iki mod vardır. Depolama kimlik bilgisi sağlanır, toplu iş modu işi göndermek için kullanılır. Aksi takdirde, etkileşimli mod kullanılır. Küme meşgul ise, aşağıdaki hata alabilirsiniz.

![Eclipse alma hatası ne zaman meşgul küme](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-upload.png)

![Eclipse alma hatası ne zaman meşgul küme](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-submit.png)

## <a name="feedback"></a>Geri Bildirim
Bir Geri bildiriminiz varsa veya bu aracı kullanırken diğer herhangi bir sorunla karşılaşırsanız, bize bir e-postası gönderin hdivstool@microsoft.com.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Oluşturma ve uygulamaları çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Oluşturmak ve Spark Scala uygulamaları göndermek amacıyla Intellij için Azure Araç Seti'ni kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalar VPN üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](../hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

