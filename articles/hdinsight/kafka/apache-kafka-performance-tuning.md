---
title: Apache Kafka HDInsight kümeleri için performansı iyileştirme
description: Azure HDInsight üzerinde Apache Kafka iş yüklerini en iyi duruma getirme için teknik genel bakış sağlar.
services: hdinsight
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
origin.date: 02/21/2019
ms.date: 04/29/2019
ms.openlocfilehash: 8226d1f49b8ba73870dba009e97ff2718a0eee27
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62115037"
---
# <a name="performance-optimization-for-apache-kafka-hdinsight-clusters"></a>Apache Kafka HDInsight kümeleri için performansı iyileştirme

Bu makalede, HDInsight, Apache Kafka iş yüklerinin performansını iyileştirmek için bazı öneriler sağlar. Üretici ve aracı yapılandırma belirlenmesiyle ilgili biridir. Performansı ölçme farklı yolu vardır ve uyguladığınız en iyi duruma getirme, iş gereksinimlerinize bağlıdır.

## <a name="architecture-overview"></a>Mimariye genel bakış

Kafka konularını kayıtları düzenlemek için kullanılır. Kayıtları üreticileri tarafından üretilen ve tüketiciler tarafından tüketilen. Üreticiler kayıtlar ardından verileri depolamak Kafka aracıları için gönderin. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır.

Aracılar arasında konuların bölüm kayıtları. Kayıtları tüketirken, verilerin paralel işlemesini elde etmek için bölüm başına en fazla bir tüketici kullanabilirsiniz.

Çoğaltma, yinelenen düğümler arasında bölümler için kullanılır. Bu düğüm (aracı) kesintilerine karşı korur. Çoğaltma grubu arasında tek bir bölüm bölüm lider olarak atanır. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

## <a name="identify-your-scenario"></a>Senaryonuzu tanımlama

Apache Kafka performans iki ana boyutu – aktarım hızı ve gecikme süresi vardır. Veri işlenebilecek en yüksek hızı aktarım hızıdır. Daha yüksek iş hacmi genellikle daha iyidir. Gecikme süresi alınamıyor veya depolanan verilerin için gereken süre anlamına gelmektedir. Daha düşük gecikme süresi genellikle daha iyidir. Aktarım hızı, gecikme süresi ve uygulamanın altyapı maliyetini arasındaki doğru dengeyi bulmak zor olabilir. Performans gereksinimlerinizi büyük olasılıkla yüksek aktarım hızı, düşük gecikme süresi veya her ikisi de gereksiniminiz olup üzerinde göre aşağıdaki üç yaygın durumlar, biri eşleşir:

* Yüksek aktarım hızı, düşük gecikme süresi. Bu senaryo, hem yüksek aktarım hızına ve düşük gecikme süresi (~ 100 milisaniye cinsinden) gerektirir. Bu tür bir uygulama örneği olan hizmet kullanılabilirliği izleme.
* Yüksek aktarım hızı, gecikme süresi yüksek. Bu senaryo, yüksek aktarım hızı (~1.5 GB/sn) gerektirir, ancak daha yüksek gecikme süresi (< 250 ms) kabul edebilir. Telemetri veri alımı için neredeyse gerçek zamanlı işlemleri güvenlik ve yetkisiz erişim algılama uygulamalar gibi bu tür bir uygulama örneğidir.
* Düşük aktarım hızı, düşük gecikme süresi. Bu senaryo, düşük gecikme süresi (< 10 ms) için gerçek zamanlı işleme gerektirir, ancak daha düşük aktarım hızı dayanabilir. Çevrimiçi yazım ve dilbilgisi denetimleri bu tür bir uygulama örneğidir.

## <a name="producer-configurations"></a>Üretici yapılandırmaları

Aşağıdaki bölümlerde bazı, Kafka üreticileri performansını iyileştirmek için en önemli yapılandırma özellikleri vurgular. Tüm yapılandırma özellikleri hakkında ayrıntılı açıklaması için bkz [üretici yapılandırmaları üzerinde Apache Kafka belgeleri](https://kafka.apache.org/documentation/#producerconfigs).

### <a name="batch-size"></a>Toplu işlem boyutu

Apache Kafka üreticileri (toplu olarak adlandırılır) tek bir depolama bölümünde depolanması için bir birim olarak gönderilen iletilerin grupları toplayın. Toplu iş boyutu o grubun iletilmeden önce bulunması gereken bayt sayısını gösterir. Artan `batch.size` işleme ağ ve g/ç istekleri Gelen ek yükü azaltır çünkü parametresi üretilen işi artırabilir. Açık yük altında üretici bir batch hazır olmasını bekler olarak artan toplu iş boyutu Kafka gönderme gecikmesi artırabilir. Ağır yük koşullarında, aktarım hızı ve gecikme süresini iyileştirmek için toplu iş boyutunu artırmak için önerilir.

### <a name="producer-required-acknowledgements"></a>Gerekli üretici onayları

Gerekli üretici `acks` yapılandırması yazma isteği kabul edilmeden önce bölüm öncü tarafından gerekli kaynaklar sayısını belirler tamamlandı. Bu ayar olasılığına karşı veri güvenirliliğini etkiler ve değerlerini alır `0`, `1`, veya `-1`. Değerini `-1` yazma tamamlanmadan önce bir bildirim tüm çoğaltmalardan alınan gerekir anlamına gelir. Ayar `acks = -1` daha güçlü garanti eder ancak veri kaybına karşı da daha yüksek gecikme ve düşük aktarım hızı sonuçlarını sağlar. Uygulama gereksinimlerinize daha yüksek iş hacmi isteğe bağlı ayar deneyin `acks = 0` veya `acks = 1`. Tüm çoğaltmalar sıkan değil olasılığına karşı veri güvenirliliğini azaltabilir, unutmayın.

### <a name="compression"></a>Sıkıştırma

Kafka üretici aracılarına göndermeden önce iletileri sıkıştırmak için yapılandırılabilir. `compression.type` Ayar kullanılacak sıkıştırma codec bileşeni belirtir. Desteklenen bir sıkıştırma codec "gzip," "snappy," ve "lz4." olan Sıkıştırma yararlıdır ve disk kapasitesi sınırlaması yoksa düşünülmelidir.

İki yaygın olarak kullanılan sıkıştırma codec bileşenleri arasında `gzip` ve `snappy`, `gzip` daha yüksek CPU yükü, düşük disk kullanımı sonuçları daha yüksek bir sıkıştırma oranına sahiptir. `snappy` Codec bileşeni, daha az CPU yükü daha azdır sıkıştırmasıyla sağlar. Kullanılacak codec bileşeni Aracısı disk veya üretici CPU sınırlamalarına bağlı karar verebilirsiniz. `gzip` Veri beş kat daha yüksek bir hızda sıkıştırabilirsiniz `snappy`.

Veri sıkıştırma kullanarak bir diske depolanmış kayıt sayısını artırır. Bu da CPU ek yükü durumlarda artırabilir üretici ve aracı tarafından kullanılan sıkıştırma biçimleri arasında bir uyuşmazlık olduğunda. Verileri göndermeden önce sıkıştırılmış ve ardından işleme önce eklenmişti.

## <a name="broker-settings"></a>Aracı Ayarları

Aşağıdaki bölümlerde bazı Kafka aracılarınız performansını iyileştirmek için en önemli ayarlar vurgular. Tüm ayrıntılı bir açıklaması için aracı ayarları, bkz: [üretici yapılandırmaları üzerinde Apache Kafka belgeleri](https://kafka.apache.org/documentation/#producerconfigs).


### <a name="number-of-disks"></a>Disk sayısı

Depolama diskleri sınırlı IOPS (giriş/çıkış işlem / saniye) ve okuma/yazma bayt / saniye. Yeni bölüm oluştururken, Kafka en az mevcut bölümler arasında kullanılabilir diskleri bunları dengelemek için disk üzerinde her yeni bölüme depolar. Her disk üzerindeki bölüm çoğaltmalarını yüzlerce işlerken depolama strateji rağmen Kafka kullanılabilir disk aktarım hızı bir kolayca karşılayabilir. Aktarım hızı ile maliyet artırabilen burada arasındadır. Daha fazla yönetilen uygulamanızın daha fazla performans gerekiyorsa, küme oluşturma aracı başına disk sayısı. HDInsight, yönetilen diskler çalıştıran bir kümeye ekleme şu anda desteklemiyor. Yönetilen disk sayısını yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma depolama ve ölçeklenebilirlik için HDInsight üzerinde Apache Kafka](apache-kafka-scalability.md). Depolama alanını artırmak için kümenizdeki düğümleri maliyet etkilerini anlayın.

### <a name="number-of-topics-and-partitions"></a>Konuları ve bölümleri sayısı

Kafka üreticileri konulara yazın. Kafka tüketicileri konuları okuyun. Bir konu disk üzerindeki veri yapısıdır bir oturum ile ilişkilidir. Kafka, kayıtları bir producer(s) konu günlük sonuna ekler. Birden çok dosya yayılan birçok bölümlerin konu günlük oluşur. Sırayla bu dosyaları birden fazla Kafka küme düğümleri arasında yayılır. Tüketiciler, kendi temposu, Kafka konularını okuma ve konu günlük konumlarını (kaydırma) seçebilirsiniz.

Bir günlük dosyası sistem üzerindeki her Kafka bölümdür ve üretici iş parçacığı aynı anda birden çok günlüklerine yazabilirsiniz. Benzer şekilde, her bir tüketicinin iş parçacığı bir bölümden iletileri okuyan olduğundan, birden çok bölümdeki verileri kullanan de paralel olarak ele alınır.

Bölüm yoğunluk (bölüm başına aracısı sayısı) artırılması, meta veri işlemleri ve her bölüm istek/yanıt bölümü lideri, takipçi arasındaki ile ilgili bir yükü ekler. Olmadığında üzerinden akan veri içinde bölüm çoğaltmalarını yine de verileri göndermek için ek işlem sonuçlanır ve ağ üzerinden isteklerini alacak öncüleri dan getirin.

İçin Apache Kafka 1.1 kümeleri ve yukarıda HDInsight içinde en fazla 1000 bölüm çoğaltmaları dahil olmak üzere, aracı başına sahip olmanızı öneririz. Aracı başına bölüm sayısının artırılması, aktarım hızı azaltır ve konu kullanılamazlık da neden olabilir. Kafka bölüm desteği hakkında daha fazla bilgi için bkz. [resmi Apache Kafka Web günlüğü gönderisini 1.1.0 sürümünde desteklenen bölüm sayısı artış](https://blogs.apache.org/kafka/entry/apache-kafka-supports-more-partitions). Değiştirme konuları hakkında daha fazla bilgi için bkz [Apache Kafka: değiştirme konuları](https://kafka.apache.org/documentation/#basic_ops_modify_topic).

### <a name="number-of-replicas"></a>Çoğaltma sayısı

Daha yüksek bir çoğaltma faktörü ek istekler arasında takipçileri ve bölümü lideri sonuçlanır. Sonuç olarak, daha fazla disk ve CPU ek isteklerini işlemek için daha yüksek bir çoğaltma faktörü tüketir artırma yazma gecikme süresi ve aktarım hızı azaltma.

Azure HDInsight Kafka'da için en az 3 x çoğaltma kullanmanızı öneririz. Çoğu Azure bölgeleri üç hata etki alanlarına sahip ancak bölgelerde yalnızca iki hata etki alanları ile kullanıcıların 4 x çoğaltma kullanmanız gerekir.

Çoğaltma hakkında daha fazla bilgi için bkz. [Apache Kafka: çoğaltma](https://kafka.apache.org/documentation/#replication) ve [Apache Kafka: çoğaltma faktörü artırma](https://kafka.apache.org/documentation/#basic_ops_increase_replication_factor).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure üzerinde Apache Kafka ile günde milyarlarca olayı işleme](https://azure.microsoft.com/blog/processing-trillions-of-events-per-day-with-apache-kafka-on-azure/)
* [HDInsight üzerinde Apache Kafka nedir?](apache-kafka-introduction.md)
