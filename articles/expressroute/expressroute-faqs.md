---
title: Azure ExpressRoute SSS | Microsoft Docs
description: "ExpressRoute SSS desteklenen Azure hizmetlerini, maliyet, veri ve bağlantıları, SLA, sağlayıcıları ve konumları, bant genişliği ve ek teknik ayrıntılar hakkında bilgi içerir."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 09b17bc4-d0b3-4ab0-8c14-eed730e1446e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 4b8b547e3fc57d51f35aa7ca31b76f09593bb5f1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="expressroute-faq"></a>ExpressRoute SSS

## <a name="what-is-expressroute"></a>ExpressRoute nedir?

ExpressRoute, Microsoft veri merkezleri ile şirketinizde veya bir birlikte bulundurma yerde bulunan altyapı arasında özel bağlantılar oluşturmanızı sağlayan bir Azure hizmetidir. ExpressRoute bağlantıları değil genel Internet üzerinden gidin ve Internet üzerinden daha yüksek güvenlik, güvenilirlik ve genel bağlantılara daha düşük gecikme ile hızları sunar.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>ExpressRoute ve özel ağ bağlantıları kullanmanın avantajları nelerdir?

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bunlar, daha yüksek güvenlik, güvenilirlik ve Internet üzerinden genel bağlantılara daha tutarlı ve daha düşük gecikme süreleriyle hızları sunar. Bazı durumlarda, arasında veri aktarmak için ExpressRoute bağlantıları kullanarak şirket içi cihazları ve Azure önemli maliyet avantajları sağlar.

### <a name="where-is-the-service-available"></a>Burada hizmeti var mı?

Hizmet konumu ve kullanılabilirliği için bu sayfaya bakın: [ExpressRoute ortakları ve konumları](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>ExpressRoute ı ortaklıkları taşıyıcı ExpressRoute ortakları ile yoksa, Microsoft'a bağlanmak için nasıl kullanabilirim?

Bölgesel bir operatör seçin ve Ethernet bağlantıları desteklenen exchange birine sağlayıcı konumları güden. Sağlayıcı konumda Microsoft ile eş. Son Kısım denetleyin [ExpressRoute ortakları ve konumları](expressroute-locations.md) hizmet sağlayıcınıza exchange konumlardan herhangi birinde mevcut olup olmadığını görmek için. Ardından, bir expressroute bağlantı hattı için Azure bağlanmak için hizmet sağlayıcısı aracılığıyla sıralayabilirsiniz.

### <a name="how-much-does-expressroute-cost"></a>ExpressRoute maliyet mu ne kadar?

Denetleme [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/expressroute/) fiyatlandırma bilgileri için.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Belirli bir bant genişliği bir expressroute bağlantı hattı için ödeme ise my ağ servis sağlayıcısı'ndan satın VPN bağlantısı aynı hızlı olmasını var mı?

Hayır. Bir VPN bağlantısı herhangi hızı hizmet sağlayıcınızdan satın alabilirsiniz. Ancak, Azure bağlantınızı satın aldığınız ExpressRoute bağlantı hattı bant genişliğiyle sınırlı değildir.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-necessary"></a>Daha yüksek hız kadar gerekirse veri bloğu yeteneği, belirli bir bant genişliği bir expressroute bağlantı hattı için ödeme yapıyorsanız var mı?

Evet. ExpressRoute bağlantı hatları için ek ücret ödemeden edindiğiniz en çok iki kez bant genişliği sınırını veri bloğu olanak tanımak için yapılandırılır. Bu özellik destekleyip desteklemediğini öğrenmek için hizmet sağlayıcınıza başvurun.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Aynı özel ağ bağlantısı sanal ağ ve diğer Azure hizmetleriyle aynı anda kullanabilir miyim?

Evet. Bir kez kurma, bir expressroute bağlantı hattı bir sanal ağ içindeki Hizmetler ve diğer Azure hizmetleriyle aynı anda erişmenize olanak tanır. Sanal ağlar özel eşleme yolu üzerinden ve diğer hizmetlere ortak eşleme yolu üzerinden bağlayın.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>ExpressRoute hizmet düzeyi sözleşmesi (SLA) sunar?

Bilgi için bkz: [ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) sayfası.

## <a name="supported-services"></a>Desteklenen hizmetler

ExpressRoute destekler [üç yönlendirme etki alanları](expressroute-circuit-peerings.md) çeşitli türlerdeki Hizmetleri için.

### <a name="private-peering"></a>Özel eşleme

* Tüm sanal makineler ve bulut Hizmetleri dahil olmak üzere, sanal ağlar

### <a name="public-peering"></a>Ortak eşleme

* Power BI
* Dynamics 365 Finans ve işlemleri (önceki adıyla Dynamics AX çevrimiçi bilinir)
* Aşağıdaki Azure hizmetlerini çoğu birkaç özel durum:
  * CDN
  * Visual Studio Team Services yük test etme
  * Multi-factor Authentication
  * Traffic Manager

### <a name="microsoft-peering"></a>Microsoft eşlemesi

* [Office 365](http://aka.ms/ExpressRouteOffice365)
* Dynamics 365 müşteri katılım uygulamaları (önceki adıyla CRM Online bilinir)
  * Dynamics 365 satış
  * Dynamics 365 Müşteri Hizmetleri
  * Dynamics 365 alan hizmeti
  * Dynamics 365 proje hizmeti
* Kullanarak [yol filtreleri](#route-filters-for-microsoft-peering), Microsoft eşlemesi ile aynı ortak Hizmetleri erişmek:
  * Power BI
  * Dynamics 365 Finans ve işlemleri
  * Aşağıdaki Azure hizmetlerini çoğu birkaç özel durum:
    * CDN
    * Visual Studio Team Services yük test etme
    * Multi-factor Authentication
    * Traffic Manager

## <a name="data-and-connections"></a>Veri ve bağlantıları

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>ExpressRoute kullanarak aktarabilirsiniz veri miktarına bir sınır var mıdır?

Biz bir sınır veri aktarımı miktarı ayarlamayın. Başvurmak [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/expressroute/) bant genişliği oranları hakkında bilgi için.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Hangi bağlantı hızları ExpressRoute tarafından destekleniyor mu?

Desteklenen bant genişliği sunar:

50 Mb/sn, 100 Mb/sn, 200 Mb/sn, 500 Mb/sn, 1 Gb/sn, 2 Gb/sn, 5 Gb/sn, 10 Gb/sn

### <a name="which-service-providers-are-available"></a>Hangi hizmet sağlayıcıları var mı?

Bkz: [ExpressRoute ortakları ve konumları](expressroute-locations.md) hizmet sağlayıcıları ve konumları listesi.

## <a name="technical-details"></a>Teknik Ayrıntılar

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>My şirket içi konumu Azure'a bağlanmak için teknik gereksinimleri nelerdir?

Bkz: [ExpressRoute önkoşulları sayfasında](expressroute-prerequisites.md) gereksinimleri için.

### <a name="are-connections-to-expressroute-redundant"></a>Bağlantıları ExpressRoute için yedek misiniz?

Evet. Her expressroute bağlantı hattı yüksek kullanılabilirlik sağlamak için yapılandırılan bağlantıları çapraz yedek çifti var.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Bağlantı, ExpressRoute Bağlantılarım biri başarısız olursa kaybedersiniz?

Çapraz bağlantıları biri başarısız olursa, bağlantı kaybedersiniz değil. Yedekli bağlantı, ağ yükünü desteklemek kullanılabilir. Ayrıca, birden çok bağlantı hatları hatası esnekliği elde etmek için farklı bir eşleme konumda oluşturabilirsiniz.

## <a name="how-do-i-ensure-high-availability-on-a-virtual-network-connected-to-expressroute"></a>Yüksek kullanılabilirlik Expressroute'a bağlı bir sanal ağ üzerinde nasıl emin olursunuz?

Sanal ağınız için farklı eşleme konumlarda birden çok ExpressRoute bağlantı hatları bağlanarak yüksek kullanılabilirlik elde edebilirsiniz. Bir ExpressRoute site devre dışı kalırsa, örneğin, bağlantı üzerinden başka bir ExpressRoute siteye başarısız olur. Varsayılan olarak, sanal ağınızı trafiğe eşit maliyet çoklu yol yönlendirmesi (ECMP üzerinde) temel yönlendirilir. Tercih ettiğiniz başka bir bağlantı için bağlantı ağırlık kullanabilirsiniz. Bkz: [ExpressRoute yönlendirmeyi en iyi duruma getirme](expressroute-optimize-routing.md) bağlantı ağırlık hakkında daha fazla ayrıntı için.

### <a name="onep2plink"></a>Bulut değişiminde birlikte bulunan değilim ve my hizmet sağlayıcısı, noktadan noktaya bir bağlantı sunar, my şirket içi ağınız ve Microsoft arasında iki fiziksel bağlantılar sipariş gerekiyor mu?

Hizmet sağlayıcınıza fiziksel bağlantı üzerinden iki Ethernet sanal bağlantı hatları oluşturabilir, yalnızca tek bir fiziksel bağlantısı gerekir. Fiziksel bağlantı (örneğin, bir fiber optik) bir katman 1 (L1) cihaz sonlandırılır (görüntü bakın). İki Ethernet sanal devreler, birincil bağlantı hattı için bir tane ve bir ikincil için farklı VLAN kimlikleri ile etiketlenir. Bu VLAN kimlikleri dış 802.1Q Ethernet üstbilgisi ' dir. İç 802.1Q Ethernet üstbilgi (gösterilmez), belirli bir eşlendiği [ExpressRoute Yönlendirme etki alanı](expressroute-circuit-peerings.md).

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>I my VLAN'lar biri için Azure ExpressRoute kullanarak genişletebilir miyim?

Hayır. Katman 2 bağlantı uzantıları Azure'da desteklemiyoruz.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Aboneliğimi içinde birden fazla expressroute bağlantı hattı olabilir mi?

Evet. Aboneliğinizde birden fazla expressroute bağlantı hattı olabilir. Varsayılan sınır 10 olarak ayarlandı. Gerekirse sınırını artırmak için Microsoft Support başvurabilirsiniz.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>ExpressRoute bağlantı hatları farklı hizmet sağlayıcılarından olabilir mi?

Evet. Çok sayıda hizmet sağlayıcıları ile ExpressRoute bağlantı hattına sahip olabilir. Her expressroute bağlantı hattı, yalnızca bir hizmet sağlayıcısı ile ilişkilidir. 

### <a name="can-i-have-multiple-expressroute-circuits-in-the-same-location"></a>Aynı konumda birden çok ExpressRoute bağlantı hatları olabilir mi?

Evet. Aynı konumda aynı veya farklı hizmet sağlayıcıları ile birden çok ExpressRoute bağlantı hattına sahip olabilir. Ancak, birden fazla expressroute bağlantı hattı aynı sanal ağa aynı konumdan bağlayamazsınız.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı nasıl my sanal ağlara bağlanabilir

Temel adımlar şunlardır:

* Bir expressroute bağlantı hattı oluşturun ve etkinleştirin hizmeti sağlayıcısı yüklü.
* Siz veya sağlayıcı BGP eşliği (s) yapılandırmanız gerekir.
* Sanal ağ ExpressRoute devresine bağlama.

Daha fazla bilgi için bkz: [devre sağlama ve devre durumları için ExpressRoute iş akışlarını](expressroute-workflows.md).

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>ExpressRoute devremi bağlantı sınırları vardır?

Evet. [ExpressRoute ortakları ve konumları](expressroute-locations.md) makale, bir expressroute bağlantı sınırları genel bir bakış sağlar. Bağlantı bir expressroute bağlantı hattı için tek bir coğrafi bölge sınırlıdır. Bağlantı, ExpressRoute premium özelliğini etkinleştirerek coğrafi bölgeler arası için genişletilebilir.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı için birden fazla sanal ağ bağlayabilir miyim?

Evet. Standart bir expressroute bağlantı hattı ve 100 kadar en fazla 10 sanal ağlara bağlantılar sahip bir [premium expressroute bağlantı hattı](#expressroute-premium). 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Sanal ağları içeren birden çok Azure aboneliğiniz var. Tek bir expressroute bağlantı hattı için ayrı abonelikleri bulunan sanal ağlara bağlanabilir miyim?

Evet. Tek bir expressroute bağlantı hattı kullanmak için en fazla 10 başka Azure abonelikleri yetki verebilir. Bu sınır, ExpressRoute premium özelliğini etkinleştirerek artırılabilir.

Daha fazla bilgi için bkz: [bir expressroute bağlantı hattı birden çok abonelik paylaşımı](expressroute-howto-linkvnet-arm.md).

### <a name="i-have-multiple-azure-subscriptions-associated-to-different-azure-active-directory-tenants-or-enterprise-agreement-enrollments-can-i-connect-virtual-networks-that-are-in-separate-tenants-and-enrollments-to-a-single-expressroute-circuit-not-in-the-same-tenant-or-enrollment"></a>Farklı Azure Active Directory kiracıları veya Kurumsal Anlaşma kayıtları ilişkili birden çok Azure aboneliğiniz var. Ayrı kiracılar ve aynı Kiracı veya kayıt tek bir expressroute bağlantı hattı için kayıtları bulunan sanal ağlara bağlanabilir miyim?

Evet. ExpressRoute yetkilerini abonelik, Kiracı ve kayıt sınırları gereken ek bir yapılandırma olmadan yayılabilir. 

Daha fazla bilgi için bkz: [bir expressroute bağlantı hattı birden çok abonelik paylaşımı](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Sanal ağlar birbirlerinden aynı bağlantı hattı bağlı?

Hayır. Yönlendirme açısından, aynı expressroute bağlantı hattı bağlı tüm sanal ağları aynı yönlendirme etki alanının parçası olan ve birbirinden yalıtılmış değildir. Rota yalıtım gerekiyorsa, ayrı bir expressroute bağlantı hattı oluşturmanız gerekir.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Birden fazla expressroute bağlantı hattına bağlı bir sanal ağ olabilir mi?

Evet. En fazla dört ExpressRoute bağlantı hatları ile tek bir sanal ağa bağlayabilirsiniz. Bunlar farklı dört sıralanmalıdır [ExpressRoute konumları](expressroute-locations.md).

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>ExpressRoute bağlantı hatları için bağlı my sanal ağlardan Internet erişebilirim?

Evet. Varsayılan yol (0.0.0.0/0) veya Internet rota öneki BGP oturumu üzerinden tanıtılan değil, bir expressroute bağlantı hattı için bağlı bir sanal ağdan Internet'e bağlanabilir.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>ExpressRoute bağlantı hatları için bağlı sanal ağlar için Internet bağlantısı engelleyebilir miyim?

Evet. Bir sanal ağ içindeki dağıtılan sanal makinelerin tüm Internet bağlantısı engellemek için varsayılan yol (0.0.0.0/0) tanıtım ve giden tüm trafiği expressroute bağlantı hattı üzerinden rota.

Varsayılan yolları tanıtma, biz trafiği, şirket için ortak eşleme (örneğin, Azure storage ve SQL DB) geri üzerinden sunulan hizmetlere zorlar. Trafiği için Azure ortak eşleme yolu üzerinden veya Internet üzerinden döndürülecek yönlendiricileriniz yapılandırmanız gerekecektir. Hizmeti için hizmet uç noktası (Önizleme) etkinleştirdiyseniz, hizmet trafiği, şirket zorunlu değil. Trafiği Azure omurga ağı içinde kalır. Hizmet uç noktaları hakkında daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md?toc=%2fazure%2fexpressroute%2ftoc.json)

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Aynı expressroute bağlantı hattı bağlı sanal ağlar birbirlerine iletişim kurabilirsiniz?

Evet. Sanal ağlar aynı expressroute bağlantı hattı bağlı olarak dağıtılan sanal makinelerin birbirleriyle iletişim kurabilir.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>ExpressRoute ile birlikte sanal ağlar için siteden siteye bağlantı kullanabilir miyim?

Evet. ExpressRoute ile siteden siteye VPN bulunabilir.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Bir sanal ağ ExpressRoute kullanmak için siteden siteye / noktası site yapılandırmasından taşıyabilir miyim?

Evet. Bir ExpressRoute ağ geçidi sanal ağınızdaki oluşturmanız gerekir. İşlemle ilişkili kısa bir kapalı kalma süresi yok.

### <a name="why-is-there-a-public-ip-address-associated-with-the-expressroute-gateway-on-a-virtual-network"></a>Neden bir sanal ağda ExpressRoute ağ geçidi ile ilgili genel bir IP adresi var mı?

Genel IP adresi, yalnızca dahili yönetimi için kullanılır. Bu genel IP adresi Internet'e açık değildir ve güvenlik açıklarını sanal ağınızın niteliğinde değildir.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute"></a>ExpressRoute Azure depolama alanına bağlanmak için ne gerekir?

Bir expressroute bağlantı hattı kurmak ve ortak eşleme rotaları yapılandırmanız gerekir.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Tanıtım yolları sayısına bir sınır var mıdır?

Evet. Özel eşleme 4000 rota önekleri ve 200 her ortak eşleme ve Microsoft eşlemesi için kadar kabul. Bu, ExpressRoute premium özelliğini etkinleştirirseniz, özel eşleme için 10.000 yolları artırabilir.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>IP aralıklarını BGP oturumunda tanıtmayı sınırlamalar var mı?

Size özel önekleri (RFC1918) ortak ve Microsoft eşleme BGP oturumu kabul etmeyin.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Sınırlar BGP aşmanız durumunda ne olur?

BGP oturumlarını bırakılır. Ön eki sayım sınırın altına gider sonra bunlar sıfırlanır.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>ExpressRoute BGP tutma süresi nedir? Ayarlanabilir mi?

Tutma süresi 180'dir. Etkin tutma iletileri 60 saniyede gönderilir. Bu ayarlar değiştirilemez Microsoft tarafında sabittir. Farklı zamanlayıcılar yapılandırmak için mümkündür ve BGP oturumu parametreleri uygun şekilde anlaşma.

### <a name="after-i-advertise-the-default-route-00000-to-my-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-to-i-fix-this"></a>I my sanal ağlara varsayılan yol (0.0.0.0/0) tanıtma sonra ı my Azure VM'ler üzerinde çalıştırılan Windows etkinleştirilemiyor. İçin bunu nasıl düzeltirim?

Aşağıdaki adımları etkinleştirme isteği tanıması Azure yardımcı olur:

1. Expressroute bağlantı hattı için ortak eşleme oluşturun.
2. DNS araması gerçekleştirmek ve IP adresini bulmak **kms.core.windows.net**
3. Anahtar Yönetim hizmeti etkinleştirme isteği Azure ve Uy isteği geldiğini tanıması gerekir. Aşağıdaki üç görevlerden birini gerçekleştirin:

   * 2. adımda ortak eşleme aracılığıyla Azure dön aldığınız IP adresi için hedeflenen trafiği, şirket içi ağınızda rota.
   * NSP sağlayıcısı artı-PIN'inizi ortak eşleme aracılığıyla geri Azure trafiği vardır.
   * Bir sonraki atlama olarak Internet sahip IP işaret eden kullanıcı tanımlı bir yol oluşturun ve bu sanal makineleri olduğu alt ağı için geçerlidir.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı bant değiştirebilir miyim?

Evet, Azure portalında veya PowerShell kullanarak, expressroute bağlantı hattı bant genişliğini artırmak deneyebilirsiniz. Varsa kapasite kullanılabilir hattınız oluşturulduğu fiziksel bağlantı noktası, değişikliğinizin başarılı olur. 

Değişikliğiniz başarısız, ya da geçerli bağlantı noktasında sol yeterli kapasitesi hiç anlamına gelir ve daha yüksek bant genişliği ile yeni bir expressroute bağlantı hattı oluşturmak gereken veya bu konumda hiçbir ek kapasite olan bulunmuyorsa, bu durumda, artırmak açamazsınız bant genişliği. 

Bant genişliği artış desteklemek için kendi ağları içinde kısıtlamaları güncelleştirme emin olmak için bağlantı sağlayıcınız ile izlenmesi gerekir. Ancak, expressroute bağlantı hattı bant indiremezsiniz. Düşük bant genişliğine sahip yeni bir expressroute bağlantı hattı oluşturma ve eski devreyi silmek gerekir.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı bant genişliğini nasıl değişiyor?

REST API veya PowerShell cmdlet'ini kullanarak expressroute bağlantı hattı bant genişliğini güncelleştirebilirsiniz.

## <a name="expressroute-premium"></a>ExpressRoute premium

### <a name="what-is-expressroute-premium"></a>ExpressRoute premium nedir?

ExpressRoute premium, aşağıdaki özellikler koleksiyonudur:

* Yönlendirme tablosu sayısı sınırını 4000 yolların özel eşleme için 10.000 yolları çıkardı.
* Artırılmış expressroute bağlantı hattı bağlı Vnet sayısı (varsayılan değer 10). Daha fazla bilgi için bkz: [ExpressRoute sınırları](#limits) tablo.
* Office 365 ve Dynamics 365 bağlantısını.
* Microsoft Çekirdek Ağ üzerinden genel bağlantı. Şimdi sanal ağ bir coğrafi bölge içinde başka bir bölgede bir expressroute bağlantı hattı ile bağlayabilirsiniz.<br>
    **Örnekler:**

    *  Batı Avrupa Silikon vadisi oluşturulan bir expressroute bağlantı hattı için oluşturulan bir sanal ağa bağlayabilirsiniz. 
    *  Sağlayacak şekilde, örneğin, SQL Azure Batı Avrupa, Silikon vadisi devredeki gelen bağlanabileceğiniz ortak eşleme üzerindeki diğer jeopolitik bölgeler ön ekleri bildirilir.


### <a name="limits"></a>ExpressRoute premium etkinleştirilirse kaç tane sanal ağlar t bir expressroute bağlantı hattı bağlayabilir miyim?

Aşağıdaki tablolar ExpressRoute sınırları ve expressroute bağlantı hattı başına Vnet sayısı göster:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>ExpressRoute premium nasıl etkinleştirebilirim?

ExpressRoute premium özellikleri özelliği etkinleştirilmişse, etkin ve devre durumunu güncelleştirerek kapatılabilir. ExpressRoute premium hattı oluşturma zamanında etkinleştirebilir veya REST API'sini çağırmak / PowerShell cmdlet'i.

### <a name="how-do-i-disable-expressroute-premium"></a>ExpressRoute premium nasıl devre dışı bırakabilirim?

REST API veya PowerShell cmdlet'ini çağırarak ExpressRoute premium devre dışı bırakabilirsiniz. ExpressRoute premium devre dışı bırakmadan önce varsayılan sınırını karşılamak için bağlantı gereksinimlerinizi ölçeklendirilmiş emin olmanız gerekir. Varsayılan sınırlarının dışına kullanımınızı ölçeklendirir ExpressRoute premium devre dışı bırakma isteği başarısız olur.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Çekme ve özellikleri premium özellik kümesinden seçmem?

Hayır. Özellikleri seçemezsiniz. ExpressRoute premium üzerinde etkinleştirdiğinizde tüm özelliklerini etkinleştiririz.

### <a name="how-much-does-expressroute-premium-cost"></a>ExpressRoute premium maliyet mu ne kadar?

Başvurmak [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/expressroute/) maliyeti.

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>ExpressRoute premium standart ExpressRoute ücretleri yanı sıra için ödeme yaparım?

Evet. ExpressRoute premium ExpressRoute bağlantı hattı ücretleri ve ücretleri bağlantı sağlayıcı tarafından gerekli üstünde uygulanabilir.

## <a name="expressroute-for-office-365-and-dynamics-365"></a>Office 365 ve Dynamics 365 için ExpressRoute

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-dynamics-365"></a>Office 365 Hizmetleri ve Dynamics 365 bağlamak için expressroute bağlantı hattı nasıl oluşturulur?

1. Gözden geçirme [ExpressRoute önkoşulları sayfasında](expressroute-prerequisites.md) gereksinimleri karşıladığından emin olmak için.
2. Bağlantı gereksinimlerinizi karşılandığından emin olmak için hizmet sağlayıcıları ve konumları listesini gözden geçirin [ExpressRoute ortakları ve konumları](expressroute-locations.md) makalesi.
3. Gözden geçirerek, kapasite gereksinimlerini planlama [ağ planlaması ve Office 365 için performans ayarlaması](http://aka.ms/tune/).
4. Bağlantı kurmak için iş akışlarında listelenen adımları izleyin [devre sağlama ve devre durumları için ExpressRoute iş akışlarını](expressroute-workflows.md).

> [!IMPORTANT]
> Office 365 hizmetlerinizi ve Dynamics 365 bağlantısını yapılandırırken ExpressRoute premium eklentisi etkinleştirdiğinizden emin olun.
> 
> 

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-dynamics-365"></a>Azure ortak Office 365 Hizmetleri ve Dynamics 365 bağlanmak eşleme etkinleştirmeniz gerekiyor mu?

Hayır, yalnızca Microsoft Peering etkinleştirmeniz gerekiyor. Azure ad kimlik doğrulama trafiğini Microsoft Peering gönderilir. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-dynamics-365"></a>My mevcut ExpressRoute bağlantı hatları Office 365 Hizmetleri ve Dynamics 365 bağlantıyı destekliyor mu?

Evet. Mevcut expressroute bağlantı hattı, Office 365 hizmetlerine bağlantıyı desteklemek üzere yapılandırılabilir. Office 365 hizmetlerine bağlanmak için yeterli kapasiteye sahip ve premium eklentisi etkinleştirdiğinizden emin olun. [Ağ planlaması ve Office 365 için performans ayarlaması](http://aka.ms/tune/) bağlantınızı planlamanıza yardımcı gerekiyor. Ayrıca bkz [oluşturma ve bir expressroute bağlantı hattı değiştirme](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Hangi Office 365 Hizmetleri bir ExpressRoute bağlantı üzerinden erişilebilir?

Başvurmak [Office 365 URL'leri ve IP adresi aralıkları](http://aka.ms/o365endpoints) ExpressRoute üzerinde desteklenen hizmetlerin güncel bir listesi için sayfa.

### <a name="how-much-does-expressroute-for-office-365-services-and-dynamics-365-cost"></a>Ne kadar ExpressRoute Office 365 hizmetlerinizi ve Dynamics 365 maliyet mu?

Office 365 Hizmetleri ve Dynamics 365 premium Eklentisi etkin olmasını gerektirir. Bkz: [fiyatlandırma ayrıntıları sayfası](https://azure.microsoft.com/pricing/details/expressroute/) maliyetleri için.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Office 365 için ExpressRoute hangi bölgeleri desteklenir?

Bkz: [ExpressRoute ortakları ve konumları](expressroute-locations.md) bilgi.

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>ExpressRoute Kuruluşum için yapılandırılmış olsa bile Internet üzerinden Office 365 erişebilirim?

Evet. ExpressRoute, ağınız için yapılandırılmış olsa bile office 365 hizmet uç noktaları Internet aracılığıyla erişilebilir. Office 365 hizmetlerine ExpressRoute aracılığıyla bağlanmak üzere yapılandırılmış bir konumda varsa, ExpressRoute aracılığıyla bağlanır.

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Office 365 US Government topluluk (GCC) Hizmetleri Azure ABD devlet kurumları expressroute bağlantı hattı erişebilir mi?

Evet. Office 365 GCC hizmet uç noktaları Azure ABD devlet kurumları ExpressRoute aracılığıyla erişilebilir. Ancak, ilk Microsoft'a bildirmek istiyorsanız önekleri sağlamak için Azure portalında bir destek bileti açmanız gerekir. Destek bileti çözümlendikten sonra Office 365 GCC hizmetlerine bağlantı kurulur. 

### <a name="can-dynamics-365-for-operations-formerly-known-as-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics 365 (önceki adıyla Dynamics AX çevrimiçi bilinir) işlemleri için ExpressRoute bağlantısı üzerinden erişilebilir?

Evet. [Dynamics 365 işlemleri için](https://www.microsoft.com/dynamics365/operations) Azure üzerinde barındırılan. Azure ortak bağlanmak için expressroute bağlantı hattı üzerinde eşleme etkinleştirebilirsiniz.

## <a name="route-filters-for-microsoft-peering"></a>Microsoft eşlemesi için yol filtreleri

### <a name="i-am-turning-on-microsoft-peering-for-the-first-time-what-routes-will-i-see"></a>I am kapatma ilk olarak, Microsoft eşlemesi üzerinde hangi yolları ı görürsünüz?

Tüm yollar görmezsiniz. Önek reklamları başlatmak için bağlantı hattınız için bir rota filtre eklemek zorunda. Yönergeler için bkz: [Microsoft eşlemesi için yapılandırma yol filtreleri](how-to-routefilter-powershell.md).

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-to-select-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-to-do-it"></a>T Microsoft eşlemesi üzerinde açık ve Exchange Online seçmek şimdi çalışıyorum, ancak bunu bana ı yapmak için yetkilendirilmemiş hata veren.

Yol filtreleri kullanırken, Microsoft eşlemesi üzerinde herhangi bir müşteriye kapatabilirsiniz. Ancak, Office 365 Hizmetleri kullanma için hala Office 365 tarafından yetki gerekir.

### <a name="do-i-need-to-get-authorization-for-turning-on-dynamics-365-over-microsoft-peering"></a>Microsoft eşlemesi üzerinde Dynamics 365 kapatma yetki alma gerekiyor mu?

Hayır, yetkilendirme için Dynamics 365 gerekmez. Bir kural oluşturmak ve yetkilendirme olmadan Dynamics 365 topluluğu seçin.

### <a name="i-enabled-microsoft-peering-prior-to-august-1st-2017-how-can-i-take-advantage-of-route-filters"></a>T Microsoft nasıl yol filtreleri avantajlarından sürebilir 1 Ağustos 2017 önce eşlemesini etkinleştirilmiş mi?

Var olan bağlantı hattınız için Office 365 ve Dynamics 365 önekleri reklam devam eder. Aynı Microsoft eşlemesi üzerinden Azure genel ön ekten reklamları eklemek istiyorsanız, bir rota filtre oluşturabilir, (ihtiyacınız Office 365 hizmetlerini ve Dynamics 365 dahil) tanıtılan istediğiniz hizmetleri seçin ve Microsoft filtre ekleme eşleme. Yönergeler için bkz: [Microsoft eşlemesi için yapılandırma yol filtreleri](how-to-routefilter-powershell.md).

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-to-enable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>Microsoft bir konumda eşlemesini sahip, artık başka bir konumda etkinleştirmek çalışıyorum ve tüm ön eklerin görüyorum değil.

* 1 Ağustos 2017 önce yapılandırılmış olan ExpressRoute bağlantı hatları, Microsoft eşlemesi yol filtreleri tanımlanmamış olsa bile eşleme, Microsoft tarafından tanıtılan tüm hizmet ön eklerin sahip olur.

* Ve 1 Ağustos 2017 sonrasında yapılandırılmış ExpressRoute bağlantı hatları, Microsoft eşlemesi sahip olmaz tüm ön eklerin rota filtre devresine bağlanana kadar tanıtılan. Varsayılan olarak hiçbir önekleri görürsünüz.
