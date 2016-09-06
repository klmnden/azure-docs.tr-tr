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
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# VPN Gateway hakkında


VPN Gateway sanal ağlar ve şirket içi konumlara arasında ağ trafiği göndermek için kullanılan bir kaynak koleksiyonudur. Ağ geçitleri Siteden Siteye, Noktadan Siteye ve ExpressRoute bağlantıları için kullanılır. VPN Gateway ayrıca Azure’da (VNet-VNet) birden çok sanal ağ arasında trafik göndermek için de kullanılır. 

Bağlantı oluşturmak için bir VNet'e sanal ağ geçidi ekleyip diğer VPN Gateway kaynaklarını ve ayarlarını yapılandırırsınız. Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir.

Ağ geçidi gereksinimleri hakkında bilgi için bkz. [Ağ Geçidi Gereksinimleri](vpn-gateway-about-vpn-gateway-settings.md#requirements). Tahmin edilen toplam verimlilik için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md#aggthroughput). Fiyatlandırma için bkz. [VPN Gateway Fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway). Abonelikler ve hizmet sınırlamaları için bkz. [Ağ Limitleri](../articles/azure-subscription-service-limits.md#networking-limits).

VPN Gateway'i yapılandırırken kullanacağınız yönergeler, sanal ağınızı oluşturmak için kullandığınız dağıtım modeline bağlıdır. Örneğin,VNet'inizi klasik dağıtım modeli kullanarak oluşturduysanız VPN ağ geçidi ayarlarınızı oluşturmak ve yapılandırmak için klasik dağıtım modeline ilişkin yönergeleri kullanırsınız. Daha fazla bilgi için bkz. [Resource Manager ve klasik dağıtım modellerini anlama](../resource-manager-deployment-model.md).

Aşağıdaki bölümlerde, yapılandırmaya ilişkin şu bilgilerin yer aldığı tablolar bulunur:

- kullanılabilir dağıtım modeli
- kullanılabilir yapılandırma araçları
- varsa sizi doğrudan bir makaleye yönlendiren bağlantılar


Gereksinimlerinize uygun yapılandırma topolojisini seçmenize yardımcı olması için diyagramları ve açıklamaları kullanabilirsiniz. Diyagramlarda temel topolojilerin başlıca olanları gösterilmektedir ancak diyagramları bir kılavuz olarak kullanıp daha karmaşık yapılandırmalar da oluşturabilirsiniz. Her yapılandırma, seçtiğiniz VPN Gateway ayarlarını kullanır.

### VPN Gateway ayarlarını yapılandırma

VPN Gateway bir kaynak koleksiyonu olduğundan, kaynakların bazılarını bir araç kullanarak yapılandırabilir ve ardından farklı kaynakları yapılandırmak üzere başka bir araca geçiş yapabilirsiniz. Şu anda Azure portalında her VPN ağ geçidi kaynak ayarını yapılandıramazsınız. Her yapılandırmaya ilişkin makaledeki yönergelerde, belirli bir aracın gerekip gerekmediği belirtilmiştir. Klasik dağıtım modeliyle çalışıyorsanız bu aşamada klasik portalda çalışmak veya PowerShell kullanmak isteyebilirsiniz. Kullanılabilen ayarların her biri ile ilgili bilgi edinmek için bkz. [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).



## Siteden Siteye ve Çok Siteli

### Siteden Siteye

Siteden Siteye (S2S) bağlantı, IPSec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bu bağlantı türü için, şirket içinde ortak IP adresi atanmış olan ve NAT'nin arkasında bulunmayan bir VPN cihazı gerekir. S2S bağlantıları, şirket içi ve dışı yapılandırmalar ile birlikte karma yapılandırmalar için kullanılabilir.   

![S2S bağlantısı](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")


### Çok Siteli

VNet'iniz ve birden fazla şirket içi ağınız arasında bir VPN bağlantısı oluşturup yapılandırabilirsiniz. Birden fazla bağlantıyla çalışırken yol tabanlı VPN türü (klasik VNet'ler için dinamik ağ geçidi) kullanmanız gerekir. Bir VNet'in yalnızca bir sanal ağ geçidi olabileceğinden, ağ geçidi boyunca tüm bağlantılar mevcut bant genişliğini paylaşır. Bu yapılandırma türü genellikle "çok siteli" bağlantı olarak adlandırılır.
 

![Çok Siteli bağlantı](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## Sanal Ağdan Sanal Ağa

Bir sanal ağı başka bir sanal ağa bağlamak (VNet'ten VNet'e), bir VNet'i şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de Azure VPN ağ geçidini kullanarak IPsec/IKE ile güvenli bir tünel sunar. Hatta Sanal Ağdan Sanal Ağa iletişimini çok siteli yapılandırmalarla bile birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

Bağladığınız VNet'ler,

- aynı veya farklı bölgelerde olabilir
- aynı veya farklı aboneliklerde olabilir 
- aynı veya farklı dağıtım modellerinde olabilir



![Sanal Ağdan Sanal Ağa bağlantı](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")



### Dağıtım modelleri arasındaki bağlantılar

Azure'ın şu anda iki dağıtım modeli vardır: Klasik ve Resource Manager. Azure'ı bir süredir kullanıyorsanız klasik VNet'te çalışan Azure VM'leriniz ve örnek rollerinizin olması olasıdır. Daha yeni VM'leriniz ve rol örnekleriniz Resource Manager'da oluşturulan bir VNet'te çalışıyor olabilir. Bir VNet'teki kaynakların bir diğerindeki kaynaklarla doğrudan iletişim kurabilmesini sağlamak üzere VNet'ler arasında bir bağlantı oluşturabilirsiniz.


### Dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

### VNet eşlemesi

Sanal ağ yapılandırmanız belirli gereksinimleri karşılıyorsa bağlantınızı oluşturmak için VNet eşlemesini kullanabilirsiniz. VNet eşlemesi sanal ağ geçidini kullanmaz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md) şu anda Önizlemede.



## Noktadan Siteye

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır. P2S bağlantılarının çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. İstemci bilgisayardan başlatarak VPN bağlantısını kurarsınız. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da bir VNet'e bağlanması gereken yalnızca birkaç istemciniz bulunduğunda bu ideal bir çözümdür. 


![Noktadan siteye bağlantılar](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

ExpressRoute hakkında daha fazla bilgi için bkz: [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md).


## Siteden Siteye ve ExpressRoute eşzamanlı bağlantıları

ExpressRoute, WAN bağlantınızdan (genel İnternet üzerinden değil) Azure dahil olmak üzere Microsoft Hizmetlerine doğrudan, özel olarak gerçekleştirilen bir bağlantıdır. Siteden Siteye VPN trafiği genel İnternet üzerinden şifrelenmiş olarak hareket eder. Aynı sanal ağ için Siteden Siteye VPN ve ExpressRoute bağlantıları yapılandırabiliyor olmanın çeşitli avantajları vardır.

ExpressRoute için güvenli bir yük devretme yolu olarak Siteden Siteye VPN yapılandırabilir veya ağınızın parçası olmayıp ExpressRoute üzerinden bağlanılan sitelere bağlanmak için Siteden Siteye VPN'ler kullanabilirsiniz. Bu yapılandırma, aynı sanal ağ için biri -GatewayType Vpn, diğeri -GatewayType ExpressRoute kullanan iki sanal ağ geçidinin kullanılmasını gerektirir.


![Eşzamanlı bağlantı](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Dağıtım modelleri ve yöntemleri

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Sonraki adımlar

VPN Gateway hakkında daha fazla bilgi edinmek için bkz. [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md)

Şirket içi konumunuzu bir VNet'e bağlayın. Bkz. [Siteden Siteye Bağlantı Oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md).





 



<!--HONumber=ago16_HO5-->


