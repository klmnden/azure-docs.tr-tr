---
title: "Azure sanal ağı | Microsoft Docs"
description: "Azure Virtual Network kavramları ve özellikler hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 6d6afd2b9b956138ed400fbd6cabd3b480fde0f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-virtual-network"></a>Azure Sanal Ağ

Azure sanal ağ hizmeti, güvenli bir şekilde Azure kaynaklarını birbirlerine sanal ağlar (Vnet'ler) bağlanmanıza olanak sağlar. Bir VNet kendi ağ bulutta gösterimidir. Bir VNet Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtım ' dir. Ayrıca, sanal ağlar, şirket içi ağınıza bağlanabilir. Aşağıdaki resimde bazı Azure Virtual Network service özelliklerini gösterir:

![Ağ Diyagramı](./media/virtual-networks-overview/virtual-network-overview.png)

Aşağıdaki Azure sanal ağ özellikleri hakkında daha fazla bilgi edinmek için özellik tıklatın:
- **[Yalıtım:](#isolation)**  sanal ağlar birbirinden yalıtılmış. Geliştirme, test ve üretim için aynı CIDR adres bloklarını kullanan ayrı Vnet'ler oluşturabilirsiniz. Buna karşılık, ağları birbirine bağlamak ve farklı CIDR adres bloklarını kullanan birden çok sanal ağlar oluşturabilirsiniz. Bir sanal ağ birden çok alt ağa bölebilirsiniz. Azure, bir sanal ağa bağlı VM'ler ve bulut Hizmetleri rol örnekleri için dahili ad çözümlemesi sağlar. İsteğe bağlı olarak bir VNet Azure dahili ad çözümlemesi kullanmak yerine kendi DNS sunucularını kullanmak için yapılandırabilirsiniz.
- **[Internet bağlantısı:](#internet)**  bir sanal ağa bağlı tüm Azure sanal makineler (VM) ve bulut Hizmetleri rol örnekleri varsayılan olarak, Internet erişimi vardır. Belirli kaynaklara gelen erişim gerektiğinde de etkinleştirebilirsiniz.
- **[Azure kaynak bağlantısı:](#within-vnet)**  bulut Hizmetleri ve sanal makineleri gibi Azure kaynaklarını aynı Vnet'e bağlı. Farklı alt ağlarda olsalar bile, kaynakları birbirlerine özel IP adresleri kullanarak bağlanabilir. Azure, yapılandırmak ve yollar yönetmek zorunda kalmamak için varsayılan alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar.
- **[VNet bağlantısı:](#connect-vnets)**  sanal ağlar bağlanması birbirine herhangi bir kaynak başka bir VNet ile iletişim kurmak için hiçbir sanal ağa bağlı kaynaklar etkinleştirme.
- **[Şirket içi bağlantı:](#connect-on-premises)**  sanal ağlara bağlanabilir ağınız ve Azure arasında özel ağ bağlantıları üzerinden veya siteden siteye VPN bağlantısı üzerinden şirket içi ağlar Internet üzerinden.
- **[Trafik filtreleme:](#filtering)**  VM ve bulut Hizmetleri rol örneklerini ağ trafiği filtre gelen ve giden kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokolü tarafından.
- **[Yönlendirme:](#routing)**  isteğe bağlı olarak, kendi yolları yapılandırmak veya bir ağ geçidi üzerinden BGP yollarını kullanarak yönlendirme Azure'un varsayılan ayarlarını geçersiz kılabilir.

## <a name = "isolation"></a>Ağ yalıtımı ve kesimleme

Her Azure içinde birden çok sanal ağlar uygulayabilirsiniz [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ve Azure [bölge](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Her sanal ağ, diğer sanal ağlardan yalıtılır. Her sanal ağ için şunları yapabilirsiniz:
- Ortak ve özel (RFC 1918) adreslerini kullanarak özel bir özel IP adres alanını belirtin. Azure atar kaynakları Vnet'e özel bir IP adresi atadığınız adres alanından bağlı.
- Sanal ağ bir veya daha fazla alt ağlara ayırabilir ve her alt ağ için sanal ağ adres alanının bir bölümü ayırın.
- Azure tarafından sağlanan ad çözümlemesi kullanın veya bir sanal ağa bağlı kaynaklar tarafından kullanmak için kendi DNS sunucusu belirtin. Sanal ağlar ad çözümleme hakkında daha fazla bilgi için okuma [VM'ler ve bulut Hizmetleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md) makalesi.

## <a name = "internet"></a>Internet'e bağlanmak
Bir sanal ağa bağlı tüm kaynakları, varsayılan olarak giden Internet bağlantısı vardır. Özel IP adresi kaynağının çevrilmiş kaynak ağ adresine (SNAT) genel bir IP adresi Azure altyapısı tarafından ' dir. Giden Internet bağlantısı hakkında daha fazla bilgi için okuma [azure'da giden bağlantılar anlama](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) makalesi. Varsayılan bağlantı özel Yönlendirme ve trafik filtreleme uygulayarak değiştirebilirsiniz.

Azure kaynaklarına Internet'ten gelen bağlantı kurmak veya SNAT olmadan Internet'e giden iletişim kurmak için bir kaynak genel bir IP adresi atanması gerekir. Genel IP adresleri hakkında daha fazla bilgi için okuma [ortak IP adresleri](virtual-network-public-ip-address.md) makalesi.

## <a name="within-vnet"></a>Azure kaynaklarına bağlanma
Sanal makineler (VM), bulut Hizmetleri, uygulama hizmeti ortamları ve sanal makine ölçek kümeleri gibi bir VNet birkaç Azure kaynaklarına bağlanabilir. Bir alt ağ bir ağ arabirimi (NIC) aracılığıyla bir sanal ağ içindeki VM'ler bağlayın. NIC hakkında daha fazla bilgi için okuma [ağ arabirimleri](virtual-network-network-interface.md) makalesi.

## <a name="connect-vnets"></a>Sanal ağlara bağlanabilir

Sanal ağlar birbirlerine Vnet'lerde birbirleri ile iletişim kurmak için herhangi bir Vnet'e bağlı kaynakları etkinleştirme bağlayabilirsiniz. Sanal ağları birbirine bağlama veya her ikisini aşağıdaki seçenekleri kullanabilirsiniz:
- **Eşliği:** farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure konumunda içinde bağlı kaynaklar sağlar. Sanal ağlar arasında gecikme süresi ve bant genişliği aynıdır kaynaklar aynı Vnet'e bağlıymış gibi. Eşleme hakkında daha fazla bilgi için okuma [sanal ağ eşlemesi](virtual-network-peering-overview.md) makalesi.
- **VNet-VNet bağlantısı:** farklı Azure sanal ağ içinde aynı veya farklı Azure konumu için bağlı kaynaklar sağlar. Bir Azure VPN ağ geçidi üzerinden trafik akışı gerekir çünkü eşliği farklı olarak, bant genişliği sanal ağlar arasında sınırlıdır. VNet-VNet bağlantısı ile sanal ağlara bağlanma hakkında daha fazla bilgi için okuma [VNet-VNet bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.

## <a name="connect-on-premises"></a>Bir şirket ağına bağlanma

Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir bileşimini kullanarak bir sanal ağa bağlanabilir:
- **Noktadan siteye sanal özel ağ (VPN):** , ağ ve sanal ağ için bağlı tek bir bilgisayar arasında kurulan. Varolan ağınız çok az kayıpla veya hiç değişiklik gerektirmediği Bu bağlantı türü, yalnızca Azure ile ya da geliştiricileri için başlıyorsanız mükemmeldir. Bağlantı, PC ve sanal ağ arasında Internet üzerinden şifreli iletişim sağlamak için SSTP protokolünü kullanır. Bir noktadan siteye VPN için gecikme süresini tahmin edilemez, olduğu Internet trafiği erişir.
- **Siteden siteye VPN:** VPN cihazınız arasındaki bir Azure VPN ağ geçidi kuruldu. Bu bağlantı türü bir VNet erişmek üzere yetkilendirmek herhangi bir şirket içi kaynak sağlar. Şirket içi Cihazınızı ve Azure VPN ağ geçidi arasında Internet üzerinden şifrelenmiş iletişimi sağlayan bir IPSec/IKE VPN bağlantısıdır. Siteden siteye bağlantı için gecikme süresini tahmin edilemez, olduğu Internet trafiği erişir.
- **Azure ExpressRoute:** ağınız ve Azure arasında bir expressroute bağlantı ortağı ile oluşturulmuş. Bu bağlantı özeldir. Trafik Internet'e erişmez. ExpressRoute bağlantısı için gecikme süresini tahmin edilebilir, olduğu trafik Internet'e çapraz geçiş değil.

Önceki tüm bağlantı seçenekleri hakkında daha fazla bilgi için okuma [bağlantı topoloji diyagramları](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) makalesi.

## <a name="filtering"></a>Ağ trafiği filtreleme
Veya aşağıdaki seçeneklerden birini ikisini birden kullanarak alt ağlar arasında ağ trafiğinin filtreleyebilirsiniz:
- **Ağ güvenlik grubu (NSG):** her NSG trafiğine kaynak ve hedef IP adresi, bağlantı noktası ve protokol göre filtre uygulamak için etkinleştirmeniz birden fazla gelen ve giden güvenlik kuralları içerebilir. Her NIC'nin bir VM için bir NSG uygulayabilirsiniz. Bir NSG'yi bir NIC alt ağına uygulayabilirsiniz veya diğer Azure kaynak bağlı. Nsg'ler hakkında daha fazla bilgi için okuma [ağ güvenlik grupları](virtual-networks-nsg.md) makalesi.
- **Ağ sanal Gereçleri (NVA):** bir NVA olan bir güvenlik duvarı gibi bir ağ işlevi gerçekleştiren yazılımı çalıştıran bir VM. İçinde kullanılabilir NVAs listesini görüntülemek [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs WAN iyileştirmesi ve diğer ağ trafiği işlevleri sağlayan de kullanılabilir durumdadır. NVAs, kullanıcı tanımlı ile genellikle kullanılır ya da BGP yolları. Bir NVA, sanal ağlar arasında trafiği filtrelemek için de kullanabilirsiniz.

## <a name="routing"></a>Ağ trafiği yönlendirme

Azure varsayılan olarak birbirleri ile iletişim kurmak için herhangi bir VNet içindeki herhangi bir alt ağa bağlı kaynaklar etkinleştirmek yönlendirme tabloları oluşturur. Ya da Azure oluşturduğu varsayılan yolların geçersiz kılmak için aşağıdaki seçenekleri uygulayabilirsiniz:
- **Kullanıcı tanımlı yollar:** özel yol tablolarını burada trafik yönlendirilir için her alt ağ için bu denetim yollar oluşturabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için, [Kullanıcı tanımlı yollar](virtual-networks-udr-overview.md) makalesini okuyun.
- **BGP yolları:** bir Azure VPN ağ geçidi veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza ağınızı bağlanırsanız, sanal ağlar için BGP yollarını yayabilir.

## <a name="pricing"></a>Fiyatlandırma

Güvenlik grupları sanal ağlar, alt ağlar, yol tablolarını veya ağ için ücret ödemeden yoktur. Giden Internet bant genişliği kullanımı, ortak IP adresleri, sanal ağ eşlemesi, VPN ağ geçitleri ve ExpressRoute her kendi fiyatlandırma yapılarına sahip. Görünüm [sanal ağ](https://azure.microsoft.com/pricing/details/virtual-network), [VPN ağ geçidi](https://azure.microsoft.com/pricing/details/vpn-gateway), ve [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) sayfalar daha fazla bilgi için fiyatlandırma.

## <a name="faq"></a>SSS

Sanal ağ ile ilgili sık sorulan soruları gözden geçirmek için bkz: [Virtual Network SSS](virtual-networks-faq.md) makalesi.


## <a name="next-steps"></a>Sonraki adımlar

- İlk sanal ağınızı oluşturun ve birkaç VM'ler, içindeki adımları tamamlayarak bağlanmak [ilk sanal ağınızı oluşturmak](virtual-network-get-started-vnet-subnet.md) makalesi.
- İçindeki adımları tamamlayarak bir sanal ağa noktadan siteye bağlantı oluşturmak [noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.
- Başka bir anahtar bazıları hakkında bilgi edinin [ağ yeteneklerini](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
