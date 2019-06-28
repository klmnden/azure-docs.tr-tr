---
title: Azure Sanal Ağı | Microsoft Docs
description: Azure Sanal Ağı kavramları ve özellikleri hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anavinahar
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: anavin
ms.openlocfilehash: 0d6762c8f3034923ddc0fe7dcf0cc2df34bd3629
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332114"
---
# <a name="what-is-azure-virtual-network"></a>Azure Sanal Ağı nedir?

Azure sanal ağı (VNet), azure'daki özel ağınızın temel yapı taşıdır. VNet birçok türde Azure kaynakları gibi Azure sanal makineler (güvenli bir şekilde birbiriyle, internet iletişim kurmak için VM) etkinleştirir ve şirket içi ağlar. VNet, kendi veri merkezinizde çalışan, ancak onunla getirir, geleneksel bir ağa benzer Azure altyapısı ölçek, kullanılabilirlik ve yalıtımı gibi ek avantajları.

## <a name="vnet-concepts"></a>Sanal ağ kavramları

- **Adres alanı:** Sanal ağ oluştururken, genel ve özel (RFC 1918) adresleri kullanarak özel bir özel IP adres alanı belirtmeniz gerekir. Azure, bir sanal ağdaki kaynaklara, atadığınız adres alanından özel bir IP adresi atar. Örneğin, bir VNet adres alanı ile bir VM dağıtırsanız, 10.0.0.0/16, sanal Makinenin 10.0.0.4 gibi özel bir IP atanır.
- **Alt ağlar:** Alt ağlar, sanal ağ bir veya daha fazla alt ağlara segmentlere ayırın ve her alt ağ sanal ağın adres alanının bir bölümü ayırın sağlar. Ardından, belirli bir alt ağdaki Azure kaynaklarını da dağıtabilirsiniz. Gibi geleneksel bir ağa, alt ağlar, VNet adres alanınızı kuruluşunuzun iç ağ için uygun olan parçalara bölmek olanak tanır. Bu da adresi ayırma verimliliği artırır. Ağ güvenlik grupları kullanarak alt ağlar içindeki kaynakların güvenliğini sağlayabilirsiniz. Daha fazla bilgi için [güvenlik grupları](/security-overview.md).
- **Bölgeleri**: Sanal ağ, tek bir bölge/konum kapsamlıdır; Ancak, farklı bölgelerdeki birden fazla sanal ağ sanal ağ eşlemesi kullanarak birbirine bağlanabilir.
- **Abonelik:** Sanal ağ aboneliği kapsamlıdır. Her Azure [aboneliğinde](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ve Azure [bölgesinde](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) birden çok sanal ağ uygulayabilirsiniz.

## <a name="best-practices"></a>En iyi uygulamalar

Azure'daki ağınızı oluştururken aşağıdaki Evrensel tasarım ilkelerini göz önünde bulundurmanız önemlidir:

- Adres alanları çakışmamalıdır emin olun. VNet adres alanınızı (CIDR bloğu) mevcut olduğundan emin olun kullanıcının diğer ağ Aralıklarınızın kuruluşunuz ile çakışıyor.
- Alt ağlarınızı vnet'in tüm adres alanını kapsamalıdır değil. Önceden planlayın ve geleceğe yönelik bazı adres alanı ayırın.
- Daha az büyük sanal ağları daha küçük birden fazla sanal ağ olması önerilir. Bu, yönetim yükünü engeller.
- Ağ güvenlik grupları (Nsg'ler) kullanarak Vnet'inizin güvenliğini sağlama.

## <a name="communicate-with-the-internet"></a>İnternet ile iletişim kurma

Bir sanal ağ içindeki tüm kaynaklara İnternet'e giden varsayılan olarak iletişim kurabilir. Bir kaynağa genel IP adresi veya genel Load Balancer atayarak o kaynağa gelen yönde iletişim kurabilirsiniz. Giden bağlantılarınızı yönetmek için genel IP adresi veya genel Load Balancer da kullanabilirsiniz.  Azure'daki giden bağlantılar hakkında daha fazla bilgi edinmek için bkz. [Giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md), [Genel IP adresleri](virtual-network-public-ip-address.md) ve [Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
>Yalnızca sistem içi [Standart Load Balancer](../load-balancer/load-balancer-standard-overview.md) kullanıldığında [giden bağlantıların](../load-balancer/load-balancer-outbound-connections.md) örnek düzeyinde genel IP veya genel Load Balancer ile nasıl çalışacağını tanımlamadığınız sürece giden bağlantı kullanılamaz.

## <a name="communicate-between-azure-resources"></a>Azure kaynakları arasında iletişim kurma

Azure kaynakları, aşağıdaki yöntemlerden birini uygulayarak birbiriyle güvenli şekilde iletişim kurar:

- **Bir sanal ağ üzerinden**: Azure App Service ortamları, Azure Kubernetes Service (AKS) ve Azure sanal makine ölçek kümeleri gibi sanal ağ için VM'ler ve diğer çeşitli Azure kaynaklarını dağıtın. Bir sanal ağa dağıtabileceğiniz Azure kaynaklarının tam listesini görüntülemek için bkz. [Sanal ağ hizmeti tümleştirmesi](virtual-network-for-azure-services.md).
- **Bir sanal ağ hizmet uç noktası aracılığıyla**: Sanal ağ özel adres alanınızı ve Azure depolama hesapları ve Azure SQL veritabanları gibi Azure hizmet kaynakları için sanal ağınızın kimliğini doğrudan bağlantı üzerinden genişletin. Hizmet uç noktaları, kritik Azure hizmeti kaynaklarınızı yalnızca bir sanal ağla sınırlayarak güvenliğini sağlamanıza imkan verir. Daha fazla bilgi için bkz. [Sanal ağ hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md).
- **VNet eşlemesi aracılığıyla**: Sanal ağları birbirine bağlayarak sanal ağ eşlemesi yoluyla herhangi bir sanal ağdaki kaynakların birbiriyle iletişim kurmasını sağlayabilirsiniz. Bağlandığınız sanal ağlar aynı veya farklı Azure bölgelerinde bulunabilir. Daha fazla bilgi edinmek için bkz. [Sanal ağ eşlemesi](virtual-network-peering-overview.md).

## <a name="communicate-with-on-premises-resources"></a>Şirket içi kaynaklarla iletişim kurma

Aşağıdaki seçenekleri bir arada kullanarak şirket içi bilgisayarlarınızı ve ağlarınızı bir sanal ağa bağlayabilirsiniz:

- **Noktadan siteye sanal özel ağ (VPN):** Ağınızdaki tek bir bilgisayar ile sanal ağ arasındaki kurdu. Bir sanal ağ ile bağlantı kurmak isteyen her bilgisayar, bağlantısını yapılandırmalıdır. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Bilgisayarınız ile bir sanal ağ arasındaki iletişim, İnternet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Noktadan siteye VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **Siteden siteye VPN:** Şirket içi VPN cihazınız ile bir sanal ağda dağıtılan Azure VPN Gateway arasında kurdu. Bu bağlantı türü, yetkilendirdiğiniz şirket içi kaynakların bir sanal ağa erişmesini sağlar. Şirket içi VPN cihazınız ile Azure VPN ağ geçidi arasındaki iletişim, İnternet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Siteden siteye VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla ağınız ve Azure arasında kurdu. Bu bağlantı özeldir. Trafik, İnternet üzerinden geçmez. Daha fazla bilgi edinmek için bkz. [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Ağ trafiğini filtreleme

Aşağıdaki seçeneklerden birini veya her ikisini de kullanarak alt ağlar arasındaki ağ trafiğini filtreleyebilirsiniz:

- **Güvenlik grupları:** Ağ güvenlik grupları ve uygulama güvenlik grupları, kaynakları kaynak ve hedef IP adresi, bağlantı noktası ve protokol gelen ve giden trafiği filtrelemek için olanak sağlayan birden çok gelen ve giden güvenlik kuralı içerebilir. Daha fazla bilgi için bkz. [ağ güvenlik grupları](security-overview.md#network-security-groups) veya [uygulama güvenlik grupları](security-overview.md#application-security-groups).
- **Ağ sanal Gereçleri:** Bir ağ sanal Gereci, bir güvenlik duvarı, WAN iyileştirmesi veya diğer ağ işlevi gibi bir ağ işlevi gerçekleştiren bir vm'dir. Bir sanal ağda dağıtabileceğiniz kullanılabilir ağ sanal gereçleri listesini görüntülemek için bkz. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Ağ trafiğini yönlendirme

Azure varsayılan olarak alt ağlar, bağlı sanal ağlar, şirket içi ağlar ve İnternet arasındaki trafiği yönlendirir. Azure’ın oluşturduğu varsayılan rotaları geçersiz kılmak için aşağıdaki seçeneklerden birini veya her ikisini uygulayabilirsiniz:

- **Rota tabloları:** Trafiği için her alt ağ için yönlendirildiği denetleyen rotalarla özel rota tabloları oluşturabilirsiniz. [Rota tabloları](virtual-networks-udr-overview.md#user-defined) hakkında daha fazla bilgi edinin.
- **Ağ Geçidi Protokolü (BGP) rotaları sınır:** Sanal ağınızı bir Azure VPN Gateway veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza bağlanırsa, şirket içi BGP rotalarınızı sanal ağlarınıza yayabilir. [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange) ile BGP’yi kullanma hakkında daha fazla bilgi edinin.

## <a name="azure-vnet-limits"></a>Azure sanal ağ limitleri

Dağıtabileceğiniz Azure kaynaklarının sayısını etrafında belirli bir sınırı yoktur. Çoğu Azure ağ sınırları en fazla değerlerdir. Ancak, [belirli ağ sınırları artırmak](../azure-supportability/networking-quota-requests.md) belirtildiği [VNet sınırları sayfasından](../azure-subscription-service-limits.md#networking-limits). 

## <a name="pricing"></a>Fiyatlandırma

Azure sanal ağ kullanmak için ücret alınmaz, ücretsizdir. Sanal makineler (VM) gibi kaynakları ve diğer ürünler için standart ücretler uygulanabilir. Daha fazla bilgi için bkz. [sanal ağ fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network/) ve Azure [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/).

## <a name="next-steps"></a>Sonraki adımlar

 Bir sanal ağı kullanmaya başlamak için bir sanal ağ oluşturun, bu sanal ağa birkaç sanal makine dağıtın ve sanal makineler arasında iletişim kurun. Nasıl olduğunu öğrenmek için [Sanal ağ oluşturma](quick-create-portal.md) başlıklı hızlı başlangıca bakın.
