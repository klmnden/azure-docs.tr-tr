---
title: Azure Hdınsight'ta Spark akış | Microsoft Docs
description: Hdınsight Spark kümeleri üzerinde Spark akış uygulamaları kullanma
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: maxluk
ms.openlocfilehash: 2f521df81e5153affa95248cda2aa001bc5d6484
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164790"
---
# <a name="overview-of-spark-streaming"></a>Spark akış genel bakış

Spark akış veri akışı işleme Hdınsight Spark kümeleri, bir düğüm arıza durumunda bile herhangi bir giriş olay tam olarak bir kez işlenir garantisi sağlar. Spark kaynakları, Azure Event Hubs, bir Azure IOT Hub, Kafka, Flume, Twitter, ZeroMQ, ham TCP yuvaları dahil olmak üzere çok çeşitli veya HDFS bağlanan dosya sistemlerinin izleme giriş verileri alan bir uzun süre çalışan iş akışıdır. Yalnızca olay denetimli bir işlem aksine bir Spark akış 2 saniyelik dilim gibi zaman pencereleri halinde giriş verileri toplu işlemleri ve veri eşlemesi kullanarak her bir toplu iş dönüştürür, azaltmak, birleştirme ve işlemleri ayıklayın. Spark akış sonra dönüştürülmüş verilerin bağlanan dosya sistemlerinin, veritabanları, panolar ve konsol yazar.

![Akış Hdınsight Spark akış ile işleme](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Spark akış uygulamaları bir saniyenin her toplamak için beklemesi gereken *mikro toplu* toplu işleme için göndermeden önce olayların. Buna karşılık, her olay bir olay denetimli uygulama hemen işler. Spark akış gecikme süresi genellikle birkaç altında saniyedir. Mikro toplu yaklaşım yararları daha verimli veri işleme ve daha basit toplam hesaplamaları ' dir.

## <a name="introducing-the-dstream"></a>DStream Tanıtımı

Spark akış gelen verileri kullanarak, sürekli akışı temsil eden bir *ayrılmış akış* bir DStream çağrılır. Olay hub'ları veya Kafka gibi ya da başka bir DStream dönüşümleri uygulama giriş kaynaklardan bir DStream oluşturulabilir.

Bir DStream ham olay verileri en üstünde bir soyutlama katmanı sağlar. 

Tek bir olayla başlatmak için bağlı thermostat okuma sıcaklık söyleyin. Bu olay Spark akış uygulamanızı geldiğinde olay burada birden çok düğümde çoğaltılan güvenilir bir şekilde depolanır. Tek bir düğüm hatası olayınızın kaybına neden değil Bu hataya dayanıklılık sağlar. Spark core kümedeki birden çok düğüm arasında verilerini dağıtan bir veri yapısı burada her düğüm genellikle kendi veri bellek içi en iyi performans için kullanır. Bu veri yapısını adlı bir *dayanıklı dağıtılmış dataset* (RDD).

Her RDD adlı bir kullanıcı tanımlı bir zaman çerçevesi içinde toplanan olayları temsil eden *toplu iş aralığı*. Her toplu iş aralığı sona erdiğinde gibi yeni RDD bu aralığından tüm verileri içeren oluşturulur. RDDs sürekli kümesini DStream toplanır. Örneğin, toplu iş aralığı bir saniye uzun olması durumunda, DStream toplu sırasında bu ikinci alınan tüm verileri içeren ikinci her içeren bir RDD yayar. DStream işlerken sıcaklık olay bu toplu biri görüntülenir. Spark akış uygulama olayları içeren toplu işler ve sonuçta her RDD depolanan veriler üzerinde çalışır.

![Örnek DStream sıcaklık olayları ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Spark akış uygulama yapısı

Spark akış uygulama alma kaynaklardan veri aldığı, verileri işlemek için dönüşümleri uygular ve verileri bir veya daha fazla hedeflere sonra iter uzun süre çalışan bir uygulamadır. Spark akış uygulama yapısını statik bölümü ve dinamik bir bölümü vardır. Statik bölümü işlenmesi, verileri yapmak için hangi verilerin geldiği durumlarda ve burada tanımlar sonuçları tamamlamalıdır. Dinamik bölümü süresiz olarak, bir durdurma sinyali için bekleyen uygulama çalışıyor.

Örneğin, aşağıdaki basit uygulama metin satırının üzerinde bir TCP yuvasını alır ve her sözcüğün görünür sayısını sayar.

### <a name="define-the-application"></a>Uygulama tanımlama

Uygulama mantığı tanımı dört adım vardır:

1. Bir StreamingContext oluşturun.
2. Bir DStream StreamingContext oluşturun.
3. Dönüşümleri DStream uygulayın.
4. Sonuçları çıktı.

Bu statik tanımıdır ve uygulama çalıştırana kadar veri işlenir.

#### <a name="create-a-streamingcontext"></a>Bir StreamingContext oluşturma

Bir StreamingContext kümenize işaret SparkContext oluşturun. Bir StreamingContext oluştururken, toplu iş boyutu saniye cinsinden, örneğin belirtin:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Bir DStream oluşturma

StreamingContext örneğiyle giriş kaynağınız için bir giriş DStream oluşturun. Bu durumda, uygulama için Hdınsight kümesine bağlı varsayılan depolama yeni dosyalar görünümünü izliyor.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Dönüştürmeleri

DStream dönüşümleri uygulama tarafından işlenmesi uygulayın. Bu uygulama tek satırlık metin dosyasından bir zaman alır, her satırın sözcüklere böler ve her sözcüğün görünür kez sayısı için bir map-reduce desen kullanır.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Çıktı sonuçları

Dönüşüm sonuçları çıktı işlemleri uygulayarak hedef sistemleri itme. Bu durumda, her çalıştırmayı hesaplama sonucu konsol çıktısı yazdırılır.

    wordCounts.print()

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Akış uygulama başlatma ve sonlandırma sinyali alınana kadar çalıştırın.

    ssc.start()            
    ssc.awaitTermination()

Olay kaynakları, dönüşümler ve destekliyorsa, çıkış işlemlerinin yanı sıra Spark akış API hakkında ayrıntılı bilgi için bkz: [Spark akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Aşağıdaki örnek uygulama içinde çalıştırabilmeniz için kendi içinde bulunan bir [Jupyter not defteri](apache-spark-jupyter-notebook-kernels.md). Bu örnek, beş saniyede bir sayaç ve milisaniye olarak geçerli süre değeri çıkarır DummySource sınıfında sahte veri kaynağı oluşturur. Yeni bir StreamingContext nesnesi 30 saniyelik bir toplu iş aralığı vardır. Bir toplu iş her oluşturulduğunda akış uygulama üretilen RDD inceler, bir Spark DataFrame RDD dönüştürür ve DataFrame üzerinde geçici bir tablo oluşturur.

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

Örneğin bu SQL sorgusunu kullanarak, düzenli aralıklarla toplu işinde var olan değerleri geçerli kümesini görmek için DataFrame sonra sorgulayabilirsiniz:

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

DummySource 5 saniyede bir değer oluşturur ve uygulama her 30 saniyede bir toplu yayar altı değerleri vardır.

## <a name="sliding-windows"></a>Kayan pencere

Örneğin son iki saniye içinde bir ortalama sıcaklık almak bazı süre boyunca, DStream üzerinde toplama hesaplamalar gerçekleştirmek için kullanabileceğiniz *kayan pencere* Spark akış ile dahil işlemleri. Bir kayan bir pencere bir süresi olan (penceresi uzunluğu) ve Pencere içeriklerini sırasında olduğu aralığı (slayt aralığı) değerlendirilir.

Windows kayan üst üste, örneğin, bir saniyede slayt iki saniye uzunluğa sahip bir pencere tanımlayabilirsiniz. Bunun anlamı, bir toplama hesaplamayı her zaman son bir saniye önceki penceresinin verilerden yanı sıra yeni veriler ileri bir ikinci pencere içerecektir.

![Sıcaklık olaylarla ilk penceresi örneği](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Kayan sonra sıcaklık olaylarla örnek penceresi](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Aşağıdaki örnek, bir dakikalık süre ve bir dakikalık slayt penceresine toplu toplamak için DummySource kullanan kodu güncelleştirir.

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

İlk dakika sonra 12 girişleri - her iki toplu işlemlerin penceresinde toplanan altı girişlerinden vardır.

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

Spark akış API'kayan pencere işlevleri penceresi, countByWindow, reduceByWindow ve countByValueAndWindow içerir. Bu işlevler hakkında daha fazla bilgi için bkz: [DStreams dönüşümler](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Denetim noktası oluşturma

Dayanıklılık ve hataya dayanıklılık sağlamak üzere akış işleme düğümü hataları karşısında bile kesintisiz devam edebilmesi için denetim noktası oluşturma Spark akış seçeneğini kullanır. Hdınsight'ta Spark kontrol noktalarını kalıcı depolama birimine (Azure Storage veya Data Lake Store) oluşturur. Bu kontrol noktalarının yapılandırması, uygulama ve sıraya alınan ancak henüz işlenmemiş yığını tarafından tanımlanan işlemleri gibi akış uygulama hakkındaki meta verileri depolar. Bazı durumlarda, verileri daha hızlı bir şekilde Spark tarafından yönetilen RDDs mevcut nedir verileri durumunu yeniden oluşturmak için RDDs kaydetme kontrol noktalarını bir de dahil edilir.

## <a name="deploying-spark-streaming-applications"></a>Spark akış uygulamaları dağıtma

Genellikle bir Spark akış uygulamasına yerel olarak JAR dosyasını oluşturur ve bunu hdınsight'ta Spark Hdınsight kümenize bağlı varsayılan depolama JAR dosyasına kopyalayarak dağıtın. Uygulamanızı bir POST işlemini kullanarak kümenizden LIVY REST API'leri kullanılabilir başlatabilirsiniz. POSTANIN gövdesi, ana yöntem tanımlar ve akış uygulama ve isteğe bağlı olarak (örneğin, yürütücüler, bellek ve çekirdek sayısı) iş kaynak gereksinimlerini çalışır sınıfın adını, JAR yol sağlayan bir JSON belgesi içerir , ve uygulama kodunuz herhangi bir yapılandırma ayarı gerektirir.

![Spark akış uygulamasını dağıtma](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumunu da LIVY uç noktası bir GET isteği ile denetlenebilir. Son olarak, çalışan bir uygulama LIVY uç noktası bir silme isteği göndererek sonlandırabilir. LIVY API'si hakkında daha fazla bilgi için bkz: [LIVY uzak işleriyle](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight'ta bir Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Spark işlerinin LIVY ile uzaktan başlatma](apache-spark-livy-rest-interface.md)
