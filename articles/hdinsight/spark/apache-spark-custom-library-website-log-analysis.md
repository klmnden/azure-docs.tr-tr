---
title: "Spark - Azure Python kitaplıkları ile Web sitesi günlüklerini çözümleme | Microsoft Docs"
description: "Bu Not Azure hdınsight'taki Spark ile özel bir kitaplık kullanılarak günlük verilerini analiz etme gösterir."
services: hdinsight
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 9f733e8b0c21b25b212266bb6047416cfb29d0ad
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a>Hdınsight'ta Spark kümesinde ile özel bir Python kitaplığı kullanarak Web sitesi günlüklerini çözümleme

Bu Not hdınsight'ta Spark ile özel bir kitaplık kullanılarak günlük verilerini analiz etme gösterir. Adlı bir Python kitaplığı kullandığımız özel kitaplığı olan **iislogparser.py**.

> [!TIP]
> Bu öğretici, Hdınsight'ta oluşturma (Linux) Spark kümesinde Jupyter not defteri olarak da kullanılabilir. Not Defteri deneyimi Python parçacıkları dizüstü çalıştırmadan olanak sağlar. Gelen öğretici bir not defteri içinde gerçekleştirmek için bir Spark kümesi oluşturma, Jupyter not defteri başlatın (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ve ardından not defteri çalıştırın **ile özel bir library.ipynb kullanarak Spark günlüklerini çözümleme** altında **PySpark**  klasörü.
>
>

**Ön koşullar:**

Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Ham veri bir RDD Kaydet
Bu bölümde, kullanırız [Jupyter](https://jupyter.org) ham örnek verileri işlemek ve Hive tablo olarak Kaydet işlerini çalıştırmak için hdınsight'ta bir Apache Spark kümesi ile ilişkili dizüstü bilgisayar. Örnek, bir .csv dosyası (hvac.csv) kullanılabilir varsayılan olarak tüm kümelerde verilerdir.

Verilerinizi bir Hive tablosu olarak kaydedildikten sonra sonraki bölümde biz Power BI ve Tableau gibi BI araçları kullanarak Hive tablosu bağlanır.

1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   
2. Spark kümesi dikey penceresinden **Küme Panosu**’na ve ardından **Jupyter Notebook**’a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Yeni bir Jupyter not defteri oluşturma")
4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Not defteri adını belirtme")
5. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Boş bir hücreye aşağıdaki kod parçacığını yapıştırın ve sonra basın **SHIFT + ENTER**.

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. Kümede zaten mevcut örnek günlük verileri kullanarak bir RDD oluşturun. Konumundaki küme ile ilişkili varsayılan depolama hesabındaki verilere erişebilir **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. Örnek günlük doğrulamak için kümesinin başarıyla tamamlandı ve önceki adımla alın.

        logs.take(5)

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Özel bir Python kitaplığı kullanarak günlük verileri analiz
1. Yukarıdaki çıktıda üst bilgileri ilk birkaç satırı içerir ve bu başlığında açıklanan şema kalan her satırın eşleştirir. Bu tür günlükleri ayrıştırma karmaşık olabilir. Bu nedenle, özel bir Python kitaplığı kullandığımız (**iislogparser.py**) bu tür günlükleri çok daha kolay ayrıştırma yapar. Varsayılan olarak, bu kitaplık hdınsight'ta Spark kümenizin birlikte **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Ancak, bu kitaplık olmayan `PYTHONPATH` Biz bu gibi bir içeri aktarma deyimini kullanarak kullanamazlar `import iislogparser`. Bu kitaplığı kullanmak için biz için tüm çalışan düğümleri dağıtmanız gerekir. Aşağıdaki kod parçacığında çalıştırın.

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. `iislogparser`bir işlev sağlar `parse_log_line` döndüren `None` günlük satır bir başlık satırıdır ve örneğini döndürür, `LogLine` günlük satırı karşılaşırsa sınıfı. Kullanım `LogLine` sınıfı yalnızca günlük satırları RDD ayıklamak için:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. Birkaç doğrulamak için ayıklanan günlük satırları başarıyla tamamlandı adım alır.

       logLines.take(2)

   Çıktının aşağıdakine benzer olması gerekir:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. `LogLine` Sınıfı, sırasıyla sahip bazı kullanışlı yöntemler gibi `is_error()`, bir günlük girişi bir hata kodu sahip olup olmadığını döndürür. Ayıklanan günlük satırları hataların sayısı işlem için bunu kullanın ve farklı bir dosya için tüm hataların oturum açın.

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   Aşağıdaki gibi bir çıktı görmeniz gerekir:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. Aynı zamanda **Matplotlib** bir veri görselleştirmesi oluşturmak için. Örneğin, uzun süredir çalışan istekleri nedenini ayırt etmek istiyorsanız, ortalama hizmet en zaman dosyaları bulmak isteyebilirsiniz.
   Aşağıdaki kod parçacığını bir isteğe hizmet vermek için çoğu sürdü üst 25 kaynakları alır.

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   Aşağıdaki gibi bir çıktı görmeniz gerekir:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. Ayrıca bu bilgileri çizim biçiminde sunabilir. Bir çizim oluşturmak için ilk adım, bize ilk geçici bir tablo oluşturun **AverageTime**. Tablo günlükleri belirli bir zamanda herhangi bir olağan dışı gecikme ani olup olmadığını görmek için zamana göre gruplandırır.

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. Ardından tüm kayıtları almak için aşağıdaki SQL sorgusunu çalıştırın **AverageTime** tablo.

       %%sql -o averagetime
       SELECT * FROM AverageTime

   `%%sql` Sihirli arkasından `-o averagetime` sorgu çıktısı (genellikle küme headnode) Jupyter sunucuda yerel olarak kalıcı olmasını sağlar. Çıktı olarak kalıcı bir [Pandas](http://pandas.pydata.org/) belirtilen ada sahip dataframe **averagetime**.

   Aşağıdaki gibi bir çıktı görmeniz gerekir:

   ![SQL sorgu çıktısı](./media/apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL sorgu çıktısı")

   Hakkında daha fazla bilgi için `%%sql` Sihirli, bkz: [parametreleri desteklenen ile %% sql Sihirli](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
7. Artık Matplotlib, veri görselleştirme oluşturmak için kullanılan bir kitaplık bir çizim oluşturmak için de kullanabilirsiniz. Çizim oluşturulması gerekir çünkü yerel olarak kalıcı gelen **averagetime** dataframe, kod parçacığında ile başlamalıdır `%%local` Sihirli. Bu kodu Jupyter sunucu üzerinde yerel olarak çalıştırın sağlar.

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   Aşağıdaki gibi bir çıktı görmeniz gerekir:

   ![Matplotlib çıkış](./media/apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib çıkış")
8. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](../hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
