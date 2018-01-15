---
title: "Apache Spark kullanma yapılandırılmış Azure hdınsight'ta Event Hubs ile akış | Microsoft Docs"
description: "Örnek veri akışı Azure Event Hub'ına olayları alıp göndermek sonra bu scala uygulaması kullanarak Hdınsight Spark kümesinde konusunda akış bir Apache Spark oluşturun."
keywords: "Apache spark akış, spark akış, spark örnek, apache spark akış örneği, olay hub'ı azure örneği, spark örnek"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: e0486d2c5f78da1d1e4a12703f120eccef43c305
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-structured-streaming-on-hdinsight-to-process-events-from-event-hubs"></a>Apache Spark yapılandırılmış akış olay hub'ları olayları işlemek için hdınsight'ta

Bu makalede, Spark yapılandırılmış akış kullanarak gerçek zamanlı telemetri işlemek öğrenin. Bunu gerçekleştirmek için aşağıdaki üst düzey adımları gerçekleştirin:

1. Derleme ve yerel iş istasyonunda Event Hubs'a göndermek için olayları oluşturan bir örnek olay üretici uygulamayı çalıştırın.
2. Kullanım [Spark Kabuk](apache-spark-shell.md) tanımlama ve basit bir Spark yapılandırılmış akış uygulamayı çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

* Bir Azure Event Hubs ad alanı. Bkz: [Azure Event Hubs ad alanı oluşturma](apache-spark-eventhub-streaming.md#create-an-azure-event-hub) daha fazla bilgi için.

* Oracle Java Geliştirme Seti. Şuradan yükleyebilirsiniz [burada](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

* Apache Maven. Buradan indirebilirsiniz [burada](https://maven.apache.org/download.cgi). Maven yüklemeye yönelik yönergeleri [burada](https://maven.apache.org/install.html).

## <a name="build-configure-and-run-the-event-producer"></a>Derleme, yapılandırma ve olay üretici çalıştırma
Bu görevde, rastgele olayları oluşturan ve yapılandırılmış bir Event Hub'ına gönderir örnek bir uygulama kopyalayın. Bu örnek uygulama, GitHub üzerinde kullanılabilir [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).

1. Git araçlarının yüklü olduğundan emin olun. İndirebileceğiniz [GIT indirmeleri](http://www.git-scm.com/downloads). 
2. Bir komut istemi veya terminal Kabuğu'nu açın ve projeyi kopyalamak için tercih ettiğiniz dizininden aşağıdaki komutu çalıştırın.
        
        git clone https://github.com/hdinsight/eventhubs-sample-event-producer.git eventhubs-client-sample

3. Bu eventhubs istemci örnek adlı yeni bir klasör oluşturur. Kabuk veya komut istemi içinde bu klasöre gidin.
4. Aşağıdaki komutu kullanarak uygulamayı oluşturmak için Maven çalıştırın:

          mvn package

5. Kabuk veya komut istemi içinde oluşturulan ve dosyayı içeren hedef dizine gidin ``com-microsoft-azure-eventhubs-client-example-0.2.0.jar``.
6. Ardından, olay üretici karşı olay Hub'ınızı çalıştırmak için komut satırı derleme gerekir. Bu komut değerler şu şekilde değiştirerek yapın:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "<namespace>" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "<policyKey>" --message-length 32 --thread-count 1 --message-count -1

7. Örneğin, tam komut aşağıdakine benzer görünür:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "sparkstreaming" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU=" --message-length 32 --thread-count 1 --message-count -1

8. Komut başlatıldığında ve yapılandırmanızı birkaç dakika sonra doğru ise, olay hub ' ınızı aşağıdakine benzer gönderme olayları ilgili çıktı bakın:

        Map('policyKey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, 'eventhubsName -> hub1, 'policyName -> RootManageSharedAccessKey, 'eventhubsNamespace -> sparkstreaming, 'messageCount -> -1, 'messageLength -> 32, 'threadCount -> 1)
        Events per thread: -1 (-1 for unlimited)
        10 > Sun Jun 18 11:32:58 PDT 2017 > 1024 > 1024 > Sending event: ZZ93958saG5BUKbvUI9wHVmpuA2TrebS
        10 > Sun Jun 18 11:33:46 PDT 2017 > 2048 > 2048 > Sending event: RQorGRbTPp6U2wYzRSnZUlWEltRvTZ7R
        10 > Sun Jun 18 11:34:33 PDT 2017 > 3072 > 3072 > Sending event: 36Eoy2r8ptqibdlfCYSGgXe6ct4AyOX3
        10 > Sun Jun 18 11:35:19 PDT 2017 > 4096 > 4096 > Sending event: bPZma9V0CqOn6Hj9bhrrJT0bX2rbPSn3
        10 > Sun Jun 18 11:36:06 PDT 2017 > 5120 > 5120 > Sending event: H2TVD77HNTVyGsVcj76g0daVnYxN4Sqs

9. Adımları devam ederken olay üretici çalışır durumda bırakın.

## <a name="run-spark-shell-on-your-hdinsight-cluster"></a>Hdınsight kümenize Spark Shell çalıştırma
Bu görevde şunları yapacaksınız SSH baş düğüme Hdınsight kümenizin Spark Kabuğu'nu başlatın ve alır ve olayları Event Hubs'dan işleyen bir Spark akış uygulamayı çalıştırın. 

Bu noktaya Hdınsight kümenize hazır olmanız gerekir. Aksi durumda, sağlama sonlanana kadar beklemeniz gerekir. Hazır olduktan sonra aşağıdaki adımlarla devam edin:

1. SSH kullanarak Hdınsight kümesine bağlanın. Yönergeler için bkz: [SSH kullanarak Hdınsight Bağlan](../hdinsight-hadoop-linux-use-ssh-unix.md).

5. Oluşturduğunuz uygulama Spark akış olay hub'ları paketi gerektirir. Bu bağımlılık gelen otomatik olarak alır Spark Kabuk çalıştırılacak [Maven Merkezi](https://search.maven.org), emin olun paketleri geçiş Maven koordinatlarıyla gibi sağlayın:

        spark-shell --packages "com.microsoft.azure:spark-streaming-eventhubs_2.11:2.1.5"

6. Spark Kabuk tamamlandıktan sonra yükleme, görmeniz gerekir:

        Welcome to
            ____              __
            / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
        /___/ .__/\_,_/_/ /_/\_\   version 2.1.1.2.6.2.3-1
            /_/
                
        Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_151)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> 

7. Aşağıdaki kod parçacığını bir metin düzenleyicisine kopyalayın ve ilke anahtarına sahipse ve Namespace olay Hub'ınız için uygun şekilde ayarlamak için değiştirin.

        val eventhubParameters = Map[String, String] (
            "eventhubs.policyname" -> "RootManageSharedAccessKey",
            "eventhubs.policykey" -> "<policyKey>",
            "eventhubs.namespace" -> "<namespace>",
            "eventhubs.name" -> "hub1",
            "eventhubs.partition.count" -> "2",
            "eventhubs.consumergroup" -> "$Default",
            "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            "eventhubs.sql.containsProperties" -> "true"
            )
            
8. Aşağıdaki biçimde EventHub ile uyumlu uç noktanızı bakarsanız, okuyan bölümü `iothub-xxxxxxxxxx` EventHub-compatible Namespace adınız ve için kullanılan `eventhubs.namespace`. Alan `SharedAccessKeyName` için kullanılan `eventhubs.policyname`, ve `SharedAccessKey` için `eventhubs.policykey`: 

        Endpoint=sb://iothub-xxxxxxxxxx.servicebus.windows.net/;SharedAccessKeyName=xxxxx;SharedAccessKey=xxxxxxxxxx 

9. Bekleme scala içinde değiştirilen parçacığını yapıştırın > istemine ve return tuşuna basın. Aşağıdakine benzer bir çıktı görmeniz gerekir:

        scala> val eventhubParameters = Map[String, String] (
            |       "eventhubs.policyname" -> "RootManageSharedAccessKey",
            |       "eventhubs.policykey" -> "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU",
            |       "eventhubs.namespace" -> "sparkstreaming",
            |       "eventhubs.name" -> "hub1",
            |       "eventhubs.partition.count" -> "2",
            |       "eventhubs.consumergroup" -> "$Default",
            |       "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            |       "eventhubs.sql.containsProperties" -> "true"
            |     )
        eventhubParameters: scala.collection.immutable.Map[String,String] = Map(eventhubs.sql.containsProperties -> true, eventhubs.name -> hub1, eventhubs.consumergroup -> $Default, eventhubs.partition.count -> 2, eventhubs.progressTrackingDir -> /eventhubs/progress, eventhubs.policykey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, eventhubs.namespace -> hdiz-docs-eventhubs, eventhubs.policyname -> RootManageSharedAccessKey)

10. Ardından, akış bir Spark yapılandırılmış Yazar başlamadan sorgu kaynağı belirtmeyi. Aşağıdaki Spark kabuğundan yapıştırın ve return tuşuna basın.

        val inputStream = spark.readStream.
        format("eventhubs").
        options(eventhubParameters).
        load()

11. Aşağıdakine benzer bir çıktı görmeniz gerekir:

        inputStream: org.apache.spark.sql.DataFrame = [body: binary, offset: bigint ... 5 more fields]

12. Ardından, böylece çıktısını konsola yazar sorgu yazar. Bunu aşağıdaki Spark kabuğundan yapıştırma ve return tuşuna basın.

        val streamingQuery1 = inputStream.writeStream.
        outputMode("append").
        format("console").start().awaitTermination()

13. Aşağıdakine benzer bir çıktı başlayın bazı toplu görmeniz gerekir

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        [Stage 0:>                                                          (0 + 2) / 2]

14. Bu olayların her microbatch işlenmesini çıkış sonuçlarını tarafından izlenir. 

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        17/06/18 18:57:39 WARN TaskSetManager: Stage 1 contains a task of very large size (419 KB). The maximum recommended task size is 100 KB.
        +--------------------+------+---------+------------+---------+------------+----------+
        |                body|offset|seqNumber|enqueuedTime|publisher|partitionKey|properties|
        +--------------------+------+---------+------------+---------+------------+----------+
        |[7B 22 74 65 6D 7...|     0|        0|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   112|        1|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   224|        2|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   336|        3|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   448|        4|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   560|        5|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   672|        6|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   784|        7|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   896|        8|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1008|        9|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1120|       10|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1232|       11|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1344|       12|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1456|       13|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1568|       14|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1680|       15|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1792|       16|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1904|       17|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2016|       18|  1497734889|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2128|       19|  1497734889|     null|        null|     Map()|
        +--------------------+------+---------+------------+---------+------------+----------+
        only showing top 20 rows

15. Yeni olaylar olay üreticiden geldikçe, bunlar bu yapılandırılmış akış sorgu tarafından işlenir.
16. Bu örnek çalıştıran bittiğinde Hdınsight kümenizi sildiğinizden emin olun.



## <a name="see-also"></a>Ayrıca bkz.

* [Spark akış genel bakış](apache-spark-streaming-overview.md)













