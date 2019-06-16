---
title: Python kitaplıkları Spark - Azure ile Web sitesi günlüklerini çözümleme
description: Bu Not, Azure HDInsight üzerinde Spark ile özel bir kitaplık kullanarak günlük verilerini nasıl çözümleyeceğinizi gösterir.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: bef71f210e015dc10cd6f5c0c655d0d3beee3655
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64728927"
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-apache-spark-cluster-on-hdinsight"></a>HDInsight üzerinde Apache Spark kümesi ile özel bir Python kitaplığı kullanarak Web sitesi günlüklerini çözümleme

Bu not defteri, HDInsight üzerinde Apache Spark ile özel bir kitaplık kullanarak günlük verilerini nasıl çözümleyeceğinizi gösterir. Adlı bir Python kitaplığı kullandığımız özel bir kitaplık olan **iislogparser.py**.

> [!TIP]  
> Bu öğreticide, HDInsight içinde oluşturduğunuz bir Spark (Linux) kümesinde Jupyter not defteri olarak da kullanılabilir. Not Defteri deneyimi not defterinden Python kod çalıştırmanıza olanak tanır. Öğreticinin bir not defteri içinde gerçekleştirmek için Spark kümesi oluşturma, Jupyter not defteri başlatın (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ve sonra not defteri çalıştırma **özel library.ipynb kullanarak Spark ile günlükleri çözümleme** altında **PySpark**  klasör.
>
>

**Ön koşullar:**

Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Ham veri bir RDD Kaydet
Bu bölümde, kullandığımız [Jupyter](https://jupyter.org) not defteri ham örnek verilerinizi işleyen ve Hive tablo olarak Kaydet işlerini çalıştırmak için HDInsight, Apache Spark kümesi ile ilişkili. Örnek bir .csv dosyası (hvac.csv) varsayılan olarak tüm kümelerde verilerdir.

Verilerinizi bir Apache Hive tablosu olarak kaydedildikten sonra sonraki bölümde biz Power BI ve Tableau gibi BI araçları kullanarak Hive tablosu bağlanır.

1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)**  > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   
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
5. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gerekli olan türleri içeri aktararak başlayabilirsiniz. Aşağıdaki kod parçacığını boş bir hücreye yapıştırın ve sonra **SHIFT + ENTER** tuşlarına basın.

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. Kümede zaten kullanılabilir örnek günlük verileri kullanarak bir RDD oluşturun. Konumundaki kümeye ilişkilendirilmiş varsayılan depolama hesabındaki verilere erişebilir **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. Doğrulamak için ayarlanmış bir örnek günlük başarıyla tamamlandı. önceki adım alın.

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

## <a name="analyze-log-data-using-a-custom-python-library"></a>Özel bir Python kitaplığı kullanarak günlük verilerini çözümleme
1. Yukarıdaki çıktıda kalan her satırı, üst bilgisinde açıklanan şemayla eşleştiğinden ve ilk birkaç satırı üst bilgi bilgileri içerir. Bu tür günlükleri ayrıştırma karmaşık olabilir. Bu nedenle, özel bir Python kitaplığı kullandığımız (**iislogparser.py**) bu tür günlükleri çok daha kolay ayrıştırma yapar. Varsayılan olarak, bu kitaplık HDInsight Spark kümenizde bulunan **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Ancak, bu kitaplık içinde değil `PYTHONPATH` Biz bu gibi bir içeri aktarma deyimi kullanılarak kullanamazlar `import iislogparser`. Bu kitaplığı kullanmak için size tüm çalışan düğümleri dağıtmanız gerekir. Aşağıdaki kod parçacığını çalıştırın.

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. `iislogparser` bir işlev sağlar `parse_log_line` döndüren `None` günlük satır bir başlık satırıdır ve örneğini döndürür `LogLine` günlük satır karşılaşırsa, sınıf. Kullanım `LogLine` sınıfı yalnızca günlük satırları RDD ayıklamak için:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. Ayıklanan günlük satırları doğrulamak için birkaç adım başarıyla tamamlandı alın.

       logLines.take(2)

   Çıktının aşağıdakine benzer olması gerekir:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. `LogLine` Sınıfı sırayla, olan bazı kullanışlı bir yöntem gibi `is_error()`, bir günlük girişi bir hata koduna sahip olup olmadığını döndürür. İşlem hatası ayıklanan günlük satırları için bunu kullanın ve ardından farklı bir dosyaya tüm hataları oturum.

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
4. Ayrıca **Matplotlib** verilerin bir görselleştirme oluşturmak için. Örneğin, uzun süredir çalışan istekleri nedenini yalıtmak isterseniz, ortalama sunmak için en çok zaman alan dosyaları bulmak isteyebilirsiniz.
   Aşağıdaki kod parçacığında, bir isteğe hizmet vermek için en çok zaman harcanan en çok 25 kaynakları alır.

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
5. Bu bilgiler çizim biçiminde de sunabilir. Bir çizim oluşturmak için ilk adım, bize öncelikle geçici tablo oluşturmak **AverageTime**. Tablo günlükleri belirli bir zamanda herhangi bir olağan dışı gecikme ani yükselme olup olmadığını görmek için zamana göre gruplandırır.

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. Ardından tüm kayıtları almak için aşağıdaki SQL sorgusunu çalıştırın **AverageTime** tablo.

       %%sql -o averagetime
       SELECT * FROM AverageTime

   `%%sql` Sihirli arkasından `-o averagetime` sorgunun çıkışı (genellikle bir kümenin baş) Jupyter sunucu üzerinde yerel olarak kalıcı olmasını sağlar. Çıktı olarak kalıcı bir [Pandas](https://pandas.pydata.org/) belirtilen ada sahip bir dataframe **averagetime**.

   Aşağıdaki gibi bir çıktı görmeniz gerekir:

   ![SQL sorgu çıktısı](./media/apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL sorgu çıktısı")

   Hakkında daha fazla bilgi için `%%sql` Sihirli, bkz: [parametreleri desteklenen ile %% sql Sihri](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
7. Matplotlib, veri görselleştirme oluşturmak için kullanılan bir kitaplık artık bir çizim oluşturmak için de kullanabilirsiniz. Çizim oluşturulması gerektiğinden yerel olarak kalıcı gelen **averagetime** dataframe, kod parçacığı ile başlamalıdır `%%local` Sihirli. Bu kod Jupyter sunucusunda yerel olarak çalıştırmanızı sağlar.

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
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Oluşturmak ve Apache Spark Scala uygulamaları göndermek amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
