---
title: 'Intellij için Azure Araç Seti: Spark HDInsight kümesi için uygulamalar oluşturma | Microsoft Docs'
description: Spark Scala içinde yazılmış uygulamalar geliştirmek için Intellij için Azure araç takımı kullanın ve bunları bir HDInsight Spark kümesine göndermek.
services: hdinsight
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/25/2017
ms.author: maxluk
ms.openlocfilehash: 891a568af7892048eb84646acbf495f32ddd00b2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427433"
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Spark uygulamaları için bir HDInsight kümesi oluşturmak için Intellij için Azure Araç Seti'ni kullanma

Spark Scala içinde yazılmış uygulamalar geliştirmek için eklenti Intellij için Azure araç takımı kullanın ve bunları doğrudan Intellij tümleşik geliştirme ortamından (IDE) bir HDInsight Spark kümesine göndermek. Birkaç yöntemle eklenti kullanabilirsiniz:

* Geliştirin ve bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulaması gönderin.
* Azure HDInsight Spark kümesi kaynaklarınıza erişin.
* Geliştirin ve yerel olarak Scala Spark uygulamasını çalıştırın.

Projenizi oluşturmak için görüntüleme [Intellij için Azure araç seti ile Spark uygulamaları oluşturmak](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.

> [!IMPORTANT]
> Oluşturma ve uygulamalar yalnızca Linux'ta bir HDInsight Spark kümesine göndermek için bu eklentiyi kullanabilirsiniz.
> 

## <a name="prerequisites"></a>Önkoşullar

- HDInsight Linux üzerinde Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
- Oracle Java Geliştirme Seti. Buradan yükleyebilirsiniz [Oracle Web sitesi](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- Intellij Idea. Bu makalede, sürüm 2017.1 kullanır. Buradan yükleyebilirsiniz [JetBrains Web sitesi](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Intellij için Azure araç setini yükleme
Yükleme yönergeleri için bkz. [Intellij için Azure Araç Seti'ni yükleme](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="get-started"></a>Başlarken
Kullanıcı şunları yapabilir ya da [Azure aboneliği için oturum açın](#sign-in-to-your-azure-subscription), veya [bir HDInsight kümesini bağlarsınız](#link-a-cluster) Ambari kullanarak kullanıcı adı/parola veya etki alanına katılmış başlatmak için kimlik bilgisi.


## <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Intellij IDE başlatın ve Azure Gezgini'ni açın. Üzerinde **görünümü** menüsünde **aracı Windows**ve ardından **Azure Gezgini**.
       
   ![Azure Gezgini bağlantısı](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

1. Sağ **Azure** düğümüne tıklayın ve ardından **oturum**.

1. İçinde **Azure oturum açma** iletişim kutusunda **oturum**ve ardından Azure kimlik bilgilerinizi girin.

    ![Azure oturum açma iletişim kutusu](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

1. Oturum açtıktan sonra **abonelikleri seçin** iletişim kutusunda kimlik bilgileriyle ilişkili olan tüm Azure abonelikleri listelenir. Seçin **seçin** düğmesi.

    ![Abonelikleri seçin iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

1. Üzerinde **Azure Gezgini** sekmesinde, genişletme **HDInsight** aboneliğinizdeki HDInsight Spark kümeleri görüntülemek için.
   
    ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

1. Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görüntülemek için başka bir küme adı düğümü genişletebilirsiniz.
   
    ![Genişletilmiş bir küme adı düğümü](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Küme bağlantı
Ambari yönetilen kullanıcı adı kullanarak, normal bir HDInsight kümesine bağlayabilirsiniz. Benzer şekilde, bir etki alanına katılmış HDInsight kümesi için etki alanı ve kullanıcı adı, aşağıdaki gibi kullanarak bağlayabilirsiniz user1@contoso.com.

1. Seçin **küme bağlantı** gelen **Azure Gezgini**.

   ![bağlantı kümesi bağlam menüsü](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)


1. Girin **küme adı**, **kullanıcı adı** ve **parola**. Kullanıcı adı ve parola kimlik doğrulama hatası geldiyseniz denetlemek gerekir. İsteğe bağlı olarak, depolama hesabı depolama anahtarı ekleyin ardından depolama kapsayıcısından bir kapsayıcıyı seçin. Depolama Gezgini'nde soldaki ağaçtan depolama bilgilerini içindir
   
   ![bağlantı kümesi iletişim kutusu](./media/apache-spark-intellij-tool-plugin/link-a-cluster-dialog.png)

   > [!NOTE]
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlantılı depolama anahtarı, kullanıcı adı ve parola kullanın.
   > ![Depolama Gezgini'nde Intellij](./media/apache-spark-intellij-tool-plugin/storage-explorer-in-IntelliJ.png)

   
1. Bağlı küme gördüğünüz **HDInsight** giriş bilgileri doğru ise, düğüm. Artık bağlantılı bu kümeye bir uygulama gönderebilir.

   ![bağlı küme](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

1. Ayrıca, bir kümeden kesebilir **Azure Gezgini**.
   
   ![bağlantısız küme](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulamasını çalıştırma

1. IntelliJ IDEA’yı başlatın ve sonra bir proje oluşturun. **Yeni Proje** iletişim kutusunda aşağıdakileri yapın: 

   a. **HDInsight** > **HDInsight’ta Spark (Scala)** seçeneğini belirleyin.

   b. **Derleme aracı** listesinde, gereksiniminize göre aşağıdakilerden birini seçin:

      * Scala projesi oluşturma sihirbazı desteği için **Maven**
      * Scala projesi için bağımlılıkları ve derlemeyi yönetmek için **SBT**

    ![Yeni Proje iletişim kutusu](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

1. **İleri**’yi seçin.

1. Scala projesi oluşturma sihirbazı, Scala eklentisini yükleyip yüklemediğinizi otomatik olarak algılar. **Yükle**’yi seçin.

   ![Scala Eklentisi Denetimi](./media/apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

1. Scala eklentisini indirmek için **Tamam**’ı seçin. IntelliJ’yi yeniden başlatmak için yönergeleri izleyin. 

   ![Scala eklentisi yükleme iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

1. **Yeni Proje** penceresinde aşağıdakileri yapın:  

    ![Spark SDK’sını seçme](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Bir proje adı ve konum girin.

   b. **Proje SDK’sı** açılır listesinde, Spark 2.x kümesi için **Java 1.8**’i seçin veya Spark 1.x kümesi için **Java 1.7**’yi seçin.

   c. **Spark sürümü** açılır listesinde Scala projesi oluşturma sihirbazı, Spark SDK’sı ve Scala SDK’sı için uygun sürümü bütünleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, **Spark 2.0.2 (Scala 2.11.8)** kullanır.

1. **Son**’u seçin.

1. Spark projesi bir yapıt sizin için otomatik olarak oluşturur. Yapıtı görüntülemek için aşağıdakileri yapın:

   a. Üzerinde **dosya** menüsünde **Proje yapısı**.

   b. İçinde **Proje yapısı** iletişim kutusunda **Yapıtları** oluşturulan varsayılan yapıtı görüntülemek için. Artı işaretini seçerek kendi yapıt oluşturabilirsiniz (**+**).

      ![Yapıt bilgileri iletişim kutusunda](./media/apache-spark-intellij-tool-plugin/default-artifact.png)
      
1. Aşağıdakileri yaparak, uygulama kaynak kodunu ekleyin:

   a. Proje Gezgini'nde sağ **src**, işaret **yeni**ve ardından **Scala sınıfı**.
      
      ![Proje Gezgini'nde Scala sınıfı oluşturmak için komutları](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. İçinde **yeni Scala Sınıf Oluştur** iletişim kutusunda, bir ad belirtin, seçin **nesne** içinde **tür** kutusuna ve ardından **Tamam**.
      
      ![Yeni Scala sınıf iletişim kutusu oluşturma](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. İçinde **MyClusterApp.scala** dosyasında, aşağıdaki kodu yapıştırın. Kod HVAC.csv (tüm HDInsight Spark kümelerinde kullanılabilen) verileri okur, yedinci sütunda CSV dosyasındaki tek basamak içeren satırları alır ve çıktıyı Yazar **/HVACOut** varsayılan depolama kapsayıcısı altında Küme için.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

1. Uygulama, aşağıdakileri yaparak bir HDInsight Spark kümesi üzerinde çalıştırın:

   a. Proje Gezgini'nde proje adına sağ tıklayın ve ardından **gönderin, HDInsight için Spark uygulaması**.
      
      ![HDInsight komutu Gönder Spark uygulaması](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. Azure aboneliği kimlik bilgilerinizi girmeniz istenir. İçinde **Spark gönderimi** iletişim kutusunda aşağıdaki değerleri sağlayın ve ardından **Gönder**.
      
      * İçin **Spark kümeleri (yalnızca Linux)**, uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.

      * Intellij projeden bir yapı seçin veya bir sabit sürücüden seçin.

      * İçinde **ana sınıf adı** kutusunda, üç noktayı seçin (**...** ), uygulama kaynak kodunuzda ana sınıfı seçin ve ardından **Tamam**.

        ![Main sınıfı seçin iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Uygulama kodu bu örnekteki komut satırı bağımsız değişkenleri gerektirmez veya jar dosyaları dışındaki veya dosyalara başvurmak için diğer kutuları boş bırakabilirsiniz. İletişim kutusu, tüm bilgileri verdikten sonra aşağıdaki görüntüde benzemelidir.
        
        ![Spark gönderim iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. **Spark gönderimi** pencerenin alt kısmındaki sekme, ilerleme durumunu görüntüleme başlamalıdır. Kırmızı düğmeye seçerek uygulama durdurabilirsiniz **Spark gönderimi** penceresi.
      
     ![Spark gönderimi penceresi](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      İş çıktısı erişim öğrenmek için bkz. "erişim ve Intellij için Azure araç setini kullanarak HDInsight Spark kümeleri yönetme" bölümünde bu makalenin sonraki bölümlerinde.

## <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Spark uygulamalarında bir HDInsight kümesi üzerinde yerel olarak veya uzaktan hata ayıklama 
Spark uygulamayı kümeye gönderilirken bir başka yolu da öneririz. Parametreleri ayarlayarak bunu yapabilirsiniz **Çalıştır/hata ayıklama yapılandırmaları** IDE. Daha fazla bilgi için [Spark uygulamaları yerel olarak veya uzaktan Azure araç seti ile HDInsight kümesinde SSH üzerinden Intellij için hata ayıklama](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).



## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Erişim ve Intellij için Azure Araç Seti'ni kullanarak HDInsight Spark kümeleri yönetme
Intellij için Azure Araç Seti'ni kullanarak çeşitli işlemler gerçekleştirebilirsiniz.

### <a name="access-the-job-view"></a>İş görünümü erişim
1. Azure Gezgini'nde **HDInsight**Spark küme adını genişletin ve ardından **işleri**.  

    ![İş görünümü düğümü](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

1. Sağ bölmede, **Spark iş görünümü** sekmesi, küme üzerinde çalıştırılan tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

    ![Uygulama ayrıntıları](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)
    >Not
    >

1. Temel çalışan iş bilgilerini görüntülemek için iş grafiğinin getirin. Aşamaları graph ve her işin oluşturduğu bilgileri görüntülemek için iş grafiğinin üzerindeki bir düğüm seçin.

    ![İş aşaması ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

1. Sık görüntülemek için günlükleri gibi kullanılan *sürücü Stderr*, *sürücü Stdout*, ve *dizin bilgisi*seçin **günlük** sekmesi.

    ![Günlük Ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

1. Spark geçmiş UI ve YARN kullanıcı Arabiriminde (uygulama düzeyinde) pencerenin üstünde bir bağlantıyı seçerek de görüntüleyebilirsiniz.

### <a name="access-the-spark-history-server"></a>Erişim Spark geçmiş sunucusu
1. Azure Gezgini'nde **HDInsight**Spark küme adınızı sağ tıklayın ve ardından **açık Spark geçmiş UI**. 

1. İstendiğinde, küme oluşturma sırasında belirtilen kümenin yönetici kimlik bilgilerinizi girin.

1. Spark geçmiş sunucusu Panoda, uygulama adı, uygulama için yalnızca çalışması sona aramak için kullanabilirsiniz. Önceki kodda, uygulama adı kullanarak ayarladığınız `val conf = new SparkConf().setAppName("MyClusterApp")`. Bu nedenle, Spark uygulaması adınız olduğu **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Ambari portalını başlatma
1. Azure Gezgini'nde **HDInsight**Spark küme adınızı sağ tıklayın ve ardından **açık küme yönetim portalı (Ambari)**. 

1. İstendiğinde, küme için yönetici kimlik bilgilerini girin. Küme kurulumu sırasında bu kimlik bilgileri belirtirsiniz.

### <a name="manage-azure-subscriptions"></a>Azure aboneliklerini yönetme
Varsayılan olarak, tüm Azure abonelik Spark kümeleri Intellij için Azure Araç Seti listeler. Gerekirse, erişmek istediğiniz abonelikleri belirtebilirsiniz. 

1. Azure Gezgini içinde sağ **Azure** kök düğümünü ve ardından **Aboneliklerini Yönet**. 

1. İletişim kutusunda, erişim ve ardından istemediğiniz abonelikleri yanındaki onay kutularını temizleyin **Kapat**. Belirleyebilirsiniz **oturum kapatma** dışında Azure aboneliğinizde oturum açmak istiyorsanız.

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Intellij için Azure Araç Seti kullanmak için Intellij Idea uygulamalara dönüştürün
Mevcut Spark Scala dönüştürebilirsiniz Intellij için Azure araç seti ile uyumlu olacak şekilde Intellij Idea'te oluşturduğunuz uygulamalar. Ardından eklenti uygulamaları bir HDInsight Spark kümesine göndermek için kullanabilirsiniz.

1. Intellij Idea oluşturulan bir mevcut Scala Spark uygulaması için ilişkili .iml dosyasını açın.

1. Kök dizininde düzeyidir bir **Modülü** öğesi aşağıdaki gibi:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Eklenecek öğenin Düzenle `UniqueKey="HDInsightTool"` böylece **Modülü** öğesi aşağıdaki gibi görünür:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

1. Değişiklikleri kaydedin. Uygulamanız artık Intellij için Azure araç seti ile uyumlu olmalıdır. Proje Gezgini'nde proje adına sağ tıklayarak test edebilirsiniz. Açılır menü seçeneği artık etkin **gönderin, HDInsight için Spark uygulaması**.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Yerel çalıştırma hatası: *Lütfen daha büyük bir yığın boyutu kullanın*
Spark 1.6 içinde yerel çalıştırma sırasında bir 32-bit Java SDK'sı kullanıyorsanız, şu hatalarla karşılaşabilirsiniz:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Yığın boyutu, çalıştırılacak Spark için yeterince büyük olmadığı için bu hataları gerçekleşir. Spark, en az 471 MB gerekir. (Daha fazla bilgi için [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Basit bir çözüm, bir 64 bit Java SDK'sı kullanmaktır. Aşağıdaki seçenekler ekleyerek Intellij JVM ayarlarını da değiştirebilirsiniz:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Intellij "VM Seçenekleri" kutusunda seçenekleri ekleme](./media/apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>SSS
Ne zaman bir küme bağlantı, ben depolama kimlik bilgileri vermenizi Öner.

![Küme bağlantı, depolama kimlik bilgilerini belirtin](./media/apache-spark-intellij-tool-plugin/link-cluster-with-storage-credential-intellij.png)

İşleri göndermek için iki mod vardır. Depolama kimlik bilgisi sağlanır, toplu iş modu işi göndermek için kullanılır. Aksi takdirde, etkileşimli mod kullanılır. Küme meşgul ise, aşağıdaki hata alabilirsiniz.

![Intellij alma hatası ne zaman meşgul küme](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-upload.png)

![Intellij alma hatası ne zaman meşgul küme](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-submit.png)

## <a name="feedback-and-known-issues"></a>Geri bildirim ve bilinen sorunlar
Şu anda, Spark çıkışları doğrudan görüntüleme desteklenmiyor.

Tüm öneriler veya Geri bildiriminiz varsa veya bu eklenti kullandığınızda herhangi bir sorunla karşılaşırsanız adresinden bize e-posta hdivstool@microsoft.com.

## <a name="seealso"></a>Sonraki adımlar
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="demo"></a>Tanıtım
* Scala proje oluştur (video): [Spark Scala uygulamaları oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [uzaktan HDInsight kümesi üzerinde Spark uygulamalarında hata ayıklamak amacıyla Intellij için Azure Araç Seti'ni kullanma](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Oluşturma ve uygulamaları çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak amacıyla Intellij için Azure Araç Seti'ni kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak amacıyla Intellij için Azure Araç Seti'ni kullanma](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçlarını kullanın](apache-spark-eclipse-tool-plugin.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
