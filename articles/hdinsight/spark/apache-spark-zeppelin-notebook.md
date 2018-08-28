---
title: Azure HDInsight üzerinde Apache Spark kümesi ile Zeppelin not defterlerini kullanma
description: Azure HDInsight üzerinde Apache Spark kümeleri ile Zeppelin not defterlerini kullanma konusunda adım adım yönergeler.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.openlocfilehash: 9b076709ee24c61b2699672d28bd61204c88a744
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048050"
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Azure HDInsight üzerinde Apache Spark kümesi ile Zeppelin not defterlerini kullanma

HDInsight Spark kümeleri, Spark işlerini çalıştırmak için kullanabileceğiniz Zeppelin not defterlerini içerir. Bu makalede, bir HDInsight kümesinde Zeppelin not defteri kullanmayı öğrenin.

> [!NOTE]
> Zeppelin not defterlerini yalnızca HDInsight 3.5 üzerinde Spark 1.6.3 ve HDInsight 3.6 üzerinde Spark 2.1.0 için kullanılabilir.
>

**Ön koşullar:**

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Zeppelin not defteri başlatma
1. Spark kümesi dikey penceresinden tıklayın **küme Panosu**ve ardından **Zeppelin not defteri**. İstenirse, küme için yönetici kimlik bilgilerini girin.
   
   > [!NOTE]
   > Zeppelin not defteri kümeniz için aşağıdaki URL'yi tarayıcınızda açarak da ulaşabilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
1. Yeni bir not defteri oluşturun. Üstbilgi Bölmesi'nden tıklayın **not defteri**ve ardından **oluşturma Yeni Not**.
   
    ![Yeni bir Zeppelin not defteri oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "yeni Zeppelin not defteri oluşturma")
   
    Not defteri için bir ad girin ve ardından **oluşturma Not**.
1. Ayrıca, Not Defteri Başlığı bağlı durumu göründüğünden emin olun. Sağ üst köşedeki yeşil bir noktayı tarafından belirtilir.
   
    ![Zeppelin not defteri durumu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin not defteri durumu")
1. Örnek verilerini geçici bir tabloya yükleyin. HDInsight, örnek veri dosyasını bir Spark kümesi oluşturduğunuzda **hvac.csv**, altındaki ilişkili depolama hesabına kopyalanır **\HdiSamples\SensorSampleData\hvac**.
   
    Yeni not defterinde varsayılan olarak oluşturulan boş paragraf içinde aşağıdaki kod parçacığını yapıştırın.
   
        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Tuşuna **SHIFT + ENTER** veya **Play** kod parçacığında çalıştırılacak paragraf düğmesi. Paragrafın sağ üst köşedeki durumu, hazır, BEKLİYOR, çalışıyor Tamamlandı'den ilerleme. Aynı paragraf alt kısmındaki çıkış gösterir. Ekran aşağıdaki gibi görünür:
   
    ![Ham verilerden geçici bir tablo oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "ham verilerden geçici bir tablo oluşturma")
   
    Ayrıca, her bir paragrafına başlık sağlayabilir. Sağ taraftaki köşesdeki **ayarları** simgesine ve ardından **başlığı göster**.
1. Artık Spark SQL deyimleri çalıştırabilirsiniz **hvac** tablo. Yeni bir paragrafa aşağıdaki sorguyu yapıştırın. Sorgu, yapı kimliği ve her biri belirli bir tarihte oluşturmak için gerçek Sıcaklıkların ve hedef arasındaki farkı alır. Tuşuna **SHIFT + ENTER**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    **% Sql** deyimi başında Livy Scala yorumlayıcı kullanmak için Not defterini söyler.
   
    Çıktı aşağıdaki ekran görüntüsünde gösterilmiştir.
   
    ![Not Defteri kullanarak Spark SQL deyimi çalıştırma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Not Defteri kullanarak Spark SQL deyimi çalıştırma")
   
     Aynı çıktı için farklı temsilleri arasında geçiş yapmak için görüntüleme seçeneklerini (vurgulu dikdörtgen içinde) tıklayın. Tıklayın **ayarları** çıktısında ne consitutes anahtarı ve değerleri seçmek için. Yukarıdaki ekran görüntüsü yakalamayı kullanan **buildingID** anahtar ve ortalama olarak **temp_diff** değeri.
1. Sorgu değişkenleri kullanarak Spark SQL deyimi de çalıştırabilirsiniz. Sonraki kod parçacığında, bir değişkeni tanımlamak gösterilmektedir **Temp**, olası değerler ile istediğiniz ile sorgulamak sorgu. Sorguyu ilk kez çalıştırdığınızda, bir açılan değişkeni için belirtilen değerleri otomatik olarak doldurulur.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Bu kod parçacığını yapıştırın bir yeni bir paragrafa ve ENTER tuşuna **SHIFT + ENTER**. Çıktı aşağıdaki ekran görüntüsünde gösterilmiştir.
   
    ![Not Defteri kullanarak Spark SQL deyimi çalıştırma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Not Defteri kullanarak Spark SQL deyimi çalıştırma")
   
    Sonraki sorgular için açılan listeden yeni bir değer seçin ve sorguyu yeniden çalıştırın. Tıklayın **ayarları** çıktısında ne consitutes anahtarı ve değerleri seçmek için. Yukarıdaki ekran görüntüsü yakalamayı kullanan **buildingID** ortalamasını anahtarı **temp_diff** değer olarak ve **targettemp** grup olarak.
1. Uygulamadan çıkmak için Livy yorumlayıcısını yeniden başlatın. Bunu yapmak için oturum açmış kullanıcı adına sağ üst köşedeki tıklayarak yorumlayıcı Ayarları'nı açın ve ardından **yorumlayıcı**.
   
    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkış")
1. Livy yorumlayıcı ayarlarına gidin ve ardından **yeniden**.
   
    ![Livy intepreter yeniden](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin intepreter yeniden başlatın")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Not Defteri ile dış paketleri nasıl kullanabilirim?
Dahil edilen çıkış,-hazır kümede olmayan harici, topluluk tarafından katkıda bulunulan paketlerini kullanmak için (Linux) HDInsight üzerinde Apache Spark kümesinde Zeppelin not defteri yapılandırabilirsiniz. Arama yapabilirsiniz [Maven deposu](http://search.maven.org/) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketler listesini alabilirsiniz. Örneğin, topluluk tarafından katkıda bulunulan paketlerin tam bir listesi kullanılabilir [Spark paketleri](http://spark-packages.org/).

Bu makalede, nasıl kullanılacağını görürsünüz [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) Jupyter not defteri ile paket.

1. Yorumlayıcı ayarlarını açın. Sağ üst köşedeki oturum açmış kullanıcı adına tıklayın ve ardından **yorumlayıcı**.
   
    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkış")
1. Livy yorumlayıcı ayarlarına gidin ve ardından **Düzenle**.
   
    ![Yorumlayıcı Ayarları Değiştir](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "yorumlayıcı ayarlarını değiştir")
1. Yeni bir anahtar ekleyin adlı **livy.spark.jars.packages** ve değerini ayarlama biçiminde `group:id:version`. Bunu kullanmak isterseniz [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketi anahtarının değerini ayarlamalısınız `com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Yorumlayıcı Ayarları Değiştir](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "yorumlayıcı ayarlarını değiştir")
   
    Tıklayın **Kaydet** ve Livy yorumlayıcısını yeniden başlatın.
1. **İpucu**: ulaşırsınız nasıl anlamak istiyorsanız anahtarının değerini Yukarıda girilen, burada nasıl.
   
    a. Paket Maven depoda bulun. Bu öğretici için kullandığımız [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Depodan değerlerini toplamak **GroupID**, **Artifactıd**, ve **sürüm**.
   
    ![Jupyter not defteri ile dış paketleri kullanma](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Jupyter not defteri ile dış paketleri kullanma")
   
    c. Virgül ile ayrılmış üç değerleri birleştirebilir (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Zeppelin not defterlerini kaydedildiği?
Zeppelin not defterlerini küme baş düğümüne kaydedilir. Bu nedenle, kümeyi silmeniz halinde, not defterlerini de silinir. Diğer kümelerinde daha sonra kullanmak için not defterlerinizi korumak istiyorsanız, işleri çalıştırma tamamladıktan sonra vermeniz gerekir. Bir not defteri dışarı aktarmak için tıklayın **dışarı** aşağıdaki resimde gösterildiği gibi simgesi.

![Not Defteri indirme](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "not defterini indirin")

Bu, Not Defteri, indirme konumuna bir JSON dosyası olarak kaydeder.

## <a name="livy-session-management"></a>Livy oturum yönetimi
Zeppelin not defterinin ilk kod paragraf çalıştırdığınızda, yeni bir Livy oturumu HDInsight Spark kümenizde oluşturulur. Bu oturumda, daha sonra oluşturduğunuz tüm Zeppelin not defterlerini arasında paylaşılır. Herhangi bir nedenden dolayı oturum Livy sonlandırılan varsa (küme yeniden başlatma, vb.), Zeppelin not defterinden işlerini çalıştırmak mümkün olmayacaktır.

Böyle bir durumda Zeppelin not defterinden çalışan işler başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir. 

1. Zeppelin not defterindeki Livy yorumlayıcısını yeniden başlatın. Bunu yapmak için oturum açmış kullanıcı adına sağ üst köşedeki tıklayarak yorumlayıcı Ayarları'nı açın ve ardından **yorumlayıcı**.
   
    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkış")
1. Livy yorumlayıcı ayarlarına gidin ve ardından **yeniden**.
   
    ![Livy intepreter yeniden](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin intepreter yeniden başlatın")
1. Mevcut bir Zeppelin not defterinden bir kod hücresi çalıştırın. Bu, HDInsight kümesinde yeni bir Livy oturumu oluşturur.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md 







