---
title: -Azure Hdınsight Spark ile uygulama Insight günlüklerini çözümleme | Microsoft Docs
description: Blob depolama ve ardından günlüklerini hdınsight'ta Spark ile analiz etmek için uygulama Insight günlükleri dışarı aktarmak öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: larryfr
ms.openlocfilehash: 31068376e20b240a440432319e65f4e479163ee0
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33939609"
---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>Application Insights telemetri günlüklerini hdınsight'ta Spark ile çözümleme

Uygulama Insight telemetri verileri çözümlemek için Hdınsight'ta Spark kullanmayı öğrenin.

[Visual Studio Application Insights](../../application-insights/app-insights-overview.md) web uygulamalarınızın izleyen bir analiz hizmetidir. Application Insights tarafından oluşturulan telemetri verileri Azure depolama alanına aktarılabilir. Verileri Azure depolama alanında olduğunda, Hdınsight incelemek üzere kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

* Application Insights kullanmak üzere yapılandırılmış bir uygulama.

* Linux tabanlı Hdınsight kümesi oluşturma ile benzer. Daha fazla bilgi için bkz: [hdınsight'ta Spark oluşturma](apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]
  > Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir web tarayıcısı.

Aşağıdaki kaynaklar bu belgeyi test ve geliştirme de kullanılıyordu:

* Uygulama Insights telemetri verilerini kullanarak üretilen bir [Application Insights kullanmak üzere yapılandırılmış Node.js web uygulamasına](../../application-insights/app-insights-nodejs.md).

* Linux tabanlı bir Spark Hdınsight kümesi sürüm 3.5 üzerinde verileri çözümlemek için kullanıldı.

## <a name="architecture-and-planning"></a>Mimari ve planlama

Aşağıdaki diyagramda bu örnek, hizmet mimarisi gösterilmektedir:

![hdınsight'ta Spark tarafından işlenmekte olan sonra Application Insights blob depolama alanına akan veri gösteren diyagram](./media/apache-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure Storage

Application Insights telemetri bilgilerini BLOB'lar için sürekli olarak dışarı aktarmak için yapılandırılabilir. Hdınsight sonra blob'larda depolanan verileri okuyabilir. Ancak, uymanız gereken bazı gereksinimler vardır:

* **Konum**: depolama hesabı ve Hdınsight farklı konumlarda varsa, gecikme süresini artırabilir. Aynı zamanda bölgeler arasında taşıma verilere ücretleri uygulanır çıkış olarak maliyeti artırır.

    > [!WARNING]
    > Hdınsight farklı bir konumda bir depolama hesabıyla desteklenmiyor.

* **BLOB türü**: Hdınsight yalnızca blok bloblarını destekler. Blok blobları kullanarak uygulama Öngörüler varsayılanlarını şekilde çalışmalıdır varsayılan Hdınsight ile.

Varolan bir kümeye depolama ekleme hakkında daha fazla bilgi için bkz: [ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md) belge.

### <a name="data-schema"></a>Veri şeması

Application Insights sağlar [veri modeli verme](../../application-insights/app-insights-export-data-model.md) bilgi telemetri veri biçimi için dışa aktarılan BLOB'lar için. Bu belgede yer alan adımlar, verilerle çalışmak için Spark SQL kullanın. Spark SQL, Application Insights tarafından günlüğe kaydedilen JSON veri yapısı için bir şema otomatik olarak oluşturabilir.

## <a name="export-telemetry-data"></a>Telemetri verileri dışarı aktarma

Adımları [yapılandırma sürekli verme](../../application-insights/app-insights-export-telemetry.md) bir Azure storage blobuna telemetri bilgilerini dışarı aktarmak için Application Insights yapılandırmak için.

## <a name="configure-hdinsight-to-access-the-data"></a>Hdınsight verilere erişmek için yapılandırma

Hdınsight kümesi oluşturuyorsanız, küme oluşturma sırasında depolama hesabı ekleyin.

Varolan bir kümeye Azure depolama hesabı eklemek için bilgileri kullanın. [ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md) belge.

## <a name="analyze-the-data-pyspark"></a>Verileri çözümlemek: PySpark

1. Gelen [Azure portal](https://portal.azure.com), Hdınsight kümesinde, Spark seçin. Gelen **hızlı bağlantılar** bölümünde, select **küme panolarında**ve ardından **Jupyter not defteri** küme Dashboard__ bölümünden.

    ![Küme panolarında](./media/apache-spark-analyze-application-insight-logs/clusterdashboards.png)

2. Jupyter sayfanın sağ üst köşesinde seçin **yeni**ve ardından **PySpark**. Python tabanlı Jupyter not defteri içeren yeni bir tarayıcı sekmesi açar.

3. İlk alanında (adlı bir **hücre**) sayfasında, aşağıdaki metni girin:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Bu kod yinelemeli olarak erişim dizin yapısına giriş verileri için Spark yapılandırır. Application Insights telemetrisi benzer bir dizin yapısına kaydedilir `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Kullanım **SHIFT + ENTER** kodu çalıştırmak için. Hücre sol tarafındaki bir '\*' Bu hücreyi kodda yürütülmekte olan göstermek için köşeli ayraçlar arasında görünür. Tamamlandığında, '\*' numarası ve aşağıdakine benzer bir çıktı değişiklikler hücrede görüntülenir:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Yeni bir hücreye birinci altında oluşturulur. Yeni hücreye aşağıdaki metni girin. Değiştir `CONTAINER` ve `STORAGEACCOUNT` Azure depolama hesabı adı ve Application Insights verileri içeren blob kapsayıcı adı.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Kullanım **SHIFT + ENTER** Bu hücreyi yürütülecek. Aşağıdakine benzer bir sonuç bakın:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Döndürülen wasb yol Application Insights telemetri verilerini konumudur. Değişiklik `hdfs dfs -ls` satır hücresine döndürülen wasb yolu kullanın ve sonra **SHIFT + ENTER** hücre yeniden çalıştırmak için. Bu süre, telemetri verilerini içeren dizinleri sonuçları görüntülemesi gerekir.

   > [!NOTE]
   > Bu bölümdeki adımları kalanı için `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` dizin kullanıldı. Dizin yapısını farklı olabilir.

6. Sonraki hücreye aşağıdaki kodu girin: Değiştir `WASB_PATH` önceki adımdan yoluna sahip.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Bu kod bir dataframe sürekli dışa aktarma işlemi tarafından dışarı aktarılan JSON dosyaları oluşturur. Kullanım **SHIFT + ENTER** Bu hücreyi çalıştırmak için.
7. Sonraki hücrenin girin ve JSON dosyaları için Spark oluşturulan şemasını görüntülemek için aşağıdakini çalıştırın:

   ```python
   jsonData.printSchema()
   ```

    Şemanın telemetri her tür için farklıdır. Aşağıdaki örnek web istekleri için oluşturulan şema aynıdır (depolanan verileri `Requests` alt dizin):

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
8. Geçici bir tablo olarak dataframe kaydetmek ve veri karşı sorgu çalıştırmak için aşağıdakileri kullanın:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Bu sorgu, burada context.location.city null olmayan ilk 20 kayıt Şehir bilgilerini döndürür.

   > [!NOTE]
   > Bağlam yapısı Application Insights tarafından günlüğe kaydedilen tüm telemetri bulunur. Şehir öğesi, günlüklerinize doldurulmuş değil. Günlükleriniz için verileri içerebilir Sorgulayabileceğiniz diğer öğeleri tanımlamak için şema kullanın.

    Bu sorgu bilgileri aşağıdaki metni benzer döndürür:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-scala"></a>Verileri çözümlemek: Scala

1. Gelen [Azure portal](https://portal.azure.com), Hdınsight kümesinde, Spark seçin. Gelen **hızlı bağlantılar** bölümünde, select **küme panolarında**ve ardından **Jupyter not defteri** küme Dashboard__ bölümünden.

    ![Küme panolarında](./media/apache-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Jupyter sayfanın sağ üst köşesinde seçin **yeni**ve ardından **Scala**. Scala tabanlı Jupyter not defteri içeren yeni bir tarayıcı sekmesi görüntülenir.
3. İlk alanında (adlı bir **hücre**) sayfasında, aşağıdaki metni girin:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Bu kod yinelemeli olarak erişim dizin yapısına giriş verileri için Spark yapılandırır. Application Insights telemetrisi benzer bir dizin yapısına kaydedilir `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Kullanım **SHIFT + ENTER** kodu çalıştırmak için. Hücre sol tarafındaki bir '\*' Bu hücreyi kodda yürütülmekte olan göstermek için köşeli ayraçlar arasında görünür. Tamamlandığında, '\*' numarası ve aşağıdakine benzer bir çıktı değişiklikler hücrede görüntülenir:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Yeni bir hücreye birinci altında oluşturulur. Yeni hücreye aşağıdaki metni girin. Değiştir `CONTAINER` ve `STORAGEACCOUNT` Azure depolama hesabı adı ve Application Insights içeren blob kapsayıcı adı ile günlüğe kaydeder.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Kullanım **SHIFT + ENTER** Bu hücreyi yürütülecek. Aşağıdakine benzer bir sonuç bakın:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Döndürülen wasb yol Application Insights telemetri verilerini konumudur. Değişiklik `hdfs dfs -ls` satır hücresine döndürülen wasb yolu kullanın ve sonra **SHIFT + ENTER** hücre yeniden çalıştırmak için. Bu süre, telemetri verilerini içeren dizinleri sonuçları görüntülemesi gerekir.

   > [!NOTE]
   > Bu bölümdeki adımları kalanı için `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` dizin kullanıldı. Bir web uygulaması için telemetri verilerinizi olmadığı sürece bu dizinin var olmayabilir.

6. Sonraki hücreye aşağıdaki kodu girin: Değiştir `WASB\_PATH` önceki adımdan yoluna sahip.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Bu kod bir dataframe sürekli dışa aktarma işlemi tarafından dışarı aktarılan JSON dosyaları oluşturur. Kullanım **SHIFT + ENTER** Bu hücreyi çalıştırmak için.

7. Sonraki hücrenin girin ve JSON dosyaları için Spark oluşturulan şemasını görüntülemek için aşağıdakini çalıştırın:

   ```scala
   jsonData.printSchema
   ```

    Şemanın telemetri her tür için farklıdır. Aşağıdaki örnek web istekleri için oluşturulan şema aynıdır (depolanan verileri `Requests` alt dizin):

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

8. Geçici bir tablo olarak dataframe kaydetmek ve veri karşı sorgu çalıştırmak için aşağıdakileri kullanın:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Bu sorgu, burada context.location.city null olmayan ilk 20 kayıt Şehir bilgilerini döndürür.

   > [!NOTE]
   > Bağlam yapısı Application Insights tarafından günlüğe kaydedilen tüm telemetri bulunur. Şehir öğesi, günlüklerinize doldurulmuş değil. Günlükleriniz için verileri içerebilir Sorgulayabileceğiniz diğer öğeleri tanımlamak için şema kullanın.
   >
   >

    Bu sorgu bilgileri aşağıdaki metni benzer döndürür:

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

Veriler ve Azure Hizmetleri ile çalışmak için Spark kullanmanın daha fazla örnek için aşağıdaki belgelere bakın:

* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

Oluşturma ve Spark çalışan uygulamalar hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)
