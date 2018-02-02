---
title: "Spark akış işleri ile tam olarak oluşturma-kez olay işleme - Azure Hdınsight | Microsoft Docs"
description: "Spark akış yalnızca tek bir kez bir olay işlemek için nasıl kurulur."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ramoha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: ramoha
ms.openlocfilehash: ebab9ebc92ae1dff8902d618d0a474ce2b2a0af3
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Spark akış işleri ile tam olarak oluşturma-kez olay işleme

Akış işleme uygulamaları nasıl bunlar yeniden işlem iletileri bazı hatasından sonra sistemde işlemek için farklı yaklaşımlara alın:

* En az bir kez: her ileti işlenmek üzere sağlanır, ancak birden çok kez işlendikten.
* En fazla bir kez: her ileti olabilir veya işlenmemiş. Bir ileti işlediğinde, bu yalnızca bir kez işlenir.
* Tam olarak bir kez: her ileti yalnızca tek bir kez işlenecek garanti edilmez.

Bu makalede tam olarak elde etmek için Spark akış yapılandırılacağı gösterilmiştir-bir kez işleme.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Tam olarak-kez semantiği Spark akış ile

İlk olarak, göz önünde bulundurun hata tüm sistem noktaları bir sorun yaşıyor sonra yeniden başlatma ve veri kaybını önlemek nasıl. Spark akış uygulama vardır:

* Bir giriş kaynağı
* Giriş kaynağından veri çekme bir veya daha fazla alıcı işlemleri
* Veri işleme görevleri
* Çıkış havuzu
* Uzun süre çalışan iş yöneten bir sürücü işlemi

Tam olarak-herhangi bir noktada veri kaybolur ve bu ileti işlenirken hata oluştuğu bağımsız olarak yeniden başlatılabilir, semantiği gerektiren sonra.

### <a name="replayable-sources"></a>Replayable kaynakları

Spark akış uygulamanız, olayları okuma kaynağı olmalıdır *replayable*. Bu, burada iletisi alındı, ancak ileti kalıcı veya işlenen önce sistem başarısız durumda kaynak aynı ileti yeniden sağlamanız gereken anlamına gelir.

Azure'da, hem Azure Event Hubs hem de hdınsight'ta Kafka replayable kaynakları sağlar. Hataya dayanıklı dosya sistemi HDFS, Azure Storage bloblarında gibi replayable bir kaynağı başka bir örneği olan veya Azure Data Lake Store, burada tüm veri tutulur sonsuza kadar ve herhangi bir noktada, bütün verileri yeniden okuyabilir.

### <a name="reliable-receivers"></a>Güvenilir Alıcılar

Spark akış olay hub'ları ve Kafka yok gibi kaynakları *Güvenilir Alıcılar*, burada her alıcı kaynak okuma ilerleme durumunu izler. Güvenilir bir alıcı ZooKeeper içinde veya Spark akış denetim noktaları HDFS için yazılmış hataya dayanıklı depolama alanına durumuna devam ettirir. Bu tür bir alıcı başarısız olursa ve daha sonra yeniden, bu kaldığı yerden yukarı seçebilirsiniz.

### <a name="use-the-write-ahead-log"></a>Yazma tamamlanan günlük kullanın

Spark akış bir yazma tamamlanan burada alınan her olay ilk hataya dayanıklı bir depolama alanında Spark denetim noktası dizinine yazılmış ve bir esnek Dağıtılmış veri kümesi (RDD içinde) depolanan günlük kullanımını destekler. Azure'da, hataya dayanıklı depolama Azure Storage veya Azure Data Lake Store tarafından yedeklenen HDFS ' dir. Spark akış uygulamanızda ayarlayarak yazma tamamlanan günlük için tüm alıcıları etkin `spark.streaming.receiver.writeAheadLog.enable` yapılandırma ayarına `true`. Yazma tamamlanan günlük sürücü ve yürütücüler hataları için hata toleransı sağlar.

Olay verileri karşı çalışan görevlerin çalışanları için her ikisi de çoğaltılır ve birden çok Worker arasında dağıtılmış tanımı her RDD gereğidir. Bu nedenle olay verileri çoğaltmasını sahip başka bir çalışan üzerinde kilitlenen çalışan çalışan görev yeniden çünkü bir görev başarısız olursa, olay kayıp değil.

### <a name="use-checkpoints-for-drivers"></a>Kontrol noktaları sürücülerini kullanın

İş sürücüleri yeniden başlatılabilir olması gerekir. Spark akış uygulamanızı çalıştıran sürücü çökerse, tüm çalışan alıcıları, görevler ve olay verilerini depolamak RDDs başarısız olur. Bu durumda, daha sonra devam edebilmek için işin ilerleme durumunu kaydetmek olması gerekir. Bu denetim noktası oluşturma tarafından gerçekleştirilir yönlendirilmiş Çevrimsiz grafik (DAG), düzenli aralıklarla hataya dayanıklı bir depolama alanına DStream. Akış uygulama, uygulama tanımlama işlemleri ve sıraya alınmış ancak henüz tamamlanan toplu işleri oluşturmak için kullanılan yapılandırma DAG meta verileri içerir. Bu meta verileri kontrol noktası bilgileri yeniden başlatılması başarısız bir sürücü sağlar. Sürücünün yeniden başlatıldığında, kendilerini RDDs yazma tamamlanan günlüğünden uygulamasına geri olay verileri kurtarmak yeni alıcılar başlatır.

Kontrol noktaları Spark akış iki adımda etkinleştirilir. 

1. StreamingContext nesnesinde depolama yolu için kontrol noktalarını yapılandırın:

    VAL ssc yeni StreamingContext (spark, Seconds(1)) ssc.checkpoint("/path/to/checkpoints") =

    Hdınsight'ta, kümeniz için Azure Storage veya Azure Data Lake Store bağlı varsayılan depolama için bu kontrol noktalarının kaydedilmesi gerekir.

2. Ardından, DStream üzerinde bir denetim noktası aralığı (saniye cinsinden) belirtin. Her aralıkta giriş olayından türetilmiş durumu verilerini depolama birimine kalıcıdır. Kalıcı durum veri kaynağı olay durumundan yeniden oluştururken gerekli hesaplama azaltabilir.

    VAL satırları ssc.socketTextStream ("ana bilgisayar adı", 9999) = lines.checkpoint(30) ssc.start() ssc.awaitTermination()

### <a name="use-idempotent-sinks"></a>Idempotent havuzlarını kullanın

İşinizin sonuçları yazacağı hedef havuz burada da aynı sonucu birden çok kez verilir durum kaldırabilir olmalıdır. Havuz böyle yinelenen sonuçları algılamak ve onları göz ardı kurabilmesi gerekir. Bir *ıdempotent* havuz durumunun değişiklik olmadan aynı verilerle birden çok kez çağrılamaz.

İlk veri deposu gelen sonucunda varlığını denetler mantığı uygulayarak ıdempotent havuzlarını oluşturabilirsiniz. Sonuç zaten varsa, yazma, Spark iş açısından başarılı olması için görünür, ancak gerçekte yinelenen verileri data store yoksayıldı. Sonuç yoksa, sonra havuzu bu yeni sonucu, depolama alanına eklemeniz gerekir. 

Örneğin, olayları bir tabloya ekler Azure SQL veritabanı ile bir saklı yordamı kullanabilirsiniz. Bu saklı yordam ilk anahtar alanları ve yalnızca eşleşen hiç olay bulunan tabloya eklenen kayıt olduğunda olay arar.

Başka bir örnek bir Azure Storage bloblarını gibi bölümlenmiş dosya sistemi veya Azure Data Lake deposu kullanmaktır. Bu durumda, havuz mantığınızı dosyasının varlığını denetlemek gerekmez. Olay temsil eden dosya varsa, yalnızca aynı verilerle üzerine yazılır. Aksi takdirde, yeni bir dosya hesaplanan yolda oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış Spark akış](apache-spark-streaming-overview.md)
* [Yüksek oranda kullanılabilir Spark akış işleri YARN içinde oluşturuluyor](apache-spark-streaming-high-availability.md)
