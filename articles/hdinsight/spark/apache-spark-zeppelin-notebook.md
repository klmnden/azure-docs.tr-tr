---
title: Azure hdınsight'ta Apache Spark kümesinde ile Zeppelin not defterlerini kullanma | Microsoft Docs
description: Azure hdınsight'ta Apache Spark kümeleri ile Zeppelin not defterlerini kullanma hakkında adım adım yönergeler.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: b2f47dce058af7a39366c06d0b33117a66ed116a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Azure hdınsight'ta Apache Spark kümesinde ile Zeppelin not defterlerini kullanma

Hdınsight Spark kümeleri Spark işlerini çalıştırmak için kullanabileceğiniz Zeppelin not defterlerini içerir. Bu makalede bir Hdınsight kümesine Zeppelin not defteri kullanmayı öğrenin.

> [!NOTE]
> Zeppelin not defterlerini yalnızca 1.6.3 Spark Hdınsight 3.5 ve 2.1.0 Spark Hdınsight 3.6 için kullanılabilir.
>

**Ön koşullar:**

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Bir Zeppelin not defteri Başlat
1. Spark kümesi dikey penceresinden tıklatın **küme Panosu**ve ardından **Zeppelin not defteri**. İstenirse, küme için yönetici kimlik bilgilerini girin.
   
   > [!NOTE]
   > Tarayıcınızda aşağıdaki URL'yi açarak kümeniz için Zeppelin Not Defteri de ulaşabilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Yeni bir not defteri oluşturun. Üstbilgi Bölmesi'nden tıklatın **not defteri**ve ardından **oluşturma Yeni Not**.
   
    ![Yeni bir Zeppelin not defteri oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "yeni bir Zeppelin not defteri oluşturma")
   
    Dizüstü bilgisayar için bir ad girin ve ardından **oluşturma Not**.
3. Ayrıca, Not Defteri Başlığı bağlı durumu gösterdiğinden emin olun. Sağ üst köşedeki yeşil bir nokta olarak belirtilir.
   
    ![Zeppelin not defteri durumu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin not defteri durumu")
4. Örnek verilerini geçici bir tabloya yükleyin. Örnek veri dosyası olan Hdınsight'ta Spark kümesi oluşturduğunuzda **hvac.csv**, altındaki ilişkili depolama hesabına kopyalanır **\HdiSamples\SensorSampleData\hvac**.
   
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
   
    Tuşuna **SHIFT + ENTER** veya tıklatın **yürütmek** kod parçacığında çalıştırmak paragraf düğmesi. Paragraf sağ köşesindeki durumu, hazır BEKLEYEN, ÇALIŞTIĞINDAN tamamlandı kadar ilerleme. Aynı paragraf alt kısmındaki çıkış gösterir. Ekran aşağıdaki gibi görünür:
   
    ![Ham verileri geçici bir tablo oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "ham verilerden geçici bir tablo oluştur")
   
    Ayrıca, her paragraf başlık sağlayabilir. Sağ köşesinden tıklatın **ayarları** simgesine ve ardından **Göster başlık**.
5. Üzerinde şimdi Spark SQL deyimlerini çalışabilir **hvac** tablo. Aşağıdaki sorgu, yeni bir paragraf yapıştırın. Sorgu oluşturma kimliği ve her belirli bir tarihte oluşturmaya yönelik gerçek etme ve hedef arasındaki farkı alır. Tuşuna **SHIFT + ENTER**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    **% Sql** başında deyimi Livy Scala yorumlayıcı kullanmak için Not Defteri söyler.
   
    Aşağıdaki ekran görüntüsünde çıktısını gösterir.
   
    ![Not Defteri kullanarak Spark SQL deyimini çalıştırın](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Not Defteri kullanarak Spark SQL deyimini çalıştırın")
   
     Aynı çıktı için farklı sunumu arasında geçiş yapmak için (dikdörtgende vurgulanan) görüntüleme seçeneklerini tıklatın. Tıklatın **ayarları** hangi consitutes çıktıda anahtarı ve değerleri seçin. Yukarıdaki ekran yakalama kullanan **buildingID** anahtar ve ortalama olarak **temp_diff** değeri olarak.
6. Sorguda değişkenler kullanarak Spark SQL deyimlerini de çalıştırabilirsiniz. Sonraki kod parçacığını bir değişkeni tanımlamak gösterilmiştir **Temp**, sorgudaki ile sorgulamak istediğiniz olası değerler ile. Sorgu ilk kez çalıştırdığınızda, bir açılan değişken için belirtilen değerlerle otomatik olarak doldurulur.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Yeni bir paragraf ve tuşuna bu parçacığını yapıştırın **SHIFT + ENTER**. Aşağıdaki ekran görüntüsünde çıktısını gösterir.
   
    ![Not Defteri kullanarak Spark SQL deyimini çalıştırın](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Not Defteri kullanarak Spark SQL deyimini çalıştırın")
   
    Sonraki sorgular için açılan listeden yeni bir değer seçin ve sorguyu yeniden çalıştırın. Tıklatın **ayarları** hangi consitutes çıktıda anahtarı ve değerleri seçin. Yukarıdaki ekran yakalama kullanan **buildingID** ortalamasını anahtar olarak **temp_diff** değeri olarak ve **targettemp** grup olarak.
7. Uygulamadan çıkmak için Livy yorumlayıcı yeniden başlatın. Bunu yapmak için oturum açan kullanıcı adını sağ üst köşesinden tıklayarak yorumlayıcı Ayarları'nı açın ve ardından **yorumlayıcı**.
   
    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıktı")
8. Livy yorumlayıcı ayarlar'e gidin ve ardından **yeniden**.
   
    ![Livy intepreter yeniden](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin intepreter yeniden başlatın")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Dış paketler not defteri ile nasıl kullanabilirim?
Dahil edilen out-of--box kümedeki olmayan dış, topluluk katkıda bulunan paketlerini kullanmak için Zeppelin Not Defteri (Linux) hdınsight'ta Apache Spark kümesinde yapılandırabilirsiniz. Arama yapabilirsiniz [Maven depo](http://search.maven.org/) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketlerin listesini alabilirsiniz. Örneğin, tamamını topluluk katkıda bulunan paketlerin listesini adresten edinilebilir [Spark paketleri](http://spark-packages.org/).

Bu makalede, nasıl kullanılacağını görürsünüz [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) Jupyter not defteri paketiyle.

1. Yorumlayıcı Ayarları'nı açın. Sağ üst köşesinden oturum açan kullanıcı adına tıklayın ve ardından **yorumlayıcı**.
   
    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıktı")
2. Livy yorumlayıcı ayarlar'e gidin ve ardından **Düzenle**.
   
    ![Yorumlayıcı ayarları değiştirme](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "yorumlayıcı ayarlarını değiştir")
3. Yeni bir anahtar ekleyin adlı **livy.spark.jars.packages** ve biçiminde değerini ayarlama `group:id:version`. Bunu kullanmak istiyorsanız, [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketi, anahtar değerini ayarlamanız gerekir `com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Yorumlayıcı ayarları değiştirme](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "yorumlayıcı ayarlarını değiştir")
   
    Tıklatın **kaydetmek** ve Livy yorumlayıcı yeniden başlatın.
4. **İpucu**: adresindeki gelmesi öğrenmek istiyorsanız, anahtarın değerini Yukarıda girilen, burada nasıl.
   
    a. Paket Maven deposunda bulun. Bu öğretici için kullandık [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Depodan değerlerini toplamak **GroupID**, **Artifactıd**, ve **sürüm**.
   
    ![Jupyter not defteri ile dış paketleri kullanma](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Jupyter not defteri ile dış paketleri kullanma")
   
    c. Virgülle ayrılmış üç değerden birleştirme (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Zeppelin not defterlerini kaydedildiği?
Zeppelin not defterlerini küme headnodes kaydedilir. Bu nedenle, kümeyi silmeniz halinde, dizüstü bilgisayarlar da silinir. Diğer kümelerinde daha sonra kullanmak için not defterlerinizi korumak istiyorsanız, bir iş çalışmadığı bitirdikten sonra vermeniz gerekir. Bir not defteri vermek için tıklatın **verme** aşağıdaki resimde gösterildiği gibi simgesi.

![Not Defteri karşıdan](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "not defteri indirin")

Bu, Not Defteri yükleme konumunuzu bir JSON dosyası olarak kaydeder.

## <a name="livy-session-management"></a>Livy oturum yönetimi
İlk kod paragraf Zeppelin not defterinde çalıştırdığınızda, yeni bir Livy oturum Hdınsight Spark kümenizin oluşturulur. Bu oturumda daha sonra oluşturduğunuz tüm Zeppelin not defterlerini arasında paylaşılır. (Küme yeniden başlatma, vb.) herhangi bir nedenden dolayı oturum Livy sonlandırıldı, işleri Zeppelin dizüstü bilgisayarınızı çalıştırabilir ve olmaz.

Böyle bir durumda, Zeppelin not defteri çalışan işler başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir. 

1. Livy yorumlayıcı Zeppelin not defteri gelen yeniden başlatın. Bunu yapmak için oturum açan kullanıcı adını sağ üst köşesinden tıklayarak yorumlayıcı Ayarları'nı açın ve ardından **yorumlayıcı**.
   
    ![Yorumlayıcı başlatma](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıktı")
2. Livy yorumlayıcı ayarlar'e gidin ve ardından **yeniden**.
   
    ![Livy intepreter yeniden](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin intepreter yeniden başlatın")
3. Kod hücresini varolan bir Zeppelin not defterini ' çalıştırın. Bu, Hdınsight kümesinde yeni bir Livy oturum oluşturur.

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







