---
title: "Premium Azure Redis önbelleği için veri kalıcılığını yapılandırma"
description: "Yapılandırma ve Premium katmanı Azure Redis önbelleği örnekleri veri kalıcılığını yönetme hakkında bilgi edinin"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: wesmc
ms.openlocfilehash: 270158bbf85a58a48a367a091ad2b09a9d114b2b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Premium Azure Redis önbelleği için veri kalıcılığını yapılandırma
Azure Redis önbelleği, önbellek boyutunu ve özelliklerini, kümeleme, sürdürme ve sanal ağ desteği gibi Premium katmanı özellikleri dahil olmak üzere seçimi esneklik sağlayan farklı önbellek teklifleri vardır. Bu makalede, bir premium Azure Redis önbelleği örneğine kalıcılığı yapılandırma açıklar.

Diğer premium önbellek özellikleri hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Veri kalıcılığını nedir?
[Redis kalıcılığı](https://redis.io/topics/persistence) Redis içinde depolanan veriler kalıcı olanak tanır. Ayrıca, anlık görüntülerini almak ve bir donanım hatası durumunda yük verileri yedekleyin. Burada tüm verileri bellekte depolanır ve ön bellek düğümleri aşağı nerede arıza durumunda olası veri kaybı olabilir temel veya standart katmanı üzerinde büyük bir avantaj budur. 

Azure Redis önbelleği Redis kalıcılığı aşağıdaki modelleri kullanarak sunar:

* **RDB kalıcılığı** -zaman RDB (Redis veritabanı) Kalıcılık yapılandırılır, Azure Redis önbelleği devam ederse, disk ikili biçime bağlı yapılandırılabilir bir yedekleme sıklığı Redis Redis önbelleğinde görüntüsünü. Hem birincil hem de çoğaltma önbelleği devre dışı bırakan geri dönülemez bir olay meydana gelirse, önbellek son anlık görüntü kullanılarak düzenlenir. Daha fazla bilgi edinmek [avantajları](https://redis.io/topics/persistence#rdb-advantages) ve [dezavantajları](https://redis.io/topics/persistence#rdb-disadvantages) RDB kalıcı olarak.
* **AOF kalıcılığı** -zaman AOF (yalnızca dosya ekleme) Kalıcılık yapılandırılır, Azure Redis önbelleği her yazma işlemi bir Azure depolama hesabıyla saniye başına en az bir kez kaydedilmiş bir günlüğe kaydeder. Hem birincil hem de çoğaltma önbelleği devre dışı bırakan geri dönülemez bir olay meydana gelirse, önbellekte saklanan yazma işlemleri kullanarak düzenlenir. Daha fazla bilgi edinmek [avantajları](https://redis.io/topics/persistence#aof-advantages) ve [dezavantajları](https://redis.io/topics/persistence#aof-disadvantages) AOF kalıcı olarak.

Kalıcılık gelen yapılandırılmış **yeni Redis önbelleği** dikey önbellek oluşturma sırasında ve **kaynak menü** için varolan premium önbelleğe alır.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Premium fiyatlandırma katmanına seçildikten sonra tıklatın **Redis kalıcılığı**.

![Redis kalıcılığı][redis-cache-persistence]

Sonraki bölümde yer alan adımları yeni premium önbelleğiniz Redis kalıcılığı yapılandırma açıklanmaktadır. Redis kalıcılığı yapılandırıldıktan sonra tıklatın **oluşturma** yeni premium önbelleğiniz Redis kalıcılığı ile oluşturmak için.

## <a name="enable-redis-persistence"></a>Redis kalıcılığı etkinleştir

Redis kalıcılığı etkin **Redis veri kalıcılığını** ya da seçerek dikey **RDB** veya **AOF** Kalıcılık. Yeni önbellekler için bu dikey penceresinde önceki bölümde açıklandığı gibi önbelleği oluşturma işlemi sırasında erişilir. Varolan önbellekler için **Redis veri kalıcılığını** dikey erişilen **kaynak menü** önbelleğiniz için.

![Ayarları redis][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>RDB kalıcılığı yapılandırma

RDB kalıcılığını etkinleştirmek için **RDB**. Daha önce etkinleştirilmiş premium önbellekteki RDB Kalıcılık devre dışı bırakmak için **devre dışı**.

![Redis RDB kalıcılığı][redis-cache-rdb-persistence]

Yedekleme aralığını yapılandırmak için seçin bir **yedekleme sıklığı** aşağı açılan listeden. Seçenekleri **15 dakika**, **30 dakika**, **60 dakika**, **6 saat**, **12 saat**ve **24 saat**. Bu aralık, önceki yedekleme işlemi başarıyla tamamlandıktan sonra sona erdiğinde yeni bir yedekleme başlatılır sayım başlatır.

Tıklatın **depolama hesabı** ve ya da seçin için depolama hesabı seçmek için **birincil anahtar** veya **ikincil anahtar** kullanmak için **depolama anahtarı** açılır. Önbellek ile aynı bölgede bir depolama hesabı seçmeniz gerekir ve bir **Premium depolama** hesabı, premium depolama daha yüksek verimlilik olduğundan önerilir. 

> [!IMPORTANT]
> Kalıcılık hesabınız için depolama anahtarı yeniden oluşturulursa istediğiniz anahtarı yapılandırmalısınız **depolama anahtarı** açılır.
> 
> 

Tıklatın **Tamam** kalıcı yapılandırmayı kaydetmek için.

Yedekleme sıklığı aralığı geçtikten sonra sonraki yedekleme (veya ilk yedek yeni önbellekler için) başlatılır.

## <a name="configure-aof-persistence"></a>AOF kalıcılığı yapılandırma

AOF kalıcılığını etkinleştirmek için **AOF**. Daha önce etkinleştirilmiş premium önbellekteki AOF Kalıcılık devre dışı bırakmak için **devre dışı**.

![Redis AOF kalıcılığı][redis-cache-aof-persistence]

AOF Kalıcılık yapılandırmak için belirtmeniz bir **ilk depolama hesabı**. Bu depolama hesabını önbellek ile aynı bölgede olması gerekir ve bir **Premium depolama** hesabı, premium depolama daha yüksek verimlilik olduğundan önerilir. Adlı bir ek depolama alanı hesabı isteğe bağlı olarak yapılandırabilirsiniz **ikinci depolama hesabı**. İkinci bir depolama hesabı yapılandırdıysanız, çoğaltma önbelleği yazmaları ikinci bu depolama hesabına yazılır. Her yapılandırılan depolama hesabı için ya da seçin **birincil anahtar** veya **ikincil anahtar** kullanmak için **depolama anahtarı** açılır. 

> [!IMPORTANT]
> Kalıcılık hesabınız için depolama anahtarı yeniden oluşturulursa istediğiniz anahtarı yapılandırmalısınız **depolama anahtarı** açılır.
> 
> 

AOF Kalıcılık etkinleştirildiğinde, belirtilen depolama hesabı (veya ikinci bir depolama hesabı yapılandırdıysanız hesapları) işlemleri önbelleğe kaydedilir yazma. Hem birincil hem de çoğaltma önbelleği alan geri dönülemez bir arıza olması durumunda depolanan AOF günlük önbellek yeniden oluşturmak için kullanılır.

## <a name="persistence-faq"></a>Kalıcılık SSS
Aşağıdaki listede, Azure Redis önbelleği Kalıcılık hakkında sık sorulan soruların yanıtlarını içerir.

* [Önceden oluşturulmuş önbellekteki Kalıcılık etkinleştirebilirim?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Aynı anda AOF ve RDB Kalıcılık etkinleştirebilirim?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Hangi Kalıcılık modeli seçmem gerekir?](#which-persistence-model-should-i-choose)
* [Farklı bir boyutta ölçeklendirilebilir ve bir yedek ölçekleme işlemi önce yaptığınız geri ne olur?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>RDB kalıcılığı
* [RDB yedekleme sıklığı, önbellek oluşturduktan sonra değiştirebilir miyim?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Neden 60 dakikada bir RDB yedekleme sıklığını varsa var 60 dakikadan uzun yedeklemeler arasında?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Yeni bir yedekleme yapılan eski RDB yedeklemelerin ne olur?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF kalıcılığı
* [İkinci bir depolama hesabı ne zaman kullanmalıyım?](#when-should-i-use-a-second-storage-account)
* [AOF Kalıcılık etkileyen boyunca, gecikme veya my önbelleğinin performansını mu?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [İkinci depolama hesabı nasıl kaldırabilir miyim?](#how-can-i-remove-the-second-storage-account)
* [Bir yeniden yazma nedir ve my önbellek nasıl etkiler?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Ne ı etkin AOF bir önbellek ölçeklendirme beklemeniz gerekir?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [AOF verilerimi depolama nasıl düzenlenir?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Önceden oluşturulmuş önbellekteki Kalıcılık etkinleştirebilirim?
Evet, Redis kalıcılığı önbellek oluşturmada ve varolan premium önbelleklere yapılandırılabilir.

### <a name="can-i-enable-aof-and-rdb-persistence-at-the-same-time"></a>Aynı anda AOF ve RDB Kalıcılık etkinleştirebilirim?

Hayır, yalnızca RDB veya AOF, ancak ikisini aynı anda etkinleştirebilirsiniz.

### <a name="which-persistence-model-should-i-choose"></a>Hangi Kalıcılık modeli seçmem gerekir?

AOF Kalıcılık kaydeder her yazma bazı etkisi üzerinde üretilen işi, bir günlük için yapılandırılan yedekleme, performansı üzerinde en az etkiyle aralığında yedeklemeleri kaydedileceği RDB Kalıcılık ile karşılaştırılan. Veri kaybını en aza indirmek için birincil amacınız ise ve önbelleğiniz için işleme düşüş işleyebilir AOF Kalıcılık seçin. RDB Kalıcılık önbelleğiniz üzerinde en iyi üretilen işi sürdürmek istiyor, ancak hala veri kurtarma için bir mekanizma istiyorsanız seçin.

* Daha fazla bilgi edinmek [avantajları](https://redis.io/topics/persistence#rdb-advantages) ve [dezavantajları](https://redis.io/topics/persistence#rdb-disadvantages) RDB kalıcı olarak.
* Daha fazla bilgi edinmek [avantajları](https://redis.io/topics/persistence#aof-advantages) ve [dezavantajları](https://redis.io/topics/persistence#aof-disadvantages) AOF kalıcı olarak.

AOF Kalıcılık kullanırken performans hakkında daha fazla bilgi için bkz: [mu AOF Kalıcılık etkileyen boyunca, gecikme veya my önbelleğinin performansını?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Farklı bir boyutta ölçeklendirilebilir ve bir yedek ölçekleme işlemi önce yaptığınız geri ne olur?

RDB ve AOF kalıcılığını:

* Daha büyük bir boyutu ölçeği, üzerinde etkisi yoktur.
* Daha küçük bir boyuta ölçeği ve özel bir sahip [veritabanları](cache-configure.md#databases) ayarından daha büyük [veritabanları sınırı](cache-configure.md#databases) yeni boyutunuz için bu veritabanlarını verileri geri değil. Daha fazla bilgi için bkz: [olan ölçeklendirme sırasında etkilenen ayarı my özel veritabanlarını?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Daha küçük bir boyuta ölçeği ve tüm son yedekleme verileri tutmak için daha küçük boyutu yeterli alan yok, genellikle kullanarak geri yükleme işlemi sırasında anahtarları çıkarılacak [allkeys lru](http://redis.io/topics/lru-cache) çıkarma ilkesi.

### <a name="can-i-change-the-rdb-backup-frequency-after-i-create-the-cache"></a>RDB yedekleme sıklığı, önbellek oluşturduktan sonra değiştirebilir miyim?
Evet, üzerinde RDB kalıcılığı için yedekleme sıklığını değiştirebilirsiniz **Redis veri kalıcılığını** dikey. Yönergeler için bkz: [yapılandırma Redis kalıcılığı](#configure-redis-persistence).

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Neden 60 dakikada bir RDB yedekleme sıklığını varsa var 60 dakikadan uzun yedeklemeler arasında?
RDB Kalıcılık yedekleme sıklığı aralığını önceki yedekleme işlemi başarıyla tamamlanana kadar başlatılmaz. Yedekleme sıklığı 60 dakikadır ve onu bir yedekleme işlemi 15 dakika başarıyla tamamlanması için gereken, sonraki yedekleme önceki yedekleme başlangıç zamanından sonra 75 dakika kadar başlatılmaz.

### <a name="what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made"></a>Yeni bir yedekleme yapılan eski RDB yedeklemelerin ne olur?
En sonuncudan hariç tüm RDB Kalıcılık yedeklemeler otomatik olarak silinir. Bu silme işleminin hemen olmayabilir ancak eski yedeklemeler süresiz olarak kalıcı değildir.


### <a name="when-should-i-use-a-second-storage-account"></a>İkinci bir depolama hesabı ne zaman kullanmalıyım?

Önbellekteki beklenen ayarlama işlemleri daha yüksek olan düşünüyorsanız olduğunda AOF kalıcılığını ikinci bir depolama hesabı kullanmanız gerekir.  İkincil depolama hesabı ayarlama önbelleğiniz depolama bant genişliği sınırlarına ulaşma değil sağlamaya yardımcı olur.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>AOF Kalıcılık etkileyen boyunca, gecikme veya my önbelleğinin performansını mu?

AOF kalıcılığı önbellek en yüksek yük altında olduğunda bu verimliliği yaklaşık % 15-20 oranında etkiler (CPU ve sunucu yükü hem % 90'altında). Olmamalıdır gecikmesi sorunları önbellek bu sınırlar içinde olduğunda. Ancak, önbellek bu sınırlamaları daha erken etkin AOF ile tamamlayacaktır.

### <a name="how-can-i-remove-the-second-storage-account"></a>İkinci depolama hesabı nasıl kaldırabilir miyim?

İlk Depolama hesabı ile aynı olacak şekilde ikinci depolama hesabı ayarlayarak AOF Kalıcılık ikincil depolama hesabını kaldırabilirsiniz. Yönergeler için bkz: [yapılandırma AOF kalıcılığı](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Bir yeniden yazma nedir ve my önbellek nasıl etkiler?

AOF dosya yeterince büyük hale geldiğinde, bir yeniden yazma önbelleği sıraya alındı otomatik olarak. Yeniden yazma işlemleri geçerli veri kümesi oluşturmak için gereken en az sayıda AOF dosyasıyla göre yeniden boyutlandırır. Yeniden yazdırmaya sırasında performans sınırlarını er özellikle büyük veri kümeleriyle ilgilenirken ulaşmak bekler. Genellikle AOF dosyası büyük hale gelir, ancak önemli miktarda zaman zaman gerçekleşeceğini olacak şekilde yeniden yazdırmaya daha az oluşur.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Ne ı etkin AOF bir önbellek ölçeklendirme beklemeniz gerekir?

Ölçeklendirme sırasındaki AOF dosya önemli ölçüde büyükse ölçeklendirme tamamlandıktan sonra dosyayı yeniden beri beklenenden daha uzun sürmesine ölçeklendirme işlemi bekler.

Ölçeklendirme ile ilgili daha fazla bilgi için bkz: [farklı bir boyutta ölçeklendirilebilir ve bir yedek ölçekleme işlemi önce yaptığınız geri ne olur?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>AOF verilerimi depolama nasıl düzenlenir?

AOF dosyalarında depolanan verileri depolama alanına veri kaydetme performansını artırmak için düğüm başına birden çok sayfa bloblarını ayrılmıştır. Aşağıdaki tabloda, kaç tane sayfa bloblarını her fiyatlandırma katmanının için kullanılan görüntüler:

| Premium katman | Bloblar |
|--------------|-------|
| P1           | parça başına 4    |
| P2           | parça başına 8    |
| P3           | parça başına 16   |
| P4           | parça başına 20   |

Kümeleme etkinleştirildiğinde, her parça önbelleğinde önceki tabloda belirtildiği gibi sayfa BLOB'ları, kendi kümesi vardır. Örneğin, üç parça P2 önbellekle AOF dosya 24 sayfa blobları (8 BLOB'lar ile 3 parça parça başına) dağıtır.

Bir yeniden yazma sonra depoda AOF dosyaları iki kümesi yok. Yeniden yazdırmaya arka planda oluşur ve ikinci kümesine ekleme önbelleğine yeniden yazma sırasında gönderilen ayarlama işlemleri sırasında dosyaların ilk kümesine ekleyin. Bir yedekleme hatası durumunda yeniden yazdırmaya sırasında geçici olarak depolanır, ancak bir yeniden yazma tamamlandıktan sonra hemen silinir.


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerinin nasıl kullanılacağını öğrenin.

* [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
