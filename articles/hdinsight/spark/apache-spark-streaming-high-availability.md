---
title: YARN - Azure HDInsight yüksek oranda kullanılabilir Spark akış işleri oluşturma
description: Nasıl Spark akışı bir yüksek kullanılabilirlik senaryo için ayarlanır.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/26/2018
ms.openlocfilehash: 1d9a7caa7ab70ef1f0da41e1ec3f30780f93536a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60536959"
---
# <a name="create-high-availability-apache-spark-streaming-jobs-with-yarn"></a>YARN ile yüksek kullanılabilirlik Apache Spark akış işleri oluşturma

[Apache Spark](https://spark.apache.org/) akış işleme veri akışları için ölçeklenebilir, yüksek performanslı, hataya dayanıklı uygulamalar uygulama olanak sağlar. Bir HDInsight Spark kümesi üzerinde Spark akış uygulamaları Azure Event Hubs, Azure IOT Hub gibi veri kaynaklarının çeşitli bağlanabilirsiniz [Apache Kafka](https://kafka.apache.org/), [Apache Flume](https://flume.apache.org/), Twitter, [ ZeroMQ](http://zeromq.org/), ham TCP yuvaları veya izleme [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) değişiklikler için dosya sistemi. Spark akış, herhangi bir belirli olay bile bir düğüm hatası ile tam bir kez işlenir Garantisi ile hata toleransı destekler.

Spark akışı, uzun süre çalışan işler aşamasında, dönüştürmeleri için veri ve dosya sistemleri, veritabanları, panolar ve konsol için sonuçları ardından dışına oluşturur. Spark akışı, ilk olarak tanımlanan zaman aralığı içinde toplu olayları toplayarak micro-verileri toplu işler. Ardından, toplu işleme ve çıkış için gönderilir. Batch zaman aralıkları genellikle ikinci bölümleri içinde tanımlanır.

![Spark akış](./media/apache-spark-streaming-high-availability/spark-streaming.png)

## <a name="dstreams"></a>DStreams

Spark akış verileri kullanarak, sürekli bir akış temsil eden bir *ayrılmış stream* (DStream). Event Hubs veya Kafka gibi veya başka bir DStream üzerinde dönüşümler uygulayarak giriş kaynaklarından bu DStream oluşturulabilir. Bir olay Spark akışı uygulamanızı geldiğinde olayı güvenilir bir biçimde depolanır. Diğer bir deyişle, böylece birden fazla düğüme sahip bir kopyasını olay verileri çoğaltılır. Bu, herhangi tek bir düğümün başarısız olay kaybına neden olmayan sağlar.

Spark core kullanan *dayanıklı Dağıtılmış veri kümeleri* (Rdd). Rdd, burada her düğüm genellikle verilerini tamamen bellek içi en iyi performansı korur kümedeki birden fazla düğümde verileri dağıtın. Her RDD bir toplu iş aralığı içinde toplanan olayları temsil eder. Spark Streaming, toplu iş aralığı sona erdiğinde, bu aralıktaki tüm verileri içeren yeni bir RDD üretir. Bu sürekli Rdd kümesi bir DStream toplanır. Uygulama Spark akışı, her toplu işin RDD içinde depolanan verileri işler.

![Spark DStream](./media/apache-spark-streaming-high-availability/DStream.png)

## <a name="spark-structured-streaming-jobs"></a>Spark yapılandırılmış akış işleri

Spark yapılandırılmış akış Spark 2.0 akış yapılandırılmış verileri kullanmak için bir analiz motoru olarak sunulmuştur. Spark yapılandırılmış akışı altyapısı API'leri toplu işleme SparkSQL kullanır. Spark yapılandırılmış akışı ile Spark Streaming gibi kendi hesaplamalar sürekli olarak gelen micro-toplu işler üzerinde veri olarak çalışır. Spark yapılandırılmış akışı olarak sınırsız sayıda satır içeren bir giriş tablo veri akışı temsil eder. Diğer bir deyişle, giriş tablosu yeni veriler ulaştıkça devam etmektedir. Bu girdi tablosu, uzun süre çalışan bir sorgu tarafından sürekli olarak işlenir ve bir çıktı tablosu için sonuçları yazılır.

![Spark yapılandırılmış akış](./media/apache-spark-streaming-high-availability/structured-streaming.png)

Yapılandırılmış akış, verileri bir anda sistemin ulaşır ve hemen giriş tablosu içine alınır. Bu giriş tablosu karşılayacağımızı sorguları yazmanız. Sorgu çıktıyı sonuçlar tablosu adlı başka bir tablo oluşturur. Sonuçlar tablosu, dış bir veri deposuna gönderilecek verileri çizim Sorgunuzun sonuçlarını içeren ilişkisel bir veritabanı. *Tetikleyici aralığı* ne zaman giriş tablosundaki verilerin işlenmesi için zamanlama ayarlar. Bunu ulaşır ulaşmaz varsayılan olarak, yapılandırılmış akış verileri işler. Ancak, akış verilerini zaman tabanlı toplu olarak işlenir böylece, uzun bir aralıkta, çalıştırılacak tetikleyici yapılandırabilirsiniz. Sonuçlar tablosunda veri akış sorgu başlamasından bu yana tüm çıktı verilerini içerir, böylece yeni veri olduğu her zaman tamamen yenilenmiş olabilir (*tam modda*), ya da yalnızca en son yenilikler veri içerebilir Sorgu işlendiği zaman (*ekleme modunda açıldıysa*).

## <a name="create-fault-tolerant-spark-streaming-jobs"></a>Hataya dayanıklı Spark akış işleri oluşturma

Spark Streaming işleriniz için yüksek oranda kullanılabilir bir ortam oluşturmak için kurtarma bireysel işlerle hatası durumunda kodlama yaparak başlayın. Hataya dayanıklı Self kurtarma gibi işler.

Rdd, hataya dayanıklı ve yüksek oranda kullanılabilir Spark akış işleri yardımcı çeşitli özelliklere sahiptir:

* Toplu bir DStream Rdd içinde depolanan girdi verilerini, bellekteki hataya dayanıklılık için otomatik olarak çoğaltılır.
* Çalışan düğümleri kullanılabilir olduğu sürece veri çalışan hatası nedeniyle kaybolması farklı çalışanlarında çoğaltılmış giriş verilerinden değeri yeniden.
* Hızlı hata kurtarma, hesaplama, bellekte aracılığıyla hataları/sona kalanlar kurtarma olduğu sürece bir saniye içinde ortaya çıkabilir.

### <a name="exactly-once-semantics-with-spark-streaming"></a>Tam olarak-Spark akışı ile bir kez semantiği

Her olay bir kez (ve yalnızca bir kez) işleyen bir uygulama oluşturmak için göz önünde bulundurun hata tüm sistemin noktaları bir sorun atandıktan sonra yeniden başlatma ve veri kaybını önlemek nasıl. Tam olarak-sonra hiçbir veri herhangi bir noktada kaybolur ve bu ileti işleme hatanın oluştuğu bağımsız olarak yeniden başlatılabilir, semantiği gerektirir. Bkz: [oluşturma Spark akış işleri ile tam olarak-kere olay işleme](apache-spark-streaming-exactly-once.md).

## <a name="spark-streaming-and-apache-hadoop-yarn"></a>Spark akış ve Apache Hadoop YARN

HDInsight küme iş tarafından denetlenen *henüz başka bir Resource Negotiator* (YARN). Spark akış için yüksek kullanılabilirlik tasarımı teknikleri için Spark akışı ve YARN bileşenleri içerir.  YARN'ı kullanarak bir örnek yapılandırması aşağıda gösterilmiştir. 

![YARN mimarisi](./media/apache-spark-streaming-high-availability/yarn-arch.png)

Aşağıdaki bölümlerde, bu yapılandırma için tasarım konuları açıklanmaktadır.

### <a name="plan-for-failures"></a>Hataları planlama

Yüksek kullanılabilirlik için YARN bir yapılandırma oluşturmak için olası bir yürütücü veya sürücü hatası planlamanız gerekir. Bazı Spark akış işleri, ek yapılandırma ve Kurulum gereken veri garantisi gereksinimleri de içerir. Örneğin, bir akış uygulaması barındırma akış sistem veya HDInsight küme içinde oluşan hataları rağmen garanti iş gereksinimleri için sıfır-data-kaybı olabilir.

Varsa bir **Yürütücü** başarısız, görevleri ve alıcıları yeniden başlatılana Spark tarafından otomatik olarak, gerekli herhangi bir yapılandırma değişikliği olması.

Ancak, bir **sürücü** başarısız olursa, tüm kendi ilişkili Yürütücü başarısız ve alınan tüm bloklar ve sonuçlarını kaybolur. Bir sürücü hatadan kurtarmak için *DStream denetim noktası* açıklandığı [oluşturma Spark akış işleri ile tam olarak-kere olay işleme](apache-spark-streaming-exactly-once.md#use-checkpoints-for-drivers). Düzenli aralıklarla DStream denetim noktası kaydeder *yönlendirilmiş Çevrimsiz grafik* (DAG), Azure depolama gibi hataya dayanıklı depolama DStreams.  Denetim noktası oluşturma, denetim noktası bilgilerini başarısız sürücüsünden yeniden başlatmak Spark yapılandırılmış akışı sağlar.  Bu sürücü yeniden başlatma yeni yürütücüler başlatır ve ayrıca alıcılar yeniden başlatır.

DStream denetim noktası sürücüleriyle kurtarmak için:

* Sürücü otomatik yeniden başlatma YARN üzerinde yapılandırma ayarıyla yapılandırın. `yarn.resourcemanager.am.max-attempts`.
* Bir denetim noktası dizini HDFS ile uyumlu bir dosya sistemi ile ayarlanan `streamingContext.checkpoint(hdfsDirectory)`.
* Örneğin kontrol noktaları, kurtarma için kullanılacak kaynak kodu yeniden yapılandırma:

    ```scala
        def creatingFunc() : StreamingContext = {
            val context = new StreamingContext(...)
            val lines = KafkaUtils.createStream(...)
            val words = lines.flatMap(...)
            ...
            context.checkpoint(hdfsDir)
        }

        val context = StreamingContext.getOrCreate(hdfsDir, creatingFunc)
        context.start()
    ```

* İle yazma önceden yazılan günlük (WAL) etkinleştirerek kayıp veri kurtarma yapılandırma `sparkConf.set("spark.streaming.receiver.writeAheadLog.enable","true")`ve bellek içi çoğaltma ile giriş DStreams için devre dışı bırakma `StorageLevel.MEMORY_AND_DISK_SER`.

Özetlemek gerekirse, kullanarak denetim noktası oluşturma, WAL + Güvenilir Alıcılar, "en az bir kez" verileri kurtarma sunmak mümkün olacaktır:

* Tam olarak bir kez, alınan verilere kaybolmuş değil ve çıkışları eşgüçlüdür sürece veya işlem.
* Tam bir kez ile Kafka doğrudan yeni yaklaşıma Kafka çoğaltılmış günlük alıcılar veya WALs kullanmak yerine kullanır.

### <a name="typical-concerns-for-high-availability"></a>Yüksek kullanılabilirlik için tipik sorunları

* Akış işi daha toplu işleri izlemek daha zordur. Spark akış işleri genellikle uzun süre çalışan ve bir iş tamamlanana kadar YARN günlükleri toplama gerçekleştirmez.  Uygulama veya Spark yükseltme işlemleri sırasında Spark kontrol noktaları kaybedilir ve yükseltme sırasında denetim noktası dizini temizlemeniz gerekir.

* Bir istemci başarısız olsa bile sürücüleri çalıştırmak için YARN küme modunu yapılandırın. Sürücüleri otomatik olarak yeniden başlatmayı ayarlamak için:

    ```
    spark.yarn.maxAppAttempts = 2
    spark.yarn.am.attemptFailuresValidityInterval=1h
    ```

* Spark ve Spark akış UI yapılandırılabilir Ölçüm sistemi vardır. Pano ölçümleri 'işlenen numarası kayıtları gibi' indirmek için Grafit/Grafana, ' Bellek/GC kullanımı' sürücü & yürütücüler gibi ek kitaplıklar da kullanabilirsiniz 'toplam gecikme', 'küme kullanımı' ve benzeri. Kullanabileceğiniz yapılandırılmış akış sürüm 2.1 veya daha büyük, `StreamingQueryListener` ek ölçümleri toplamak için.

* Uzun süre çalışan işler segmentlere.  Uygulama Spark akışı kümeye gönderildiğinde, iş çalıştığı YARN kuyruk tanımlanması gerekir. Kullanabileceğiniz bir [YARN kapasite Planlayıcısı](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) kuyrukları ayırmak için uzun süre çalışan işleri göndermek için.

* Akış uygulamanızın düzgün bir şekilde kapatıldı. Uzaklıkları, bilinen ve harici olarak depolanan tüm uygulama durumu, akış uygulamanızı ilgili yerde programlı olarak durdurabilirsiniz. Bir tekniktir "hooks Spark, kontrol ederek bir dış bayrağı için iş parçacığı" kullanılacak her *n* saniye. Ayrıca bir *işaretçi dosyası* , uygulama başlatılırken HDFS üzerinde oluşturulan, ardından durdurmak istediğinizde kaldırıldı. İşaretçi dosyası yaklaşım için ayrı bir iş parçacığı Spark uygulamanızdaki kod şuna benzer çağırır kullanın:

    ```scala
    streamingContext.stop(stopSparkContext = true, stopGracefully = true)
    // to be able to recover on restart, store all offsets in an external database
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark akış genel bakış](apache-spark-streaming-overview.md)
* [Apache Spark akış işleri ile tam olarak oluşturma-kere olay işleme](apache-spark-streaming-exactly-once.md)
* [Uzun süre çalışan Apache akış işleri YARN üzerinde Spark](https://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/) 
* [Yapılandırılmış akış: Hataya dayanıklı semantiği](https://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html#fault-tolerance-semantics)
* [Ayrılmış akışları: Hataya dayanıklı bir Model için ölçeklenebilir Stream işleme](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
