---
title: Premium Azure önbelleği için Redis veri kalıcılığı yapılandırma
description: Yapılandırma ve Azure Cache Redis örneği için Premium katman veri kalıcılığı yönetme hakkında bilgi edinin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: yegu
ms.openlocfilehash: de0b2e3ef7b0268540ef4896ade132a297ee88ff
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60543515"
---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-cache-for-redis"></a>Premium Azure önbelleği için Redis veri kalıcılığı yapılandırma
Azure önbelleği için Redis önbellek boyutunu ve özelliklerini, kümeleme, Kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri dahil olmak üzere tercih ettiğiniz esneklik sağlayan farklı bir önbellek teklifleri sahiptir. Bu makalede de Redis örneği için bir premium Azure Cache kalıcılığı yapılandırma açıklanır.

Diğer premium önbellek özelliklerini hakkında daha fazla bilgi için bkz: [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Veri kalıcılığı nedir?
[Redis kalıcılığı](https://redis.io/topics/persistence) Redis içinde depolanan verileri kalıcı hale getirmenize olanak tanır. Ayrıca, anlık ve bir donanım hata durumunda yükleyebilirsiniz verileri yedekleyin. Burada tüm verileri, bellekte depolanır ve önbellek düğümleri aşağı nerede arıza durumunda olası veri kaybı olabilir temel veya standart katman büyük avantaj budur. 

Azure önbelleği için Redis aşağıdaki modelleri kullanarak Redis kalıcılığı sunar:

* **RDB Kalıcılık** -zaman RDB (Redis veritabanı) Kalıcılık yapılandırılır, Azure önbelleği için Redis diske ikili biçimi, yapılandırılabilir bir yedekleme sıklığına göre bir Redis, Redis için Azure önbellek anlık görüntüsünü sürdürür. Birincil ve çoğaltma önbellek devre dışı bırakan bir felaket ortaya çıkarsa, önbellek en son anlık görüntü kullanılarak düzenlenir. Daha fazla bilgi edinin [avantajları](https://redis.io/topics/persistence#rdb-advantages) ve [dezavantajları](https://redis.io/topics/persistence#rdb-disadvantages) RDB Kalıcılık.
* **AOF Kalıcılık** -zaman AOF (yalnızca dosya ekleme) Kalıcılık yapılandırılır, Azure önbelleği için Redis her yazma işlemi en az bir kez bir Azure depolama hesabına saniyede kaydedilen bir günlüğe kaydeder. Birincil ve çoğaltma önbellek devre dışı bırakan bir felaket ortaya çıkarsa, önbellekte saklanan yazma işlemleri kullanarak düzenlenir. Daha fazla bilgi edinin [avantajları](https://redis.io/topics/persistence#aof-advantages) ve [dezavantajları](https://redis.io/topics/persistence#aof-disadvantages) AOF Kalıcılık.

Kalıcılık yapılandırıldı **yeni Azure önbelleği için Redis** dikey önbellek oluşturma sırasında ve **kaynak menüsünde** için var olan premium önbelleğe alır.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Premium fiyatlandırma katmanının seçildikten sonra tıklayın **Redis kalıcılığı**.

![Redis kalıcılığı][redis-cache-persistence]

Sonraki bölümde yer alan adımları yeni premium önbelleğinizi Redis kalıcılığı yapılandırma açıklanmaktadır. Redis kalıcılığı yapılandırıldıktan sonra tıklayın **Oluştur** Redis kalıcılığı ile yeni premium önbellek oluşturmak için.

## <a name="enable-redis-persistence"></a>Redis kalıcılığı etkinleştir

Redis kalıcılığı etkin **Redis veri kalıcılığı** seçerek ya da dikey **RDB** veya **AOF** Kalıcılık. Yeni önbellekler için bu dikey pencere önceki bölümde açıklandığı gibi önbellek oluşturma işlemi sırasında erişilir. Mevcut önbellekler için **Redis veri kalıcılığı** dikey erişilen **kaynak menüsünde** önbelleğiniz.

![Redis ayarları][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>RDB kalıcılığı yapılandırma

RDB kalıcılığını etkinleştirmek için tıklayın **RDB**. Daha önce etkinleştirilmiş premium cache RDB kalıcılığı devre dışı bırakmak için **devre dışı bırakılmış**.

![Redis RDB kalıcılığı][redis-cache-rdb-persistence]

Yedekleme zaman aralığını yapılandırmak için seçin bir **yedekleme sıklığı** aşağı açılan listeden. Seçenekler şunlardır **15 dakika**, **30 dakika**, **60 dakika**, **6 saat**, **12 saat**ve **24 saat**. Bu aralık, önceki yedekleme işlemi başarıyla tamamlandıktan sonra sona erdiğinde, yeni bir yedekleme başlatılır sayım başlatır.

Tıklayın **depolama hesabı** kullanın ve seçmeniz için depolama hesabını seçmek için **birincil anahtar** veya **ikincil anahtar** kullanmak için **Depolamaanahtarı** açılır. Önbellek ile aynı bölgede bir depolama hesabı seçin ve bir **Premium depolama** hesabı, premium depolama, yüksek aktarım hızı olduğundan önerilir. 

> [!IMPORTANT]
> Kalıcılık hesabınız için depolama anahtarı yeniden oluşturulursa, istediğiniz anahtarı yapılandırmalısınız **depolama anahtarı** açılır.
> 
> 

Tıklayın **Tamam** kalıcı yapılandırmayı kaydetmek için.

Yedekleme sıklığı aralığı sona erdiğinde sonra sonraki yedeklemeden (veya yeni önbellekler için ilk yedekleme) başlatılır.

## <a name="configure-aof-persistence"></a>AOF kalıcılığı yapılandırma

AOF kalıcılığını etkinleştirmek için tıklayın **AOF**. Daha önce etkinleştirilmiş premium önbellek kalıcılığı AOF devre dışı bırakmak için **devre dışı bırakılmış**.

![Redis AOF kalıcılığı][redis-cache-aof-persistence]

AOF kalıcılığı yapılandırma için belirtin bir **ilk depolama hesabı**. Bu depolama hesabı, önbellek ile aynı bölgede olmalıdır ve bir **Premium depolama** hesabı, premium depolama, yüksek aktarım hızı olduğundan önerilir. İsteğe bağlı olarak adlı ek bir depolama hesabı yapılandırın **ikinci depolama hesabı**. Çoğaltma önbellek yazma işlemleri, ikinci bir depolama hesabı yapılandırdıysanız, bu ikinci depolama hesabına yazılır. Her bir yapılandırılmış depolama hesabı için seçin **birincil anahtar** veya **ikincil anahtar** kullanmak için **depolama anahtarı** açılır. 

> [!IMPORTANT]
> Kalıcılık hesabınız için depolama anahtarı yeniden oluşturulursa, istediğiniz anahtarı yapılandırmalısınız **depolama anahtarı** açılır.
> 
> 

AOF Kalıcılık etkin olduğunda, belirtilen depolama hesabı (veya ikinci bir depolama hesabı yapılandırdıysanız, hesapları) işlemleri önbelleğe kaydedilir yazın. Birincil ve çoğaltma önbellek alan bir arıza durumunda depolanan AOF günlük önbelleği yeniden oluşturmak için kullanılır.

## <a name="persistence-faq"></a>Kalıcılık SSS
Aşağıdaki liste, Redis kalıcılığı için Azure önbellek hakkında sık sorulan soruların yanıtlarını içerir.

* [Önceden oluşturulmuş bir önbellek kalıcılığı etkinleştirebilirim?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Aynı anda AOF ve RDB Kalıcılık etkinleştirebilirim?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Hangi Kalıcılık modeli seçmem gerekir?](#which-persistence-model-should-i-choose)
* [Farklı bir boyuta ölçeklendirilir ve ölçeklendirme işlemi önce yapılan bir yedekleme geri ne olur?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>RDB kalıcılığı
* [RDB yedekleme sıklığı, önbellek oluşturabilirim sonra değiştirebilir miyim?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Neden RDB yedekleme sıklığı 60 dakika varsa var 60 dakikadan fazla yedeklemeler arasında?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Eski RDB yedekleri, yeni bir yedekleme yapıldığında ne olur?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF kalıcılığı
* [İkinci bir depolama hesabı ne zaman kullanmalıyım?](#when-should-i-use-a-second-storage-account)
* [AOF Kalıcılık etkileyen boyunca, gecikme süresi veya önbelleğimin performansını mu?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [İkinci depolama hesabına nasıl kaldırabilir miyim?](#how-can-i-remove-the-second-storage-account)
* [Bir yeniden yazma nedir ve önbelleğimin nasıl etkiler?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Ne AOF etkin bir önbellek ölçeklendirme beklemeliyim?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [AOF verilerimi depolama nasıl düzenlendiği?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Önceden oluşturulmuş bir önbellek kalıcılığı etkinleştirebilirim?
Evet, Redis kalıcılığı önbellek oluşturma sırasında ve var olan premium önbellekler yapılandırılabilir.

### <a name="can-i-enable-aof-and-rdb-persistence-at-the-same-time"></a>Aynı anda AOF ve RDB Kalıcılık etkinleştirebilirim?

Hayır, yalnızca RDB veya AOF, ancak ikisini aynı anda etkinleştirebilirsiniz.

### <a name="which-persistence-model-should-i-choose"></a>Hangi Kalıcılık modeli seçmem gerekir?

AOF Kalıcılık kaydeder her yazma bazı performans etkisi, bir günlük için yapılandırılan yedekleme, performans üzerinde en az etki ile aralığında yedeklemeleri kaydedileceği RDB kalıcılığı ile karşılaştırılır. AOF Kalıcılık, veri kaybını en aza indirmek için birincil amacınız ise ve önbelleğiniz için bir düşüş aktarım hızını işleyebilir seçin. RDB Kalıcılık önbelleğinizi en iyi aktarım hızını korumak, ancak yine de veri kurtarma için bir mekanizma istiyor istiyorsanız seçin.

* Daha fazla bilgi edinin [avantajları](https://redis.io/topics/persistence#rdb-advantages) ve [dezavantajları](https://redis.io/topics/persistence#rdb-disadvantages) RDB Kalıcılık.
* Daha fazla bilgi edinin [avantajları](https://redis.io/topics/persistence#aof-advantages) ve [dezavantajları](https://redis.io/topics/persistence#aof-disadvantages) AOF Kalıcılık.

AOF Kalıcılık kullanırken performans hakkında daha fazla bilgi için bkz. [boyunca mu AOF Kalıcılık etkiler, gecikme süresi veya önbelleğimin performansını?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Farklı bir boyuta ölçeklendirilir ve ölçeklendirme işlemi önce yapılan bir yedekleme geri ne olur?

RDB hem AOF Kalıcılık için:

* Daha büyük bir boyuta ölçeklendirdiyseniz, herhangi bir etkisi yoktur.
* Daha küçük bir boyuta ölçeklendirilir ve sahip olduğunuz özel bir [veritabanları](cache-configure.md#databases) ayarı büyüktür [veritabanları sınırı](cache-configure.md#databases) yeni boyutunuz için verileri bu veritabanlarını geri. Daha fazla bilgi için [olan ölçeklendirme sırasında etkilenen ayarı my özel veritabanları?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Daha küçük bir boyuta ölçeklendirilir ve tüm son yedekleme veritabanından verileri tutmak için daha küçük boyutu yeterli yer yok, genellikle kullanarak geri yükleme işlemi sırasında anahtarları çıkarılacak [allkeys lru](https://redis.io/topics/lru-cache) çıkarma ilkesi.

### <a name="can-i-change-the-rdb-backup-frequency-after-i-create-the-cache"></a>RDB yedekleme sıklığı, önbellek oluşturabilirim sonra değiştirebilir miyim?
Evet, üzerinde RDB Kalıcılık için yedekleme sıklığını değiştirebilirsiniz **Redis veri kalıcılığı** dikey penceresi. Yapılandırma Redis kalıcılığı yönergeler için bkz.

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Neden RDB yedekleme sıklığı 60 dakika varsa var 60 dakikadan fazla yedeklemeler arasında?
RDB Kalıcılık yedekleme sıklığı aralığını önceki yedekleme işlemi başarıyla tamamlanana kadar başlatılmaz. Yedekleme sıklığı 60 dakikadır ve başarıyla tamamlanması 15 dakikada bir yedekleme işlemi alır, sonraki yedekleme başlangıç saati önceki yedeklemeden sonra 75 dakika kadar başlatılmaz.

### <a name="what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made"></a>Eski RDB yedekleri, yeni bir yedekleme yapıldığında ne olur?
En son dışındaki tüm RDB Kalıcılık yedeklemeleri otomatik olarak silinir. Bu silme işlemi hemen olmayabilir ancak daha eski yedeklemelerin süresiz olarak kalıcı yapılmaz.


### <a name="when-should-i-use-a-second-storage-account"></a>İkinci bir depolama hesabı ne zaman kullanmalıyım?

Önbellekteki beklenen ayarlama işlemleri daha yüksek olan düşündüğünüzde AOF Kalıcılık için ikinci bir depolama hesabı kullanmanız gerekir.  İkincil depolama hesabı ayarlama, özel olarak önbelleğinizin depolama bant genişliği sınırlarını ulaşmaz sağlamaya yardımcı olur.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>AOF Kalıcılık etkileyen boyunca, gecikme süresi veya önbelleğimin performansını mu?

AOF kalıcılığı önbellek en yüksek yük altında olduğunda bu aktarım hızı yaklaşık % 15-20 oranında etkiler (CPU ve sunucu hem de yük altında %90). Olmamalıdır gecikme sorunlarını önbellek bu sınırlar içinde olduğunda. Ancak, önbellek limitler daha çabuk ile AOF etkin ulaşacak.

### <a name="how-can-i-remove-the-second-storage-account"></a>İkinci depolama hesabına nasıl kaldırabilir miyim?

İlk Depolama hesabı ile aynı olacak şekilde ikinci depolama hesabı ayarlayarak AOF Kalıcılık ikincil depolama hesabı kaldırabilirsiniz. Yönergeler için [yapılandırma AOF Kalıcılık](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Bir yeniden yazma nedir ve önbelleğimin nasıl etkiler?

AOF dosya yeterince büyük olduğunda, bir yeniden yazma önbelleği üzerine otomatik olarak kuyruğa alınır. Yeniden yazma işlemleri geçerli veri kümesi oluşturmak için gereken en az sayıda AOF dosyasıyla yeniden boyutlandırır. Yeniden yazma işlemleri sırasında özellikle büyük veri kümeleriyle ilgilenirken daha çabuk performans sınırlarını ulaşmak bekler. Genellikle AOF dosyanın büyük hale gelir, ancak ne zaman olacağı süreyi önemli ölçüde olacak şekilde yeniden daha az oluşur.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Ne AOF etkin bir önbellek ölçeklendirme beklemeliyim?

AOF dosyanın ölçeklendirmenin zaman önemli ölçüde büyükse, ardından ölçeklendirme işlemi, ölçeklendirme tamamlandıktan sonra dosyayı yeniden beri beklenenden daha uzun sürmesine bekler.

Ölçeklendirme hakkında daha fazla bilgi için bkz. [farklı bir boyuta ölçeklendirilir ve ölçeklendirme işlemi önce yapılan bir yedekleme geri ne olur?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>AOF verilerimi depolama nasıl düzenlendiği?

AOF dosyalarında depolanan veriler, veri depolamaya kaydetme performansını artırmak için düğüm başına birden çok sayfa blobları ayrılmıştır. Aşağıdaki tabloda, kaç sayfa BLOB'ları için her fiyatlandırma katmanının kullanılan görüntüler:

| Premium katman | Bloblar |
|--------------|-------|
| P1           | parça başına 4    |
| P2           | parça başına 8    |
| P3           | parça başına 16   |
| P4           | parça başına 20   |

Kümeleme etkin olduğunda, her parça önbelleğinde önceki tabloda belirtildiği şekilde kendi sayfa BLOB'ları kümesi vardır. Örneğin, üç parça P2 önbellekle 24 sayfa blobları (3 parçalar ile bir parça başına 8 blobları) arasında AOF dosyası dağıtır.

Sonra bir yeniden yazma, AOF dosyalarını iki depoda yok. Yeniden yazdırmaya, arka planda gerçekleşir ve ikinci kümeye ekleme sırasında yeniden yazma önbelleği için gönderilen kümesi işlemleri sırasında dosyaları ilk kümesine ekleyin. Bir yedekleme başarısız olması durumunda yeniden yazma işlemleri sırasında geçici olarak depolanır, ancak bir yeniden yazma tamamlandıktan sonra hemen silinir.


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerini kullanmayı öğrenin.

* [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
