---
title: Azure üzerinde Scala ve Spark kullanarak veri bilimi | Microsoft Docs
description: Denetimli makine öğrenimi görevlerini Spark ölçeklenebilir Mllib'i ve Spark ML paketleri ile bir Azure Hdınsight Spark kümesinde Scala kullanılmak üzere nasıl.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 8f8b252d8771dff23d0a8c89e057fc17ba321a65
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="data-science-using-scala-and-spark-on-azure"></a>Azure üzerinde Scala ve Spark kullanan Veri Bilimi
Bu makalede Scala için denetimli makine öğrenimi görevlerini Spark ölçeklenebilir Mllib'i ve Spark ML paketleri ile bir Azure Hdınsight Spark kümesinde nasıl kullanıldığını gösterir. Oluşturduğunu görevlerinde anlatılmaktadır [veri bilimi işlem](http://aka.ms/datascienceprocess): veri alımı ve keşfi, görselleştirme, özellik Mühendisliği, model ve model tüketim. Makaleyi modellerinde Lojistik ve doğrusal regresyon, rastgele ormanları ve gradyan boosted ağaçları (GBTs) yanı sıra iki ortak denetimli makine öğrenimi görevlerini içerir:

* Regresyon sorunu: Tahmin ücreti seyahat ipucu tutar ($)
* İkili sınıflandırma: tahmin ipucu veya ücreti seyahat için ipucu yok (1/0)

Model oluşturma işlemi, eğitim ve sınama veri kümesi ve ilgili doğruluğu ölçümleri değerlendirme gerektirir. Bu makalede, bu modeller Azure Blob storage'da depolamak nasıl ve puanlama ve Tahmine dayalı kendi performansını değerlendirmek nasıl öğrenebilirsiniz. Bu makalede, çapraz doğrulama ve parametre hyper Süpürme kullanarak modelleri iyileştirmek nasıl daha gelişmiş konular yer almaktadır. Kullanılan verileri 2013 NYC ücreti seyahat ve ücreti veri kümesinin Github'da bulunan bir örnektir.

[Scala](http://www.scala-lang.org/), Java sanal makineye dayalı bir dil nesne yönelimli ve işlevsel dil kavramları tümleştirir. Bu, bulutta dağıtılan işleme için uygundur ve Azure Spark kümeleri üzerinde çalışan ölçeklenebilir bir dildir.

[Spark](http://spark.apache.org/) büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen bir açık kaynak paralel işleme altyapısıdır. Spark işleme altyapısı hızı, kullanımı kolay, gelişmiş analizler için yerleşik olarak bulunur. Spark'ın bellek içi dağıtılmış hesaplama özellikleri machine learning ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim yapın. [Spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) paket yardımcı olabilecek çerçeveleri oluşturmak ve ardışık düzen öğrenme pratik makine ayarlamak veri üstünde yerleşik yüksek düzey API'leri Tekdüzen kümesi sağlar. [Mllib'i](http://spark.apache.org/mllib/) dağıtılmış bu ortama modelleme yetenekleri getirir Spark'ın ölçeklenebilir machine learning kitaplığı.

[Hdınsight Spark](../../hdinsight/spark/apache-spark-overview.md) açık kaynak Spark Azure barındırılan sunulması değil. Ayrıca Spark kümesinde Jupyter Scala dizüstü bilgisayarlar için destek içerir ve dönüştürmek için filtre ve Azure Blob depolamada depolanan verileri görselleştirmek için Spark SQL etkileşimli sorguları çalıştırabilirsiniz. Çözümler sunar ve verileri görselleştirmek için ilgili çizimleri Göster Scala kod parçacıkları bu makalede yüklü üzerinde Spark kümeleri Jupyter not defterleri çalıştırın. Bu konularda modelleme adımlarda eğitmek için değerlendirmek, kaydetme ve her türde bir model tüketen gösterir koduna sahip.

Bu makaledeki kod ve kurulum adımları için Azure Hdınsight 3.4 Spark 1.6 var. Ancak, bu makalede ve buna kod [Scala Jupyter not defteri](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) geneldir ve tüm Spark kümesi üzerinde çalışması gerekir. Küme kurulumu ve Yönetimi adımları Hdınsight Spark kullanmıyorsanız ne bu makalede gösterilenden biraz farklı olabilir.

> [!NOTE]
> Scala yerine Python uçtan uca veri bilimi işlemi için görevleri tamamlamak için nasıl kullanılacağını gösteren bir konuya bakın [veri bilimi Azure Hdınsight'ta Spark kullanmanın](spark-overview.md).
> 
> 

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure aboneliğiniz olmalıdır. Zaten bir yoksa [Azure ücretsiz deneme sürümünü edinin](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Aşağıdaki yordamları tamamlamak için bir Azure Hdınsight 3.4 Spark 1.6 kümesi gerekir. Bir küme oluşturmak için deki yönergelere bakın [Başlarken: Azure hdınsight'ta Apache Spark oluşturma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Küme türü ve sürümü Ayarla **küme türü seçin** menüsü.

![Hdınsight küme türü yapılandırma](./media/scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

Spark kümesinde Jupyter not defteri gelen kod yürütmek yönergeler ve NYC ücreti seyahat veri açıklaması için ilgili bölümlere bakın [genel bakış, verileri Azure Hdınsight'ta Spark kullanmanın Bilim](spark-overview.md).  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spark kümesinde Jupyter not defteri gelen Scala kodu yürütme
Jupyter not defteri Azure portalından başlatabilirsiniz. Panonuz Spark kümesinde bulun ve Yönetim sayfasında, kümeniz için girmek için tıklatın. Bundan sonra öğesini **küme panolarında**ve ardından **Jupyter not defteri** Spark kümesi ile ilişkili not defteri açın.

![Küme panosu ve Jupyter Not Defterleri](./media/scala-walkthrough/spark-jupyter-on-portal.png)

Jupyter not defterleri https:// sırasında da erişebilirsiniz&lt;clustername&gt;.azurehdinsight.net/jupyter. Değiştir *clustername* , küme adı. Jupyter not defterlerini erişmek, yönetici hesabı için parola gerekir.

![Küme adını kullanarak Jupyter not defterleri için Git](./media/scala-walkthrough/spark-jupyter-notebook.png)

Seçin **Scala** PySpark API kullanan paketlenmiş not defterlerini birkaçı sahip bir dizini görmek için. Modelleme ve kod içerir Scala.ipynb Not Defteri kullanarak Puanlama araştırması örnekleri üzerinde Spark konular bu paketi kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).

Spark kümesinde Jupyter not defteri sunucusuna doğrudan github'dan not defteri karşıya yükleyebilirsiniz. Jupyter giriş sayfanızda tıklatın **karşıya** düğmesi. Dosya Gezgini'nde Scala not defteri GitHub (Ham içerik) URL'sini yapıştırın ve ardından **açık**. Scala Not Defteri, aşağıdaki URL'de kullanılabilir:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Kurulumu: Hazır Spark ve Hive bağlamları, Spark sihirler ve Spark kitaplıkları
### <a name="preset-spark-and-hive-contexts"></a>Spark ve Hive bağlamları hazır
    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Jupyter not defterleri ile sağlanan Spark tekrar bağlamları önceden. Açıkça Spark ayarlamanız gerekmez veya uygulama ile çalışmaya başlamadan önce Hive bağlamları geliştirme. Hazır bağlamları şunlardır:

* `sc` SparkContext için
* `sqlContext` HiveContext için

### <a name="spark-magics"></a>Spark sihirler
Bazı önceden tanımlanmış "sihirleri" ile çağırabilir özel komutlar olduğu Spark çekirdek sağlar `%%`. Bu komutların iki aşağıdaki kod örnekleri kullanılır.

* `%%local` sonraki satırların kodda yerel olarak yürütülecek belirtir. Kod geçerli Scala kodu olmalıdır.
* `%%sql -o <variable name>` bir Hive sorgusu yürütür `sqlContext`. Varsa `-o` parametresi geçirilir, sorgunun sonucu kalıcı hale getirilir `%%local` Scala bağlamı Spark veri çerçeve olarak.

İle arama, tekrar Jupyter not defterlerini ve bunların önceden tanımlanmış hakkında daha fazla bilgi "magics için" `%%` (örneğin, `%%local`), bkz: [Jupyter not defterlerinde kullanılabilen çekirdekler Hdınsight Spark Linux kümeleri Hdınsight](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma
Spark, Mllib'i ve aşağıdaki kodu kullanarak gerekir diğer kitaplıkları içeri aktarın.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Veri alımı
Veri bilimi sürecinde ilk adım, çözümlemek istediğiniz veri alma olmaktır. Verilerin dış kaynaklara veya sistemlerinden bulunduğu veri keşfi ve modelleme ortamınıza duruma getirin. Bu makalede, alma veri birleştirilmiş %0,1 (.tsv dosyası olarak depolanır) ücreti seyahat ve ücreti dosya örneğidir. Veri keşfi ve modelleme Spark ortamıdır. Bu bölümde aşağıdaki görev dizisini tamamlamak için kod içerir:

1. Depolama veri ve model için dizin yolu olarak ayarlayın.
2. Veri kümesindeki veriler giriş (.tsv dosyası olarak depolanır) okuyun.
3. Veriler için bir şema tanımlayabilir ve veri temizleme.
4. Temizlenen veri çerçevesi oluşturun ve bellekte önbelleğe.
5. Veri SQLContext geçici tablo olarak kaydedin.
6. Tablo Sorgu ve veri çerçeveye sonuçları alın.

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Dizin yolları depolama konumları için Azure Blob depolama alanına ayarlayın
Spark okuma ve Azure Blob depolama alanına yazma. Varolan verilerinizi işlemek için Spark kullanın ve sonra sonuçları Blob storage'da depolamak.

Modelleri veya dosyaları Blob depolama alanına kaydetmek için doğru yolu belirtmeniz gerekir. İle başlayan bir yolu kullanarak Spark kümeye eklenen varsayılan kapsayıcı başvuru `wasb:///`. Diğer konumları kullanarak başvuru `wasb://`.

Aşağıdaki kod örneği okumak için giriş verileri ve Spark kümeye eklenen Blob Depolama Birimi yolu model kaydedileceği konumu belirtir.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Veri alma, bir RDD oluşturun ve veri çerçevesi şema göre tanımlayın
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıktı:**

Hücre çalıştırma süresi: 8 saniye.

### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Tablo Sorgu ve veri çerçevesinde sonuçları Al
Ardından, tablo ücreti, yolcu ve ipucu veri için sorgu; bozuk ve harici verilerini filtre; ve birkaç satır yazdırın.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Çıktı:**

| fare_amount | passenger_count | tip_amount | Eğimli |
| --- | --- | --- | --- |
|        13.5 |1.0 |2.9 |1.0 |
|        16.0 |2.0 |3.4 |1.0 |
|        10.5 |2.0 |1.0 |1.0 |

## <a name="data-exploration-and-visualization"></a>Veri keşfi ve görselleştirme
Spark verileri aldıktan sonra sonraki veri bilimi işleminde araştırması ve görselleştirme verilerine daha derin bir anlayış kazanmak için adımdır. Bu bölümde, SQL sorguları kullanarak ücreti verileri inceleyin. Ardından, sonuçlar Jupyter otomatik görselleştirme özelliğini kullanarak visual İnceleme için olası özellikleri ve hedef değişkenleri çizmek için bir veri çerçevesi alın.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Yerel ve SQL Sihirli verileri çizmek için kullanın
Varsayılan olarak, Jupyter not defteri çalıştırmak herhangi kod parçacığını çıktısını çalışan düğümlerine kalıcı oturum bağlamında kullanılabilir. Her hesaplama çalışan düğümleri için bir seyahat kaydedin ve hesaplama için gereken tüm verileri (Bu baş düğüm) yerel olarak Jupyter sunucu düğümünde kullanılabilir ise, kullanabileceğiniz istiyorsanız `%%local` Sihirli üzerinde Jupyter kod parçacığında çalıştırmak için Sunucu.

* **SQL Sihirli** (`%%sql`). Hdınsight Spark çekirdek SQLContext kolay satır içi HiveQL sorguları destekler. (`-o VARIABLE_NAME`) Bağımsız değişkeni devam ederse SQL sorgusu çıktısını Jupyter sunucuda Pandas veri çerçeve olarak. Başka bir deyişle, yerel modda kullanılabilir olması.
* `%%local` **Sihirli**. `%%local` Sihirli kodu yerel olarak Hdınsight küme baş düğümüne olan Jupyter sunucuda çalışır,. Genellikle, kullandığınız `%%local` birlikte Sihirli `%%sql` ile Sihirli `-o` parametresi. `-o` Parametresi SQL sorgusu yerel olarak çıktısını kalıcı ve ardından `%%local` Sihirli karşı ve yerel olarak kalıcı çıkış SQL sorguları, yerel olarak çalıştırmak için kod parçacığını bir sonraki kümesini tetiklemek.

### <a name="query-the-data-by-using-sql"></a>SQL kullanarak verileri Sorgulama
Bu sorgu ücreti tutarı, yolcu sayısı ve ipucu tutarı tarafından ücreti dönüşleri alır.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Aşağıdaki kodda, `%%local` Sihirli sqlResults bir yerel veri çerçevesi oluşturur. SqlResults matplotlib kullanarak çizmek için kullanabilirsiniz.

> [!TIP]
> Yerel Sihirli birden çok kez bu makalede kullanılır. Veri kümenizi büyük olursa, lütfen yerel belleğe sığması veri çerçevesi oluşturmak için örnek.
> 
> 

### <a name="plot-the-data"></a>Veri Çiz
Yerel bağlamı Pandas veri çerçeve olarak veri çerçevesi olduktan sonra Python kodu kullanarak çizebilirsiniz.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Kod çalıştırdıktan sonra Spark çekirdek (HiveQL) SQL sorguları çıktısını otomatik olarak visualizes. Görselleştirmeleri çeşitli türleri arasında seçim yapabilirsiniz:

* Tablo
* Pasta
* Çizgi
* Alan
* Çubuk

Verileri çizmek için kod aşağıdaki gibidir:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Çıktı:**

![İpucu tutar histogram](./media/scala-walkthrough/plot-tip-amount-histogram.png)

![Yolcu sayısına göre ipucu tutar](./media/scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![İpucu tutar ücreti miktar](./media/scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Özellikler oluşturmak ve özellikleri dönüştürme ve veri işlevleri modelleme içine girişi için hazırla
Spark ML ve Mllib'i ağaç tabanlı modelleme işlevleri için hedef ve özellik binning, dizin oluşturma, bir seyrek kodlama ve vectorization gibi teknikler çeşitli kullanarak hazırlamanız gerekir. Bu bölümde izlemek için yordamlar şunlardır:

1. Yeni bir özellik tarafından oluşturma **binning** trafiği saate demet saat.
2. Uygulama **dizin oluşturma ve bir hot kodlama** kategorik özelliklerine.
3. **Örnek ve veri kümesinin bölme** eğitim ve test kesirler içine.
4. **Eğitim değişken ve Özellikler belirtmek**, eğitim kodlanmış dizinlenmiş veya bir dinamik sonra oluşturmak ve esnek giriş etiketli noktası sınama Dağıtılmış veri kümeleri (RDDs) veya veri çerçevesi.
5. Otomatik olarak **kategorilere ayırmak ve özellikleri ve hedefleri vectorize** makine öğrenimi modellerini için girdi olarak kullanılacak.

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Yeni bir özellik tarafından binning saatleri trafiği zaman demet oluşturun.
Bu kod, yeni bir özellik tarafından binning saatleri trafiği zaman demet oluşturma ve sonuçta elde edilen veri çerçevesi bellekte önbelleğe almak nasıl gösterir. RDDs ve veri çerçevelerini tekrar tekrar kullanıldığı geliştirilmiş yürütme sürelerinin müşteri adaylarına önbelleğe alma. Buna, RDDs ve veri çerçevelerini aşağıdaki yordamları çeşitli aşamalarında önbelleğe alacağız.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Dizin oluşturma ve bir hot kategorik özelliklerini kodlama
Modelleme ve Mllib'i işlevlerini gerektiren dizine veya öncesinde kullanım kodlanmış kategorik giriş verisi özelliklerle tahmin etmek. Bu bölümde dizin veya modelleme işlevleri giriş için kategorik özellikleri kodlamak gösterilmektedir.

Dizin veya modele bağlı olarak, farklı şekillerde Modellerinizi kodlamak gerekir. Örneğin, bir seyrek kodlama Lojistik ve doğrusal regresyon modeli gerektirir. Örneğin, üç kategoride özelliğiyle üç özellik sütunlara genişletilebilir. Her sütun, 0 veya 1 bir gözlem kategorisini bağlı olarak içerecektir. Mllib'i sağlar [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) bir hot kodlama için işlevi. Bu Kodlayıcı ikili vektörlerinin en çok bir değerle tek bir-bir sütunu etiketi dizinlerini sütunun eşler. Bu kodlama ile Lojistik regresyon gibi sayısal değerli özellikleri beklediğiniz algoritmaları kategorik özellikleri uygulanabilir.

Burada karakter dizelerdir örnekler göstermek için yalnızca dört değişkenleri dönüştürün. Kategorik değişkenleri olarak sayısal değerleri tarafından temsil edilen diğer gibi değişkenleri hafta içi günü, ayrıca dizin oluşturabilirsiniz.

Dizin oluşturma için kullanmak `StringIndexer()`ve bir hot kodlaması için `OneHotEncoder()` Mllib'i işlevlerden. Dizini oluşturmak ve kategorik özellikleri kodlamak için kod aşağıdaki gibidir:

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıktı:**

Hücre çalıştırma süresi: 4 saniye.

### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Örnek ve eğitim ve test kesirler veri kümesine Böl
Bu kod bir rastgele örnekleme veri (Bu örnekte, %25) oluşturur. Örnekleme Bu örnekte veri kümesinin boyutu gerekli olmamasına karşın, makaleyi gerektiğinde kendi sorunları kullanma bilmesi nasıl, örnek oluşturabilirsiniz gösterir. Örnekleri büyük olduğunda modelleri eğitme sırada bu önemli zaman kazanabilirsiniz. Ardından, örnek bir eğitim (Bu örnekte, %75) ve bir test bölümlerini (Bu örnekte, %25) sınıflandırma ve regresyon modelleme kullanılacak bölün.

(0 ve 1 arasında) rastgele bir sayı ("rand" sütunundaki) eğitim sırasında çapraz doğrulama Katlama seçmek için kullanılan her satır ekleyin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıktı:**

Hücre çalıştırma süresi: 2 saniye.

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Eğitim değişken ve özellikleri belirtin ve sonra eğitim ve giriş noktası RDDs ya da veri çerçevelerini etiketli test kodlanmış dizinli veya bir hot oluşturun
Bu bölümde kategorik metin veri etiketli noktası veri türü olarak dizin ve eğitmek ve Mllib'i Lojistik regresyon ve diğer sınıflandırma modelleri test etmek için kullanabileceğiniz şekilde kodlamak gösterilmiştir kodunu içerir. Etiketli noktası, girdi verisi olarak machine learning algoritmaları Mllib'i çoğu tarafından gerektiği şekilde biçimlendirilmiş RDDs nesneleridir. A [noktası etiketli](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) yerel bir vektör, yoğun veya seyrek, etiket/yanıt ile ilişkilidir.

Bu kodda hedef (bağımlı) değişkeni ve modeli eğitmek için kullanmak için özellikleri belirtin. Sonra eğitim ve giriş noktası RDDs ya da veri çerçevelerini etiketli test kodlanmış dizinlenmiş veya bir hot oluşturursunuz.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıktı:**

Hücre çalıştırma süresi: 4 saniye.

### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Otomatik olarak kategorilere ayırmak ve özellikleri ve makine öğrenimi modellerinin oluşturulmasına için girdi olarak kullanılacak hedefleri vectorize
Ağaç tabanlı modelleme işlevlerini kullanmak için özellikler ve hedef kategorilere ayırmak için Spark ML kullanın. Kod iki görevleri tamamlar:

* Sınıflandırma için ikili hedef 0 veya 1 değerini her veri noktası 0 ile 1 arasında bir eşik değeri 0,5 kullanarak atayarak oluşturur.
* Otomatik olarak özellikleri kategorilere ayırır. Bu özellik, herhangi bir özellik için farklı sayısal değerleri sayısı 32'den az ise, kategorilere ayrılmıştır.

Bu iki görevler için kod aşağıdaki gibidir.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>İkili sınıflandırma modeli: bir ipucu Ücretli olup olmadığını tahmin etme
Bu bölümde, bir ipucu Ücretli olsun veya olmasın tahmin etmek için ikili sınıflandırma modelleri üç tür oluşturun:

* A **Lojistik regresyon modeli** Spark ML kullanarak `LogisticRegression()` işlevi
* A **rastgele orman sınıflandırma modeli** Spark ML kullanarak `RandomForestClassifier()` işlevi
* A **gradyan artırma ağacı sınıflandırma modeli** Mllib'i kullanarak `GradientBoostedTrees()` işlevi

### <a name="create-a-logistic-regression-model"></a>Lojistik regresyon modeli oluşturma
Ardından, Spark ML kullanarak Lojistik regresyon modeli oluşturma `LogisticRegression()` işlevi. Bir dizi adımı kod oluşturma modeli oluşturun:

1. **Modeli eğitmek** bir parametre kümesi ile verileri.
2. **Modeli değerlendirin** ölçümlerle sınama veri kümesi üzerinde.
3. **Modeli kaydedin** gelecekteki tüketimi için Blob Depolama birimindeki.
4. **Modeli Puanlama** karşı test verileri.
5. **Sonuçları çizim** özelliği (ROC) Eğriler işletim alıcı ile.

Bu yordamlar için kod aşağıdaki gibidir:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Yük, Puanlama ve sonuçları kaydedebilirsiniz.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Çıktı:**

Test verileri ROC 0.9827381497557599 =

Python ROC eğrisi çizmek için yerel Pandas veri kareleri kullanın.

    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
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


**Çıktı:**

![İpucu veya hiçbir ipucu ROC eğrisi](./media/scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a>Rastgele orman sınıflandırma modeli oluşturma
Ardından, Spark ML kullanarak bir rastgele orman sınıflandırma modeli oluşturma `RandomForestClassifier()` işlev ve modelin test verileri değerlendirin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Çıktı:**

Test verileri ROC 0.9847103571552683 =

### <a name="create-a-gbt-classification-model"></a>GBT sınıflandırma modeli oluşturma
Ardından, Mllib'i'nın kullanarak GBT sınıflandırma modeli oluşturma `GradientBoostedTrees()` işlev ve modelin test verileri değerlendirin.

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Çıktı:**

ROC eğrisi alanında: 0.9846895479241554

## <a name="regression-model-predict-tip-amount"></a>Regresyon modeli: ipucu miktarı tahmin etmek
Bu bölümde, iki tür ipucu miktarı tahmin etmek için regresyon modeli oluşturun:

* A **regularized doğrusal regresyon modeli** Spark ML kullanarak `LinearRegression()` işlevi. Modeli kaydedin ve test veri modelini değerlendir.
* A **gradyan artırmanın ağacı regresyon modeli** Spark ML kullanarak `GBTRegressor()` işlevi.

### <a name="create-a-regularized-linear-regression-model"></a>Regularized doğrusal regresyon modelini oluşturma
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıktı:**

Hücre çalıştırma süresi: 13 saniye.

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Çıktı:**

R-sqr test verileri 0.5960320470835743 =

Ardından, test sonuçları verileri çerçeve olarak sorgu ve onu görselleştirmek için AutoVizWidget ve matplotlib kullanabilirsiniz.

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Kod bir yerel veri çerçevesi sorgu çıktısından oluşturur ve veri çizer. `%%local` Sihirli oluşturur yerel veri çerçeve `sqlResults`, hangi matplotlib ile çizmek için kullanabilirsiniz.

> [!NOTE]
> Bu Spark Sihirli birden çok kez bu makalede kullanılır. Veri miktarını büyükse, yerel belleğe sığması veri çerçevesi oluşturmak için örnek.
> 
> 

Çizimler, Python matplotlib kullanarak oluşturun.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Çıktı:**

![Tutar İpucu: Gerçek ve tahmin edilen](./media/scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a>Bir GBT regresyon modeli oluşturma
Spark ML kullanarak bir GBT regresyon modeli oluşturma `GBTRegressor()` işlev ve modelin test verileri değerlendirin.

[Gradyan boosted ağaçları](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) olan karar ağaçları ensembles. GBTs tekrarlayarak kaybı işlevi en aza indirmek için karar ağaçları eğitmek. GBTs regresyon ve sınıflandırma için kullanabilirsiniz. Bunlar kategorik özellikleri işleyebilir, özellik ölçeklendirme gerektirmez ve nonlinearities ve özellik etkileşimleri yakalayabilirsiniz. Bunları bir sınıflandırma veya çoklu sınıflar ayarını da kullanabilirsiniz.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Çıktı:**

Test R-sqr olduğu: 0.7655383534596654

## <a name="advanced-modeling-utilities-for-optimization"></a>En iyi duruma getirme için Gelişmiş modelleme yardımcı programları
Bu bölümde, geliştiricilerin modeli iyileştirme için sık kullandığınız machine learning yardımcı programlarını kullanın. Özellikle, makine öğrenimi modellerini üç farklı yolla parametre Süpürme ve çapraz doğrulama kullanarak en iyi duruma getirebilirsiniz:

* Veri eğitimi ve doğrulama ayarlar bölme, eğitim kümesinde hyper-parametre Süpürme kullanarak model iyileştirmek ve doğrulama kümesinde (doğrusal regresyon) değerlendir
* Çapraz doğrulama ve hyper-Spark ML'ın CrossValidator işlevi (ikili sınıflandırma) kullanarak yerleştirmez parametresini kullanarak model en iyi duruma getirme
* İşlev ve parametre kümesi (doğrusal regresyon) öğrenme herhangi bir makineye kullanmak için özel çapraz doğrulama ve parametre Süpürme kod kullanarak model en iyi duruma getirme

**Çapraz doğrulama** ne kadar iyi bilinen bir veri kümesi üzerinde eğitilmiş bir model veri kümeleri üzerinde eğitilmedi özelliklerini tahmin etmek için generalize değerlendirir bir tekniktir. Genel Bu teknik arkasındaki bir model bilinen veri bir veri kümesinde eğitildi ve kendi tahminleri doğruluğunu bağımsız bir veri kümesi karşı sonra test olur. Bir ortak bir veri kümesine bölmek için uygulamasıdır *k*-Katlama ve hepsini şekilde Katlama biri dışındaki tüm modeli eğitmek.

**Hyper-parametre iyileştirme** kümesiyle genellikle bir ölçü bağımsız bir veri kümesi üzerinde algoritması'nın performansını en iyi duruma getirme amacı hyper-parametrelerini bir öğrenme algoritması seçme sorunudur. Parametre hyper dışında model eğitim yordamı belirtmelisiniz bir değerdir. Hyper-parametre değerleri hakkında varsayımlar esneklik ve modelin doğruluğunu etkileyebilir. Karar ağaçları hyper-parametreleri, örneğin, istenen derinliği ve bırakır ağacında sayısı gibi var. Destek vektör makinesi (SVM) misclassification cezası terim ayarlamanız gerekir.

Hyper-parametre iyileştirme gerçekleştirmek için yaygın bir yolu olarak da adlandırılan bir kılavuz arama kullanmaktır bir **parametresi tarama**. Bir kılavuz aramada ayrıntılı aramasını belirtilen bir alt bir öğrenme algoritması hyper-parametre alan değerlerini aracılığıyla gerçekleştirilir. Çapraz doğrulama çıkışı kılavuz arama algoritması tarafından üretilen en iyi sonuçları sıralamak için bir performans ölçümü sağlayabilir. Çapraz doğrulama parametre hyper Süpürme kullanırsanız, eğitim veri modeline overfitting gibi sınırı sorunları yardımcı olabilir. Bu şekilde, model, eğitim verileri ayıklandı veri genel kümesine uygulamak için kapasite korur.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Doğrusal regresyon modeli parametre hyper Süpürme ile en iyi duruma getirme
Ardından, veri eğitimi ve doğrulama kümeleri, kullan hyper-model en iyi duruma getirme ve bir doğrulama kümesi (doğrusal regresyon) değerlendirmek için Eğitim kümesi yerleştirmez parametresi bölün.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Çıktı:**

Test R-sqr olduğu: 0.6226484708501209

### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Çapraz doğrulama ve parametre hyper Süpürme kullanarak ikili sınıflandırma modeli en iyi duruma getirme
Bu bölümde bir ikili sınıflandırma modeli çapraz doğrulama ve parametre hyper Süpürme kullanarak iyileştirmek nasıl gösterir. Bu Spark ML kullanır `CrossValidator` işlevi.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıktı:**

Hücre çalıştırma süresi: 33 saniye.

### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Özel çapraz doğrulama ve parametre Süpürme kod kullanarak doğrusal regresyon modeli en iyi duruma getirme
Ardından, özel kod kullanarak model iyileştirmek ve en yüksek doğruluk ölçütü kullanarak en iyi modeli parametreleri tanımlar. Sonra son model oluşturun, modelin test verileri değerlendirmek ve Blob depolama alanına modeli kaydedin. Son olarak, modeli yüklemek, test verileri puan ve doğruluk değerlendirin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Çıktı:**

Hücre çalıştırma süresi: 61 saniye.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Spark yerleşik makine öğrenimi modellerini Scala ile otomatik olarak kullanma
Azure veri bilimi işlemi oluşturan görevler size yol konuları genel bakış için bkz: [takım veri bilimi işlemi](http://aka.ms/datascienceprocess).

[Ekip veri bilimi süreci gözden geçirmeleri](walkthroughs.md) belirli senaryoları için takım veri bilimi işlemdeki adımlar gösteren diğer uçtan uca talimatlara açıklar. İzlenecek yollar da Bulut ve şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir.

[Spark yerleşik machine learning modellerini puan](spark-model-consumption.md) Scala kodu otomatik olarak yüklemek ve yeni veri kümeleri ile Spark oluşturulmuş ve Azure Blob depolama alanına kaydedildi machine learning modellerini puan için nasıl kullanılacağını gösterir. Var. sağlanan yönergeleri izleyin ve yalnızca bu makalede otomatik tüketimi için Scala kodla Python kodu değiştirin.

