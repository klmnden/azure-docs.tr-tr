---
title: 'Öğretici - Intellij için Azure Araç Seti: Spark uygulamaları için bir HDInsight kümesi oluşturma'
description: Öğretici - Spark Scala içinde yazılmış uygulamalar geliştirmek için Intellij için Azure araç takımı kullanın ve bunları bir HDInsight Spark kümesine göndermek.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/26/2019
ms.author: hrasheed
ms.openlocfilehash: 0a434246791e73e24af1ffe7abd722f5265ca5b6
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462373"
---
# <a name="tutorial-use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Öğretici: Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için Intellij için Azure Araç Seti'ni kullanma

Bu eğitimde yazılan Apache Spark uygulamaları geliştirmek için eklenti Intellij için Azure Araç Seti'ni kullanma [Scala](https://www.scala-lang.org/)ve ardından bir HDInsight Spark kümesine doğrudan tümleştirilmiş olan Intellij gönderin geliştirme ortamı (IDE). Birkaç yöntemle eklenti kullanabilirsiniz:

* Geliştirin ve bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulaması gönderin.
* Azure HDInsight Spark kümesi kaynaklarınıza erişin.
* Geliştirin ve yerel olarak Scala Spark uygulamasını çalıştırın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Eklenti Intellij için Azure araç takımı kullanın
> * Apache Spark uygulamaları geliştirin
> * Azure HDInsight kümesine uygulama gönderme

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* [Oracle Java Development kit](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  Bu öğreticide, Java Sürüm 8.0.202 kullanılır.

* IntelliJ IDEA. Bu makalede [Intellij Idea topluluk ver.  2018.3.4](https://www.jetbrains.com/idea/download/).

* Intellij için Azure Araç Seti.  Bkz: [Intellij için Azure araç setini yükleme](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable).

* WINUTILS. EXE.  Bkz: [Windows üzerinde Hadoop çalıştırırken sorun](https://wiki.apache.org/hadoop/WindowsProblems).

## <a name="install-scala-plugin-for-intellij-idea"></a>IntelliJ IDEA için Scala eklentisini yükleme

Scala eklentisini yüklemek için aşağıdaki adımları gerçekleştirin:

1. IntelliJ IDEA’yı açın.

2. Hoş Geldiniz ekranında gidin **yapılandırma** > **eklentileri** açmak için **eklentileri** penceresi.
   
    ![Scala eklentisini etkinleştirme](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin.png)

3. Seçin **yükleme** yeni pencerede öne çıkan Scala eklentisi için.  

    ![Scala eklentisini yükleme](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Eklenti başarıyla yüklendikten sonra IDE’yi yeniden başlatmanız gerekir.

## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>Bir Scala Spark uygulaması için bir HDInsight Spark kümesi oluşturma

1. Intellij Idea başlatın ve **yeni proje oluştur** açmak için **yeni proje** penceresi.

2. Seçin **Azure Spark/HDInsight** sol bölmeden.

3. Seçin **Spark proje (Scala)** ana penceresinde.

4. Gelen **derleme aracı** aşağı açılan listesinde, aşağıdakilerden birini seçin:
   * **Maven** Scala Proje Oluşturma Sihirbazı'nı desteği.
   * **SBT** bağımlılıkları yönetmek ve için Scala projesi oluşturma.

     ![Yeni Proje iletişim kutusu](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. **İleri**’yi seçin.

6. İçinde **yeni proje** penceresinde aşağıdaki bilgileri sağlayın:  

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |Proje adı| Bir ad girin.  Bu öğreticide `myApp` kullanılır.|  
    |Proje&nbsp;konumu| Projenizi kaydetmek istediğiniz konumu girin.|
    |Proje SDK'sı| Bu, ilk fikir kullanımınız boş olabilir.  Seçin **yeni...**  ve, JDK için gidin.|
    |Spark sürümü|Oluşturma Sihirbazı'nı, Spark SDK'sı ve Scala SDK'sı için doğru sürüm tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnekte **Spark 2.3.0 (Scala 2.11.8)** .|

    ![Spark SDK’sını seçme](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

7. **Son**’u seçin.  Bu projenin kullanılabilir duruma gelmesi birkaç dakika sürebilir.

8. Spark projesi bir yapıt sizin için otomatik olarak oluşturur. Yapıtı görüntülemek için aşağıdakileri yapın:

   a. Menü çubuğundan gidin **dosya** > **Proje yapısı...** .

   b. Gelen **Proje yapısı** penceresinde **Yapıtları**.  

   c. Seçin **iptal** yapıt görüntüledikten sonra.

      ![Yapıt bilgileri iletişim kutusunda](./media/apache-spark-intellij-tool-plugin/default-artifact.png)

9. Aşağıdakileri yaparak, uygulama kaynak kodunu ekleyin:

    a. Projeden gidin **myApp** > **src** > **ana** > **scala**.  

    b. Sağ **scala**ve ardından gidin **yeni** > **Scala sınıfı**.

   ![Projeden bir Scala sınıfı oluşturmak için komutları](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. İçinde **yeni Scala Sınıf Oluştur** iletişim kutusunda, bir ad belirtin, seçin **nesne** içinde **tür** aşağı açılan liste ve ardından **Tamam**.

     ![Yeni Scala sınıf iletişim kutusu oluşturma](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. **MyApp.scala** dosya ana görünümünde sonra açılır. Varsayılan kodu aşağıdaki kodla değiştirin:  

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object myApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("myApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasbs:///HVACOut")
            }
    
        }

    Kod HVAC.csv (tüm HDInsight Spark kümelerinde kullanılabilen) verileri okur, yedinci sütunda CSV dosyasındaki tek basamak içeren satırları alır ve çıktıyı Yazar `/HVACOut` kümenin varsayılan depolama kapsayıcısını altında.

## <a name="connect-to-your-hdinsight-cluster"></a>HDInsight kümenize bağlanın
Kullanıcı şunları yapabilir ya da [Azure aboneliği için oturum açın](#sign-in-to-your-azure-subscription), veya [bir HDInsight kümesini bağlarsınız](#link-a-cluster) Ambari kullanarak kullanıcı adı/parola veya etki alanına katılmış HDInsight kümenize bağlanmak için kimlik bilgisi.

### <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Menü çubuğundan gidin **görünümü** > **aracı Windows** > **Azure Gezgini**.
       
   ![Azure Gezgini bağlantısı](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Azure Gezgini'nden sağ **Azure** düğümüne tıklayın ve ardından **oturum**.
   
   ![Azure Gezgini bağlantısı](./media/apache-spark-intellij-tool-plugin/explorer-rightclick-azure.png)

3. İçinde **Azure oturum açma** iletişim kutusunda **cihaz oturum açma**ve ardından **oturum**.

    ![Azure oturum açma iletişim kutusu](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. İçinde **Azure cihaz oturum açma** iletişim kutusu, tıklayın **açın & Kopyala**.
   
   ![Azure oturum açma iletişim kutusu](./media/apache-spark-intellij-tool-plugin/view-explorer-5.png)

5. Tarayıcı arabirimi kodu yapıştırın ve ardından **sonraki**.
   
   ![Azure oturum açma iletişim kutusu](./media/apache-spark-intellij-tool-plugin/view-explorer-6.png)

6. Azure kimlik bilgilerinizi girin ve sonra Tarayıcıyı kapatın.
   
   ![Azure oturum açma iletişim kutusu](./media/apache-spark-intellij-tool-plugin/view-explorer-7.png)

7. Oturum açtıktan sonra **abonelikleri seçin** iletişim kutusunda kimlik bilgileriyle ilişkili olan tüm Azure abonelikleri listelenir. Aboneliğinizi seçin ve ardından **seçin** düğmesi.

    ![Abonelikleri seçin iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

8. Gelen **Azure Gezgini**, genişletme **HDInsight** aboneliklerinizde olan HDInsight Spark kümeleri görüntülemek için.

    ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

9.  Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görüntülemek için başka bir küme adı düğümü genişletebilirsiniz.

    ![Genişletilmiş bir küme adı düğümü](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

### <a name="link-a-cluster"></a>Küme bağlantı

Yönetilen Apache Ambari kullanıcı adını kullanarak bir HDInsight kümesine bağlayabilirsiniz. Benzer şekilde, bir etki alanına katılmış HDInsight kümesi için etki alanı ve kullanıcı adı, aşağıdaki gibi kullanarak bağlayabilirsiniz user1@contoso.com. Ayrıca, Livy Service kümesi bağlayabilirsiniz.

1. Menü çubuğundan gidin **görünümü** > **aracı Windows** > **Azure Gezgini**.

2. Azure Gezgini'nden sağ **HDInsight** düğümüne tıklayın ve ardından **bağlantı bir küme**.

   ![bağlantı kümesi bağlam menüsü](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

3. Mevcut seçenekler **bağlantı bir küme** penceresi, bağlı olarak hangi değeri seçin değişir **bağlantı kaynağı türü** aşağı açılan listesi.  Değerlerinizi girin ve ardından **Tamam**.

    * **HDInsight kümesi**  
  
        |Özellik |Değer |
        |----|----|
        |Bağlantı kaynağı türü|Seçin **HDInsight küme** aşağı açılan listeden.|
        |Küme adı/URL| Küme adı girin.|
        |Kimlik doğrulaması türü| Olarak bırakın **temel kimlik doğrulaması**|
        |Kullanıcı adı| Küme kullanıcı adı girin, varsayılan admin'dir.|
        |Parola| Parola için kullanıcı adı girin.|
    
        ![HDInsight küme iletişim bağlantı](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Livy hizmeti**  
  
        |Özellik |Değer |
        |----|----|
        |Bağlantı kaynağı türü|Seçin **Livy hizmet** aşağı açılan listeden.|
        |Livy uç noktası| Livy uç noktasını girin|
        |Küme Adı| Küme adı girin.|
        |Yarn uç noktası|İsteğe bağlı.|
        |Kimlik doğrulaması türü| Olarak bırakın **temel kimlik doğrulaması**|
        |Kullanıcı adı| Küme kullanıcı adı girin, varsayılan admin'dir.|
        |Parola| Parola için kullanıcı adı girin.|

        ![Livy küme iletişim bağlantı](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

1. Bağlantılı kümenizden gördüğünüz **HDInsight** düğümü.

   ![bağlı küme](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

2. Ayrıca, bir kümeden kesebilir **Azure Gezgini**.

   ![bağlantısız küme](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulamasını çalıştırma

Scala uygulama oluşturduktan sonra küme gönderebilirsiniz.

1. Projeden gidin **myApp** > **src** > **ana** > **scala**  >  **myApp**.  Sağ **myApp**seçip **gönderme Spark uygulaması** (Bu büyük olasılıkla listesinin en altında bulunan olacaktır).
    
      ![HDInsight komutu Gönder Spark uygulaması](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. İçinde **gönderme Spark uygulaması** Seç iletişim kutusu penceresine **1. HDInsight üzerinde Spark**.

3. İçinde **Upravit konfiguraci** penceresinde aşağıdaki değerleri sağlayın ve ardından **Tamam**:

    |Özellik |Değer |
    |----|----|
    |Spark kümeleri (yalnızca Linux)|Uygulamanızı çalıştırmak istiyorsanız HDInsight Spark kümesini seçin.|
    |Göndermek için bir yapı seçin|Varsayılan ayarı değiştirmeyin.|
    |Ana sınıf adı|Seçili dosya ana sınıftan varsayılan değerdir. Sınıfı üç nokta seçerek değiştirebilirsiniz ( **...** ) ve başka bir sınıfı seçme.|
    |Proje yapılandırmaları|Varsayılan anahtarlar ve/veya değerleri değiştirebilirsiniz. Daha fazla bilgi için [Apache Livy REST API](https://livy.incubator.apache.org./docs/latest/rest-api.html).|
    |Komut satırı bağımsız değişkenleri|Gerekirse ana sınıfı için boşluk tarafından ayrılmış bağımsız değişkenleri girebilirsiniz.|
    |Başvurulan jar dosyaları dışındaki ve başvurulan dosyaları|Varsa, başvurulan jar dosyaları dışındaki ve dosyaları için yol girebilirsiniz. Şu anda yalnızca ADLS Gen 2 küme destekleyen Azure sanal dosya sisteminde dosyaları da göz atabilirsiniz. Daha fazla bilgi için: [Apache Spark yapılandırma](https://spark.apache.org/docs/latest/configuration.html#runtime-environment).  Ayrıca bkz [küme kaynaklarını karşıya nasıl](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer).|
    |İşi karşıya yükleme depolama|Ek seçenekleri görmek için genişletin.|
    |Depolama türü|Seçin **karşıya yüklemek için kullanım Azure Blob** aşağı açılan listeden.|
    |Depolama Hesabı|Depolama hesabınızı girin.|
    |Depolama anahtarı|Depolama anahtarınızı girin.|
    |Depolama kapsayıcısı|Aşağı açılan listeden depolama kapsayıcısını seçmek **depolama hesabı** ve **depolama anahtarı** girildi.|

    ![Spark gönderim iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-02.png)

4. Seçin **SparkJobRun** seçili küme projenize göndermek için. **Kümedeki uzak bir Spark işi** sekmesi altındaki iş yürütme ilerleme durumunu görüntüler. Kırmızı düğmeye tıklayarak uygulama durdurabilirsiniz. İş çıktısı erişim öğrenmek için bkz. "erişim ve Intellij için Azure araç setini kullanarak HDInsight Spark kümeleri yönetme" bölümünde bu makalenin sonraki bölümlerinde.  
      
    ![Spark gönderimi penceresi](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Apache Spark uygulamaları bir HDInsight kümesi üzerinde yerel olarak veya uzaktan hata ayıklama 

Spark uygulamayı kümeye gönderilirken bir başka yolu da öneririz. Parametreleri ayarlayarak bunu yapabilirsiniz **Çalıştır/hata ayıklama yapılandırmaları** IDE. Daha fazla bilgi için [Apache Spark uygulamaları yerel olarak veya uzaktan Azure araç seti ile HDInsight kümesinde SSH üzerinden Intellij için hata ayıklama](apache-spark-intellij-tool-debug-remotely-through-ssh.md).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Erişim ve Intellij için Azure Araç Seti'ni kullanarak HDInsight Spark kümeleri yönetme

Intellij için Azure Araç Seti'ni kullanarak çeşitli işlemler gerçekleştirebilirsiniz.  İşlemlerin çoğu başlatılan **Azure Gezgini**.  Menü çubuğundan gidin **görünümü** > **aracı Windows** > **Azure Gezgini**.

### <a name="access-the-job-view"></a>İş görünümü erişim

1. Azure Gezgini'nden gidin **HDInsight** > \<bilgisayarınızı küme >> **işleri**.

    ![İş görünümü düğümü](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

2. Sağ bölmede, **Spark iş görünümü** sekmesi, küme üzerinde çalıştırılan tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

    ![Uygulama ayrıntıları](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)

3. Temel çalışan iş bilgilerini görüntülemek için iş grafiğinin getirin. Aşamaları graph ve her işin oluşturduğu bilgileri görüntülemek için iş grafiğinin üzerindeki bir düğüm seçin.

    ![İş aşaması ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Sık görüntülemek için günlükleri gibi kullanılan *sürücü Stderr*, *sürücü Stdout*, ve *dizin bilgisi*seçin **günlük** sekmesi.

    ![Günlük Ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

5. Spark geçmiş UI ve YARN kullanıcı Arabiriminde (uygulama düzeyinde) pencerenin üstünde bir bağlantıyı seçerek de görüntüleyebilirsiniz.

### <a name="access-the-spark-history-server"></a>Erişim Spark geçmiş sunucusu

1. Azure Gezgini'nden, **HDInsight**Spark küme adınızı sağ tıklayın ve ardından **açık Spark geçmiş UI**.  
2. İstendiğinde, küme oluşturma sırasında belirtilen kümenin yönetici kimlik bilgilerinizi girin.

3. Spark geçmiş sunucusu Panoda, uygulama adı, uygulama için yalnızca çalışması sona aramak için kullanabilirsiniz. Önceki kodda, uygulama adı kullanarak ayarladığınız `val conf = new SparkConf().setAppName("myApp")`. Bu nedenle, Spark uygulaması adınız olduğu **myApp**.

### <a name="start-the-ambari-portal"></a>Ambari portalını başlatma

1. Azure Gezgini'nden, **HDInsight**Spark küme adınızı sağ tıklayın ve ardından **açık küme yönetimi Portal(Ambari)** .  

2. İstendiğinde, küme için yönetici kimlik bilgilerini girin. Küme kurulumu sırasında bu kimlik bilgileri belirtirsiniz.

### <a name="manage-azure-subscriptions"></a>Azure aboneliklerini yönetme

Varsayılan olarak, tüm Azure abonelik Spark kümeleri Intellij için Azure Araç Seti listeler. Gerekirse, erişmek istediğiniz abonelikleri belirtebilirsiniz.  

1. Azure Gezgini'nden sağ **Azure** kök düğümünü ve ardından **abonelikleri seçin**.  

2. Gelen **abonelikleri seçin** penceresindeki onay kutularını yanındaki Abonelikleri, açık erişim ve daha sonra seçmek istemiyorsanız **Kapat**.

## <a name="spark-console"></a>Spark Konsolu

Spark yerel Console(Scala) çalıştırın ya da Spark Livy etkileşimli oturumu Console(Scala) çalıştırın.

### <a name="spark-local-consolescala"></a>Spark yerel Console(Scala)

WINUTILS uyduğunuzdan emin olun. EXE önkoşul.

1. Menü çubuğundan gidin **çalıştırma** > **yapılandırmalarını Düzenle...** .

2. Gelen **Çalıştır/hata ayıklama yapılandırmaları** pencerede sol bölmede gidin **HDInsight üzerinde Apache Spark** >  **[HDInsight üzerinde Spark] myApp**.

3. Ana penceredeki seçin **yerel olarak çalıştırma** sekmesi.

4. Aşağıdaki değerleri sağlayın ve ardından **Tamam**:

    |Özellik |Değer |
    |----|----|
    |Proje ana sınıfı|Seçili dosya ana sınıftan varsayılan değerdir. Sınıfı üç nokta seçerek değiştirebilirsiniz ( **...** ) ve başka bir sınıfı seçme.|
    |Ortam değişkenleri|HADOOP_HOME değeri doğru olduğundan emin olun.|
    |WINUTILS.exe konumu|Yolun doğru olduğundan emin olun.|

    ![Yerel konsol kümesi yapılandırması](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. Projeden gidin **myApp** > **src** > **ana** > **scala**  >  **myApp**.  

6. Menü çubuğundan gidin **Araçları** > **Spark konsol** > **Spark yerel Console(Scala) çalıştırma**.

7. Ardından, otomatik istiyorsanız bağımlılıkları düzeltme istemek için iki iletişim kutuları görüntülenebilir. Bu durumda, seçin **otomatik düzeltme**.

    ![Spark otomatik Fix1](./media/apache-spark-intellij-tool-plugin/console-auto-fix1.png)

    ![Spark otomatik Fix2](./media/apache-spark-intellij-tool-plugin/console-auto-fix2.png)

8. Konsol resme benzer olmalıdır. Konsol penceresi türü `sc.appName`ve sonra ctrl + Enter tuşlarına basın.  Sonuç gösterilir. Kırmızı düğmeye tıklayarak yerel konsol sonlandırabilirsiniz.

    ![Yerel konsol sonucu](./media/apache-spark-intellij-tool-plugin/local-console-result.png)

### <a name="spark-livy-interactive-session-consolescala"></a>Livy etkileşimli oturumu Console(Scala) spark

Yalnızca, Intellij 2018.2 ve 2018.3 desteklenir.

1. Menü çubuğundan gidin **çalıştırma** > **yapılandırmalarını Düzenle...** .

2. Gelen **Çalıştır/hata ayıklama yapılandırmaları** pencerede sol bölmede gidin **HDInsight üzerinde Apache Spark** >  **[HDInsight üzerinde Spark] myApp**.

3. Ana penceredeki seçin **uzaktan çalıştırmak kümesinde** sekmesi.

4. Aşağıdaki değerleri sağlayın ve ardından **Tamam**:

    |Özellik |Değer |
    |----|----|
    |Spark kümeleri (yalnızca Linux)|Uygulamanızı çalıştırmak istiyorsanız HDInsight Spark kümesini seçin.|
    |Ana sınıf adı|Seçili dosya ana sınıftan varsayılan değerdir. Sınıfı üç nokta seçerek değiştirebilirsiniz ( **...** ) ve başka bir sınıfı seçme.|

    ![Etkileşimli konsol kümesi yapılandırması](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. Projeden gidin **myApp** > **src** > **ana** > **scala**  >  **myApp**.  

6. Menü çubuğundan gidin **Araçları** > **Spark konsol** > **çalıştırma Spark Livy etkileşimli oturumu Console(Scala)** .

7. Konsol resme benzer olmalıdır. Konsol penceresi türü `sc.appName`ve sonra ctrl + Enter tuşlarına basın.  Sonuç gösterilir. Kırmızı düğmeye tıklayarak yerel konsol sonlandırabilirsiniz.

    ![Etkileşimli konsol sonucu](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Spark konsola seçimi Gönder

Komut dosyası sonucu yerel konsol veya Livy etkileşimli oturumu Console(Scala) için bazı kod göndererek öngörüyor için uygundur. Scala dosyasındaki bazı kod vurgulayın ve ardından sağ **Spark Konsolu seçimin Gönder**. Seçilen kod konsola gönderilir ve gerçekleştirilmesi. Sonuç, kodun konsolunda görüntülenir. Konsolu, varsa hataları denetler.  

   ![Spark konsola seçimi Gönder](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="reader-only-role"></a>Salt okuyucu rolü

Kullanıcıların gönderme salt okuyucu rolü izni olan bir küme için iş, Ambari kimlik bilgileri gereklidir.

### <a name="link-cluster-from-context-menu"></a>Bağlam menüsünden bağlantı kümesi

1. Salt okuyucu rolü hesabıyla oturum açın.
       
2. Gelen **Azure Gezgini**, genişletme **HDInsight** aboneliğinizde HDInsight kümelerinin görüntülemek için. Kümeler işaretlenmiş **"Rol: okuyucu"** yalnızca salt okuyucu rolü izni vardır.

    ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-15.png)

3. Salt okuyucu rolü izni olan bir kümeye sağ tıklayın. Seçin **bu kümeye bağlantı** bağlamak için bağlam menüsünden küme. Ambari kullanıcı adı ve parola girin.

  
    ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-11.png)

4. HDInsight kümesi başarıyla bağlıysa, yenilenir.
   Aşama kümenin bağlı olur.
  
    ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>İşleri düğümünü genişleterek bağlantı kümesi

1. Tıklayın **işleri** düğümünün **küme iş erişim reddedildi.** penceresi açılır.
   
2. Tıklayın **bu kümeye bağlantı** kümesini bağlamak için.
   
    ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-9.png)

### <a name="link-cluster-from-rundebug-configurations-window"></a>Bağlantı kümesi Çalıştır/hata ayıklama yapılandırmaları penceresinden

1. HDInsight yapılandırması oluşturun. Ardından **uzaktan çalıştırmak kümesinde**.
   
2. Salt okuyucu rolü izni olan bir küme seçmeniz için **Spark kümeleri (yalnızca Linux)** . Çıkış, uyarı iletisi gösterilir. Tıklayabilirsiniz **bu kümeye bağlantı** kümesini bağlamak için.
   
   ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/create-config-1.png)
   
### <a name="view-storage-accounts"></a>Görünüm depolama hesapları

* Salt okuyucu rolü izni olan kümeler için tıklatın **depolama hesapları** düğümünün **depolama erişim reddedildi** penceresi açılır. Tıklayabilirsiniz **Azure Depolama Gezgini'ni açın** Depolama Gezgini'ni açın.
     
   ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-14.png)

   ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-10.png)

* Bağlı kümeler için tıklatın **depolama hesapları** düğümünün **depolama erişim reddedildi** penceresi açılır. Tıklayabilirsiniz **açın Azure depolama** Depolama Gezgini'ni açın.
     
   ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-13.png)

   ![HDInsight Spark kümeleri, Azure Gezgini](./media/apache-spark-intellij-tool-plugin/view-explorer-12.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Intellij için Azure Araç Seti kullanmak için Intellij Idea uygulamalara dönüştürün

Mevcut Spark Scala dönüştürebilirsiniz Intellij için Azure araç seti ile uyumlu olacak şekilde Intellij Idea'te oluşturduğunuz uygulamalar. Ardından eklenti uygulamaları bir HDInsight Spark kümesine göndermek için kullanabilirsiniz.

1. Intellij Idea oluşturulan bir mevcut Scala Spark uygulaması için ilişkili .iml dosyasını açın.

2. Kök dizininde düzeyidir bir **Modülü** öğesi aşağıdaki gibi:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Eklenecek öğenin Düzenle `UniqueKey="HDInsightTool"` böylece **Modülü** öğesi aşağıdaki gibi görünür:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Değişiklikleri kaydedin. Uygulamanız artık Intellij için Azure araç seti ile uyumlu olmalıdır. Projedeki proje adını sağ tıklayarak test edebilirsiniz. Açılır menü seçeneği artık etkin **gönderin, HDInsight için Spark uygulaması**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımlarla oluşturduğunuz kümeyi silin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. İçinde **arama** kutusunun üstündeki türü **HDInsight**.

1. Seçin **HDInsight kümeleri** altında **Hizmetleri**.

1. Görüntülenen listeden HDInsight kümelerinin, seçin **...**  yanında, Bu öğretici için oluşturduğunuz küme.

1. **Sil**’i seçin. Seçin **Evet**.

![HDInsight kümesini silme](./media/apache-spark-intellij-tool-plugin/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yazılan Apache Spark uygulamaları geliştirmek için eklenti Intellij için Azure Araç Seti kullanmayı öğrendiniz [Scala](https://www.scala-lang.org/)ve ardından bir HDInsight Spark kümesine doğrudan tümleştirilmiş olan Intellij gönderildi geliştirme ortamı (IDE). Power BI gibi bir BI analiz aracı Apache Spark, kayıtlı verileri nasıl çekilebilir görmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [BI araçlarını kullanarak verileri analiz etme](apache-spark-use-bi-tools.md)
