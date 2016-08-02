<properties 
   pageTitle="Sanal ağlar için güvenli şirket içi bağlantılar hakkında | Microsoft Azure"
   description="Sanal ağlar için güvenli şirket içi bağlantı çeşitleri olan ,Siteden siteye, Noktadan Siteye ve ExpressRoute hakkında bilgi sahibi olun."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="cherylmc" />

# Sanal ağlar için güvenli şirket içi bağlantılar hakkında 

Bu makalede, şirket içi sitenizi bir Azure sanal ağına bağlanmanın farklı yolları açıklanmaktadır. Bu tablo hem Resource Manager, hem de klasik dağıtım modelleri için geçerlidir. VPN ağ geçidi için bağlantı diyagramları arıyorsanız bkz [Azure VPN ağ geçidi bağlantı topolgies](vpn-gateway-topology.md).

Siteden Siteye, Noktadan Siteye ve ExpressRoute.olmak üzere kullanılabilir üç bağlantı seçeneği vardır. Belirlediğiniz seçenek şu gibi çeşitli noktalara bağlı olabilir:


- Çözümünüz ne tür bir verimlilik gerektiriyor?
- Güvenli VPN aracılığıyla ortak Internet üzerinden mi, yoksa özel bir bağlantı üzerinden mi iletişim kurmak istiyorsunuz?
- Kullanıma uygun bir genel bir IP adresiniz var mı?
- VPN cihazı kullanmayı planlıyor musunuz? Bu uyumlu bir VPN cihazı mı?
- Sadece bir kaç bilgisayarla mı bağlanıyorsunuz yoksa siteniz için kalıcı bir bağlantı istiyor musunuz?
- Oluşturmak istediğiniz çözüm için ne tür bir VPN ağ geçidi gerekli?

Aşağıdaki tablo çözümünüz için en iyi bağlantı seçeneğine karar vermenize yardımcı olabilir.

[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]                                                                    

## Siteden siteye bağlantılar

Siteden siteye VPN, şirket içi siteniz ve sanal ağınız arasında güvenli bir bağlantı oluşturmanıza olanak sağlar. Siteden siteye bağlantı oluşturmak için şirket içi ağınızda bulunan bir VPN cihazı Azure VPN ağ geçidi ile güvenli bir bağlantı oluşturmak için yapılandırılır. Bağlantı oluşturulduğunda, yerel ağınızda bulunan kaynaklar ile sanal ağınızda bulunan kaynaklar doğrudan ve güvenli bir şekilde iletişim kurabilir. Siteden siteye bağlantıları, sanal ağ kaynaklarına erişmek için yerel ağınızdaki her istemci bilgisayar için ayrı bir bağlantı kurmanızı gerektirmez.

**Şu durumlarda Siteden Siteye bağlantı kullanın:**

- Karma bir çözüm oluşturmak istediğinizde.
- İstemci tarafından ilave yapılandırmalara gerek kalmadan şirket içi konumunuz ve sanal ağınız arasında bir bağlantı kurmak istediğinizde.
- Kalıcı bir bağlantı istediğinizde. 

**Gereksinimler**

- Şirket içi VPN aygıtının Internet'e yönelik bir IPv4 IP adresi olmalıdır. Bu bir NAT'ın arkasında olamaz
- Uyumlu bir VPN cihazınız olmalıdır. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). 
- Kullandığınız VPN cihazı çözümünüz için gerekli olan ağ geçidi türü ile uyumlu olmalıdır. Bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md)
- Ağ geçidi SKU’su aynı zamanda toplam verimliliği de etkiler. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gwsku)  

**Kullanılabilir S2S dağıtım modelleri ve yöntemleri**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 


## Noktadan siteye bağlantılar

Noktadan Siteye VPN’i sanal ağınıza güvenli bir bağlantı oluşturmanızı sağlar. Bir Noktadan Siteye yapılandırmasında, bağlantı sanal ağa bağlamak istediğiniz her bir istemci bilgisayarında ayrı olarak yapılandırılır. Noktadan Siteye bağlantıları bir VPN cihazı gerektirmez. Bu bağlantı türü, her istemci bilgisayara yüklediğiniz bir VPN istemcisini kullanır. VPN, bağlantının şirket içi istemci bilgisayarından el ile başlatmasıyla oluşturulur.

Noktadan Siteye ve Siteden Siteye yapılandırmaları eşzamanlı olarak bulunabilir. Ancak Noktadan Siteye bağlantıları Siteden Siteye bağlantılarının aksine, eşzamanlı olarak bir ExpressRoute bağlantısıyla aynı sanal ağda yapılandırılamaz.

**Şu durumlarda Noktadan Siteye bağlantı kullanın:**

- Bir sanal ağa bağlanmak için yalnızca birkaç istemciyi yapılandırmanız gerekir.

- Uzak bir konumdan, sanal ağınıza bağlanmak istediğinizde. Örneğin, bir kafeterya veya konferans salonundan bağlanırken

- Siteden siteye bağlantınızın olduğu, ancak uzak bir konumdan bağlanması gereken bazı istemcilerin olduğu durumlarda.

- Siteden siteye bağlantı için minimum gereksinimleri karşılayan bir VPN cihazına erişiminiz yoksa.

- Şirket içi VPN aygıtınızın Internet'e yönelik bir IPv4 IP adresi yoksa

**P2S için kullanılabilir dağıtım modelleri ve yöntemleri**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## ExpressRoute bağlantıları

Azure ExpressRoute, Azure veri merkezleri ile şirketinizde veya bir birlikte bulundurma ortamında bulunan altyapı arasında özel bağlantılar oluşturmanızı sağlar. ExpressRoute bağlantıları ortak Internet’e geçmez ve Internet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sunar.

Bazı durumlarda, ExpressRoute bağlantıları kullanarak şirket içi ve Azure arasında veri aktarmak önemli maliyet avantajları da sağlar. ExpressRoute ile bir ExpressRoute konumuyla (Exchange Provider tesisi) Azure arasında bağlantı kurabilir veya doğrudan bir ağ hizmeti sağlayıcısı tarafından sağlanan mevcut WAN ağınız (örneğin, bir MPLS VPN gibi) üzerinden Azure bağlanabilirsiniz.

ExpressRoute hakkında daha fazla bilgi için bkz: ExpressRoute’a [Teknik Genel Bakış](../expressroute/expressroute-introduction.md).


## Sonraki adımlar

- VPN ağ geçidi hakkında daha fazla bilgi için bkz: [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md), VPN ağ geçidi [SSS](vpn-gateway-vpn-faq.md) ve [Planlama ve Tasarım](vpn-gateway-plan-design.md).

- ExpressRoute hakkında daha fazla bilgi için bkz: ExpressRoute’a [Teknik Genel Bakış](../expressroute/expressroute-introduction.md), [SSS](../expressroute/expressroute-faqs.md) ve [İş Akışları](../expressroute/expressroute-workflows.md)







<!--HONumber=Jun16_HO2-->


