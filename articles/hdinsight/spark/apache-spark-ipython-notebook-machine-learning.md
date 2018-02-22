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
ms.date: 01/23/2018
ms.author: jgao
ms.openlocfilehash: 2f7dcb9bea05a79a6647b549896c8107f9e830af
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Azure hdınsight'ta Apache Spark machine learning uygulamaları derleme

Hdınsight'ta Spark kümesi kullanarak uygulama öğrenme bir Apache Spark makine oluşturmayı öğrenin. Bu makalede kullanılabilir Jupyter not defteri ile küme oluşturmak ve bu uygulamayı test için nasıl kullanılacağı gösterilmektedir. Uygulama, varsayılan olarak tüm kümelerde kullanılabilir örnek HVAC.csv verileri kullanır.

[Mllib'i](https://spark.apache.org/docs/1.1.0/mllib-guide.html) olduğu ortak algoritmaları ve yardımcı programlar öğrenme, Sınıflandırma, regresyon, kümeleme, işbirliği filtreleme, boyut azaltma dahil olmak üzere, hem de arka plandaki oluşan Spark'ın ölçeklenebilir makine öğrenme kitaplığı en iyi duruma getirme temelleri.

## <a name="prerequisites"></a>Ön koşullar:

Aşağıdaki öğe olması gerekir:

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Veri kümesi anlama

Aşağıdaki veriler, hedef sıcaklık ve gerçek HVAC sistemleri yüklü olan bazı bina sıcaklığını gösterir. **Sistem** sütun sistem Kimliğini temsil eder ve **SystemAge** sütun HVAC sistem yapı yerinde açıldı yıl sayısını temsil eder. Bir yapı sistem Kimliğini ve sistem yaş verilen hedef sıcaklık hotter veya colder tabanlı olup olmayacağını tahmin etmek için Bu öğreticide verileri kullanın.

![Spark machine learning örnek için kullanılan verilerin anlık görüntüsünü](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Spark machine learning örnek için kullanılan verilerin anlık görüntüsünü")

Veri dosyası **HVAC.csv**, konumda bulunan **\HdiSamples\HdiSamples\SensorSampleData\hvac** tüm Hdınsight kümeleri üzerinde.

## <a name="app"></a>Spark Mllib'i kullanarak Spark machine learning uygulaması yazma
Bu uygulamada bir Spark kullanmak [ML ardışık düzen](https://spark.apache.org/docs/2.2.0/ml-pipeline.html) belge sınıflandırması yapmak için. ML komut zincirleri oluşturun ve ardışık düzen öğrenme pratik makine ayarlamak kullanıcıların Yardım DataFrames üstünde yerleşik yüksek düzey API'leri Tekdüzen kümesi sağlar. Ardışık düzeninde sözcüklere belgeyi bölme, bir sayısal özellik vektör sözcükleri dönüştürme ve son olarak özellik vektörlerinin ve etiketleri kullanarak bir tahmin modeli yapı. Uygulama oluşturmak için aşağıdaki adımları gerçekleştirin.

1. PySpark çekirdeği kullanarak bir Jupyter not defteri oluşturun. Yönergeler için bkz: [Jupyter not defteri oluşturma](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).
2. Bu senaryo için gereken türleri içeri aktarın. Boş bir hücreye aşağıdaki kod parçacığını yapıştırın ve sonra basın **SHIFT + ENTER**. 

    ```PySpark
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
    ```
3. (Hvac.csv) veri yükleme, bunu ayrıştırabilir ve modeli eğitmek için kullanın. 

    ```PySpark
    # Define a type called LabelDocument
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
    ```

    Kod parçacığında, gerçek sıcaklık hedef sıcaklık ile karşılaştıran bir işlev tanımlayın. Gerçek sıcaklık büyükse, yapı değeri tarafından belirtilen hareketli, **1.0**. Aksi takdirde yapı değeri tarafından belirtilen soğuk, **0,0**. 

4. Üç aşamadan oluşur Spark machine learning ardışık düzenini yapılandırın: belirteç Oluşturucu, hashingTF ve lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    İşlem hattı nedir ve bakın nasıl çalıştığı hakkında daha fazla bilgi için <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning ardışık</a>.

5. Ardışık Düzen eğitim belgeye uygun.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Denetim noktası eğitim belgeye uygulamayla ilerleme durumunuzu doğrulayın.
   
    ```PySpark
    training.show()
    ```
   
    Bu, aşağıdakine benzer bir çıktı vermeniz gerekir:

    ```
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
    ```

    Ham CSV dosyası karşı çıktı karşılaştırma. Örneğin, CSV dosyasının ilk satırı bu verileri vardır:

    ![Çıkış Spark machine learning örnek için anlık görüntü verileri](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Spark machine learning örnek için çıktı veri anlık görüntüsü")

    Nasıl gerçek sıcaklık yapı soğuk öneren hedef sıcaklık değerinden olduğuna dikkat edin. Bu nedenle çıktı, eğitim değeri **etiket** ilk sırada **0,0**, yapı başka bir deyişle, etkin değil.

7. Bir veri kümesi ve eğitilen modele karşı çalıştırmak için hazırlayın. Bunu yapmak için bir sistem Kimliğini ve sistem yaş geçirdiğiniz (olarak gösterilen **SystemInfo** eğitim çıkışı), ve bu sistem kimliği ve sistem yaş yapı hotter olup olmayacağını modeli tahmin (1.0 tarafından gösterilen) veya daha soğuk (0.0 tarafından gösterilen).
   
    ```PySpark   
    # SystemInfo here is a combination of system ID followed by system age
    Document = Row("id", "SystemInfo")
    test = sc.parallelize([(1L, "20 25"),
                    (2L, "4 15"),
                    (3L, "16 9"),
                    (4L, "9 22"),
                    (5L, "17 10"),
                    (6L, "7 22")]) \
        .map(lambda x: Document(*x)).toDF() 
    ```
8. Son olarak, test verileri üzerindeki tahminlerde. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   Tahmin ilk satırdan kimliği 20 ve 25 yıllık sistem geçerlilik süresi ile bir HVAC sistemi için yapı etkin olduğunu görebilirsiniz (**tahmin = 1.0**). DenseVector (0.49999) ilk değeri 0,0 tahmin karşılık gelir ve ikinci değer (0.5001) 1.0 tahmin karşılık gelir. Çıktıda ikinci değer yalnızca fazladır daha yüksek olmasına rağmen modelini gösteren **tahmin = 1.0**.
10. Kaynakları serbest bırakmak için Not Defteri kapatın. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Kapatılır ve not defteri kapatın.

## <a name="anaconda"></a>Anaconda scikit kullanın-Spark machine learning için kitaplık öğrenin
Hdınsight'ta Apache Spark kümeleri Anaconda kitaplıkları içerir. Bu da içerir **scikit-öğrenin** machine learning için kitaplık. Kitaplık Ayrıca örnek uygulamalardan doğrudan Jupyter not defteri oluşturmak için kullanabileceğiniz çeşitli veri kümelerini içerir. Scikit kullanma ile ilgili örnekler-kitaplık bilgi [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
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
