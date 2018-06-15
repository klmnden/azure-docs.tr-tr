---
title: Azure Redis önbelleği için coğrafi çoğaltma yapılandırma | Microsoft Docs
description: Coğrafi bölgeler arasında Azure Redis önbelleği örnekleri çoğaltmak öğrenin.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: wesmc
ms.openlocfilehash: 883683f6af7943fa4da49095c9a15aefd5cfa719
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27911379"
---
# <a name="how-to-configure-geo-replication-for-azure-redis-cache"></a>Azure Redis önbelleği için coğrafi çoğaltma yapılandırma

Coğrafi çoğaltma iki Premium katmanı Azure Redis önbelleği örnekleri bağlama için bir mekanizma sağlar. Bir önbellek birincil bağlantılı önbellek ve diğer ikincil bağlantılı önbelleği olarak atanır. İkincil bağlantılı önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlantılı önbelleğine çoğaltılır. Bu işlev, Azure bölgeler arasında bir önbellek çoğaltmak için kullanılabilir. Bu makale, Premium katmanı Azure Redis önbelleği örnekleri için coğrafi çoğaltma yapılandırmak için bir kılavuz sağlar.

## <a name="geo-replication-prerequisites"></a>Coğrafi çoğaltma önkoşulları

Coğrafi çoğaltma arasında iki önbellekleri yapılandırmak için aşağıdaki gereksinimleri karşılamanız gerekir:

- Her iki önbellekleri olmalıdır [Premium katmanı](cache-premium-tier-intro.md) önbelleğe alır.
- Her iki önbellekleri aynı Azure aboneliğindeki olması gerekir.
- İkincil bağlantılı önbellek aynı fiyatlandırma katmanı veya birincil bağlantılı önbellek daha büyük bir fiyatlandırma katmanı olmalıdır.
- Birincil bağlantılı önbellek kümeleme özelliği etkinleştirilmiş varsa, ikincil bağlantılı önbellek kümeleme birincil bağlantılı önbellek aynı sayıda parça ile özelliği etkinleştirilmiş olması gerekir.
- Her iki önbellekleri oluşturulmalıdır ve çalışır durumda.
- Kalıcılık ya da önbellek etkinleştirilmemelidir.
- Coğrafi çoğaltma aynı sanal ağda önbellekleri arasında desteklenir. İki Vnet Vnet'lerdeki kaynaklar birbirine yoluyla TCP bağlantısı ulaşabilmesi biçimde yapılandırılmış olduğu sürece coğrafi çoğaltma farklı vnet'lerdeki önbellekleri arasında da desteklenir.

Coğrafi çoğaltma yapılandırıldıktan sonra bağlantılı önbellek çifti aşağıdaki kısıtlamalar geçerlidir:

- İkincil bağlantılı önbellek salt okunurdur; okuyabilir, ancak herhangi bir veri yazamıyor. 
- Bağlantıyı eklenmeden önce ikincil bağlantılı önbelleğinde olan tüm veriler kaldırılır. Coğrafi çoğaltma sonradan ancak kaldırılırsa, çoğaltılan veriler ikincil bağlantılı önbellekte kalır.
- İşlemi başlatamaz bir [işlemi ölçeklendirme](cache-how-to-scale.md) ya da önbellekteki veya [parça sayısını değiştirme](cache-how-to-premium-clustering.md) önbellek kümeleme özelliği etkinleştirilmiş sahipse.
- Kalıcılık ya da önbellekteki etkinleştiremezsiniz.
- Kullanabileceğiniz [dışarı](cache-how-to-import-export-data.md#export) ya da önbelleğiyle ancak yalnızca [alma](cache-how-to-import-export-data.md#import) birincil bağlantılı önbelleğine.
- Bağlantılı önbellek veya coğrafi çoğaltma bağlantısı kaldırana kadar bunları içeren kaynak grubu silinemiyor. Daha fazla bilgi için bkz: [neden işlem başarısız my bağlantılı önbelleğini silmek çalışırken?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- İki önbellekleri farklı bölgelerde bulunuyorsa, ağ çıkışı maliyeti ikincil bağlantılı önbelleğe bölgeler arasında çoğaltılan verileri uygulanır. Daha fazla bilgi için bkz: [nasıl maliyeti Azure bölgeler arasında verilerimi çoğaltmak için?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Birincil önbelleği (ve onun çoğaltması) kesilirse hiç otomatik yük devretme ikincil bağlantılı önbelleğe yoktur. Yük devretme istemci uygulamaları için sırayla el ile coğrafi çoğaltma bağlantısını kaldırmak ve istemci uygulamaları önceden ikincil bağlantılı önbellek olan önbelleğe noktası gerekir. Daha fazla bilgi için bkz: [nasıl yapabilmesini ikincil bağlantılı önbelleğe çalışır?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısı ekleme

1. Coğrafi çoğaltma için iki premium önbelleklere birbirine bağlamak için tıklatın **coğrafi çoğaltma** bağlı birincil olarak hedeflenen önbelleğin kaynak menüsünden önbelleğe ve ardından **Ekle önbellek çoğaltma bağlantısı** gelen **coğrafi çoğaltma** dikey.

    ![Bağlantı Ekle](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. İstenen ikincil önbellekten adına tıklayın **uyumlu önbellekleri** listesi. İstenen önbelleğiniz listede görüntülenmiyorsa, doğrulayın [coğrafi çoğaltma Önkoşullar](#geo-replication-prerequisites) istenen ikincil önbelleğine karşılanır. Önbellekleri bölgeye göre filtre uygulamak için yalnızca önbelleklere görüntülemek eşlemek istediğiniz bölgede tıklatın **uyumlu önbellekleri** listesi.

    ![Coğrafi çoğaltma uyumlu önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Ayrıca, bağlama işlemini başlatmak veya bağlam menüsünü kullanarak ikincil önbelleği hakkında ayrıntıları görüntüleyin.

    ![Coğrafi çoğaltma bağlam menüsü](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Tıklatın **bağlantı** iki önbellekleri birbirine bağlamak ve çoğaltma işlemine başlamak için.

    ![Bağlantı önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Çoğaltma işleminin ilerleme durumunu görüntüleyebilirsiniz **coğrafi çoğaltma** dikey.

    ![Bağlantı durumu](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Üzerinde bağlantı durumunu görüntüleyebilirsiniz **genel bakış** dikey birincil ve ikincil önbellekler için.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Çoğaltma işlemi tamamlandıktan sonra **bağlantı durumu** değişikliklerini **başarılı**.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Bağlama işlemi sırasında birincil bağlantılı önbellek kullanılabilir kalır, ancak bağlama işlemi tamamlanana kadar ikincil bağlantılı önbellek kullanılabilir değil.

## <a name="remove-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısını Kaldır

1. İki önbellekleri arasındaki bağlantıyı kaldırın ve coğrafi çoğaltma durdurmak için tıklatın **bağlantısını önbellekleri** gelen **coğrafi çoğaltma** dikey.
    
    ![Önbellek bağlantılarını kaldır](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Bağlantı kaldırma işlemi tamamlandıktan sonra ikincil Önbellek Okuma ve yazma işlemleri için kullanılabilir.

>[!NOTE]
>Coğrafi çoğaltma bağlantısı kaldırıldığında, çoğaltılan verilerin birincil bağlantılı önbellekten ikincil önbellekte kalır.
>
>

## <a name="geo-replication-faq"></a>Coğrafi çoğaltma ile ilgili SSS

- [Coğrafi çoğaltma ile standart ya da temel katmanı önbellek kullanabilir miyim?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [My önbellek bağlantı veya bağlantı kaldırma işlemi sırasında kullanılabilir mi?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [İkiden fazla önbellekleri birlikte bağlayabilir miyim?](#can-i-link-more-than-two-caches-together)
- [Farklı Azure aboneliklerinden iki önbellekleri bağlayabilir miyim?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Farklı boyutlarda iki önbelleklerle bağlayabilir miyim?](#can-i-link-two-caches-with-different-sizes)
- [Coğrafi çoğaltma kümeleme özelliği etkinleştirilmiş kullanabilir miyim?](#can-i-use-geo-replication-with-clustering-enabled)
- [Bir sanal ağda my önbellekleri ile coğrafi çoğaltma kullanabilir miyim?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Redis coğrafi çoğaltma için çoğaltma zamanlamasını nedir?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [Coğrafi çoğaltma çoğaltma ne kadar sürer?](#how-long-does-geo-replication-replication-take)
- [Çoğaltma kurtarma noktası sağlanır?](#is-the-replication-recovery-point-guaranteed)
- [Coğrafi çoğaltma yönetmek için PowerShell veya Azure CLI kullanabilir miyim?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Ne kadar verilerimi Azure bölgeler arasında çoğaltma maliyet?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [My bağlantılı önbelleğini silmek çalışırken neden işlemi başarısız oldu?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [My ikincil bağlantılı önbelleği için hangi bölge kullanmalıyım?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [İkincil bağlantılı önbelleğe yapabilmesini nasıl çalışır?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Coğrafi çoğaltma ile standart ya da temel katmanı önbellek kullanabilir miyim?

Coğrafi çoğaltma Hayır, yalnızca Premium katmanı önbellekler için olarak kullanılabilir.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>My önbellek bağlantı veya bağlantı kaldırma işlemi sırasında kullanılabilir mi?

- İki önbellekleri coğrafi çoğaltma için birlikte bağlanırken birincil bağlantılı önbellek kullanılabilir kalır, ancak bağlama işlemi tamamlanana kadar ikincil bağlantılı önbellek kullanılabilir değil.
- İki önbellekleri arasında coğrafi çoğaltma bağlantı kaldırırken, her iki önbellekleri kullanılabilir kalır.

### <a name="can-i-link-more-than-two-caches-together"></a>İkiden fazla önbellekleri birlikte bağlayabilir miyim?

Hayır, coğrafi çoğaltma kullanırken, yalnızca iki önbellekleri birbirine bağlayabilirsiniz.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Farklı Azure aboneliklerinden iki önbellekleri bağlayabilir miyim?

Hayır, her iki önbellekleri aynı Azure aboneliğindeki olması gerekir.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Farklı boyutlarda iki önbelleklerle bağlayabilir miyim?

Evet, ikincil bağlı olduğu sürece önbellek birincil bağlantılı önbellek büyüktür.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Coğrafi çoğaltma kümeleme özelliği etkinleştirilmiş kullanabilir miyim?

Evet, hem önbellekleri parça aynı sayıda olduğu sürece.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Bir sanal ağda my önbellekleri ile coğrafi çoğaltma kullanabilir miyim?

Evet, coğrafi çoğaltma vnet'lerdeki önbelleklerinin desteklenir. 

- Coğrafi çoğaltma aynı sanal ağda önbellekleri arasında desteklenir.
- İki Vnet Vnet'lerdeki kaynaklar birbirine yoluyla TCP bağlantısı ulaşabilmesi biçimde yapılandırılmış olduğu sürece coğrafi çoğaltma farklı vnet'lerdeki önbellekleri arasında da desteklenir.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Redis coğrafi çoğaltma için çoğaltma zamanlamasını nedir?

Çoğaltma belirli bir zamanlamada gerçekleşmez sürekli ve zaman uyumsuz yani olduğu birincil için yapılan tüm yazma işlemlerini ikincil eşzamanlı olarak zaman uyumsuz olarak çoğaltılır.

### <a name="how-long-does-geo-replication-replication-take"></a>Coğrafi çoğaltma çoğaltma ne kadar sürer?

Artımlı, zaman uyumsuz ve sürekli çoğaltma ve geçen süre genellikle bölgeler arasında gecikme çok farklı değildir. Belirli koşullar altında belirli zamanlarda birincil sunucudan verilerin tam bir eşitleme yapmak için ikincil gerekebilir. Çoğaltma süre bu durumda söz numarası gibi etkenlere bağlıdır: birincil önbelleği, önbellek makinede kullanılabilir bant genişliği üzerindeki yükü bölge gecikme vb. ağlar arası. Örneğin, bazı çoğaltma bundan tam için kullanıma bulduk testlere göre 53 GB'a coğrafi olarak çoğaltılmış eşleştirin Doğu'BİZE ve Batı ABD bölgeler herhangi bir yere 5-10 dakika arasında olabilir.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Çoğaltma kurtarma noktası sağlanır?

Şu anda, coğrafi olarak çoğaltılmış bir modda önbellekler için kalıcılığını ve içeri/dışarı aktarma işlevi devre dışı bırakıldı. Burada çoğaltma bağlantısı açıldı coğrafi olarak çoğaltılmış çifti arasında bozuk durumda veya bir müşteri tarafından başlatılan yük devretme durumunda ikincil bellek korumak için birincil sunucudan'in eşitlenmiş olarak o zaman noktasına kadar veri. Bu gibi durumlarda sağlanan kurtarma noktası garantisi yoktur.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Coğrafi çoğaltma yönetmek için PowerShell veya Azure CLI kullanabilir miyim?

Şu anda yalnızca coğrafi çoğaltma Azure portalını kullanarak yönetebilirsiniz.

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Ne kadar verilerimi Azure bölgeler arasında çoğaltma maliyet?

Coğrafi çoğaltma kullanırken, birincil bağlantılı önbellekten veri ikincil bağlantılı önbelleğine çoğaltılır. İki bağlı önbellekleri aynı Azure bölgesinde varsa, veri aktarımı için ücret ödemeden yoktur. İki bağlı önbellekleri farklı Azure bölgelerinde varsa, coğrafi çoğaltma veri aktarımı ücretsiz olarak bu verileri başka bir Azure bölgesine çoğaltma bant genişliği maliyetidir. Daha fazla bilgi için bkz: [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>My bağlantılı önbelleğini silmek çalışırken neden işlemi başarısız oldu?

İki önbellekleri birbirine bağlı olduğunda, önbellek veya coğrafi çoğaltma bağlantısı kaldırana kadar bunları içeren kaynak grubu silinemiyor. Birine veya ikisine de bağlı önbellekleri içeren kaynak grubunu silme girişimi, kaynak grubundaki diğer kaynaklar silinir, ancak kaynak grubu içinde kalır `deleting` durum ve her bağlantılı önbellekleri kaynak grubunda kalır `running` durumu. Kaynak grubu ve içerdiği bağlantılı önbellekleri silme işlemini tamamlamak için coğrafi çoğaltma açıklandığı gibi bağı [coğrafi çoğaltma bağlantısını kaldırmak](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>My ikincil bağlantılı önbelleği için hangi bölge kullanmalıyım?

Genel olarak, aynı Azure bölgesinde eriştiği uygulama olarak mevcut bir önbellek hesabınız için önerilir. Birincil ve geri dönüş bölge uygulamanız varsa, birincil ve ikincil önbellekleri bu aynı bölgede var olmalıdır. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz: [en iyi uygulamaları – Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>İkincil bağlantılı önbelleğe yapabilmesini nasıl çalışır?

Coğrafi çoğaltma ilk sürümünde, Azure Redis önbelleği Azure bölgeler arasında otomatik yük devretme desteklemez. Coğrafi çoğaltma öncelikle bir olağanüstü durum kurtarma senaryosunda kullanılır. Distater kurtarma senaryosunda, müşteriler yedekleme bir bölgedeki tüm uygulama yığınını tek tek uygulama bileşenleri, yedeklerini kendi başlarına geçmek karar vermenize izin vererek yerine koordineli bir şekilde getirecek. Bu, özellikle Redis için uygundur. Redis başlıca avantajlarından biri, çok düşük gecikme süreli depolama olmasıdır. Farklı bir Azure bölgesine bir uygulama tarafından kullanılan Redis yöneltilir ancak işlem katmanı yok, eklenen gidiş dönüş süresi performans üzerinde belirgin bir etkisi yoktur. Bu nedenle, Redis başarısız üzerinden otomatik olarak geçici kullanılabilirlik sorunlarından kaçınmak isteriz.

Şu anda, yük devretme başlatmak için Azure portalında coğrafi çoğaltma bağlantısını kaldırmak ve ardından bağlantı uç noktası Redis istemci birincil bağlantılı önbellekten (önceki adıyla bağlantılı) İkincil Önbelleğe değiştirmeniz gerekir. İki önbellekleri ilişkilendirmesi eklediğinizde, çoğaltma normal bir okuma-yazma önbelleği yeniden olur ve Redis istemcileri doğrudan gelen istekleri kabul eder.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [Azure Redis önbelleği Premium katmanına](cache-premium-tier-intro.md).

