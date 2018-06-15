---
title: Azure Hdınsight'ta bir Apache Spark machine learning ardışık düzen - oluşturun | Microsoft Docs
description: Apache Spark machine learning kitaplığı veri ardışık düzen oluşturmak için kullanın.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: maxluk
ms.openlocfilehash: c3ff29404858a768737536e7d31c3c6858eea7d2
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164559"
---
# <a name="create-a-spark-machine-learning-pipeline"></a>Spark makine öğrenimi işlem hattı oluşturma

Apache Spark'ın ölçeklenebilir machine learning kitaplığı (Mllib'i) dağıtılmış bir ortama modelleme yetenekleri getirir. Spark paket [ `spark.ml` ](http://spark.apache.org/docs/latest/ml-pipeline.html) DataFrames üzerinde yerleşik yüksek düzey API kümesidir. Bu API'ları oluşturmak ve pratik makine öğrenme ardışık düzen ince ayar yardımcı olur.  *Spark machine learning* bu Mllib'i DataFrame tabanlı API'sine olmayan eski RDD tabanlı ardışık düzen API başvuruyor.

Machine learning (ML) ardışık birden çok machine learning algoritmaları birlikte birleştirme tam bir iş akışı ' dir. İşlem ve bir dizi algoritmaları gerektiren verilerden bilgi almak için gereken birçok adım olabilir. Ardışık Düzen aşamaları ve işlem öğrenme bir makineye sıralama tanımlayın. Mllib'i içinde bir ardışık düzen aşamaları PipelineStages, burada bir dönüştürücü ve bir tahmin görevleri belirli bir dizi temsil edilir.

Bir dönüştürücü kullanarak bir DataFrame diğerine dönüştüren bir algoritmadır `transform()` yöntemi. Örneğin, bir özellik transformer tek sütunlu bir DataFrame okuma, başka bir sütuna eşleme ve eklenmiş eşlenen sütun ile yeni bir DataFrame çıktı.

Bir tahmin algoritmaları öğrenme bir soyutlamadır ve sığdırma veya bir dönüştürücü üretmek bir veri kümesine eğitim sorumludur. Adlı bir yöntem bir tahmin uygulayan `fit()`, bir DataFrame kabul eder ve bir dönüştürücü olan bir DataFrame üretir.

Bir dönüştürücü veya bir tahmin durum bilgisiz her örneğinin Parametreler belirtildiğinde kullanılan kendi benzersiz tanımlayıcısı vardır. Her ikisi de, bir Tekdüzen API Bu parametreler belirtmek için kullanın.

## <a name="pipeline-example"></a>Ardışık Düzen örnek

Bu örnek bir ML ardışık pratik kullanımını göstermek için örnek kullanır `HVAC.csv` Hdınsight kümenizi, Azure Storage veya Data Lake Store için varsayılan depolama önceden yüklenmiş gelen veri dosyası. Dosya içeriğini görüntülemek için gidin `/HdiSamples/HdiSamples/SensorSampleData/hvac` dizin. `HVAC.csv` hem hedef hem de gerçek etme zamanlarında kümesi için HVAC içerir (*ısıtma, havalandırma ve klima*) çeşitli binalar sistemlerinde. Veri modeli eğitmek ve belirli bir yapı için tahmin sıcaklık üretmek için belirtilir.

Aşağıdaki kod:

1. Tanımlayan bir `LabeledDocument`, depolayan `BuildingID`, `SystemInfo` (sistemin tanımlayıcısı ve geçerlilik süresi) ve bir `label` (yapı çok sıcak ise 1.0 0,0 aksi).
2. Bir özel ayrıştırıcı işlev oluşturur `parseDocument` veri satırı (satır) alır ve gerçek sıcaklık için hedef sıcaklık karşılaştırarak, yapı "etkin" olup olmadığını belirler.
3. Ayrıştırıcının veri kaynağını çıkarılırken geçerlidir.
4. Eğitim verileri oluşturur.

```python
# The data structure (column meanings) of the data array:
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
data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
training = documents.toDF()
```

Bu örnek ardışık üç aşamadan oluşur: `Tokenizer` ve `HashingTF` (her iki dönüştürücüler), ve `Logistic Regression` (tahmin).  Ayıklanan ve Ayrıştırılan verilerde `training` DataFrame ardışık düzen üzerinden akar zaman `pipeline.fit(training)` olarak adlandırılır.

1. Birinci aşama `Tokenizer`, böler `SystemInfo` giriş sütununu (sistem tanımlayıcısı ve yaş değerlerini içeren) bir `words` çıkış sütun. Bu yeni `words` sütun DataFrame eklenir. 
2. İkinci aşama `HashingTF`, yeni dönüştürür `words` özelliği vektörlerinin sütununa. Bu yeni `features` sütun DataFrame eklenir. İlk iki Bu aşamalar dönüştürücüler ' dir. 
3. Üçüncü aşaması `LogisticRegression`bir tahmin olduğu ve bu nedenle ardışık çağırır `LogisticRegression.fit()` yöntemini üretmeye bir `LogisticRegressionModel`. 

```python
tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)

# Build the pipeline with our tokenizer, hashingTF, and logistic regression stages
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(training)
```

Yeni görmek için `words` ve `features` tarafından eklenen sütunlar `Tokenizer` ve `HashingTF` dönüştürücüler ve bir örnek `LogisticRegression` tahmin çalıştırmak, bir `PipelineModel.transform()` özgün DataFrame yöntemi. Üretim kodunda testinde eğitim doğrulamak için DataFrame geçirmek için sonraki adım olacaktır.

```python
peek = model.transform(training)
peek.show()

# Outputs the following:
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|BuildingID|SystemInfo|label|   words|            features|       rawPrediction|         probability|prediction|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|         4|     13 20|  0.0|[13, 20]|(262144,[250802,2...|[0.11943986671420...|[0.52982451901740...|       0.0|
|        17|      3 20|  0.0| [3, 20]|(262144,[89074,25...|[0.17511205617446...|[0.54366648775222...|       0.0|
|        18|     17 20|  1.0|[17, 20]|(262144,[64358,25...|[0.14620993833623...|[0.53648750722548...|       0.0|
|        15|      2 23|  0.0| [2, 23]|(262144,[31351,21...|[-0.0361327091023...|[0.49096780538523...|       1.0|
|         3|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|         4|     13 28|  0.0|[13, 28]|(262144,[69821,25...|[0.14630166986618...|[0.53651031790592...|       0.0|
|         2|     12 24|  0.0|[12, 24]|(262144,[187043,2...|[-0.0509556393066...|[0.48726384581522...|       1.0|
|        16|     20 26|  1.0|[20, 26]|(262144,[128319,2...|[0.33829638728900...|[0.58377663577684...|       0.0|
|         9|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|        12|       6 5|  0.0|  [6, 5]|(262144,[18659,89...|[0.07513008136562...|[0.51877369045183...|       0.0|
|        15|     10 17|  1.0|[10, 17]|(262144,[64358,25...|[-0.0291988646553...|[0.49270080242078...|       1.0|
|         7|      2 11|  0.0| [2, 11]|(262144,[212053,2...|[0.03678030020834...|[0.50919403860812...|       0.0|
|        15|      14 2|  1.0| [14, 2]|(262144,[109681,2...|[0.06216423725633...|[0.51553605651806...|       0.0|
|         6|       3 2|  0.0|  [3, 2]|(262144,[89074,21...|[0.00565582077537...|[0.50141395142468...|       0.0|
|        20|     19 22|  0.0|[19, 22]|(262144,[139093,2...|[-0.0769288695989...|[0.48077726176073...|       1.0|
|         8|     19 11|  0.0|[19, 11]|(262144,[139093,2...|[0.04988910033929...|[0.51246968885151...|       0.0|
|         6|      15 7|  0.0| [15, 7]|(262144,[77099,20...|[0.14854929135994...|[0.53706918109610...|       0.0|
|        13|      12 5|  0.0| [12, 5]|(262144,[89689,25...|[-0.0519932532562...|[0.48700461408785...|       1.0|
|         4|      8 22|  0.0| [8, 22]|(262144,[98962,21...|[-0.0120753606650...|[0.49698119651572...|       1.0|
|         7|      17 5|  0.0| [17, 5]|(262144,[64358,89...|[-0.0721054054871...|[0.48198145477106...|       1.0|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+

only showing top 20 rows
```

`model` Nesne artık tahminlerde için kullanılabilir. Bu makine öğrenimi uygulama ve çalıştırmaya yönelik adım adım yönergeleri tam örnek için bkz: [yapı Apache Spark machine learning uygulamaları Azure hdınsight'ta](apache-spark-ipython-notebook-machine-learning.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Veri bilimi Scala ve Spark Azure üzerinde kullanma](../../machine-learning/team-data-science-process/scala-walkthrough.md)
