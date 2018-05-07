---
title: Veri keşfi ve modelleme Spark ile | Microsoft Docs
description: Azure üzerinde Spark Mllib'i Araç Seti veri keşfi ve modelleme yetenekleri gösterir.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: b989b918-5ba5-4696-b8d0-76ae510a23f4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath
ms.openlocfilehash: d505d432897eb7f4e792b45a95ec91ce1802eeb8
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="data-exploration-and-modeling-with-spark"></a>Spark ile veri keşfi ve modelleme
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

Bu kılavuzda Hdınsight Spark veri keşfi yapmak için kullanır ve ikili sınıflandırma ve görevleri NYC örneğinde modelleme regresyon Seyahat Ücreti 2013 dataset masrafları.  Adımlarda size yol gösterir [veri bilimi işlemi](http://aka.ms/datascienceprocess), uçtan uca, veri ve modelleri depolamak için bir Hdınsight Spark kümesi işleme ve Azure BLOB'ları için kullanma. İşlem bir Azure Storage Blobundan getirildi veri visualizes inceler ve Tahmine dayalı modelleri oluşturmak için verileri hazırlar. Bu modeller ikili sınıflandırma ve regresyon modelleme görevleri gerçekleştirmek için Spark Mllib'i Araç Seti kullanarak yapı ' dir.

* **İkili sınıflandırma** görevdir ipucu seyahat için ücretli olsun veya olmasın tahmin etmek için. 
* **Regresyon** görevdir diğer ipucu özelliklerini temel alarak ipucu miktarı tahmin etmek için. 

Kullanırız modelleri Lojistik ve doğrusal regresyon, rastgele ormanları ve gradyan boosted ağaçları şunlardır:

* [Doğrusal regresyon SGD ile](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) Stokastik gradyan düşüşü (SGD) yöntemini kullanan doğrusal regresyon modeli ve ipucu tutarlar tahmin etmek için ölçeklendirme en iyi duruma getirme ve özellik için ücretli. 
* [LBFGS ile Lojistik regresyon](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) veya "logit" regresyon bağımlı değişken veri sınıflandırması yapmak için kategorik olduğunda kullanılabilen bir regresyon modeli. LBFGS sınırlı bir bilgisayarın bellek miktarını kullanarak Broyden – Fletcher'dan – Goldfarb – Shanno (BFGS) algoritması benzeyen ve machine learning'de yaygın olarak kullanılan bir yarı-Newton iyileştirme algoritması ' dir.
* [Rastgele ormanlar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçları ensembles şunlardır.  Bunlar overfitting riskini azaltmak için birçok karar ağaçları birleştiren. Rastgele ormanlar regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir ve çok sınıflı sınıflandırma ayarı genişletilebilir. Bunlar özelliği ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özelliği. Rastgele ormanlar en başarılı makine öğrenimi modellerini sınıflandırma ve regresyon biridir.
* [Gradyan boosted ağaçları](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) olan karar ağaçları ensembles. GBTs tekrarlayarak kaybı işlevi en aza indirmek için karar ağaçları eğitmek. GBTs regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özelliği. Bir sınıflandırma veya çoklu sınıflar ayarında de kullanılabilir.

Model oluşturma adımları ayrıca eğitmek, değerlendirmek ve her türde bir model kaydetmek nasıl gösteren kod içerir. Python çözümü kod ve ilgili çizimleri göstermek için kullanılmış.   

> [!NOTE]
> Spark Mllib'i Araç Seti büyük veri kümelerinde çalışmak üzere tasarlanmıştır ancak görece küçük bir örnek (yaklaşık 30 170 K satır, yaklaşık özgün NYC dataset %0,1 kullanarak Mb) buraya kolaylık sağlamak için kullanılır. Burada verilen alıştırma verimli bir şekilde (yaklaşık 10 dakika cinsinden) 2 çalışan düğümleri ile bir Hdınsight kümesi üzerinde çalışır. Aynı kodla küçük değişiklikler, daha büyük veri bellekte önbelleğe alma ve küme boyutunu değiştirmek için uygun değişiklikleri-kümeleri, verileri işlemek için kullanılabilir.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bir Azure hesabı ve Spark 1.6 (veya Spark 2.0) ihtiyacınız bu yönlendirmeyi tamamlamak için Hdınsight kümesi. Bkz: [genel bakış, verileri Azure Hdınsight'ta Spark kullanmanın Bilim](spark-overview.md) yönelik bu gereksinimleri karşılamak yönergeler. Bu konu ayrıca açıklamasını buraya kullanılan NYC 2013 ücreti verileri ve Spark kümesinde Jupyter not defteri gelen kod yürütmek yönergeler içerir. 

## <a name="spark-clusters-and-notebooks"></a>Spark kümeleri ve dizüstü bilgisayarlar
Kurulum adımlarını ve kod bu kılavuzda bir Hdınsight Spark 1.6 kullanmak için sağlanır. Ancak Jupyter not defterleri Hdınsight Spark 1.6 ve Spark 2.0 kümeleri için sağlanır. Not defterlerini ve bağlantılarını bir açıklaması verilmiştir [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) bunları içeren GitHub deposunu için. Ayrıca, kodu buraya ve bağlantılı not defterlerini geneldir ve tüm Spark kümesi üzerinde çalışması gerekir. Hdınsight Spark kullanmıyorsanız küme kurulum ve yönetim adımlar ne burada gösterilenden biraz farklı olabilir. Kolaylık olması için Spark (Jupyter not defteri sunucunun pySpark Çekirdeği'nde çalıştırılacak) 1.6 ve Spark 2. 0'ı (Jupyter not defteri sunucunun pySpark3 Çekirdeği'nde çalıştırılacak) için Jupyter not defterleri bağlantılar şunlardır:

### <a name="spark-16-notebooks"></a>Spark 1.6 dizüstü bilgisayarlar

[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Provides information on how to perform data exploration, modeling, and scoring with several different algorithms.

### <a name="spark-20-notebooks"></a>Spark 2.0 dizüstü bilgisayarlar
Bir Spark 2.0 kümesi kullanılarak uygulanan regresyon ve sınıflandırma görevler ayrı not defterlerinde ve sınıflandırma dizüstü bilgisayar farklı bir veri kümesi kullanır:

- [Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosyayı veri keşfi, modelleme, gerçekleştirmeyle ilgili bilgi sağlar ve Spark 2. 0'Puanlama NYC ücreti seyahat kullanarak kümelerini ve ücreti verileri-set açıklanan [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Bu Not hızlı bir şekilde Spark 2.0 için sağladık kod keşfetme için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC ücreti verileri analiz eder, bu listedeki sonraki dizüstü bakın. Bu not defterlerini karşılaştırmak bu liste aşağıdaki notlara bakın. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu dosyayı nasıl wrangling verileri (işlem), Spark SQL ve dataframe modelleme ve NYC ücreti seyahat ve açıklanan ücreti veri kümesini kullanarak Puanlama araştırması gerçekleştirileceğini gösterir [burada ](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosyayı wrangling verileri (işlem), Spark SQL ve dataframe modelleme ve bilinen uçak zamanında ayrılma kullanarak Puanlama araştırması gerçekleştirmek nasıl gösterir veri kümesi 2011 ve 2012. Bu hava durumu özellikleri modele dahil edilebilir biz havaalanı hava durumu verileri (ör. windspeed, sıcaklık, yükseklik vb.) içeren hava yolu veri kümesi modelleme önce tümleşik.

<!-- -->

> [!NOTE]
> Uçak dataset sınıflandırma algoritmalarının kullanımını daha iyi anlamak için Spark 2.0 dizüstü bilgisayarlar için eklenmiştir. Aşağıdaki bağlantılar ayrılma dataset ve hava durumu dataset zamanında hava yolu hakkında bilgi için bkz:

>- Uçak zamanında ayrılma verileri: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Havaalanı hava durumu verileri: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
NYC ücreti ve uçak uçuş gecikme veri kümeleri üzerinde Spark 2.0 not defterlerini 10 dakika veya (HDI kümesi boyutuna bağlı olarak) çalıştırmak için daha fazla sürebilir. Yukarıdaki listede ilk dizüstü veri keşfi pek çok görünüşünün gösterir, Görselleştirme ve ML model eğitim aşağı örneklenen NYC veri içinde ücreti ve ücreti dosyaları önceden birleştirilmiş kümesi ile çalıştırmak için daha az zaman alan bir Not: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) bu not (2-3 dakika) son çok daha kısa sürer ve olması iyi bir başlangıç noktası hızla biz olması koşuluyla Spark 2.0 için kod keşfetme için. 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
Açıklamaları Spark 1.6 kullanmayla ilgilidir. Spark 2.0 sürümleri için lütfen açıklanmıştır ve yukarıdaki bağlı not defterlerini kullanın. 

<!-- -->

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulumu: depolama konumları, kitaplıklar ve hazır Spark bağlamı
Spark okuyabilmesini ve Azure Storage Blobuna (WASB olarak da bilinir) yazma. Varolan verilerinizi depolanan şekilde var. Spark ve yeniden WASB içinde depolanan sonuçları kullanarak işlenebilir.

Modelleri veya dosyaları içinde WASB kaydetmek için yolun düzgün belirtilmesi gerekiyor. Spark kümeye eklenen varsayılan kapsayıcı ile başlayan bir yol kullanarak başvurulabilir: "wasb: / / /". Başka konumlara tarafından başvurulan "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Dizin yolları için depolama konumları WASB ayarlayın
Aşağıdaki kod örneği okunacak veriler ve model çıkış kaydedildiği modeli depolama dizini için yol konumunu belirtir:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Kitaplıkları içeri aktarma
Ayarlama, ayrıca gerekli kitaplıkları içeri aktarma gerektirir. Spark bağlamını ayarlayın ve aşağıdaki kod ile gerekli kitaplıkları içeri aktarma:

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


### <a name="preset-spark-context-and-pyspark-magics"></a>Spark bağlamını ve PySpark sihirler hazır
Jupyter not defterleri ile sağlanan PySpark tekrar önceden belirlenmiş bir içerik var. Bu nedenle Spark kümesi gerekmez veya açıkça uygulama ile çalışmaya başlamadan önce Hive bağlamları geliştirme. Bu içerikler varsayılan olarak sizin için kullanılabilir. Bu içerikler şunlardır:

* SC - Spark 
* sqlContext - Hive için

Bazı önceden tanımlanmış "sihirleri" ile çağırabilir özel komutlar olduğu PySpark çekirdeği sağlar %%. Bu kod örneklerinde kullanılan olan iki komut vardır.

* **%% yerel** sonraki satırların kodda yerel olarak yürütüleceğini belirtir. Kod geçerli Python kodu olmalıdır.
* **%% sql -o <variable name>**  sqlContext bir Hive sorgusu yürütür. -O parametre verilmezse, sorgunun sonucu kalıcı hale getirilir %% Pandas DataFrame olarak yerel Python bağlamı.

Tekrar Jupyter not defterlerini ve önceden tanımlanmış hakkında daha fazla bilgi "magics için" sağladıkları, bkz: [Jupyter not defterlerinde kullanılabilen çekirdekler Hdınsight Spark Linux kümeleri Hdınsight'ta](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>Ortak blob gelen veri alımı
İlk veri bilimi işlemi kaynaklardan analiz için verileri almak için adımdır nerede olduğu veri keşfi ve modelleme ortamınıza yer alıyor. Spark bu kılavuzda ortamıdır. Bu bölümde, bir dizi görevi tamamlamak için kod içerir:

* model alınacak veri örneği alma
* girdi veri kümesi (.tsv dosyası olarak depolanır) okuyun
* biçimlendirmek ve veri temizleme
* oluşturma ve nesneleri (RDDs veya veri çerçevelerini) bellekte önbelleğe alma
* SQL bağlam tabloda geçici olarak kaydeder.

Veri alımı için kod aşağıda verilmiştir.

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

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 51.72 saniye

## <a name="data-exploration--visualization"></a>Veri keşfi & Görselleştirme
Veri Spark alındıktan sonra sonraki veri bilimi işlemi araştırması ve görselleştirme verilerine daha derinden anlayabilmek için adımdır. Bu bölümde, SQL sorgularını kullanarak ücreti verileri incelemek ve hedef değişkenleri ve görsel İnceleme için olası özellikleri çizmek. Özellikle, yolcu sayıları ücreti dönüşleri, ipucu tutarlar sıklığını ve nasıl ipuçları ödeme tutar ve türüne göre farklılık sıklığını çizmek.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogram yolcu sayısı frekansların ücreti dönüşleri örnekteki Çiz
Bu kodu ve sonraki parçacıkları verileri çizmek için yerel Sihirli ve örnek sorgu için SQL Sihirli kullanın.

* **SQL Sihirli (`%%sql`)** Hdınsight PySpark çekirdeği sqlContext kolay satır içi HiveQL sorguları destekler. (-O deðiþken_adý) bağımsız değişkeni devam ederse SQL sorgusu çıktısını Pandas DataFrame Jupyter sunucuda olarak. Bu, yerel modda kullanılabilir olduğu anlamına gelir.
* **`%%local` Sihirli** kod Hdınsight kümesi headnode olan Jupyter sunucuda yerel olarak çalıştırmak için kullanılır. Genellikle, kullandığınız `%%local` birlikte Sihirli `%%sql` - o parametresiyle Sihirli. -O parametresiyle SQL sorgusu yerel olarak çıktısını kalıcı ve ardından %% yerel Sihirli karşı ve yerel olarak kalıcı çıkış SQL sorguları, yerel olarak çalıştırmak için kod parçacığını bir sonraki kümesini tetiklemek

Çıkış kodu çalıştırdıktan sonra otomatik olarak görünür.

Bu sorgu dönüşleri yolcu sayısına göre alır. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Bu kod bir yerel veri çerçevesi sorgu çıktısından oluşturur ve veri çizer. `%%local` Sihirli oluşturur çerçeve, yerel veri `sqlResults`, kullanılabileceği ile matplotlib çizdirmek için. 

> [!NOTE]
> Bu PySpark Sihirli birden çok kez bu kılavuzda kullanılır. Veri miktarını büyükse, yerel bellekte bir veri sığabilecek çerçeve oluşturmak için örnek.
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Dönüş yolcu sayımlarla çizmek için kodu

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

**ÇIKTI:**

![Yolcu sayısına göre seyahat sıklığı](./media/spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Kullanarak birkaç farklı türde görselleştirmeleri (tablo, pasta, çizgi, alan veya çubuğu) arasında seçebilirsiniz **türü** menü düğmelerini Not. Çubuğu çizim burada gösterilir.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Çizim histogram ipucu tutarlar ve nasıl ipucu tutar yolcu sayısı ve ücreti tutarlar göre değişir.
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


Bu kod hücresini üç çizimleri verileri oluşturmak için SQL sorgusu kullanır.

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


**ÇIKTI:** 

![İpucu tutar dağıtımı](./media/spark-data-exploration-modeling/tip-amount-distribution.png)

![Yolcu sayısına göre ipucu tutar](./media/spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![İpucu tutar ücreti miktar](./media/spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Modelleme mühendislik, dönüştürme ve veri hazırlığı özelliği
Bu bölümde açıklar ve ML modelleme kullanmak için verileri hazırlamak için kullanılan yordamlar için kod sağlar. Aşağıdaki görevlerin nasıl yapılacağını gösterir:

* Yeni bir özellik tarafından binning saatleri trafiği zaman demet oluşturun.
* Dizin ve kategorik özellikleri kodlama
* ML işlevleri giriş etiketli noktası nesneleri oluşturma
* Rastgele bir alt örnekleme veri oluşturun ve eğitim ve test kümesi olarak bölme
* Ölçeklendirme özelliği
* Bellek önbelleği nesneleri

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Yeni bir özellik tarafından binning saatleri trafiği zaman demet oluşturun.
Bu kod, yeni bir özellik tarafından binning saatleri trafiği zaman demet oluşturma ve sonra sonuçta elde edilen veri çerçevesi bellekte önbelleğe almak nasıl gösterir. Esnek Dağıtılmış veri kümeleri (RDDs) ve veri çerçevelerini art arda kullanıldığı önbelleğe alma için geliştirilmiş yürütme sürelerinin yol açar. Buna göre biz RDDs ve veri çerçevelerini Kılavuzu çeşitli aşamalarında önbelleğe alır. 

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

**ÇIKTI:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Dizin ve işlevleri modelleme içine girişi için kategorik özellikleri kodlama
Bu bölümde, dizin veya modelleme işlevleri giriş için kategorik özellikleri kodlamak gösterilmektedir. Modelleme ve Mllib'i işlevlerini gerektiren dizine veya öncesinde kullanım kodlanmış kategorik giriş verisi özelliklerle tahmin etmek. Modeline bağlı olarak, dizin veya farklı şekillerde kodlamak gerekir:  

* **Ağaç tabanlı modelleme** sayısal değerleri olarak kodlanması için kategoriler gerektirir (örneğin, üç kategoride özelliğiyle kodlanması 0, 1, 2). Bu Mllib'i tarafından 's sağlanan [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) işlevi. Bu işlev bir dize sütunu etiket etiket sıklıklarını tarafından sıralanan etiket dizinlerini sütununa kodlar. Giriş ve veri işleme için sayısal değerleri içeren dizine rağmen ağaç tabanlı algoritmalar bunları uygun şekilde kategoriler olarak işlemek için belirtilebilir. 
* **Lojistik ve doğrusal regresyon modeli** bir hot kodlaması, gerektiren nerede, örneğin, üç kategoriye sahip bir özellik içeren her 0 veya 1 bir gözlem kategorisini bağlı olarak üç özellik sütunlara genişletilebilir. Mllib'i sağlar [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) bir hot kodlama yapmak için işlevi. Bu Kodlayıcı ikili vektörler, en çok bir değerle tek bir-bir sütunu etiketi dizinlerini sütunun eşler. Bu kodlama kategorik özellikleri uygulanacak Lojistik regresyon gibi sayısal değerli özellikleri beklediğiniz algoritmaları sağlar.

Dizini oluşturmak ve kategorik özellikleri kodlamak için kod aşağıdaki gibidir:

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

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 1.28 saniye

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>ML işlevleri giriş etiketli noktası nesneleri oluşturma
Bu bölümde kategorik metin veri etiketli noktası veri türü olarak dizin ve böylece eğitme ve test Mllib'i Lojistik regresyon ve diğer sınıflandırma modelleri için kullanılabilir kodlamak gösterilmektedir kodunu içerir. Etiketli noktası, esnek Dağıtılmış veri kümeleri (RDD) girdi verisi olarak Mllib'i ML algoritmalara çoğu tarafından gerektiği şekilde biçimlendirilmiş nesneleridir. A [noktası etiketli](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) yerel bir vektör, yoğun veya seyrek, etiket/yanıt ile ilişkilidir.  

Bu bölümde kategorik metin verileri olarak dizin gösteren kodu içeren bir [noktası etiketli](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) veri yazın ve böylece eğitme ve test Mllib'i Lojistik regresyon ve diğer sınıflandırma modelleri için kullanılabilir kodlama. Etiketli noktası, esnek Dağıtılmış veri kümeleri (RDD) bir etiket (hedef/yanıt değişken) ve özellik vektör oluşan nesneleridir. Bu biçim Mllib'i içinde birçok ML algoritması tarafından giriş olarak gereklidir.

Aşağıda, dizin ve ikili sınıflandırma için metin özellikleri kodlamak için kodu verilmiştir.

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

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
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


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Rastgele bir alt örnekleme veri oluşturun ve eğitim ve test kümesi olarak bölme
Bu kod bir rastgele örnekleme (% 25 burada kullanılır) veri oluşturur. Bu örnek veri kümesi için boyutu gerekli olmamasına karşın, gerektiğinde kendi sorunu için kullanmak üzere nasıl bilmesi nasıl, burada örnek oluşturabilirsiniz göstermektedir. Örnekleri büyük olduğunda bu eğitim modelleri sırasında önemli zamandan tasarruf edebilirsiniz. Sonraki biz örnek sınıflandırma ve regresyon modelleme kullanmak için eğitim bölümü (burada %75) ve bir test bölümü (% 25 burada) bölün.

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

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 0.24 saniye

### <a name="feature-scaling"></a>Ölçeklendirme özelliği
Özellik ölçeklendirme, veri normalleştirme da bilinen, yaygın olarak yapılan değerlerle özellikleri olan belirli bir aşırı tartmanız olduğunu hedefi işlevinde oluşturmasını sağlar. Kullandığı ölçeklendirme özelliğini kodunu [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) birim farkı özellikleri ölçeklendirmek için. Doğrusal regresyon ile Stokastik gradyan düşüşü (SGD), diğer machine learning modellerini regularized gerileme veya destek vektör makineler (SVM) gibi çeşitli eğitim için yaygın olarak kullanılan bir algoritma kullanmak için Mllib'i tarafından sağlanır.

> [!NOTE]
> Ölçeklendirme özelliği hassas olarak LinearRegressionWithSGD algoritması bulduk.
> 
> 

Burada, Ölçek değişkenlere regularized doğrusal SGD algoritması ile kullanmak için kodu verilmiştir.

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

**ÇIKTI:**

Hücre yürütülmesi için geçen süre: 13.17 saniye

### <a name="cache-objects-in-memory"></a>Bellek önbelleği nesneleri
Eğitim ve ML algoritmalardan sınamak için harcanan süre nesneleri için sınıflandırma, regresyon, kullanılan ve özellikleri ölçeklendirilmiş giriş verisi çerçeve önbelleğe alarak azaltılabilir.

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

**ÇIKTI:** 

Hücre yürütülmesi için geçen süre: 0,15 saniye

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Bir ipucu ile ikili sınıflandırma modelleri Ücretli olsun veya olmasın tahmin etme
Bu bölümde, bir ipucu ücreti seyahat için ücretli olup olmadığına bakılmaksızın kullanım üç tahmin için ikili sınıflandırma görevi nasıl modeller gösterir. Sunulan model şunlardır:

* Regularized Lojistik regresyon 
* Rastgele orman modeli
* Gradyan artırma ağaçları

Her model kod bölümünde oluşturma adımları ayrılır: 

1. **Eğitim modeli** bir parametre kümesi ile verileri
2. **Model değerlendirme** ölçümlerle sınama veri kümesi üzerinde
3. **Model kaydetme** gelecekteki tüketimi için blob içinde

### <a name="classification-using-logistic-regression"></a>Sınıflandırma Lojistik regresyon kullanma
Bu bölümdeki kod eğitmek, değerlendirmek ve lojistik regresyon modeli ile kaydetmek nasıl gösterir [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) tahmin ipucu seyahat kümesindeki NYC ücreti seyahat ve ücreti ödenen olsun veya olmasın.

**MS ve hyperparameter Süpürme kullanarak Lojistik regresyon modelini eğitme**

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


**ÇIKTI:** 

Katsayısını: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Hücre yürütülmesi için geçen süre: 14.43 saniye

**Standart ölçümlerle ikili sınıflandırma modelini değerlendir**

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

**ÇIKTI:** 

PR alanında 0.985297691373 =

ROC alanında 0.983714670256 =

Özet istatistikleri

Duyarlık 0.984304060189 =

Geri çağırma 0.984304060189 =

F1 Puan 0.984304060189 =

Hücre yürütülmesi için geçen süre: 57.61 saniye

**ROC eğrisi çizme.**

*PredictionAndLabelsDF* bir tablo olarak kayıtlı *tmp_results*, önceki hücrenin. *tmp_results* sorgular ve çıktı sonuçları sqlResults veri çerçeveye çizdirmek için kullanılabilir. Kod aşağıdaki gibidir.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Burada, tahminlerde ve ROC eğrisi çizmek için kodu verilmiştir.

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


**ÇIKTI:**

![Lojistik regresyon ROC curve.png](./media/spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a>Rastgele orman sınıflandırma
Bu bölümdeki kod, eğitme, değerlendirmek ve ipucu seyahat kümesindeki NYC ücreti seyahat ve ücreti ödenen olsun veya olmasın tahmin rastgele orman modeli kaydedin gösterilmektedir.

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

**ÇIKTI:**

ROC alanında 0.985297691373 =

Hücre yürütülmesi için geçen süre: 31.09 saniye

### <a name="gradient-boosting-trees-classification"></a>Gradyan artırma ağaçları sınıflandırma
Bu bölümdeki kod, eğitme, değerlendirmenize ve bir ipucu NYC ücreti Seyahatteki seyahat için ücretli olsun veya olmasın tahmin bir gradyan artırma ağaçları modeli kaydedin ve veri kümesi masrafları gösterilmektedir.

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


**ÇIKTI:**

ROC alanında 0.985297691373 =

Hücre yürütülmesi için geçen süre: 19.76 saniye

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Regresyon modellerle ücreti dönüşleri ipucu tutarlarının tahmin etme
Bu bölümde, nasıl diğer ipucu özelliklerini temel alarak ücreti seyahat için kullanım üç modelleri ipucu miktarı tahmin etmeye yönelik regresyon görevi için ücretli gösterir. Sunulan model şunlardır:

* Regularized doğrusal regresyon
* Rastgele orman
* Gradyan artırma ağaçları

Bu modeller girişte açıklandığı gibi. Her model kod bölümünde oluşturma adımları ayrılır: 

1. **Eğitim modeli** bir parametre kümesi ile verileri
2. **Model değerlendirme** ölçümlerle sınama veri kümesi üzerinde
3. **Model kaydetme** gelecekteki tüketimi için blob içinde

### <a name="linear-regression-with-sgd"></a>Doğrusal regresyon SGD ile
Bu bölümdeki kod genişletilmiş özellikler için en iyi duruma getirme stokastik gradyan düşüşü (SGD) kullanan bir doğrusal regresyon eğitmek için nasıl kullanılacağını ve nasıl puan, değerlendirmek ve Azure Blob Storage (WASB) modeli kaydedin gösterir.

> [!TIP]
> Deneyimi bizim LinearRegressionWithSGD modelleri yakınsama sorunları olabilir ve parametreleri değiştirilen/geçerli bir model dikkatle almak için en iyi duruma getirilmiş olması gerekir. Değişkenleri önemli ölçüde ölçeklendirmeyi yakınsama yardımcı olur. 
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

**ÇIKTI:**

Katsayısını: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Intercept: 0.853872718283

RMSE 1.24190115863 =

R sqr 0.608017146081 =

Hücre yürütülmesi için geçen süre: 58.42 saniye

### <a name="random-forest-regression"></a>Rastgele orman regresyon
Bu bölümdeki kod, eğitme, değerlendirmek ve ipucu tutar NYC ücreti seyahat veriler için tahmin rastgele orman regresyon Kaydet gösterilmektedir.

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

**ÇIKTI:**

RMSE 0.891209218139 =

R sqr 0.759661334921 =

Hücre yürütülmesi için geçen süre: 49.21 saniye

### <a name="gradient-boosting-trees-regression"></a>Gradyan artırma ağaçları regresyon
Bu bölümdeki kod, eğitme, değerlendirmek ve ipucu tutar NYC ücreti seyahat veriler için tahmin bir gradyan artırma ağaçları modeli kaydedin gösterilmektedir.

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

    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇIKTI:**

RMSE 0.908473148639 =

R sqr 0.753835096681 =

Hücre yürütülmesi için geçen süre: 34.52 saniye

**Çizim**

*tmp_results* önceki hücrenin Hive tablo olarak kaydedilir. Tablodan sonuçlar halinde çıkış *sqlResults* çizdirmek için veri çerçeve. Kodu

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Jupyter sunucu kullanarak verileri çizmek için kod aşağıdaki gibidir.

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


**ÇIKTI:**

![Fiili-vs-tahmin-ipucu-tutarlar](./media/spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a>Bellek nesneleri Temizle
Kullanım `unpersist()` bellekte önbelleğe alınmış nesneleri silmek için.

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


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Kullanım ve puanlama modellerini kayıt depolama konumları
Kullanabilir ve açıklanan bağımsız bir veri kümesini puan [puanı ve Spark yerleşik machine learning modellerini değerlendirme](spark-model-consumption.md) konu, kopyalama ve yapıştırma burada tüketim Jupyter not defteri oluşturulan kaydedilmiş modelleri içeren bu dosya adları için ihtiyacınız. Burada, var. gereksinim modeli dosyaları yollara yazdırmak için kodu verilmiştir.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**ÇIKTI**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"

## <a name="whats-next"></a>Sırada ne var?
Regresyon ve sınıflandırma modelleri ile Spark Mllib'i oluşturduğunuza göre Puanlama ve bu modeller değerlendirmek öğrenmek hazırsınız. Gelişmiş Veri keşfi ve not defteri modelleme çapraz doğrulama, hyper-yerleştirmez, parametre dahil eden daha derin çekecek ve değerlendirme model. 

**Model tüketimi:** puan ve bu konuda oluşturulan sınıflandırma ve regresyon modelleri değerlendirmek öğrenmek için bkz: [puanı ve Spark yerleşik makine öğrenimi modellerini değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hyperparameter Süpürme**: bkz [veri keşfi ve modelleme Spark ile Gelişmiş](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabilir üzerinde çapraz doğrulama ve parametre hyper Süpürme kullanılarak eğitilmiş

