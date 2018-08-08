---
title: Akış Azure HDInsight Spark
description: HDInsight Spark kümelerinde Spark akış uygulamaları kullanma
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: b3420737147f9ee67d5d2d021c28a98d34e209df
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617624"
---
# <a name="overview-of-spark-streaming"></a>Spark akış genel bakış

Spark akışı veri akışı işleme HDInsight Spark kümeleri, bir düğüm arıza durumunda bile herhangi bir giriş olayı tam bir kez işlenir garantisi sağlar. Spark Stream giriş veri kaynakları, Azure Event Hubs, bir Azure IOT Hub, Kafka, Flume, Twitter, ZeroMQ, ham TCP yuvaları dahil olmak üzere çok çeşitli veya HDFS dosya sistemleri izleme alan bir uzun süre çalışan iş. Yalnızca olay temelli bir işlemin aksine bir Spark Stream giriş verisi gibi bir 2 saniyelik dilim zaman pencereleri halinde toplu işlemleri ve ardından her toplu işin eşlemesini kullanarak veri dönüşümleri, azaltmak, katılın ve işlemleri ayıklayın. Spark Stream ardından dönüştürülmüş verileri için dosya sistemleri, veritabanları, panolar ve konsol yazıyor.

![Stream, HDInsight ve akış Spark ile işleme](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Spark akış uygulamaları bir saniyenin her toplamak için beklemesi gereken *micro-batch* toplu işleme için gönderme önce olay. Buna karşılık, olay temelli bir uygulamayı hemen her olay işler. Spark Streaming gecikme süresi genellikle birkaç altında saniyedir. Mikro toplu yaklaşımın avantajları şunlardır: daha verimli veri işleme ve daha basit toplama hesaplamalar.

## <a name="introducing-the-dstream"></a>DStream ile tanışın

Spark akışı gelen verileri kullanarak, sürekli bir akış temsil eden bir *ayrılmış stream* bir DStream çağrılır. Giriş kaynaklarından olay hub'ları veya Kafka gibi veya başka bir DStream üzerinde dönüşümler uygulayarak bir DStream oluşturulabilir.

Bir DStream bir ham olay verilerinizi en üstünde bir soyutlama katmanı sağlar. 

Tek bir olay ile Başlat, bağlı bir thermostat okurken bir sıcaklık varsayalım. Bu olay Spark akışı uygulamanızı geldiğinde olay birden çok düğümde burada çoğaltılır, güvenilir bir şekilde depolanır. Herhangi bir tek düğüm hatası olay kaybına neden olmayan bu hataya dayanıklılık sağlar. Spark core, burada her düğüm, kendi verileri bellek içinde en iyi performans için genellikle tutar kümedeki birden fazla düğümde verileri dağıtan bir veri yapısı kullanır. Bu veri yapısını adlı bir *dayanıklı Dağıtılmış veri kümesi* (RDD).

Adlı bir kullanıcı tanımlı zaman çerçevesi içinde toplanan olayları her RDD temsil *toplu iş aralığı*. Her toplu iş aralığı sona erdiğinde gibi bu aralığın tüm verileri içeren yeni bir RDD oluşturulur. Rdd sürekli kümesi bir DStream toplanır. Örneğin, uzun bir ikinci toplu iş aralığı ise, DStream toplu, saniyede alınan tüm veriler içeren ikinci her içeren bir RDD yayar. DStream işlerken sıcaklık olay bu toplu birinde görünür. Uygulama Spark akışı, olayları içeren toplu işler ve sonuçta her RDD içinde depolanan veriler üzerinde çalışır.

![Örnek DStream sıcaklık olayları ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Uygulama Spark akışı yapısı

Uygulama Spark akışı alma kaynaklardan gelen verileri alır, veriyi işlemek için dönüşümleri uygular ve sonra verileri bir veya daha fazla hedefe gönderir uzun süre çalışan bir uygulamasıdır. Uygulama Spark akışı yapısını, statik bir bölümü ve dinamik bir bölümü vardır. Veriler, veri yapmak için hangi işleme gelir ve burada statik bölümünü tanımlayan sonuçları tamamlamalıdır. Dinamik bölümü, bir durdurma sinyali beklenirken uygulaması çalışıyor.

Örneğin, aşağıdaki basit uygulama, metin satırının üzerinde bir TCP yuva alır ve her bir sözcüğün kaç kez sayar.

### <a name="define-the-application"></a>Uygulama tanımlama

Mantıksal uygulama tanımı dört adım vardır:

1. Bir StreamingContext oluşturun.
2. Bir DStream StreamingContext oluşturun.
3. Dönüşümler DStream için geçerlidir.
4. Sonuçları gönderir.

Bu tanımı statiktir ve uygulamayı çalıştırana kadar hiçbir veriler işlenir.

#### <a name="create-a-streamingcontext"></a>Bir StreamingContext oluşturma

Bir StreamingContext kümenize işaret zamanda sparkcontext öğesini oluşturun. Bir StreamingContext oluştururken, toplu işin boyutunu saniye cinsinden, örneğin belirtin:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Bir DStream oluşturma

Giriş kaynağınız için bir giriş DStream StreamingContext örneği ile oluşturun. Bu durumda, uygulama için bağlı bir HDInsight kümesi için varsayılan depolama alanı yeni dosyaları görünümünü izliyor.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Dönüşüm Uygulama

İşleme, dönüşümler üzerinde DStream uygulayarak uygulayın. Bu uygulama, tek satırlık bir metin dosyasından bir zaman alır, her satırı içinde sözcükleri böler ve sonra her bir sözcüğün kaç kez sayısını map-reduce deseni kullanır.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Çıktı sonuçları

Dönüşüm sonuçları çıkış işlemleri uygulayarak hedef sistemleri için anında. Bu durumda, hesaplama ile her çalıştırma sonucunu konsol çıkışında yazdırılır.

    wordCounts.print()

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Akış uygulama başlatma ve sonlandırma sinyal alınana kadar çalıştırın.

    ssc.start()            
    ssc.awaitTermination()

Spark Stream API ile olay kaynakları, dönüşümler ve çıkış işlemleri desteklediği hakkında ayrıntılı bilgi için bkz. [Spark akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Aşağıdaki örnek uygulama içinde çalıştırabilmeniz için kendi içinde bir [Jupyter not defteri](apache-spark-jupyter-notebook-kernels.md). Bu örnekte beş saniyede bir sayaç ve milisaniye olarak geçerli süre değerini çıkarır DummySource sınıfında sahte veri kaynağı oluşturur. Yeni bir StreamingContext nesnesi, bir toplu iş aralığı 30 saniye sahiptir. Her bir toplu işi oluşturulduğunda akış uygulaması üretilen RDD inceler, bir Spark DataFrame için RDD dönüştürür ve DataFrame üzerinde geçici bir tablo oluşturur.

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

Ardından, düzenli aralıklarla toplu işinde var olan değerleri geçerli kümesini görmek için veri çerçevesi Örneğin bu SQL sorgusunu kullanarak da sorgulama yapabilirsiniz:

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

DummySource 5 saniyede bir değer oluşturur ve uygulama, her 30 saniyede bir toplu iş yayar altı değerleri vardır.

## <a name="sliding-windows"></a>Kayan windows

Örneğin, son iki saniye içinde bir ortalama sıcaklık almak bazı süre boyunca, DStream üzerinde toplama hesaplamalar gerçekleştirmek için kullanabileceğiniz *kayan pencere* Spark akışı ile dahil edilen işlemler. Bir süre bir kayan pencereye sahip (Pencere uzunluğunun) ve pencerenin içeriği olan aralığı (slayt aralığı) değerlendirilir.

Windows kaydırma binebilir, örneğin, saniyede bir slayt iki saniye olarak uzunluğunu içeren bir pencere tanımlayabilirsiniz. Bunun anlamı, her zaman bir toplama hesaplamayı pencerenin önceki penceresinin son bir saniye verilerden yanı sıra bir sonraki ikinci yeni veriler içerir.

![Sıcaklık olaylarla ilk penceresi örneği](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Örnek penceresiyle kayan sonra sıcaklık olayları](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Aşağıdaki örnek, bir dakikalık süre ve bir dakikada slayt içeren bir pencere toplu işlerin toplanması için DummySource kullanan kodu güncelleştirir.

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

İlk dakikanın, 12 giriş - her iki toplu işlerin penceresinde toplanan altı girişler vardır.

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

Spark akış API'de kayan pencere işlevleri penceresi, countByWindow reduceByWindow ve countByValueAndWindow içerir. Bu işlevler hakkında daha fazla bilgi için bkz: [DStreams dönüşümler](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Denetim noktası oluşturma

Dayanıklılık ve hataya dayanıklılık sağlamak üzere akış işleme düğüm hataları karşılaşıldığında bile kesintiye uğramadan devam edebileceğinizden emin olmak için denetim noktası Spark Streaming seçeneğini kullanır. HDInsight Spark kontrol noktalarını kalıcı depolama (Azure Depolama'da veya Data Lake Store) oluşturur. Bu kontrol noktalarının yapılandırması, uygulama ve sıraya alınmış ancak henüz işlenmemiş toplu işlemler tarafından tanımlanan işlemleri gibi akış uygulaması hakkındaki meta verileri depolar. Bazı durumlarda, verileri daha hızlı bir şekilde mevcut Rdd Spark tarafından yönetilen nedir verileri durumunu yeniden Rdd kaydetme kontrol noktalarını de içerecektir.

## <a name="deploying-spark-streaming-applications"></a>Spark akış uygulamaları dağıtma

Genellikle bir JAR dosyasına yerel olarak Spark akışı bir uygulama oluşturun ve ardından HDInsight üzerinde Spark HDInsight kümenize bağlı varsayılan depolama alanı için JAR dosyasını kopyalayarak dağıtın. Uygulamanızı bir POST işlemi'ni kullanarak kümenizi LIVY REST API'ler kullanılabilir başlayabilirsiniz. Gönderinin gövdesi olan ana yöntem tanımlar ve akış uygulaması ve isteğe bağlı olarak kaynak gereksinimlerini (örneğin, yürütücüler, bellek ve çekirdek sayısı) işin çalışan sınıfın adı, JAR yol sağlayan bir JSON belgesini içerir. , ve uygulama kodunuzun herhangi bir yapılandırma ayarı gerektirir.

![Bir Spark akışı uygulamasını dağıtma](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumunu, ayrıca bir LIVY uç noktasına karşı bir GET isteğiyle denetlenebilir. Son olarak, LIVY uç nokta karşı bir silme isteği göndererek çalışan bir uygulamayı sonlandırabilirsiniz. LIVY API hakkında daha fazla bilgi için bkz: [LIVY ile uzak işler](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Spark işlerinde LIVY ile uzaktan başlatma](apache-spark-livy-rest-interface.md)
