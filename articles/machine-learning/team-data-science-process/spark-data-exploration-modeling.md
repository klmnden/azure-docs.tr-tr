---
title: Spark - Team Data Science Process ile veri keşfi ve modelleme
description: Azure üzerinde Spark MLlib Araç Seti veri keşfi ve modelleme özellikleri gösterir.
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
ms.openlocfilehash: acc701431afa458efd0768fb3d6898fd1920e333
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528093"
---
# <a name="data-exploration-and-modeling-with-spark"></a>Spark ile veri keşfi ve modelleme

Bu kılavuzda HDInsight Spark veri araştırma yapmak için kullanır ve ikili sınıflandırma ve regresyon görevleri NYC örneği üzerinde modelleme seyahat taksi 2013 dataset masrafları.  Bu adımlarında size kılavuzluk eder [Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/), uçtan uca, verilere ve modelleri depolamak için işleme ve Azure BLOB'ları için bir HDInsight Spark kümesi kullanarak. İşlem inceler ve bir Azure Storage Blobundan getirildi verileri görselleştiren ve ardından Tahmine dayalı modeller oluşturmak için verileri hazırlar. Bu ikili sınıflandırma ve regresyon modelleme görevleri gerçekleştirmek için Spark MLlib araç setini kullanarak derleme modelleridir.

* **İkili sınıflandırma** görev ipucu için seyahat Ücretli olup olmadığını tahmin etmektir. 
* **Regresyon** görevdir ipucu diğer özelliklere göre bahşiş miktarını tahmin edin. 

Mantıksal ve doğrusal regresyon, rastgele ormanları ve gradyan artırmalı ağaçları kullandığımız modelleri şunlardır:

* [Doğrusal regresyonla SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) Stokastik gradyan düşüşü (SGD) yöntemini kullanan bir doğrusal regresyon modeli ve ipucu miktarları tahmin etmek için ölçeklendirme, iyileştirme ve özellik için ücretli. 
* [LBFGS ile Lojistik regresyon](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) veya "logit" gerileme bağımlı değişken veri sınıflandırması yapmak için kategorik olduğunda kullanılabilecek bir regresyon modeli. LBFGS yarı-Newton iyileştirme algoritması, sınırlı bir bilgisayarın bellek miktarını Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritması benzeyen ve machine learning'de yaygın olarak kullanılan bir ' dir.
* [Rastgele ormanları](https://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçları Kümelemeler olan.  Bunlar overfitting riskini azaltmak için birçok karar ağaçları birleştirin. Rastgele ormanları regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir ve çok sınıflı sınıflandırma ayarı genişletilebilir. Bunlar, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özellik olanağına sahip olursunuz. Rastgele ormanları en başarılı makine öğrenimi için sınıflandırma ve regresyon modellerini biridir.
* [Gradyan boosted ağaçları](https://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) olan Kümelemeler karar ağaçları (GBTs). GBTs çalıştırmalarınızı kaybı işlevi en aza indirmek için karar ağaçları eğitin. GBTs regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özellik olanağına sahip olursunuz. Bir sınıflandırma veya çoklu sınıflar ayarında de kullanılabilir.

Modelleme adımları ayrıca her türü modeli eğitmek ve değerlendirmek nasıl gösteren kod içerir. Python kodu çözüm ve ilgili çizimleri göstermek için kullanıldı.   

> [!NOTE]
> Spark MLlib Araç Seti büyük veri kümelerinde çalışmak üzere tasarlanmış olsa da, nispeten küçük bir örnek (yaklaşık 30 Mb 170 bin satır, yaklaşık %0,1 özgün NYC veri kümesini kullanarak), burada kolaylık sağlamak için kullanılır. Burada verilen alıştırma 2 çalışan düğümü ile bir HDInsight kümesi üzerinde verimli bir şekilde (yaklaşık 10 dakika içinde) çalıştırır. Aynı kodla küçük değişiklikler, daha büyük veri kümeleri, verileri bellek içinde önbelleğe alma ve küme boyutunu değiştirmek için uygun değişiklikleri ile işlemek için kullanılabilir.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bir Azure hesabı ve Spark 1.6 (veya Spark 2.0) ihtiyacınız Bu izlenecek yolu tamamlamak için HDInsight kümesi. Bkz: [genel bakış, verilerin Azure HDInsight üzerinde Spark'ı kullanarak bilimi](spark-overview.md) bu gereksinimleri karşılamak yönergeler. Bu konu ayrıca açıklamasını burada kullanılan NYC 2013 taksi verileri ve Spark kümesinde Jupyter not defteri gelen kodu çalıştırmak yönergeler içerir. 

## <a name="spark-clusters-and-notebooks"></a>Spark kümeleri ve Not Defterleri
Bu izlenecek yolda, kurulum adımları ve kod kullanarak bir HDInsight Spark 1.6 için sağlanır. Ancak Jupyter not defterleri, kümeler, HDInsight Spark 1.6 hem de Spark 2.0 için sağlanır. Not defterlerini ve bağlantıları onlara bir açıklamasını sağlanan [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) bunları içeren GitHub deposu. Ayrıca, kodu buraya bağlı not defterlerinde geneldir ve herhangi bir Spark kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulum ve yönetim adımları ne burada gösterilenden biraz farklı olabilir. Kolaylık olması için Jupyter not defterlerini Spark 1.6'ı (Jupyter Notebook sunucusu pySpark Çekirdeği'nde çalışacak şekilde) ve (Jupyter Notebook sunucusu pySpark3 çekirdek içinde çalışacak şekilde) Spark 2.0 için bağlantılar şunlardır:

### <a name="spark-16-notebooks"></a>Spark 1.6 Not Defterleri

[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Veri keşfi, modelleme ve birkaç farklı algoritma ile Puanlama gerçekleştirme hakkında bilgi sağlar.

### <a name="spark-20-notebooks"></a>Spark 2.0 Not Defterleri
Spark 2.0 kümesi kullanarak uygulanan regresyon ve sınıflandırma ayrı not defterlerinde görevleridir ve farklı bir veri kümesi sınıflandırma not defteri kullanır:

- [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosya, veri keşfi, modelleme, gerçekleştirme konusunda bilgi sağlar ve Spark 2.0 Puanlama NYC taksi seyahat ve açıklanan taksi verileri kümesi kullanarak kümelerini [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Bu not defteri hızla Spark 2.0 için sağladık Kodu Keşfetme için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC taksi verileri analiz eder, bu listedeki sonraki not bakın. Bu not defterlerini karşılaştırma bu listeye aşağıdaki notlara bakın. 
- [Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu dosya denetimi veri (işlem), Spark SQL ve veri çerçevesi model ve puanlama NYC taksi seyahat ve açıklanan taksi verileri kümesi kullanarak keşif gerçekleştirmeyi gösterir [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosya, denetimi veri (işlem), Spark SQL ve veri keşfi, modelleme ve iyi bilinen Havayolu zamanında kalkış veri kümesinden 2011 ve 2012 kullanarak Puanlama yapma işlemi açıklanır. Bu hava durumu özellikleri modele dahil edilebilecek şekilde (örneğin, windspeed, sıcaklık, yükseklik vb.) havaalanı hava durumu verilerini Havayolu kümesiyle modelleme önce tümleştirdik.

<!-- -->

> [!NOTE]
> Havayolu veri kümesini sınıflandırma algoritmalarının kullanımını daha iyi anlamak için Spark 2.0 not defterleri için eklendi. Aşağıdaki bağlantıları kalkış veri kümesi ve hava durumu dataset zamanında Havayolu hakkında bilgi için bkz.
> 
> - Havayolu zamanında kalkış verileri: [https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Havaalanı hava durumu verileri: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 

<!-- -->

<!-- -->

> [!NOTE]
> Spark 2.0 not defterleri ile ilgili NYC taksi ve Havayolu uçuş gecikme veri kümeleri, 10 dakika veya (HDI kümenizin boyutuna bağlı olarak) çalıştırmak için daha fazla sürebilir. Yukarıdaki listede ilk not defterini veri keşfi, Görselleştirme ve ML model eğitiminin birçok yönden alt örneklenen NYC veri taksi ve taksi dosyalarını önceden birleştirilmiş silinmiş kümesi ile çalıştırmak için daha az zaman alan bir not defteri gösterir: [Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) bu not defteri (2-3 dakika) tamamlanması daha kısa bir zaman alır ve olması iyi bir başlangıç noktası için hızlı olması koşuluyla Kodu Keşfetme Spark 2.0 için. 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
> Spark 1.6 kullanımıyla ilgili aşağıdaki açıklamaları. Spark 2.0 sürümleri için lütfen açıklanmış ve yukarıdaki bağlı not defterlerini kullanma. 

<!-- -->

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulum: depolama konumları, kitaplıklar ve önceden oluşturulmuş Spark bağlamı
Spark, okuma ve yazma (WASB olarak da bilinir) Azure Storage Blobuna kuramıyor. Depolanan mevcut verilerinizi şekilde var. Spark ile yeniden WASB içinde depolanan sonuçları işlenebilir.

WASB içinde modelleri veya dosyaları kaydetmek için yolun düzgün bir şekilde belirtilmesi gerekiyor. Spark kümesine eklenen varsayılan kapsayıcı ile başlayan bir yol kullanılarak başvurulabilir: "wasb: / / /". Diğer konumlara tarafından başvurulan "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Dizin yolları depolama konumları WASB ayarlayın
Aşağıdaki kod örneği, okunacak verileri ve model çıktısını kaydedildiği modeli depolama dizini için yol konumunu belirtir:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Kitaplıkları içeri aktarma
Ayarlama, ayrıca gerekli kitaplıkları alma gerektirir. Spark bağlamını ayarlayın ve gerekli kitaplıkları aşağıdaki kod ile içeri aktarma:

    # IMPORT LIBRARIES
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
Jupyter not defterleri ile sağlanan PySpark çekirdekleri, önceden ayarlanmış bir bağlam yoktur. Bu nedenle bir Spark kümesi gerekmez veya açıkça, uygulama ile çalışmaya başlamadan önce Hive bağlamları geliştirme. Bu içerikler varsayılan olarak sizin için kullanılabilir. Şu bağlamlarda şunlardır:

* SC - Spark 
* sqlContext - Hive için

PySpark çekirdeği bazı önceden tanımlanmış "işlevlerini" ile çağırabileceğiniz özel komutlar olduğu sağlar %%. Bu kod örneklerinde kullanılan iki tür komutlar vardır.

* **%% yerel** sonraki satırların kodu yerel olarak yürütüleceğini belirtir. Kod, geçerli Python kodu olmalıdır.
* **%% sql -o \<değişken adı >** sqlContext bir Hive sorgusu çalıştırır. -O parametreye geçirilmişse, sorgu sonucu kalıcı hale getirilir %% Pandas DataFrame olarak yerel Python bağlamı.

Jupyter not defterleri ve önceden tanımlanmış çekirdekler hakkında daha fazla bilgi "magics için" sağlarlar, bkz: [için Jupyter not defterlerinde kullanılabilen çekirdekler HDInsight Spark Linux kümeleri HDInsight](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>Ortak blob veri alma
Veri bilimi işlemi ilk adımında kaynaklardan Analiz edilecek verileri alma, nerede olduğu veri keşfi ve modelleme ortamınıza yer alıyor. Spark Bu izlenecek yolda ortamıdır. Bu bölüm, bir görev dizisini tamamlamak için kodu içerir:

* model alınacak veri örnek alma
* Giriş veri kümesinde (.tsv dosyası olarak depolanır) okuyun
* biçimlendirme ve verileri temizleme
* oluşturma ve nesneleri (Rdd veya veri çerçevelerini) bellek içinde önbelleğe alma
* SQL bağlamında bir geçici tablo olarak kaydedin.

Veri alımı için kod aşağıdaki gibidir.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 51.72 saniye

## <a name="data-exploration--visualization"></a>Veri keşfi ve görselleştirme
Verileri Spark yönetilmeye başladıktan sonra veri bilimi işlemi sonraki adımda, veri keşfi ve görselleştirme aracılığıyla daha iyi anlayabilmek sağlamaktır. Bu bölümde, SQL sorgularını kullanarak taksi verileri incelemek ve görsel denetim için olası özellikleri ve hedef değişkenler çizim. Özellikle, yolcular sayıları taksi gelişlerin, ipucu miktarları sıklığını ve ipuçları, ödeme tutarı ve türüne göre nasıl değişiklik sıklığını gösterir.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogram yolcular sayısı frekansların taksi gelişlerin örneğinde Çiz
Bu kod ve sonraki kod parçacıkları örnek ve verileri çizmek için yerel Sihirli sorgulamak için SQL Sihri kullanın.

* **SQL Sihri (`%%sql`)** HDInsight PySpark çekirdeği kolay satır içi HiveQL sqlContext sorguları destekler. (-O deðiþken_adý) bağımsız değişkeni bir Pandas DataFrame Jupyter sunucuda olarak SQL sorgusunun çıktısını sürdürür. Başka bir deyişle, yerel modda kullanılabilir.
*  **`%%local` Sihirli** kod HDInsight küme baş düğümüne olan Jupyter sunucu üzerinde yerel olarak çalıştırmak için kullanılır. Genellikle, kullandığınız `%%local` birlikte Sihirli `%%sql` Sihirli -o parametresi. -O parametresi yerel SQL sorgusunun çıktılarını kalıcı hale getirme ve ardından %% yerel Sihirli yerel olarak karşı ve yerel olarak kalıcı çıkış SQL sorguları çalıştırmak için kod parçacığı bir sonraki kümesini tetikleme

Çıktı, kodu çalıştırdıktan sonra otomatik olarak görselleştirilir.

Bu sorgu gelişlerin yolcular sayısına göre alır. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Bu kod, sorgu çıktısı, yerel bir veri çerçevesi oluşturur ve verileri çizer. `%%local` Sihirli bir yerel veri çerçeve, oluşturur `sqlResults`, kullanılabilen ile matplotlib çizmek için. 

> [!NOTE]
> Bu PySpark Sihirli Bu izlenecek yolda birden çok kez kullanıldı. Veri miktarı büyükse, bir veri sığabilecek çerçeve yerel bellekte oluşturmaya yönelik örnek.
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Kodu yolcular sayımlarla gelişlerin çizmek için

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**ÇIKIŞ:**

![Seyahat sıklığı yolcular sayısına göre](./media/spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Kullanılarak birkaç farklı türde (tablo, pasta, çizgi, alan veya çubuk) görselleştirmeler arasında seçebilirsiniz **türü** düğmeleri Not. Çubuğu çizim burada gösterilir.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>İpucu miktarları ve ipucu tutarı yolcular sayısı ve taksi tutarları ile nasıl değişeceğini bir histogram gösterir.
Örnek veriler için bir SQL sorgusunu kullanın.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Bu kodu hücreyi, verileri üç çizimleri oluşturmak için SQL sorgusu kullanır.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**ÇIKIŞ:** 

![İpucu tutar dağıtımı](./media/spark-data-exploration-modeling/tip-amount-distribution.png)

![İpucu miktarda yolcular sayısı](./media/spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Taksi miktar tutarı İpucu](./media/spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Özellik Mühendisliği, dönüştürme ve veri hazırlığı modelleme
Bu bölümde açıklanmaktadır ve ML model kullanmak için veri hazırlamak için kullanılan yordamları için kodu sağlar. Bu, aşağıdaki görevlerin nasıl yapılacağını gösterir:

* Yeni bir özellik olarak gruplama saat trafiği zaman demetlerin içine oluşturun.
* Dizin ve kategorik özellikleri kodlayın
* ML işlevleri giriş etiketli noktası nesneleri oluşturma
* Rastgele bir alt örnekleme veri oluşturun ve eğitim ve test etme halinde bölme
* Ölçeklendirme özelliği
* Bellekte önbelleğe nesneler

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Yeni bir özellik olarak gruplama saat trafiği zaman demetlerin içine oluşturun.
Bu kod, yeni bir özellik olarak gruplama saat trafiği zaman demetlerin içine oluşturma ve sonuçta elde edilen veri çerçevesi bellekte önbelleğe alınacağını gösterir. Dayanıklı Dağıtılmış veri kümesi (Rdd) ve veri çerçevelerini sürekli olarak kullanıldığı yerlerde, önbelleğe alma için geliştirilmiş yürütme sürelerini yol açar. Buna göre biz Rdd ve veri çerçevelerini izlenecek yol çeşitli aşamalarında önbelleğe alır. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**ÇIKIŞ:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Dizin ve kodlama işlevleri modelleme içine girişi için kategorik özellikleri
Bu bölümde, dizin veya kodlama giriş modelleme işlevleri için kategorik özellikleri gösterilmektedir. Modelleme ve tahmin MLlib işlevlerini dizine veya kullanılmadan önce kodlanmış için kategorik girdi verilerini özelliklerle gerektirir. Modeline bağlı olarak, dizin veya onları farklı şekillerde kodlama yapmanız gerekir:  

* **Ağaç tabanlı modelleme** sayısal değerler kodlanacak kategorileri gerektirir (örneğin, üç kategoriye sahip bir özellik kodlanmış olabilecek 0, 1, 2). Bu, MLlib tarafından 's sağlanır [StringIndexer](https://spark.apache.org/docs/latest/ml-features.html#stringindexer) işlevi. Bu işlev bir dize sütunu etiketi frekans tarafından sıralanan etiket dizinleri içeren bir sütun için bir etiket kodlar. Giriş ve veri işleme için sayısal değerlerle dizine olsa da, bunları uygun şekilde kategori olarak ele almanız için ağaç tabanlı algoritmalar belirtilebilir. 
* **Mantıksal ve doğrusal regresyon modellerini** gerektiren bir seyrek kodlama, where, örneğin, üç kategoriye sahip bir özellik içeren her 0 veya 1 gözlemi kategorisine bağlı olarak üç özellik sütunlara genişletilebilir. MLlib sağlar [OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) bir seyrek kodlama yapmak için işlevi. Bu Kodlayıcı etiket dizinleri içeren bir sütun ikili vektörler, en fazla bir değerle tek bir-bir sütunu eşlenir. Bu kodlama beklediğiniz gibi kategorik özellikleri uygulanacak Lojistik regresyon, sayısal değerli özellikler algoritmalar sağlar.

Dizin ve kategorik özellikleri kodlamak için kod aşağıdaki gibidir:

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 1.28 saniye

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>ML işlevleri giriş etiketli noktası nesneleri oluşturma
Bu bölüm etiketli noktası veri türü olarak kategorik metin verileri ve eğitme ve test MLlib Lojistik regresyon ve diğer sınıflandırma modelleri için kullanılabilir, böylece kodlayamadığı gösteren kod içerir. Etiketli noktası, dayanıklı Dağıtılmış veri kümeleri (RDD) girdi verisi olarak MLlib ML algoritmaları çoğu için gerekli olan bir şekilde biçimlendirilmiş nesneleridir. A [noktası etiketli](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) yerel bir vektör, yoğun ya da seyrek, etiket/yanıt ile ilişkilidir.  

Bu bölümde kategorik metin verileri olarak dizinleme gösteren kod içeren bir [noktası etiketli](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) veri türüne dönüştürün ve eğitme ve test MLlib Lojistik regresyon ve diğer sınıflandırma modelleri için kullanılabilir, böylece kodlayamadığı. Etiketli noktası, dayanıklı Dağıtılmış veri kümeleri (RDD) bir etiket (hedef/yanıt değişken) ve özellik vektör nesneleridir. Bu biçim, MLlib birçok ML algoritmaları tarafından giriş olarak gereklidir.

Aşağıda, dizin ve ikili sınıflandırma özellikleri metin kodlama için kodu verilmiştir.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC REGRESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Kodlanacak ve kategorik metin özellikleri doğrusal regresyon çözümleme için dizin kod aşağıdaki gibidir.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Rastgele bir alt örnekleme veri oluşturun ve eğitim ve test etme halinde bölme
Bu kod, rastgele bir örnekleme (% 25 burada kullanılır) veri oluşturur. Bu örnekte veri kümesinin boyutu nedeniyle gerekli olmamasına karşın, gerektiğinde kendi sorunu için kullanma bilmesi nasıl, burada örnek oluşturabilirsiniz gösterir. Örnekleri büyük olduğunda bu eğitimi modellerini sırasında önemli zamandan tasarruf edebilirsiniz. Sonraki biz örnek bir eğitim bölümü (burada %75) ve test bölümü (% 25 oranında burada) sınıflandırma ve regresyon modelleme bölün.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 0.24 saniye

### <a name="feature-scaling"></a>Ölçeklendirme özelliği
Veri normalleştirme da bilinen özellik ölçeklendirme, yaygın olarak yapılan değerlerle özellikleri olan belirli bir aşırı ağırlık, hedef işlevi oluşturmasını sağlar. Kodu için kullandığı ölçeklendirme özelliğini [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) birim varyansı özellikleri ölçeklendirmek için. Doğrusal regresyon ile Stokastik gradyan düşüşü (SGD), diğer makine öğrenimi modellerini regularized gerilemeleri veya destek vektör makineler (SVM) gibi çok çeşitli eğitim için popüler bir algoritma kullanmak için MLlib tarafından sağlanır.

> [!NOTE]
> Ölçeklendirme özelliğini hassas olmasını LinearRegressionWithSGD algoritması bulduk.
> 
> 

Ölçek değişkenlere regularized doğrusal SGD algoritması ile kullanmak için kod aşağıdaki gibidir.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

Hücre yürütülmesi için geçen süre: 13.17 saniye

### <a name="cache-objects-in-memory"></a>Bellekte önbelleğe nesneler
Eğitim ve ML algoritmaları test için geçen süre, nesneleri özellikleri ölçeği ve Sınıflandırma, regresyon, kullanılan giriş veri çerçevesi önbelleğe alarak azaltılabilir.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:** 

Hücre yürütülmesi için geçen süre: 0,15 saniye

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>İpucu ile ikili sınıflandırma modellerini Ücretli olup olmadığını tahmin edin
Bu bölüm, ipucu, taksi yolculuğu için ücretli olup olmadığını tahmin etmek için ikili sınıflandırma görevini nasıl üç kullanım modelleri gösterir. Sunulan modeller şunlardır:

* Regularized Lojistik regresyon 
* Rastgele orman modeli
* Gradyan artırırken ağaçları

Her model kod bölümünde oluşturmaya adımlarına ayrılmıştır: 

1. **Eğitim modeli** bir parametre kümesi ile verileri
2. **Model değerlendirme** ölçümlerle test veri kümesinde
3. **Model kaydediliyor** gelecekteki kullanım için BLOB

### <a name="classification-using-logistic-regression"></a>Sınıflandırma Lojistik regresyon kullanma
Bu bölümdeki kod Eğitimi, değerlendirin ve bir Lojistik regresyon modeli ile kaydetme işlemi gösterilmektedir [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) tahmin eden bir ipucu NYC taksi seyahat ve taksi veri kümesinde bir gidiş dönüş için ücretli olup olmadığını.

**CV ve hiper parametre Süpürme kullanarak Lojistik regresyon modelini eğitme**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇIKIŞ:** 

Katsayılar: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e - 05,-0.00247646947233,-0.00165897881503 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921, - 0.664263411392-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Hücre yürütülmesi için geçen süre: 14.43 saniye

**Standart ölçümlerle ikili sınıflandırma modeli değerlendirme**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ÇIKIŞ:** 

Çekme isteği alanında 0.985297691373 =

ROC alanında 0.983714670256 =

Özet istatistikleri

Duyarlık 0.984304060189 =

Geri çağırma 0.984304060189 =

F1 Puan 0.984304060189 =

Hücre yürütülmesi için geçen süre: 57.61 saniye

**ROC eğrisi çizebilirsiniz.**

*PredictionAndLabelsDF* bir tablo olarak kayıtlı *tmp_results*, önceki hücrenin. *tmp_results* sorgular ve sonuçları çıkış sqlResults veri çerçevesine çizmek için kullanılabilir. Kod aşağıdaki gibidir.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Aşağıda, tahminlerde bulunabilir ve ROC eğrisi çizmek için kodu verilmiştir.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**ÇIKIŞ:**

![Lojistik regresyon ROC curve.png](./media/spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a>Rastgele orman sınıflandırma
Bu bölümdeki kod eğitmek, değerlendirin ve ipucu NYC taksi seyahat ve taksi veri kümesinde bir gidiş dönüş için ücretli olup olmadığını tahmin eden bir rastgele orman modeli kaydetme işlemi gösterilmektedir.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

ROC alanında 0.985297691373 =

Hücre yürütülmesi için geçen süre: 31.09 saniye

### <a name="gradient-boosting-trees-classification"></a>Gradyan artırırken ağaçları sınıflandırma
Bu bölümdeki kod Eğitimi, değerlendirmek ve bir ipucu NYC taksi Seyahatteki bir gidiş dönüş için ücretli olup olmadığını tahmin eden bir gradyan artırırken ağaçları modeli kaydedin ve veri kümesi masrafları gösterilmektedir.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇIKIŞ:**

ROC alanında 0.985297691373 =

Hücre yürütülmesi için geçen süre: 19.76 saniye

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Regresyon modelleri ile taksi gelişlerin ipucu tutarlarının tahmin edin
Bu bölüm, nasıl diğer ipucu özelliklerini temel alarak bir taksi seyahat için gerileme görevini bahşiş miktarını tahmin etmek için üç model kullanmak Ücretli gösterir. Sunulan modeller şunlardır:

* Regularized doğrusal regresyon
* Rastgele orman
* Gradyan artırırken ağaçları

Bu modeller giriş açıklandığı gibi. Her model kod bölümünde oluşturmaya adımlarına ayrılmıştır: 

1. **Eğitim modeli** bir parametre kümesi ile verileri
2. **Model değerlendirme** ölçümlerle test veri kümesinde
3. **Model kaydediliyor** gelecekteki kullanım için BLOB

### <a name="linear-regression-with-sgd"></a>SGD ile doğrusal regresyon
Bu bölümdeki kod ölçeği özellikleri iyileştirme için stokastik aşama (SGD) kullanan bir doğrusal regresyon eğitmek için nasıl kullanılacağını ve nasıl Puanlama, değerlendirin ve Azure Blob Storage (WASB) modeli kaydedin gösterir.

> [!TIP]
> Deneyimimizde, yakınsama LinearRegressionWithSGD modelleri ile ilgili sorunlar olabilir ve parametreleri değiştirildi/geçerli bir model dikkatli bir şekilde almak için en iyi duruma getirilmiş olmanız gerekir. Değişkenler önemli ölçüde ölçeklendirme yakınsamalı yardımcı olur. 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

Katsayılar: [0.00457675809917-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,- 0.00990211159703-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Kesme noktası: 0.853872718283

RMSE 1.24190115863 =

R sqr 0.608017146081 =

Hücre yürütülmesi için geçen süre: 58.42 saniye

### <a name="random-forest-regression"></a>Rasgele orman regresyon
Bu bölümdeki kod Eğitimi, değerlendirmek ve NYC taksi seyahat verilerini ipucunu tutarındaki tahmin eden bir rastgele orman gerileme kaydetme işlemi gösterilmektedir.

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

RMSE 0.891209218139 =

R sqr 0.759661334921 =

Hücre yürütülmesi için geçen süre: 49.21 saniye

### <a name="gradient-boosting-trees-regression"></a>Gradyan artırırken ağaçları regresyon
Bu bölümdeki kod NYC taksi seyahat verilerini ipucunu tutarındaki tahmin eden bir gradyan artırırken ağaçları modeli kaydedin eğitme ve değerlendirme işlemi gösterilmektedir.

**Eğitme ve değerlendirme**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVERT RESULTS TO DF AND REGISTER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKIŞ:**

RMSE 0.908473148639 =

R sqr 0.753835096681 =

Hücre yürütülmesi için geçen süre: 34.52 saniye

**Çizim**

*tmp_results* önceki hücrenin bir Hive tablosunda olarak kaydedilir. Tablodan sonuçlar çıktı içine *sqlResults* çizmek için veri çerçeve. Kod aşağıdaki gibidir

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Jupyter sunucu kullanarak verilerini çizmek için kod aşağıdaki gibidir.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


**ÇIKIŞ:**

![Gerçek-vs-öngörülen-ipucu-tutarları](./media/spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a>Bellek nesneleri Temizle
Kullanım `unpersist()` nesneleri bellekte önbelleğe alınmış silinemiyor.

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Kayıt depolama konumları için kullanım ve puanlama modelleri
Kullanmasına ve bağımsız bir veri kümesi açıklanan puanlamak için [puanı ve Spark'a yerleşik machine learning modellerini değerlendirme](spark-model-consumption.md) konu, kopyalama ve yapıştırma burada tüketim oluşturulan kayıtlı modelleri içeren bu dosya adları için ihtiyacınız Jupyter not defteri. Aşağıda, ihtiyacınız olan model dosyalara olan yolları yazdırmak için kodu verilmiştir.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**ÇIKIŞ**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"

## <a name="whats-next"></a>Sırada ne var?
Spark MlLib ile oluşturduğunuz regresyon ve sınıflandırma modelleri, Puanlama ve bu modellerin değerlendirmesi hakkında bilgi edinmek hazır olursunuz. Gelişmiş Veri keşfi ve modelleme not defteri çapraz doğrulama, hyper-Süpürme saldırısı yapılabilir, parametresini dahil eden derin türlerine geçiyor ve değerlendirme model. 

**Model tüketimi:** Puanlama ve bu konu başlığında oluşturduğunuz sınıflandırma ve regresyon modellerini değerlendirme konusunda bilgi almak için bkz: [puanı ve Spark'a yerleşik machine learning modellerini değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hiper parametre Süpürme**: Bkz: [Gelişmiş Veri keşfi ve modelleme Spark ile](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabileceğini üzerinde çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitim

