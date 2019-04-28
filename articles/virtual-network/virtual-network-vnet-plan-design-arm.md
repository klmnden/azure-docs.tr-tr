---
title: Azure sanal ağları planlama | Microsoft Docs
description: Sanal ağ yalıtımı, bağlantı ve konum gereksinimlerinize göre planlama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2018
ms.author: kumud
ms.openlocfilehash: acd7a88acb31b9d3bd3ba714387561e91b3524a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034748"
---
# <a name="plan-virtual-networks"></a>Sanal ağları planlama

Denemek için bir sanal ağ oluşturmak oldukça kolaydır, ancak büyük olasılıkla olan, kuruluşunuzun üretim gereksinimlerini desteklemek için zaman içinde birden çok sanal ağı dağıtır. Bazı planlama ile sanal ağlar'ı dağıtma ve daha etkili bir şekilde ihtiyacınız olan kaynakları bağlanabilir olacaktır. Bu makaledeki bilgiler, sanal ağlarla zaten biliyor ve bunlarla çalışma biraz deneyim sahibi en yararlıdır. Sanal ağlar ile ilgili bilgi sahibi değilseniz, okumanız önerilir [sanal ağa genel bakış](virtual-networks-overview.md).

## <a name="naming"></a>Adlandırma

Tüm Azure kaynaklarını bir ada sahip. Ad, her kaynak türü için değişebilen bir kapsam içinde benzersiz olmalıdır. Örneğin, bir sanal ağın adını içinde benzersiz olmalıdır bir [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), ancak içinde yinelenen bir [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) veya Azure [bölge](https://azure.microsoft.com/regions/#services). Kaynakları adlandırırken tutarlı bir şekilde kullanabileceğiniz bir adlandırma kuralı tanımlayan zaman içindeki çeşitli ağ kaynaklarını yönetirken yararlı olur. Öneriler için bkz [adlandırma kuralları](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#networking).

## <a name="regions"></a>Bölgeler

Tüm Azure kaynakları, bir Azure bölgesi ve abonelik oluşturulur. Bir kaynak yalnızca aynı bölge ve abonelik kaynağı olarak var olan bir sanal ağda oluşturulabilir. Bununla birlikte, farklı Aboneliklerde ve bölgelerde mevcut sanal ağları bağlayabilir. Daha fazla bilgi için [bağlantı](#connectivity). Kaynakları dağıtmak için hangi bölgelerin karar verirken, kaynakları tüketicilerinin fiziksel olarak bulunduğu yere göz önünde bulundurun:

- Kaynakları tüketicilerinin genellikle düşük ağ gecikme süresi kaynaklarına istersiniz. Belirtilen bir konuma Azure bölgesi arasındaki göreli gecikme belirlemek için bkz: [görüntülemek göreli gecikme](../network-watcher/view-relative-latencies.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Veri yerleşikliği, özerkliği, uyumluluk ve dayanıklılık gereksinimleri var mı? Bu durumda gereksinimlerine uygun bölgeyi seçmek önemlidir. Daha fazla bilgi için [Azure coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/).
- Dağıttığınız kaynakların aynı Azure bölgesindeki Azure kullanılabilirlik alanları genelinde esneklik gerekiyor mu? Sanal makineler (VM) gibi kaynakları, aynı sanal ağ içindeki farklı kullanılabilirlik bölgelerine dağıtabilirsiniz. Tüm Azure bölgeleri, ancak kullanılabilirlik alanlarını destekler. Kullanılabilirlik alanları ve bunları destekleyen bölgeleri hakkında daha fazla bilgi için bkz: [kullanılabilirlik](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="subscriptions"></a>Subscriptions

En fazla sayıda sanal ağlar her Abonelikteki gerektiği gibi dağıtabilirsiniz [sınırı](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Bazı kuruluşlar, örneğin farklı Departmanlar için farklı Aboneliklerde olabilir. Daha fazla bilgi ve dikkat edilecek noktalar geçici abonelikleri için bkz. [abonelik İdaresi](/azure/architecture/cloud-adoption-guide/subscription-governance#define-your-hierarchy).

## <a name="segmentation"></a>Segmentasyon

Bölge ve abonelik başına birden çok sanal ağlar oluşturabilir. Her sanal ağ içindeki birden çok alt ağ oluşturabilirsiniz. Aşağıdaki konular kaç sanal ağlar ve alt ağlar gerektirir belirlemenize yardımcı olmak:

### <a name="virtual-networks"></a>Sanal ağlar

Azure genel ağında sanal, yalıtılmış bir kısmı bir sanal ağdır. Her bir sanal ağ, aboneliğe ayrılmıştır. Bir Abonelikteki bir sanal ağ ya da birden fazla sanal ağ oluşturmak karar dikkate almanız gerekenler:

- Ayrı sanal ağlara trafiği yalıtma için herhangi bir kuruluşun güvenlik gereksinimleri var mı? Veya sanal ağları bağlamak seçebilirsiniz. Sanal ağları birbirine bağlama, sanal ağlar arasındaki trafik akışını denetlemek için bir güvenlik duvarı gibi bir ağ sanal Gereci uygulayabilirsiniz. Daha fazla bilgi için [güvenlik](#security) ve [bağlantı](#connectivity).
- Ayrı sanal ağları yalıtmak için Kurumsal gereksinimlere mevcut [abonelikleri](#subscriptions) veya [bölgeleri](#regions)?
- A [ağ arabirimi](virtual-network-network-interface.md) diğer kaynaklarla iletişim kurmak bir VM sağlar. Her bir ağ arabirimine atanmış bir veya daha fazla özel IP adresi vardır. Kaç ağ arabirimleri ve [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) sanal ağ içinde ihtiyaç duyuyorsunuz? Vardır [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) ağ arabirimleri ve sanal ağ içinde sahip olduğunuz özel IP adresleri sayısı.
- Şirket içi ağ ve sanal ağ başka bir sanal ağa bağlanmak istiyor musunuz? Bazı sanal ağları birbirine veya şirket içi ağlarda, ancak diğerlerini bağlamak tercih edebilirsiniz. Daha fazla bilgi için [bağlantı](#connectivity). Her başka bir sanal ağa bağlanan sanal ağ ya da şirket içi ağı, benzersiz adres alanı olmalıdır. Bir sanal ağın kendi adres alanına atanan bir veya daha fazla genel veya özel adres aralıklarını sahiptir. Bir adres aralığı, Sınıfsız internet etki alanına yönlendirme (CIDR) biçiminde 10.0.0.0/16 gibi belirtilir. Daha fazla bilgi edinin [adres aralıkları](manage-virtual-network.md#add-or-remove-an-address-range) sanal ağlar için.
- Farklı sanal ağlarda bulunan kaynaklar için herhangi bir kuruluş yönetim gereksinimleri var mı? Bu nedenle, kaynakları basitleştirmek için ayrı sanal ağa ayırabilir, [izin atama](#permissions) kişilerin kuruluşunuzdaki veya farklı ağlarda farklı ilkeleri atamak için.
- Bazı Azure hizmet kaynaklarını bir sanal ağa dağıttığınızda, bunlar kendi sanal ağ oluşturur. Bir Azure hizmeti, kendi sanal ağ oluşturur olup olmadığını belirlemek için her bilgi [bir sanal ağa dağıtılan Azure hizmeti](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network).

### <a name="subnets"></a>Alt ağlar

Bir sanal ağ alt ağına bir veya daha fazla kadar bölümlenebilecek [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Bir abonelikte bir alt ağ ya da birden fazla sanal ağ oluşturmak karar dikkate almanız gerekenler:

- Her alt ağ CIDR biçiminde adres alanı sanal ağ içinde belirtilen bir benzersiz adresi aralığı olmalıdır. Adres aralığı, diğer alt ağlara sanal ağ ile örtüşemez.
- Bazı Azure hizmet kaynaklarını bir sanal ağa dağıtmayı planlıyorsanız, bunlar gerektiren ya da kendi alt ağına oluşturun, burada yeterli ayrılmamış olmalıdır Bunu yapmak için boşluk. Bir Azure hizmeti, kendi alt ağı oluşturur olup olmadığını belirlemek için her bilgi [bir sanal ağa dağıtılan Azure hizmeti](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Örneğin, bir Azure VPN ağ geçidi kullanarak şirket içi ağına bir sanal ağ birbirine bağlarsanız, sanal ağ ağ geçidi için ayrılmış alt ağında olmalıdır. Daha fazla bilgi edinin [ağ geçidi alt ağları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub).
- Azure yollarını varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasındaki trafiği ağ. Azure'nın varsayılan Azure alt ağlar arasında yönlendirme önlemek veya örneğin bir ağ sanal Gereci üzerinden alt ağlar arasında trafiği yönlendirmek için yönlendirme ayarlarını geçersiz kılabilir. Bu trafiği ağ sanal Gereci (NVA) aracılığıyla aynı sanal ağ akışı içindeki kaynaklar arasında gerektiriyorsa, kaynakları farklı alt ağlara dağıtma. Daha fazla bilgi [güvenlik](#security).
- Bir Azure depolama hesabı veya belirli alt ağlara sanal ağ hizmet uç noktası ile Azure SQL veritabanı gibi Azure kaynaklarına erişimi sınırlayabilirsiniz. Ayrıca, kaynaklara internet'ten erişimi reddedebilirsiniz. Birden çok alt ağı oluşturmak ve bazı alt ağlar, ancak bazıları için hizmet uç noktasını girin. Daha fazla bilgi edinin [hizmet uç noktalarını](virtual-network-service-endpoints-overview.md), ve Azure kaynakları için bunları etkinleştirebilirsiniz.
- Bir sanal ağdaki her alt ağ sıfır veya bir ağ güvenlik grubunu ilişkilendirebilirsiniz. Aynı veya farklı bir ilişkilendirme, güvenlik grubunun her alt ağ için ağ. Her ağ güvenlik grubu izin ver veya Reddet kaynaklarına ve hedeflere gelen ve giden trafik kurallarını içerir. Daha fazla bilgi edinin [ağ güvenlik grupları](#traffic-filtering).

## <a name="security"></a>Güvenlik

Ağ güvenlik grupları ve ağ sanal Gereçleri kullanarak bir sanal ağ içindeki kaynaklarla gelen ve giden ağ trafiğini filtreleyebilirsiniz. Azure tarafından nasıl yönlendirdiği denetleyebilirsiniz trafiği alt ağlar. Kuruluşunuzda kimlerin sanal ağlardaki kaynaklarla çalışabilir sınırlayabilirsiniz.

### <a name="traffic-filtering"></a>Trafik filtreleme

- Bir ağ güvenlik grubu, ağ trafiği veya her ikisini de filtreleyen bir NVA kullanarak bir sanal ağdaki kaynaklar arasındaki ağ trafiğini filtreleyebilirsiniz. Bir güvenlik duvarı gibi bir NVA filtresi ağ trafiğini dağıtmak için bkz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?subcategories=appliances&page=1). Bir NVA kullanırken, NVA alt ağlara giden trafiği yönlendirmek için özel yollar oluşturabilir. Daha fazla bilgi edinin [trafik yönlendirme](#traffic-routing).
- Bir ağ güvenlik grubu izin veren veya reddeden veya kaynaklardan gelen trafiği birkaç varsayılan güvenlik kuralları içerir. Bir ağ güvenlik grubu, bir ağ arabirimi, ağ arabiriminin içinde bulunduğu alt ağa veya her ikisi de ilişkilendirilebilir. Güvenlik kuralları yönetimini basitleştirmek için belirli alt ağlar için ağ güvenlik grubu ilişkilendirmek yerine alt ağ içinde mümkün olduğunda ayrı ayrı ağ arabirimleri önerilir.
- Uygulanmış olan farklı güvenlik kuralları bir alt ağ içinde farklı Vm'lere ihtiyacınız varsa, bir veya daha fazla uygulama güvenlik grupları VM ağ arabiriminin ilişkilendirebilirsiniz. Bir güvenlik kuralı, bir uygulama güvenlik grubu, kaynak, hedef veya her ikisi de belirtebilirsiniz. Bu kural yalnızca sonra uygulama güvenlik grubuna üye olan ağ arabirimlerini uygular. Daha fazla bilgi edinin [ağ güvenlik grupları](security-overview.md) ve [uygulama güvenlik grupları](security-overview.md#application-security-groups).
- Azure, birkaç varsayılan güvenlik kuralları her ağ güvenlik grubu içinde oluşturur. Bir varsayılan kural tüm trafiği sanal ağ içindeki tüm kaynaklar arasında akmasına izin verir. Bu davranışı geçersiz kılmak için ağ güvenliği kullanın, trafiği yönlendirmek için bir NVA veya her ikisini de Yönlendirme özel gruplar. Tüm Azure'nın planladığınızdan önerilir [varsayılan güvenlik kuralları](security-overview.md#default-security-rules) ve ağ güvenlik grubu kuralları bir kaynağa uygulanma anlayın.

Azure ile kullanarak internet arasında DMZ uygulama için örnek tasarımları görüntüleyebileceğiniz bir [NVA](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) veya [ağ güvenlik grupları](virtual-networks-dmz-nsg.md).

### <a name="traffic-routing"></a>Trafik yönlendirme

Azure, bir alt ağdan giden trafik için birkaç varsayılan yollar oluşturur. Azure'nın varsayılan bir yol tablosu oluşturup bir alt ağ ile ilişkilendirme yönlendirme ayarlarını geçersiz kılabilir. Yaygın nedenler Azure'nın varsayılan geçersiz kılma için yönlendirme şunlardır:
- Bir NVA üzerinden akmasını alt ağlar arasındaki trafiği istediğinden. Kullanma hakkında daha fazla bilgi için [NVA aracılığıyla trafiği zorlamak için rota tablolarını yapılandırmak](tutorial-create-route-table-portal.md).
- Tüm İnternet'e bağlı trafiği NVA ya da şirket içinde bir Azure VPN ağ geçidi üzerinden zorla olmak istememiz. İnternet trafiğini şirket içi İnceleme için zorlama ve günlüğe kaydetme genellikle için zorlamalı tünel denir. Nasıl yapılandırılacağı hakkında daha fazla bilgi [zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).

Özel yönlendirme uygulamanız gerekiyorsa, ile kendinizi alıştırın önerilir [Azure'da yollar](virtual-networks-udr-overview.md).

## <a name="connectivity"></a>Bağlantı

Bir Azure VPN ağ geçidi kullanarak sanal ağ eşlemesini kullanmanın diğer sanal ağlara ya da şirket içi ağınıza bir sanal ağa bağlanabilir.

### <a name="peering"></a>Eşleme

Kullanırken [sanal ağ eşlemesi](virtual-network-peering-overview.md), sanal ağların aynı veya farklı, Azure bölgeleri desteklenir. Sanal ağlar aynı veya farklı Azure abonelikleri (hatta abonelikleri farklı Azure Active Directory kiracılara ait) olabilir. Bir eşleme oluşturmadan önce tüm eşlemesi planladığınızdan önerilir [gereksinimler ve kısıtlamalar](virtual-network-manage-peering.md#requirements-and-constraints). Sanal ağlardaki kaynaklar arasında bant genişliği aynı sanal ağdaki kaynaklar gibi aynı olduğundan aynı bölgedeki eşlenmiş.

### <a name="vpn-gateway"></a>VPN ağ geçidi

Bir Azure kullanabileceğiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanarak, şirket içi ağ bir sanal ağa bağlanmak için bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json), veya Azure ile ayrılmış bir bağlantı kullanarak [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Birleştirebilirsiniz eşleme ve oluşturmak için bir VPN ağ geçidi [merkez ve uç ağ](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)burada ağlı sanal ağlar bir merkez sanal ağa bağlayın ve hub gibi bir şirket içi ağa bağlanır.

### <a name="name-resolution"></a>Ad çözümlemesi

Bir sanal ağ içindeki kaynaklarla Azure'nın eşlenmiş sanal ağdaki kaynakların adlarını çözümleyemiyor [yerleşik DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md). Eşlenen sanal ağ, adları çözümlemek için [kendi DNS sunucunuzu dağıtmak](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server), ya da Azure DNS kullanacak [özel etki alanları](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bir sanal ağdaki kaynaklar ve şirket içi ağlar arasında ad çözümleme Ayrıca kendi DNS sunucunuzu dağıtmak gerektirir.

## <a name="permissions"></a>İzinler

Azure kullanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (RBAC) kaynaklara. İzinleri atanmış bir [kapsam](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#scope) aşağıdaki hiyerarşi içinde: Abonelik, yönetim grubu, kaynak grubu ve tek başına bir kaynak. Hiyerarşi hakkında daha fazla bilgi için bkz: [kaynaklarınızı düzenleme](../azure-resource-manager/management-groups-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure sanal ağları ve tüm ilgili yeteneklerini eşdüzey hizmet sağlama, ağ güvenlik grupları, hizmet uç noktaları ve rota tabloları gibi çalışmak için kuruluş üyelerinin yerleşik olarak atayabilirsiniz [sahibi](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#owner), [Katkıda bulunan](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#contributor), veya [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve ardından Ata rol için uygun kapsam. Sanal ağ özelliklerinin bir alt kümesi için belirli izinleri atamak istiyorsanız, oluşturun bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve için gereken belirli izinleri atama [sanal ağlar](manage-virtual-network.md#permissions), [ alt ağları ve hizmet uç noktaları](virtual-network-manage-subnet.md#permissions), [ağ arabirimleri](virtual-network-network-interface.md#permissions), [eşlemesi](virtual-network-manage-peering.md#permissions), [ağ ve uygulama güvenlik grupları](manage-network-security-group.md#permissions), veya [rota tabloları](manage-route-table.md#permissions) rolü.

## <a name="policy"></a>İlke

Azure ilkesi oluşturmak, atamak ve ilke tanımları yönetmenize olanak sağlar. Kaynakları kuruluş standartlarınız ve hizmet düzeyi sözleşmeleri ile uyumlu kalmasını sağlar ilke tanımları, kaynaklarınız üzerinden farklı kuralları uygular. Azure İlkesi, sahip olduğunuz ilke tanımlarıyla uyumlu olmayan kaynaklar için tarama kaynaklarınızın bir değerlendirmesini çalışır. Örneğin, tanımlamak ve yalnızca belirli bir kaynak grubu veya bölgedeki sanal ağları oluşturulmasına izin veren bir ilke uygulayın. Başka bir ilke, her alt ağ ile ilişkili ağ güvenlik grubu olduğunu gerektirebilir. İlkeler, ardından kaynakların oluşturulup güncelleştirildiği sırada değerlendirilir.

İlkeler aşağıdaki hiyerarşiye uygulanır: Abonelik, yönetim grubu ve kaynak grubu. Daha fazla bilgi edinin [Azure İlkesi](../governance/policy/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya bazı sanal ağı dağıtmak [ilke şablonu](policy-samples.md) örnekleri.

## <a name="next-steps"></a>Sonraki adımlar

Tüm görevler, ayarları ve seçenekleri hakkında bir [sanal ağ](manage-virtual-network.md), [alt ağı ve hizmet uç noktası](virtual-network-manage-subnet.md), [ağ arabirimi](virtual-network-network-interface.md), [eşleme](virtual-network-manage-peering.md), [ağ ve uygulama güvenlik grubu](manage-network-security-group.md), veya [yol tablosu](manage-route-table.md).
