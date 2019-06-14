---
title: Spark'a yerleşik machine learning modelleri - Team Data Science Process çalışır hale getirme
description: Yükleme ve Python ile Azure Blob Storage (WASB) içinde depolanan öğrenimi modellerini puanlamanıza nasıl.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 03/15/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: dd0467479960df30b1d44aeaef7ed0ed0d6c2a87
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60253169"
---
# <a name="operationalize-spark-built-machine-learning-models"></a>Spark'a yerleşik machine learning modelleri kullanıma hazır hale getirme

Bu konu Python kullanarak HDInsight Spark kümelerinde kaydedilmiş machine learning modeli (ML) kullanıma hazır hale getirmeye nasıl gösterir. Bu, Spark MLlib kullanarak yerleşik makine öğrenimi modelleri yükleme işlemini açıklar ve Azure Blob Storage (WASB) ve bunları da WASB içinde depolanan veri kümeleriyle puanlamak nasıl depolanır. Bu, giriş verilerini önceden işleyebilir, MLlib araç setindeki dizin oluşturma ve kodlama işlevleri kullanarak özellik dönüştürme yapmayı ve ML modelleriyle Puanlama için giriş olarak kullanılabilecek bir etiketli noktası veri nesnesinin nasıl oluşturulacağını gösterir. Puanlama için kullanılan model, doğrusal regresyon, lojistik regresyon, rastgele orman modelleri ve gradyan artırma ağaç modeli içerir.

## <a name="spark-clusters-and-jupyter-notebooks"></a>Spark kümelerine ve Jupyter Not Defterleri
Kurulum adımları ve ML modeli hazır hale getirmek için kodu bu kılavuzda bir Spark 2.0 küme yanı sıra bir HDInsight Spark 1.6 kümesi kullanmak için sağlanır. Bu yordamları için kodu, Jupyter not defterlerinde de sağlanır.

### <a name="notebook-for-spark-16"></a>Spark 1.6 için not defteri
[PySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter Not Defteri, Python kullanarak HDInsight kümelerinde kaydedilmiş bir modeli kullanıma hazır hale getirmeye nasıl gösterir. 

### <a name="notebook-for-spark-20"></a>Spark 2.0 için not defteri
Bir HDInsight Spark 2.0 kümesi ile kullanmak üzere Spark 1.6 için Jupyter not defterini değiştirmek için Python kodu dosyayla değiştirin [bu dosyayı](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py). Bu kod, Spark 2.0 sürümünde oluşturulmuş modelleri kullanma işlemi gösterilmektedir.


## <a name="prerequisites"></a>Önkoşullar

1. Bir Azure hesabı ve Spark 1.6 (veya Spark 2.0) ihtiyacınız Bu izlenecek yolu tamamlamak için HDInsight kümesi. Bkz: [genel bakış, verilerin Azure HDInsight üzerinde Spark'ı kullanarak bilimi](spark-overview.md) bu gereksinimleri karşılamak yönergeler. Bu konu ayrıca açıklamasını burada kullanılan NYC 2013 taksi verileri ve Spark kümesinde Jupyter not defteri gelen kodu çalıştırmak yönergeler içerir. 
2. Ayrıca makine öğrenimi modelleri aracılığıyla burada puanlanması oluşturmalısınız [Spark ile veri keşfi ve modelleme](spark-data-exploration-modeling.md) Spark 1.6 küme veya not defterlerini Spark 2.0 için konu. 
3. Spark 2.0 not defterleri, sınıflandırma görevi, iyi bilinen Havayolu zamanında kalkış veri kümesinden 2011 ve 2012 için ek bir veri kümesi kullanın. Not defterlerini ve bağlantıları onlara bir açıklamasını sağlanan [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) bunları içeren GitHub deposu. Ayrıca, kodu buraya bağlı not defterlerinde geneldir ve herhangi bir Spark kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulum ve yönetim adımları ne burada gösterilenden biraz farklı olabilir. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulum: depolama konumları, kitaplıklar ve önceden oluşturulmuş Spark bağlamı
Spark, okuma ve yazma bir Azure depolama Blob (WASB) kuramıyor. Depolanan mevcut verilerinizi şekilde var. Spark ile yeniden WASB içinde depolanan sonuçları işlenebilir.

WASB içinde modelleri veya dosyaları kaydetmek için yolun düzgün bir şekilde belirtilmesi gerekiyor. Spark kümesine eklenen varsayılan kapsayıcı ile başlayan bir yol kullanılarak başvurulabilir: *"wasb / / /"* . Aşağıdaki kod örneği, okunacak verileri ve model çıktısını kaydedildiği modeli depolama dizini için yol konumunu belirtir. 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Dizin yolları depolama konumları WASB ayarlayın
Modelleri kaydedilir: "wasb: / / / kullanıcı/remoteuser/NYCTaxi/modelleri". Bu yolu doğru şekilde ayarlanmamışsa, modellerini Puanlama için yüklü değil.

Puanlanmış sonuçların içinde kaydedildi: "wasb: / / / kullanıcı/remoteuser/NYCTaxi/ScoredResults". Klasör yolu yanlışsa, bu klasörde sonuçları kaydedilmedi.   

> [!NOTE]
> Dosya yolu konumlarını kopyalanır ve bu kod son hücreye çıktısından içindeki yer tutucuları içine yapıştırdığınız **machine-learning-data-science-spark-data-exploration-modeling.ipynb** dizüstü bilgisayar.   
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

**ÇIKIŞ:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma
Spark bağlamını ayarlayın ve gerekli kitaplıkları aşağıdaki kod ile içeri aktarma

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


### <a name="preset-spark-context-and-pyspark-magics"></a>Spark bağlamını ve PySpark işlevlerini hazır
Jupyter not defterleri ile sağlanan PySpark çekirdekleri, önceden ayarlanmış bir bağlam yoktur. Bu nedenle bir Spark kümesi gerekmez veya açıkça, uygulama ile çalışmaya başlamadan önce Hive bağlamları geliştirme. Bunlar varsayılan olarak sizin için kullanılabilir. Şu bağlamlarda şunlardır:

* SC - Spark 
* sqlContext - Hive için

PySpark çekirdeği bazı önceden tanımlanmış "işlevlerini" ile çağırabileceğiniz özel komutlar olduğu sağlar %%. Bu kod örneklerinde kullanılan iki tür komutlar vardır.

* **%% yerel** belirtilen sonraki satırların kodu yerel olarak yürütülür. Kod, geçerli Python kodu olmalıdır.
* **%% sql -o \<değişken adı >** 
* Bir Hive sorgusu sqlContext karşı yürütür. -O parametreye geçirilmişse, sorgu sonucu kalıcı hale getirilir %% Pandas dataframe olarak yerel Python bağlamı.

Jupyter not defterleri ve önceden tanımlanmış çekirdekler hakkında daha fazla bilgi "magics için" sağlarlar, bkz: [için Jupyter not defterlerinde kullanılabilen çekirdekler HDInsight Spark Linux kümeleri HDInsight](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Veri alma ve Temizlenen veri çerçevesi oluşturun
Bu bölüm, bir dizi puanlanması veri almak için gereken görevler için kod içerir. (.Tsv dosya olarak depolanan) taksi seyahat ve taksi dosya içinde bir birleştirilmiş %0,1 örnek biçim verileri okuma ve verileri temizleme çerçeve oluşturur.

Taksi seyahat ve taksi dosyaları göre sağlanan yordam üzerinde katılan: [Team Data Science Process'in çalışması: HDInsight Hadoop kümeleri kullanarak](hive-walkthrough.md) konu.

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

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 46.37 saniye

## <a name="prepare-data-for-scoring-in-spark"></a>Spark, Puanlama için verileri hazırlama
Bu bölümde, dizin, kodlama ve ölçeklendirmek için sınıflandırma ve regresyon MLlib denetimli öğrenme algoritmalarını içinde kullanıma hazırlamak için kategorik özellikleri gösterilmektedir.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Özellik dönüşüm: dizin ve puanlama modelleri giriş için kategorik özellikleri kodlayın
Bu bölüm sütunları ise kategorik veriler kullanılarak dizinleme gösterir bir `StringIndexer` ve özelliklerle kodlama `OneHotEncoder` modellerini giriş.

[StringIndexer](https://spark.apache.org/docs/latest/ml-features.html#stringindexer) etiketlerin etiket dizinleri içeren bir sütun için bir dize sütunu kodlar. Dizinleri etiket frekans tarafından sıralanır. 

[OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) etiket dizinleri içeren bir sütun ikili vektörler, en fazla bir değerle tek bir-bir sütunu eşlenir. Bu kodlama beklediğiniz gibi kategorik özellikleri uygulanacak Lojistik regresyon, sürekli değerli özellikler algoritmalar sağlar.

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

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 5.37 saniye

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Giriş modelleri için özellik dizilerle RDD nesneleri oluşturma
Bu bölüm RDD nesne olarak kategorik metin verileri ve eğitme ve test MLlib Lojistik regresyon ve ağaç tabanlı modeller için kullanılabilmesi için bir anında kodlayamadığı gösteren kod içerir. Dizinlenmiş verileri depolanan [dayanıklı Dağıtılmış veri kümesi (RDD)](https://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) nesneleri. Bu, Spark temel soyutlama vardır. RDD nesne üzerinde Spark ile paralel işletilebilir öğelerinin sabit, bölümlenmiş bir koleksiyonunu temsil eder.

Ayrıca verilerle ölçeklendirme gösteren kod içeren `StandardScalar` kullanılmak üzere doğrusal regresyon ile Stokastik gradyan düşüşü (SGD), makine öğrenimi modelleri çok çeşitli eğitim için popüler bir algoritma MLlib tarafından sağlanan. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) birim varyansı özellikleri ölçeklendirmek için kullanılır. Veri normalleştirme da bilinen özellik ölçeklendirme, yaygın olarak yapılan değerlerle özellikleri olan belirli bir aşırı ağırlık, hedef işlevi oluşturmasını sağlar. 

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

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC REGRESSION MODELS
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

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 11.72 saniye

## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>İle Lojistik regresyon modeli Puanlama ve çıkış BLOB kaydedin
Bu bölümdeki kod Azure blob depolama alanında kaydedilmiş bir Lojistik regresyon modeli yüklemek ve bir ipucu taksi seyahate Ücretli olup olmadığını tahmin, standart sınıflandırma Ölçümleriyle puan kaydedin ve sonuçların stora blob çizmek için kullanma gösterilmektedir Ge. Puanlanmış sonuçların RDD nesneleri depolanır. 

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

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 19.22 saniye

## <a name="score-a-linear-regression-model"></a>Bir doğrusal regresyon modeli Puanlama
Kullandık [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) Stokastik gradyan düşüşü (SGD) iyileştirme ipucu Ücretli miktarı tahmin etmek için kullanarak bir doğrusal regresyon modeli eğitmek için. 

Bu bölümdeki kod, Azure blob depolamadan bir doğrusal regresyon modeli yüklemek, puan ölçeklendirilmiş değişkenleri kullanma ve sonuçları bloba kaydedin işlemi gösterilmektedir.

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


**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 16.63 saniye

## <a name="score-classification-and-regression-random-forest-models"></a>Sınıflandırma ve regresyon rastgele orman modellerini Puanlama
Bu bölümdeki kod kaydedilmiş sınıflandırma yüklemeyi gösterir ve regresyon rastgele orman modelleri, Azure blob depolama alanında kaydedildi, standart sınıflandırıcı ve regresyon ölçülerle performanslarını ve sonuçları blob depolama alanına kaydedin.

[Rastgele ormanları](https://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçları Kümelemeler olan.  Bunlar overfitting riskini azaltmak için birçok karar ağaçları birleştirin. Rastgele ormanları kategorik özellikleri işleyebilir sapmalar yakalamak ve etkileşimleri özellik sınıflı sınıflandırma ayarı genişletmek ve ölçeklendirme özelliğini gerektirmez. Rastgele ormanları en başarılı makine öğrenimi için sınıflandırma ve regresyon modellerini biridir.

[Spark.mllib](https://spark.apache.org/mllib/) ikili ve çok sınıflı sınıflandırma ve regresyon, sürekli ve kategorik özelliklerini kullanarak rastgele ormanlar destekler. 

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

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 31.07 saniye

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Sınıflandırma ve regresyon gradyan artırma ağaç modellerini Puanlama
Bu bölümdeki kod, Sınıflandırma ve regresyon gradyan artırma ağaç modeller Azure blob depolama alanından yüklenemiyor, standart sınıflandırıcı ve regresyon ölçülerle performanslarını ve sonuçları blob depolama alanına kaydedin işlemi gösterilmektedir. 

**Spark.mllib** GBTs ikili sınıflandırma ve regresyon, sürekli ve kategorik özelliklerini kullanarak destekler. 

[Gradyan artırma ağaçları](https://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) olan Kümelemeler karar ağaçları (GBTs). GBTs çalıştırmalarınızı kaybı işlevi en aza indirmek için karar ağaçları eğitin. GBTs kategorik özellikleri işleyebilir, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özellik olanağına sahip olursunuz. Bir sınıflandırma veya çoklu sınıflar ayarında de kullanılabilir.

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

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 14.6 saniye

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Bellek nesneleri temizleyin ve puanlanmış dosya konumları yazdırma
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


**ÇIKIŞ:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016 05 0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016 05 0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016 05 0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016 05 0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt

## <a name="consume-spark-models-through-a-web-interface"></a>Bir web arabirimi aracılığıyla Spark modelleri kullanma
Spark uzaktan Livy adlı bir bileşen ile batch işleri veya bir REST arabirimi üzerinden etkileşimli sorguları göndermek için bir mekanizma sağlar. Livy HDInsight Spark kümeniz üzerindeki varsayılan olarak etkindir. Livy ile ilgili daha fazla bilgi için bkz: [Uzaktan Livy kullanarak Spark işleri göndermek](../../hdinsight/spark/apache-spark-livy-rest-interface.md). 

Bir Azure blob içinde depolanır ve ardından sonuçları başka bir bloba yazan bir dosya uzaktan puanlarını toplu bir işi göndermek için Livy kullanabilirsiniz. Bunu yapmak için Python betiği karşıya yükleyin.  
[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) Spark kümesinin blob için. Gibi bir araç kullanabilirsiniz **Microsoft Azure Depolama Gezgini** veya **AzCopy** betik küme bloba kopyalamak için. Örneğimizde betiği dosyamızı ***wasb:///example/python/ConsumeGBNYCReg.py***.   

> [!NOTE]
> Erişim anahtarları, Spark kümesiyle ilişkili depolama hesabı için portal bulunabilir. 
> 
> 

Bu konuma karşıya sonra dağıtılmış bir bağlamda Spark küme içindeki bu betiği çalıştırır. Bu modeli yükler ve giriş dosyalarını modele dayalı Öngörüler çalışır.  

Livy üzerinde basit bir HTTPS/REST isteği yaparak bu betiği uzaktan çalıştırabilirsiniz.  Python betiğini uzaktan çağırmak için bir HTTP isteği oluşturmak için bir curl komutu aşağıda verilmiştir. CLUSTERLOGIN, CLUSTERPASSWORD CLUSTERNAME Spark kümeniz için uygun değerlerle değiştirin.

    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Temel kimlik doğrulaması ile basit bir HTTPS çağrısı yaparak Lıvy aracılığıyla Spark işi çağırmak için Uzak sistemde herhangi bir dil kullanabilirsiniz.   

> [!NOTE]
> Bu HTTP çağrısı yaparken Python istekleri kitaplığı kullanmak kullanışlı, ancak şu anda Azure işlevleri'nde varsayılan olarak yüklü değildir. Bu nedenle eski HTTP kitaplıkları yerine kullanılır.   
> 
> 

HTTP çağrısı için Python kod aşağıdaki gibidir:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILABLE BY DEFAULT
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


Ayrıca bu Python kodu ekleyebilirsiniz [Azure işlevleri](https://azure.microsoft.com/documentation/services/functions/) Zamanlayıcı, oluşturma veya güncelleştirme bir blobun gibi çeşitli olayları temel alan bir blob puanlar bir Spark işi göndermeyi tetiklemek için. 

Bir kod ücretsiz istemci deneyimi tercih ediyorsanız, kullanın [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) Spark toplu işlem üzerinde bir HTTP eylemi tanımlayarak Puanlama çağrılacak **Logic Apps Tasarımcısı'nda** ve kendi parametreleri ayarlanıyor. 

* Azure Portalı'ndan seçerek yeni bir mantıksal uygulama oluşturma **+ yeni** -> **Web + mobil** -> **mantıksal uygulama**. 
* Ortaya çıkarmak için **Logic Apps Tasarımcısı'nda**, mantıksal uygulama ve App Service planı adı girin.
* HTTP eylem seçin ve aşağıdaki şekilde gösterilen parametreler girin:

![Logic Apps Tasarımcısı](./media/spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a>Sırada ne var?
**Çapraz doğrulama ve hiper parametre Süpürme**: Bkz: [Gelişmiş Veri keşfi ve modelleme Spark ile](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabileceğini üzerinde çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitim.

