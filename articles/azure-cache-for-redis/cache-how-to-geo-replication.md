---
title: Coğrafi çoğaltma, Azure önbelleği için Redis için yapılandırma | Microsoft Docs
description: Azure önbelleği için Redis örneği coğrafi bölgeler arasında çoğaltmak öğrenin.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: yegu
ms.openlocfilehash: 383ea07005d7dae47cd0ef1da8a4a57d8b20d613
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57435822"
---
# <a name="how-to-configure-geo-replication-for-azure-cache-for-redis"></a>Coğrafi çoğaltma, Azure önbelleği için Redis için yapılandırma

Coğrafi çoğaltma, iki Premium katmanı Azure Cache Redis örneği için bağlama için bir mekanizma sağlar. Bir önbelleği birincil bağlı önbellek ve diğer bağlı ikincil önbellek olarak atanır. Bağlı ikincil önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlı önbellek için çoğaltılır. Bu işlev, Azure bölgeleri arasında bir önbellek çoğaltmak için kullanılabilir. Bu makalede, Azure Cache Premium katmanı Redis örneği için coğrafi çoğaltmayı yapılandırma için bir kılavuz sağlar.

## <a name="geo-replication-prerequisites"></a>Coğrafi çoğaltma önkoşulları

İki önbellekler arasında coğrafi çoğaltmayı yapılandırmak için aşağıdaki önkoşullar karşılanmalıdır:

- Her iki önbellekler olmalıdır [Premium katmanı](cache-premium-tier-intro.md) önbelleğe alır.
- Her iki önbellekler, aynı Azure aboneliğinde olması gerekir.
- Bağlı ikincil önbellek aynı fiyatlandırma katmanını ya da birincil bağlı önbellek değerinden daha büyük bir fiyatlandırma katmanı olması gerekir.
- Birincil bağlı önbellek kümeleme özelliği etkinleştirildiğinde varsa, ikincil bağlı önbellek kümeleme birincil bağlı önbellek aynı sayıda parça ile özelliği etkinleştirilmiş olması gerekir.
- Her iki önbellekler oluşturulması gerekir ve çalışır durumda.
- Kalıcılık ya da önbelleği etkin olmamalıdır.
- Aynı vnet'teki önbellekler arasında coğrafi çoğaltma desteklenir. 
- Aynı bölgedeki eşlenmiş sanal ağlardaki önbellekler arasında coğrafi çoğaltma şu anda bir önizleme özelliğidir. İki sanal ağ vnet'lerdeki kaynaklar birbirine TCP bağlantıları ulaşabilir şekilde yapılandırılması gerekir.
- Eşlenmiş sanal ağlar farklı bölgelerde önbelleklerinde arasında coğrafi çoğaltma, henüz desteklenmiyor, ancak Önizleme Yakında sunulacak.

Coğrafi çoğaltma yapılandırdıktan sonra bağlı önbellek çiftinizi aşağıdaki kısıtlamalar uygulanır:

- Bağlı ikincil önbellek salt okunur; Buradan okuyabilirsiniz, ancak herhangi bir veri yazamaz. 
- Bağlantıyı eklenmeden önce ikincil bağlı önbellekte olan tüm veriler kaldırılır. Coğrafi çoğaltma sonradan ancak kaldırılırsa, çoğaltılan veriler ikincil bağlı önbellekte kalır.
- Başlatamazsınız bir [ölçeklendirme işlemi](cache-how-to-scale.md) ya da önbellekteki veya [parça sayısını değiştirmek](cache-how-to-premium-clustering.md) önbellek kümeleme özelliği etkinleştirildiğinde varsa.
- Her iki önbellek kalıcılığı etkinleştirilemiyor.
- Kullanabileceğiniz [dışarı](cache-how-to-import-export-data.md#export) ya da önbellek ile ancak yalnızca [içeri aktarma](cache-how-to-import-export-data.md#import) birincil bağlı önbelleğine.
- Bağlı önbellek veya coğrafi çoğaltma bağlantısı kaldırana kadar bunları içeren kaynak grubu silinemiyor. Daha fazla bilgi için [neden işlem başarısız bağlı önbelleğimin silmeye çalışırken?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- İki önbellekleri farklı bölgelerde bulunuyorsa, ağ çıkışı maliyeti bağlı ikincil önbellek için bölgede çoğaltılan verilere uygulanır. Daha fazla bilgi için [ücreti ne kadardır verilerimi Azure bölgeleri arasında çoğaltılmasını?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Birincil önbellek (ve onun çoğaltması) kesilirse hiçbir otomatik yük devretme ikincil bağlı önbellek için yoktur. Yük devretme istemci uygulamaları için sırayla el ile coğrafi çoğaltma bağlantısını kaldırmak ve istemci uygulamaları, eski ikincil bağlı önbellek önbelleğe noktası gerekir. Daha fazla bilgi için [nasıl yük devretme ikincil bağlı önbellek çalışır?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısı Ekle

1. İki premium önbellekler için coğrafi çoğaltmayı birbirine bağlamak için tıklayın **coğrafi çoğaltma** bağlı birincil hedeflenen önbelleğin kaynak menüsünden önbellek ve ardından **Ekle önbellek çoğaltma bağlantısı** gelen **coğrafi çoğaltma** dikey penceresi.

    ![Bağlantı Ekle](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. İstenen ikincil önbellekten adına **uyumlu önbellekler** listesi. İstenen önbelleğinizi listede görüntülenmiyorsa, doğrulayın [coğrafi çoğaltma önkoşulları](#geo-replication-prerequisites) istediğiniz ikincil önbellek için karşılanır. Haritanın önbelleklere yalnızca istenen bölgede önbellekleri bölgeye göre filtrelemek için tıklayın **uyumlu önbellekler** listesi.

    ![Coğrafi çoğaltma uyumlu önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Bağlama işlemini başlatmak veya bağlam menüsünü kullanarak ikincil önbellek hakkında ayrıntıları görüntüleyin.

    ![Coğrafi çoğaltma bağlam menüsü](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Tıklayın **bağlantı** iki önbellekleri birbirine bağlamak ve çoğaltma işlemini başlatmak için.

    ![Bağlantı önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Çoğaltma işleminin ilerleme durumunu görüntüleyebileceğiniz **coğrafi çoğaltma** dikey penceresi.

    ![Bağlantı durumu](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Üzerinde bağlantı durumunu görüntüleyebilirsiniz **genel bakış** birincil ve ikincil önbellek için dikey pencere.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Çoğaltma işlemi tamamlandıktan sonra **bağlantı durumu** değişikliklerini **başarılı**.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Bağlama işlemi sırasında birincil bağlı önbellek kullanılabilir kalır, ancak ikincil bağlı önbellek bağlama işlemi tamamlanana kadar kullanılamaz.

## <a name="remove-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısını Kaldır

1. İki önbellekler arasındaki bağlantıyı kaldırın ve coğrafi çoğaltma durdurmak için tıklatın **önbelleklerinin bağlantısını** gelen **coğrafi çoğaltma** dikey penceresi.
    
    ![Önbellek bağlantılarını kaldır](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Bağlantı kaldırma işlemi tamamlandıktan sonra ikincil Önbellek Okuma ve yazma işlemleri için kullanılabilir.

>[!NOTE]
>Coğrafi çoğaltma bağlantısı kaldırıldığında, birincil bağlı önbellek çoğaltılan verilerini ikincil önbellekte kalır.
>
>

## <a name="geo-replication-faq"></a>Coğrafi çoğaltma ile ilgili SSS

- [Coğrafi çoğaltma standart veya temel katmanı önbelleği ile kullanabilir miyim?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Önbelleğimin bağlantı veya bağlantı kaldırma işlemi sırasında kullanılabilir durumda?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [İkiden fazla önbellekler birlikte bağlayabilir miyim?](#can-i-link-more-than-two-caches-together)
- [Ben, iki farklı Azure aboneliklerinde önbelleklerden bağlayabilir miyim?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Ben, iki farklı boyutlarda önbelleklerle bağlayabilir miyim?](#can-i-link-two-caches-with-different-sizes)
- [Coğrafi çoğaltma Etkin kümeleme ile kullanabilir miyim?](#can-i-use-geo-replication-with-clustering-enabled)
- [Bir vnet'teki benim önbellek ile coğrafi çoğaltma kullanabilir miyim?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Redis coğrafi çoğaltma için çoğaltma zamanlamasını nedir?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [Coğrafi çoğaltma çoğaltma ne kadar sürer?](#how-long-does-geo-replication-replication-take)
- [Çoğaltma kurtarma noktası sağlanır?](#is-the-replication-recovery-point-guaranteed)
- [Coğrafi çoğaltmayı yönetmek için PowerShell veya Azure CLI'yı kullanabilir miyim?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Bu benim verilerimi Azure bölgeleri arasında çoğaltılmasını nin ücreti ne kadardır?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Neden bağlı önbelleğimin silmeye çalıştığınızda işlem başarısız oldu?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [İkinci bağlantılı önbelleğimin için hangi bölgeyi kullanmalıyım?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Yük devretme ikincil bağlı önbellek nasıl çalışır?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Coğrafi çoğaltma standart veya temel katmanı önbelleği ile kullanabilir miyim?

Coğrafi çoğaltma Hayır, yalnızca Premium katmanı önbellek olarak kullanılabilir.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>Önbelleğimin bağlantı veya bağlantı kaldırma işlemi sırasında kullanılabilir durumda?

- İki önbellekler için coğrafi çoğaltmayı birlikte bağlanırken, birincil bağlı önbellek kullanılabilir kalır, ancak ikincil bağlı önbellek bağlama işlemi tamamlanana kadar kullanılamaz.
- İki önbellekler arasında coğrafi çoğaltma bağlantısı kaldırırken, her iki önbellekler kullanılabilir kalır.

### <a name="can-i-link-more-than-two-caches-together"></a>İkiden fazla önbellekler birlikte bağlayabilir miyim?

Hayır, coğrafi çoğaltma kullanırken yalnızca iki önbellekler birbirine bağlayabilirsiniz.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Ben, iki farklı Azure aboneliklerinde önbelleklerden bağlayabilir miyim?

Hayır, her iki önbellekler, aynı Azure aboneliğinde olması gerekir.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Ben, iki farklı boyutlarda önbelleklerle bağlayabilir miyim?

İkincil bağlı önbellek birincil bağlı önbellek daha büyük olduğu sürece Evet.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Coğrafi çoğaltma Etkin kümeleme ile kullanabilir miyim?

Her iki önbellekler aynı parça sayısı olduğu sürece Evet.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Bir vnet'teki benim önbellek ile coğrafi çoğaltma kullanabilir miyim?

Evet, sanal ağlar önbelleklerinde coğrafi çoğaltma desteklenir. 

- Aynı vnet'teki önbellekler arasında coğrafi çoğaltma desteklenir.
- İki sanal ağ vnet'lerdeki kaynaklar birbirine TCP bağlantıları ulaşabilir şekilde yapılandırılmış olduğu sürece farklı sanal ağlardaki önbellekler arasında coğrafi çoğaltma ayrıca desteklenir.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Redis coğrafi çoğaltma için çoğaltma zamanlamasını nedir?

Çoğaltma, belirli bir zamanlamaya göre gerçekleşmez sürekli ve zaman uyumsuz yani olduğu Birincil siteye yapılan tüm yazma işlemlerini ikincil anında zaman uyumsuz olarak çoğaltılır.

### <a name="how-long-does-geo-replication-replication-take"></a>Coğrafi çoğaltma çoğaltma ne kadar sürer?

Artımlı, zaman uyumsuz ve sürekli çoğaltma ve geçen süreyi genellikle bölgeler arasında gecikme süresi ' çok farklı değildir. Belirli koşullar altında belirli zamanlarında birincil verilerin tam bir eşitleme yapmak için ikincil gerekebilir. Çoğaltma süre bu durumda söz gibi bir dizi etkene bağlıdır: bölge gecikme vb. birincil önbellek, önbellek makinede kullanılabilir bant genişliği üzerindeki yükü arası. Örneğin, bazı Bu çoğaltma süresi bir tam çıkış bulduk teste dayanan 53 GB coğrafi olarak çoğaltılmış eşleştirin Doğu ABD ve Batı ABD bölgelerinde herhangi bir yere 5-10 dakika arasında olabilir.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Çoğaltma kurtarma noktası sağlanır?

Şu anda, coğrafi olarak çoğaltılmış bir modda önbellekler için Kalıcılık ve içeri/dışarı aktarma işlevi devre dışı bırakıldı. Burada bir çoğaltma bağlantısının coğrafi olarak çoğaltılmış çifti bozuk durumda veya bir müşteri tarafından başlatılan yük devretme durumunda ikincil bellek içi korur, böylece bu birincil düğümden'in eşitlenmiş olarak zaman o noktaya kadar veri. Bu gibi durumlarda sağlanan kurtarma noktası garantisi yoktur.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Coğrafi çoğaltmayı yönetmek için PowerShell veya Azure CLI'yı kullanabilir miyim?

Şu anda Azure portalını kullanarak coğrafi çoğaltma yalnızca yönetebilirsiniz.

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Bu benim verilerimi Azure bölgeleri arasında çoğaltılmasını nin ücreti ne kadardır?

Coğrafi çoğaltma kullanırken birincil bağlı önbellek verilerini ikincil bağlı önbellek için çoğaltılır. İki bağlı önbellekleri aynı Azure bölgesinde, veri aktarımı için ücret alınmaz. İki bağlı önbellekleri farklı Azure bölgelerinde olduğunda, coğrafi çoğaltma veri aktarımı ücreti, verileri başka bir Azure bölgesine çoğaltmak bant genişliği maliyeti olur. Daha fazla bilgi için [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Neden bağlı önbelleğimin silmeye çalıştığınızda işlem başarısız oldu?

İki önbellekler birbirine bağlı, önbellek ya da coğrafi çoğaltma bağlantısı kaldırana kadar bunları içeren kaynak grubunu silinemiyor. Birine veya ikisine de bağlı önbellekleri içeren kaynak grubunu silmeye çalışıyorsanız, kaynak grubundaki diğer kaynaklar silinir, ancak kaynak grubu içinde kalır `deleting` durum ve tüm bağlı önbellekler kaynak grubunda kalması içinde `running`durumu. Kaynak grubunu ve içerdiği bağlı önbellekleri silme işlemini tamamlamak için coğrafi çoğaltma açıklandığı bağı [coğrafi çoğaltma bağlantısını kaldırmak](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>İkinci bağlantılı önbelleğimin için hangi bölgeyi kullanmalıyım?

Genel olarak, eriştiği uygulama ile aynı Azure bölgesinde mevcut bir önbellek hesabınız için önerilir. Birincil ve geri dönüş bölge uygulamanız varsa, birincil ve ikincil önbelleklerinizi bu aynı bölgede olması gerekir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [en iyi uygulamaları – Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>Yük devretme ikincil bağlı önbellek nasıl çalışır?

Coğrafi çoğaltma ilk sürümünde, Azure önbelleği için Redis Azure bölgeleri arasında otomatik yük devretmeyi desteklemez. Coğrafi çoğaltma, öncelikle bir olağanüstü durum kurtarma senaryosunda kullanılır. Bir olağanüstü durum kurtarma senaryosunda müşteriler yedekleme bölgesindeki tüm uygulama yığınını tek tek uygulama bileşenleri, yedeklerini kendi geçmek ne zaman karar izin vermek yerine koordineli bir şekilde getirecek. Bu, özellikle Redis için geçerlidir. Redis başlıca avantajlarından biri, çok düşük gecikmeli depolama olmasıdır. Farklı bir Azure bölgesinde bir uygulama tarafından kullanılan Redis devreder ancak bilgi işlem katmanı yok, eklenen gidiş dönüş süresi performans üzerinde fark edilebilir etkisi gerekir. Bu nedenle, Redis başarısız üzerinden otomatik olarak nedeniyle geçici kullanılabilirlik sorunları önlemek istiyoruz.

Şu anda yük devretmeyi başlatmak için Azure portalında coğrafi çoğaltma bağlantısını kaldırmak ve bir Redis istemci bağlantı uç noktası (eski adıyla bağlı) ikincil önbellek için birincil bağlı önbellekten değiştirmenizi gerekir. İki önbellekleri ilişkisi, çoğaltma normal bir okuma-yazma önbelleği yeniden haline gelir ve Redis istemcileri doğrudan gelen istekleri kabul eder.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure önbelleği için Redis Premium katmanı](cache-premium-tier-intro.md).

