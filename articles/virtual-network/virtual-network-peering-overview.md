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
ms.openlocfilehash: 7c384f07ec6b71596dcdbc5b7214fa7ce65d0b7d
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="virtual-network-peering"></a>Sanal ağ eşleme

Sanal ağ eşlemesi sayesinde iki Azure [sanal ağına](virtual-networks-overview.md) sorunsuz bir şekilde bağlanabilirsiniz. Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlarda bulunan sanal makineler arasındaki trafik, Microsoft omurga altyapısı aracılığıyla tıpkı sanal ağdaki sanal makineler arasında olduğu gibi yalnızca *özel* IP adresleri üzerinden yönlendirilir. 

Sanal ağ eşlemesini kullanmanın avantajları şunlardır:

* Eşlenen sanal ağlar arasındaki ağ trafiği gizlidir. Sanal ağlar arasındaki trafik Microsoft omurga ağı üzerinde tutulur. Sanal ağlar arasındaki iletişimde genel İnternet, ağ geçidi veya şifreleme gerekli değildir.
* Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süresi ve yüksek bant genişlikli bağlantı.
* Sanal ağlar eşlendikten sonra Sanal ağların birindeki kaynaklar farklı bir sanal ağdaki kaynaklarla iletişim kurabilir.
* Verilerinizi farklı Azure abonelikleri, dağıtım modelleri ve Azure bölgeleri (önizleme) arasında taşıma imkanı.
* Azure Resource Manager ile oluşturulan sanal ağları eşleyebilme veya Resource Manager ile oluşturulan bir sanal ağı klasik dağıtım modeliyle oluşturulan sanal ağ ile eşleyebilme özelliği. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini kavrama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Eşleme oluştururken veya eşleme oluşturulduktan sonra iki sanal ağdaki kaynaklarda da kesinti süresi yaşanmaz.

## <a name="requirements-constraints"></a>Gereksinimler ve kısıtlamalar

* Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Farklı bölgelerdeki sanal ağları eş düğümleme, şu anda ABD Orta Batı, Kanada Orta, ABD Batı 2, Kore Güney, UK Güney, UK Batı, Kanada Doğu, Hindistan Güney, Hindistan Orta ve Hindistan Batı bölgelerinde önizleme aşamasındadır. Farklı bölgelerdeki sanal ağları eşlemeden önce, önizleme için [aboneliğinizi kaydetmeniz](virtual-network-create-peering.md#register) gerekir. Önizleme için kaydı tamamlamadıysanız, farklı bölgelerdeki sanal ağlar arasında bir eşleme oluşturma denemesi başarısız olur.
    > [!WARNING]
    > Birden fazla bölge arasında oluşturulan sanal ağ eşlemeleri genel kullanım sürümünde mevcut olan eşlemelerle aynı kullanılabilirlik ve güvenilirlik seviyesine sahip değildir. Sanal ağ eşlemeleri sınırlı özelliklere sahip olabilir ve tüm Azure bölgelerinde kullanılamayabilir. Bu özelliğin kullanılabilirliği ve durumuyla ilgili en güncel bildirimler için, [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasına bakın.

* Eşlenmiş sanal ağların IP adresi alanları çakışmamalıdır.
* Bir sanal ağ başka bir sanal ağla eşlendikten sonra sanal ağa adres aralığı eklenemez veya ağdaki bir adres aralığı silinemez. Eşlenmiş sanal ağın adres alanına adres aralığı eklemeniz gerekiyorsa eşlemeyi kaldırmanız, adres alanını eklemeniz ve eşlemeyi tekrar kurmanız gerekir.
* Sanal ağ eşlemesi iki sanal ağ arasında gerçekleşir. Eşlemeler arasında türetilmiş geçişli bir ilişki yoktur. Örneğin, virtualNetworkA ile virtualNetworkB; virtualNetworkB ile de virtualNetworkC eşlenirse, virtualNetworkA ile virtualNetworkC arasında eşleme *olmaz*.
* Eşlemenin her iki aboneliğin de ayrıcalıklı bir kullanıcı (bkz. [belirli izinler](create-peering-different-deployment-models-subscriptions.md#permissions)) tarafından yetkilendirilmiş olması ve aboneliklerin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi şartıyla, iki farklı abonelikte mevcut olan sanal ağları eşleyebilirsiniz. Farklı Active Directory kiracılarıyla ilişkili aboneliklerdeki sanal ağları bağlamak için bir [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
* Her iki sanal ağ da Resource Manager dağıtım modeliyle oluşturulursa veya bir sanal ağ Resource Manager dağıtım modeliyle, diğeri ise klasik dağıtım modeliyle oluşturulursa, sanal ağlar eşlenebilir. Ancak, klasik dağıtım modeliyle oluşturulan sanal ağlar birbiriyle eşlenemez. Klasik dağıtım modeliyle oluşturulan sanal ağları bağlamak için [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
* Eşlenmiş sanal ağlardaki sanal makineler arasında kurulan iletişim için ek bant genişliği kısıtlamaları olmasa da, sanal makine boyutuna bağlı olarak hala geçerli olan bir ağ bant genişliği üst sınırı vardır. Farklı sanal makine boyutlarına yönelik ağ bant genişliği üst sınırları hakkında daha fazla bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları makalelerini okuyun.

     ![Temel sanal ağ eşleme](./media/virtual-networks-peering-overview/figure03.png)

## <a name="connectivity"></a>Bağlantı

Sanal ağlar eşlendikten sonra, eşlenen sanal ağlardan herhangi birindeki kaynaklar diğer sanal ağın kaynaklarıyla doğrudan bağlantı kurabilir.

Aynı bölgede yer alan eşlenmiş sanal ağlardaki sanal makineler arasındaki ağ gecikme süresi, tek bir sanal ağdaki gecikme süresiyle aynıdır. Ağ verimi, büyüklüğüne orantılı olarak sanal makine için izin verilen bant genişliğine bağlıdır. Eşleme içindeki bant genişliği ile ilgili herhangi bir ek kısıtlama yoktur.

Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki trafik bir ağ geçidi veya genel İnternet üzerinden değil, doğrudan Microsoft omurga altyapısı aracılığıyla yönlendirilir.

Bir sanal ağ üzerindeki sanal makineler, aynı bölgedeki eşlenmiş sanal ağ üzerindeki iç yük dengeleyiciye erişebilir. İç yük dengeleyici desteği genel olarak eşlenmiş sanal ağlara (önizleme) genişletilmez. Genel sanal ağ eşlemesinin genel kullanım sürümünde iç yük dengeleyici desteği sunulacaktır.

İstendiğinde, diğer sanal ağlara veya alt ağlara erişimi engellemek için her bir sanal ağda ağ güvenlik grupları uygulanabilir.
Sanal ağ eşlemesi yapılandırırken, sanal ağlar arasındaki ağ güvenlik grubu kurallarını açabilir veya kapatabilirsiniz. Eşlenen sanal ağlar arasında tam bağlantıyı (varsayılan seçenek) açarsanız, belirli erişimleri engellemek ya da reddetmek için belirli alt ağlara veya sanal makinelere ağ güvenlik grupları uygulayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek bkz. [Ağ güvenlik gruplarına genel bakış](virtual-networks-nsg.md).

## <a name="service-chaining"></a>Hizmet zinciri

Hizmet zinciri oluşturmayı etkinleştirmek için *sonraki atlama* IP adresi olarak eşlenen sanal ağlardaki sanal makinelere veya sanal makine ağ geçitlerine işaret eden kullanıcı tanımlı yollar yapılandırabilirsiniz. Hizmet zinciri oluşturma, kullanıcı tanımlı yollar aracılığıyla trafiği bir sanal ağdan eşlenmiş bir sanal ağdaki bir sanal gerece veya bir sanal ağ geçidine yönlendirmenize imkan tanır.

Ayrıca, hub ve bağlı bileşen ağlarını da verimli bir şekilde oluşturabilirsiniz. Bu ağlarda hub sanal ağı, ağ sanal gereci veya VPN ağ geçidi gibi altyapı bileşenlerini barındırabilir. Daha sonra, tüm ağlı sanal ağlar merkezi sanal ağla eşlenebilir. Trafik, hub sanal ağındaki ağ sanal gereçleri veya VPN ağ geçitleri üzerinden akabilir. 

Sanal ağ eşlemesi sayesinde, kullanıcı tanımlı yolda bir sonraki atlama, eşlenen sanal ağdaki veya bir VPN ağ geçidindeki bir sanal makinenin IP adresi olabilir. Ancak, sonraki atlama türü olarak ExpressRoute ağ geçidini belirten kullanıcı tanımlı bir yol ile sanal ağlar arasında yönlendirme yapamazsınız. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [Kullanıcı tanımlı yollara genel bakış](virtual-networks-udr-overview.md#user-defined). Merkez ve uç ağ topolojisi oluşturmayı öğrenmek için bkz. [Merkez ve uç ağ topolojisi oluşturma](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).

## <a name="gateways-and-on-premises-connectivity"></a>Ağ geçitleri ve şirket içi bağlantı

Her sanal ağ başka bir sanal ağ ile eşlenip eşlenmediğine bakılmaksızın kendi ağ geçidine sahip olabilir ve bu sanal ağ geçidini şirket içi bir ağa bağlanmak için kullanabilir. Ayrıca, sanal ağlar eşlenmiş olsa bile ağ geçitlerini kullanarak [sanal ağlar arası bağlantılar](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırabilirsiniz.

Sanal ağlar arası bağlantı için her iki seçenek de yapılandırıldığında, sanal ağlar arasındaki trafik, eşleme yapılandırması (Azure omurgası) üzerinden akış gerçekleştirir.

Aynı bölgedeki sanal ağlar eşlendiğinde, eşlenmiş sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Bu durumda, uzak ağ geçidi kullanan sanal ağın kendi ağ geçidi olamaz. Bir sanal ağın yalnızca bir ağ geçidi olabilir. Ağ geçidi, aşağıdaki resimde gösterildiği gibi yerel veya uzak bir ağ geçidi (eşlenen sanal ağda) olabilir:

![Sanal ağ eşleme geçişi](./media/virtual-networks-peering-overview/figure04.png)

Farklı dağıtım modelleriyle veya farklı bölgelerde oluşturulmuş sanal ağlar arasındaki eşleme ilişkisinde ağ geçidi geçişi desteklenmez. Bir ağ geçidi geçişinin çalışması için eşleme ilişkisindeki her iki sanal ağ da Resource Manager ile ve aynı bölgede oluşturulmuş olmalıdır. Genel olarak eşlenmiş sanal ağlar şu anda ağ geçidi geçişi desteği sunmamaktadır.

Tek bir Azure ExpressRoute bağlantısını kullanan sanal ağlar eşlendiğinde, bu iki sanal ağ arasındaki trafik, eşleme ilişkisi (Azure omurga ağı) üzerinden akış gerçekleştirir. Şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilirsiniz. Alternatif olarak, paylaşılan bir ağ geçidini kullanıp şirket içi bağlantı için bir geçiş yapılandırabilirsiniz.

## <a name="permissions"></a>İzinler

Sanal ağ eşlemesi ayrıcalıklı bir işlemdir. VirtualNetworks ad alanı altında yer alan ayrı bir işlevdir. Bir kullanıcıya eşlemeyi yetkilendirmesi için belirli haklar verilebilir. Sanal ağa yönelik okuma/yazma erişimi olan bir kullanıcı bu haklara otomatik olarak sahip olur.

Eşleme özelliğinin yöneticisi ya da ayrıcalıklı kullanıcısı olan bir kullanıcı, başka bir sanal ağ üzerinde eşleme işlemi başlatabilir. Gerekli minimum izin seviyesi Ağ Katılımcısı olarak belirlenmiştir. Diğer tarafta eşleme için eşleşen bir istek varsa ve diğer gereksinimler karşılanırsa eşleme gerçekleştirilir.

Örneğin myVirtualNetworkA ve myVirtualNetworkB adlı sanal ağları eşliyorsanız hesabınıza her bir sanal ağ için aşağıdaki minimum rol veya izin atanmış olmalıdır:

|Sanal ağ|Dağıtım modeli|Rol|İzinler|
|---|---|---|---|
|myVirtualNetworkA|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Yok|
|myVirtualNetworkB|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

## <a name="monitor"></a>İzleme

Resource Manager ile oluşturulmuş olan iki sanal ağı eşlerken eşlemedeki her sanal ağ için bir eşleme yapılandırılması gerekir. Eşleme bağlantınızın durumunu izleyebilirsiniz. Eşleme durumu aşağıdakilerden biri olabilir:

* **Başlatıldı**: Birinci sanal ağdan ikinci sanal ağa eşleme oluşturduğunuzda gösterilen durum.
* **Bağlı**: İkinci sanal ağdan birinci sanal ağa eşleme oluşturduğunuzda gösterilen durum. Birinci sanal ağın *Başlatıldı* olan eşleme durumu *Bağlı* olarak değişir. İki sanal ağ eşlemesinin de eşleme durumu *Bağlı* olana kadar sanal ağ eşlemesi başarıyla oluşturulmaz.
* **Bağlantı kesildi**: İki sanal ağ arasında eşleme kurulduktan sonra bir sanal ağdan diğerine eşleme silindiğinde gösterilen durum.

## <a name="troubleshoot"></a>Sorun giderme

Bir sanal ağ eşlemesini onaylamak için, bir sanal ağdaki herhangi bir alt ağın ağ arabirimine yönelik [etkili yolları denetleyebilirsiniz](virtual-network-routes-troubleshoot-portal.md). Bir sanal ağ eşlemesi zaten varsa, sanal ağ içindeki tüm alt ağlar, eşlenen her bir sanal ağdaki her bir adres alanı için sonraki atlama türü *VNet eşlemesi* olan yollara sahip olur.

Eşlenmiş sanal ağdaki bir sanal makinenin bağlantı durumuyla ilgili sorunları gidermek için Ağ İzleyicisi'nin [bağlantı denetimini](../network-watcher/network-watcher-connectivity-portal.md) de kullanabilirsiniz. Bağlantı denetimi sayesinde trafiğin bir kaynak sanal makinenin ağ arabiriminden hedef sanal makinenin ağ arabirimine nasıl yönlendirildiğini denetleyebilirsiniz.

## <a name="limits"></a>Sınırlar

Tek bir sanal ağ için izin verilen eşleme sayısı sınırlıdır. Ayrıntılar için bkz. [Azure ağ sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

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
