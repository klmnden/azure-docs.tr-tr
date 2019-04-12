---
title: Azure sanal ağ eşlemesi
titlesuffix: Azure Virtual Network
description: Azure'daki sanal ağ eşlemesi hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: anavin
ms.openlocfilehash: e6c5a9aa3e4e173ecfc79f4072d091493677afed
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59489990"
---
# <a name="virtual-network-peering"></a>Sanal ağ eşleme

Sanal Ağ eşlemesi sayesinde Azure sorunsuz bir şekilde bağlanmak [sanal ağlar](virtual-networks-overview.md). Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlarda bulunan sanal makineler arasındaki trafik, Microsoft omurga altyapısı aracılığıyla tıpkı sanal ağdaki sanal makineler arasında olduğu gibi yalnızca *özel* IP adresleri üzerinden yönlendirilir. Azure’ın destekledikleri:
* Sanal ağ eşleme - aynı Azure bölgesindeki sanal ağları bağlama
* Genel sanal ağ eşleme - Azure bölgeleri arasında sanal ağları bağlama

Yerel veya genel sanal ağ eşlemesini kullanmanın avantajları şunlardır:

* Eşlenen sanal ağlar arasındaki ağ trafiği gizlidir. Sanal ağlar arasındaki trafik Microsoft omurga ağı üzerinde tutulur. Sanal ağlar arasındaki iletişimde genel İnternet, ağ geçidi veya şifreleme gerekli değildir.
* Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süresi ve yüksek bant genişlikli bağlantı.
* Sanal ağlar eşlendikten sonra Sanal ağların birindeki kaynaklar farklı bir sanal ağdaki kaynaklarla iletişim kurabilir.
* Verilerinizi farklı Azure abonelikleri, dağıtım modelleri ve Azure bölgeleri arasında taşıma imkanı.
* Azure Resource Manager ile oluşturulan sanal ağları eşleyebilme veya Resource Manager ile oluşturulan bir sanal ağı klasik dağıtım modeliyle oluşturulan sanal ağ ile eşleyebilme özelliği. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini kavrama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Eşleme oluştururken veya eşleme oluşturulduktan sonra iki sanal ağdaki kaynaklarda da kesinti süresi yaşanmaz.

## <a name="connectivity"></a>Bağlantı

Sanal ağlar eşlendikten sonra, eşlenen sanal ağlardan herhangi birindeki kaynaklar diğer sanal ağın kaynaklarıyla doğrudan bağlantı kurabilir.

Aynı bölgede yer alan eşlenmiş sanal ağlardaki sanal makineler arasındaki ağ gecikme süresi, tek bir sanal ağdaki gecikme süresiyle aynıdır. Ağ verimi, büyüklüğüne orantılı olarak sanal makine için izin verilen bant genişliğine bağlıdır. Eşleme içindeki bant genişliği ile ilgili herhangi bir ek kısıtlama yoktur.

Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki trafik bir ağ geçidi veya genel İnternet üzerinden değil, doğrudan Microsoft omurga altyapısı aracılığıyla yönlendirilir.

İstendiğinde, diğer sanal ağlara veya alt ağlara erişimi engellemek için her bir sanal ağda ağ güvenlik grupları uygulanabilir.
Sanal ağ eşlemesi yapılandırırken, sanal ağlar arasındaki ağ güvenlik grubu kurallarını açabilir veya kapatabilirsiniz. Eşlenen sanal ağlar arasında tam bağlantıyı (varsayılan seçenek) açarsanız, belirli erişimleri engellemek ya da reddetmek için belirli alt ağlara veya sanal makinelere ağ güvenlik grupları uygulayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek bkz. [Ağ güvenlik gruplarına genel bakış](security-overview.md).

## <a name="service-chaining"></a>Hizmet zinciri

Hizmet zinciri oluşturmayı etkinleştirmek için *sonraki atlama* IP adresi olarak eşlenen sanal ağlardaki sanal makinelere veya sanal makine ağ geçitlerine işaret eden kullanıcı tanımlı yollar yapılandırabilirsiniz. Hizmet zinciri oluşturma, kullanıcı tanımlı yollar aracılığıyla trafiği bir sanal ağdan eşlenmiş bir sanal ağdaki bir sanal gerece veya bir sanal ağ geçidine yönlendirmenize imkan tanır.

Ayrıca, hub ve bağlı bileşen ağlarını da verimli bir şekilde oluşturabilirsiniz. Bu ağlarda hub sanal ağı, ağ sanal gereci veya VPN ağ geçidi gibi altyapı bileşenlerini barındırabilir. Daha sonra, tüm ağlı sanal ağlar merkezi sanal ağla eşlenebilir. Trafik, hub sanal ağındaki ağ sanal gereçleri veya VPN ağ geçitleri üzerinden akabilir. 

Sanal ağ eşlemesi sayesinde, kullanıcı tanımlı yolda bir sonraki atlama, eşlenen sanal ağdaki veya bir VPN ağ geçidindeki bir sanal makinenin IP adresi olabilir. Ancak, sonraki atlama türü olarak ExpressRoute ağ geçidini belirten kullanıcı tanımlı bir yol ile sanal ağlar arasında yönlendirme yapamazsınız. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [Kullanıcı tanımlı yollara genel bakış](virtual-networks-udr-overview.md#user-defined). Merkez ve uç ağ topolojisi oluşturmayı öğrenmek için bkz. [Merkez ve uç ağ topolojisi oluşturma](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="gateways-and-on-premises-connectivity"></a>Ağ geçitleri ve şirket içi bağlantı

Her sanal ağ başka bir sanal ağ ile eşlenip eşlenmediğine bakılmaksızın kendi ağ geçidine sahip olabilir ve bu sanal ağ geçidini şirket içi bir ağa bağlanmak için kullanabilir. Ayrıca, sanal ağlar eşlenmiş olsa bile ağ geçitlerini kullanarak [sanal ağlar arası bağlantılar](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırabilirsiniz.

Sanal ağlar arası bağlantı için her iki seçenek de yapılandırıldığında, sanal ağlar arasındaki trafik, eşleme yapılandırması (Azure omurgası) üzerinden akış gerçekleştirir.

Sanal ağlar eşlendiğinde, eşlenmiş sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Bu durumda, uzak ağ geçidi kullanan sanal ağın kendi ağ geçidi olamaz. Bir sanal ağın yalnızca bir ağ geçidi olabilir. Ağ geçidi, aşağıdaki resimde gösterildiği gibi yerel veya uzak bir ağ geçidi (eşlenen sanal ağda) olabilir:

![Sanal ağ eşleme geçişi](./media/virtual-networks-peering-overview/figure04.png)

VNet eşlemesi hem genel sanal ağ eşleme (Önizleme) için ağ geçidi geçişi desteklenir. Önizleme'de genel olarak eşlenmiş sanal ağlardaki ağ geçidi aktarımına izin ver ya da uzak ağ geçitlerini kullan. Önizleme, tüm Azure bölgeleri, Çin bulut bölgeleri ve kamu bulut bölgelerinde kullanılabilir. Hiçbir beyaz listeye ekleme gereklidir. CLI, PowerShell, şablonları veya API üzerinden önizlemede test edebilirsiniz. Portal Önizleme sürümünde desteklenmiyor.
Yalnızca ağ geçidi sanal ağ (Resource Manager) varsa (Resource Manager ve klasik) farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasındaki ağ geçidi geçişi desteklenir. Geçiş için bir ağ geçidi kullanma hakkında daha fazla bilgi için bkz. [Sanal ağ eşlemesinde geçiş için bir VPN ağ geçidi yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Tek bir Azure ExpressRoute bağlantısını kullanan sanal ağlar eşlendiğinde, bu iki sanal ağ arasındaki trafik, eşleme ilişkisi (Azure omurga ağı) üzerinden akış gerçekleştirir. Şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilirsiniz. Alternatif olarak, paylaşılan bir ağ geçidini kullanıp şirket içi bağlantı için bir geçiş yapılandırabilirsiniz.

## <a name="troubleshoot"></a>Sorun giderme

Bir sanal ağ eşlemesini onaylamak için, bir sanal ağdaki herhangi bir alt ağın ağ arabirimine yönelik [etkili yolları denetleyebilirsiniz](diagnose-network-routing-problem.md). Bir sanal ağ eşlemesi zaten varsa, sanal ağ içindeki tüm alt ağlar, eşlenen her bir sanal ağdaki her bir adres alanı için sonraki atlama türü *VNet eşlemesi* olan yollara sahip olur.

Eşlenmiş sanal ağdaki bir sanal makinenin bağlantı durumuyla ilgili sorunları gidermek için Ağ İzleyicisi'nin [bağlantı denetimini](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de kullanabilirsiniz. Bağlantı denetimi sayesinde trafiğin bir kaynak sanal makinenin ağ arabiriminden hedef sanal makinenin ağ arabirimine nasıl yönlendirildiğini denetleyebilirsiniz.

Ayrıca deneyebilirsiniz [sanal ağ eşleme sorunları için sorun giderici](https://support.microsoft.com/help/4486956/troubleshooter-for-virtual-network-peering-issues).

## <a name="requirements-and-constraints"></a>Gereksinimler ve kısıtlamalar

Yalnızca, sanal ağlar genel olarak eşlenmiş aşağıdaki kısıtlamalar uygulanır:
- Bir sanal ağ içindeki kaynaklarla genel olarak eşlenmiş sanal ağdaki bir iç temel yük dengeleyicinin ön uç IP adresi ile iletişim kuramıyor. Temel yük dengeleyici desteği yalnızca aynı bölge içinde bulunmaktadır. Küresel VNet eşlemesi için standart yük dengeleyici desteği yok.

Gereksinimler ve kısıtlamalar hakkında daha fazla bilgi edinmek için bkz. [Sanal ağ eşleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints). Bir sanal ağ için oluşturabileceğiniz eşleme sayısı sınırları hakkında bilgi edinmek için bkz. [Azure ağ sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). 

## <a name="permissions"></a>İzinler

Bir sanal ağ eşlemesi oluşturmak için gereken izinler hakkında bilgi edinmek için bkz. [Sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).

## <a name="pricing"></a>Fiyatlandırma

Sanal ağ eşleme bağlantısı kullanan girdi ve çıkış trafiği için nominal bir ücret uygulanır. Sanal Ağ Eşleme ve Genel Sanal Ağ eşleme fiyatlandırması hakkında daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).

Ağ geçidi aktarımı, bir sanal ağın şirket içi ve dışı karışık bağlantı ya da sanal ağlar arası bağlantı için eşlenmiş sanal ağdaki bir VPN ağ geçidinden yararlanmasını sağlayan bir eşleme özelliğidir. Bu senaryoda uzak ağ geçidinden geçen trafik [VPN ağ geçidi ücretlerine](https://azure.microsoft.com/pricing/details/vpn-gateway/) tabidir ve [Sanal ağ eşleme ücretleri](https://azure.microsoft.com/pricing/details/virtual-network) alınmaz. Örneğin, Sanalağa şirket içi bağlantı için bir VPN ağ geçidi varsa ve Sanalağb Sanalağa için yapılandırılmış uygun özellikleri ile eşlenmişse, şirket içi Sanalağb gelen trafiği yalnızca VPN ağ geçidi fiyatlandırması çıkışı ücretlendirilir. Sanal ağ eşleme ücretleri uygulanmaz. [Sanal ağ eşlemesi için VPN ağ geçidi aktarımını yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* Aynı veya farklı aboneliklerde, aynı veya farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında, bir sanal ağ eşlemesi oluşturulur. Aşağıdaki senaryolardan biri için öğreticiyi tamamlayın:

    |Azure dağıtım modeli             | Abonelik  |
    |---------                          |---------|
    |Her ikisi de Resource Manager              |[Aynı](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Fark](create-peering-different-subscriptions.md)|
    |Biri Resource Manager, diğeri klasik  |[Aynı](create-peering-different-deployment-models.md)|
    |                                   |[Fark](create-peering-different-deployment-models-subscriptions.md)|

* [Hub ve bağlı bileşen ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmayı öğrenin.
* Tüm [sanal ağ eşleme ayarları ve ayarların nasıl değiştirileceği](virtual-network-manage-peering.md) hakkında bilgi edinin.
* Sık sorulan Sanal Ağ Eşleme ve Genel Sanal Ağ Eşleme sorularının yanıtları için bkz. [Sanal Ağ Eşleme Hakkında SSS](virtual-networks-faq.md#vnet-peering)
