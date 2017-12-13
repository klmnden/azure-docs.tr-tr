---
title: "Spark Mllib'i hdınsight'ta - Azure Machine learning örnekle | Microsoft Docs"
description: "Spark Mllib'i Lojistik regresyon aracılığıyla sınıflandırmasını kullanan bir veri kümesi analiz bir makine öğrenme uygulaması oluşturmak için nasıl kullanılacağını öğrenin."
keywords: "spark machine Learning, spark machine learning örneği"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: f98659081b991d403b6477196042c6ff3d40bb12
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="use-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a>Machine learning uygulama oluşturmak ve bir veri kümesi analiz etmek için Spark Mllib'i kullanın

Spark kullanmayı öğrenin **Mllib'i** açık bir veri kümesi üzerinde basit Tahmine dayalı analiz yapmak için uygulama öğrenme bir makine oluşturmak için. Bu örnek kitaplıkları öğrenme Spark'ın yerleşik makineden kullanır *sınıflandırma* Lojistik regresyon aracılığıyla. 

> [!TIP]
> Bu örnek, Hdınsight'ta oluşturma (Linux) Spark kümesinde Jupyter not defteri olarak da kullanılabilir. Not Defteri deneyimi Python parçacıkları dizüstü çalıştırmadan olanak sağlar. Gelen öğretici bir not defteri içinde takip etmek için bir Spark kümesi oluşturma ve Jupyter not defteri başlatın (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Ardından, Not Defteri çalıştırın **Spark Machine Learning - yemek İnceleme verileri MLlib.ipynb kullanarak Tahmine dayalı analiz** altında **Python** klasör.
>
>

Mllib'i makine öğrenimi görevlerini uygun yardımcı programlar da dahil olmak üzere, birçok yardımcı programını yararlı sağlayan bir çekirdek Spark kitaplığıdır:

* Sınıflandırma
* regresyon
* Kümeleme
* Konu modelleme
* Tekil değer ayrıştırma (SVD) ve asıl bileşen çözümlemesi (PCA)
* Test etme ve örnek istatistikleri hesaplama varsayımınızın

## <a name="what-are-classification-and-logistic-regression"></a>Sınıflandırma ve lojistik regresyon nelerdir?
*Sınıflandırma*, görev, öğrenme popüler bir makine kategoriye giriş verileri sıralama işlemidir. Sağladığınız veri girişi için "etiketleri" atayın nasıl bulmak için bir sınıflandırma algoritmasıdır işi var. Örneğin, stok bilgilerini giriş olarak kabul eder ve iki kategoride hisse senedi böler makine öğrenme algoritmasının düşündüğünüz: satmak stoklar ve hisse tutmanız gerekir.

Lojistik regresyon sınıflandırma için kullandığınız algoritmasıdır. Spark'ın Lojistik regresyon API için yararlıdır *ikili sınıflandırma*, veya iki gruplarının biri giriş verileri sınıflandırmaya. Lojistik gerileme hakkında daha fazla bilgi için bkz: [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Özet olarak, üreten Lojistik regresyon sürecinin bir *Lojistik işlevi* bir giriş vektörü bir grup veya diğer ait olasılık tahmin etmek için kullanılabilir.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Tahmine dayalı analiz örnek yemek İnceleme verileri
Bu örnekte, Spark yemek İnceleme veriler üzerinde bazı Tahmine dayalı analiz gerçekleştirmek için kullandığınız (**Food_Inspections1.csv**), edinilen aracılığıyla [Şehir, Chicago veri portalı](https://data.cityofchicago.org/). Bu veri kümesi her oluşturulması, (varsa) bulunan ihlalleri ve İnceleme sonuçlarını hakkında bilgiler dahil olmak üzere Chicago'da gerçekleştirildi yemek kurma incelemeleri hakkında bilgiler içerir. CSV veri dosyası zaten konumundaki küme ile ilişkilendirilmiş depolama hesabına kullanılabilir **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

Aşağıdaki adımları geçti veya kaldı yemek İnceleme için neler görmek için bir model geliştirin.

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a>Spark MMLib machine learning uygulama oluşturmaya başlayın
1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   
1. Spark kümesi dikey penceresinden **Küme Panosu**’na ve ardından **Jupyter Notebook**’a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. Bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Jupyter not defteri oluşturma](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "yeni bir Jupyter not defteri oluşturma")
1. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Not defteri adını belirtme")
1. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Uygulamanın bu senaryo için gereken türleri içeri aktararak öğrenme makinenizi oluşturmaya başlayabilirsiniz. Bunu yapmak için imleci hücre ve tuşuna koyun **SHIFT + ENTER**.

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Bir giriş dataframe oluşturun
Biz kullanabilirsiniz `sqlContext` üzerinde yapılandırılmış veri dönüşümleri gerçekleştirir. Örnek verileri yüklemek için ilk görev, ((**Food_Inspections1.csv**)) bir Spark SQL içine *dataframe*.

1. Ham verileri bir CSV biçiminde olduğundan, her satır dosyanın yapılandırılmamış metin olarak belleğe istek için Spark bağlam kullanmamız gerekiyor; Ardından, her satırın tek tek ayrıştırmak için Python'un CSV kitaplığını kullanın.

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. Şimdi CSV dosyası bir RDD sahibiz.  Veri ve şema anlamak için bir satır RDD alıyoruz.

        inspections.take(1)

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

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
1. Önceki çıkış bize giriş dosyası şeması hakkında bir fikir verir. Her kuruluş, kurma, adresini, incelemeleri başka şeylerin konumu ve veri türü adını içerir. Şimdi bizim Tahmine dayalı analiz için yararlı olan ve ardından geçici bir tablo oluşturmak için kullanırız bir dataframe Grup sonuçları birkaç sütunları seçin.

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. Şimdi sahip olduğumuz bir *dataframe*, `df` üzerinde biz gerçekleştirebilir bizim çözümleme. Ayrıca bir geçici tablo çağrı sahibiz **CountResults**. Dört sütun dataframe ilgi dahil ettiğiniz: **kimliği**, **adı**, **sonuçları**, ve **ihlalleri**.

    Şimdi küçük bir örnek veri alın:

        df.show(5)

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Veri anlama
1. Hangi veri içeren bir fikir almak başlayalım tıklatın. Örneğin, hangi farklı değerler **sonuçları** sütun?

        df.select('results').distinct().show()

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. Hızlı görselleştirme bize yardımcı olabilecek neden bu sonuçlar dağıtılması hakkında. Biz veriler geçici bir tablo zaten **CountResults**. Tabloda sonuçları nasıl dağıtıldığını daha iyi anlamak için aşağıdaki SQL sorgusunu çalıştırabilirsiniz.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    `%%sql` Sihirli arkasından `-o countResultsdf` sorgu çıktısı (genellikle küme headnode) Jupyter sunucuda yerel olarak kalıcı olmasını sağlar. Çıktı olarak kalıcı bir [Pandas](http://pandas.pydata.org/) belirtilen ada sahip dataframe **countResultsdf**.

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

    ![SQL sorgu çıktısı](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL sorgu çıktısı")

    `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
1. Matplotlib, veri görselleştirme oluşturmak için kullanılan bir kitaplık, bir çizim oluşturmak için de kullanabilirsiniz. Çizim oluşturulması gerekir çünkü yerel olarak kalıcı gelen **countResultsdf** dataframe, kod parçacığında ile başlamalıdır `%%local` Sihirli. Bu kodu Jupyter sunucu üzerinde yerel olarak çalıştırın sağlar.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

    ![Spark machine learning uygulama çıktı - beş farklı İnceleme sonuçlarını içeren pasta grafik](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning sonuç çıktısı")
1. Bir inceleme olabilir 5 ayrı sonuçları olduğunu görebilirsiniz:

   * Değil bulunan iş
   * Başarısız
   * Geçişi
   * Koşulları içeren PSS
   * İş dışı

     Bize yemek İnceleme sonucunu tahmin edebilirsiniz bir model ihlalleri verilen geliştirin. Bir ikili sınıflandırma yöntemi Lojistik regresyon olduğuna göre iki kategoride verilerimizi grubuna mantıklıdır: **başarısız** ve **geçirmek**. Bir "geçirmek içeren koşullara" hala bir geçiş olduğunu biz modeli eğitmek, biz iki sonucu eşdeğer göz önünde şekilde. Biz bizim eğitim kümesinden kaldırmak için diğer sonuçları ("İş değil bulunan" veya "İş dışı") ile veri yararlı değildir. Bu iki kategoriye sonuçları küçük bir yüzdesi yine de yapmak beri bu uygun olmalıdır.
1. Bize bir tane var olan bizim dataframe dönüştürme (`df`) burada her denetleme temsil edildiği bir etiket ihlalleri çifti olarak yeni bir dataframe içine. Bu örnekte bir etiketin `0.0` hata, bir etiketi temsil eder `1.0` başarı ve bir etiketi temsil eden `-1.0` bu iki yanı sıra bazı sonuçlarını temsil eder. Biz bu diğer sonuçlar yeni veri çerçevesi hesaplanırken filtreleme.

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    Hangi etiketli veri görülüyor görmek için şimdi bir satır alın.

        labeledData.take(1)

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Giriş dataframe Lojistik regresyon modeli oluşturma
Bizim son etiketli verileri Lojistik regresyon tarafından çözümlenebilir bir biçime dönüştürmek üzere bir görevdir. Giriş Lojistik regresyon algoritması için bir dizi olmalıdır *etiket özelliği vektör çiftleri*, "özelliği vektör" vektör giriş noktasını temsil eden sayı olduğu. Bu nedenle, biz yarı yapılandırılmış ve serbest metin, bir dizi bir makine kolayca anlayabileceği gerçek sayılar için birçok açıklamaları içeren "ihlalleri" sütun dönüştürmeniz gerekir.

"Dizin" ayrı her sözcüğün atamak ve sağlayacak şekilde her dizinin değeri metin dizesindeki sözcüğün göreli sıklığı içeren öğrenme algoritmasının makineye vektör geçirmek için doğal dil işleme için yaklaşımı öğrenme bir standart makine bulunuyor.

Mllib'i bu işlemi gerçekleştirmek için kolay bir yol sağlar. İlk olarak, "sözcükleri tek tek her dizesinde almak için her ihlalleri dize simgeleştirilecek". Ardından, bir `HashingTF` her kümesi belirteçleri, bir model oluşturmak için Lojistik regresyon algoritması aktarılabilecek bir özellik vektör dönüştürmek için. Biz bu adımların tümü "ardışık düzen" kullanılarak sırayla gerçekleştirin.

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Ayrı bir test veri kümesi üzerinde modelini değerlendir
Çok daha önce oluşturduğumuz modeli kullanırız *tahmin* yeni incelemeleri sonuçlarını ne olacağı, gözlenen ihlalleri üzerinde temel. Biz bu model dataset üzerinde eğitilmiş **Food_Inspections1.csv**. Bize ikinci bir veri kümesini kullan **Food_Inspections2.csv**, *değerlendirmek* bu modeli yeni verilere gücünü. Bu ikinci veri kümesi (**Food_Inspections2.csv**) kümesi ile ilişkili varsayılan depolama kapsayıcısı içinde olması gerekir.

1. Aşağıdaki kod parçacığında yeni dataframe oluşturur **predictionsDf** modeli tarafından oluşturulan tahmin içerir. Kod parçacığını da adlı geçici bir tablo oluşturur **tahminleri** üzerinde dataframe göre.

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. Tahminleri birini arayın. Bu kod parçacığında çalıştırın:

        predictionsDf.take(1)

   İlk giriş sınama veri kümesi için tahmini yoktur.
1. `model.transform()` Yöntemi aynı şema yeni verileri aynı dönüştürmeyi uygular ve verileri sınıflandırmak nasıl bir tahmini ulaşır. Bizim tahminleri ne kadar doğru olan bir fikir almak için bazı basit istatistikleri yapabiliriz:

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Çıktı aşağıdaki gibi görünür:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    Lojistik regresyon ile Spark kullanarak bize doğru bir model ihlalleri açıklamaları İngilizce ve belirli bir iş veya geçirmek yemek İnceleme başarısız arasındaki ilişkinin sağlar.

## <a name="create-a-visual-representation-of-the-prediction"></a>Görsel bir tahmin oluşturma
Bize yardımcı olmak için son bir görsel öğe artık bu testi sonuçlarıyla ilgili nedeni oluşturabilirsiniz.

1. Farklı Öngörüler ve sonuçları çıkartarak Başlat gelen **tahminleri** daha önce oluşturulan geçici bir tablo. Aşağıdaki sorgularda çıktısı olarak ayrı *true_positive*, *false_positive*, *true_negative*, ve *false_negative*. Sorgularda biz görselleştirme kullanarak kapatmanız `-q` ve ayrıca çıkış kaydedin (kullanarak `-o`) ile birlikte kullanılabilir dataframes olarak `%%local` Sihirli.

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. Son olarak, çizim kullanarak oluşturmak için aşağıdaki kod parçacığında kullanın **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Şu çıktı görmeniz gerekir:

    ![Uygulama çıkış - başarısız yemek incelemeleri pasta grafik yüzdelerini öğrenme Spark makine. ] (./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning sonuç çıktısı")

    Geçirilen bir incelemesi için negatif bir sonuç başvuruyor ancak bu grafikte bir "pozitif" sonuç başarısız yemek İnceleme için ifade eder.

## <a name="shut-down-the-notebook"></a>Not Defteri Kapat
Uygulamayı çalıştıran bitirdikten sonra kaynakları serbest bırakmak için Not Defteri kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bu kapanır ve not defterini kapatır.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
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
