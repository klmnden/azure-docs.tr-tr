---
title: "Azure hdınsight'ta Spark Akış nedir | Microsoft Docs"
description: "Hdınsight Spark kümeleri üzerinde Spark akış uygulamaları kullanma"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: raghavmohan
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: ramoha
ms.openlocfilehash: 9debf78480e6c77ab23581754350fb95f34f8fc4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="overview-of-spark-streaming"></a>Spark akış genel bakış

Spark akış veri akışı işleme Hdınsight Spark kümeleri, bir düğüm arıza durumunda bile herhangi bir giriş olay tam olarak bir kez işlenir garantisi sağlar. Spark kaynakları, Azure Event Hubs, bir Azure IOT Hub, Kafka, Flume, Twitter, ZeroMQ, ham TCP yuvaları dahil olmak üzere çok çeşitli veya HDFS bağlanan dosya sistemlerinin izleme giriş verileri alan bir uzun süre çalışan iş akışıdır. Yalnızca olay denetimli bir işlem aksine bir Spark akış 2 saniyelik dilim gibi zaman pencereleri halinde giriş verileri toplu işlemleri ve veri eşlemesi kullanarak her bir toplu iş dönüştürür, azaltmak, birleştirme ve işlemleri ayıklayın. Bunu ardından dönüştürülmüş verilerin bağlanan dosya sistemlerinin, veritabanları, panolar ve konsol yazar.

![Akış Hdınsight Spark akış ile işleme](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Spark akış uygulamaları birkaç kesirler her toplamak için bir saniye beklemesi gereken *mikro toplu* toplu işleme için göndermeden önce olayların. Buna karşılık, her olay bir olay denetimli uygulama hemen işler. Spark akış gecikme süresi genellikle birkaç altında saniyedir. Mikro toplu yaklaşım yararları daha verimli veri işleme ve daha basit toplam hesaplamaları ' dir.

## <a name="introducing-the-dstream"></a>DStream Tanıtımı

Spark akış gelen verileri kullanarak, sürekli akışı temsil eden bir *ayrılmış akış* bir DStream çağrılır. Olay hub'ları veya Kafka gibi ya da başka bir DStream dönüşümleri uygulama giriş kaynaklardan bir DStream oluşturulabilir.

Bir DStream ham olay verileri en üstünde bir soyutlama katmanı sağlar. 

Tek bir olayla başlatmak için bağlı thermostat okuma sıcaklık söyleyin. Bu olay Spark akış uygulamanızı geldiğinde olay güvenilir bir şekilde depolanır, diğer bir deyişle, birden çok düğümde çoğaltılır. Tek bir düğüm hatası olayınızın kaybına neden değil Bu hataya dayanıklılık sağlar. Spark core kümedeki birden çok düğüm arasında verilerini dağıtan bir veri yapısı burada her düğüm genellikle kendi veri bellek içi en iyi performans için kullanır. Bu veri yapısını adlı bir *dayanıklı dağıtılmış dataset* (RDD).

Her RDD toplu iş aralığı adı verilen bir kullanıcı tanımlı bir zaman çerçevesi toplanan olayları temsil eder. Her toplu iş aralığı sona erdiğinde gibi yeni RDD bu aralığından tüm verileri içeren oluşturulur. RDDs sürekli kümesini DStream toplanır. Örneğin, toplu iş aralığı bir saniye uzun olması durumunda, DStream toplu sırasında bu ikinci alınan tüm verileri içeren ikinci her içeren bir RDD yayar. DStream işlerken sıcaklık olay bu toplu biri görüntülenir. Spark akış uygulama olayları içeren toplu işler ve sonuçta her RDD depolanan veriler üzerinde çalışır.

![Örnek DStream sıcaklık olayları ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Spark akış uygulama yapısı
Spark akış uygulama alma kaynaklardan veri aldığı, verileri işlemek için dönüşümleri uygular ve verileri bir veya daha fazla hedeflere sonra iter uzun süre çalışan bir uygulamadır. Spark akış uygulama yapısını iki ana bölümden oluşur. Verileri, verilerin yapmak için hangi işleme geldiği durumlarda ve burada tanımladığınız ilk olarak, sonuçların tamamlamalıdır. İkinci olarak, uygulamayı süresiz olarak, bir durdurma sinyali için bekleyen çalıştırın.

Örneğin, metin satırının üzerinde bir TCP yuvasını alır ve her sözcüğün görünür sayısını sayar basit bir uygulamanız aşağıdadır.

### <a name="define-the-application"></a>Uygulama tanımlama

Uygulama mantığı tanımı dört adımdan oluşur:

1. Bir StreamingContext oluşturun.
2. Bir DStream StreamingContext oluşturun.
3. Dönüşümleri DStream uygulayın.
4. Sonuçları çıktı.

Bu statik tanımıdır ve uygulama çalıştırana kadar veri işlenir.

#### <a name="create-a-streamingcontext"></a>Bir StreamingContext oluşturma
Bir StreamingContext kümenize işaret SparkContext oluşturun. Bir StreamingContext oluştururken, toplu iş boyutu saniye cinsinden, örneğin belirtin:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Bir DStream oluşturma
Oluşturduğunuz StreamingContext örneği kullanarak, giriş kaynağınız için bir giriş DStream oluşturun. Bu durumda, biz Hdınsight kümesine bağlı varsayılan depolama yeni dosyalar görünümünü için izlerken.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Dönüştürmeleri
DStream dönüşümleri uygulama tarafından işlenmesi uygulayın. Uygulamamız tek satırlık metin dosyasından bir zaman alır, her satırın sözcüklere böler ve her sözcüğün görünür kez sayısı için bir map-reduce desen kullanır.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Çıktı sonuçları
Dönüşüm sonuçları çıktı işlemleri uygulayarak hedef sistemleri itme. Bu durumda, her çalıştırmayı hesaplama sonucu konsol çıktısı gösteriyoruz.

    wordCounts.print()

### <a name="run-the-application"></a>Uygulamayı çalıştırma
Akış uygulama başlatma ve sonlandırma sinyali alınana kadar çalıştırın.

    ssc.start()            
    ssc.awaitTermination()

Spark akış API'si hakkında daha fazla ayrıntı için olay kaynakları, dönüştürmeleri ve çıkış işlemlerinin yanı sıra bakın destekliyorsa [Spark akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Aşağıdaki örnek uygulama içinde çalıştırabilmeniz için kendi içinde bulunan bir [Jupyter not defteri](apache-spark-jupyter-notebook-kernels.md). Örnekte, bir sayacın değeri çıkarır DummySource sınıfında sahte veri kaynağı ve milisaniye olarak geçerli süre beş saniyede oluşturuyoruz. 30 saniyelik bir toplu iş aralığı olan yeni bir StreamingContext nesneyi oluşturuyoruz. Bir toplu iş her oluşturulduğunda, üretilen RDD inceler, bir Spark DataFrame dönüştürür ve DataFrame üzerinde geçici bir tablo oluşturur.

    class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

        /** Start the thread that simulates receiving data */
        def onStart() {
            new Thread("Dummy Source") { override def run() { receive() } }.start()
        }

        def onStop() {  }

        /** Periodically generate a random number from 0 to 9, and the timestamp */
        private def receive() {
            var counter = 0  
            while(!isStopped()) {
            store(Iterator((counter, System.currentTimeMillis)))
            counter += 1
            Thread.sleep(5000)
            }
        }
    }

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process RDDs in the batch
    stream.foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
            .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Biz sonra düzenli aralıklarla toplu işinde var olan değerleri geçerli kümesini görmek için DataFrame sorgulayabilirsiniz. Bu durumda, aşağıdaki SQL sorgusunu kullanın:

    %%sql
    SELECT * FROM demo_numbers

Sonuçta çıktı aşağıdaki gibi görünür:

| değer | time |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

DummySource 5 saniyede bir değer oluşturur ve biz her 30 saniyede bir toplu yayma altı değerleri görmek bekliyoruz.

## <a name="sliding-windows"></a>Kayan pencere
Örneğin bir ortalama sıcaklık son 2 saniye almak bazı süre boyunca, DStream üzerinde toplama hesaplamalar gerçekleştirmek istiyorsanız, Spark akış ile dahil kayan pencere işlemleri kullanabilirsiniz. Bir kayan bir pencere bir süresi olan olarak tanımlanır (penceresi uzunluğu) ve pencerenin içeriği sırasında olduğu aralığı (slayt aralığı) değerlendirilir.

Bu windows kayan binebilir, örneğin her 1 saniyede slayt 2 saniye uzunluğa sahip bir pencere tanımlayabilirsiniz. Bunun anlamı, bir toplama hesaplamayı her zaman son 1 saniye önceki penceresinin verilerden yanı sıra yeni veriler İleri 1 saniye içinde pencere içerecektir.

![Sıcaklık olaylarla ilk penceresi örneği](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Kayan sonra sıcaklık olaylarla örnek penceresi](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Aşağıdaki örnekte, bir dakikalık süre ve bir dakikalık slayt penceresine toplu toplamak için DummySource kullanan kodu güncelleştiriyoruz.

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process batches in 1 minute windows
    stream.window(org.apache.spark.streaming.Minutes(1)).foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
        .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

İlk dakika sonra bu her iki toplu işlemlerin penceresinde toplanan altı girişlerinden 12 girişleri - sunacak.

| değer | time |
| --- | --- |
| 1 | 1497316294139 |
| 2 | 1497316299158
| 3 | 1497316304178
| 4 | 1497316309204
| 5 | 1497316314224
| 6 | 1497316319243
| 7 | 1497316324260
| 8 | 1497316329278
| 9 | 1497316334293
| 10 | 1497316339314
| 11 | 1497316344339
| 12 | 1497316349361

Spark akış API'kayan pencere işlevleri penceresi, countByWindow, reduceByWindow ve countByValueAndWindow içerir. Bu işlevler hakkında ayrıntılar için bkz: [DStreams dönüşümler](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Denetim noktası oluşturma
Dayanıklılık ve hataya dayanıklılık sağlamak üzere akış işleme düğümü hataları karşısında bile kesintisiz devam edebilirsiniz güvence altına almaya denetim noktası oluşturma Spark akış seçeneğini kullanır. Hdınsight'ta Spark kontrol noktalarını kalıcı depolama birimine (Azure Storage veya Data Lake Store) oluşturur. Bu kontrol noktalarının yapılandırması, uygulama ve sıraya alınan ancak henüz işlenmemiş yığını tarafından tanımlanan işlemleri gibi akış uygulama hakkındaki meta verileri depolar. Bazı durumlarda, Spark tarafından yönetilen RDDs mevcut nedir verileri durumunu yeniden süresini kısaltmak için RDDs veriler kaydedilirken kontrol noktalarını bir de dahil edilir.

## <a name="deploying-spark-streaming-applications"></a>Spark akış uygulamaları dağıtma
Genellikle, Spark akış uygulamanızı yerel olarak oluşturur ve bunu hdınsight'ta Spark Hdınsight kümenize bağlı varsayılan depolama uygulamanıza içeren JAR dosyasını kopyalayarak dağıtın. Ardından, uygulamanızın kümenizden kullanılabilir LIVY REST API'lerini kullanarak başlatabilirsiniz. Bu POST işlemine burada POST gövdesinde, ana yöntem tanımlar ve akış uygulama ve isteğe bağlı olarak (sayısı gibi iş kaynak gereksinimlerini çalışır sınıfın adını, JAR yol sağlayan bir JSON belgesi dahildir ecutors, bellek ve çekirdek), ve uygulama kodunuz herhangi bir yapılandırma ayarı gerektirir.

![Kayan sonra sıcaklık olaylarla örnek penceresi](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumunu da LIVY uç noktası bir GET isteği ile denetlenebilir. Son olarak, dinamik uç noktası bir silme isteği göndererek çalışan bir uygulama sonlandırabilir. LIVY API'si hakkında daha fazla bilgi için bkz: [LIVY uzak işleriyle](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight'ta bir Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Spark işlerinin LIVY ile uzaktan başlatma](apache-spark-livy-rest-interface.md)