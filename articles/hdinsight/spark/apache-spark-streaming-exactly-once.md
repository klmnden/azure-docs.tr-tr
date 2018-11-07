---
title: Spark akış işleri ile tam olarak oluşturma-kere olay işleme - Azure HDInsight
description: Spark akış yalnızca tek bir kez bir olayı işlemek için nasıl kurulur.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 6c39eb02e9610e0020ab2abe8a192dabf0b768d9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241332"
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Spark akış işleri ile tam olarak oluşturma-kere olay işleme

Stream işleme uygulamalarını nasıl bunlar yeniden işlem iletileri bazı hatadan sonra sistemde işlemek için farklı yaklaşımlara uygulayın:

* En az bir kez: her iletinin işlenmesi sağlanır, ancak birden çok kez işlendikten.
* En fazla bir kez: her ileti olabilir veya işlenmemiş. Bir ileti işlediğinde, yalnızca bir kez işlenir.
* Tam bir kez: her iletiyi yalnızca tek bir kez işlenmesi sağlanır.

Bu makalede, tam olarak elde etmek için Spark akışı yapılandırma gösterir-bir kez işlenmesini.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Tam olarak-Spark akışı ile bir kez semantiği

İlk olarak göz önünde bulundurun hata tüm sistemin noktaları bir sorun atandıktan sonra yeniden başlatma ve veri kaybını önlemek nasıl. Uygulama Spark akışı vardır:

* Bir giriş kaynağı
* Bir veya daha fazla alıcı işlemleri ise giriş kaynağından veri çekme
* Veri işleme görevleri
* Çıkış havuzu
* Uzun süre çalışan iş yöneten bir sürücü işlemi

Tam olarak-sonra hiçbir veri herhangi bir noktada kaybolur ve bu ileti işleme hatanın oluştuğu bağımsız olarak yeniden başlatılabilir, semantiği gerektirir.

### <a name="replayable-sources"></a>Yeniden oynatılabilir kaynakları

Spark Streaming uygulamanız, olaylarından okuma kaynağı olmalıdır *yeniden oynatılabilir*. Bu durumda, burada iletisi alındı, ancak iletinin kalıcı veya işlenen önce sistem başarısız oldu, kaynak aynı ileti yeniden sağlamanız gereken anlamına gelir.

Azure'da, hem Azure Event Hubs hem de HDInsight üzerinde kafka'yı yeniden oynatılabilir kaynakları sağlar. HDFS, Azure depolama blobları gibi bir hataya dayanıklı dosya sistemi yeniden oynatılabilir bir kaynağı başka bir örneği olduğu veya nerede tüm veriler tutulur süresiz olarak ve herhangi bir noktada, Azure Data Lake Store, verilerin tamamına yeniden okuyabilirsiniz.

### <a name="reliable-receivers"></a>Güvenilir Alıcılar

Spark akış, olay hub'ları ve Kafka gibi kaynakları *Güvenilir Alıcılar*, burada her alıcı kaynak okuma, ilerleme durumunu izler. Güvenilir bir alıcı, hataya dayanıklı depolama, ZooKeeper içinde veya Spark akışı denetim noktaları HDFS'ye yazılan içine durumuna devam ettirir. Böyle bir alıcı başarısız olursa ve daha sonra yeniden başlatıldı, bu kaldığı yerden devam edebiliyorduk.

### <a name="use-the-write-ahead-log"></a>Yazma önceden yazılan günlük kullanın

Spark akışı, bir yazma tamamlanan alınan her olayın burada ilk hataya dayanıklı depolama, Spark'ın denetim noktası dizinine yazılır ve sonra bir dayanıklı Dağıtılmış veri kümesi (RDD içinde) depolanan günlük kullanımını destekler. Azure'da, Azure depolama veya Azure Data Lake Store tarafından desteklenen HDFS hataya dayanıklı depolama alanıdır. Spark Streaming uygulamanızda ayarlayarak yazma önceden yazılan günlük için tüm alıcıları etkin `spark.streaming.receiver.writeAheadLog.enable` yapılandırma ayarı `true`. Yazma önceden yazılan günlük hem sürücü hem de Yürütücü hataları için hata toleransı sağlar.

Olay verilerini karşı çalışan görevlerin çalışanları için hem çoğaltılacak ve birden fazla çalışana arasında dağıtılmış tanımı her RDD gereğidir. Kilitlenen çalışan alt görev olayı veri çoğaltmasını sahip başka bir çalışan üzerinde yeniden başlatılacak bir görev başarısız olur, böylece olay kaybolmaz.

### <a name="use-checkpoints-for-drivers"></a>Sürücüleri kontrol noktalarını kullanma

İş sürücüleri yeniden başlatılabilir olması gerekir. Spark Streaming uygulamanızı çalıştıran sürücünün kilitlenmesi durumunda, tüm çalışan alıcılar, görevleri ve olay verilerini saklamak istediğiniz Rdd başarısız olur. Bu durumda, daha sonra devam edebilmek için işinin ilerleme durumunu kaydetmek için gerekir. Bu denetim noktası tarafından gerçekleştirilir yönlendirilmiş Çevrimsiz graf (DAG) DStream düzenli aralıklarla hataya dayanıklı depolama için. Akış uygulaması, uygulama tanımlama işlemleri ve sıraya alınmış ancak henüz tamamlanmadığı toplu işler oluşturmak için kullanılan yapılandırma DAG meta veriler içerir. Bu meta veriler denetim noktası bilgilerinden başlatılması başarısız sürücü sağlar. Sürücünün yeniden başlatıldıktan sonra yeni alıcılar kendilerini yazma tamamlanan günlüğünden Rdd uygulamasına geri olay verileri kurtarmak başlatılır.

Kontrol noktaları Spark akışı iki adımda etkinleştirilir. 

1. StreamingContext nesnesinde depolama yolu için kontrol noktalarını yapılandırın:

    ```Scala
    val ssc = new StreamingContext(spark, Seconds(1))
    ssc.checkpoint("/path/to/checkpoints")
    ```

    HDInsight bu kontrol noktalarının kümenize, Azure depolama veya Azure Data Lake Store bağlı varsayılan depolama alanı için kaydedilmesi gerekir.

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

Başka bir örnek, bir Azure depolama blobları gibi bölümlenmiş dosya sistemi veya Azure Data Lake store kullanmaktır. Bu durumda bir dosyanın varlığını denetlemek havuz mantığınızı gerekmez. Olay temsil eden dosya varsa, yalnızca aynı verilerle üzerine yazılır. Aksi takdirde, yeni bir dosya hesaplanan yolda oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Spark akış genel bakış](apache-spark-streaming-overview.md)
* [YARN yüksek oranda kullanılabilir Spark akış işleri oluşturma](apache-spark-streaming-high-availability.md)
