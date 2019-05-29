---
title: 'Öğretici: Spark makine öğrenme Azure HDInsight uygulama oluşturun'
description: Jupyter not defteri kullanarak HDInsight Spark kümelerinde Apache Spark makine öğrenimi uygulaması derlemeye ilişkin adım adım yönergeler.
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/24/2019
ms.author: hrasheed
ms.openlocfilehash: ed6a8f83d2ef31513aeadbc6741dd77c30c30070
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252892"
---
# <a name="tutorial-build-an-apache-spark-machine-learning-application-in-hdinsight"></a>Öğretici: HDInsight uygulama öğrenme bir Apache Spark makine oluşturun 

Bu öğreticide, şunların nasıl kullanılacağını [Jupyter not defteri](https://jupyter.org/) oluşturmak için bir [Apache Spark](https://spark.apache.org/) makine öğrenimi uygulaması Azure HDInsight için. 

[MLlib](https://spark.apache.org/docs/latest/ml-guide.html); sınıflandırma, regresyon, kümeleme, ortak filtreleme, boyut düzeyi azaltma gibi genel öğrenme algoritmaları ve yardımcı programlarının yanı sıra temel alınan iyileştirme temellerinden oluşan, Spark’ın makine öğrenimi kitaplığıdır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir Apache Spark makine öğrenimi uygulama geliştirin

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz: [bir Apache Spark kümesi oluşturma](./apache-spark-jupyter-spark-sql-use-portal.md).

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi. Daha fazla bilgi için [veri yükleme ve HDInsight üzerinde Apache Spark ile sorguları çalıştırma](./apache-spark-load-data-run-query.md).

## <a name="understand-the-data-set"></a>Veri kümesini anlamak

Uygulama, varsayılan olarak tüm kümelerde bulunan örnek HVAC.csv verilerini kullanır. Dosya şu konumdadır `\HdiSamples\HdiSamples\SensorSampleData\hvac`. Veriler, HVAC sistemlerinin yüklü olduğu bazı binaların hedef sıcaklığı ile gerçek sıcaklığını gösterir. **System** sütunu sistem kimliğini, **SystemAge** sütunu ise HVAC sisteminin binada kaç yıldır kullanıldığını ifade eder. Verileri kullanarak, bir sistem kimliği ve sistem yaşı için binanın hedef sıcaklığa göre daha sıcak ya da daha soğuk olacağını öngörebilirsiniz.

![Spark makine öğrenimi örneği için kullanılan verilerin anlık görüntüsü](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Spark makine öğrenimi örneği için kullanılan verilerin anlık görüntüsü")

## <a name="develop-a-spark-machine-learning-application-using-spark-mllib"></a>Spark MLlib kullanarak Spark makine öğrenimi uygulaması geliştirme

Bu uygulamada bir belge sınıflandırması gerçekleştirmek için Spark [ML işlem hattı](https://spark.apache.org/docs/2.2.0/ml-pipeline.html) kullanırsınız. ML İşlem Hatları, kullanıcıların pratik makine öğrenimi işlem hatları oluşturmasına ve ayarlamasına yardımcı olan DataFrames temel alınarak geliştirilmiş bir dizi tekdüzen yüksek düzey API kümesi sağlar. İşlem hattında, belgeyi sözcüklere böler, sözcükleri sayısal bir özellik vektörüne dönüştürür ve son olarak özellik vektörleri ile etiketleri kullanarak bir tahmin modeli oluşturursunuz. Uygulamayı oluşturmak için aşağıdaki adımları gerçekleştirin.

1. PySpark çekirdeği kullanarak bir Jupyter not defteri oluşturun. Yönergeler için bkz. [Jupyter not defteri oluşturma](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).
2. Bu senaryo için gereken türleri içeri aktarın. Aşağıdaki kod parçacığını boş bir hücreye yapıştırın ve sonra **SHIFT + ENTER** tuşlarına basın. 

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

3. Verileri yükleyin (hvac.csv), ayrıştırın ve modeli eğitmek için kullanın. 

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
    data = sc.textFile("/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
    training = documents.toDF()
    ```

    Kod parçacığında gerçek sıcaklığı hedef sıcaklıkla karşılaştıran bir işlev tanımlarsınız. Gerçek sıcaklık büyükse, **1.0** değeriyle gösterildiği gibi bina sıcaktır. Aksi takdirde, **0,0** değeriyle gösterildiği gibi bina soğuktur. 

4. Üç aşamadan oluşan Spark makine öğrenimi işlem hattını yapılandırın: tokenizer, hashingTF ve lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    İşlem hattı ve nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Apache Spark makine öğrenimi işlem hattı](https://spark.apache.org/docs/latest/ml-pipeline.html).

5. İşlem hattını eğitim belgesine uygun hale getirin.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Uygulama ile gerçekleştirdiğiniz ilerlemeyi kontrol etmek için eğitim belgesini doğrulayın.
   
    ```PySpark
    training.show()
    ```
   
    Çıktı şuna benzer olacaktır:

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

    Çıktıyı ham CSV dosyasıyla karşılaştırın. Örneğin, CSV dosyasının bu verileri içeren ilk satırı:

    ![Spark makine öğrenimi örneği için çıktı verileri anlık görüntüsü](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Spark makine öğrenimi örneği için çıktı verileri anlık görüntüsü")

    Binanın soğuk olduğunu göstermek üzere gerçek sıcaklığın hedef sıcaklıktan az olduğuna dikkat edin. Bu nedenle, eğitim çıktısındaki ilk satırda **label** değeri **0.0**’dır ve binanın sıcak olmadığı anlamına gelir.

7. Eğitilen modeli çalıştırmak için bir veri kümesi hazırlayın. Bunu yapmak için, bir sistem kimliği ve sistem yaşı (eğitim çıktısında **SystemInfo** olarak belirtilir) geçirin. Model, bu sistem kimliği ve sistem yaşına sahip binanın daha sıcak (1.0 ile belirtilir) veya daha soğuk (0.0 ile belirtilir) olacağını tahmin eder.
   
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
8. Son olarak, test verileri üzerinde tahminlerde bulunun. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    Çıktı şuna benzer olacaktır:

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   Tahminin ilk satırından Kimliği 20 ve sistem yaşı 25 yıl olan bir HVAC sistemi için binanın sıcak olduğunu (**tahmin=1.0**) görebilirsiniz. Birinci DenseVector değeri (0.49999) 0.0 tahminine, ikinci değer (0.5001) ise 1.0 tahminine karşılık gelir. Çıktıda ikinci değer yalnızca çok az yüksek olsa bile model **tahmin = 1.0** değerini gösterir.
10. Kaynakları serbest bırakmak için not defterini kapatın. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’u seçin. Bu eylem, not defterini kapatır.

## <a name="use-anaconda-scikit-learn-library-for-spark-machine-learning"></a>Spark makine öğrenimi için Anaconda scikit-learn kitaplığını kullanma
HDInsight’ta Apache Spark kümeleri, Anaconda kitaplıklarını içerir. Ayrıca, makine öğrenimi **scikit-learn** kitaplığını içerir. Kitaplık aynı zamanda, aynı uygulamaları bir Jupyter not defterinden doğrudan derlemek için kullanabileceğiniz çeşitli veri kümeleri içerir. scikit-learn kitaplığını kullanma örnekleri için bkz. [https://scikit-learn.org/stable/auto_examples/index.html](https://scikit-learn.org/stable/auto_examples/index.html).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure HDInsight için uygulama öğrenme bir Apache Spark machine oluşturmak için Jupyter not defterini kullanma öğrendiniz. Spark işleri için IntelliJ IDEA kullanma hakkında bilgi edinmek üzere sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Intellij kullanarak Scala Maven uygulama oluşturma](./apache-spark-create-standalone-application.md)