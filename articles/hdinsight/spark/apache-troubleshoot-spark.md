---
title: Spark Azure Hdınsight kullanarak sorun giderme | Microsoft Docs
description: Apache Spark ve Azure Hdınsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
keywords: Sorun giderme kılavuzu, ortak sorunları, uygulama yapılandırması, Ambari azure Hdınsight, Spark, SSS,
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: ''
editor: ''
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 11/2/2017
ms.author: arijitt
ms.openlocfilehash: c097a346e64fa378f171e0a0fe03155551da98ed
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Spark Azure Hdınsight kullanarak sorun giderme

Apache Ambari, Apache Spark yükü ile çalışırken, üst sorunları ve bunların çözümleri hakkında bilgi edinin.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Spark uygulama kümelerinde Ambari kullanarak nasıl yapılandırırım?

### <a name="resolution-steps"></a>Çözüm adımları

Bu yordam için yapılandırma değerlerini daha önce Hdınsight'ta ayarlandı. Hangi Spark belirlemek için yapılandırmaları ayarlanması ve değerleri için bkz: gerekir [ne bir Spark uygulama OutofMemoryError özel duruma neden olan](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. Kümeleri listesinde seçin **Spark2**.

    ![Küme listeden seçin](./media/apache-troubleshoot-spark/update-config-1.png)

2. Seçin **yapılandırmalar** sekmesi.

    ![Yapılandırmalar sekmesini seçin](./media/apache-troubleshoot-spark/update-config-2.png)

3. Yapılandırmaları listesinde seçin **özel spark2 varsayılanları**.

    ![Özel spark varsayılanlarını seçin](./media/apache-troubleshoot-spark/update-config-3.png)

4. Gibi ayarlamak için gereken değer ayarını Ara **spark.executor.memory**. Bu durumda, değeri **4608m** çok yüksek.

    ![Spark.executor.memory alanı seçin](./media/apache-troubleshoot-spark/update-config-4.png)

5. Önerilen ayar değeri ayarlayın. Değer **2048m** Bu ayar için önerilir.

    ![2048 m değerini değiştirin](./media/apache-troubleshoot-spark/update-config-5.png)

6. Değer kaydedin ve ardından yapılandırmayı kaydedin. Araç çubuğunda seçin **kaydetmek**.

    ![Ayar ve yapılandırmayı kaydedin](./media/apache-troubleshoot-spark/update-config-6a.png)

    Tüm yapılandırmaları dikkat etmeniz gereken varsa size bildirilir. Öğeleri not edin ve ardından **yine de devam**. 

    ![Select yine de devam etmek](./media/apache-troubleshoot-spark/update-config-6b.png)

    Yapılandırma değişiklikleri hakkında bir not yazın ve ardından **kaydetmek**.

    ![Yaptığınız değişiklikleri hakkında bir not girin](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Bir yapılandırma kaydedildiği zaman hizmeti yeniden başlatmanız istenir. Seçin **yeniden**.

    ![Yeniden başlatma seçin](./media/apache-troubleshoot-spark/update-config-7a.png)

    Yeniden başlatma onaylayın.

    ![Select onaylayın tüm yeniden başlatın](./media/apache-troubleshoot-spark/update-config-7b.png)

    Çalışan işlemleri gözden geçirebilirsiniz.

    ![Çalışan işlemleri gözden geçirin](./media/apache-troubleshoot-spark/update-config-7c.png)

8. Yapılandırmaları ekleyebilirsiniz. Yapılandırmaları listesinde seçin **özel spark2 varsayılanları**ve ardından **Özellik Ekle**.

    ![Özellik Seç ekleme](./media/apache-troubleshoot-spark/update-config-8.png)

9. Yeni bir özellik tanımlayın. Veri türü gibi belirli ayarlar için bir iletişim kutusunu kullanarak tek bir özellik tanımlayabilirsiniz. Ya da her satırda tek bir tanım kullanarak birden çok özellik tanımlayabilirsiniz. 

    Bu örnekte, **spark.driver.memory** özellik değeri ile tanımlanmış **4g**.

    ![Yeni özellik tanımlama](./media/apache-troubleshoot-spark/update-config-9.png)

10. Yapılandırmayı kaydetmek ve 6 ve 7. adımda açıklandığı gibi hizmeti yeniden başlatın.

Bu değişiklikler, küme çapında ancak Spark iş gönderdiğinizde geçersiz kılınabilir.

### <a name="additional-reading"></a>Ek kaynaklar

[Hdınsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Spark uygulama kümelerinde Jupyter Not Defteri kullanarak nasıl yapılandırırım?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark belirlemek için yapılandırmaları ayarlanması ve değerleri için bkz: gerekir [ne bir Spark uygulama OutofMemoryError özel duruma neden olan](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Jupyter Not Defteri, ilk hücrenin sonra **%% yapılandırma** yönergesi, Spark yapılandırmalarını geçerli JSON biçiminde belirtin. Gerçek değerleri gerektiği gibi değiştirin:

    ![Bir yapılandırma ekleyin](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Ek kaynaklar

[Hdınsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Spark uygulama kümelerinde Livy kullanarak nasıl yapılandırırım?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark belirlemek için yapılandırmaları ayarlanması ve değerleri için bkz: gerekir [ne bir Spark uygulama OutofMemoryError özel duruma neden olan](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. REST istemcisi cURL gibi kullanarak Livy Spark uygulamaya gönderin. Aşağıdakine benzer bir komutunu kullanın. Gerçek değerleri gerektiği gibi değiştirin:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Ek kaynaklar

[Hdınsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Kümelerinde nasıl kullanarak uygulama spark gönderme Spark yapılandırırım?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark belirlemek için yapılandırmaları ayarlanması ve değerleri için bkz: gerekir [ne bir Spark uygulama OutofMemoryError özel duruma neden olan](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Spark Kabuk aşağıdakine benzer bir komut kullanarak başlatın. Yapılandırmaları gerçek değeri gerektiği gibi değiştirin: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Ek kaynaklar

[Hdınsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Bir Spark uygulama OutofMemoryError özel durum nedeni nedir?

### <a name="detailed-description"></a>Ayrıntılı açıklama

Spark uygulama, aşağıdaki türden yakalanmayan bir özel durum ile başarısız olur:

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Olası neden

Büyük olasılıkla bu özel durum yığın bellek yetersiz Java sanal makinelere (JVMs) ayrılır nedenidir. Bu JVMs yürütücüler veya sürücüler olarak Spark uygulamanın bir parçası başlatılır. 

### <a name="resolution-steps"></a>Çözüm adımları

1. Spark veri en büyük boyutunu belirlemek uygulama işler. Giriş verilerini, giriş verileri dönüştürme tarafından üretilen ara veri ve uygulama daha fazla ara veri dönüştürülürken üreten çıktı verilerini en büyük boyutuna göre tahmin yapabilirsiniz. İlk resmi tahmin kuramıyorsa bu işlem bir yinelemeli olabilir. 

2. Kullanacağınız Hdınsight kümesi bellek ve Spark uygulama uyum sağlamak için çekirdek açısından yeterli kaynaklara sahip olduğundan emin olun. Bu değerler için YARN kullanıcı Arabiriminde küme ölçümleri bölümünü görüntüleyerek belirlemek **kullanılan bellek** vs. **Bellek toplam**, ve **VCores kullanılan** vs. **VCores toplam**.

3. Aşağıdaki Spark yapılandırmaları kullanılabilir bellek ve çekirdek % 90'ını aşmamalıdır uygun değerlere ayarlayın. Değerler iyi bellek gereksinimlerini Spark uygulama içinde olmalıdır: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    Tüm yürütücüler tarafından kullanılan toplam bellek hesaplama için: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
   Sürücü tarafından kullanılan toplam bellek hesaplama için:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>Ek kaynaklar

- [Spark bellek yönetimine genel bakış](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Bir Hdınsight kümesi üzerinde Spark uygulamanızın hatalarını ayıklama](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)


### <a name="see-also"></a>Ayrıca Bkz.
[Azure Hdınsight kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)

