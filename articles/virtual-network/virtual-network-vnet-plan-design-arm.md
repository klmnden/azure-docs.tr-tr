---
title: Azure sanal ağları planlama | Microsoft Docs
description: Yalıtımı, bağlantı ve konum gereksinimleri göre sanal ağlar için planlama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2018
ms.author: jdial
ms.openlocfilehash: 83558b9d8d47ac5e6bd15dd54db38125376d11bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="plan-virtual-networks"></a>Sanal ağlar planlama

Denemeniz için sanal ağ oluşturma yeterince kolay, ancak büyük olasılıkla, birden çok sanal ağ, kuruluşunuzun üretim gereksinimlerini desteklemek için zaman içinde dağıtır. Bazı planlama ile sanal ağlar dağıtmak ve daha etkili bir şekilde ihtiyacınız olan kaynakları bağlamak mümkün olacaktır. Bu makaledeki bilgileri, zaten sanal ağlarla tanıdık ve bunlarla çalışmak biraz deneyim sahibi en yararlı olur. Sanal ağlarla bilmiyorsanız okumanız önerilir [sanal ağa genel bakış](virtual-networks-overview.md).

## <a name="naming"></a>Adlandırma

Tüm Azure kaynaklarına aynı ada sahip. Adı, her kaynak türü için değişebilir bir kapsam içinde benzersiz olmalıdır. Örneğin, bir sanal ağ adı içinde benzersiz olmalıdır bir [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), ancak içinde yinelenen bir [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) veya Azure [bölge](https://azure.microsoft.com/regions/#services). Kaynakları adlandırırken sürekli olarak kullanabileceğiniz bir adlandırma kuralı tanımlayan çeşitli ağ kaynaklarına zamanla yönetirken yararlıdır. Öneriler için bkz [adlandırma kuralları](/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Bölgeler

Tüm Azure kaynakları bir Azure bölgesi ve abonelik oluşturulur. Bir kaynak yalnızca aynı bölge ve abonelik kaynak olarak mevcut bir sanal ağ oluşturulabilir. Ancak, farklı Aboneliklerde ve bölgeler mevcut sanal ağlara bağlanabilir. Daha fazla bilgi için bkz: [bağlantı](#connectivity). Kaynakları dağıtmak için hangi bilgiler karar verirken, kaynakların tüketicileriyle fiziksel olarak nerede göz önünde bulundurun:

- Kaynakların tüketicileriyle genellikle en düşük ağ gecikmesini kaynaklarına istiyor. Belirtilen bir konuma ve Azure bölgeleri arasında göreceli gecikmeleri belirlemek için bkz: [görüntülemek göreceli gecikmeleri](../network-watcher/view-relative-latencies.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Veri residency Egemenlik, uyumluluk veya dayanıklılık gereksinimleri var mı? Öyleyse gereksinimlerine hizalar bölge seçme kritik önem taşır. Daha fazla bilgi için bkz: [Azure coğrafyalara](https://azure.microsoft.com/global-infrastructure/geographies/).
- Dağıttığınız kaynaklar aynı Azure bölgesinde içinde Azure kullanılabilirlik dilimlerinde dayanıklılık gerektiriyor mu? Sanal makineler (VM) gibi kaynaklar aynı sanal ağ içindeki farklı kullanılabilirlik bölgeleri dağıtabilirsiniz. Tüm Azure bölgeleri kullanılabilirlik bölgeleri ancak destekler. Kullanılabilirlik bölgeleri ve bunları destekleyen bölgeler hakkında daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="subscriptions"></a>Abonelikler

En fazla sayıda sanal ağlar her abonelik içindeki gerektiği gibi dağıtabilirsiniz [sınırı](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Bazı kuruluşlar, örneğin farklı Departmanlar için farklı Aboneliklerde sahiptir. Daha fazla bilgi ve abonelikleri geçici bir çözüm değerlendirmeleri için bkz: [abonelik idare](../azure-resource-manager/resource-manager-subscription-governance.md?toc=%2fazure%2fvirtual-network%2ftoc.json#define-your-hierarchy).

## <a name="segmentation"></a>Segmentasyon

Bölge ve abonelik başına birden çok sanal ağlar oluşturabilir. Her sanal ağ içindeki birden çok alt ağlar oluşturabilirsiniz. İzleyin dikkat edilecek noktalar, kaç tane sanal ağlar ve alt ağları ihtiyaç duyduğunuz belirlemenize yardımcı:

### <a name="virtual-networks"></a>Sanal ağlar

Bir sanal ağ, Azure ortak ağ, sanal, yalıtılmış bir bölümüdür. Her sanal ağ aboneliğinize ayrılır. Ne zaman göz önünde bulundurmanız gerekenler bir abonelikte bir sanal ağ veya birden çok sanal ağlar oluşturmak karar:

- Ayrı sanal ağa trafiği yalıtmak için tüm kurumsal güvenlik gereksinimleri var mı? Sanal ağlar veya bağlanmayı seçebilirsiniz. Sanal ağlar bağlanıyorsanız, sanal ağlar arasındaki trafik akışını denetlemek için bir güvenlik duvarı gibi bir ağ sanal gereç uygulayabilirsiniz. Daha fazla bilgi için bkz: [güvenlik](#security) ve [bağlantı](#connectivity).
- Sanal ağlar ayrı yalıtmak için Kurumsal gereksinimlere mevcut [abonelikleri](#subscriptions) veya [bölgeleri](#regions)?
- A [ağ arabirimi](virtual-network-network-interface.md) diğer kaynakları ile iletişim için bir VM sağlar. Her bir ağ arabirimine atanmış bir veya daha fazla özel IP adresleri vardır. Kaç tane ağ arabirimleri ve [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) sanal bir ağa gerektiriyor mu? Vardır [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) ağ arabirimleri ve sanal ağ içindeki olabilir özel IP adresleri sayısı.
- Başka bir sanal ağ için sanal ağ veya şirket içi ağ bağlanmak istiyor? Bazı sanal ağları birbirine veya şirket içi ağlarda, ancak diğer için bağlanmak tercih edebilirsiniz. Daha fazla bilgi için bkz: [bağlantı](#connectivity). Her başka bir sanal ağa bağlanan sanal ağ veya şirket içi ağ benzersiz adres alanı içermesi gerekir. Her sanal ağ adres alanı için atanan bir veya daha fazla ortak veya özel adres aralıkları vardır. Bir adres aralığı, Sınıfsız Internet etki alanı yönlendirme (CIDR) biçiminde 10.0.0.0/16 gibi belirtilir. Daha fazla bilgi edinmek [adres aralığına](manage-virtual-network.md#add-or-remove-an-address-range) sanal ağlar için.
- Farklı sanal ağlarda kaynakları için tüm kuruluş yönetim gereksinimleri var mı? Bu nedenle, basitleştirmek için ayrı sanal ağa kaynakları ayırabilir, [iznini atama](#permissions) kişiler, kuruluşunuzda ya da farklı atamak için [ilkeleri](#policies) farklı sanal Ağ.
- Bir sanal ağ içinde bazı Azure hizmeti kaynaklar dağıttığınızda, bunlar kendi sanal ağ oluşturur. Bir Azure hizmeti, kendi sanal ağ oluşturur olup olmadığını belirlemek için her bilgi [bir sanal ağ içinde dağıtılabilir Azure hizmeti](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network).

### <a name="subnets"></a>Alt ağlar

Bir sanal ağ bir veya daha fazla alt ağ kadar kesimlere ayrılmıştır [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Ne zaman göz önünde bulundurmanız gerekenler bir alt ağ veya birden çok sanal ağlar bir abonelikte oluşturmaya karar:

- Her alt ağ CIDR biçiminde adres alanı sanal ağ içinde belirtilen bir benzersiz adres aralığı olması gerekir. Adres aralığı, diğer alt ağlara sanal ağ ile örtüşemez.
- Bazı Azure hizmeti kaynaklar sanal bir ağa dağıtmayı planlıyorsanız, bunlar gerektiren veya oluşturma, kendi alt ağ, yani vardır yeterli ayrılmamış olmalıdır Bunu yapmak için boşluk. Bir Azure hizmeti, kendi alt ağı oluşturur olup olmadığını belirlemek için her bilgi [bir sanal ağ içinde dağıtılabilir Azure hizmeti](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Örneğin, bir Azure VPN ağ geçidi kullanarak şirket içi ağı için bir sanal ağ bağlanıyorsanız, sanal ağın ağ geçidi için ayrılmış bir alt ağ olması gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub).
- Azure yollar varsayılan olarak bir sanal ağdaki tüm alt ağlar arasında trafiği ağ. Azure alt ağlar arasında yönlendirme engellemek veya örneğin bir ağ sanal gereç aracılığıyla alt ağlar arasında trafiği yönlendirmek için yönlendirme Azure'un varsayılan ayarlarını geçersiz kılabilir. Bu trafiği ağ sanal gereç (NVA) üzerinden aynı sanal ağ akışı kaynaklar arasında gerektiriyorsa, kaynakları farklı alt ağlara dağıtın. Daha fazla bilgi edinin [güvenlik](#security).
- Bir Azure depolama hesabı veya belirli alt ağlara sanal ağ hizmet uç noktası ile Azure SQL veritabanı gibi Azure kaynaklarına erişimi sınırlayabilirsiniz. Ayrıca, internet'ten kaynaklara erişimi reddedebilirsiniz. Birden fazla alt ağ oluşturmak ve bir hizmet uç noktası bazı alt ağlar, ancak diğer için etkinleştir. Daha fazla bilgi edinmek [hizmet uç noktaları](virtual-network-service-endpoints-overview.md), ve Azure kaynakları için etkinleştirebilirsiniz.
- Her bir sanal ağ alt sıfır veya bir ağ güvenlik grubuna ilişkilendirebilirsiniz. Aynı veya farklı bir ilişkilendirme, her alt ağ güvenlik grubuna ağ. Her ağ güvenlik grubu izin ver veya Reddet kaynakları ve hedeflere gelen ve giden trafik kurallarını içerir. Daha fazla bilgi edinmek [ağ güvenlik grubu](#traffic-filtering).

## <a name="security"></a>Güvenlik

Ağ güvenlik grupları ve ağ sanal Gereçleri kullanarak bir sanal ağ kaynaklarında gelen ve giden ağ trafiği filtreleyebilirsiniz. Azure nasıl yönlendirir denetleyebilirsiniz alt ağ trafiği. Kuruluşunuzda kimlerin sanal ağlarda bulunan kaynaklar ile çalışabilirsiniz sınırlayabilirsiniz.

### <a name="traffic-filtering"></a>Trafik filtreleme

- Ağ güvenlik grubu, ağ trafiği veya her ikisi de filtreler bir NVA kullanarak bir sanal ağ kaynaklarında arasındaki ağ trafiğini filtreleyebilirsiniz. Filtre ağ trafiğinin bir güvenlik duvarı gibi bir NVA dağıtmak için bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?subcategories=appliances&page=1). Bir NVA kullanırken, NVA alt ağ trafiği yönlendirmek için özel yollar oluşturabilir. Daha fazla bilgi edinmek [trafik yönlendirme](#traffic-routing).
- Ağ güvenlik grubu izin vermek veya reddetmek için veya kaynaklardan gelen trafiğin birkaç varsayılan güvenlik kuralları içerir. Bir ağ güvenlik grubu, bir ağ arabirimi, ağ arabiriminin bulunduğu alt ağ veya her ikisi de ilişkilendirilebilir. Güvenlik kuralları yönetimini basitleştirmek için ayrı alt ağlar için ağ güvenlik grubu ilişkilendirmek yerine tek tek ağ içindeki alt ağı, mümkün olduğunda arabirimleri önerilir.
- Bir alt ağ içindeki farklı VM'ler uygulanmış farklı güvenlik kuralları gerekiyorsa, bir veya daha fazla uygulama güvenlik grupları VM ağ arabiriminde ilişkilendirebilirsiniz. Güvenlik kuralı uygulama güvenlik grubu kendi kaynak, hedef veya her ikisini de belirtebilirsiniz. Bu kuralı yalnızca sonra uygulama güvenlik grubunun üyesi olan ağ arabirimleri için geçerlidir. Daha fazla bilgi edinmek [ağ güvenlik grubu](security-overview.md) ve [uygulama güvenlik grupları](security-overview.md#application-security-groups).
- Azure güvenlik kuralları her ağ güvenlik grubu içinde birkaç varsayılan oluşturur. Bir varsayılan kural bir sanal ağdaki tüm kaynaklar arasındaki tüm trafiğe izin verir. Bu davranışı geçersiz kılmak için ağ güvenliği kullanın, bir NVA veya her ikisine de trafiğini yönlendirmek için yönlendirme özel gruplar. Tüm Azure'nın öğrenmeniz olduğunu önerilir [güvenlik kuralları varsayılan](security-overview.md#default-security-rules) ve ağ güvenlik grubu kuralları bir kaynağa nasıl uygulandığını anladığınızdan emin olun.

Azure ile kullanarak internet arasında DMZ uygulamak için örnek tasarımları görüntüleyebileceğiniz bir [NVA](/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) veya [ağ güvenlik grubu](virtual-networks-dmz-nsg.md).

### <a name="traffic-routing"></a>trafik yönlendirme

Azure birkaç varsayılan yollar giden trafik için bir alt ağdan oluşturur. Bir yol tablosu oluşturarak ve bir alt ağa ilişkilendirme yönlendirme Azure'un varsayılan ayarlarını geçersiz kılabilir. Yaygın nedenler Azure'nın varsayılan geçersiz kılma için yönlendirme şunlardır:
- Bir NVA akış için alt ağlar arasında trafiği istediklerinden. Hakkında daha fazla bilgi edinmek için [bir NVA trafiğinin zorlamak için yol tablolarını yapılandırmak](tutorial-create-route-table-portal.md)
- Bir NVA veya şirket içi, bir Azure VPN ağ geçidi üzerinden aracılığıyla tüm Internet'e bağlı trafik zorlayabilirsiniz çünkü. Internet trafiği şirket içi İnceleme için zorlama ve günlüğe kaydetme genellikle zorlanan tünel olarak adlandırılır. Nasıl yapılandırılacağı hakkında daha fazla bilgi [zorlanan tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).

Özel yönlendirme uygulamanız gerekiyorsa, kendinizle tanıyın önerilir [Azure'da yönlendirme](virtual-networks-udr-overview.md).

## <a name="connectivity"></a>Bağlantı

Bir sanal ağ, bir Azure VPN ağ geçidi kullanarak sanal ağ eşlemesi kullanmanın diğer sanal ağlara ya da şirket içi ağınıza bağlanabilir.

### <a name="peering"></a>Eşleme

Kullanırken [sanal ağ eşlemesi](virtual-network-peering-overview.md), sanal ağları olabilir aynı veya farklı, Azure bölgeleri desteklenir. Her iki aboneliğin aynı Azure Active Directory Kiracı atanan sürece sanal ağlar aynı ya da farklı Azure Aboneliklerde olabilir. Bir eşleme oluşturmadan önce tüm eşliği ile öğrenmeniz olduğunu önerilir [gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints). Kaynaklar aynı sanal ağda değilmiş gibi eşlenen sanal ağlarda bulunan kaynaklar arasındaki bant genişliği aynıdır.

### <a name="vpn-gateway"></a>VPN ağ geçidi

Bir Azure kullanabilirsiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanarak, şirket içi ağ bir sanal ağa bağlanmak için bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json), veya Azure ile ayrılmış bir bağlantıyı kullanarak [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Birleştirebilirsiniz eşliği ve oluşturmak için bir VPN ağ geçidi [hub ve bağlı bileşen ağları](/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)burada spoke sanal ağlar bir hub sanal ağa bağlanmak ve hub örneğin bir şirket içi ağına bağlanır.

### <a name="name-resolution"></a>Ad çözümlemesi

Bir sanal ağ kaynaklarında Azure'un kullanarak eşlenen sanal ağ içindeki kaynakların adları çözmek olamaz [yerleşik DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md). Eşlenmiş bir sanal ağ adları çözümlemek için [kendi DNS sunucusu dağıtma](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server), ya da Azure DNS kullanabilir [özel etki alanları](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bir sanal ağ kaynaklarında ve şirket içi ağlar arasında ad çözümleme Ayrıca kendi DNS sunucusu dağıtmayı gerektirir.

## <a name="permissions"></a>İzinler

Azure yararlanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (RBAC) kaynaklara. İzinleri atanır bir [kapsam](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-hierarchy-and-access-inheritance) aşağıdaki hiyerarşideki: Abonelik, yönetim grubu, kaynak grubu ve tek tek kaynak. Hiyerarşi hakkında daha fazla bilgi için bkz: [kaynaklarınızı düzenleme](../azure-resource-manager/management-groups-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure sanal ağlar ve tüm ilgili yeteneklerini eşliği, ağ güvenlik grupları, hizmet uç noktaları ve yol tablolarını gibi çalışmak için kuruluş üyelerinin yerleşik olarak atayabilirsiniz [sahibi](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#owner), [Katkıda bulunan](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#contributor), veya [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve sonra Ata uygun kapsamı role. Sanal ağ özelliklerinin alt kümeleri için belirli izinler atamak istiyorsanız, oluşturma bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve için gereken belirli izinleri atama [sanal ağlar](manage-virtual-network.md#permissions), [ alt ağları ve hizmet uç noktaları](virtual-network-manage-subnet.md#permissions), [ağ arabirimleri](virtual-network-network-interface.md), [eşliği](virtual-network-manage-peering.md#permissions), [ağ ve uygulama güvenlik grupları](manage-network-security-group.md#permissions), veya [yol tablosu](manage-route-table.md#permissions) rolü.

## <a name="policy"></a>İlke

Azure ilke oluşturmak, atamak ve ilke tanımları yönetmenizi sağlar. Kaynakları kuruluş standartları ve hizmet düzeyi sözleşmeleri ile uyumlu olmak için İlke tanımları farklı kurallar ve etkileri kaynaklarınızı zorlar. Azure ilke elinizde ilke tanımları ile uyumlu olmayan kaynaklar için tarama kaynaklarınızı değerlendirme çalışır. Örneğin, yalnızca belirli bir kaynak grubunda sanal ağlar oluşturulmasına izin veren bir ilke olabilir. Başka bir ilke, her alt ağ için ilişkili bir ağ güvenlik grubu olduğunu gerektirebilir. İlkeleri oluşturma ve kaynakları güncelleştirme, ardından değerlendirilir.

İlkeleri aşağıdaki hiyerarşi uygulanır: Abonelik, yönetim grubu ve kaynak grubu. Daha fazla bilgi edinmek [Azure ilke](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ya da bazı sanal ağı dağıtmak [ilke şablonu](policy-samples.md) örnekleri.
