---
title: "Azure hdınsight'ta Spark Jupyter'de ile özel Maven paketleri kullanma | Microsoft Docs"
description: "Özel Maven paketlerini kullanmak için adım adım yönergeler Hdınsight Spark kümeleri ile Jupyter not defterleri kullanılabilir yapılandırmak."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: nitinme
ms.openlocfilehash: 71a64f3d23b495a3b00d36b1d4557425604a772d
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Hdınsight'ta Apache Spark kümeleri Jupyter not defterlerinde ile dış paketleri kullanma
> [!div class="op_single_selector"]
> * [Hücre Sihirli kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Betik eylemi kullanarak](apache-spark-python-package-installation.md)
>
>

Dış, topluluk katkıda bulunan kullanmak için hdınsight'ta Apache Spark kümesinde Jupyter not defteri yapılandırmayı öğrenin **maven** kümede olmayan paketler dahil Giden kutusu. 

Arama yapabilirsiniz [Maven depo](http://search.maven.org/) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketlerin listesini alabilirsiniz. Örneğin, tamamını topluluk katkıda bulunan paketlerin listesini adresten edinilebilir [Spark paketleri](http://spark-packages.org/).

Bu makalede, nasıl kullanılacağını öğreneceksiniz [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) Jupyter not defteri paketiyle.



## <a name="prerequisites"></a>Ön koşullar
Aşağıdakilere sahip olmanız gerekir:

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Jupyter not defterleri ile dış paketleri kullanma
1. [Azure Portal](https://portal.azure.com/)’daki başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Tüm** > **HDInsight Kümelerine Gözat** altından kümenize gidebilirsiniz.   
2. Spark kümesi dikey penceresinden **Hızlı Bağlantılar**’a ve sonra **Küme Panosu** dikey penceresinden **Jupyter Not Defteri**’ne tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

    > [!NOTE]
    > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. Yeni bir not defteri oluşturun. Tıklatın **yeni**ve ardından **Spark**.
   
    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Yeni bir Jupyter not defteri oluşturma")

4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.
   
    ![Not defteri adını belirtme](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Not defteri adını belirtme")

5. Kullanacağınız `%%configure` Sihirli bir dış paketini kullanmak üzere dizüstü bilgisayar yapılandırın. Dış paketleri kullanma defterlerinde çağırmanız emin olun `%%configure` birinci kod hücresini Sihirli. Bu, çekirdek oturum başlamadan önce paketini kullanmak için yapılandırıldığını sağlar.

    >[!IMPORTANT] 
    >Çekirdek ilk hücreye yapılandırmak unutursanız, kullanabileceğiniz `%%configure` ile `-f` parametresi, ancak oturumu başlatacak ve tüm ilerleme kaybolacak.

    | Hdınsight sürümü | Komut |
    |-------------------|---------|
    |Hdınsight 3.3 ve Hdınsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | Hdınsight 3.5 ve Hdınsight 3.6 için | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. Yukarıdaki kod parçacığında Maven merkezi bir depoya dış paketinde maven koordinatları bekliyor. Bu parçacığında bulunan `com.databricks:spark-csv_2.10:1.4.0` maven koordinatı olan **spark csv** paket. İşte bir paket için koordinatları nasıl oluşturun.
   
    a. Paket Maven deposunda bulun. Bu öğretici için kullandığımız [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Depodan değerlerini toplamak **GroupID**, **Artifactıd**, ve **sürüm**. Toplamanız değerleri kümenizi eşleştiğinden emin olun. Bu durumda, bir Scala 2.10 ve Spark 1.4.0 paketini kullanıyoruz, ancak uygun Scala için farklı sürümleri veya Spark sürüm kümenizdeki seçmeniz gerekebilir. Out Scala sürüm kümenizde çalıştırarak bulabileceğiniz `scala.util.Properties.versionString` Spark Jupyter çekirdek veya Spark Gönder. Out Spark sürüm kümenizde çalıştırarak bulabileceğiniz `sc.version` Jupyter Not.
   
    ![Jupyter not defteri ile dış paketleri kullanma](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Jupyter not defteri ile dış paketleri kullanma")
   
    c. Virgülle ayrılmış üç değerden birleştirme (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

7. Kod hücreyle çalıştırmak `%%configure` Sihirli. Bu, sağladığınız paketini kullanmak için temel alınan Livy oturum yapılandırır. Not Defteri sonraki hücrelerde, aşağıda gösterildiği gibi paketin artık kullanabilirsiniz.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    Hdınsight 3.6 için aşağıdaki kod parçacığını kullanmanız gerekir.

        val df = spark.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. Kod parçacıkları gibi ardından çalıştırın önceki adımda oluşturduğunuz dataframe verileri görüntülemek için aşağıda gösterilen.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Hdınsight Linux üzerinde Apache Spark kümeleri Jupyter not defterlerinde ile dış python paketlerini kullanma](apache-spark-python-package-installation.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

