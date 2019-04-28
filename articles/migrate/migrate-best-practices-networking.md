---
title: En iyi uygulamalar, iş yüklerini Azure'a geçiş için ağ kurma ayarlanacak | Microsoft Docs
description: Azure'a geçirdikten sonra geçirilen iş yükleriniz için ağ kurma için en iyi alın.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: raynew
ms.openlocfilehash: 302445038dc9767bd412e232f62fc5249a1a7f09
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61296546"
---
# <a name="best-practices-to-set-up-networking-for-workloads-migrated-to-azure"></a>İş yükleri için ağ kurma için en iyi uygulamaları için Azure geçişi

Plan ve tasarım geçiş kendisini ek olarak, geçiş için en önemli adımlardan birini tasarım ve uygulama, Azure ağının gibidir. Bu makalede, Azure Iaas ve PaaS uygulamalarında için geçiş sırasında ağ için en iyi uygulamalar açıklanmaktadır.

> [!IMPORTANT]
> En iyi yöntemler ve bu makalede açıklanan fikirlerini Azure platformunda dayalıdır ve Özellikler makalenin yazıldığı sırada hizmet. Özellikler ve yetenekler zamanla değişir. Tüm öneriler dağıtımınız için bu nedenle select geçerli olabilecek size olanlar.


## <a name="design-virtual-networks"></a>Sanal ağları tasarlama

Azure sanal ağları (Vnet) sağlar:
- Azure kaynaklarını iletişim özel olarak, doğrudan ve güvenli bir şekilde diğer sanal ağlar üzerinden.
- Uç nokta bağlantıları sanal ağlar VM'ler ve internet iletişimi gerektiren hizmetler için yapılandırabilirsiniz.
- Bir sanal ağ, Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtım olur.
- Her Azure aboneliği ve Azure bölgesi içinde birden fazla sanal ağ uygulayabilirsiniz.
- Her VNet diğer Vnet'lere yalıtılır.
- Sanal ağlar içinde tanımlanan özel ve genel IP adresleri içerebilir [RFC 1918](https://tools.ietf.org/html/rfc1918), CIDR gösteriminde cinsinden. Genel IP adresleri doğrudan internet'ten erişilemez.
- Sanal ağlar, VNet eşlemesi kullanarak bağlanabilirsiniz. Bağlı sanal ağlar aynı veya farklı bölgelerde olabilir. Bu nedenle bir vnet'teki kaynakların diğer vnet'lerdeki kaynaklar bağlanabilirsiniz.
- Varsayılan olarak, bir sanal ağ içindeki alt ağlar arasında trafiği yönlendirdiği sanal ağlar, şirket içi ağlara ve İnternet'e bağlı.


Nesnelerin interneti ihtiyacınız olduğunda IP adresini düzenlemek nasıl dahil olmak üzere sanal ağ topolojinizi planlama, nasıl uygulanacağını bir merkez-uç ağ alanları hakkında düşünmek birçok nasıl Azure kullanılabilirlik alanları uygulayan sanal ağlar, alt ağlara ayırabilir ve DNS ayarlamalısınız.

## <a name="best-practice-plan-ip-addressing"></a>En iyi yöntem: IP adresleme planlama

Sanal ağlar, geçişin bir parçası oluşturduğunuzda, sanal ağ IP adresi alanınıza planlamanız önemlidir.

- /16 her sanal ağ için CIDR aralığı değerinden büyük olmayan bir adres alanı atamanız gerekir. Sanal ağlar 65536 IP adreslerinin kullanıma izin veren ve /16 değerinden daha küçük bir önek atama IP adresleri kaybına neden olur. RFC 1918 tarafından tanımlanan özel aralıkları bulundukları bile IP adresleri, vakit değil önemlidir.
- VNet adres alanının şirket içi ağ aralığı ile çakışma olmaması gerekir.
- Ağ adresi çevirisi (NAT) kullanılmamalıdır.
- Adresleri çakışan bağlantı kurulamaz ağları neden olabilir ve yönlendirme düzgün çalışmaz. Ağları çakışırsa, ağ yeniden tasarlayıp tasarlayamayacağınızı veya ağ adresi çevirisi (NAT) kullanıp gerekecektir.

**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) , Azure sanal ağları.
- [Okuma](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq) ağ iletişimi SSS.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) ağ kısıtlamaları.


## <a name="best-practice-implement-a-hub-spoke-network-topology"></a>En iyi yöntem: Merkez-uç ağ topolojisi uygulama

Merkez-uç ağ topolojisi, kimlik ve güvenlik gibi hizmetleri paylaşırken iş yüklerini yalıtır.
- Hub merkezi bir bağlantı noktası davranan Azure sanal ağı ' dir.
- Uçlar, merkez sanal ağa bağlanan sanal ağları olan VNet eşlemesi kullanarak.
- Paylaşılan hizmetler merkezde dağıtılırken, tek tek iş yükleri de uçlarda dağıtılır. 

Aşağıdaki topluluklara bir göz atın:
- Azure'da bir hub ve bağlı bileşen topolojisi uygulama, bağlantıları şirket içi ağlarda, güvenlik duvarları ve sanal ağlar arasında yalıtım gibi ortak Hizmetleri merkeze alır. Merkez sanal ağa uç sanal ağlarında barındırılan iş yüklerinin merkezi bir nokta şirket içi ağlara bağlantı ve ana bilgisayar Hizmetleri kullanımı için bir yer sağlar.
- Bir hub ve bağlı bileşen yapılandırması, genellikle büyük kuruluşlar tarafından kullanılır. Daha küçük ağlarda, maliyet ve karmaşıklık kaydetmek için daha basit bir tasarımı göz önünde bulundurabilirsiniz.
- Uç sanal ağları ile diğer uçlardan ayrı olarak yönetilen her bir uçtaki iş yüklerini yalıtmak için kullanılabilir. Her iş yükü birden fazla katman ve Azure yük dengeleyicileri ile bağlı olan birden çok alt ağ ekleyebilirsiniz.
- Merkez ve uç Vnet'ler farklı kaynak gruplarında ve hatta farklı Aboneliklerde uygulanabilir. Farklı Aboneliklerdeki sanal ağları eşleyebilme, aynı veya farklı Azure Active Directory (AD) kiracılar için abonelikler ilişkilendirilebilir. Bu, merkezi olmayan her iş yükü yönetimi için hub ağda tutulan hizmetler paylaşırken sağlar.

![Değişiklik Yönetimi](./media/migrate-best-practices-networking/hub-spoke.png)
*Hub ve bağlı bileşen topolojisi*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) bir hub ve bağlı bileşen topolojisi.
- Azure'ı çalıştırmak için ağ önerileri almak [Windows](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/windows-vm) ve [Linux](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/linux-vm) VM'ler.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) VNet eşlemesi.


## <a name="best-practice-design-subnets"></a>En iyi yöntem: Tasarım alt ağlar

İçinde bir sanal ağ yalıtımı sağlamak için bir veya daha fazla alt ağa bölün ve her alt ağ için alt ağın adres alanının bir bölümü ayırın.
- Her sanal ağ içindeki birden çok alt ağ oluşturabilirsiniz.
- Varsayılan olarak, bir sanal ağ içindeki tüm alt ağlar arasındaki trafiği yönlendirdiği ağ.
- Alt ağ kararlarınızı teknik ve kurumsal gereksinimlerinize göre temel alır.  
- Alt ağlarını CIDR gösterimini kullanarak oluşturursunuz.
- Alt ağlar için ağ aralığı karar verirken, Azure'nın beş kullanılamaz her alt ağ IP adreslerinden korur dikkat edin önemlidir. Örneğin, / 29 (sekiz IP adresleriyle) en küçük kullanılabilir alt oluşturursanız, yalnızca alt ağdaki Konaklara atanabilir üç kullanılabilir adresleri alacak şekilde Azure beş korur.
- Çoğu durumda, / 28'i kullanarak küçük bir alt ağ önerilir.

### <a name="example"></a>Örnek

Tablo 10.245.16.0/20 planlanan bir geçiş için alt ağlar ile bölümlenmiş bir adres alanı ile bir sanal ağ örneği gösterilmektedir.

**Alt ağ** | **CIDR** | **Adresleri** | **Kullanma**
--- | --- | --- | ---
GELİŞTİRME FE EUS2 | 10.245.16.0/22 | 1019 | Ön uç/web katmanı VM'ler
GELİŞTİRME UYGULAMA EUS2 | 10.245.20.0/22 | 1019 | Uygulama katmanı Vm'leri
GELİŞTİRME DB EUS2 | 10.245.24.0/23 | 507 | Veritabanı VM'ler

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#segmentation) alt ağları tasarlama.
- [Bilgi nasıl](https://docs.microsoft.com/azure/migrate/contoso-migration-infrastructure) kurgusal bir şirkette (Contoso) ağ altyapılarını geçiş için hazır.


## <a name="best-practice-set-up-a-dns-server"></a>En iyi yöntem: Bir DNS sunucusunu ayarlayın

Sanal ağ dağıtma, azure varsayılan olarak bir DNS sunucusu ekler. Bu, sanal ağlar hızlı bir şekilde oluşturup kaynakları dağıtma sağlar. Ancak, bu DNS sunucusu bu VNet üzerinde yalnızca kaynaklara hizmetleri sağlar. Birden çok sanal ağları birbirine bağlama veya bir şirket içi sunucusuna ait sanal ağlar bağlanmak istiyorsanız, ek ad çözümlemesi becerileri gerekir. Örneğin, sanal ağlar arasında DNS adlarını çözümlemek için Active Directory gerekebilir. Bunu yapmak için azure'da kendi özel DNS sunucusu dağıtın.

- Bir sanal ağın DNS sunucuları azure'da özyinelemeli Çözümleyicileri için DNS sorgularını iletebilir. Bu sanal ağ içindeki ana bilgisayar adlarını çözümlemek etkinleştirir. Örneğin, Azure'da çalışan bir etki alanı denetleyicisi, kendi etki alanları için DNS sorgularını yanıtlamak ve diğer tüm sorguları Azure'a iletmek.
- DNS iletme hem şirket içi kaynaklara (aracılığıyla etki alanı denetleyicisi) hem de Azure tarafından sağlanan ana bilgisayar adları (ileticisi'ni kullanarak) görmek VM'lerin sağlar. Sanal IP adresi 168.63.129.16 kullanarak azure'da özyinelemeli Çözümleyicileri erişim sağlanır.
- DNS iletme ayrıca Vnet'ler arasında DNS çözümlemesi sağlar ve Azure tarafından sağlanan ana bilgisayar adlarını çözümlemek şirket içi makinelerin sağlar.
    - VM konak adını çözümlemek için DNS sunucusu VM aynı sanal ağda bulunması gerekir ve ileriye doğru ana bilgisayar adı sorgularını azure'a şekilde yapılandırılması.
    - DNS son eki her bir sanal ağda farklı olduğundan, doğru VNet çözümlemesi için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz.
- Kendi DNS sunucularınızı kullandığınızda, her sanal ağ için birden çok DNS sunucusu belirtebilirsiniz. Birden çok DNS sunucusu, ağ arabirimi (Azure Resource Manager için) veya Bulut hizmetinden (Klasik dağıtım modeli için) başına de belirtebilirsiniz.
- Bir ağ arabirimi veya Bulut hizmeti için belirtilen DNS sunucularını VNet için belirtilen DNS sunucularını daha önceliklidir.
- Azure Resource Manager dağıtım modelinde bir sanal ağ ve ağ arabirimi için DNS sunucuları belirtebilirsiniz, ancak yalnızca sanal ağlar ayarını kullanmak için en iyi uygulamadır.

    ![DNS sunucuları](./media/migrate-best-practices-networking/dns2.png) *sanal ağın DNS sunucuları*

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/migrate/contoso-migration-infrastructure) kendi DNS sunucunuzu kullanırken ad çözümlemesi.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-subscriptions) DNS adlandırma kuralları ve kısıtlamalar.


## <a name="best-practice-set-up-availability-zones"></a>En iyi yöntem: Kullanılabilirlik alanları ayarlayın

Kullanılabilirlik alanları, uygulamalarınızın ve verilerinizin veri merkezi arızasına karşı korumak için yüksek kullanılabilirlik artırın.

- Kullanılabilirlik, bir Azure bölgesi içinde benzersiz fiziksel konumlara bölgeleridir.
- Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur.
- Dayanıklılık sağlamak için üç ayrı bölge etkinleştirilmiş tüm bölgelerde en az yoktur.
- Bir bölge içinde kullanılabilirlik alanlarının fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi arızasına karşı korur.
- Bölgesel olarak yedekli Hizmetleri, uygulamaları ve verileri tek hata noktalarından korumak için kullanılabilirlik alanları genelinde çoğaltın. --Azure, kullanılabilirlik alanları ile % 99,99 VM çalışma süresi SLA'sı sunar.

    ![Kullanılabilirlik alanı](./media/migrate-best-practices-networking/availability-zone.png) *kullanılabilirlik alanı*

- Planlama ve yüksek kullanılabilirlik, işlem, depolama, ağ ve şirket içi veri kaynaklarına bir bölge içinde birlikte bulundurma ve bunları diğer bölgelere çoğaltma tarafından geçiş Mimarinizi oluşturun. Kullanılabilirlik alanlarını destekleyen azure Hizmetleri'nin iki kategoriye ayrılır:
    - Bölgesel hizmetler: Bir kaynak belirli bir bölge ile ilişkilendirebilirsiniz. Örnek VM'ler için yönetilen diskler, IP adresleri).
    - Bölgesel olarak yedekli Hizmetleri: Kaynak dilimlerinde otomatik olarak çoğaltır. Örneğin, bölgesel olarak yedekli depolama, Azure SQL veritabanı.
- İnternet'e yönelik iş yükleri veya bölgesel hataya dayanıklılık sağlamak için uygulama katmanı, dengeli standart bir Azure yük dağıtabilirsiniz.

    ![Yük Dengeleyici](./media/migrate-best-practices-networking/load-balancer.png) *yük dengeleyici*

**Daha fazla bilgi edinin:**
-   [Genel bakışın](https://docs.microsoft.com/azure/availability-zones/az-overview) kullanılabilirlik bölgeleri.



## <a name="design-hybrid-cloud-networking"></a>Tasarım karma bulut ağ

Başarılı bir geçiş için şirket içi Kurumsal ağlara Azure'a bağlanmak için önemlidir. Bu hizmetleri Azure'dan sağlandığını burada bir karma bulut ağ olarak da bilinen bir her zaman açık bağlantı oluşturur şirket kullanıcılarına bulut. Bu ağ türü oluşturmak için iki seçenek vardır:

- **Siteden siteye VPN:** Siteden siteye dağıtılan Azure VPN ağ geçidi ile uyumlu şirket içi VPN cihazınız arasında sanal ağ içinde bağlantı. Şirket içi yetkili kaynak, sanal ağlar erişebilir. Siteden siteye iletişim, internet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. 
- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla şirket içi ağınız ve Azure arasında bir Azure ExpressRoute bağlantısı kurun. Bu bağlantı özeldir ve trafiği internet üzerinden Git değil.

**Daha fazla bilgi edinin:**

- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/vpn) karma bulut ağı hakkında.

## <a name="best-practice-implement-a-highly-available-site-to-site-vpn"></a>En iyi yöntem: Yüksek oranda kullanılabilir bir siteden siteye VPN uygulayın

Siteden siteye VPN uygulamak için Azure VPN ağ geçidi ayarlayın.
- Bir VPN ağ geçidi, belirli bir genel Internet üzerinden bir Azure sanal ağı ve şirket içi konum arasında şifrelenmiş trafik göndermek için kullanılan sanal ağ geçidi türüdür.
- Microsoft ağı üzerinden Azure sanal ağları arasında şifrelenmiş trafik göndermek için bir VPN ağ geçidi'ni de kullanabilirsiniz.
- Her sanal ağ, yalnızca bir VPN ağ geçidi olabilir.
- Aynı VPN ağ geçidi ile birden fazla bağlantı oluşturabilirsiniz. Birden fazla bağlantı oluşturduğunuzda, tüm VPN tünelleri kullanılabilir ağ geçidi bant genişliğini paylaşır.
- Her Azure VPN gateway, etkin bir bekleme yapılandırmasında iki örnekten oluşur.
    - Planlı Bakım veya plansız kesintide etkin bir örneği için yük devretme gerçekleştikten ve bekleme örneğine otomatik olarak girer ve siteden siteye veya VNet-VNet bağlantısı devam ettirir. 
    - Geçiş kısa bir kesintiye neden olur.
    - Planlı bakım için bağlantı 10 ila 15 saniye içinde geri yüklenmelidir.
    - Planlanmamış sorunlar için bağlantı kurtarma yaklaşık 1.5 bir dakika, en kötü durumda daha uzun olur.
    - Noktadan siteye (P2S) VPN ağ geçidi istemci bağlantıları kesilir ve kullanıcıların istemci makinelerden yeniden bağlantı kurması gerekir.

Siteden siteye VPN ayarlama, aşağıdakileri yapın:
 - Bir sanal ağa ihtiyacınız olan adres aralığı değil VPN bağlanacağı şirket içi ağ ile çakışıyor.
 - Ağdaki bir ağ geçidi alt ağı oluşturun.
 - Ağ geçidi türünü (VPN) ve ağ geçidi ilke tabanlı ve rota tabanlı olup belirtin, bir VPN ağ geçidi oluşturun. RouteBased VPN daha uyumlu ve gelecekte de korunmasına olarak önerilir.
 - Şirket içi bir yerel ağ geçidi oluşturma ve şirket içi VPN Cihazınızı yapılandırma. 
 - Sanal ağ geçidi ve şirket içi cihaz arasında bir yük devretme siteden siteye VPN bağlantısı oluşturun. Rota tabanlı VPN kullanarak Azure Aktif-Pasif veya aktif-aktif bağlantıları sağlar. Rota tabanlı ayrıca hem siteden siteye (herhangi bir bilgisayardan) hem de noktadan siteye (tek bir bilgisayardan) bağlantıları aynı anda destekler.
 - Ağ geçidi SKU'sunu kullanmak istediğiniz belirt Bu iş yükü gereksinimlerini, aktarım hızı, özellik ve SLA'ları bağlıdır.
 - Sınır Ağ Geçidi Protokolü (BGP) isteğe bağlı bir özelliktir, şirket içi BGP rotalarınızı sanal ağlarınıza yaymak için rota tabanlı VPN ağ geçitleri Azure ExpressRoute ile kullanabilirsiniz.

![VPN](./media/migrate-best-practices-networking/vpn.png)
*siteden siteye VPN*
 
**Daha fazla bilgi edinin:**

- [Gözden geçirme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) uyumlu şirket içi VPN cihazları.
- [Genel bakışın](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) VPN ağ geçitlerini.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable) yüksek oranda kullanılabilir VPN bağlantıları.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) planlayıp tasarlarken bir VPN ağ geçidi.
- [Gözden geçirme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) VPN gateway ayarları.
- [Gözden geçirme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku) ağ geçidi SKU'ları.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview) Azure VPN ağ geçitleri ile BGP ayarı.


### <a name="best-practice-configure-a-gateway-for-vpn-gateways"></a>En iyi yöntem: VPN ağ geçitleri için bir ağ geçidi yapılandırma

Azure VPN ağ geçidi oluşturduğunuzda, GatewaySubnet adlı özel bir alt ağı kullanmalıdır. Bu alt ağ Not Bu en iyi uygulamaları oluştururken:

- Ağ geçidi alt ağının ön ek uzunluğu en fazla ön ek uzunluğu 29 (örneğin, 10.119.255.248/29) olabilir. Geçerli bir önek uzunluğu 27 (örneğin, 10.119.255.224/27) kullanmanız önerilir.
- Ağ geçidi alt ağı adres alanının tanımladığınızda, VNet adres alanının son bölümü kullanın.
- Azure GatewaySubnet kullanırken, hiçbir zaman herhangi bir VM veya ağ geçidi alt ağı için uygulama ağ geçidi gibi diğer cihazları dağıtın.
- Bu alt ağa bir ağ güvenlik grubu (NSG) atamayın. Ağ geçidinin çalışmayı durdurmasına neden olur.

**Daha fazla bilgi edinin:**
- [Bu aracı kullanmak](https://gallery.technet.microsoft.com/scriptcenter/Address-prefix-calculator-a94b6eed) IP adresi alanınıza belirlemek için.

## <a name="best-practice-implement-azure-virtual-wan-for-branch-offices"></a>En iyi yöntem: Azure sanal WAN şube ofisleri için uygulama

Birden çok VPN bağlantıları için Azure sanal WAN üzerinden Azure en iyi duruma getirilmiş ve otomatik dal dal bağlantı sağlayan bir ağ hizmetidir.
- Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya bir sanal WAN iş ortağı aracılığıyla tercih edilen sağlayıcısı cihazları kullanarak yapılabilir.
- Tercih edilen sağlayıcısı cihazları kullanarak basit kullanın, bağlantı ve yapılandırma yönetimi sağlar.
- Azure WAN yerleşik bir pano, zaman tasarrufu ve büyük ölçekli siteden siteye bağlantı izlemek için kolay bir yolunu sunuyor hızlı sorun giderme Öngörüler sunar. 

**Daha fazla bilgi edinin:**
[öğrenin](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-about) Azure sanal WAN.

### <a name="best-practice-implement-expressroute-for-mission-critical-connections"></a>En iyi yöntem: ExpressRoute için Görev açısından kritik bağlantıları uygulama

Azure ExpressRoute hizmeti, Azure sanal veri merkezi arasında özel bağlantılar oluşturarak, şirket içi altyapınızı Microsoft bulutuna genişletmenizi sağlar ve şirket içi ağlara.
- ExpressRoute bağlantıları, herhangi bir ağdan herhangi bir (IP VP) ağ noktadan noktaya Ethernet ağı üzerinden veya bağlantı sağlayıcısı üzerinden olabilir. Bunlar, genel internet üzerinden kurulmaz.
- ExpressRoute bağlantıları, daha yüksek güvenlik, güvenilirlik ve daha yüksek hız (en fazla 10 GB/sn), tutarlı bir gecikme süresi ile birlikte sunar.
- Müşteriler, özel bir bağlantı ile ilişkili uyumluluk kuralları avantajlarını alabilir olarak ExpressRoute sanal veri merkezleri için kullanışlıdır.
- ExpressRoute Direct ile doğrudan Microsoft yönlendiricileri en büyük bant genişliği gereksinimlerinize göre bir 100Gbps bağlanabilirsiniz.
- ExpressRoute, şirket içi ağlar, Azure örnekleri, Microsoft genel adresleri arasındaki yolları için BGP kullanır.

ExpressRoute bağlantıları genellikle dağıtımı, bir ExpressRoute hizmet sağlayıcısı ile ilgi çekici içerir. Bir hızlı başlangıç için başlangıçta sanal veri merkezi arasında bağlantı kurmak için bir siteden siteye VPN kullanımı yaygındır ve şirket içi kaynaklara ve bir ExpressRoute bağlantısı için servis sağlayıcınız ile bir fiziksel bağlantısı olduğunda geçirmenize kurdu.

**Daha fazla bilgi edinin:**
- [Genel bakışın](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ExpressRoute biri.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/expressroute/expressroute-erdirect-about) ExpressRoute doğrudan.

### <a name="best-practice-optimize-expressroute-routing-with-bgp-communities"></a>En iyi yöntem: ExpressRoute ile BGP toplulukları yönlendirmeyi iyileştirme

Birden çok ExpressRoute bağlantı hattına sahip olduğunuzda, Microsoft'a bağlanmak için birden fazla yolunuz vardır. Sonuç olarak, yetersiz yönlendirme gerçekleşebilir ve trafiğiniz Microsoft ve Microsoft ağınıza ulaşması daha uzun bir yol alabilir. Uzun ağ yolu, daha yüksek gecikme süresi. Gecikme süresi, uygulama performansı ve kullanıcı deneyimi üzerinde doğrudan bir etkisi yoktur.

#### <a name="example"></a>Örnek

Bir örneği ele alalım.

- ABD'de biri Los Angeles ve diğeri New York'ta iki ofisiniz var.
- Ofisleriniz, kendi omurga ağınız veya hizmet sağlayıcınızın IP VPN olabilen bir WAN bağlanır.
- Ayrıca, WAN’da bağlı biri ABD Batı ve diğeri ABD Doğu’da olmak üzere iki ExpressRoute bağlantı hattınız vardır. Belli ki, Microsoft ağına bağlanmak için iki yolunuz vardır.


 
**Sorun** artık hem ABD Batı hem de ABD Doğu, bir Azure dağıtımına (örneğin, Azure App Service) sahip olduğunuzu düşünün.
- En iyi deneyim için en yakın Azure hizmetlerine erişmek için her ofisteki kullanıcıların kullanmanız gerekir.
- Bu nedenle Azure ABD Batı ve kullanıcıları New York'ta Azure ABD Doğu Los Angeles'taki kullanıcılar bağlanmak istediğiniz.
- Bu, Doğu Yakası kullanıcıları için ancak Batı çalışır. Sorun aşağıdaki gibidir:
    - Her ExpressRoute bağlantı hattı biz hem Azure ABD Doğu (23.100.0.0/16) ve Azure ABD Batı (13.100.0.0/16) öneklerini.
    - Hangi önekin hangi bölgeden olduğunu bilmenin olmadan önekleri olmayan kabul farklı.
    - WAN ağınız her iki öneklerini ABD Doğu ABD, Batı yakındır ve bu nedenle ABD Los Angeles ofisinde kullanıcılar için en iyi deneyimi küçüktür sağlama Doğu'daki ExpressRoute bağlantı hattına her iki ofis kullanıcıları rota varsayabilirsiniz.

![VPN](./media/migrate-best-practices-networking/bgp1.png)
*BGP toplulukları iyileştirilmemiş bağlantı*

**Çözüm**

Her iki ofis kullanıcıları için yönlendirmeyi en iyi duruma getirmek için hangi önekin Azure Batı BİZE olduğunu, hangi Azure Doğu ABD olduğunu bilmeniz gerekir. Bu bilgiler, BGP topluluk değerlerini kullanarak şifreleyebilirsiniz.
- Her bir Azure bölgesine benzersiz bir BGP topluluk değeri atadığınız. Örneğin 12076: 51004 ABD Doğu için; 12076: 51006 Batı ABD için.
- Hangi önekin hangi Azure bölgesine ait boş olduğundan, tercih edilen bir ExpressRoute bağlantı hattını yapılandırabilirsiniz.
- Yönlendirme bilgilerinin değişimi için BGP kullandığımızdan, yönlendirmeyi etkilemek için BGP'nin yerel tercihini kullanabilirsiniz.
- Bizim örneğimizde, ABD Batı'daki ABD Doğu'daki 13.100.0.0/16 için daha yüksek bir yerel tercih değeri ve benzer şekilde, daha yüksek bir yerel tercih değeri ABD Doğu'daki ABD Batı'daki 23.100.0.0/16 atayabilirsiniz. 
- Bu yapılandırma, hem Microsoft yolunun kullanılabilir, Azure ABD Batı bağlantı hattı kullanılarak Batı için kullanıcıların Los Angeles'taki bağlanır ve kullanıcıları New York'ta Azure ABD Doğu'ya bağlanmak Doğu bağlantı hattı kullanılarak sağlar. Her iki tarafta da yönlendirme en iyi duruma getirilmiştir.

![VPN](./media/migrate-best-practices-networking/bgp2.png)
*BGP toplulukları bağlantı için iyileştirilmiş*


**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing) yönlendirme en iyi duruma getirme

## <a name="securing-vnets"></a>Sanal ağları güvenli hale getirme

Sanal ağlar güvenliğini sağlama sorumluluğunu Microsoft ile sizin arasında paylaşılır. Microsoft, birçok ağ özellikleri, hem de kaynaklarının güvenli kalmasına yardımcı hizmetler sağlar. Güvenlik için sanal ağları tasarlarken, bir sayı, bir çevre ağındaki uygulama, filtreleme ve güvenlik gruplarını kullanarak, IP adresleri ve kaynaklara erişim güvenliğini sağlama ve saldırı koruma uygulanması da dahil olmak üzere, izlemelidir en iyi yöntemler vardır.

**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices) ağ güvenliği için en iyi yöntemler.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#security) güvenli ağlar için tasarım.

## <a name="best-practice-implement-an-azure-perimeter-network"></a>En iyi yöntem: Bir Azure çevre ağındaki uygulama

Microsoft bulut altyapısını koruma konusunda yoğun yatırım alışkanlıklarını olsa da, bulut Hizmetleri ve kaynak gruplarını korumanız gerekir. Çok katmanlı bir güvenlik yaklaşımı, en iyi savunma sağlar. Yerinde bir çevre ağına koyma, savunma stratejisinin önemli bir parçasıdır.

- Bir çevre ağına, güvenilmeyen bir ağdan iç ağ kaynaklarına korur. 
- İnternet'e en dıştaki katmanıdır. Bu genellikle Kurumsal altyapı ve internet arasında genellikle bazı form koruma her iki tarafında bulunur. 
- Tipik kurumsal bir ağ topolojisi çekirdek altyapısı yoğun olarak en çok katmanlı güvenlik cihazları ile duvarlar fortified. Her katmanın sınır, cihazlar ve ilke zorlama noktaları oluşur.
- Her katman, güvenlik duvarları, hizmet reddi (DoS) önleme, izinsiz giriş algılama/yetkisiz erişim koruma sistemlerini (IDs/IPS) ve VPN cihazları ağ güvenlik çözümlerini bir birleşimini içerebilir.
- İlke zorlaması çevre ağındaki güvenlik duvarı ilkeleri, erişim denetim listeleri (ACL'ler) veya belirli yönlendirme kullanabilirsiniz.
- Gelen trafiği internet'ten ulaştıkça kesildi ve meşru istekler ağa sağlarken savunması çözümü blok saldırıların ve zararlı trafiği bir birleşimi tarafından işlenen.
- Çevre ağındaki kaynaklara doğrudan gelen trafiği yönlendirebilirsiniz. Ardından, çevre ağ kaynağı daha ileri trafik ağınıza doğrulamadan sonra taşıma, ağdaki diğer kaynaklarla iletişim kurabilir.

Aşağıdaki şekilde, iki güvenlik sınırları ile bir kurumsal ağda tek alt ağ çevre ağı örneği gösterilmektedir.

![VPN](./media/migrate-best-practices-networking/perimeter.png)
*çevre ağ dağıtımı*

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) Azure ile şirket içi veri merkezi arasında bir çevre ağına dağıtılıyor.


## <a name="best-practice-filter-vnet-traffic-with-nsgs"></a>En iyi yöntem: Nsg'ler ile sanal ağ trafiğini filtreleme

Ağ güvenlik grupları (NSG), kaynaklara gelen ve giden trafiği filtrelemek birden çok gelen ve giden güvenlik kuralları içerir. Filtreleme, kaynak ve hedef IP adresi, bağlantı noktası ve protokol olabilir. 
- Nsg'ler izin veren veya gelen ağ trafiği (veya giden ağ trafiği) reddeden güvenlik kuralları içerir birçok türde Azure kaynağı. Her kural için kaynak, hedef, bağlantı noktası ve protokol belirtebilirsiniz.
- NSG kuralları, 5 demet bilgi (kaynak, kaynak bağlantı noktası, hedef, hedef bağlantı noktası ve protokol) izin vermek veya trafiği reddetmeye yönelik kullanarak öncelik sırasına göre değerlendirilir.
- Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir.
- Bir akış kaydı bir NSG, durum bilgisi olan olmasını sağlar. Örneğin, bağlantı noktası 80 üzerinden herhangi bir adrese bir giden güvenlik kuralı belirtirseniz giden trafiğe yanıt için bir gelen güvenlik kuralı gerekmez. Yalnızca iletişimin dışarıdan başlatılması halinde bir gelen güvenlik kuralı belirtmeniz gerekir.
- Bunun tersi de geçerlidir. Bir bağlantı noktası üzerinden gelen trafiğe izin verilirse, bağlantı noktasından geçen trafiğe yanıt için bir giden güvenlik kuralı belirtmeniz gerekmez.
- Varolan bağlantılar kesintiye akışı etkin bir güvenlik kuralı kaldırdığınızda. Trafik akışları bağlantılarını durdurulur ve en az birkaç dakika için herhangi bir yönde trafik akışı kesilir.
- Nsg'ler oluştururken, mümkün olduğunca ancak gereken çok az sayıda olarak oluşturun.

### <a name="best-practice-secure-northsouth-and-eastwest-traffic"></a>En iyi yöntem: Kuzey/Güney ve Doğu/Batı trafiği güvenli hale getirme

Sanal ağlar'ı güvenli hale getirirken, saldırı vektörlerinin dikkate almak önemlidir.
- Yalnızca alt ağ Nsg'leri kullanarak ortamınıza basitleştirir, ancak yalnızca, alt ağa trafiğin güvenliğini sağlar. Bu, Kuzey/Güney trafiği bilinir.
- Aynı alt ağdaki sanal makineler arasındaki trafik, Doğu/Batı trafiği bilinir.
- Bir bilgisayar korsanının dışarıdan erişim kazanırsa, aynı alt ağda bulunan makineleri eklemek çalışırken durdurulması böylece koruma, her iki biçimi yararlanmak önemlidir.

### <a name="use-service-tags-on-nsgs"></a>Hizmet etiketleri ağlardaki Nsg'ler kullanın

Hizmet etiketini, IP adresi ön eki grubunu temsil eder. Hizmet etiketini kullanarak NSG kuralları oluşturduğunuzda karmaşıklığını en aza yardımcı olur.
- Kuralları oluştururken belirli IP adresleri yerine hizmet etiketlerini kullanabilirsiniz.
- Microsoft, adres ön ekleri ilgili hizmet etiketiyle yönetir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirir.
- Kendi hizmet etiketinizi oluşturamaz veya hangi IP adreslerinin bir etiket içinde yer belirtin.

Hizmet etiketleri, bir kural Azure hizmetlerinin gruplara atamaya dışında el ile iş gerektirir. Örneğin, bir Azure SQL veritabanı web sunucularına erişimi içeren bir sanal ağ alt ağı izin vermek istiyorsanız, 1433 numaralı bağlantı noktasına bir giden kuralı oluşturmak ve kullanmak **Sql** hizmet etiketi.
- Bu **Sql** etiket, Azure SQL veritabanı ve Azure SQL veri ambarı hizmetlerinin adres ön eklerini belirtir.
- Belirtirseniz **Sql** değer, trafiği izin veya Sql reddedildi.
- Yalnızca erişime izin vermek istiyorsanız **Sql** belirli bir bölgede o bölgeyi belirtebilirsiniz. Örneğin, yalnızca Doğu ABD bölgesindeki Azure SQL veritabanı erişimine izin vermek istiyorsanız, belirtebileceğiniz **Sql.EastUS** hizmet etiketini.
- Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin, etiket Azure SQL veritabanı hizmetini temsil eder, ancak belirli SQL veritabanı veya sunucu temsil etmez.
- Bu etiketle temsil edilen tüm adres ön ekleri **Internet** etiketi tarafından da temsil edilir.


**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/security-overview) Nsg'ler.
- [Gözden geçirme](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) hizmet etiketleri Nsg'ler için kullanılabilir.


## <a name="best-practice-use-application-security-groups"></a>En iyi yöntem: Uygulama güvenlik grupları kullanın

Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı yapılandırmanıza olanak sağlar.

- Vm'leri gruplandırın ve uygulama güvenlik gruplarını temel alan ağ güvenlik ilkeleri tanımlar.
- Uygulama güvenlik grupları, güvenlik ilkeniz uygun ölçekte olmadan açık IP adreslerinin bakımını el ile yeniden etkinleştirin.
- Uygulama güvenlik grupları, açık IP adreslerinin ve birden çok kural kümesi, iş mantığınıza odaklanabilirsiniz işleyin. 

### <a name="example"></a>Örnek

![Uygulama güvenlik grubu](./media/migrate-best-practices-networking/asg.png)
*uygulama güvenlik grubu örneği*

**Ağ arabirimi** | **Uygulama güvenlik grubu**
--- | ---
NIC1 | AsgWeb
NIC2 | AsgWeb
NIC3 | AsgLogic
NIC4 | AsgDb 

- Bizim örneğimizde her ağ arabirimi yalnızca bir uygulama güvenlik grubuna ait, ancak aslında bir arabirim Azure limitleri uygun şekilde birden çok gruba ait olabilir.
- Ağ arabirimlerinden hiçbiri, ilişkili bir NSG'si vardır. NSG1 her iki alt ağlara ilişkili olan ve aşağıdaki kurallar içerir.

    **Kural adı** | **Amacı** | **Ayrıntılar**
    --- | --- | ---   
    Allow-HTTP-Inbound-Internet | Trafik, internet'ten web sunucuları sağlar. Ek bir kural AsgLogic veya AsgDb uygulama güvenlik grupları için gereken şekilde internet'ten gelen trafiği DenyAllInbound varsayılan güvenlik kuralı tarafından reddedildi. | Önceliği: 100<br/><br/> Kaynak: internet<br/><br/> Kaynak bağlantı noktası: *<br/><br/> Hedef: AsgWeb<br/><br/> Hedef bağlantı noktası: 80<br/><br/> Protokol: TCP<br/><br/> Erişim: İzin verir.
    Deny-Database-All | Aynı sanal ağ kaynakları arasındaki tüm iletişim Allowvnetınbound varsayılan güvenlik kuralından sağlar, bu kural, tüm kaynaklardan trafiği reddetmeye yönelik gereklidir. | Önceliği: 120<br/><br/> Kaynak: *<br/><br/> Kaynak bağlantı noktası: *<br/><br/> Hedef: AsgDb<br/><br/> Hedef bağlantı noktası: 1433<br/><br/> Protokol: Tümü<br/><br/> Erişim: İzin verme.
    Allow-Database-BusinessLogic | Trafik AsgLogic uygulama güvenlik grubuna AsgDb uygulama güvenlik grubuna izin verin. Bu kuralın önceliğini Reddet veritabanı tüm kural yüksektir ve söz konusu kuralı önce işlenir, AsgLogic uygulama güvenlik grubuna gelen trafiğe izin verilir ve diğer tüm trafik engellenir. | Önceliği: 110<br/><br/> Kaynak: AsgLogic<br/><br/> Kaynak bağlantı noktası: *<br/><br/> Hedef: AsgDb<br/><br/> Hedef bağlantı noktası: 1433<br/><br/> Protokol: TCP<br/><br/> Erişim: İzin verir.

- Bir uygulama güvenlik grubunu kaynak veya hedef olarak belirten kurallar yalnızca uygulama güvenlik grubuna üye olan ağ arabirimlerine uygulanır. Ağ arabirimi bir uygulama güvenlik grubuna üye değilse, ağ güvenlik grubu alt ağ ile ilişkilendirilmiş olsa dahi kural ağ arabirimine uygulanmaz.

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) uygulama güvenlik grupları.


### <a name="best-practice-secure-access-to-paas-using-vnet-service-endpoints"></a>En iyi yöntem: Sanal ağ hizmet uç noktaları kullanarak PaaS hizmetlerine güvenli erişim

Sanal ağ hizmet uç noktaları, sanal ağ özel adres alanınızı ve kimlik Azure hizmetlerine doğrudan bağlantı üzerinden genişletin.

- Uç noktalar kritik Azure hizmeti kaynaklarınızı yalnızca, sanal ağlar için güvenli olanak sağlar. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır.
- Sanal ağ özel adres alanınızı çakışan olabilir ve bu nedenle bir sanal ağdan kaynaklanan trafik benzersiz olarak tanımlanabilmesi için kullanılamaz.
- Hizmet uç noktaları ağınızda etkinleştirildikten sonra hizmet kaynaklarını bir sanal ağ kuralı ekleyerek Azure hizmet kaynaklarının güvenliğini sağlayabilirsiniz. Bu, tam olarak kaynaklara genel internet erişimini kaldırarak ve yalnızca ağınızdan trafiğe izin veren gelişmiş güvenlik sağlar.

![Hizmet uç noktalarını](./media/migrate-best-practices-networking/endpoint.png)
*hizmet uç noktaları*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) sanal ağ hizmet uç noktaları.


## <a name="best-practice-control-public-ip-addresses"></a>En iyi yöntem: Denetim genel IP adresleri

Azure'da genel IP adresleri, sanal makineleri, yük Dengeleyiciler, uygulama ağ geçitleri ve VPN ağ geçitleri ile ilişkilendirilebilir.

- Genel IP adresleri, internet kaynaklarının Azure kaynakları için gelen iletişim kurmak ve iletişim kurmak için Azure kaynaklarının İnternet'e giden izin verir.
- Genel IP adresleri arasındaki farkları pek çok temel veya standart bir SKU ile oluşturulur. Standart SKU'ları herhangi bir hizmeti atanabilir, ancak genellikle en Vm'leri, yük Dengeleyiciler ve uygulama ağ geçitleri üzerinde yapılandırılır.
- Temel genel IP adresi otomatik olarak yapılandırılmış bir NSG'ye sahip olmadığını unutmayın. Erişimi denetlemek için kurallar atayın ve kendi yapılandırmanız gerekir. Standart SKU IP adreslerinin bir NSG ve varsayılan olarak atanan kuralları vardır.
- En iyi uygulama, Vm'leri bir genel IP adresiyle yapılandırılmış olmaması gerekir.
    - Açık bir bağlantı noktası gerekiyorsa, yalnızca web Hizmetleri gibi bağlantı noktası 80 veya 443 olmalıdır.
    - SSH (22) ve RDP (3389) gibi standart uzaktan yönetimi bağlantı noktaları, Nsg'leri kullanarak tüm diğer bağlantı noktaları ile birlikte, engellemek için ayarlanması gerekir.
- Vm'leri bir Azure yük dengeleyici veya application gateway koymak daha iyi bir uygulamadır. Uzak Yönetim noktalarına erişimi gerekiyorsa, Azure Güvenlik Merkezi'nde tam zamanında VM erişimi kullanabilirsiniz.

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) azure'da genel IP adresleri.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) Azure Güvenlik Merkezi'nde tam zamanında VM erişimi.


## <a name="leverage-azure-security-features-for-networking"></a>Ağ için Azure güvenlik özelliklerden yararlanın

Azure kullanımı kolaydır ve ortak ağ saldırılarına karşı zengin önlemler sağlayın, platform güvenlik özellikleri vardır. Bunlar, Azure güvenlik duvarı, Web uygulaması güvenlik duvarı ve Ağ İzleyicisi'ni içerir.

## <a name="best-practice-deploy-azure-firewall"></a>En iyi yöntem: Azure Güvenlik Duvarı'nı dağıtma

Azure güvenlik duvarı, ağ kaynakları koruyan bir yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Bir durum bilgisi olan tam olarak güvenlik duvarı olarak-hizmet ile yerleşik yüksek kullanılabilirlik ve ölçeklenebilirlik sınırsız bulut var.

![Hizmet uç noktalarını](./media/migrate-best-practices-networking/firewall.png)
*Azure güvenlik duvarı*

- Merkezi olarak Azure Güvenlik Duvarı'nda oluşturma, zorlama ve abonelikleri ve sanal ağlar arasında bağlantı ilkeleri uygulama ve ağ oturum.
- Azure güvenlik duvarı, ağınızdan kaynaklanan trafiği tanımlamak için güvenlik duvarı dışında izin vererek, VNet kaynaklarınız için bir statik genel IP adresi kullanır.
- Günlüğe kaydetme ve analiz için Azure güvenlik duvarı Azure İzleyici ile tamamen tümleşiktir.
- Azure güvenlik duvarı kuralları oluştururken en iyi uygulama, FQDN etiketleri kuralları oluşturmak için kullanın.
    - Bir FQDN etiketi FQDN'lerin iyi bilinen Microsoft hizmetleriyle ilişkili bir grubu temsil eder.
    - Gerekli giden ağ trafiğini güvenlik duvarı üzerinden izin vermek için bir FQDN etiketi kullanabilirsiniz.
- Örneğin, el ile Windows Update ağ trafiği, güvenlik duvarı üzerinden izin vermek için birden çok uygulama kuralları oluşturma gerekecektir. FQDN etiketleri kullanarak, bir uygulama kuralı oluşturun ve Windows güncelleştirmelerini etiketi içerir. Bu kuralı, Microsoft Windows Update uç noktalarına ağ trafiği, güvenlik duvarı üzerinden akabilir.

**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/firewall/overview) Azure Güvenlik Duvarı'nın.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN etiketler.


## <a name="best-practice-deploy-azure-web-application-firewall-waf"></a>En iyi yöntem: Azure Web uygulaması Güvenlik Duvarı (WAF) dağıtma

Web uygulamalarını yaygın olarak bilinen açıklarından kötü amaçlı saldırıların giderek hedeflerdir. SQL ekleme saldırıları ve siteler arası betik saldırıları davranışları içerir. Bu tür saldırılarını önleme uygulama kodunda zor olabilir ve ayrıntılı bakım, düzeltme eki uygulama ve uygulama topolojisinin birden çok katmanına izleme gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve uygulama yöneticileri tehditleri ya da izinsiz giriş karşı korumanıza yardımcı olur. Web uygulaması güvenlik duvarı, bilinen güvenlik açıklarının yerine tek tek web uygulamalarının güvenliğini sağlama merkezi bir konumda düzeltme eki uygulayarak güvenlik tehditleri daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

Azure Web uygulaması Güvenlik Duvarı (WAF), Azure application Gateway özelliğidir.
- WAF, web uygulamalarının, ortak açıklardan yararlanmaya ve güvenlik açıkları merkezi koruma sağlar.
- Arka uç kodunda değişiklik yapmadan WAF korur.
- Aynı zamanda bir application gateway arkasında birden fazla web uygulaması koruyabilir
- WAF, Azure Güvenlik Merkezi ile tümleşiktir.
- WAF kurallarını ve kural gruplarını, uygulama gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.
- En iyi uygulama, bir WAF önde Azure Vm'leri üzerinde ya da bir Azure App Service uygulamaları dahil olmak üzere, herhangi bir web'e dönük uygulamada kullanmanız gerekir.

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/application-gateway/waf-overview) WAF.
- [Gözden geçirme](https://docs.microsoft.com/azure/application-gateway/application-gateway-waf-configuration) WAF sınırlamalar ve özel durumlar.


## <a name="best-practice-implement-azure-network-watcher"></a>En iyi yöntem: Uygulama Azure Ağ İzleyicisi

Azure Ağ İzleyicisi, kaynaklar ve bir Azure sanal ağ iletişimi izlemek için araçlar sağlar. Örneğin, bir VM'nin başka bir VM veya FQDN, kaynakları görüntüle ve bir sanal ağ içindeki kaynak ilişkileri gibi bir uç nokta arasındaki iletişimin izlemek ya da ağ trafiği sorunları tanılayın.

![Ağ İzleyicisi](./media/migrate-best-practices-networking/network-watcher.png)
*Ağ İzleyicisi*

- Ağ İzleyicisi ile izleyebilir ve Vm'leri oturum açarak olmadan ağ sorunlarını tanılayın.
- Uyarılar ayarlayarak paket yakalaması tetikleyin ve paket düzeyinde gerçek zamanlı performans bilgilerine erişim elde edebilirsiniz. Bir sorun gördüğünüzde, ayrıntılı olarak İnceleme yapabilirsiniz.
- En iyi yöntem, NSG akış günlüklerini gözden geçirmek için Ağ İzleyicisi'ni kullanmanız gerekir.
    - NSG akış günlüklerini Ağ İzleyicisi'nde giriş ve çıkış IP trafiğini bir NSG ile ilgili bilgileri görüntülemek izin verin.
    - Akış günlüklerini json biçiminde yazılır.
    - Akış günlüklerini giden ve gelen akışları Kural başına temelinde, akışı uygulandığı, 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası ve protokol) akış ve trafiği izin verilen veya reddedilen ağ arabirimi (NIC) gösterir.

**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/network-watcher) Ağ İzleyicisi'nin.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) hakkında NSG akış günlüklerinizi.

## <a name="use-partner-tools-in-the-azure-marketplace"></a>Azure Market'teki iş ortağı araçları kullanın

Daha karmaşık ağ topolojisi, belirli bir ağ sanal Gereçleri (Nva), Microsoft iş ortaklarının güvenlik ürün kullanabilir.

- Bir NVA bir güvenlik duvarı, WAN iyileştirmesi veya diğer ağ işlevi gibi bir ağ işlevi gerçekleştiren bir vm'dir.
- Nva'ları, sanal ağ güvenlik ve ağ işlevleri destekleyecek. Bunlar, yüksek oranda kullanılabilir güvenlik duvarları, yetkisiz erişim önleme, izinsiz giriş algılama, web uygulaması güvenlik duvarları (Waf), WAN iyileştirmesi, yönlendirme, Yük Dengeleme, VPN, sertifika yönetimi, Active Directory ve çok faktörlü kimlik doğrulaması için dağıtılabilir.
- İçinde çok sayıda satıcılardan NVA kullanılabilir [Azure Marketi](https://azuremarketplace.microsoft.com/). 
 

## <a name="best-practice-implement-firewalls-and-nvas-in-hub-networks"></a>En iyi yöntem: Uygulama güvenlik duvarları ve hub ağlardaki nva'ları

Hub'ında Web uygulaması güvenlik duvarları (Waf) ile veya bir güvenlik duvarı Grup, bir Azure güvenlik duvarı aracılığıyla çevre ağındaki (ile internet erişimi) normal şekilde yönetilir. Aşağıdaki karşılaştırmalar göz önünde bulundurun.

**Güvenlik Duvarı türü** | **Ayrıntılar**
--- | ---
Waf'ler | Web uygulamaları, ortak olan ve güvenlik açıkları ve olası açıklardan yararlanmaya karşı etkilese eğilimindedir.<br/><br/> Waf'ler saldırılarına karşı web uygulamalarının (HTTP/HTTPS), daha açık belirtmek gerekirse algılamak için tasarlanmıştır daha genel bir güvenlik duvarı.<br/><br/> Waf'ler yılda güvenlik duvarı teknolojisi ile karşılaştırıldığında, iç web sunucularını tehditlere karşı koruma belirli özellikleri kümesine sahiptir.
Azure Güvenlik Duvarı | Azure güvenlik duvarı, NVA güvenlik duvarı grupları gibi şirket içi ağlara erişimi denetlemek ve bileşen ağlarını, barındırılan iş yüklerini korumak için bir ortak yönetim mekanizması ve güvenlik kuralları kümesi kullanır.<br/><br/> Azure güvenlik duvarı, yerleşik ölçeklenebilirlik sahiptir.
NVA güvenlik duvarları | Azure güvenlik duvarı NVA gibi güvenlik duvarı grupları ortak yönetim mekanizması ve bağlı bileşen ağ ve şirket içi ağlara erişimi denetlemek için barındırılan iş yüklerini korumak için güvenlik kuralları kümesi var.<br/><br/> NVA güvenlik duvarları, el ile bir yük dengeleyicinin arkasına ölçeklendirilebilir.<br/><br/> Bir NVA güvenlik duvarı daha az olmasına karşın bir WAF daha özel yazılım, filtreleme ve herhangi bir türde çıkış ve giriş trafiği incelemek için daha geniş uygulama kapsamına sahiptir.<br/><br/> NVA kullanmak istiyorsanız Azure Market'te bulabilirsiniz.

İnternet'ten kaynaklanan trafik için bir dizi Azure Güvenlik Duvarı (veya nva'ları) kullanmanızı öneririz ve kaynaklanan trafik için başka şirket içi.
- Ağ trafiğinin iki kümesi arasında hiçbir güvenlik çevresi sağladığı gibi yalnızca bir dizi güvenlik duvarları her ikisi için bir güvenlik riski kullanmaktır.
- Katmanları ayrı bir Güvenlik Duvarı'nı kullanarak güvenlik kurallarının denetlenmesine karmaşıklığı azaltan ve boş olduğundan hangi gelen ağ isteğine hangi kuralları karşılık gelir.

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) nva'larını kullanarak bir Azure sanal ağınızda.

## <a name="next-steps"></a>Sonraki adımlar 

Diğer en iyi uygulamaları gözden geçirin:

- [En iyi uygulamalar](migrate-best-practices-security-management.md) güvenlik ve yönetim geçişten sonra.
- [En iyi uygulamalar](migrate-best-practices-costs.md) geçişten sonra maliyet yönetimi için.
