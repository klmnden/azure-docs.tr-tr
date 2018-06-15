---
title: Azure Sanal Ağı | Microsoft Docs
description: Azure Sanal Ağı kavramları ve özellikleri hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/23/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 851c8c1eb13497355038ef4a8d5f1f9326c8c3bc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33781189"
---
# <a name="what-is-azure-virtual-network"></a>Azure Sanal Ağı nedir?

Azure Sanal Ağı, Azure Sanal Makineleri (VM) gibi birçok Azure kaynağı türünün birbiriyle, İnternet ile ve şirket içi ağlarla güvenli şekilde iletişim kurmasına olanak sağlar. Azure Sanal Ağı aşağıdaki temel özellikleri sağlar:

## <a name="isolation-and-segmentation"></a>Yalıtma ve segmentasyon

Her Azure [aboneliğinde](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ve Azure [bölgesinde](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) birden çok sanal ağ uygulayabilirsiniz. Her sanal ağ, diğer sanal ağlardan yalıtılır. Her sanal ağ için şunları yapabilirsiniz:
- Genel ve özel (RFC 1918) adresleri kullanarak özel bir gizli IP adresi alanı belirtin. Azure, bir sanal ağdaki kaynaklara, atadığınız adres alanından özel bir IP adresi atar.
- Sanal ağı bir veya daha fazla alt ağa bölün ve sanal ağın adres alanının bir kısmını her bir alt ağa ayırın.
- Bir sanal ağdaki kaynaklar tarafından kullanılmak üzere kendi DNS sunucunuzu belirtin veya Azure tarafından sağlanan ad çözümlemesini kullanın.

## <a name="communicate-with-the-internet"></a>İnternet ile iletişim kurma

Bir sanal ağdaki tüm kaynaklar varsayılan olarak İnternet’e giden yönde iletişim kurabilir. Bir kaynağa genel IP adresi atayarak o kaynağa gelen yönde iletişim kurabilirsiniz. Daha fazla bilgi için bkz. [Genel IP adresleri](virtual-network-public-ip-address.md).

## <a name="communicate-between-azure-resources"></a>Azure kaynakları arasında iletişim kurma

Azure kaynakları, aşağıdaki yöntemlerden birini uygulayarak birbiriyle güvenli şekilde iletişim kurar:

- **Bir sanal ağ üzerinden**: Azure App Service Ortamları, Azure Kubernetes Service (AKS) ve Azure Sanal Makine Ölçek Kümeleri gibi sanal makineleri ve diğer birçok türde Azure kaynağını bir sanal ağa dağıtabilirsiniz. Bir sanal ağa dağıtabileceğiniz Azure kaynaklarının tam listesini görüntülemek için bkz. [Sanal ağ hizmeti tümleştirmesi](virtual-network-for-azure-services.md). 
- **Bir sanal ağ hizmeti uç noktası üzerinden**: Sanal ağ özel adres alanınızı ve sanal ağınızın kimliğini, doğrudan bir bağlantı üzerinden Azure Depolama hesapları ve Azure SQL Veritabanları gibi Azure hizmet kaynaklarına genişletin. Hizmet uç noktaları, kritik Azure hizmeti kaynaklarınızı yalnızca bir sanal ağla sınırlayarak güvenliğini sağlamanıza imkan verir. Daha fazla bilgi için bkz. [Sanal ağ hizmeti uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md).
 
## <a name="communicate-with-on-premises-resources"></a>Şirket içi kaynaklarla iletişim kurma

Aşağıdaki seçenekleri bir arada kullanarak şirket içi bilgisayarlarınızı ve ağlarınızı bir sanal ağa bağlayabilirsiniz:

- **Noktadan siteye sanal özel ağ (VPN):** Ağınızdaki tek bir bilgisayar ile sanal ağ arasında oluşur. Bir sanal ağ ile bağlantı kurmak isteyen her bilgisayar, bağlantısını yapılandırmalıdır. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Bilgisayarınız ile bir sanal ağ arasındaki iletişim, İnternet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Noktadan siteye VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **Siteden siteye VPN:** Şirket içi VPN cihazınız ile bir sanal ağda dağıtılan Azure VPN Gateway arasında oluşur. Bu bağlantı türü, yetkilendirdiğiniz şirket içi kaynakların bir sanal ağa erişmesini sağlar. Şirket içi VPN cihazınız ile Azure VPN ağ geçidi arasındaki iletişim, İnternet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Siteden siteye VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla ağınız ile Azure arasında oluşur. Bu bağlantı özeldir. Trafik, İnternet üzerinden geçmez. Daha fazla bilgi edinmek için bkz. [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Ağ trafiğini filtreleme
Aşağıdaki seçeneklerden birini veya her ikisini de kullanarak alt ağlar arasındaki ağ trafiğini filtreleyebilirsiniz:
- **Ağ güvenlik grupları:** Ağ güvenlik grubu, kaynaklara gelen ve kaynaklardan giden trafiği, kaynak ve hedef IP adresine, bağlantı noktasına ve protokole göre filtrelemenize olanak sağlayan birden çok gelen ve giden güvenlik kuralı içerebilir. Daha fazla bilgi için bkz. [Ağ güvenlik grupları](security-overview.md#network-security-groups).
- **Ağ sanal gereçleri:** Ağ sanal gereci; güvenlik duvarı, WAN iyileştirmesi veya diğer ağ işlevi gibi ağ işlevlerini gerçekleştiren bir sanal makinedir. Bir sanal ağda dağıtabileceğiniz kullanılabilir ağ sanal gereçleri listesini görüntülemek için bkz. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Ağ trafiğini yönlendirme

Azure varsayılan olarak alt ağlar, bağlı sanal ağlar, şirket içi ağlar ve İnternet arasındaki trafiği yönlendirir. Azure’ın oluşturduğu varsayılan rotaları geçersiz kılmak için aşağıdaki seçeneklerden birini veya her ikisini uygulayabilirsiniz:
- **Rota tabloları:** Her bir alt ağ için trafiğin nereye yönlendirileceğini denetleyen rotalarla özel rota tabloları oluşturabilirsiniz. [Rota tabloları](virtual-networks-udr-overview.md#user-defined) hakkında daha fazla bilgi edinin.
- **Sınır ağ geçidi protokolü (BGP) rotaları:** Azure VPN Gateway veya ExpressRoute bağlantısı kullanarak sanal ağınızı şirket içi ağınıza bağlarsanız, şirket içi BGP rotalarınızı sanal ağlarınıza yayabilirsiniz. [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange) ile BGP’yi kullanma hakkında daha fazla bilgi edinin.

## <a name="connect-virtual-networks"></a>Sanal ağları bağlama

Sanal ağları birbirine bağlayarak sanal ağ eşlemesi yoluyla herhangi bir sanal ağdaki kaynakların birbiriyle iletişim kurmasını sağlayabilirsiniz. Bağlandığınız sanal ağlar aynı veya farklı Azure bölgelerinde bulunabilir. Daha fazla bilgi edinmek için bkz. [Sanal ağ eşlemesi](virtual-network-peering-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Sanal Ağının genel bakışını elde edebilirsiniz. Bir sanal ağı kullanmaya başlamak için bir sanal ağ oluşturun, bu sanal ağa birkaç sanal makine dağıtın ve sanal makineler arasında iletişim kurun. Nasıl olduğunu öğrenmek için [Sanal ağ oluşturma](quick-create-portal.md) başlıklı hızlı başlangıca bakın.