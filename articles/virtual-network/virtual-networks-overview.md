---
title: "Azure sanal ağı | Microsoft Docs"
description: "Azure Virtual Network kavramları ve özellikler hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: jdial
ms.openlocfilehash: dc6916bd25c5a020fdcef0707fe28a1e34fb0f88
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="azure-virtual-network"></a>Azure Sanal Ağ

Microsoft Azure sanal ağ hizmeti, diğer sanal bir ağa güvenli iletişim Azure kaynaklarını sağlar. Bir sanal ağ kendi ağ bulutta gösterimidir. Azure bulutunun aboneliğinize adanmış mantıksal ayırma bir sanal ağdır. Sanal ağlar, diğer sanal ağlara ya da şirket içi ağınıza bağlanabilir. Aşağıdaki resimde bazı Azure Virtual Network service özelliklerini gösterir:

![Ağ Diyagramı](./media/virtual-networks-overview/virtual-network-overview.png)

Aşağıdaki Azure sanal ağ özellikleri hakkında daha fazla bilgi edinmek için özellik tıklatın:
- **[Yalıtım:](#isolation)**  sanal ağlar birbirinden yalıtılmış. Ayrı oluşturabilirsiniz, aynı CIDR (örneğin, 10.0.0.0/0) kullanan sanal ağlar geliştirme, test ve üretim için adres blokları. Buna karşılık, ağları birbirine bağlamak ve farklı CIDR adres bloklarını kullanan birden çok sanal ağlar oluşturabilir. Bir sanal ağ birden çok alt ağa bölebilirsiniz. Azure sanal makineler ve sanal ağ içinde dağıtılmıştır Azure Cloud Services rol örnekleri için dahili ad çözümlemesi sağlar. İsteğe bağlı olarak bir sanal ağ Azure dahili ad çözümlemesi kullanmak yerine kendi DNS sunucularını kullanmak için yapılandırabilirsiniz.
- **[Internet iletişimini:](#internet)**  bir sanal ağdaki tüm Azure sanal makineler ve bulut Hizmetleri rol örnekleri varsayılan olarak Internet erişimi vardır. Belirli kaynaklara gelen erişim gerektiğinde de etkinleştirebilirsiniz.
- **[Azure kaynak iletişim:](#within-vnet)**  bulut Hizmetleri ve sanal makineler gibi Azure kaynaklarını aynı sanal ağ içinde dağıtılabilir. Farklı alt ağlarda olsalar bile, kaynakları birbirleri ile özel IP adresleri kullanarak iletişim kurabilir. Azure, yapılandırmak ve yollar yönetmek zorunda kalmamak için varsayılan alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar. Azure'nın yine de Yönlendirme isterseniz özelleştirebilirsiniz.
- **[Sanal ağ bağlantısı:](#connect-vnets)**  sanal ağlar bağlanması birbirine herhangi bir sanal ağ kaynaklarında ile iletişim kurmak için herhangi bir sanal ağ kaynaklarında etkinleştirme.
- **[Şirket içi bağlantı:](#connect-on-premises)**  bir sanal ağ özel olarak bir şirket içi ağ veya Internet üzerinden bir siteden siteye VPN bağlantısı kullanarak bağlanabilir.
- **[Trafik filtreleme:](#filtering)**  rol örneği ağ trafiğini sanal makineler ve bulut Hizmetleri filtre gelen ve giden kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokolü tarafından.
- **[Yönlendirme:](#routing)**  isteğe bağlı olarak, kendi yollarını yapılandırarak yönlendirme Azure'un varsayılan ayarlarını geçersiz kılabilir veya ağ geçidinden BGP yayarak yönlendirir.

## <a name = "isolation"></a>Ağ yalıtımı ve kesimleme

Her Azure birden çok sanal ağ uygulayabilirsiniz [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ve Azure [bölge](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Her sanal ağ, diğer sanal ağlardan ayrılır. Her sanal ağ için şunları yapabilirsiniz:
- Ortak ve özel (RFC 1918) adreslerini kullanarak özel bir özel IP adres alanını belirtin. Azure sanal ağ özel bir IP adresi kaynaklarında atadığınız adres alanından atar.
- Sanal ağ bir veya daha fazla alt ağlara ayırabilir ve bir kısmı her alt ağ için sanal ağın adres alanı ayırın.
- Azure tarafından sağlanan ad çözümlemesi kullanın veya sanal ağ kaynaklarında tarafından kullanmak için kendi DNS sunucusu belirtin. Sanal ağlar ad çözümleme hakkında daha fazla bilgi için bkz: [VM'ler ve bulut Hizmetleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md) makalesi.

## <a name = "internet"></a>Internet iletişimi
Bir sanal ağdaki tüm kaynakları giden internet, varsayılan olarak iletişim kurabilir. Özel IP adresi kaynağının çevrilmiş kaynak ağ adresine (SNAT) Azure altyapısı tarafından seçilen genel bir IP adresi var. Giden Internet bağlantısı hakkında daha fazla bilgi için okuma [azure'da giden bağlantılar anlama](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) makalesi. Giden Internet bağlantısı engellemek için özel yollar veya trafik filtreleme uygulayabilirsiniz.

Azure kaynaklarına Internet'ten gelen bağlantı kurmak veya SNAT olmadan Internet'e giden iletişim kurmak için bir kaynak genel bir IP adresi atanması gerekir. Genel IP adresleri hakkında daha fazla bilgi için okuma [ortak IP adresleri](virtual-network-public-ip-address.md) makalesi.

## <a name="within-vnet"></a>Azure kaynakları arasındaki iletişimin güvenliğini sağlama

Bir sanal ağ içinde sanal makineler dağıtabilirsiniz. Sanal makineler, bir ağ arabirimi üzerinden sanal bir ağdaki diğer kaynaklarla iletişim kurar. Ağ arabirimleri hakkında daha fazla bilgi için bkz: [ağ arabirimleri](virtual-network-network-interface.md).

Sanal bir ağa, Azure sanal makineler, Azure bulut Hizmetleri, Azure App Service ortamları ve Azure sanal makine ölçek kümeleri gibi diğer çeşitli Azure kaynaklarını da dağıtabilirsiniz. Bir sanal ağa dağıtabileceğiniz Azure kaynaklarını tam bir listesi için bkz: [Azure Hizmetleri için sanal ağ hizmeti tümleştirme](virtual-network-for-azure-services.md).

Bazı kaynaklar sanal bir ağ dağıtılamaz, ancak yalnızca bir sanal ağ içinde kaynaklardan iletişimini kısıtla olanak sağlar. Kaynaklara erişimi kısıtlama hakkında daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları](virtual-network-service-endpoints-overview.md). 

## <a name="connect-vnets"></a>Sanal ağlara bağlanabilir

Sanal ağlar birbirlerine birbirleri ile sanal ağ eşlemesi kullanarak iletişim kurmak için her iki sanal ağ kaynaklarında etkinleştirme bağlayabilirsiniz. Bant genişliği ve farklı sanal ağlarda bulunan kaynaklar arasındaki iletişim gecikmesi aynıdır kaynaklar aynı sanal ağda değilmiş gibi. Eşleme hakkında daha fazla bilgi için okuma [sanal ağ eşlemesi](virtual-network-peering-overview.md) makalesi.

## <a name="connect-on-premises"></a>Bir şirket ağına bağlanma

Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir bileşimini kullanarak bir sanal ağa bağlanabilir:
- **Noktadan siteye sanal özel ağ (VPN):** ağınızdaki bir sanal ağ ile tek bir bilgisayar arasında kurulan. Bir sanal ağla bağlantı kurmak istediği her bilgisayar bağımsız olarak kendi bağlantılarını yapılandırmanız gerekir. Varolan ağınız çok az kayıpla veya hiç değişiklik gerektirmediği Bu bağlantı türü, yalnızca Azure ile ya da geliştiricileri için başlıyorsanız mükemmeldir. Bağlantı, bilgisayar ve bir sanal ağ arasında Internet üzerinden şifreli iletişim sağlamak için SSTP protokolünü kullanır. Bir noktadan siteye VPN için gecikme süresini tahmin edilemez, olduğu Internet trafiği erişir.
- **Siteden siteye VPN:** VPN cihazınız arasındaki bir sanal ağda dağıtılmış bir Azure VPN ağ geçidi kuruldu. Bu bağlantı türü, bir sanal ağa erişmek üzere yetkilendirmek herhangi bir şirket içi kaynak sağlar. Şirket içi Cihazınızı ve Azure VPN ağ geçidi arasında Internet üzerinden şifrelenmiş iletişimi sağlayan bir IPSec/IKE VPN bağlantısıdır. Siteden siteye bağlantı için gecikme süresini tahmin edilemez, olduğu Internet trafiği erişir.
- **Azure ExpressRoute:** ağınız ve Azure arasında bir expressroute bağlantı ortağı ile oluşturulmuş. Bu bağlantı özeldir. Trafik Internet'e erişmez. ExpressRoute bağlantısı için gecikme süresini tahmin edilebilir, olduğu trafik Internet'e çapraz geçiş değil.

Önceki tüm bağlantı seçenekleri hakkında daha fazla bilgi için okuma [bağlantı topoloji diyagramları](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) makalesi.

## <a name="filtering"></a>Ağ trafiği filtreleme
Veya aşağıdaki seçeneklerden birini ikisini birden kullanarak alt ağlar arasında ağ trafiğinin filtreleyebilirsiniz:
- **Ağ güvenlik grupları:** trafiğine kaynak ve hedef IP adresi, bağlantı noktası ve protokol göre filtre uygulamak için etkinleştirmeniz birden fazla gelen ve giden güvenlik kuralları bir ağ güvenlik grubu içerebilir. Her bir sanal makine ağ arabirimi için ağ güvenlik grubu uygulayabilirsiniz. Bir ağ arabirimi alt ağına ağ güvenlik grubu da uygulayabilir veya diğer Azure kaynak. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](security-overview.md#network-security-groups).
- **Ağ sanal Gereçleri:** bir güvenlik duvarı gibi bir ağ işlevi gerçekleştiren yazılımı çalıştıran bir sanal makine ağ sanal gereç değil. Kullanılabilir ağ sanal Gereçleri içinde bir listesini görüntülemek [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Ağ sanal Gereçleri olan de kullanılabilir, WAN iyileştirmesi ve diğer ağ trafiği işlevleri sağlar. Ağ sanal Gereçleri, kullanıcı tanımlı ile genellikle kullanılır ya da BGP yolları. Bir ağ sanal gereç, sanal ağlar arasında trafiği filtrelemek için de kullanabilirsiniz.

## <a name="routing"></a>Ağ trafiği yönlendirme

Azure varsayılan olarak birbirleri ile iletişim kurmak için herhangi bir sanal ağ içinde herhangi bir alt ağa bağlı kaynaklar etkinleştirmek yönlendirme tabloları oluşturur. Ya da Azure oluşturduğu varsayılan yolların geçersiz kılmak için aşağıdaki seçenekleri uygulayabilirsiniz:
- **Kullanıcı tanımlı yollar:** özel yol tablolarını burada trafik yönlendirilir için her alt ağ için bu denetim yollar oluşturabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined).
- **BGP yolları:** sanal ağınıza bir Azure VPN ağ geçidi veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza bağlanıyorsanız, BGP yollarını sanal ağlarınıza yayabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

Güvenlik grupları sanal ağlar, alt ağlar, yol tablolarını veya ağ için ücret ödemeden yoktur. Giden Internet bant genişliği kullanımı, ortak IP adresleri, sanal ağ eşlemesi, VPN ağ geçitleri ve ExpressRoute her kendi fiyatlandırma yapılarına sahip. Görünüm [sanal ağ](https://azure.microsoft.com/pricing/details/virtual-network), [VPN ağ geçidi](https://azure.microsoft.com/pricing/details/vpn-gateway), ve [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) sayfalar daha fazla bilgi için fiyatlandırma.

## <a name="faq"></a>SSS

Azure sanal ağ ile ilgili sık sorulan soruları gözden geçirmek için bkz: [Virtual network SSS](virtual-networks-faq.md) makalesi.


## <a name="next-steps"></a>Sonraki adımlar

- İlk sanal ağınızı oluşturun ve birkaç sanal makineler, içindeki adımları tamamlayarak dağıtılacağını [ilk sanal ağınızı oluşturmak](virtual-network-get-started-vnet-subnet.md).
- İçindeki adımları tamamlayarak sanal ağa noktadan siteye bağlantı oluşturmak [noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Başka bir anahtar bazıları hakkında bilgi edinin [ağ yeteneklerini](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
