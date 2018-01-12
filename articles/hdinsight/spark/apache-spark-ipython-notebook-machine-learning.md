---
title: "Azure hdınsight'ta Apache Spark machine learning uygulamaları derleme | Microsoft Docs"
description: "Apache Spark machine learning uygulama oluşturmak adım adım yönergeler Jupyter Not Defteri kullanarak Spark Hdınsight kümeleri"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: 22a3d220966fef77e131fbeb3ea46a1f81a9ada5
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Azure hdınsight'ta Apache Spark machine learning uygulamaları derleme

Hdınsight'ta Spark kümesi kullanarak uygulama öğrenme bir Apache Spark makine oluşturmayı öğrenin. Bu makalede kullanılabilir Jupyter not defteri ile küme oluşturmak ve bu uygulamayı test için nasıl kullanılacağı gösterilmektedir. Uygulama, varsayılan olarak tüm kümelerde kullanılabilir örnek HVAC.csv verileri kullanır.

**Ön koşullar:**

Aşağıdakilere sahip olmanız gerekir:

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Veri kümesi anlama
Uygulama oluşturmaya başlamadan önce bize, biz uygulama gerçekleştiririz analiz türünü verileri yapı ve verilerin yapısını anlayın. 

Bu makaledeki örnek kullanırız **HVAC.csv** Hdınsight kümesi ile ilişkili Azure depolama hesabı kullanılabilir veri dosyası. Depolama hesabında dosya altındadır **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Karşıdan yükle ve verilerin bir anlık görüntüsünü almak için CSV dosyasını açın.  

![Spark machine learning örnek için kullanılan verilerin anlık görüntüsünü](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Spark machine learning örnek için kullanılan verilerin anlık görüntüsünü")

Verileri hedef sıcaklık ve gerçek HVAC sistemleri yüklü olan bir bina sıcaklığını gösterir. Varsayalım **sistem** sütun sistem Kimliğini temsil eder ve **SystemAge** sütun HVAC sistem yapı yerinde açıldı yıl sayısını temsil eder.

Bir yapı sistem Kimliğini ve sistem yaş verilen hedef sıcaklık hotter veya colder tabanlı olup olmayacağını tahmin etmek için bu verileri kullanırız.

## <a name="app"></a>Spark Mllib'i kullanarak Spark machine learning uygulaması yazma
Bu uygulamada bir belge sınıflandırması yapmak için Spark ML işlem hattı kullanın. Ardışık düzeninde biz sözcüklere belgeyi bölme, bir sayısal özellik vektör sözcükleri dönüştürme ve son olarak özellik vektörlerinin ve etiketleri kullanarak bir tahmin modeli yapı. Uygulama oluşturmak için aşağıdaki adımları gerçekleştirin.

1. [Azure Portal](https://portal.azure.com/)’daki başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   
2. Spark kümesi dikey penceresinden **Küme Panosu**’na ve ardından **Jupyter Notebook**’a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.
   
   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.
   
    ![Spark machine learning örneğin Jupyter not defteri oluşturma](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Spark machine learning örneğin Jupyter not defteri oluşturma")
4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.
   
    ![Spark machine learning örnek için bir not defteri ad](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Spark machine learning örneğin Not defteri adını belirtin")
5. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Boş bir hücreye aşağıdaki kod parçacığını yapıştırın ve sonra basın **SHIFT + ENTER**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Şimdi (hvac.csv) veri yükleme, bunu ayrıştırabilir ve bunu modeli eğitmek için kullanmanız gerekir. Bu, gerçek bina sıcaklığını hedef sıcaklık büyük olup olmadığını denetler bir işlev tanımlayın. Gerçek sıcaklık büyükse, yapı değeri tarafından belirtilen hareketli, **1.0**. Gerçek sıcaklık küçük yapı değeri tarafından belirtilen soğuk, ise, **0,0**. 
   
    Aşağıdaki kod parçacığını yapıştırın bir boş bir hücre ve tuşuna **SHIFT + ENTER**.

        # List the structure of data for better understanding. Because the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Üç aşamadan oluşur Spark machine learning ardışık düzenini yapılandırın: belirteç Oluşturucu, hashingTF ve lr. İşlem hattı nedir ve bakın nasıl çalıştığı hakkında daha fazla bilgi için <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning ardışık</a>.
   
    Aşağıdaki kod parçacığını yapıştırın bir boş bir hücre ve tuşuna **SHIFT + ENTER**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Ardışık Düzen eğitim belgeye uygun. Aşağıdaki kod parçacığını yapıştırın bir boş bir hücre ve tuşuna **SHIFT + ENTER**.
   
        model = pipeline.fit(training)
3. Denetim noktası eğitim belgeye uygulamayla ilerleme durumunuzu doğrulayın. Aşağıdaki kod parçacığını yapıştırın bir boş bir hücre ve tuşuna **SHIFT + ENTER**.
   
        training.show()
   
    Bu, aşağıdakine benzer bir çıktı vermeniz gerekir:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Geri dönün ve çıktı ham CSV dosyası karşı doğrulayın. Örneğin, CSV dosyasının ilk satırı bu verileri vardır:

    ![Çıkış Spark machine learning örnek için anlık görüntü verileri](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Spark machine learning örnek için çıktı veri anlık görüntüsü")

    Nasıl gerçek sıcaklık yapı soğuk öneren hedef sıcaklık değerinden olduğuna dikkat edin. Bu nedenle çıktı, eğitim değeri **etiket** ilk sırada **0,0**, yapı başka bir deyişle, etkin değil.

1. Bir veri kümesi ve eğitilen modele karşı çalıştırmak için hazırlayın. Bunu yapmak için kimliğinizi bir sistem Kimliğini ve sistem yaş geçip geçmeyeceğini (olarak gösterilen **SystemInfo** eğitim çıkışı), ve model oluşturma, sistem Kimliğini ve sistem geçerlilik süresi ile hotter olup tahmin etmek (1.0 tarafından gösterilen) veya daha soğuk (belirtilmiştir 0,0).
   
   Aşağıdaki kod parçacığını yapıştırın bir boş bir hücre ve tuşuna **SHIFT + ENTER**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Son olarak, test verileri üzerindeki tahminlerde. Aşağıdaki kod parçacığını yapıştırın bir boş bir hücre ve tuşuna **SHIFT + ENTER**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Aşağıdakine benzer bir çıktı görmeniz gerekir:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   Tahmin ilk satırdan kimliği 20 ve 25 yıllık sistem geçerlilik süresi ile bir HVAC sistemi için yapı etkin olacağını görebilirsiniz (**tahmin = 1.0**). DenseVector (0.49999) ilk değeri 0,0 tahmin karşılık gelir ve ikinci değer (0.5001) 1.0 tahmin karşılık gelir. Çıktıda ikinci değer yalnızca fazladır daha yüksek olmasına rağmen modelini gösteren **tahmin = 1.0**.
4. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.

## <a name="anaconda"></a>Anaconda scikit kullanın-Spark machine learning için kitaplık öğrenin
Hdınsight'ta Apache Spark kümeleri Anaconda kitaplıkları içerir. Bu da içerir **scikit-öğrenin** machine learning için kitaplık. Kitaplık Ayrıca örnek uygulamalardan doğrudan Jupyter not defteri oluşturmak için kullanabileceğiniz çeşitli veri kümelerini içerir. Scikit kullanma ile ilgili örnekler-kitaplık bilgi [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
