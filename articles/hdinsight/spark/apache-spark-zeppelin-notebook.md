---
title: Azure HDInsight üzerinde Apache Spark kümesi ile Zeppelin not defterlerini kullanma
description: Azure HDInsight üzerinde Apache Spark kümeleri ile Zeppelin not defterlerini kullanma konusunda adım adım yönergeler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/04/2019
ms.openlocfilehash: 219cdeea228ae3e334213a0f0654f904592cb09e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448734"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Azure HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma

HDInsight Spark kümeleri içerir [Apache Zeppelin](https://zeppelin.apache.org/) çalıştırmak için kullanabileceğiniz not defterlerini [Apache Spark](https://spark.apache.org/) işler. Bu makalede, bir HDInsight kümesinde Zeppelin not defteri kullanmayı öğrenin.

**Ön koşullar:**

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Birincil depolama alanınızı kümeleri için kullanılacak URI düzeni. Bu `wasb://` Azure Blob Depolama için `abfs://` için Azure Data Lake depolama Gen2'ye veya `adl://` Azure Data Lake depolama Gen1 için. Güvenli aktarım için Blob Depolama veya Data Lake depolama Gen2 etkinse, URI olacaktır `wasbs://` veya `abfss://`sırasıyla.  Ayrıca bkz [Azure depolamadaki güvenli aktarım gerektir](../../storage/common/storage-require-secure-transfer.md) daha fazla bilgi için.

## <a name="launch-an-apache-zeppelin-notebook"></a>Bir Apache Zeppelin not defteri başlatma

1. Spark kümesinden **genel bakış**seçin **Zeppelin not defteri** gelen **küme panoları**. Küme için yönetici kimlik bilgilerini girin.  

   > [!NOTE]  
   > Zeppelin not defteri kümeniz için aşağıdaki URL'yi tarayıcınızda açarak da ulaşabilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Yeni bir not defteri oluşturun. Üstbilgi Bölmesi'nden gidin **not defteri** > **oluştur Yeni Not**.

    ![Yeni bir Zeppelin not defteri oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "yeni Zeppelin not defteri oluşturma")

    Not defteri için bir ad girin ve ardından **oluşturma Not**.

3. Not Defteri Başlığı bağlı durumunun emin olun. Sağ üst köşedeki yeşil bir noktayı tarafından belirtilir.

    ![Zeppelin not defteri durumu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin not defteri durumu")

4. Örnek verilerini geçici bir tabloya yükleyin. HDInsight, örnek veri dosyasını bir Spark kümesi oluşturduğunuzda `hvac.csv`, altındaki ilişkili depolama hesabına kopyalanır `\HdiSamples\SensorSampleData\hvac`.

    Yeni not defterinde varsayılan olarak oluşturulan boş paragraf içinde aşağıdaki kod parçacığını yapıştırın.

    ```scala
    %livy2.spark
    //The above magic instructs Zeppelin to use the Livy Scala interpreter

    // Create an RDD using the default Spark context, sc
    val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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
    ```

    Tuşuna **SHIFT + ENTER** veya **Play** kod parçacığında çalıştırılacak paragraf düğmesi. Paragrafın sağ üst köşedeki durumu, hazır, BEKLİYOR, çalışıyor Tamamlandı'den ilerleme. Aynı paragraf alt kısmındaki çıkış gösterir. Ekran aşağıdaki gibi görünür:

    ![Ham verilerden geçici bir tablo oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "ham verilerden geçici bir tablo oluşturma")

    Ayrıca, her bir paragrafına başlık sağlayabilir. Paragraf sağ Köşeden seçin **ayarları** (sprocket) simgesine ve ardından **başlığı göster**.  

    > [!NOTE]  
    > % spark2 yorumlayıcı desteklenmiyor Zeppelin not defterlerinde tüm HDInsight sürümleri ve % arasında sh yorumlayıcı desteklenmeyecek HDInsight 4.0 ve üzeri.

5. Artık Spark SQL deyimleri çalıştırabilirsiniz `hvac` tablo. Yeni bir paragrafa aşağıdaki sorguyu yapıştırın. Sorgu, yapı kimliği ve her biri belirli bir tarihte oluşturmak için gerçek Sıcaklıkların ve hedef arasındaki farkı alır. Tuşuna **SHIFT + ENTER**.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
    ```  

    **% Sql** deyimi başında Livy Scala yorumlayıcı kullanmak için Not defterini söyler.

6. Seçin **çubuk grafik** ekranı değiştirmek için simge.  **ayarları**, seçtikten sonra görünen **çubuk grafik**, seçmenizi sağlar **anahtarları**, ve **değerleri**.  Çıktı aşağıdaki ekran görüntüsünde gösterilmiştir.

    ![Not Defteri kullanarak Spark SQL deyimi çalıştırma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Not Defteri kullanarak Spark SQL deyimi çalıştırma")

7. Sorgu değişkenleri kullanarak Spark SQL deyimi de çalıştırabilirsiniz. Sonraki kod parçacığında, bir değişkeni tanımlamak gösterilmektedir `Temp`, olası değerler ile istediğiniz ile sorgulamak sorgu. Sorguyu ilk kez çalıştırdığınızda, bir açılan değişkeni için belirtilen değerleri otomatik olarak doldurulur.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Bu kod parçacığını yapıştırın bir yeni bir paragrafa ve ENTER tuşuna **SHIFT + ENTER**. Ardından **65** gelen **Temp** açılan listesi. 

8. Seçin **çubuk grafik** ekranı değiştirmek için simge.  Ardından **ayarları** ve aşağıdaki değişiklikleri yapın:

   * **Grupları:**  Ekleme **targettemp**.  
   * **Değerler:** 1. Kaldırma **tarih**.  2. Ekleme **temp_diff**.  3.  Toplayıcısı'ndan değiştirme **toplam** için **ortalama**.  

     Çıktı aşağıdaki ekran görüntüsünde gösterilmiştir.

     ![Not Defteri kullanarak Spark SQL deyimi çalıştırma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Not Defteri kullanarak Spark SQL deyimi çalıştırma")

9. Uygulamadan çıkmak için Livy yorumlayıcısını yeniden başlatın. Bunu yapmak için oturum açmış kullanıcı adına sağ üst köşedeki seçerek yorumlayıcı Ayarları'nı açın ve ardından **yorumlayıcı**.  

    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkış")

10. Kaydırma **livy**ve ardından **yeniden**.  Seçin **Tamam** isteminde.

    ![Livy intepreter yeniden](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin intepreter yeniden başlatın")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Not Defteri ile dış paketleri nasıl kullanabilirim?
Dahil edilen çıkış,-hazır kümede olmayan harici, topluluk tarafından katkıda bulunulan paketlerini kullanmak için HDInsight üzerinde Apache Spark kümesinde Zeppelin not defteri yapılandırabilirsiniz. Arama yapabilirsiniz [Maven deposu](https://search.maven.org/) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketler listesini alabilirsiniz. Örneğin, topluluk tarafından katkıda bulunulan paketlerin tam bir listesi kullanılabilir [Spark paketleri](https://spark-packages.org/).

Bu makalede, nasıl kullanılacağını görürsünüz [spark csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) Jupyter not defteri ile paket.

1. Yorumlayıcı ayarlarını açın. Sağ üst köşedeki oturum açmış kullanıcı adını seçin ve ardından **yorumlayıcı**.

    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkış")

2. Kaydırma **livy**, ardından **Düzenle**.

    ![Yorumlayıcı Ayarları Değiştir](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "yorumlayıcı ayarlarını değiştir")

3. Adlı yeni bir anahtar ekleyin `livy.spark.jars.packages`ve değerini ayarlama biçiminde `group:id:version`. Bunu kullanmak isterseniz [spark csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketi anahtarının değerini ayarlamalısınız `com.databricks:spark-csv_2.10:1.4.0`.

    ![Yorumlayıcı Ayarları Değiştir](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "yorumlayıcı ayarlarını değiştir")

    Seçin **Kaydet** ve Livy yorumlayıcısını yeniden başlatın.

4. Ulaşırsınız nasıl anlamak istiyorsanız anahtarının değerini Yukarıda girilen, burada nasıl.
   
    a. Paket Maven depoda bulun. Bu makalede kullandığımız [spark csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Depodan değerlerini toplamak **GroupID**, **Artifactıd**, ve **sürüm**.
   
    ![Jupyter not defteri ile dış paketleri kullanma](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Jupyter not defteri ile dış paketleri kullanma")
   
    c. Virgül ile ayrılmış üç değerleri birleştirebilir ( **:** ).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Zeppelin not defterlerini kaydedildiği?
Zeppelin not defterlerini küme baş düğümüne kaydedilir. Bu nedenle, kümeyi silmeniz halinde, not defterlerini de silinir. Diğer kümelerinde daha sonra kullanmak için not defterlerinizi korumak istiyorsanız, işleri çalıştırma tamamladıktan sonra vermeniz gerekir. Bir not defteri vermek için seçin **dışarı** aşağıdaki resimde gösterildiği gibi simgesi.

![Not Defteri indirme](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "not defterini indirin")

Bu, Not Defteri, indirme konumuna bir JSON dosyası olarak kaydeder.

## <a name="livy-session-management"></a>Livy oturum yönetimi
Zeppelin not defterinin ilk kod paragraf çalıştırdığınızda, yeni bir Livy oturumu HDInsight Spark kümenizde oluşturulur. Bu oturumda, daha sonra oluşturduğunuz tüm Zeppelin not defterlerini arasında paylaşılır. Herhangi bir nedenden dolayı oturum Livy sonlandırılan varsa (küme yeniden başlatma, vb.), Zeppelin not defterinden işlerini çalıştırmak mümkün olmayacaktır.

Böyle bir durumda Zeppelin not defterinden çalışan işler başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir.  

1. Zeppelin not defterindeki Livy yorumlayıcısını yeniden başlatın. Bunu yapmak için oturum açmış kullanıcı adına sağ üst köşedeki seçerek yorumlayıcı Ayarları'nı açın ve ardından **yorumlayıcı**.

    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkış")

2. Kaydırma **livy**, ardından **yeniden**.

    ![Livy intepreter yeniden](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin intepreter yeniden başlatın")

3. Mevcut bir Zeppelin not defterinden bir kod hücresi çalıştırın. Bu, HDInsight kümesinde yeni bir Livy oturumu oluşturur.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Oluşturmak ve Apache Spark Scala uygulamaları göndermek amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md 
