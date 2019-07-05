---
title: Azure ExpressRoute ile yüksek kullanılabilirlik için tasarlama | Microsoft Docs
description: Bu sayfa, Azure Expressroute'u kullanırken, yüksek kullanılabilirlik için Mimari öneriler sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: rambala
ms.openlocfilehash: 4984b30daf6170873cad9472bfed2d879af57efe
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466640"
---
# <a name="designing-for-high-availability-with-expressroute"></a>ExpressRoute ile yüksek kullanılabilirlik için tasarlama

ExpressRoute, taşıyıcı Microsoft kaynakları sınıf özel ağ bağlantısı sağlamak yüksek kullanılabilirlik için tasarlanmıştır. Diğer bir deyişle, hiçbir tek Microsoft ağı içerisinde ExpressRoute yolunda hata noktası yoktur. Kullanılabilirliği en üst düzeye çıkarmak için müşteri ve ExpressRoute bağlantı hattı hizmet sağlayıcısı kesimi de yüksek kullanılabilirlik için desteklemesi. Bu, ilk şimdi makale içine bir ExpressRoute kullanarak güçlü bir ağ bağlantısı oluşturmak için ağ altyapısı konuları arayın ve ardından, ExpressRoute devreniz yüksek kullanılabilirliğini artırmak için yardımcı ince ayar yapma özelliklerini göz atalım.


## <a name="architecture-considerations"></a>Mimarisi konuları

Aşağıdaki şekil, bir ExpressRoute bağlantı hattı, bir ExpressRoute bağlantı hattı kullanılabilirliği en üst düzeye çıkarma kullanarak bağlanmak için önerilen yol gösterir.

 [![1]][1]

Yüksek kullanılabilirlik için uçtan uca ağ ExpressRoute bağlantı hattının yedeklilik sağlamak önemlidir. Diğer bir deyişle, şirket içi ağınız içinde artıklık sürdürmeniz gerekir ve yedeklilik hizmeti sağlayıcısı ağınızdaki tehlikeye olmamalıdır. En az yedeklilik bakımı, ağ hataları tek noktası önleme anlamına gelir. Olan yedek güç ve soğutma cihazları daha fazla ağ için yüksek oranda kullanılabilirliğini artırın.

### <a name="first-mile-physical-layer-design-considerations"></a>İlk mil fiziksel katman tasarım konuları

 Hem birincil ve ikincil bağlantıların bir ExpressRoute devrelerinin aynı müşteri şirket içi ekipman (CPE) üzerinde sonlandırılması durumunda şirket içi ağınız içinde yüksek kullanılabilirlik ödün. Ayrıca, her iki birincil ve ikincil bağlantıların aynı bağlantı noktası üzerinden bir CPE (veya farklı arabirimlerde altında iki bağlantı sonlandırma iki bağlantı iş ortağı ağı içinde birleştirerek) yapılandırırsanız, iş ortağı zorlama yüksek kullanılabilirlik de ağ segmentine aşmaya. Bu güvenlik aşılması ile aşağıdaki şekilde gösterilmiştir.

[![2]][2]

Birincil ve ikincil bağlantıların bir ExpressRoute bağlantı hatları, farklı coğrafi konumlarda sonlandırılması durumunda, diğer taraftan, ardından, ağ bağlantısının ödün. Trafiği etkin olarak yük dengeli birincil ve farklı coğrafi konumlarda sonlandırılır ikincil bağlantıların genelinde ise, iki yolu arasındaki ağ gecikmesini olası önemli farkı yetersiz ağ neden olur performans. 

Coğrafi olarak yedekli tasarım konuları için bkz: [ExpressRoute ile olağanüstü durum kurtarma için tasarlama][DR].

### <a name="active-active-connections"></a>Etkin-etkin bağlantıları

Microsoft ağ, ExpressRoute devreleri etkin-etkin modda birincil ve ikincil bağlantıların çalışması için yapılandırılır. Ancak, yol tanıtımları bir ExpressRoute bağlantı hattı Aktif-Pasif modunda çalışmak üzere yedekli bağlantılar zorlayabilirsiniz. Bir yol diğer tercih yapmak için kullanılan genel teknikler yolu eklenmesini olduğu gibi daha belirli yollar ve BGP reklam.

Yüksek kullanılabilirliği geliştirmek için etkin-etkin modda ExpressRoute bağlantı hattının her iki bağlantı çalışılacak önerilir. Etkin-etkin modda çalışan bağlantıları izin verirseniz, Microsoft ağ trafiği akış başına temelinde bağlantıları arasında Yük Dengelemesi.

ExpressRoute devresinin birincil ve ikincil bağlantıların etkin yol içinde bir hata aşağıdaki başarısız olan her iki bağlantı riskini Aktif-Pasif modu yüzdeki çalışıyor. Geçiş hata yaygın nedenleri pasif bağlantı ve eski yolların tanıtılması pasif bağlantı etkin yönetim olmaması olabilir.

Alternatif olarak, bir ExpressRoute devresinin birincil ve ikincil bağlantıların etkin-etkin modda çalışmaya izin ver sonuçları başarısız ve alma yalnızca yaklaşık yarım akışlarında yönlendirdi, izleyerek bir ExpressRoute bağlantı hatası. Bu nedenle, aktif / aktif modu önemli ölçüde ortalama süresi için Kurtarma (MTTR) artırmaya yardımcı olur.

### <a name="nat-for-microsoft-peering"></a>Microsoft eşlemesi için NAT 

Microsoft eşlemesi genel uç noktaları arasındaki iletişim için tasarlanmıştır. Microsoft eşlemesi üzerinden iletişim kurarlar önce kadar yaygın olarak, şirket içi özel ağ adresi çevirisi (NATed) müşteri veya iş ortağı ağı genel IP ile noktalarıdır. Etkin-etkin modda birincil ve ikincil bağlantıların kullandığınız varsayılarak, nerede ve nasıl, NAT ne kadar hızlı ExpressRoute bağlantıları birinde bir hata aşağıdaki kurtarma üzerinde bir etkisi yoktur. İki farklı NAT seçenekleri aşağıdaki şekilde gösterilmiştir:

[![3]][3]

Seçenek 1, NAT expressroute birincil ve ikincil bağlantıların arasındaki trafik bölme sonra uygulanır. Dönüş trafiği akışı üzerinden egressed aynı sınır cihazı gelecek şekilde NAT durum bilgisi olan gereksinimlerini karşılamak için bağımsız NAT havuzları birincil ve ikincil cihazlar arasında kullanılır.

Seçenek 2, ortak bir NAT havuzu expressroute birincil ve ikincil bağlantıların arasındaki trafik bölme önce kullanılır. Trafik bölme önce ortak NAT havuzu gelmez ayrım yapmak önemlidir tek-böylece yüksek kullanılabilirlik ödün hata noktası ile tanışın.

Bir ExpressRoute bağlantı hatası aşağıdaki seçeneği 1, karşılık gelen NAT havuzu ulaşmak için özelliği bozuk. Bu nedenle, tüm bozuk akışa sahip olacak şekilde TCP ya da yeniden oluşturulmuş veya uygulama katmanı aşağıdaki karşılık gelen penceresi zaman aşımı. Ya da NAT havuzları varsa şirket içi sunuculardan herhangi biri ön uç için kullanılan ve karşılık gelen bağlantı arızalanması durumunda, bağlantının düzeltilene kadar şirket içi sunucular Azure'dan ulaşılamıyor.

Seçenek 2 ile bir birincil veya ikincil bağlantı hatadan sonra bile NAT ulaşılabildiğinden ise. Bu nedenle, ağ katmanı hatası aşağıdaki paketler ve Yardım daha hızlı kurtarma yeniden yönlendirme. 

> [!NOTE]
> NAT seçeneği 1 (birincil ve ikincil ExpressRoute bağlantıları için NAT havuzları bağımsız) kullanın ve bir IP adresi NAT havuzunu birinden bir şirket içi sunucusuna bir bağlantı noktası eşleme, sunucunun ExpressRoute üzerinden erişilebilir olmayacak bağlantı hattı karşılık gelen bağlantı başarısız olur.
> 

## <a name="fine-tuning-features-for-private-peering"></a>Özel eşleme özelliklerini hassas ayar yapma

Bu bölümde, bize gözden geçirme (ve bağlı olarak Azure dağıtımınız için MTTR işiniz nasıl hassas) isteğe bağlı özelliklere ExpressRoute devreniz yüksek kullanılabilirliğini geliştirilmesine yardımcı olun. Özellikle, ExpressRoute sanal ağ geçitleri ve çift yönlü iletme algılama (BFD) dilimiyle uyumlu dağıtımını gözden geçirelim.

### <a name="availability-zone-aware-expressroute-virtual-network-gateways"></a>Kullanılabilirlik alanı kullanan ExpressRoute sanal ağ geçitleri

Bir Azure bölgesi içinde kullanılabilirlik alanı, hata etki alanı ve bir güncelleme etki alanı birleşimidir. Bölgesel olarak yedekli Azure Iaas dağıtım için kullanmayı seçerseniz, ExpressRoute özel eşlemesi sonlandırmak bölgesel olarak yedekli sanal ağ geçitlerini yapılandırmak isteyebilirsiniz. Daha fazla bilgi edinmek için [Azure kullanılabilirlik alanları, bölgesel olarak yedekli sanal ağ geçitleri hakkında][zone redundant vgw]. To configure zone-redundant virtual network gateway, see [Create a zone-redundant virtual network gateway in Azure Availability Zones][conf zone redundant vgw].

### <a name="improving-failure-detection-time"></a>Hata algılama süresini iyileştirir

ExpressRoute özel eşlemesi üzerinden BFD destekler. BFD hata algılama zamanı Microsoft Enterprise Edge (Msee) ve şirket içi tarafında kendi BGP komşu arasında Katman 2 ağ üzerinden yaklaşık 3 dakika (varsayılan) ikinci kısa bir azaltır. Hızlı hata algılama zamanı hatadan kurtarma hastening yardımcı olur. Daha fazla bilgi edinmek için [yapılandırma BFD ExpressRoute üzerinden][BFD].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ExpressRoute bağlantı hattı bağlantı yüksek kullanılabilirlik için tasarlama konularını ele almıştık. Bir ExpressRoute bağlantı hattı eşlemesi noktası bir coğrafi konuma sabitlenir ve bu nedenle tüm konumu etkileyen çöküşü tarafından etkilenebilir. 

Bölgenin tamamını etkileyen, geri dönülemez bir hata dayanabilir Microsoft omurga coğrafi olarak yedekli ağ bağlantısı oluşturmak tasarım konuları için bkz. [ExpressRoute özel eşlemesiileolağanüstüdurumkurtarmaiçintasarlama][DR].

<!--Image References-->
[1]: ./media/designing-for-high-availability-with-expressroute/exr-reco.png "ExpressRoute kullanarak bağlanmak için yol önerilir"
[2]: ./media/designing-for-high-availability-with-expressroute/suboptimal-lastmile-connectivity.png "Suboptimal son mil bağlantısı"
[3]: ./media/designing-for-high-availability-with-expressroute/nat-options.png "NAT seçenekleri"


<!--Link References-->
[zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/about-zone-redundant-vnet-gateways
[conf zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/create-zone-redundant-vnet-gateway
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[BFD]: https://docs.microsoft.com/azure/expressroute/expressroute-bfd
[DR]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering




