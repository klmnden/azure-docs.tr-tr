<properties 
   pageTitle="VPN Gateway hakkında| Microsoft Azure"
   description="Azure Sanal Ağları için VPN Gateway bağlantıları hakkında bilgi edinin."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/01/2016"
   ms.author="cherylmc" />

# VPN Gateway hakkında


Sanal ağ geçidi, Azure sanal ağları ile şirket içi konumlar arasında ve Azure içindeki sanal ağlar arasında (Sanal Ağdan Sanal Ağa) ağ trafiği göndermek için kullanılır. Bağlantı oluşturmak için bir sanal ağa ek kaynaklar ve ayarları ile birlikte bir sanal ağ geçidi ekleyin. 

Bir sanal ağ geçidi kaynağı oluştururken birkaç ayar yapılandırırsınız. Gerekli ayarlardan biri '-GatewayType' şeklindedir. Ağ geçidi türü, ağ geçidinin nasıl bağlandığını belirtir. İki sanal ağ geçidi türü vardır: Vpn ve ExpressRoute. Ayrılmış bir bağlantı üzerinde ağ trafiği gönderilmediğinde 'ExpressRoute' ağ geçidini kullanırsınız. Bu seçenek ExpressRoute ağ geçidi olarak da adlandırılır. Ağ trafiği ortak bir bağlantı üzerinden şifreli olarak gönderildiğinde 'Vpn' ağ geçidi türünü kullanırsınız. Bu seçenek VPN ağ geçidi olarak adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir. Bu makale öncelikli olarak VPN Gateway seçeneğine odaklanmaktadır. ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md).

Ağ geçidi gereksinimleri hakkında bilgi için bkz. [Ağ Geçidi Gereksinimleri](vpn-gateway-about-vpn-gateway-settings.md#requirements). Tahmin edilen toplam verimlilik için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md#aggthroughput). Fiyatlandırma için bkz. [VPN Gateway Fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway). Abonelikler ve hizmet sınırlamaları için bkz. [Ağ Limitleri](../articles/azure-subscription-service-limits.md#networking-limits).


## VPN Gateway yapılandırma

VPN Gateway'i yapılandırırken kullanacağınız yönergeler, sanal ağınızı oluşturmak için kullandığınız dağıtım modeline bağlıdır. Örneğin,VNet'inizi klasik dağıtım modeli kullanarak oluşturduysanız VPN ağ geçidi ayarlarınızı oluşturmak ve yapılandırmak için klasik dağıtım modeline ilişkin yönergeleri kullanırsınız. Dağıtım modelleri hakkında daha fazla bilgi için bkz. [Resource Manager’ı ve klasik dağıtım modellerini anlama](../resource-manager-deployment-model.md).

VPN ağ geçidi bağlantısı belirli ayarlarla yapılandırılmış birden fazla kaynağı kullanır. Kaynakların birçoğu ayrı ayrı yapılandırılabilir, ancak bazı durumlarda belirli bir sırayla yapılandırılması gerekir. Azure portalı gibi bir yapılandırma aracını kullanarak kaynakları oluşturmaya ve yapılandırmaya başlayabilirsiniz. Daha sonra ek kaynaklar yapılandırmak ya da uygun olduğunda var olan kaynakları değiştirmek için PowerShell gibi başka bir araca geçmeye karar verebilirsiniz. Şu anda Azure portalında her kaynağı ve kaynak ayarını yapılandıramazsınız. Her bağlantı topolojisine ilişkin makalelerdeki yönergelerde, belirli bir aracının ne zaman gerekli olduğu belirtilmektedir. VPN Gateway’e ilişkin kaynaklar ve ayarlar hakkında bilgi için bkz. [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).

Sonraki bölümlerde aşağıdaki bilgileri listeleyen tablolar bulunur:

- kullanılabilir dağıtım modeli
- kullanılabilir yapılandırma araçları
- varsa sizi doğrudan bir makaleye yönlendiren bağlantılar

Gereksinimlerinize uygun bağlantı topolojisini seçmenize yardımcı olması için diyagramları ve açıklamaları kullanabilirsiniz. Diyagramlarda temel topolojilerin başlıca olanları gösterilmektedir ancak diyagramları bir kılavuz olarak kullanıp daha karmaşık yapılandırmalar da oluşturabilirsiniz.

## Siteden Siteye ve Çok Siteli

### Siteden Siteye

Siteden Siteye (S2S) VPN ağ geçidi bağlantısı, IPSec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bu bağlantı türü için, şirket içinde ortak IP adresi atanmış olan ve NAT'nin arkasında bulunmayan bir VPN cihazı gerekir. S2S bağlantıları, şirket içi ve dışı yapılandırmalar ile birlikte karma yapılandırmalar için kullanılabilir.   

![S2S bağlantısı](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")


### Çok Siteli

VNet'iniz ve birden fazla şirket içi ağınız arasında bir VPN ağ geçidi bağlantısı oluşturup yapılandırabilirsiniz. Birden fazla bağlantıyla çalışırken Yol Tabanlı VPN türü (klasik VNet'ler için dinamik ağ geçidi) kullanmanız gerekir. Bir VNet'in yalnızca bir sanal ağ geçidi olabileceğinden VPN ağ geçidi boyunca tüm bağlantılar mevcut bant genişliğini paylaşır. Bu tür genellikle "çok siteli" bağlantı olarak adlandırılır.
 

![Çok Siteli bağlantı](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Siteden Siteye ve Çok Siteli için dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## Sanal Ağdan Sanal Ağa

Bir sanal ağı başka bir sanal ağa bağlamak (VNet'ten VNet'e), bir VNet'i şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Hatta Sanal Ağdan Sanal Ağa iletişimini çok siteli bağlantı yapılandırmalarıyla bile birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

Bağladığınız VNet'ler,

- aynı veya farklı bölgelerde olabilir
- aynı veya farklı aboneliklerde olabilir 
- aynı veya farklı dağıtım modellerinde olabilir


![Sanal Ağdan Sanal Ağa bağlantı](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")

#### Dağıtım modelleri arasındaki bağlantılar

Azure'ın şu anda iki dağıtım modeli vardır: Klasik ve Resource Manager. Azure'ı bir süredir kullanıyorsanız klasik VNet'te çalışan Azure VM'leriniz ve örnek rollerinizin olması olasıdır. Daha yeni VM'leriniz ve rol örnekleriniz Resource Manager'da oluşturulan bir VNet'te çalışıyor olabilir. Bir VNet'teki kaynakların bir diğerindeki kaynaklarla doğrudan iletişim kurabilmesini sağlamak üzere VNet'ler arasında bir bağlantı oluşturabilirsiniz.

#### VNet eşlemesi

Sanal ağınız belirli gereksinimleri karşılıyorsa bağlantınızı oluşturmak için VNet eşlemesini kullanabilirsiniz. VNet eşlemesi sanal ağ geçidini kullanmaz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md) şu anda Önizlemede.


### Sanal Ağdan Sanal Ağa dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## Noktadan Siteye

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır. P2S bağlantılarının çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. İstemci bilgisayardan başlatarak VPN bağlantısını kurarsınız. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da bir VNet'e bağlanması gereken yalnızca birkaç istemciniz bulunduğunda bu ideal bir çözümdür. Her iki bağlantı için tüm yapılandırma gereksinimlerinin uyumlu olması şartıyla P2S bağlantıları aynı VPN ağ geçidi üzerinden S2S bağlantılarıyla birlikte kullanılabilir.


![Noktadan siteye bağlantılar](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Noktadan Siteye dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Bir ExpressRoute bağlantısında sanal ağ geçidi 'Vpn' yerine 'ExpressRoute' ile yapılandırılır. ExpressRoute hakkında daha fazla bilgi için bkz: [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md).


## Siteden Siteye ve ExpressRoute eşzamanlı bağlantıları

ExpressRoute, WAN bağlantınızdan (genel İnternet üzerinden değil) Azure dahil olmak üzere Microsoft Hizmetlerine doğrudan, özel olarak gerçekleştirilen bir bağlantıdır. Siteden Siteye VPN trafiği genel İnternet üzerinden şifrelenmiş olarak hareket eder. Aynı sanal ağ için Siteden Siteye VPN ve ExpressRoute bağlantıları yapılandırabiliyor olmanın çeşitli avantajları vardır.

ExpressRoute için güvenli bir yük devretme yolu olarak Siteden Siteye VPN yapılandırabilir veya ağınızın parçası olmayıp ExpressRoute üzerinden bağlanılan sitelere bağlanmak için Siteden Siteye VPN'ler kullanabilirsiniz. Bu yapılandırma aynı sanal ağ için biri -GatewayType Vpn, diğeri -GatewayType ExpressRoute kullanan iki sanal ağ geçidinin kullanılmasını gerektirir.


![Eşzamanlı bağlantı](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### S2S ve ExpressRoute dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Sonraki adımlar

VPN ağ geçidi yapılandırmanızı planlayın. Bkz. [VPN Gateway Planlama ve Tasarımı](vpn-gateway-plan-design.md) ile [Şirket içi ağınızı Azure'a bağlama](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 



<!--HONumber=sep16_HO1-->


