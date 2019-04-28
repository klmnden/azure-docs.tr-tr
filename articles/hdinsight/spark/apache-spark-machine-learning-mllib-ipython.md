---
title: HDInsight - Azure üzerinde Spark MLlib ile Machine learning örneği
description: Spark MLlib Lojistik regresyon aracılığıyla sınıflandırmasını kullanan bir veri kümesini analiz ederek bir makine öğrenimi uygulaması oluşturmak için kullanmayı öğrenin.
keywords: spark machine Learning, spark machine learning örneği
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: hrasheed
ms.openlocfilehash: 31755dcc247ea3be5fb38249afd98dc72dcbc544
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097108"
---
# <a name="use-apache-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a>Bir machine learning uygulama oluşturmak ve bir veri kümesini analiz etmek için Apache Spark MLlib kullanın

Apache Spark'ı kullanmayı öğrenin [MLlib](https://spark.apache.org/mllib/) bir machine learning açık bir veri kümesini basit Tahmine dayalı analiz yapmak için uygulama oluşturmak için. Kitaplıkları Spark'ın yerleşik makine öğrenimi, bu örnekte *sınıflandırma* Lojistik regresyon aracılığıyla. 

MLlib için uygun yardımcı programlar da dahil olmak üzere, makine öğrenimi görevlerini yararlı birçok yardımcı programlar sağlayan bir çekirdek Spark kitaplığıdır:

* Sınıflandırma
* Regresyon
* Kümeleme
* Konu modelleme
* Tekil değer ayrıştırma (SVD) ve asıl bileşende analiz (PCA)
* Test etme ve örnek istatistikleri hesaplama varsayım

## <a name="understand-classification-and-logistic-regression"></a>Sınıflandırma ve lojistik regresyon anlama
*Sınıflandırma*, popüler makine öğrenme görev, girdi verilerini kategorilere sıralama işlemidir. Bunu nasıl sağlayan veri girişi için "labels" atamak için bir sınıflandırma algoritmasıdır işidir. Örneğin, stok bilgilerini giriş olarak kabul eder ve iki kategoride stok böler bir makine öğrenimi algoritmasının kadar aklınıza: Satış satımında ve paylaşımında tutmanız gerekir stocks.

Lojistik regresyon kullandığınız için sınıflandırma algoritmasıdır. Spark'ın Lojistik regresyon API için kullanışlıdır *ikili sınıflandırma*, ya da iki gruba birini giriş veri sınıflandırma. Lojistik gerilemeleri hakkında daha fazla bilgi için bkz: [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Özet olarak, lojistik regresyon işlemi üreten bir *Lojistik işlevi* Giriş bir vektör bir grup veya diğer ait olasılığını tahmin etmek için kullanılabilir.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Gıda denetimi veri üzerinde Tahmine dayalı analiz örneği
Bu örnekte, Yemek İnceleme verilerinizde bazı Tahmine dayalı analiz gerçekleştirmek için Spark kullanın (**Food_Inspections1.csv**) aracılığıyla alındı [Şehir, Chicago veri portalı](https://data.cityofchicago.org/). Bu veri kümesi her kuruluş, (varsa) bulunan ihlalleri ve denetleme sonuçlarını hakkında bilgiler dahil olmak üzere Chicago'daki gerçekleştirildi Gıda kurma incelemeleri hakkındaki bilgileri içerir. CSV verileri dosyası zaten kümeyle ilişkili depolama hesabı kullanılabilir **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

Aşağıdaki adımlarda, Geçti veya başarısız bir yemek İnceleme kadar sürdüğünü görmek için bir model geliştirmektir.

## <a name="create-an-apache-spark-mllib-machine-learning-app"></a>Bir Apache Spark MLlib makine öğrenimi uygulaması oluşturma

1. PySpark çekirdeği kullanarak bir Jupyter not defteri oluşturun. Yönergeler için bkz. [Jupyter not defteri oluşturma](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

2. Bu uygulama için gerekli olan türleri içeri aktarın. Kopyalayın ve aşağıdaki kodu boş bir hücreye yapıştırın ve sonra basın **SHIFT + ENTER**.

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    ```
    PySpark çekirdeği nedeniyle açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. 

## <a name="construct-the-input-dataframe"></a>Giriş veri çerçevesi oluşturun

Bir CSV biçiminde ham veriler olduğu için dosyayı belleğe yapılandırılmamış metin olarak çekmek için Spark bağlamını kullanır ve ardından her veri satırı ayrıştırmak için Python'un CSV kitaplığını kullanın.

1. Bir dayanıklı Dağıtılmış veri kümesi (RDD) girdi verilerini ayrıştırma ile oluşturmak için aşağıdaki satırları çalıştırın.

    ```PySpark
    def csvParse(s):
        import csv
        from StringIO import StringIO
        sio = StringIO(s)
        value = csv.reader(sio).next()
        sio.close()
        return value
    
    inspections = sc.textFile('/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                    .map(csvParse)
    ```

2. Böylece, bir veri şeması görünümünü bir satır RDD almak için aşağıdaki kodu çalıştırın:

    ```PySpark
    inspections.take(1)
    ```

    Çıktı.

    ```
    [['413707',
        'LUNA PARK INC',
        'LUNA PARK  DAY CARE',
        '2049789',
        "Children's Services Facility",
        'Risk 1 (High)',
        '3250 W FOSTER AVE ',
        'CHICAGO',
        'IL',
        '60625',
        '09/21/2010',
        'License-Task Force',
        'Fail',
        '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
        '41.97583445690982',
        '-87.7107455232781',
        '(41.97583445690982, -87.7107455232781)']]
    ```

    Çıktı, girdi dosyasının şeması hakkında bir fikir sunar. Bu, her kuruluş, kuruluş, adres, incelemeleri ve konum, başka şeylerin yanında, veri türü adını içerir. 

3. Bir dataframe oluşturmak için aşağıdaki kodu çalıştırın (*df*) ve geçici bir tabloya (*CountResults*) ile Tahmine dayalı analiz için kullanışlı olan birkaç sütunu. `sqlContext` yapılandırılmış veriler üzerinde dönüştürmeler gerçekleştirmek için kullanılır. 

    ```PySpark
    schema = StructType([
    StructField("id", IntegerType(), False),
    StructField("name", StringType(), False),
    StructField("results", StringType(), False),
    StructField("violations", StringType(), True)])
    
    df = spark.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
    df.registerTempTable('CountResults')
    ```

    Dört sütunların veri çerçevesi ilgilendiğiniz **kimliği**, **adı**, **sonuçları**, ve **ihlalleri**.

4. Küçük bir örnek veri almak için aşağıdaki kodu çalıştırın:

    ```PySpark
    df.show(5)
    ```

    Çıktı.

    ```
    +------+--------------------+-------+--------------------+
    |    id|                name|results|          violations|
    +------+--------------------+-------+--------------------+
    |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
    |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
    |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
    |413708|BENCHMARK HOSPITA...|   Pass|                    |
    |413722|           JJ BURGER|   Pass|                    |
    +------+--------------------+-------+--------------------+
    ```

## <a name="understand-the-data"></a>Verileri anlama

Hangi veri kümesini içeren bir fikir almak şimdi başlayın. 

1. Benzersiz değerleri göstermek için aşağıdaki kodu çalıştırın **sonuçları** sütun:

    ```PySpark
    df.select('results').distinct().show()
    ```

    Çıktı.

    ```
    +--------------------+
    |             results|
    +--------------------+
    |                Fail|
    |Business Not Located|
    |                Pass|
    |  Pass w/ Conditions|
    |     Out of Business|
    +--------------------+
    ```

2. Bu sonuçların dağılımını görselleştirmek için aşağıdaki kodu çalıştırın:

    ```PySpark
    %%sql -o countResultsdf
    SELECT COUNT(results) AS cnt, results FROM CountResults GROUP BY results
    ```

    `%%sql` Sihirli arkasından `-o countResultsdf` sorgunun çıkışı (genellikle bir kümenin baş) Jupyter sunucu üzerinde yerel olarak kalıcı olmasını sağlar. Çıktı olarak kalıcı bir [Pandas](https://pandas.pydata.org/) belirtilen ada sahip bir dataframe **countResultsdf**. Hakkında daha fazla bilgi için `%%sql` sihrinin yanı sıra, PySpark çekirdeği kullanılabilen diğer sihirler bkz [Apache Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

    Çıktı.

    ![SQL sorgu çıktısı](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL sorgu çıktısı")


3. Ayrıca [Matplotlib](https://en.wikipedia.org/wiki/Matplotlib), bir çizim oluşturmak için veri görselleştirme oluşturmak için kullanılan bir kitaplık. Çizim oluşturulması gerektiğinden yerel olarak kalıcı gelen **countResultsdf** dataframe, kod parçacığı ile başlamalıdır `%%local` Sihirli. Bu kod Jupyter sunucusunda yerel olarak çalıştırmanızı sağlar.

    ```PySpark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    labels = countResultsdf['results']
    sizes = countResultsdf['cnt']
    colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
    plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
    plt.axis('equal')
    ```

    Çıktı.

    ![Spark machine learning uygulama çıkışı - beş farklı denetimi sonuçlarını pasta grafiği](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning sonuç çıkış")

    Yemek İnceleme sonuçlarını tahmin etmek için üzerinde ihlalleri dayalı bir model geliştirmektir gerekir. Lojistik regresyon ikili sınıflandırma yöntemi olduğundan, sonuçta elde edilen veri iki kategoriler altında gruplandırmak için mantıklıdır: **Başarısız** ve **geçirmek**:

   - Geçiş
       - Geçiş
       - Koşullar ile geçirin
   - Başarısız
       - Başarısız
   - At
       - İş bulunan değil
       - İş dışında

     Diğer sonuçları ("İş değil bulunan" veya "İş dışı") ile veri kullanışlı değildir ve küçük bir bölümü sonuçları yine de olun.

4. Mevcut veri çerçevesi dönüştürmek için aşağıdaki kodu çalıştırın (`df`) içine her inceleme etiket ihlalleri çift olarak temsil burada yeni bir veri çerçevesi. Bu durumda, bir etiketi içinde `0.0` hata, bir etiketi temsil eder `1.0` başarı ve bir etiketi temsil eden `-1.0` bu iki yanı sıra bazı sonuçlarını temsil eder. 

    ```PySpark
    def labelForResults(s):
        if s == 'Fail':
            return 0.0
        elif s == 'Pass w/ Conditions' or s == 'Pass':
            return 1.0
        else:
            return -1.0
    label = UserDefinedFunction(labelForResults, DoubleType())
    labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')
    ```

5. Bir satır etiketli veri göstermek için aşağıdaki kodu çalıştırın:

    ```PySpark
    labeledData.take(1)
    ```

    Çıktı.

    ```
    [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]
    ```

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Giriş veri çerçevesi bir Lojistik regresyon modeli oluşturun

Etiketli veri Lojistik regresyon tarafından çözümlenebilir bir biçime dönüştürmek için son görevdir bakın. Giriş bir Lojistik regresyon algoritmasına bir dizi olması gereken *etiketi özelliği vektör çiftleri*giriş noktasını temsil eden bir sayıdan oluşan bir vektörü "özellik vektör" olduğu. Bu nedenle, yarı yapılandırılmış ve serbest metin, makine bir kolayca anlayabileceği bir gerçek sayı dizisi için birçok açıklamalar içeren "ihlalleri" sütununa dönüştürülecek gerekir.

Bir standart machine learning doğal dil işleme için bir yaklaşım, "Index" her benzersiz sözcüğü atayın ve ardından bir vektör makine her dizinin değeri içeren metin dizesi, bir sözcüğün sıklıkta olduğunu öğrenme algoritmasına geçirmek sağlamaktır.

MLlib bu işlemi gerçekleştirmek için kolay bir yol sağlar. İlk olarak, "kelimeler her bir dizenin almak için her ihlalleri dize simgeleştirin". Ardından, bir `HashingTF` belirteçleri her kümesi, bir model oluşturmak için Lojistik regresyon algoritması geçirilebilen özellik vektör dönüştürmek için. "İşlem hattı" kullanılarak sırayla tüm adımları gerçekleştirin.

```PySpark
tokenizer = Tokenizer(inputCol="violations", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(labeledData)
```

## <a name="evaluate-the-model-using-another-dataset"></a>Başka bir veri kümesini kullanarak modeli değerlendirin

Daha önce oluşturduğunuz modeli kullandığınız *tahmin* ne yeni incelemeleri sonuçlarını olacaktır, gözlemlenmedi ihlaller üzerinde temel. Bu model bir veri kümesini eğitilmiş **Food_Inspections1.csv**. İkinci bir veri kümesi kullanabileceğiniz **Food_Inspections2.csv**, *değerlendirmek* bu modeli yeni verilere gücü. Bu ikinci bir veri kümesi (**Food_Inspections2.csv**) kümeyle ilişkilendirilmiş varsayılan depolama kapsayıcısını bulunmaktadır.

1. Yeni bir dataframe oluşturmak için aşağıdaki kodu çalıştırın **predictionsDf** modeli tarafından oluşturulan öngörü içerir. Kod parçacığı da adlı geçici bir tablo oluşturur **Öngörüler** dataframe üzerinde temel.

    ```PySpark
    testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                .map(csvParse) \
                .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
    testDf = spark.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
    predictionsDf = model.transform(testDf)
    predictionsDf.registerTempTable('Predictions')
    predictionsDf.columns
    ```

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

    ```
    ['id',
        'name',
        'results',
        'violations',
        'words',
        'features',
        'rawPrediction',
        'probability',
        'prediction']
    ```

1. Öngörüler birini arayın. Bu kod parçacığını çalıştırın:

    ```PySpark
    predictionsDf.take(1)
    ```

   Tahmin için ilk giriş sınama veri kümesi yok.
1. `model.transform()` Yöntemi yeni veriler aynı şemaya sahip aynı dönüştürmeyi uygular ve verileri sınıflandırmak nasıl bir tahmini ulaşır. Ne kadar doğru tahmin edilen bir fikir almak için bazı basit istatistikleri yapabilirsiniz:

    ```PySpark
    numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                            (prediction = 1 AND (results = 'Pass' OR
                                                                results = 'Pass w/ Conditions'))""").count()
    numInspections = predictionsDf.count()

    print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
    print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"
    ```

    Çıktı aşağıdaki gibi görünür:

    ```
    There were 9315 inspections and there were 8087 successful predictions
    This is a 86.8169618894% success rate
    ```

    Spark ile Lojistik regresyon kullanarak İngilizce ihlalleri açıklamaları ve belirli bir iş veya geçirmek Gıda denetimi başarısız arasındaki ilişkinin doğru bir model sunar.

## <a name="create-a-visual-representation-of-the-prediction"></a>Tahmin görsel bir temsilini oluşturma
Artık yardımcı olması için son bir görselleştirme oluşturabilirsiniz bu testin sonuçları hakkında neden.

1. Farklı tahminler ve sonuçları ayıklayarak başlayın gelen **Öngörüler** daha önce oluşturulan geçici bir tablo. Çıktı olarak aşağıdaki sorguları ayrı *true_positive*, *false_positive*, *true_negative*, ve *false_negative*. Aşağıdaki sorgularda, görselleştirme kullanarak etkinleştirmeniz `-q` ve ayrıca çıkış kaydedin (kullanarak `-o`) ile kullanılabilecek veri çerçevelerini olarak `%%local` magic.

    ```PySpark
    %%sql -q -o true_positive
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'
    ```

    ```PySpark
    %%sql -q -o false_positive
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
    ```

    ```PySpark
    %%sql -q -o true_negative
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'
    ```

    ```PySpark
    %%sql -q -o false_negative
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
    ```

1. Son olarak, çizim kullanarak oluşturmak için aşağıdaki kod parçacığını kullanın **Matplotlib**.

    ```PySpark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    labels = ['True positive', 'False positive', 'True negative', 'False negative']
    sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
    colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
    plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
    plt.axis('equal')
    ```

    Aşağıdaki çıktıyı görmeniz gerekir:

    ![Spark makine öğrenme uygulama çıkışı - başarısız Gıda incelemeleri pasta grafiği yüzdesi. ](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning sonuç çıkış")

    Geçirilen bir inceleme için negatif bir sonuç başvuruyor ancak bu grafikte, sonuç "sıfırdan" başarısız yemek İnceleme için ifade eder.

## <a name="shut-down-the-notebook"></a>Not defterini Kapat
Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için Not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’u seçin. Bu işlem kapanır ve not defterini kapatır.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
