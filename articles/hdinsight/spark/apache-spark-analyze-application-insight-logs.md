---
title: -Azure HDInsight Spark ile Application Insight günlüklerini çözümleme
description: Blob depolama ve HDInsight üzerinde Spark ile ardından günlükleri analiz etmek için Application Insight günlükleri dışarı aktarma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/09/2018
ms.openlocfilehash: 730ecd306bf33709ed5d9fa334b64f7cd7a482dc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066480"
---
# <a name="analyze-application-insights-telemetry-logs-with-apache-spark-on-hdinsight"></a>HDInsight üzerinde Apache Spark ile Application Insights telemetri günlüklerini çözümleme

Nasıl kullanacağınızı öğrenin [Apache Spark](https://spark.apache.org/) Application Insight telemetri verilerini çözümlemek için HDInsight üzerinde.

[Visual Studio Application Insights](../../azure-monitor/app/app-insights-overview.md) web uygulamalarınızı izleyen bir analiz hizmetidir. Application Insights tarafından üretilen telemetri verileri Azure depolama alanına aktarılabilir. Azure Depolama'da veri hale geldikten sonra HDInsight incelemek üzere kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

* Application Insights'ı kullanacak şekilde yapılandırılmış bir uygulama.

* Linux tabanlı HDInsight kümesi oluşturma ile aşinalık. Daha fazla bilgi için [HDInsight üzerinde Apache Spark'ı oluşturma](apache-spark-jupyter-spark-sql.md).

* Bir web tarayıcısı.

Aşağıdaki kaynaklar, geliştirme ve bu belgeye sınama kullanılıyordu:

* Application Insights telemetri verilerini kullanarak oluşturulan bir [Node.js web uygulamasına Application Insights'ı kullanacak şekilde yapılandırılmış](../../azure-monitor/app/nodejs.md).

* HDInsight kümesi sürüm 3.5 üzerinde Linux tabanlı bir Spark, verileri çözümlemek için kullanıldı.

## <a name="architecture-and-planning"></a>Mimari ve planlama

Aşağıdaki diyagramda, bu örnekte service mimarisi gösterilmektedir:

![HDInsight üzerinde Spark tarafından işlenmekte olan sonra veri blob depolama alanına Application Insights'tan akışını gösteren diyagram](./media/apache-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure Storage

Application Insights telemetri bilgilerini bloblar için sürekli olarak dışarı aktarmak için yapılandırılabilir. HDInsight, ardından blob'larda depolanan verileri okuyabilir. Ancak, izlemeniz gereken bazı gereksinimler vardır:

* **Konum**: Depolama hesabı ve HDInsight farklı konumlarda ise, gecikme süresini artırabilir. Olarak çıkış ücretleri bölgeleri arasında taşıma verilere uygulanır maliyet de artar.

    > [!WARNING]  
    > HDInsight farklı bir konumda bir depolama hesabının kullanılması desteklenmez.

* **BLOB türü**: HDInsight yalnızca blok blob'larını destekler. Uygulama öngörüleri varsayılanları kullanarak blok blobları, böylece iş HDInsight ile varsayılan.

Mevcut bir kümeye depolama ekleme hakkında daha fazla bilgi için bkz: [başka depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md) belge.

### <a name="data-schema"></a>Veri şeması

Application ınsights [veri modelini dışarı aktarma](../../azure-monitor/app/export-data-model.md) telemetri veri biçimi için bilgileri dışarı BLOB'ları için. Bu belgedeki adımlarda verilerle çalışmak için Spark SQL kullanın. Spark SQL, Application Insights tarafından günlüğe kaydedilen JSON veri yapısı için bir şema otomatik olarak oluşturabilirsiniz.

## <a name="export-telemetry-data"></a>Telemetri verileri dışarı aktarma

Bağlantısındaki [sürekli dışarı aktarma yapılandırma](../../azure-monitor/app/export-telemetry.md) telemetri bilgilerini dışarı aktarmak için bir Azure depolama blobu için Application Insights'ınızı yapılandırmak için.

## <a name="configure-hdinsight-to-access-the-data"></a>Verilere erişmek için HDInsight'ı yapılandırma

Bir HDInsight kümesi oluşturuyorsanız, küme oluşturma sırasında depolama hesabı ekleyin.

Azure depolama hesabı mevcut bir kümeye eklemek için yer alan bilgileri kullanın. [ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md) belge.

## <a name="analyze-the-data-pyspark"></a>Verileri analiz edin: PySpark

1. Gelen [Azure portalında](https://portal.azure.com), HDInsight kümesi üzerinde Spark'ı seçin. Gelen **hızlı bağlantılar** bölümünden **küme panoları**ve ardından **Jupyter not defteri** küme Dashboard__ bölümünden.

    ![Küme panoları](./media/apache-spark-analyze-application-insight-logs/clusterdashboards.png)

2. Jupyter sayfanın sağ üst köşesinde seçin **yeni**, ardından **PySpark**. Python tabanlı Jupyter not defteri içeren yeni bir tarayıcı sekmesi açılır.

3. İlk alanına (adlı bir **hücre**) sayfasında, aşağıdaki metni girin:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Bu kod, giriş verileri için dizin yapısını yinelemeli olarak erişim için Spark yapılandırır. Application Insights telemetrisi benzer bir dizin yapısına günlüğe `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Kullanım **SHIFT + ENTER** kodu çalıştırmak için. Hücresinin sol tarafındaki bir '\*' Bu hücreyi kodda yürütülmekte olan göstermek için köşeli ayraçlar arasında görünür. İşlem tamamlandıktan sonra '\*' bir sayı ve çıktı aşağıdaki metne benzer değişiklikler, hücrede görüntülenir:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Yeni bir hücre, ilki oluşturulur. Yeni hücreye aşağıdaki metni girin. Değiştirin `CONTAINER` ve `STORAGEACCOUNT` Azure depolama hesabı adı ve Application Insights verileri içeren blob kapsayıcı adı.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Kullanım **SHIFT + ENTER** Bu hücreyi yürütülecek. Aşağıdaki metne benzer bir sonuç görürsünüz:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Döndürülen wasb yolu Application Insights telemetri verilerinin konumudur. Değişiklik `hdfs dfs -ls` satır hücresine döndürülen wasb yolu kullanın ve ardından **SHIFT + ENTER** hücre yeniden çalıştırılacak. Bu kez, telemetri verilerini içeren dizinleri sonuçları görüntülemesi gerekir.

   > [!NOTE]  
   > Bu bölümdeki adımları geri kalanında `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` dizini kullanıldı. Directory yapınızın farklı olabilir.

6. Sonraki hücreye aşağıdaki kodu girin: Değiştirin `WASB_PATH` önceki adımdan yola sahip.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Bu kod bir dataframe sürekli dışarı aktarma işlemi tarafından dışarı aktarılan JSON dosyaları oluşturur. Kullanım **SHIFT + ENTER** bu hücresini çalıştırmak için.
7. Sonraki hücrenin girin ve Spark'ı için JSON dosyalarını oluşturduğunuz şemasını görüntülemek için aşağıdaki komutu çalıştırın:

   ```python
   jsonData.printSchema()
   ```

    Telemetrinin her türü için şema farklıdır. Aşağıdaki örnek, web istekleri için oluşturulan şema (depolanan verileri `Requests` alt):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)
8. Geçici tablo olarak veri çerçevesi kaydetmek ve veriler karşı sorgu çalıştırmak için aşağıdakileri kullanın:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Bu sorgu, burada context.location.city null değil en çok 20 kayıtlar için Şehir bilgilerini döndürür.

   > [!NOTE]  
   > Application Insights tarafından günlüğe kaydedilen tüm telemetri mevcut bağlam yapısıdır. Şehir öğesi günlükleri doldurulmuş değil. Günlükleriniz için veri içerebilir sorgulayabilirsiniz diğer öğeleri tanımlamak için şemayı kullanın.

    Bu sorgu, aşağıdaki metne benzer bilgileri döndürür:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-scala"></a>Verileri analiz edin: Scala

1. Gelen [Azure portalında](https://portal.azure.com), HDInsight kümesi üzerinde Spark'ı seçin. Gelen **hızlı bağlantılar** bölümünden **küme panoları**ve ardından **Jupyter not defteri** küme Dashboard__ bölümünden.

    ![Küme panoları](./media/apache-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Jupyter sayfanın sağ üst köşesinde seçin **yeni**, ardından **Scala**. Scala tabanlı Jupyter not defteri içeren yeni bir tarayıcı sekmesinde görünür.
3. İlk alanına (adlı bir **hücre**) sayfasında, aşağıdaki metni girin:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Bu kod, giriş verileri için dizin yapısını yinelemeli olarak erişim için Spark yapılandırır. Application Insights telemetrisi benzer bir dizin yapısına günlüğe `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Kullanım **SHIFT + ENTER** kodu çalıştırmak için. Hücresinin sol tarafındaki bir '\*' Bu hücreyi kodda yürütülmekte olan göstermek için köşeli ayraçlar arasında görünür. İşlem tamamlandıktan sonra '\*' bir sayı ve çıktı aşağıdaki metne benzer değişiklikler, hücrede görüntülenir:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Yeni bir hücre, ilki oluşturulur. Yeni hücreye aşağıdaki metni girin. Değiştirin `CONTAINER` ve `STORAGEACCOUNT` Application Insights'ı içeren blob kapsayıcı adı ve Azure depolama hesabı adı ile günlüğe kaydeder.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Kullanım **SHIFT + ENTER** Bu hücreyi yürütülecek. Aşağıdaki metne benzer bir sonuç görürsünüz:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Döndürülen wasb yolu Application Insights telemetri verilerinin konumudur. Değişiklik `hdfs dfs -ls` satır hücresine döndürülen wasb yolu kullanın ve ardından **SHIFT + ENTER** hücre yeniden çalıştırılacak. Bu kez, telemetri verilerini içeren dizinleri sonuçları görüntülemesi gerekir.

   > [!NOTE]  
   > Bu bölümdeki adımları geri kalanında `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` dizini kullanıldı. Telemetri verileriniz için bir web uygulaması olmadığı sürece bu dizin mevcut olmayabilir.

6. Sonraki hücreye aşağıdaki kodu girin: Değiştirin `WASB\_PATH` önceki adımdan yola sahip.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Bu kod bir dataframe sürekli dışarı aktarma işlemi tarafından dışarı aktarılan JSON dosyaları oluşturur. Kullanım **SHIFT + ENTER** bu hücresini çalıştırmak için.

7. Sonraki hücrenin girin ve Spark'ı için JSON dosyalarını oluşturduğunuz şemasını görüntülemek için aşağıdaki komutu çalıştırın:

   ```scala
   jsonData.printSchema
   ```

    Telemetrinin her türü için şema farklıdır. Aşağıdaki örnek, web istekleri için oluşturulan şema (depolanan verileri `Requests` alt):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Geçici tablo olarak veri çerçevesi kaydetmek ve veriler karşı sorgu çalıştırmak için aşağıdakileri kullanın:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Bu sorgu, burada context.location.city null değil en çok 20 kayıtlar için Şehir bilgilerini döndürür.

   > [!NOTE]  
   > Application Insights tarafından günlüğe kaydedilen tüm telemetri mevcut bağlam yapısıdır. Şehir öğesi günlükleri doldurulmuş değil. Günlükleriniz için veri içerebilir sorgulayabilirsiniz diğer öğeleri tanımlamak için şemayı kullanın.
   >
   >

    Bu sorgu, aşağıdaki metne benzer bilgileri döndürür:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Sonraki adımlar

Veri ve hizmetlerinizi azure'da çalışmak için Apache Spark'ı kullanarak daha fazla örnek için aşağıdaki belgelere bakın:

* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

Oluşturma ve Spark uygulamaları çalıştırma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)
