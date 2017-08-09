---
title: "VPN Gateway’e Genel Bakış: Azure sanal ağları ile şirketler arası VPN bağlantıları oluşturma | Microsoft Docs"
description: "Bu VPN Gateway’e Genel Bakış bölümünde Internet üzerinden bir VPN bağlantısı kullanılarak Azure sanal ağlarına bağlanma yolları açıklanmaktadır. Temel bağlantı yapılandırmaları da buraya eklenmiştir."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 0f26a9b62a376daf2b1314ff5972293a2bc7f379
ms.contentlocale: tr-tr
ms.lasthandoff: 07/28/2017

---
# <a name="about-vpn-gateway"></a>VPN Gateway hakkında

VPN ağ geçidi, ortak bağlantı üzerinden şirket içi konuma şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. Ayrıca VPN ağ geçitlerini, Microsoft ağı üzerinden Azure sanal ağları arasında şifrelenmiş trafik göndermek için de kullanabilirsiniz. Azure sanal ağınız ile şirket içi siteniz arasında şifrelenmiş ağ trafiği göndermek için sanal ağınıza ilişkin bir VPN ağ geçidi oluşturmanız gerekir.

Her bir sanal ağ yalnızca bir VPN ağ geçidi içerebilir ancak aynı VPN ağ geçidi için birden çok bağlantı oluşturabilirsiniz. Çok Siteli bağlantı yapılandırması bunun bir örneğidir. Aynı VPN ağ geçidi ile birden fazla bağlantı oluşturduğunuzda, Noktadan Siteye VPN’ler dahil tüm VPN tünelleri, ağ geçidinin bant genişliğini paylaşır.

### <a name="what-is-a-virtual-network-gateway"></a>Sanal ağ geçidi nedir?

Sanal ağ geçidi GatewaySubnet adı verilen belirli bir alt ağa dağıtılan iki veya daha fazla sanal makineden oluşur. GatewaySubnet'te bulunan VM'ler, sanal ağ geçidini ayarladığınızda oluşturulur. Sanal ağ geçidi VM'leri, ağ geçidine özgü yönlendirme tablolarını ve ağ geçidi hizmetlerini içerecek şekilde yapılandırılır. Sanal ağ geçidinin parçası olan VM'leri doğrudan yapılandıramazsınız ve GatewaySubnet'e hiçbir koşulda ek kaynak dağıtmamanız gerekir.

"Vpn" ağ geçidi türünü kullanarak sanal ağ geçidi oluşturduğunuzda, trafiği şifreleyen özel bir sanal ağ geçidi türü olan VPN ağ geçidi oluşturulur. Bir VPN ağ geçidinin oluşturulması 45 dakika sürebilir. Bu süre içinde VPN ağ geçidine ilişkin VM'ler, GatewaySubnet'e dağıtılır ve belirttiğiniz ayarlarla yapılandırılır. Seçtiğiniz Gateway SKU'su VM'lerin gücünü belirler.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

## <a name="configuring-a-vpn-gateway"></a>VPN Gateway yapılandırma

VPN ağ geçidi bağlantısı belirli ayarlarla yapılandırılmış birden fazla kaynağı kullanır. Kaynakların birçoğu ayrı ayrı yapılandırılabilir, ancak bazı durumlarda belirli bir sırayla yapılandırılması gerekir.

### <a name="settings"></a>Ayarlar

Her kaynak için seçtiğiniz ayarlar başarılı bir bağlantı oluşturmak için önemlidir. VPN Gateway’e ilişkin kaynaklar ve ayarlar hakkında bilgi için bkz. [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md). Makale; ağ geçidi türleri, VPN türleri, bağlantı türleri, ağ geçidi alt ağları, yerel ağ geçitleri ve kullanmayı düşünebileceğiniz diğer çeşitli ayarlar hakkında bilgi içerir.

### <a name="deployment-tools"></a>Dağıtım araçları

Azure portalı gibi bir yapılandırma aracını kullanarak kaynakları oluşturmaya ve yapılandırmaya başlayabilirsiniz. Daha sonra ek kaynaklar yapılandırmak ya da uygun olduğunda var olan kaynakları değiştirmek için PowerShell gibi başka bir araca geçmeye karar verebilirsiniz. Şu anda Azure portalında her kaynağı ve kaynak ayarını yapılandıramazsınız. Her bağlantı topolojisine ilişkin makalelerdeki yönergelerde, belirli bir aracının ne zaman gerekli olduğu belirtilmektedir. 

### <a name="deployment-model"></a>Dağıtım modeli

VPN Gateway'i yapılandırırken uygulayacağınız adımlar, sanal ağınızı oluşturmak için kullandığınız dağıtım modeline bağlıdır. Örneğin,VNet'inizi klasik dağıtım modeli kullanarak oluşturduysanız VPN ağ geçidi ayarlarınızı oluşturmak ve yapılandırmak için klasik dağıtım modeline ilişkin yönergeleri kullanırsınız. Dağıtım modelleri hakkında daha fazla bilgi için bkz. [Resource Manager’ı ve klasik dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="diagrams"></a>Bağlantı topolojisi diyagramları

VPN ağ geçidi bağlantıları için kullanılabilecek farklı yapılandırmalar vardır. Gereksinimlerinize en uygun yapılandırmayı belirlemeniz gerekir. Aşağıdaki bölümlerde, aşağıdaki VPN ağ geçidi bağlantıları hakkında bilgi ve topoloji diyagramlarını görüntüleyebilirsiniz: Aşağıdaki bölümlerde şu listeleri içeren tablolar bulunur:

* Kullanılabilir dağıtım modeli
* Kullanılabilir yapılandırma araçları
* Varsa sizi doğrudan bir makaleye yönlendiren bağlantılar

Gereksinimlerinize uygun bağlantı topolojisini seçmenize yardımcı olması için diyagramları ve açıklamaları kullanabilirsiniz. Diyagramlarda temel topolojilerin başlıca olanları gösterilmektedir ancak diyagramları bir kılavuz olarak kullanıp daha karmaşık yapılandırmalar da oluşturabilirsiniz.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Siteden Siteye ve Çok Siteli (IPsec/IKE VPN tüneli)

### <a name="S2S"></a>Siteden Siteye

Siteden Siteye (S2S) VPN ağ geçidi bağlantısı, IPSec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bir S2S bağlantısı, şirket içinde ortak IP adresi atanmış olan ve NAT'nin arkasında bulunmayan bir VPN cihazı gerektirir. S2S bağlantıları, şirket içi ve dışı yapılandırmalar ile birlikte karma yapılandırmalar için kullanılabilir.   

![Azure VPN Gateway Siteden Siteye bağlantı örneği](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Çok Siteli

Bu türden bir bağlantı, Siteden Siteye bağlantının bir çeşididir. Sanal ağ geçidinizden genellikle birden fazla şirket içi siteye bağlanan birden fazla VPN bağlantısı oluşturursunuz. Birden fazla bağlantıyla çalışırken Yol Tabanlı VPN türü (klasik sanal ağlar ile çalışırken “dinamik ağ geçidi” adıyla kullanılır) kullanmanız gerekir. Her sanal ağın yalnızca bir VPN ağ geçidi olabileceğinden, ağ geçidi boyunca tüm bağlantılar mevcut bant genişliğini paylaşır. Bu tür genellikle "çok siteli" bağlantı olarak adlandırılır.

![Azure VPN Gateway Çok Siteli bağlantı örneği](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Siteden Siteye ve Çok Siteli için dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Noktadan Siteye (SSTP üzerinden VPN)

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır. S2S bağlantılarının aksine, P2S bağlantıları için şirket içi genel kullanıma yönelik IP adresi veya VPN cihazı gerekmez. İstemci bilgisayardan başlatarak VPN bağlantısını kurarsınız. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da bir VNet'e bağlanması gereken yalnızca birkaç istemciniz bulunduğunda bu ideal bir çözümdür. Her iki bağlantı için tüm yapılandırma gereksinimlerinin uyumlu olması şartıyla P2S bağlantıları aynı VPN ağ geçidi üzerinden S2S bağlantılarıyla birlikte kullanılabilir.

![Azure VPN Gateway Noktadan Siteye bağlantı örneği](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>Noktadan Siteye dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>Sanal Ağdan Sanal Ağa bağlantılar (IPsec/IKE VPN tüneli)

Bir sanal ağı başka bir sanal ağa bağlamak (VNet'ten VNet'e), bir VNet'i şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Hatta Sanal Ağdan Sanal Ağa iletişimini çok siteli bağlantı yapılandırmalarıyla bile birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

Bağladığınız VNet'ler,

* aynı veya farklı bölgelerde olabilir
* aynı veya farklı aboneliklerde olabilir 
* aynı veya farklı dağıtım modellerinde olabilir

![Azure VPN Gateway Sanal Ağdan Sanal Ağa bağlantı örneği](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

### <a name="connections-between-deployment-models"></a>Dağıtım modelleri arasındaki bağlantılar

Azure'ın şu anda iki dağıtım modeli vardır: Klasik ve Resource Manager. Azure'ı bir süredir kullanıyorsanız klasik VNet'te çalışan Azure VM'leriniz ve örnek rollerinizin olması olasıdır. Daha yeni VM'leriniz ve rol örnekleriniz Resource Manager'da oluşturulan bir VNet'te çalışıyor olabilir. Bir VNet'teki kaynakların bir diğerindeki kaynaklarla doğrudan iletişim kurabilmesini sağlamak üzere VNet'ler arasında bir bağlantı oluşturabilirsiniz.

### <a name="vnet-peering"></a>VNet eşlemesi

Sanal ağınız belirli gereksinimleri karşılıyorsa bağlantınızı oluşturmak için VNet eşlemesini kullanabilirsiniz. VNet eşlemesi sanal ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Sanal Ağdan Sanal Ağa dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (adanmış özel bağlantı)

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VP), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir. 

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.

ExpressRoute bağlantısı bir VPN ağ geçidi kullanmaz, ancak zorunlu yapılandırmasının bir parçası olarak sanal ağ geçidi kullanır. Bir ExpressRoute bağlantısında sanal ağ geçidi 'Vpn' yerine 'ExpressRoute' ile yapılandırılır. ExpressRoute hakkında daha fazla bilgi için bkz: [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Siteden Siteye ve ExpressRoute eşzamanlı bağlantıları

ExpressRoute, WAN bağlantınızdan (genel İnternet üzerinden değil) Azure dahil olmak üzere Microsoft Hizmetlerine doğrudan, özel olarak gerçekleştirilen bir bağlantıdır. Siteden Siteye VPN trafiği genel İnternet üzerinden şifrelenmiş olarak hareket eder. Aynı sanal ağ için Siteden Siteye VPN ve ExpressRoute bağlantıları yapılandırabiliyor olmanın çeşitli avantajları vardır.

ExpressRoute için güvenli bir yük devretme yolu olarak Siteden Siteye VPN yapılandırabilir veya ağınızın parçası olmayıp ExpressRoute üzerinden bağlanılan sitelere bağlanmak için Siteden Siteye VPN'ler kullanabilirsiniz. Bu yapılandırma, aynı sanal ağ için biri ‘Vpn’, diğeri ‘ExpressRoute’ ağ geçidi türünü kullanan iki sanal ağ geçidinin kullanılmasını gerektirir.

![ExpressRoute ve VPN Gateway eşzamanlı bağlantı örneği](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>S2S ve ExpressRoute dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Fiyatlandırma

[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

VPN Gateway’e yönelik ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="faq"></a>SSS

VPN Gateway hakkında sık sorulan sorular için bkz. [VPN Gateway hakkında SSS](vpn-gateway-vpn-faq.md).

## <a name="next-steps"></a>Sonraki adımlar

- VPN ağ geçidi yapılandırmanızı planlayın. Bkz. [VPN Gateway için Planlama ve Tasarım](vpn-gateway-plan-design.md).
- Daha fazla bilgi edinmek için [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md) makalesini görüntüleyin.
- [Abonelik ve hizmet sınırlamaları](../azure-subscription-service-limits.md#networking-limits) makalesini görüntüleyin.
- Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
