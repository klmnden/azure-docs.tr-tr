---
title: "Intellij için Azure Araç Seti: Spark Hdınsight kümesi için uygulama oluşturma | Microsoft Docs"
description: "Spark Scala içinde yazılmış uygulamalar geliştirmek için Azure Araç Seti Intellij için kullanın ve bunları bir Hdınsight Spark kümesi gönderin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2017
ms.author: maxluk,jejiang
ms.openlocfilehash: 69f5857f89271b3e4865b93e42e5233ead572715
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Intellij için Azure Araç Seti Spark Hdınsight kümesi için uygulamalar oluşturmak için kullanın

Spark Scala içinde yazılmış uygulamalar geliştirmek için Azure Araç Seti Intellij eklentisini kullanın ve ardından bir Hdınsight Spark kümesinde doğrudan Intellij tümleşik geliştirme ortamından (IDE) gönderin. Birkaç yolla eklenti kullanabilirsiniz:

* Geliştirme ve Hdınsight Spark kümesi üzerinde bir Scala Spark uygulamasının gönderin.
* Azure Hdınsight Spark küme kaynaklarına erişim.
* Geliştirme ve Scala Spark uygulama yerel olarak çalıştırın.

Projenizi oluşturmak için görüntülemek [Intellij için Azure araç seti ile Spark uygulamaları oluşturmak](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.

> [!IMPORTANT]
> Bu eklenti oluşturmak ve yalnızca Linux'ta Hdınsight Spark kümesinde uygulamalar göndermek amacıyla kullanabilirsiniz.
> 

## <a name="prerequisites"></a>Önkoşullar

- Hdınsight Linux üzerinde Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).
- Oracle Java Geliştirme Seti. Şuradan yükleyebilirsiniz [Oracle Web sitesi](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. Bu makalede sürümünü 2017.1 kullanır. Şuradan yükleyebilirsiniz [JetBrains Web sitesi](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Intellij için Azure Araç Seti yükleyin
Yükleme yönergeleri için bkz: [Intellij için Azure Araç Seti yüklemek](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Intellij IDE başlatın ve Azure Gezgini'ni açın. Üzerinde **Görünüm** menüsünde, select **aracı Windows**ve ardından **Azure Gezgini**.
       
   ![Azure Gezgini bağlantısı](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Sağ **Azure** düğümünü ve ardından **oturum**.

3. İçinde **Azure oturum açma** iletişim kutusunda **oturum**ve ardından Azure kimlik bilgilerinizi girin.

    ![Azure oturum açma iletişim kutusu](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. ' De, oturum açtığınız sonra **seçin abonelikleri** iletişim kutusu kimlik bilgileriyle ilişkili tüm Azure abonelikleri listeler. Seçin **seçin** düğmesi.

    ![Abonelik Seç iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. Üzerinde **Azure Gezgini** sekmesinde, genişletin **Hdınsight** aboneliğinizde Hdınsight Spark kümelerini görüntülemek için.
   
    ![Azure Explorer'da Hdınsight Spark kümeleri](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görüntülemek için başka bir küme adı düğümü genişletebilirsiniz.
   
    ![Genişletilmiş bir küme adı düğümü](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Bir küme bağlantı
Yönetilen Ambari kullanıcı adı kullanarak normal bir küme bağlama, ayrıca güvenlik hadoop kümesi etki alanı kullanıcı adı kullanarak bağlantı (örneğin: user1@contoso.com). 
1. Tıklatın **bir küme bağlantı** gelen **Azure Gezgini**.

   ![bağlantı küme bağlam menüsü](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

2. Girin **küme adı**, **depolama hesabı**, **depolama anahtarı**, bir kapsayıcı seçin **depolama kapsayıcısı**, en son olarak, kullanıcı adı girin ve parolası. Kullanıcı adı ve parola denetlemek gereken kimlik doğrulama hatası alır.
   
   ![bağlantı küme iletişim](./media/apache-spark-intellij-tool-plugin/link-a-cluster-dialog.png)

   > [!NOTE]
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlantılı depolama anahtarı, kullanıcı adı ve parola kullanın. 
   
3. Bir bağlı kümede görebilirsiniz **Hdınsight** giriş bilgilerin doğru olup olmadığını düğümü. Şimdi bu bağlantılı küme uygulamaya gönderebilirsiniz.

   ![bağlantılı küme](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

4. Bir kümeden bağlantısını kaldırabilirsiniz **Azure Gezgini**.
   
   ![bağlantısız küme](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Hdınsight Spark kümesi üzerinde Spark Scala uygulamayı çalıştırın

1. Intellij Idea başlatın ve bir proje oluşturun. İçinde **yeni proje** iletişim kutusunda, aşağıdakileri yapın: 

   a. Seçin **Hdınsight** > **(Scala) hdınsight'ta Spark**.

   b. İçinde **oluşturma aracını** listesinde, gereksiniminize göre aşağıdakilerden birini seçin:

      * **Maven**, Scala Proje Oluşturma Sihirbazı'nı desteği
      * **SBT**, bağımlılıkları yönetme ve Scala projeyi oluşturmak için

    ![Yeni Proje iletişim kutusu](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. **İleri**’yi seçin.

3. Scala Proje Oluşturma Sihirbazı'nı otomatik olarak eklenti Scala yüklediğiniz olup olmadığını algılar. Seçin **yükleme**.

   ![Scala eklentisi onay](./media/apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. Eklenti Scala yüklemek üzere seçin **Tamam**. Intellij yeniden başlatmak için yönergeleri izleyin. 

   ![Scala eklentisi yükleme iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. İçinde **yeni proje** penceresinde aşağıdakileri yapın:  

    ![SDK Spark seçme](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Bir proje adı ve konum girin.

   b. İçinde **proje SDK** aşağı açılan listesinden, **Java 1.8** seçin veya Spark 2.x kümesi için **Java 1.7** Spark 1.x kümesi için.

   c. İçinde **Spark sürüm** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK ve Scala SDK için uygun sürüm tümleştirir. Spark kümesi sürüm 2.0 eskiyse, seçin **1.x Spark**. Aksi takdirde seçin **Spark2.x**. Bu örnekte **Spark 2.0.2 (Scala 2.11.8)**.

6. **Son**’u seçin.

7. Spark proje bir yapı sizin için otomatik olarak oluşturur. Yapı görüntülemek için aşağıdakileri yapın:

   a. Üzerinde **dosya** menüsünde, select **proje yapısını**.

   b. İçinde **proje yapısını** iletişim kutusunda **yapıları** oluşturulan varsayılan yapı görüntülemek için. Artı işaretini seçerek kendi yapı de oluşturabilirsiniz (**+**).

      ![Yapı bilgileri iletişim kutusunda](./media/apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. Aşağıdakileri yaparak uygulama kaynak kodu ekleyin:

   a. Proje Gezgini'nde sağ **src**, işaret **yeni**ve ardından **Scala sınıfı**.
      
      ![Proje Gezgini'nde Scala sınıfı oluşturmak için komutları](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. İçinde **yeni Scala Sınıf Oluştur** iletişim kutusunda, bir ad sağlayın, seçin **nesne** içinde **türü** kutusuna ve ardından **Tamam**.
      
      ![Yeni Scala sınıf iletişim kutusu oluşturma](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. İçinde **MyClusterApp.scala** dosya, aşağıdaki kodu yapıştırın. Kod HVAC.csv (tüm Hdınsight Spark kümeleri üzerinde kullanılabilir) verileri okur, yalnızca bir rakam CSV dosyasındaki yedinci sütunundaki satırları alır ve çıktıyı Yazar **/HVACOut** varsayılan depolama kapsayıcısı altında Küme için.

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

9. Uygulama, aşağıdakileri yaparak bir Hdınsight Spark kümesinde çalıştırın:

   a. Proje Gezgini'nde proje adına sağ tıklayın ve ardından **Hdınsight için Spark uygulama gönderme**.
      
      ![Hdınsight komut gönderme Spark uygulamaya](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. Azure aboneliği kimlik bilgilerinizi girmeniz istenir. İçinde **Spark gönderme** iletişim kutusunda, aşağıdaki değerleri girin ve ardından **gönderme**.
      
      * İçin **Spark kümeleri (yalnızca Linux)**, istediğiniz uygulamanızı çalıştırmak Hdınsight Spark kümesi seçin.

      * Intellij projeden bir yapı seçin veya bir sabit sürücüden seçin.

      * İçinde **ana sınıf adı** kutusunda, üç nokta seçin (**...** ), uygulamanın kaynak kodunda ana sınıfı seçin ve ardından **Tamam**.

        ![Ana sınıf Seç iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Bu örnek uygulama kodunda komut satırı bağımsız değişkenleri gerektirmediği veya Kavanoz veya dosyaları başvurmak için kalan kutularını boş bırakabilirsiniz. İletişim kutusu, tüm bilgileri verdikten sonra aşağıdaki resimde benzer olmalıdır.
        
        ![Spark gönderme iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. **Spark gönderme** pencerenin altındaki sekmesi, ilerleme durumunu görüntüleme başlamalıdır. Kırmızı düğmesini seçerek uygulama durdurabilirsiniz **Spark gönderme** penceresi.
      
     ![Spark gönderme penceresi](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      İş çıktısı erişmek öğrenmek için bkz: "erişim ve Intellij için Azure Araç Seti kullanarak Hdınsight Spark kümeleri yönetmek" bölümünde bu makalenin sonraki bölümlerinde yer.

## <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Spark uygulamalarında Hdınsight kümesi üzerinde yerel olarak veya uzaktan hata ayıklama 
Küme Spark uygulamaya gönderilmesi bir başka yolu da öneririz. Parametreleri ayarlayarak yapabilirsiniz **Çalıştır/hata ayıklama yapılandırmaları** IDE. Daha fazla bilgi için bkz: [Spark uygulamalarında yerel olarak veya uzaktan Hdınsight kümesinde Azure araç seti ile SSH aracılığıyla Intellij için hata ayıklama](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).



## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Erişim ve Hdınsight Spark kümeleri Intellij için Azure Araç Seti kullanarak yönetme
Intellij için Azure Araç Seti kullanarak çeşitli işlemler gerçekleştirebilirsiniz.

### <a name="access-the-job-view"></a>İş görünüme erişme
1. Azure Explorer'da genişletin **Hdınsight**Spark küme adını genişletin ve ardından **işleri**.  

    ![Proje görünümü düğümü](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

2. Sağ bölmede **Spark iş görünümünde** sekmesi, küme üzerinde çalıştırılan tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

    ![Uygulama ayrıntıları](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)
    >Not
    >

3. Temel çalışan iş bilgilerini görüntülemek için iş grafiğinin getirin. Aşamaları grafik ve her iş oluşturur bilgilerini görüntülemek için iş grafiği bir düğüm seçin.

    ![İş aşama ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Sık görüntülemek için günlükleri gibi kullanılan *sürücü Stderr*, *sürücü Stdout*, ve *dizin bilgisi*seçin **günlük** sekmesi.

    ![Günlük Ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

5. Spark geçmişi kullanıcı Arabirimi ve YARN kullanıcı Arabiriminde (uygulama düzeyinde) bir bağlantı pencerenin üstündeki seçerek de görüntüleyebilirsiniz.

### <a name="access-the-spark-history-server"></a>Spark geçmişi sunucusuna erişim
1. Azure Explorer'da genişletin **Hdınsight**Spark küme adına sağ tıklayın ve ardından **açık Spark geçmişi kullanıcı Arabirimi**. 

2. İstendiğinde, kümeyi oluşturduğunuzda, belirttiğiniz kümenin yönetici kimlik girin.

3. Spark geçmişi server panosunda, uygulama için yalnızca çalışması sona aramak için uygulama adı kullanabilirsiniz. Önceki kod, uygulamanın adını kullanarak ayarladığınız `val conf = new SparkConf().setAppName("MyClusterApp")`. Bu nedenle, Spark uygulamanızın adı olan **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Ambari Portalı'nı Başlat
1. Azure Explorer'da genişletin **Hdınsight**Spark küme adına sağ tıklayın ve ardından **açık küme yönetim portalı (Ambari)**. 

2. İstendiğinde, küme için yönetici kimlik bilgilerini girin. Küme kurulumu sırasında bu kimlik bilgileri belirttiniz.

### <a name="manage-azure-subscriptions"></a>Azure Aboneliklerini yönetmek
Varsayılan olarak, tüm Azure aboneliklerinden Spark kümeleri Intellij için Azure Araç Seti listeler. Gerekirse, erişim sağlamak istediğiniz abonelikleri belirtebilirsiniz. 

1. Azure Gezgini'nde sağ **Azure** kök düğümünü ve ardından **Aboneliklerini Yönet**. 

2. İletişim kutusunda, erişim ve ardından istemediğiniz abonelikleri yanındaki onay kutularını temizleyin **Kapat**. Öğesini de seçebilirsiniz **oturum kapatma** dışında Azure aboneliğinizin imzalanacak istiyorsanız.

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Intellij için Azure araç kullanma Intellij Idea uygulamalarınız Dönüştür
Varolan Spark Scala dönüştürebilirsiniz Intellij için Azure araç seti ile uyumlu olacak şekilde Intellij Idea içinde oluşturulan uygulamaları. Ardından eklenti Hdınsight Spark kümesinde uygulamaları göndermek için kullanabilirsiniz.

1. Intellij Idea ile oluşturulmuş bir varolan Spark Scala uygulama için ilişkili .iml dosyasını açın.

2. Kök dizininde düzeyidir bir **Modülü** öğesi aşağıdaki gibi:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Eklenecek öğenin Düzenle `UniqueKey="HDInsightTool"` böylece **Modülü** öğesi aşağıdaki gibi görünür:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Değişiklikleri kaydedin. Uygulamanız artık Intellij için Azure araç seti ile uyumlu olması gerekir. Proje Gezgini'nde proje adına sağ tıklayarak test edebilirsiniz. Açılır menü seçeneği artık sahiptir **Hdınsight için Spark uygulama gönderme**.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Yerel çalıştırma hatası: *Lütfen daha büyük bir öbek boyutu kullanın*
Yerel çalıştırma sırasında bir 32 bit Java SDK'sı kullanıyorsanız, Spark 1.6 içinde aşağıdaki hatalarla karşılaşabilirsiniz:

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

Yığın boyutu çalıştırmak Spark için yeterince büyük olduğundan bu hataları gerçekleşir. Spark en az 471 MB gerektirir. (Daha fazla bilgi için bkz: [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Basit bir çözüm, bir 64-bit Java SDK'sı kullanmaktır. Aşağıdaki seçenekler ekleyerek de Intellij JVM ayarlarınızda değiştirebilirsiniz:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Intellij "VM Seçenekleri" kutusunda seçenekleri ekleme](./media/apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>SSS
Azure Data Lake Store uygulamaya göndermek için tercih **etkileşimli** Azure oturum açma işlemi sırasında modu. Seçerseniz **otomatik** modu, bir hata alabilirsiniz.

![interative-signin](./media/apache-spark-intellij-tool-plugin/interative-signin.png)

Şimdi, biz bunu çözümlendi. Uygulamanız herhangi bir oturum açma yöntemi göndermek için bir Azure Data Lake küme seçebilirsiniz.

## <a name="feedback-and-known-issues"></a>Geri bildirim ve bilinen sorunlar
Şu anda, Spark çıkışları doğrudan görüntüleme desteklenmiyor.

Tüm öneriler veya Geri bildiriminiz varsa veya bu eklenti kullandığınızda herhangi bir sorunla karşılaşırsanız, adresinden bize e-posta hdivstool@microsoft.com.

## <a name="seealso"></a>Sonraki adımlar
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="demo"></a>Tanıtım
* Scala proje oluştur (video): [Spark Scala uygulamaları oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [uzaktan Hdınsight kümesi üzerinde Spark uygulamalarında hata ayıklamasını Intellij için kullanım Azure Araç Seti](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla Hdınsight'ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için hdınsight'ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Oluşturma ve uygulamaları çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Hdınsight araçları Azure araç setini Eclipse için Spark uygulamaları oluşturmak için kullanın](apache-spark-eclipse-tool-plugin.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

