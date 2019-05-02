---
title: Spark akış işleri ile tam olarak oluşturma-kere olay işleme - Azure HDInsight
description: Spark akış yalnızca tek bir kez bir olayı işlemek için nasıl kurulur.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 388723624fde73899809b95ff8ae4ee23cf49a9d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705094"
---
# <a name="create-apache-spark-streaming-jobs-with-exactly-once-event-processing"></a>Apache Spark akış işleri ile tam olarak oluşturma-kere olay işleme

Stream işleme uygulamalarını nasıl bunlar yeniden işlem iletileri bazı hatadan sonra sistemde işlemek için farklı yaklaşımlara uygulayın:

* En az bir kez: Her ileti işlenmek üzere garanti edilmez, ancak birden çok kez işlendikten.
* En fazla bir kez: Her ileti olabilir veya işlenmemiş. Bir ileti işlediğinde, yalnızca bir kez işlenir.
* Tam bir kez: Her iletiyi yalnızca tek bir kez işlenmesini garanti edilir.

Bu makalede, tam olarak elde etmek için Spark akışı yapılandırma gösterir-bir kez işlenmesini.

## <a name="exactly-once-semantics-with-apache-spark-streaming"></a>Tam olarak-Apache Spark akışı ile bir kez semantiği

İlk olarak göz önünde bulundurun hata tüm sistemin noktaları bir sorun atandıktan sonra yeniden başlatma ve veri kaybını önlemek nasıl. Uygulama Spark akışı vardır:

* Bir giriş kaynağı.
* Veri çekme ise giriş kaynağından bir veya daha fazla alıcı işlemler.
* Veri işleme görevleri.
* Çıkış havuzu.
* Uzun süre çalışan iş yöneten bir sürücü işlemi.

Tam olarak-sonra hiçbir veri herhangi bir noktada kaybolur ve bu ileti işleme hatanın oluştuğu bağımsız olarak yeniden başlatılabilir, semantiği gerektirir.

### <a name="replayable-sources"></a>Yeniden oynatılabilir kaynakları

Spark Streaming uygulamanız, olaylarından okuma kaynağı olmalıdır *yeniden oynatılabilir*. Bu durumda, burada iletisi alındı, ancak iletinin kalıcı veya işlenen önce sistem başarısız oldu, kaynak aynı ileti yeniden sağlamanız gereken anlamına gelir.

Azure'da, hem Azure Event Hubs ve [Apache Kafka](https://kafka.apache.org/) HDInsight üzerinde yeniden oynatılabilir kaynakları sağlayın. Hataya dayanıklı dosya sistemi gibi başka bir örnek yeniden oynatılabilir kaynağı olan [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html), Azure depolama blobları veya Azure Data Lake Storage'ı seçin, burada tüm verileri her zaman tutulur ve herhangi bir noktada verilerin tamamına yeniden okuyabilirsiniz.

### <a name="reliable-receivers"></a>Güvenilir Alıcılar

Spark akış, olay hub'ları ve Kafka gibi kaynakları *Güvenilir Alıcılar*, burada her alıcı kaynak okuma, ilerleme durumunu izler. Güvenilir bir alıcı içine içinde ya da hataya dayanıklı depolama durumunu kalıcı [Apache ZooKeeper](https://zookeeper.apache.org/) veya Spark akışı denetim noktaları HDFS'ye yazılan. Böyle bir alıcı başarısız olursa ve daha sonra yeniden başlatıldı, bu kaldığı yerden devam edebiliyorduk.

### <a name="use-the-write-ahead-log"></a>Yazma önceden yazılan günlük kullanın

Spark akışı, bir yazma tamamlanan alınan her olayın burada ilk hataya dayanıklı depolama, Spark'ın denetim noktası dizinine yazılır ve sonra bir dayanıklı Dağıtılmış veri kümesi (RDD içinde) depolanan günlük kullanımını destekler. Azure'da, Azure depolama veya Azure Data Lake Depolama tarafından yedeklenen HDFS hataya dayanıklı depolama alanıdır. Spark Streaming uygulamanızda ayarlayarak yazma önceden yazılan günlük için tüm alıcıları etkin `spark.streaming.receiver.writeAheadLog.enable` yapılandırma ayarı `true`. Yazma önceden yazılan günlük hem sürücü hem de Yürütücü hataları için hata toleransı sağlar.

Olay verilerini karşı çalışan görevlerin çalışanları için hem çoğaltılacak ve birden fazla çalışana arasında dağıtılmış tanımı her RDD gereğidir. Kilitlenen çalışan alt görev olayı veri çoğaltmasını sahip başka bir çalışan üzerinde yeniden başlatılacak bir görev başarısız olur, böylece olay kaybolmaz.

### <a name="use-checkpoints-for-drivers"></a>Sürücüleri kontrol noktalarını kullanma

İş sürücüleri yeniden başlatılabilir olması gerekir. Spark Streaming uygulamanızı çalıştıran sürücünün kilitlenmesi durumunda, tüm çalışan alıcılar, görevleri ve olay verilerini saklamak istediğiniz Rdd başarısız olur. Bu durumda, daha sonra devam edebilmek için işinin ilerleme durumunu kaydetmek için gerekir. Bu denetim noktası tarafından gerçekleştirilir yönlendirilmiş Çevrimsiz graf (DAG) DStream düzenli aralıklarla hataya dayanıklı depolama için. Akış uygulaması, uygulama tanımlama işlemleri ve sıraya alınmış ancak henüz tamamlanmadığı toplu işler oluşturmak için kullanılan yapılandırma DAG meta veriler içerir. Bu meta veriler denetim noktası bilgilerinden başlatılması başarısız sürücü sağlar. Sürücünün yeniden başlatıldıktan sonra yeni alıcılar kendilerini yazma tamamlanan günlüğünden Rdd uygulamasına geri olay verileri kurtarmak başlatılır.

Kontrol noktaları Spark akışı iki adımda etkinleştirilir. 

1. StreamingContext nesnesinde depolama yolu için kontrol noktalarını yapılandırın:

    ```Scala
    val ssc = new StreamingContext(spark, Seconds(1))
    ssc.checkpoint("/path/to/checkpoints")
    ```

    HDInsight bu kontrol noktalarının kümenize, Azure depolama veya Azure Data Lake Storage bağlı varsayılan depolama alanı için kaydedilmesi gerekir.

2. Ardından, DStream üzerinde bir denetim noktası aralığı (saniye cinsinden) belirtin. Her bir aralıkta, depolama için giriş olay kaynağından türetilmiş durumu verilerini kalıcı hale getirilir. Kalıcı durum veri kaynağı olay durumu yeniden oluştururken gerekli hesaplama azaltabilir.

    ```Scala
    val lines = ssc.socketTextStream("hostname", 9999)
    lines.checkpoint(30)
    ssc.start()
    ssc.awaitTermination()
    ```

### <a name="use-idempotent-sinks"></a>Idempotent havuzlarını kullanın

Hedef havuz, iş sonuçları yazacağı durum burada da aynı sonucu birden çok kez verilir işleyebilir olması gerekir. Havuz böyle yinelenen sonuçlar algılayın ve bunların yoksayılması olması gerekir. Bir *ıdempotent* havuz durumunun herhangi bir değişiklik ile aynı verilerle birden çok kez çağrılabilir.

İlk veri deposu gelen sonucu bulunup bulunmadığını kontrol eder mantığı uygulayarak ıdempotent havuzlar oluşturabilirsiniz. Sonucu zaten varsa, Spark işi perspektifinden başarılı olması için yazma görünür, ancak gerçekte yinelenen verileri veri deponuz yok sayıldı. Sonuç yoksa havuz, depolama alanına bu yeni sonucu eklemelisiniz. 

Örneğin, olayları bir tabloya ekler, Azure SQL veritabanı ile bir saklı yordamı kullanabilirsiniz. Bu saklı yordamı ilk anahtar alanları tarafından ve yalnızca bulunan eşleşen hiçbir olay tabloya eklenen kayıt olduğunda olay arar.

Azure depolama blobları veya Azure Data Lake depolama gibi bir bölümlenmiş dosya sistemini kullanan başka bir örnektir. Bu durumda bir dosyanın varlığını denetlemek havuz mantığınızı gerekmez. Olay temsil eden dosya varsa, yalnızca aynı verilerle üzerine yazılır. Aksi takdirde, yeni bir dosya hesaplanan yolda oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark akış genel bakış](apache-spark-streaming-overview.md)
* [Apache Hadoop YARN yüksek oranda kullanılabilir bir Apache Spark akış işleri oluşturma](apache-spark-streaming-high-availability.md)
