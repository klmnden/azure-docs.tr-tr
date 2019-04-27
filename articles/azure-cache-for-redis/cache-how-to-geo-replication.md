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
ms.date: 03/06/2019
ms.author: yegu
ms.openlocfilehash: 4254175955c3560c7bd0fdd08c6b60c318238b76
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60552390"
---
# <a name="how-to-configure-geo-replication-for-azure-cache-for-redis"></a>Coğrafi çoğaltma, Azure önbelleği için Redis için yapılandırma

Coğrafi çoğaltma, iki Premium katmanı Azure Cache Redis örneği için bağlama için bir mekanizma sağlar. Bir önbelleği birincil bağlı önbellek ve diğer bağlı ikincil önbellek olarak seçilir. Bağlı ikincil önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlı önbellek için çoğaltılır. Bu işlev, Azure bölgeleri arasında bir önbellek çoğaltmak için kullanılabilir. Bu makalede, Azure Cache Premium katmanı Redis örneği için coğrafi çoğaltmayı yapılandırma için bir kılavuz sağlar.

## <a name="geo-replication-prerequisites"></a>Coğrafi çoğaltma önkoşulları

İki önbellekler arasında coğrafi çoğaltmayı yapılandırmak için aşağıdaki önkoşullar karşılanmalıdır:

- Her iki önbellekler [Premium katmanı](cache-premium-tier-intro.md) önbelleğe alır.
- Her iki önbellekler aynı Azure aboneliğinde ise.
- Bağlı ikincil önbellek aynı önbellek boyutunu ya da birincil bağlı önbellek daha büyük bir önbellek boyutu ' dir.
- Her iki önbellekler oluşturulur ve çalışır durumda.

Bazı özellikler, coğrafi çoğaltma desteklenmez:

- Kalıcılık coğrafi çoğaltma desteklenmez.
- Her iki önbellekler kümeleme özelliği etkinleştirildiğinde varsa ve aynı parça sayısı Kümelemesi desteklenir.
- Aynı vnet'teki önbellekler desteklenir.
- Farklı sanal ağlardaki önbellekler, uyarılar ile desteklenir. Bkz: [coğrafi çoğaltma bir vnet'teki benim önbellek ile kullanabilir miyim?](#can-i-use-geo-replication-with-my-caches-in-a-vnet) daha fazla bilgi için.

Coğrafi çoğaltma yapılandırdıktan sonra bağlı önbellek çiftinizi aşağıdaki kısıtlamalar uygulanır:

- Bağlı ikincil önbellek salt okunur; Buradan okuyabilirsiniz, ancak herhangi bir veri yazamaz. 
- Bağlantıyı eklenmeden önce ikincil bağlı önbellekte olan tüm veriler kaldırılır. Ancak, coğrafi çoğaltma daha geç ise ikincil bağlı önbellek çoğaltılan veriler saklanır kaldırıldı.
- Şunları yapamazsınız [ölçek](cache-how-to-scale.md) önbellekleri bağlı olsa da önbellek.
- Şunları yapamazsınız [parça sayısını değiştirmek](cache-how-to-premium-clustering.md) önbellek kümeleme özelliği etkinleştirildiğinde varsa.
- Her iki önbellek kalıcılığı etkinleştirilemiyor.
- Yapabilecekleriniz [dışarı](cache-how-to-import-export-data.md#export) ya da önbelleğinden.
- Şunları yapamazsınız [alma](cache-how-to-import-export-data.md#import) ikincil bağlı önbelleğine.
- Bağlı önbellek veya Önbelleklerin bağlantısı kadar bunları içeren kaynak grubu silinemiyor. Daha fazla bilgi için [neden işlem başarısız bağlı önbelleğimin silmeye çalışırken?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Önbellekleri farklı bölgelerde bulunuyorsa, ağ çıkışı maliyeti bölgeler arasında taşınan veriler için geçerlidir. Daha fazla bilgi için [ücreti ne kadardır verilerimi Azure bölgeleri arasında çoğaltılmasını?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Birincil ve ikincil bağlı önbellek arasında otomatik yük devretme gerçekleşmez. Daha fazla bilgi ve yük devretme bir istemci uygulaması hakkında bilgi için bkz. [nasıl yük devretme ikincil bağlı önbellek çalışır?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısı Ekle

1. İki önbellekler için coğrafi çoğaltmayı birbirine bağlamak için tıklatın kaydedeceğinize **coğrafi çoğaltma** önbelleği kaynak menüsünden birincil olmasını istediğiniz önbellek bağlı. Ardından, **Ekle önbellek çoğaltma bağlantısı** gelen **coğrafi çoğaltma** dikey penceresi.

    ![Bağlantı Ekle](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Hedeflenen ikincil önbellekten adına **uyumlu önbellekler** listesi. İkincil önbelleğinizi listede görüntülenmiyorsa, doğrulayın [coğrafi çoğaltma önkoşulları](#geo-replication-prerequisites) için ikincil önbellek karşılanır. Önbellekleri bölgeye göre filtrelemek için yalnızca önbelleklere haritanın bölgede tıklayın **uyumlu önbellekler** listesi.

    ![Coğrafi çoğaltma uyumlu önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Bağlama işlemini Başlat veya bağlam menüsünü kullanarak ikincil önbellek hakkında ayrıntıları görüntüleyin.

    ![Coğrafi çoğaltma bağlam menüsü](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Tıklayın **bağlantı** iki önbellekleri birbirine bağlamak ve çoğaltma işlemini başlatmak için.

    ![Bağlantı önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Çoğaltma işleminin ilerleme durumunu görüntüleyebileceğiniz **coğrafi çoğaltma** dikey penceresi.

    ![Bağlantı durumu](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Üzerinde bağlantı durumunu görüntüleyebilirsiniz **genel bakış** birincil ve ikincil önbellek için dikey pencere.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Çoğaltma işlemi tamamlandıktan sonra **bağlantı durumu** değişikliklerini **başarılı**.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Birincil bağlı önbellek bağlama işlemi sırasında kullanılabilir kalır. İkincil bağlı önbellek bağlama işlemi tamamlanana kadar kullanılamaz.

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

- Bağlama işlemini tamamlarken bağlanırken, birincil bağlı önbellek kullanılabilir kalır.
- Bağlama işlemi tamamlanana kadar ikincil bağlı önbellek bağlanırken kullanılamaz.
- Kaldırılırken, her iki önbelleklerinin bağlantısını kaldırma işlemi tamamlanırken kullanılabilir durumda kalır.

### <a name="can-i-link-more-than-two-caches-together"></a>İkiden fazla önbellekler birlikte bağlayabilir miyim?

Hayır, yalnızca iki önbellekler birbirine bağlayabilirsiniz.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Ben, iki farklı Azure aboneliklerinde önbelleklerden bağlayabilir miyim?

Hayır, her iki önbellekler, aynı Azure aboneliğinde olması gerekir.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Ben, iki farklı boyutlarda önbelleklerle bağlayabilir miyim?

İkincil bağlı önbellek birincil bağlı önbellek daha büyük olduğu sürece Evet.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Coğrafi çoğaltma Etkin kümeleme ile kullanabilir miyim?

Her iki önbellekler aynı parça sayısı olduğu sürece Evet.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Bir vnet'teki benim önbellek ile coğrafi çoğaltma kullanabilir miyim?

Evet, sanal ağlar önbelleklerinde coğrafi çoğaltma ile ilgili uyarılar desteklenir:

- Aynı vnet'teki önbellekler arasında coğrafi çoğaltma desteklenir.
- Farklı sanal ağlardaki önbellekler arasında coğrafi çoğaltma ayrıca desteklenir.
  - Sanal ağlar aynı bölgedeyse bağlanabilir kullanarak [VNET eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) veya [VPN ağ geçidi VNET-VNET bağlantı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#V2V).
  - Sanal ağlar farklı bölgelerde bulunuyorsa coğrafi çoğaltma kullanarak VNET eşlemesi temel iç yük Dengeleyiciler ile ilgili bir sınırlama nedeniyle desteklenmez. Sanal ağ eşleme kısıtlamaları hakkında daha fazla bilgi için bkz. [sanal ağ - eşleme - gereksinimler ve kısıtlamalar](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#requirements-and-constraints). Önerilen çözüm, bir VPN ağ geçidi VNET-VNET bağlantısı kullanmaktır.

Kullanarak [Azure bu şablonu](https://azure.microsoft.com/resources/templates/201-redis-vnet-geo-replication/), iki önbellekler coğrafi olarak çoğaltılmış bir VPN ağ geçidi VNET-VNET bağlantısı ile sanal ağ içinde dağıtabilirsiniz.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Redis coğrafi çoğaltma için çoğaltma zamanlamasını nedir?

Çoğaltma, sürekli ve zaman uyumsuz ve belirli bir zamanlamaya göre gerçekleşmez. Birincil siteye yapılan tüm yazma işlemlerini ikincil anında ve zaman uyumsuz olarak çoğaltılır.

### <a name="how-long-does-geo-replication-replication-take"></a>Coğrafi çoğaltma çoğaltma ne kadar sürer?

Artımlı, zaman uyumsuz ve sürekli çoğaltma ve bölgeler arasında geçen süre gecikme süresi ' çok farklı değildir. Belirli koşullar altında ikincil önbellek birincil verilerin tam bir eşitleme yapmanız gerekebilir. Çoğaltma süre bu durumda söz gibi bir dizi etkene bağlıdır: birincil önbellek, kullanılabilir ağ bant genişliği ve bölgeler arası gecikme yükü. Çoğaltma süresi tam 53 GB coğrafi olarak çoğaltılmış bir çifti için herhangi bir yere 5-10 dakika arasında olabilir bulduk.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Çoğaltma kurtarma noktası sağlanır?

Coğrafi olarak çoğaltılmış bir modda önbellekler için Kalıcılık devre dışı bırakıldı. Coğrafi olarak çoğaltılmış bir çifti, müşteri tarafından başlatılan bir yük devretme gibi bağlantısız ise ikincil bağlı önbellek zaman o noktaya kadar eşitlenmiş verilerini tutar. Kurtarma noktası yok, bu gibi durumlarda garanti edilir.

Bir kurtarma noktası almak için [dışarı](cache-how-to-import-export-data.md#export) ya da önbelleğinden. Daha sonra [alma](cache-how-to-import-export-data.md#import) birincil bağlı önbelleğine.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Coğrafi çoğaltmayı yönetmek için PowerShell veya Azure CLI'yı kullanabilir miyim?

Evet, coğrafi çoğaltma, Azure portal, PowerShell veya Azure CLI kullanılarak yönetilebilir. Daha fazla bilgi için [PowerShell docs](https://docs.microsoft.com/powershell/module/az.rediscache/?view=azps-1.4.0#redis_cache) veya [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/redis/server-link?view=azure-cli-latest).

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Bu benim verilerimi Azure bölgeleri arasında çoğaltılmasını nin ücreti ne kadardır?

Coğrafi çoğaltma kullanırken birincil bağlı önbellek verilerini ikincil bağlı önbellek için çoğaltılır. İki bağlı önbellekleri aynı bölgedeyse veri aktarımı için ücret alınmaz. İki bağlı önbellekleri farklı bölgelerde bulunuyorsa, veri aktarımı ücreti giren ve çıkan iki bölge arasında ağ çıkışı maliyeti ' dir. Daha fazla bilgi için [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Neden bağlı önbelleğimin silmeye çalıştığınızda işlem başarısız oldu?

Coğrafi çoğaltma bağlantısı kaldırana kadar bağlı durumdayken, coğrafi olarak çoğaltılmış bir önbellek ve bunların kaynak gruplarını silinemiyor. Birine veya ikisine de bağlı önbellekleri içeren kaynak grubunu silmeye çalışıyorsanız, kaynak grubundaki diğer kaynaklar silinir, ancak kaynak grubu içinde kalır `deleting` durum ve tüm bağlı önbellekler kaynak grubunda kalması içinde `running`durumu. Kaynak grubu ve içerdiği bağlı önbellekleri tamamen silmek için Önbelleklerin açıklandığı bağlantısı [coğrafi çoğaltma bağlantısını kaldırmak](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>İkinci bağlantılı önbelleğimin için hangi bölgeyi kullanmalıyım?

Genel olarak, eriştiği uygulama ile aynı Azure bölgesinde mevcut bir önbellek hesabınız için önerilir. Uygulamalar için ayrı birincil ve geri dönüş bölgelerle aynı bu bölgede, birincil ve ikincil önbellek mevcut önerilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [en iyi uygulamaları – Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>Yük devretme ikincil bağlı önbellek nasıl çalışır?

Otomatik Yük devretme Azure bölgelerinde coğrafi olarak çoğaltılmış bir önbellek için desteklenmiyor. Bir olağanüstü durum kurtarma senaryosunda, müşterilerin tüm uygulama yığınını oluşturan koordineli bir şekilde yedekleme bölgelerinde duruma getirmeniz gerekir. Tek tek uygulama bileşenleri karar izin vererek, yedeklerini kendi geçmek ne zaman performansı olumsuz yönde etkileyebilir. Redis başlıca avantajlarından biri, çok düşük gecikmeli depolama olmasıdır. Müşteri'nin ana uygulama önbelleğini farklı bir bölgede ise, eklenen gidiş dönüş süresi performans üzerinde fark edilebilir etkisi gerekir. Bu nedenle, biz otomatik yük devretmeyi önlemek geçici kullanılabilirlik sorunları nedeniyle.

Müşteri tarafından başlatılan bir yük devretmeyi başlatmak için önce Önbelleklerin bağlantısı. Ardından, Redis istemci bağlantı uç noktası (eski adıyla bağlı) ikincil önbellek kullanmak için değiştirin. İki önbellekleri bağlantısız olduğunda, ikincil önbellek normal bir okuma-yazma önbelleği yeniden olur ve Redis istemcileri doğrudan gelen istekleri kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure önbelleği için Redis Premium katmanı](cache-premium-tier-intro.md).
