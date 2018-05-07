---
title: Spark yerleşik makine öğrenimi modellerini faaliyete | Microsoft Docs
description: Yük ve Python ile Azure Blob Storage (WASB) içinde depolanan modelleri öğrenme puanı nasıl.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: 626305a2-0abf-4642-afb0-dad0f6bd24e9
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath
ms.openlocfilehash: 928d29da4388372ccc3721c4bcccba5d2bbf5c48
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="operationalize-spark-built-machine-learning-models"></a>Spark yerleşik makine öğrenimi modellerini faaliyete
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

Bu konu Python Hdınsight Spark kümeleri kullanarak kaydedilmiş machine learning modelini (ML) faaliyete gösterilmektedir. Spark Mllib'i kullanarak yerleşik makine öğrenimi modellerini yük açıklar ve Azure Blob Storage (WASB) ve bunları da WASB içinde depolanan veri kümeleriyle puan nasıl depolanır. Dizin oluşturma ve kodlama işlevleri Mllib'i araç setindeki kullanarak özellik dönüştürme nasıl giriş verilerini önceden işleyebilir ve ML modelleriyle Puanlama için giriş olarak kullanılabilen etiketli noktası veri nesnesinin nasıl oluşturulacağını gösterir. Puanlama için kullanılan modelleri doğrusal regresyon, lojistik regresyon, rastgele orman modeli ve gradyan artırmanın ağaç modeli içerir.

## <a name="spark-clusters-and-jupyter-notebooks"></a>Spark kümeleri ve Jupyter Not Defterleri
Kurulum adımlarını ve ML model faaliyete kodu Spark 2.0 küme yanı sıra bir Hdınsight Spark 1.6 kümesi kullanmak için bu kılavuzda sağlanır. Bu yordamlar için kod Jupyter not defterlerinde de sağlanır.

### <a name="notebook-for-spark-16"></a>Spark 1.6 için not defteri
[PySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter not defteri Hdınsight kümelerinde Python kullanarak kaydedilmiş modeli faaliyete nasıl gösterir. 

### <a name="notebook-for-spark-20"></a>Spark 2.0 için not defteri
Spark Hdınsight Spark 2.0 kümesi ile kullanmak üzere 1.6 için Jupyter not defteri değiştirmek için Python kodu dosyasıyla Değiştir [bu dosyayı](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py). Bu kod, Spark 2. 0'oluşturulan modelleri kullanma gösterilmektedir.


## <a name="prerequisites"></a>Önkoşullar

1. Bir Azure hesabı ve Spark 1.6 (veya Spark 2.0) ihtiyacınız bu yönlendirmeyi tamamlamak için Hdınsight kümesi. Bkz: [genel bakış, verileri Azure Hdınsight'ta Spark kullanmanın Bilim](spark-overview.md) yönelik bu gereksinimleri karşılamak yönergeler. Bu konu ayrıca açıklamasını buraya kullanılan NYC 2013 ücreti verileri ve Spark kümesinde Jupyter not defteri gelen kod yürütmek yönergeler içerir. 
2. Makine öğrenimi modellerini aracılığıyla çalışarak burada belirtmek için de oluşturmalısınız [veri keşfi ve modelleme Spark ile](spark-data-exploration-modeling.md) konu Spark 1.6 küme veya Spark 2.0 dizüstü bilgisayarlar için. 
3. Spark 2.0 dizüstü bilgisayarlar, sınıflandırma görevi, iyi bilinen uçak zamanında ayrılma kümesinden 2011 ve 2012 için ek bir veri kümesi kullanın. Not defterlerini ve bağlantılarını bir açıklaması verilmiştir [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) bunları içeren GitHub deposunu için. Ayrıca, kodu buraya ve bağlantılı not defterlerini geneldir ve tüm Spark kümesi üzerinde çalışması gerekir. Hdınsight Spark kullanmıyorsanız küme kurulum ve yönetim adımlar ne burada gösterilenden biraz farklı olabilir. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulumu: depolama konumları, kitaplıklar ve hazır Spark bağlamı
Spark okuyabilmesini ve bir Azure Storage blobu (WASB) yazma. Varolan verilerinizi depolanan şekilde var. Spark ve yeniden WASB içinde depolanan sonuçları kullanarak işlenebilir.

Modelleri veya dosyaları içinde WASB kaydetmek için yolun düzgün belirtilmesi gerekiyor. Spark kümeye eklenen varsayılan kapsayıcı ile başlayan bir yol kullanarak başvurulabilir: *"wasb / / /"*. Aşağıdaki kod örneği okunacak veriler ve model çıkış kaydedildiği modeli depolama dizini için yol konumunu belirtir. 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Dizin yolları için depolama konumları WASB ayarlayın
Modelleri kaydedilir: "wasb: / / / kullanıcı/remoteuser/NYCTaxi/modelleri". Bu yolu düzgün şekilde ayarlanmamışsa, modelleri Puanlama için yüklü değil.

Puanlanmış sonuçları içinde kaydedildi: "wasb: / / / kullanıcı/remoteuser/NYCTaxi/ScoredResults". Klasör yolu yanlış ise, sonuçları klasörde kaydedilmez.   

> [!NOTE]
> Dosya yolu konumlarını kopyalanır ve en son hücresini çıktısından bu kodda yer tutucuları içine yapıştırdığınız **machine-learning-data-science-spark-data-exploration-modeling.ipynb** dizüstü bilgisayar.   
> 
> 

Dizin yolları ayarlamak için kod aşağıdaki gibidir: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**ÇIKTI:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma
Spark bağlamını ayarlayın ve aşağıdaki kod ile gerekli kitaplıkları içeri aktarma

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Spark bağlamını ve PySpark sihirler hazır
Jupyter not defterleri ile sağlanan PySpark tekrar önceden belirlenmiş bir içerik var. Bu nedenle Spark kümesi gerekmez veya açıkça uygulama ile çalışmaya başlamadan önce Hive bağlamları geliştirme. Bunlar varsayılan olarak sizin için kullanılabilir. Bu içerikler şunlardır:

* SC - Spark 
* sqlContext - Hive için

Bazı önceden tanımlanmış "sihirleri" ile çağırabilir özel komutlar olduğu PySpark çekirdeği sağlar %%. Bu kod örneklerinde kullanılan olan iki komut vardır.

* **%% yerel** belirtilen sonraki satırların kodda yerel olarak yürütülür. Kod geçerli Python kodu olmalıdır.
* **%% sql -o <variable name>** 
* Bir Hive sorgusu sqlContext yürütür. -O parametre verilmezse, sorgunun sonucu kalıcı hale getirilir %% Pandas dataframe olarak yerel Python bağlamı.

Tekrar Jupyter not defterlerini ve önceden tanımlanmış hakkında daha fazla bilgi "magics için" sağladıkları, bkz: [Jupyter not defterlerinde kullanılabilen çekirdekler Hdınsight Spark Linux kümeleri Hdınsight'ta](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Veri alma ve Temizlenen veri çerçeve oluşturma
Bu bölümde belirtmek için veri alma için gereken görevleri bir dizi kodunu içerir. Bir birleştirilmiş % 0,1 örnek (.tsv dosyası olarak depolanır) ücreti seyahat ve ücreti dosyanın biçimi verileri okuma ve ardından temiz veri çerçevesi oluşturur.

Ücreti seyahat ve ücreti dosyaları göre sağlanan yordamı katılan: [takım veri bilimi işleminde eylemi: Hdınsight Hadoop kümeleri kullanarak](hive-walkthrough.md) konu.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 46.37 saniye

## <a name="prepare-data-for-scoring-in-spark"></a>Spark Puanlama için verileri hazırlama
Bu bölümde, dizin, kodlama ve bunları Mllib'i denetimli öğrenme algoritmaları kullanımda sınıflandırma ve regresyon hazırlamak için kategorik özellikleri ölçeklendirme gösterilmektedir.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Özellik dönüşümü: dizin ve puanlama modelleri giriş için kategorik özellikleri kodlama
Bu bölümde kullanarak kategorik veri dizin gösterilmektedir bir `StringIndexer` ve özelliklerle kodlamak `OneHotEncoder` modellerini giriş.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) bir dize sütunu bir sütuna etiket dizin etiketlerinin kodlayan. Dizinler etiket sıklıklarını göre sıralanır. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) ikili vektörler, en çok bir değerle tek bir-bir sütunu etiketi dizinlerini sütunun eşler. Bu kodlama kategorik özellikleri uygulanacak Lojistik regresyon gibi sürekli değerli özellikleri beklediğiniz algoritmaları sağlar.

    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()

    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 5.37 saniye

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Özellik dizisi modelleri giriş için olan RDD nesneleri oluşturma
Bu bölümde kategorik metin veri RDD nesne olarak dizin ve böylece eğitmek ve Mllib'i Lojistik regresyon ve ağaç tabanlı modelleri test etmek için kullanılabilir bir hot kodlamak gösterilmektedir kodunu içerir. Dizinlenmiş veri depolanan [dayanıklı Dağıtılmış veri kümesi (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) nesneleri. Spark temel soyutlama bunlar. RDD nesne üzerinde Spark ile paralel işletilen öğe değişmez, bölümlenmiş bir koleksiyonunu temsil eder.

Ayrıca verilerle ölçeklendirmek nasıl oluşturulduğunu gösteren kodu içerir `StandardScalar` doğrusal regresyon ile Stokastik gradyan düşüşü (SGD), machine learning modellerini çeşitli eğitim için yaygın olarak kullanılan bir algoritma kullanmak için Mllib'i tarafından sağlanan. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) birim farkı özellikleri ölçeklemek için kullanılır. Özellik ölçeklendirme, veri normalleştirme da bilinen, yaygın olarak yapılan değerlerle özellikleri olan belirli bir aşırı tartmanız olduğunu hedefi işlevinde oluşturmasını sağlar. 

    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)

    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)

    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)

    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 11.72 saniye

## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>İle Lojistik regresyon modeli Puanlama ve BLOB çıkış kaydedin
Bu bölümdeki kod Azure blob depolama alanına kaydedildi Lojistik regresyon modeli yüklemek ve bir ipucu bir ücreti seyahat Ücretli olsun veya olmasın tahmin, standart sınıflandırma Ölçümleriyle puan kaydedin ve blob depolama sonuçları çizmek için nasıl kullanılacağını gösterir. Puanlanmış sonuçları RDD nesnelerinde depolanır. 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 19.22 saniye

## <a name="score-a-linear-regression-model"></a>Doğrusal regresyon modeli Puanlama
Kullandık [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) Ücretli ipucu miktarı tahmin etmek için Stokastik gradyan düşüşü (SGD) en iyi duruma getirme kullanarak doğrusal regresyon modelini eğitmek için. 

Bu bölümdeki kod, Azure blob depolama alanından bir doğrusal regresyon modeli yüklemek, ölçeklendirilmiş değişkenler kullanarak Puanlama ve sonuçları blob geri kaydedin gösterilmektedir.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel

    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 16.63 saniye

## <a name="score-classification-and-regression-random-forest-models"></a>Sınıflandırma ve regresyon rastgele orman modeli Puanlama
Bu bölümdeki kod kaydedilmiş sınıflandırma yüklemek nasıl gösterir ve regresyon rastgele orman modelleri Azure blob depolama alanına kaydedildi, standart sınıflandırıcı ve regresyon ölçüleri kendi performansını Puanlama ve sonuçları blob depolama birimi kaydedin.

[Rastgele ormanlar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçları ensembles şunlardır.  Bunlar overfitting riskini azaltmak için birçok karar ağaçları birleştiren. Rastgele ormanlar kategorik özellikleri işlemek için çok sınıflı sınıflandırma ayarı genişletmek, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özellik. Rastgele ormanlar en başarılı makine öğrenimi modellerini sınıflandırma ve regresyon biridir.

[Spark.mllib](http://spark.apache.org/mllib/) ve sürekli ve kategorik özelliklerini kullanarak regresyon, çok sınıflı ve ikili sınıflandırma için rastgele ormanlar destekler. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 31.07 saniye

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Sınıflandırma ve regresyon gradyan artırmanın ağaç modeli Puanlama
Bu bölümdeki kod, Sınıflandırma ve regresyon gradyan artırmanın ağaç modelleri Azure blob depolama alanından yük, standart sınıflandırıcı ve regresyon ölçüleri kendi performansını Puanlama ve sonuçları blob depolama birimi kaydedin gösterilmektedir. 

**Spark.mllib** GBTs ikili sınıflandırma ve regresyon, sürekli ve kategorik özelliklerini kullanmayı destekler. 

[Gradyan artırmanın ağaçları](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) olan karar ağaçları ensembles. GBTs tekrarlayarak kaybı işlevi en aza indirmek için karar ağaçları eğitmek. GBTs kategorik özellikleri işleyebilir, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özelliği. Bir sınıflandırma veya çoklu sınıflar ayarında de kullanılabilir.

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 14.6 saniye

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Nesneleri bellekten temizlemek ve puanlanmış dosya konumları yazdırma
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**ÇIKTI:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016 05 0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016 05 0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016 05 0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016 05 0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt

## <a name="consume-spark-models-through-a-web-interface"></a>Bir web arabirimi üzerinden Spark modelleri kullanma
Spark uzaktan Livy adlı bir bileşen ile toplu işler veya bir REST arabiriminden etkileşimli sorguları göndermek için bir mekanizma sağlar. Livy Hdınsight Spark kümenizin üzerinde varsayılan olarak etkindir. Livy hakkında daha fazla bilgi için bkz: [uzaktan Livy kullanarak Spark gönderme işleri](../../hdinsight/spark/apache-spark-livy-rest-interface.md). 

Bir Azure blob depolanır ve ardından sonuçları için başka bir blob yazan bir dosya uzaktan puanları toplu bir işi göndermek için Livy kullanabilirsiniz. Bunu yapmak için Python komut dosyasını karşıya yükleyin.  
[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) Spark kümesi blob için. Gibi bir araç kullanabilirsiniz **Microsoft Azure Storage Gezgini** veya **AzCopy** küme blob komut dosyasını kopyalamak için. Örneğimizde sorundan betiğe karşıya ***wasb:///example/python/ConsumeGBNYCReg.py***.   

> [!NOTE]
> Spark kümesiyle ilişkili depolama hesabı için portalda bulunabilir erişim anahtarları. 
> 
> 

Bu konuma karşıya sonra dağıtılmış bir bağlamda Spark kümesi içinde bu komut dosyasını çalıştırır. Model yükler ve giriş dosyaları modele dayalı Öngörüler çalışır.  

Basit bir HTTPS/REST istek üzerinde Livy yaparak, bu komut dosyası uzaktan çağırabilirsiniz.  Python betiğini uzaktan çağırmak için HTTP isteği oluşturmak için curl komutunu aşağıda verilmiştir. CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME Spark kümeniz için uygun değerlerle değiştirin.

    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Temel kimlik doğrulaması ile basit bir HTTPS çağrı yaparak Livy aracılığıyla Spark iş çağrılacak uzak sistemde herhangi bir dil kullanın.   

> [!NOTE]
> Bu HTTP arama yaparken Python istekleri kitaplığı kullanmak için uygun olacaktır, ancak şu anda Azure işlevlerinde varsayılan olarak yüklü değildir. Bu nedenle eski HTTP kitaplıkları onun yerine kullanılır.   
> 
> 

HTTP çağrısı için Python kod aşağıdaki gibidir:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Bu Python kodu da ekleyebilirsiniz [Azure işlevleri](https://azure.microsoft.com/documentation/services/functions/) Zamanlayıcı, oluşturma veya güncelleştirme bir BLOB gibi çeşitli olayları temel alan bir blob puanlar bir Spark iş gönderme tetiklemek için. 

Kod boş istemci deneyimini tercih ederseniz, kullanın [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) Spark toplu üzerinde bir HTTP eylemi tanımlayarak Puanlama çağrılacak **Logic Apps Tasarımcısı** ve parametrelerini ayarlama. 

* Azure portalından seçerek yeni bir mantıksal uygulama oluşturma **+ yeni** -> **Web + mobil** -> **mantıksal uygulama**. 
* Ortaya çıkarmak için **Logic Apps Tasarımcısı**, mantıksal uygulama ve uygulama hizmeti planı adını girin.
* Bir HTTP eylem seçin ve aşağıdaki çizimde gösterilen parametreler girin:

![Logic Apps Tasarımcısı](./media/spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a>Sırada ne var?
**Çapraz doğrulama ve hyperparameter Süpürme**: bkz [veri keşfi ve modelleme Spark ile Gelişmiş](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabilir üzerinde çapraz doğrulama ve parametre hyper Süpürme kullanılarak eğitilmiş.

