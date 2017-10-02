---
title: "Azure Sanal Ağ eşlemesi | Microsoft Belgeleri"
description: "Azure'daki sanal ağ eşlemesi hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: narayan;anavin
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 082cd8a6cf50f76c89fe5995047396c734f83034
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---
# <a name="virtual-network-peering"></a>Sanal ağ eşleme

[Azure Sanal Ağ](virtual-networks-overview.md), Azure'da yer alan özel ağ alanınızdır ve Azure kaynakları arasında güvenli bağlantı kurmanızı sağlar.

Sanal ağ eşlemesi sayesinde sanal ağlara sorunsuz bir şekilde bağlanabilirsiniz. Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlarda bulunan sanal makineler birbiriyle doğrudan iletişim kurabilir.
Eşlenen sanal ağlarda bulunan sanal makineler arasındaki trafik, Microsoft omurga altyapısı aracılığıyla tıpkı sanal ağdaki sanal makineler arasında olduğu gibi yalnızca *özel* IP adresleri üzerinden yönlendirilir.

>[!IMPORTANT]
> Farklı Azure bölgelerindeki sanal ağları eşleyebilirsiniz. Bu özellik şu anda önizleme sürümündedir. [Aboneliğinizi önizleme programına kaydedebilirsiniz](virtual-network-create-peering.md). Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır.
>

Sanal ağ eşlemesini kullanmanın avantajları şunlardır:

* Sanal ağ eşlemeleri üzerinden geçen trafik tamamen gizlidir. Microsoft omurga ağı üzerinden geçer ve herhangi bir genel internet veya ağ geçidi kullanılmaz.
* Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süresi ve yüksek bant genişlikli bağlantı.
* Eşleme sonrasında bir sanal ağ içindeki kaynakları diğer sanal ağdan kullanabilme özelliği.
* Sanal ağ eşlemesi verilerinizi farklı Azure abonelikleri, dağıtım modelleri ve Azure bölgeleri (önizleme) arasında taşımanıza yardımcı olur.
* Azure Resource Manager ile oluşturulan sanal ağları eşleyebilme veya Resource Manager ile oluşturulan bir sanal ağı klasik dağıtım modeliyle oluşturulan sanal ağ ile eşleyebilme özelliği. İki Azure dağıtım modeli arasındaki fark hakkında daha fazla bilgi almak için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesini okuyun.

## <a name="requirements-constraints"></a>Gereksinimler ve kısıtlamalar

* Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Farklı bölgelerdeki sanal ağları eşleme özelliği ABD Orta Batı, Kanada Orta ve ABD Batı 2 bölgelerinde önizleme sürümündedir. [Aboneliğinizi önizleme programına kaydedebilirsiniz.](virtual-network-create-peering.md)
    > [!WARNING]
    > Bu senaryoda oluşturulan sanal ağ eşlemeleri genel kullanım sürümünde mevcut olan senaryolarla aynı kullanılabilirlik ve güvenilirlik seviyesine sahip değildir. Sanal ağ eşlemeleri sınırlı özelliklere sahip olabilir ve tüm Azure bölgelerinde kullanılamayabilir. Bu özelliğin kullanılabilirliği ve durumuyla ilgili en güncel bildirimler için, [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasına bakın.

* Eşlenmiş sanal ağların IP adresi alanları çakışmamalıdır.
* Bir sanal ağ başka bir sanal ağla eşlendikten sonra sanal ağa adres alanı eklenemez veya ağdaki bir adres alanı silinemez.
* Sanal ağ eşlemesi iki sanal ağ arasında gerçekleşir. Eşlemeler arasında türetilmiş geçişli bir ilişki yoktur. Örneğin, virtualNetworkA ile virtualNetworkB; virtualNetworkB ile de virtualNetworkC eşlenirse, virtualNetworkA ile virtualNetworkC arasında eşleme *olmaz*.
* Eşlemenin her iki aboneliğin de ayrıcalıklı bir kullanıcı (bkz. [belirli izinler](create-peering-different-deployment-models-subscriptions.md#permissions)) tarafından yetkilendirilmiş olması ve aboneliklerin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi şartıyla, iki farklı abonelikte mevcut olan sanal ağları eşleyebilirsiniz. Farklı Active Directory kiracılarıyla ilişkili aboneliklerdeki sanal ağları bağlamak için bir [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
* Her iki sanal ağ da Resource Manager dağıtım modeliyle oluşturulursa veya bir sanal ağ Resource Manager dağıtım modeliyle, diğeri ise klasik dağıtım modeliyle oluşturulursa, sanal ağlar eşlenebilir. Ancak, klasik dağıtım modeliyle oluşturulan sanal ağlar birbiriyle eşlenemez. Klasik dağıtım modeliyle oluşturulan sanal ağları bağlamak için [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
* Eşlenmiş sanal ağlardaki sanal makineler arasında kurulan iletişim için ek bant genişliği kısıtlamaları olmasa da, sanal makine boyutuna bağlı olarak hala geçerli olan bir ağ bant genişliği üst sınırı vardır. Farklı sanal makine boyutlarına yönelik ağ bant genişliği üst sınırları hakkında daha fazla bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları makalelerini okuyun.

     ![Temel sanal ağ eşleme](./media/virtual-networks-peering-overview/figure03.png)

## <a name="connectivity"></a>Bağlantı

Sanal ağlar eşlendikten sonra, eşlenen sanal ağlardan herhangi birindeki kaynaklar diğer sanal ağın kaynaklarıyla doğrudan bağlantı kurabilir.

Aynı bölgede yer alan eşlenmiş sanal ağlardaki sanal makineler arasındaki ağ gecikme süresi, tek bir sanal ağdakiyle aynıdır. Ağ verimi, büyüklüğüne orantılı olarak sanal makine için izin verilen bant genişliğine bağlıdır. Eşleme içindeki bant genişliği ile ilgili herhangi bir ek kısıtlama yoktur.

Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki trafik bir ağ geçidi veya genel internet üzerinden değil, doğrudan Microsoft omurga altyapısı aracılığıyla yönlendirilir.

Bir sanal ağ üzerindeki sanal makineler, aynı bölgedeki eşlenmiş sanal ağ üzerindeki iç yük dengeleyiciye erişebilir. İç yük dengeleyici desteği önizleme sürümünde genel olarak eşlenmiş sanal ağlara genişletilmez. Genel sanal ağ eşlemesinin genel kullanım sürümünde iç yük dengeleyici desteği sunulacaktır.

İstendiğinde, diğer sanal ağlara veya alt ağlara erişimi engellemek için her bir sanal ağda ağ güvenlik grupları uygulanabilir.
Sanal ağ eşlemesi yapılandırırken, sanal ağlar arasındaki ağ güvenlik grubu kurallarını açabilir veya kapatabilirsiniz. Eşlenen sanal ağlar arasında tam bağlantıyı (varsayılan seçenek) açarsanız, belirli erişimleri engellemek ya da reddetmek için belirli alt ağlara veya sanal makinelere ağ güvenlik grupları uygulayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına genel bakış](virtual-networks-nsg.md) makalesini okuyun.

## <a name="service-chaining"></a>Hizmet zinciri

Hizmet zinciri oluşturmayı etkinleştirmek için işlenen sanal ağlardaki sanal makineleri "sonraki atlama" IP adresi olarak işaret eden kullanıcı tanımlı yollar yapılandırabilirsiniz. Hizmet zinciri oluşturma, kullanıcı tanımlı yollar aracılığıyla trafiği bir sanal ağdan eşlenmiş bir sanal ağdaki bir sanal gerece yönlendirmenize imkan tanır.

Ayrıca, hub ve bağlı bileşen türündeki ortamları da verimli bir şekilde oluşturabilirsiniz. Bu ortamlarda hub, ağ sanal gereci gibi altyapı bileşenlerini barındırabilir. Daha sonra, tüm ağlı sanal ağlar merkezi sanal ağla eşlenebilir. Trafik, merkezi sanal ağda çalışan ağ sanal gereçleri üzerinden akabilir. Kısacası, sanal ağ eşlemesi sayesinde, kullanıcı tanımlı yolda bir sonraki atlama IP adresi, eşlenen sanal ağdaki bir sanal makinenin IP adresi olabilir. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için, [kullanıcı tanımlı yollara genel bakış](virtual-networks-udr-overview.md) makalesini okuyun. [Merkez ve uç ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering) oluşturmayı öğrenin

## <a name="gateways-and-on-premises-connectivity"></a>Ağ geçitleri ve şirket içi bağlantı

Her sanal ağ başka bir sanal ağ ile eşlenip eşlenmediğine bakılmaksızın kendi ağ geçidine sahip olabilir ve bu sanal ağ geçidini şirket içi bir ağa bağlanmak için kullanabilir. Ayrıca, sanal ağlar eşlenmiş olsa bile ağ geçitlerini kullanarak [sanal ağlar arası bağlantılar](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) yapılandırabilirsiniz.

Sanal ağlar arası bağlantı için her iki seçenek de yapılandırıldığında, sanal ağlar arasındaki trafik, eşleme yapılandırması (Azure omurgası) üzerinden akış gerçekleştirir.

Aynı bölgedeki sanal ağlar eşlendiğinde, eşlenmiş sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Bu durumda, uzak ağ geçidi kullanan sanal ağın kendi ağ geçidi olamaz. Bir sanal ağın yalnızca bir ağ geçidi olabilir. Ağ geçidi, aşağıdaki resimde gösterildiği gibi yerel veya uzak bir ağ geçidi (eşlenen sanal ağda) olabilir:

![Sanal ağ eşleme geçişi](./media/virtual-networks-peering-overview/figure04.png)

Farklı dağıtım modelleriyle veya farklı bölgelerde oluşturulmuş sanal ağlar arasındaki eşleme ilişkisinde ağ geçidi geçişi desteklenmez. Bir ağ geçidi geçişinin çalışması için eşleme ilişkisindeki her iki sanal ağ da Resource Manager ile ve aynı bölgede oluşturulmuş olmalıdır. Genel olarak eşlenmiş sanal ağlar şu anda ağ geçidi geçişi desteği sunmamaktadır.

Tek bir Azure ExpressRoute bağlantısını kullanan sanal ağlar eşlendiğinde, bu iki sanal ağ arasındaki trafik, eşleme ilişkisi (Azure omurga ağı) üzerinden akış gerçekleştirir. Şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilirsiniz. Alternatif olarak, paylaşılan bir ağ geçidini kullanıp şirket içi bağlantı için bir geçiş yapılandırabilirsiniz.

## <a name="permissions"></a>İzinler

Sanal ağ eşlemesi ayrıcalıklı bir işlemdir. VirtualNetworks ad alanı altında yer alan ayrı bir işlevdir. Bir kullanıcıya eşlemeyi yetkilendirmesi için belirli haklar verilebilir. Sanal ağa yönelik okuma/yazma erişimi olan bir kullanıcı bu haklara otomatik olarak sahip olur.

Eşleme özelliğinin yöneticisi ya da ayrıcalıklı kullanıcısı olan bir kullanıcı, başka bir sanal ağ üzerinde eşleme işlemi başlatabilir. Gerekli minimum izin seviyesi Ağ Katılımcısı olarak belirlenmiştir. Diğer tarafta eşleme için eşleşen bir istek varsa ve diğer gereksinimler karşılanırsa eşleme gerçekleştirilir.

Örneğin myvirtual networkA ve myvirtual networkB adlı sanal ağları eşliyorsanız hesabınıza her bir sanal ağ için aşağıdaki minimum rol veya izin atanmış olmalıdır:

|Sanal ağ|Dağıtım modeli|Rol|İzinler|
|---|---|---|---|
|myvirtual networkA|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Yok|
|myvirtual networkB|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

## <a name="monitor"></a>İzleme

Resource Manager ile oluşturulmuş olan iki sanal ağı eşlerken eşlemedeki her sanal ağ için bir eşleme yapılandırılması gerekir.
Eşleme bağlantınızın durumunu izleyebilirsiniz. Eşleme durumu aşağıdakilerden biri olabilir:

* **Başlatıldı**: İlk sanal ağdan ikinci sanal ağa eşleme oluşturduğunuzda eşleme durumu Başlatıldı olur.

* **Bağlandı**: İkinci sanal ağdan ilk sanal ağa eşleme oluşturduğunuzda eşleme durumu Bağlandı olur. İlk sanal ağ için eşleme durumunu görüntülerseniz Başlatıldı olan durumunun Bağlandı olarak değiştiğini görürsünüz. İki sanal ağ eşlemesinin de eşleme durumu Bağlandı olana kadar eşleme başarıyla oluşturulmuş olmaz.

* **Bağlantı kesildi**: Bağlantı kurulduktan sonra eşleme bağlantılarınızdan birinin silinmesi halinde eşleme durumu Bağlantı kesildi olur.

## <a name="troubleshoot"></a>Sorun giderme

Eşleme bağlantınızda akan trafikle ilgili sorunları gidermek için [geçerli rotalarınızı kontrol edebilirsiniz.](virtual-network-routes-troubleshoot-portal.md)

Eşlenmiş sanal ağdaki bir sanal makinenin bağlantı durumuyla ilgili sorunları gidermek için Ağ İzleyicisi'nin [bağlantı denetimini](../network-watcher/network-watcher-connectivity-portal.md) de kullanabilirsiniz. Bağlantı denetimi kaynak sanal makinenizin ağ arabiriminden hedef sanal makinenizin ağ arabirimine giden rotayı görmenizi sağlar.

## <a name="limits"></a>Sınırlar

Tek bir sanal ağ için izin verilen eşleme sayısı sınırlıdır. Varsayılan eşleme sayısı 50 olarak belirlenmiştir. Eşleme sayısını artırabilirsiniz. Daha fazla bilgi edinmek için [Azure ağ sınırlarını](../azure-subscription-service-limits.md#networking-limits) gözden geçirin.

## <a name="pricing"></a>Fiyatlandırma

Sanal ağ eşleme bağlantısı kullanan girdi ve çıkış trafiği için nominal bir ücret uygulanır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Sonraki adımlar

* Sanal ağ eşleme öğreticisini tamamlayın. Aynı veya farklı aboneliklerde, aynı veya farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında, bir sanal ağ eşlemesi oluşturulur. Aşağıdaki senaryolardan biri için öğreticiyi tamamlayın:

    |Azure dağıtım modeli  | Abonelik  |
    |---------|---------|
    |Her ikisi de Resource Manager |[Aynı](virtual-network-create-peering.md)|
    | |[Farklı](create-peering-different-subscriptions.md)|
    |Biri Resource Manager, diğeri klasik     |[Aynı](create-peering-different-deployment-models.md)|
    | |[Farklı](create-peering-different-deployment-models-subscriptions.md)|

* [Merkez ve uç ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering) oluşturmayı öğrenin.
* Tüm [sanal ağ eşleme ayarları ve ayarların nasıl değiştirileceği](virtual-network-manage-peering.md) hakkında bilgi edinin

