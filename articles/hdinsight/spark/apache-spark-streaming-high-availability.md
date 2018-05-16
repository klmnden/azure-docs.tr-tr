---
title: Yüksek oranda kullanılabilir Spark akış işleri YARN - Azure Hdınsight oluşturma | Microsoft Docs
description: Spark akış bir yüksek kullanılabilirlik senaryo için nasıl kurulur.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ramoha
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: ramoha
ms.openlocfilehash: bbb4da02cbe4b0685c715c4cd6bd7b15c6b5cce0
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="create-high-availability-spark-streaming-jobs-with-yarn"></a>Yüksek kullanılabilirliği Spark akış işleri YARN ile oluşturma

Spark akış işleme veri akışları için ölçeklenebilir, yüksek verimlilik, hataya dayanıklı uygulamalarını uygulamak etkinleştirir. Çeşitli veri kaynakları, Azure Event Hubs, Azure IOT Hub, Kafka, Flume, Twitter, ZeroMQ, ham TCP yuvaları gibi ya da HDFS filesystem değişiklikleri izleme Hdınsight Spark kümesi üzerinde Spark akış uygulamaları bağlanabilir. Spark akış herhangi bir belirli olay bile bir düğüm hatası ile tam olarak bir kez işlenir Garantisi ile hata toleransını destekler.

Spark akış sırasında veri dönüşümleri uygulayın ve ardından sonuçları bağlanan dosya sistemlerinin, veritabanları, panolar ve konsol gönderme mümkün olduğu uzun süre çalışan işleri oluşturur. Spark akış, ilk olarak tanımlı bir zaman aralığı içinde olayların toplu toplayarak veri mikro toplu işler. Ardından, toplu işleme ve çıktı için gönderilir. Toplu iş zaman aralıkları genellikle saniyenin kesirler tanımlanır.

![Spark akış](./media/apache-spark-streaming-high-availability/spark-streaming.png)

## <a name="dstreams"></a>DStreams

Spark akış verileri kullanarak, sürekli akışı temsil eden bir *ayrılmış akış* (DStream). Bu DStream giriş kaynaklarından olay hub'ları veya Kafka gibi ya da başka bir DStream dönüşümleri uygulama tarafından oluşturulabilir. Bir olay Spark akış uygulamanızı geldiğinde olay güvenilir bir biçimde depolanır. Diğer bir deyişle, böylece birden çok düğüm bir kopyasını olay verileri çoğaltılır. Bu, tek bir düğüm hatası olayınızın kaybına neden değil sağlar.

Spark core kullanan *dayanıklı Dağıtılmış veri kümeleri* (RDDs). RDDs, burada her düğüm genellikle kendi veri tamamen bellek içi en iyi performansı korur kümedeki birden çok düğüm arasında veri dağıtın. Her RDD bir toplu iş aralığı içinde toplanan olayları temsil eder. Toplu iş aralığı sona erdiğinde, Spark akış bu aralığa tüm verileri içeren yeni bir RDD üretir. RDDs sürekli bu kümesini DStream toplanır. Spark akış uygulama her toplu iş RDD içinde depolanan verileri işler.

![Spark DStream](./media/apache-spark-streaming-high-availability/DStream.png)

## <a name="spark-structured-streaming-jobs"></a>Spark yapılandırılmış akış işleri

Spark yapılandırılmış akış Spark 2. 0'analitik altyapının kullanılmak üzere yapılandırılmış veri akışı üzerinde olarak sunulmuştur. Yapılandırılmış Spark akış API'leri altyapısı toplu işleme SparkSQL kullanır. Spark yapılandırılmış akış ile Spark akış gibi kendi hesaplamalar sürekli ulaşan mikro-toplu veri olarak çalıştırır. Spark yapılandırılmış akış veri akışı sınırsız satırları içeren bir giriş tablosu olarak temsil eder. Diğer bir deyişle, Giriş tablosunda yeni veri ulaştığında büyümeye devam eder. Bu giriş tablosu sürekli uzun süre çalışan bir sorgu tarafından işlenir ve bir çıkış tablosu için sonuçları yazılır.

![Spark akış yapılandırılmış](./media/apache-spark-streaming-high-availability/structured-streaming.png)

Yapılandırılmış akış, veri sistemi ulaştığında ve giriş tabloya hemen alınan. Bu giriş tablosu karşı işlemleri sorgular yazarsınız. Sorgu çıktısı sonuçlar tablosunu adlı başka bir tablo üretir. Sonuçlar tablosunu içinden dış bir veri deposu için gönderilecek veriler çizim Sorgunuzun sonuçlarını içeren bu tür bir ilişkisel veritabanı. *Tetikleyici aralığı* girdi tablosundan işlenen verilerini ne zaman için zamanlama ayarlar. Bunu ulaşır ulaşmaz varsayılan olarak, yapılandırılmış akış verileri işler. Ancak, veri akışı zamana dayalı toplu olarak işlenen şekilde daha uzun bir aralıkta çalıştırmak için tetikleyici yapılandırabilirsiniz. Sonuçlar tablosunda veri akış sorgu başlamasından bu yana tüm çıktı verileri içeren yeni veri olduğundan her zaman tamamen yenilenmesi (*tam modu*), ya da yalnızca en son yeni verileri yalnızca içerebilir Sorgu işlendiği saati (*ekleme modu*).

## <a name="create-fault-tolerant-spark-streaming-jobs"></a>Hataya dayanıklı Spark akış işleri oluşturma

Spark akış işleriniz için yüksek oranda kullanılabilir bir ortam oluşturmak için kurtarma için tek tek işleriniz hatası durumunda kodlayarak başlatın. Bu tür Self kurtarma işleri hataya dayanıklı.

RDDs yüksek oranda kullanılabilir ve hataya dayanıklı Spark akış işi yardımcı çeşitli özelliklere sahiptir:

* Toplu DStream RDDs içinde depolanan giriş verileri otomatik olarak hataya dayanıklılık için bellekte çoğaltılır.
* Alt düğümleri kullanılabilir olduğu sürece çalışan hata nedeniyle kaybolduğunda veri farklı çalışanları üzerinde çoğaltılmış giriş verilerinden yeniden.
* Hesaplama bellekte aracılığıyla hataları/sona kalanlar kurtarma olur gibi hızlı arıza Kurtarma bir saniye içinde oluşabilir.

### <a name="exactly-once-semantics-with-spark-streaming"></a>Tam olarak-kez semantiği Spark akış ile

Her olay bir kez (ve yalnızca bir kez) işleyen bir uygulama oluşturmak için göz önünde bulundurun hata tüm sistem noktaları bir sorun yaşıyor sonra yeniden başlatma ve veri kaybını önlemek nasıl. Tam olarak-herhangi bir noktada veri kaybolur ve bu ileti işlenirken hata oluştuğu bağımsız olarak yeniden başlatılabilir, semantiği gerektiren sonra. Bkz: [Spark akış oluşturma işleri ile tam olarak-kez olay işleme](apache-spark-streaming-exactly-once.md).

## <a name="spark-streaming-and-yarn"></a>Spark akış ve YARN

Hdınsight'ta, küme iş tarafından denetlenen *henüz başka bir kaynak Uzlaştırıcı* (YARN). Spark akış için yüksek kullanılabilirlik tasarımı teknikleri Spark akış ve YARN bileşenleri içerir.  YARN kullanarak örnek bir yapılandırma aşağıda verilmiştir. 

![YARN mimarisi](./media/apache-spark-streaming-high-availability/yarn-arch.png)

Aşağıdaki bölümlerde bu yapılandırma için tasarım konuları açıklanmaktadır.

### <a name="plan-for-failures"></a>Hataları planlama

Yüksek kullanılabilirlik için YARN bir yapılandırma oluşturmak için olası bir yürütücü veya sürücü hatası planlamanız gerekir. Bazı Spark akış işleri ek yapılandırma ve Kurulum gereken veri garantisi gereksinimleri de içerir. Örneğin, bir akış uygulaması barındırma akış sistem ya da Hdınsight kümesi oluşan hataları rağmen garanti bir iş gereksinimi için sıfır-data-kaybı olabilir.

Varsa bir **Yürütücü** başarısız, görevler ve alıcıları yeniden başlatılana Spark tarafından otomatik olarak, gerekli herhangi bir yapılandırma değişikliği gelir.

Ancak, bir **sürücü** başarısız, tüm ilişkili yürütücüler başarısız ve alınan tüm blokları ve hesaplama sonuçları kaybolur. Bir sürücü hatadan kurtarmak için kullanmak *DStream denetim noktası oluşturma* açıklandığı gibi [Spark akış oluşturma işleri ile tam olarak-kez olay işleme](apache-spark-streaming-exactly-once.md#use-checkpoints-for-drivers). DStream denetim noktası oluşturma düzenli aralıklarla kaydeder *yönlendirilmiş Çevrimsiz grafik* (DAG), Azure depolama gibi hataya dayanıklı bir depolama birimine DStreams.  Denetim noktası oluşturma, Spark denetim noktası bilgi başarısız sürücüsünden yeniden başlatmak yapılandırılmış akış sağlar.  Bu sürücü yeniden başlatma yeni yürütücüler başlatır ve ayrıca alıcıları yeniden başlatır.

DStream denetim noktası oluşturma sürücüleriyle kurtarmak için:

* Otomatik sürücü yeniden YARN üzerinde yapılandırma yapılandırın `yarn.resourcemanager.am.max-attempts`.
* HDFS uyumlu dosya sistemi ile bir denetim noktası dizini kümesindeki `streamingContext.checkpoint(hdfsDirectory)`.
* Kaynak kodu denetim noktaları kurtarma için örneğin kullanacak şekilde yeniden yapılandırabilirsiniz:

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

* Veri kaybı Kurtarma ile yazma tamamlanan günlük (NİLEME) etkinleştirerek yapılandırmak `sparkConf.set("spark.streaming.receiver.writeAheadLog.enable","true")`ve bellek içi çoğaltma ile giriş DStreams için devre dışı bırakma `StorageLevel.MEMORY_AND_DISK_SER`.

Özetlemek için kullanarak denetim noktası oluşturma, WAL + Güvenilir Alıcılar, "en az bir kez" veri kurtarma teslim etmek kullanamazsınız:

* Tam olarak bir kez, alınan veriler kayıp değil ve çıkışları ya da ıdempotent olduğunuz sürece veya işlem.
* Tam olarak bir kez, yeni Kafka doğrudan yaklaşımda Kafka çoğaltılmış günlük alıcılar veya WALs kullanmak yerine kullanır.

### <a name="typical-concerns-for-high-availability"></a>Yüksek kullanılabilirlik için tipik sorunları

* Toplu işleri'den akış işi izlemek daha zordur. Spark akış işleri genellikle uzun süre çalışan ve bir işi tamamlanana kadar YARN günlüklerini toplama değil.  Uygulama veya Spark yükseltmeleri sırasında Spark kontrol noktaları kaybolur ve yükseltme sırasında denetim noktası dizini temizlemeniz gerekir.

* Bir istemci başarısız olsa bile sürücüleri çalıştırmak için YARN küme modunu yapılandırın. Sürücüleri otomatik olarak yeniden ayarlamak için:

    ```
    spark.yarn.maxAppAttempts = 2
    spark.yarn.am.attemptFailuresValidityInterval=1h
    ```

* Spark ve Spark akış UI yapılandırılabilir ölçümleri sistem sahiptir. 'İşlenen num kayıtları gibi' Pano ölçümleri indirmek için Grafit/Grafana, ' Bellek/GC kullanımı' sürücü & yürütücüler gibi ek kitaplıkları da kullanabilirsiniz 'toplam gecikme', 'küme kullanımı' vb. Kullanabileceğiniz yapılandırılmış akışında sürüm 2.1 veya büyük `StreamingQueryListener` ek ölçümleri toplamak için.

* Uzun süre çalışan işleri kesiminde.  Spark akış uygulama kümeye gönderildiğinde YARN sıranın iş çalıştığı tanımlanması gerekir. Kullanabileceğiniz bir [YARN kapasite Planlayıcısı](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) sıraları ayırmak için uzun süre çalışan işleri göndermek için.

* Akış uygulamanız düzgün biçimde kapatılamadı. Uzaklıkları bilinen ve tüm uygulama durumu harici olarak depolanan, akış uygulamanızı ilgili yerde program aracılığıyla durdurabilirsiniz. Bir tekniktir "kancaları Spark, denetleyerek bir dış bayrağı için iş parçacığı" kullanmak için her *n* saniye. Aynı zamanda bir *işaretçi dosyası* , uygulama başlatılırken HDFS üzerinde oluşturulan oluşuyorsa durdurmak istediğinizde kaldırıldı. Bir işaretçi dosyası yaklaşım için ayrı bir iş parçacığı Spark uygulamanızdaki kod bu benzer çağırır kullanın:

    ```scala
    streamingContext.stop(stopSparkContext = true, stopGracefully = true)
    // to be able to recover on restart, store all offsets in an external database
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış Spark akış](apache-spark-streaming-overview.md)
* [Spark akış işleri ile tam olarak oluşturma-kez olay işleme](apache-spark-streaming-exactly-once.md)
* [Uzun süre çalışan Spark akış işlerinin YARN üzerinde](http://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/) 
* [Yapılandırılmış akış: Hataya dayanıklı semantiği](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html#fault-tolerance-semantics)
* [Ayrılmış akışları: Hataya dayanıklı bir Model ölçeklenebilir akış işleme](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
