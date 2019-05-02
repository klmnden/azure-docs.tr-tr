---
title: Oluşturma, değiştirme veya silme bir Azure sanal ağ eşlemesi | Microsoft Docs
description: Oluşturma, değiştirme veya bir sanal ağ eşlemesini Sil öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: anavin
ms.openlocfilehash: 18d913339556c0d4b0a06bd62f4495da6a4d4223
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925912"
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Oluşturma, değiştirme veya bir sanal ağ eşlemesini Sil

Oluşturma, değiştirme veya bir sanal ağ eşlemesini Sil öğrenin. Sanal Ağ eşlemesi (diğer adıyla genel sanal ağ eşleme) bölgede ve aynı bölgedeki sanal ağları bağlamak, Azure omurga ağı aracılığıyla sağlar. Eşlendikten sonra sanal ağ ayrı kaynaklar olarak hala yönetilir. Sanal Ağ eşlemesi için yeni başlıyorsanız daha fazla bilgi edinebilirsiniz [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) veya tamamlayarak bir [öğretici](tutorial-connect-virtual-networks-portal.md).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.com, sahip bir hesap bilgilerinizle oturum [gerekli izinlere](#permissions) eşlemeleri ile çalışmak için.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` olan bir hesapla [gerekli izinleri](#permissions) Azure ile bir bağlantı oluşturmak için eşleme ile çalışmak için.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` olan bir hesapla [gerekli izinleri](#permissions) Azure ile bir bağlantı oluşturmak için eşleme ile çalışmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="create-a-peering"></a>Bir eşleme oluşturma

Bir eşleme oluşturmadan önce gereksinimler ve kısıtlamalar ile hakkında bilgilenmeli ve [gerekli izinleri](#permissions).

1. Azure portalının üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin. Seçmeyin **sanal ağlar (Klasik)** Klasik dağıtım modeliyle dağıtılan sanal ağ eşlemesi oluşturulamıyor gibi listede görünüp görünmediğine.
2. Sanal ağ eşleme oluşturmak istediğiniz listeyi seçin.
3. Altında **ayarları**seçin **eşlemeler**.
4. **+ Ekle** öğesini seçin. 
5. <a name="add-peering"></a>Aşağıdaki ayarları için değerleri seçin veya girin:
    - **Adı:** Eşleme adı, sanal ağ içinde benzersiz olmalıdır.
    - **Sanal ağ dağıtım modeli:** Eşlemek istediğiniz sanal ağı üzerinden dağıtılan hangi dağıtım modelini seçin.
    - **Kaynak Kimliğimi biliyorum:** Eşlemek istediğiniz sanal ağı okuma erişiminiz varsa, bu onay kutusunu işaretlemeden bırakın. Sanal ağ veya eşlemek istediğiniz aboneliği okuma erişimi yoksa, bu kutuyu işaretleyin. İçinde eşlemek istediğiniz sanal ağın tam kaynak Kimliğini girin **kaynak kimliği** kutusu işaretlendiğinde görünen kutusu. Kaynak Kimliği girdiğiniz aynı var olan bir sanal ağ için olmalıdır veya [farklı desteklenen](#requirements-and-constraints) Azure [bölge](https://azure.microsoft.com/regions) bu sanal ağ. Tam kaynak kimliği benzer şekilde görünür `/subscriptions/<Id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>`. Bir sanal ağ için bir sanal ağ özelliklerini görüntüleyerek, kaynak Kimliğini alabilirsiniz. Bir sanal ağ özelliklerini öğrenmek için bkz. [sanal ağlarını yönetme](manage-virtual-network.md#view-virtual-networks-and-settings). Abonelik gelen eşleme oluşturduğunuz sanal ağ ile abonelik değerinden farklı bir Azure Active Directory kiracısı ile ilişkili ise, önce bir kullanıcı her bir kiracı ekleyin bir [Konuk kullanıcı](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) karşı kiracıdaki.
    - **Abonelik:** Seçin [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) eşlemek istediğiniz sanal ağ. Bir veya daha fazla abonelik hesabınızın okuma erişimi kaç aboneliğe sahip bağlı olarak listelenir. İşaretlediyseniz **kaynak kimliği** onay kutusu, bu ayar kullanılamaz.
    - **Sanal ağ:** Eşlemek istediğiniz sanal ağı seçin. İki Azure dağıtım modeliyle oluşturulan bir sanal ağı seçebilirsiniz. Farklı bir bölgede bir sanal ağ seçmek istiyorsanız, bir sanal ağda seçmelisiniz bir [bölge desteklenen](#cross-region). Sanal ağ listede görünür olması için okuma erişimi olmalıdır. Bir sanal ağ listelenir, ancak gri, sanal ağın adres alanı, bu sanal ağın adres alanıyla çakışıyor olabilir. Sanal ağ adres alanları, bir çakışma varsa, bunlar eşlenemez. İşaretlediyseniz **kaynak kimliği** onay kutusu, bu ayar kullanılamaz.
    - **Sanal ağ erişimine izin ver:** Seçin **etkin** iki sanal ağ arasındaki iletişimi etkinleştirmek istiyorsanız (varsayılan). Sanal ağlar arası iletişimin etkinleştirilmesi, aynı sanal ağa bağlıyken olarak birbiriyle aynı bant genişliği ve gecikme süresi ile iletişim kurmak için iki sanal ağ için bağlı kaynaklar sağlar. İki sanal ağ kaynakları arasındaki tüm iletişimi Azure özel ağdır. **VirtualNetwork** eşlenen sanal ağ ve sanal ağın ağ güvenlik grupları için hizmet etiketi kapsar. Ağ güvenlik grubu hizmet etiketleri hakkında daha fazla bilgi için bkz: [ağ güvenlik gruplarına genel bakış](security-overview.md#service-tags). Seçin **devre dışı bırakılmış** eşlenmiş sanal ağa akışına istemiyorsanız. Seçtiğiniz **devre dışı bırakılmış** başka bir sanal ağ ile sanal ağ eşlendikten, ancak bazen iki sanal ağ arasındaki trafik akışını devre dışı bırakmak istiyorsanız. Etkinleştirme/devre dışı bırakma silip yeniden eşlemeler oluşturarak değerinden daha kullanışlı bulabilirsiniz. Bu ayar devre dışı bırakıldığında, trafik eşlenmiş sanal ağlar akış değil.
    - **İletilen trafiğe izin ver:** Trafiğe izin vermek için bu kutuyu *iletilen* (sanal ağdan kaynaklanan olmadı) bir sanal ağda ağ sanal Gereci tarafından bu sanal ağa bir eşlemesi üzerinden flow'a. Örneğin, 1, Spoke2 ve Hub'ı adlı üç sanal ağları göz önünde bulundurun. Her bir uç sanal ağ ile merkez sanal ağ arasında bir eşleme var, ancak bağlı sanal ağlar arasında eşleme yok. Bir ağ sanal Gereci Hub sanal ağında dağıtılan ve kullanıcı tanımlı yollar, ağ sanal Gereci üzerinden alt ağlar arasında trafiği yönlendirebilirsiniz her uç sanal ağ için uygulanır. Her bir uç sanal ağ ile merkez sanal ağ arasında eşleme için bu onay kutusu işaretli değilse, hub sanal ağlar arasındaki trafik yönlendirme değil çünkü trafiği ağlı sanal ağlar akış değil. Bu özellik etkinleştirme eşlemesi üzerinden iletilen trafiğe izin verirken, herhangi bir kullanıcı tanımlı yollar veya ağ sanal Gereçleri oluşturmaz. Kullanıcı tanımlı yollar ve ağ sanal Gereçleri ayrı olarak oluşturulur. Hakkında bilgi edinin [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined). Bir Azure VPN ağ geçidi üzerinden sanal ağlar arasındaki trafiği iletilirse, bu ayarı işaretleyin gerek yoktur.
    - **Ağ geçidi aktarımına izin ver:** Bu sanal ağa bağlı bir sanal ağ geçidi varsa, bu onay kutusunu işaretleyin ve eşlenen sanal ağ geçidinden akış gelen trafiğe izin vermek istiyor. Örneğin, bu sanal ağa bir sanal ağ geçidi üzerinden şirket içi ağa bağlı olabilir. Ağ geçidi bir ExpressRoute veya VPN ağ geçidi olabilir. Bu kutunun işaretlenmesi, bu sanal ağdan şirket içi ağa bağlı ağ geçidi üzerinden akmasını eşlenmiş sanal ağa gelen trafiğe izin verir. Bu kutuyu işaretlerseniz, eşlenen sanal ağ, yapılandırılmış bir ağ geçidi olamaz. Eşlenen sanal ağda bulunması gerekir **uzak ağ geçitlerini kullan** onay kutusunu seçili ayarlarken diğer sanal ağdan bu sanal ağa eşleme ayarlama. Bırakırsanız bu kutu işaretli (varsayılan), eşlenen sanal ağ hala akışlar, bu sanal ağa trafiği, ancak bu sanal ağa bağlı bir sanal ağ geçidi üzerinden geçirilemez. Ağ geçidi, bir sanal ağ (Resource Manager) (Klasik) bir sanal ağ arasında eşleme durumda, sanal ağ (Resource Manager) olması gerekir. Farklı bölgelerdeki sanal ağları eşleme varsa bu seçeneği etkinleştirilemiyor.

       Bir şirket içi ağ trafiği iletmek ek olarak, ağ geçidi, sanal ağlar birbiriyle eşlenmesi gerek olmadan sanal ağ ile eşlenmiş sanal ağlar arasındaki ağ trafiğini VPN ağ geçidi iletebilir. Trafiği iletmek için bir VPN ağ geçidi'ni kullanarak, bir VPN ağ geçidi bir hub'ı kullanmak istediğiniz gerektiğinde kullanışlıdır (için açıklanan merkez ve uç örneğe bakın **iletilen trafiğe izin ver**) olmayan ağlı sanal ağlar arasında trafiği yönlendirmek için sanal ağ birbirleri ile eşlenmiş. Aktarım sırasında bir ağ geçidi kullanımına izin verme hakkında daha fazla bilgi için bkz: [sanal ağ eşlemesi bir VPN ağ geçidi geçişinin yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu senaryoda, sonraki atlama türü olarak sanal ağ geçidini belirten kullanıcı tanımlı yollar uygulanması gerekir. Hakkında bilgi edinin [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined). Kullanıcı tanımlı bir yol, sonraki atlama türü olarak yalnızca bir VPN ağ geçidi belirtebilirsiniz, kullanıcı tanımlı bir yol sonraki atlama türü olarak ExpressRoute ağ geçidi belirtemezsiniz. Farklı bölgelerdeki sanal ağları eşleme varsa bu seçeneği etkinleştirilemiyor.

    - **Uzak ağ geçitlerini kullan:** Bu sanal ağ ile eşlemesi sanal ağa bağlı sanal ağ geçidi aracılığıyla akış için gelen trafiğe izin vermek için bu kutuyu işaretleyin. Örneğin, sanal ağ ile eşlemesini bağlı bir VPN ağ geçidi için bir şirket içi ağ iletişimi sağlayan sahiptir.  Bu kutunun işaretlenmesi, bu sanal ağdan eşlenmiş sanal ağa bağlı VPN ağ geçidi üzerinden akan trafiği sağlar. Bu kutuyu işaretlerseniz, eşlenmiş sanal ağa bağlı bir sanal ağ geçidi olmalıdır ve olmalıdır **ağ geçidi aktarımına izin ver** onay kutusunu işaretli. Bu kutusu işaretlenmemiş (varsayılan), eşlenen sanal ağ arasında trafik yine de bu sanal ağa akış, ancak olamaz bir sanal ağ geçidi üzerinden flow'a iliştirilmiş bu sanal ağ.
    Bu sanal ağ için yalnızca bir eşleme, bu ayar etkin olabilir.

        Sanal ağınızda yapılandırılan bir ağ geçidi zaten varsa, uzak ağ geçitlerini kullanamazsınız. Farklı bölgelerdeki sanal ağları eşleme varsa bu seçeneği etkinleştirilemiyor. Bir ağ geçidi için bir geçiş kullanma hakkında daha fazla bilgi edinmek için [sanal ağ eşlemesi bir VPN ağ geçidi geçişinin yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

6. Seçin **Tamam** seçtiğiniz sanal ağ eşlemesi eklemek için.

Farklı Abonelikteki sanal ağlar ile dağıtım modelleri arasında eşleme uygulamak için adım adım yönergeler için bkz: [sonraki adımlar](#next-steps).

### <a name="commands"></a>Komutlar

- **Azure CLI**: [az ağ vnet eşlemesi oluşturma](/cli/azure/network/vnet/peering)
- **PowerShell**: [AzVirtualNetworkPeering ekleyin](/powershell/module/az.network/add-azvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Eşleme ayarlarını görüntülemek veya değiştirmek

Bir eşleme değiştirmeden önce gereksinimler ve kısıtlamalar ile hakkında bilgilenmeli ve [gerekli izinleri](#permissions).

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin. Seçmeyin **sanal ağlar (Klasik)** Klasik dağıtım modeliyle dağıtılan sanal ağ eşlemesi oluşturulamıyor gibi listede görünüp görünmediğine.
2. Sanal ağ eşleme ayarlarını değiştirmek için istediğiniz listeyi seçin.
3. Altında **ayarları**seçin **eşlemeler**.
4. Eşleme için ayarlarını görüntülemek veya değiştirmek istiyorsanız seçin.
5. Uygun ayarını değiştirin. Her bir ayar için seçenekleri hakkında bilgi edinin [5. adım](#add-peering) bir eşleme oluştur.
6. **Kaydet**’i seçin.

**Komutları**

- **Azure CLI**: [az ağ vnet eşleme listesi](/cli/azure/network/vnet/peering) bir sanal ağ için liste eşlemeleri için [az network vnet eşleme show](/cli/azure/network/vnet/peering) ayarları bir özel eşdüzey hizmet sağlama için gösterilecek ve [az ağ sanal ağ eşleme güncelleştirme](/cli/azure/network/vnet/peering) eşleme ayarları değiştirmek için. |
- **PowerShell**: [Get-AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering) görünümü eşleme ayarları alınamadı ve [kümesi AzVirtualNetworkPeering](/powershell/module/az.network/set-azvirtualnetworkpeering) ayarlarını değiştirmek için.

## <a name="delete-a-peering"></a>Bir eşleme Sil

Bir eşleme silmeden önce hesabınızın sahip olun [gerekli izinleri](#permissions).

Bir eşleme silindiğinde, trafiği bir sanal ağdan eşlenmiş sanal ağa artık akar. Resource Manager üzerinden dağıtılan sanal ağlar eşlendiğinde, her bir sanal ağı diğer sanal ağa eşleme vardır. Bir sanal ağdan eşdüzey hizmet sağlaması siliniyor sanal ağlar arasındaki iletişimi devre dışı bırakır. ancak, diğer sanal ağdan eşleme silmez. Mevcut diğer sanal ağ eşlemesi için eşleme durumu **bağlantısı kesilmiş**. İlk sanal ağ ve iki sanal ağ değişiklikleri için eşleme durumunu eşlemesi, yeniden oluşturma kadar eşlemeyi yeniden oluşturulamaz *bağlı*.

Sanal ağlar, bazen kurmak istiyor, ancak her zaman bir eşdüzey hizmet sağlaması siliniyor yerine ayarlayabilirsiniz **sanal ağ erişimine izin ver** ayarını **devre dışı bırakılmış** yerine. Bilgi edinmek için nasıl bir eşleme bu makalenin 6. adım Oluştur okuyun. Devre dışı bırakma ve etkinleştirme ağ erişimini, silme ve eşlemelerin yeniden daha kolay bulabilirsiniz.

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin. Seçmeyin **sanal ağlar (Klasik)** Klasik dağıtım modeliyle dağıtılan sanal ağ eşlemesi oluşturulamıyor gibi listede görünüp görünmediğine.
2. Sanal ağ eşleme silmek istediğiniz listeyi seçin.
3. Altında **ayarları**seçin **eşlemeler**.
4. Silmek istediğiniz eşlemesi sağdaki, seçin **...** seçin **Sil**, ardından **Evet** ilk sanal ağdan eşlemesini silmek için.
5. Eşlemedeki diğer sanal ağ eşlemesini silmek için önceki adımları tamamlayın.

**Komutları**

- **Azure CLI**: [az ağ vnet eşleme Sil](/cli/azure/network/vnet/peering)
- **PowerShell**: [Remove-AzVirtualNetworkPeering](/powershell/module/az.network/remove-azvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Gereksinimler ve kısıtlamalar

- <a name="cross-region"></a>Aynı bölgede ya da farklı bölgelerdeki sanal ağları eşleyebilirsiniz. Farklı bölgelerdeki sanal ağları eşleme de denir olarak *genel sanal ağ eşleme*. 
- Genel eşleme oluştururken, eşlenen sanal ağlarda tüm Azure genel bulut bölgesi veya Çin bulut bölgeleri veya kamu bulut bölgeleri içinde bulunabilir. Bulutlar arasında eş olamaz. Örneğin, bir sanal ağa Azure Çin Bulutu, Azure genel bulutundaki bir VNet eşlenemez.
- Bir sanal ağ içindeki kaynaklarla genel olarak eşlenmiş sanal ağdaki bir iç temel yük dengeleyicinin ön uç IP adresi ile iletişim kuramıyor. Temel yük dengeleyici desteği yalnızca aynı bölge içinde bulunmaktadır. Standart yük dengeleyici desteği, VNet eşlemesi hem de genel sanal ağ eşleme için var. Küresel VNet eşlemesi çalışmaz temel yük dengeleyici kullanan Hizmetleri belgelenmiştir [burada.](virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)
- Genel olarak eşlenmiş sanal ağlar ve yerel olarak eşlenmiş sanal ağlarda ağ geçidi aktarımına izin ver ya da uzak ağ geçitlerini kullan.
- Sanal ağlar aynı ya da farklı Aboneliklerde olabilir. Farklı Aboneliklerdeki sanal ağları eşleyebilme, aynı veya farklı Azure Active Directory kiracısı ile ilişkilendirilmesi iki abonelik de olabilir. Bir AD kiracısına zaten sahip değilseniz, yapabilecekleriniz [oluşturmak](../active-directory/develop/quickstart-create-new-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json-a-new-azure-ad-tenant). Farklı Azure Active Directory kiracılarıyla ilişkili Aboneliklerdeki sanal ağları arasında eşleme için destek Portalı'nda kullanılabilir değil. CLI, PowerShell veya şablonları kullanabilirsiniz.
- Eş sanal ağlar, IP adresi alanları çakışmamalıdır olması gerekir.
- Adres aralıklarını ekleyin veya başka bir sanal ağ ile sanal ağ eşlendikten sonra sanal ağın adres alanından adres aralıkları silin. Adres aralıkları kaldırın, eşlemeyi silmek, eklediğinizde veya adres aralıklarını kaldırmak için ardından eşleme yeniden oluşturun. Adres aralıklarını ekleyin veya adres aralıkları sanal ağlardan bağlantısını kaldırmak için bkz: [sanal ağlarını yönetme](manage-virtual-network.md).
- Resource Manager veya Klasik dağıtım modeliyle dağıtılan bir sanal ağ ile Resource Manager üzerinden dağıtılan bir sanal ağ aracılığıyla dağıtılan iki sanal ağları eşleyebilirsiniz. Klasik dağıtım modeliyle oluşturulan iki sanal ağı eşleyebilme olamaz. Azure dağıtım modelleri hakkında bilgi sahibi değilseniz, okuma [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi. Klasik dağıtım modeliyle oluşturulan iki sanal ağı bağlamak için [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
- Resource Manager ile oluşturulmuş olan iki sanal ağı eşlerken eşlemedeki her sanal ağ için bir eşleme yapılandırılması gerekir. Eşleme durumu için aşağıdaki türlerinden birini görürsünüz: 
  - *Başlatan:* İlk sanal ağdan ikinci sanal ağa eşleme oluşturduğunuzda eşleme durumu olan *başlatılan*. 
  - *Bağlı:* İkinci sanal ağdan ilk sanal ağa eşleme oluşturduğunuzda eşleme durumu olan *bağlı*. İlk sanal ağ için eşleme durumunu görüntülerseniz, değiştirildi durumu gördüğünüz *başlatılan* için *bağlı*. Her iki sanal ağ eşlemesi için eşleme durumunu olana kadar Eşleme başarıyla oluşturulmuş olmaz *bağlı*.
- Klasik dağıtım modeliyle oluşturulan bir sanal ağ ile Resource Manager aracılığıyla oluşturulan bir sanal ağ eşlemesi, Resource Manager üzerinden dağıtılan sanal ağ eşleme yalnızca yapılandırın. Bir sanal ağ (Klasik) veya Klasik dağıtım modeliyle dağıtılan iki sanal ağ arasında eşleme yapılandıramazsınız. Sanal ağdan (Resource Manager) (Klasik) sanal ağa eşleme oluşturduğunuzda eşleme durumu olan *güncelleştirme*, için kısa bir süre içinde değişiklikleri *bağlı*.
- İki sanal ağ arasında eşleme kuruldu. Eşlemeler geçişli değildir. Eşlemeler arasında oluşturursanız:
  - VirtualNetwork1 & VirtualNetwork2
  - VirtualNetwork2 & VirtualNetwork3

  Hiçbir VirtualNetwork1 ve VirtualNetwork2 aracılığıyla VirtualNetwork3 arasında eşleme yoktur. VirtualNetwork3 VirtualNetwork1 arasında eşleme bir sanal ağ oluşturmak istiyorsanız, bir VirtualNetwork3 VirtualNetwork1 arasında eşleme oluşturma gerekir.
- Varsayılan Azure ad çözümlemesi kullanarak eşlenmiş sanal ağlarda bulunan adları çözümlenemiyor. Diğer sanal ağlara adlarını çözümlemek için kullanmanız gerekir [Azure DNS özel etki alanları için](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya özel bir DNS sunucusu. Kendi DNS sunucunuzu hakkında bilgi edinmek için bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
- Aynı sanal ağda değilmiş gibi aynı bölgedeki eşlenmiş sanal ağlarda bulunan kaynaklar birbiriyle aynı bant genişliği ve gecikme süresi ile iletişim kurabilir. Her sanal makine boyutu, ancak kendi maksimum ağ bant genişliği sahiptir. Farklı sanal makine boyutu için maksimum ağ bant genişliği hakkında daha fazla bilgi için bkz: [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları.
- Bir sanal ağ, başka bir sanal ağa eşlenebilir ve bir Azure sanal ağ geçidi ile başka bir sanal ağa bağlanması. Sanal ağları eşleme ve bir ağ geçidi bağlandığında, sanal ağlar arasındaki trafiği gateway yerine eşleme yapılandırması akar.
- Sanal Ağ eşlemesi başarıyla yeni yollar istemciye indirilen emin olmak için yapılandırıldıktan sonra noktadan siteye VPN istemcileri yeniden yüklenmesi gerekir.
- Sanal ağ eşlemesi kullanan girdi ve çıktı trafiği için nominal bir ücret uygulanır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="permissions"></a>İzinler

Sanal Ağ eşlemesi ile çalışmak için kullandığınız hesapları aşağıdaki rol atanması gerekir:

- [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor): Resource Manager üzerinden dağıtılan bir sanal ağ için.
- [Klasik ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor): Klasik dağıtım modeliyle dağıtılan bir sanal ağ için.

Hesabınızı önceki rollerden biri atanmamışsa, atanmalıdır bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) gerekli eylemleri aşağıdaki tablodan atanır:

| Eylem                                                          | Ad |
|---                                                              |---   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write  | Bir sanal ağdan sanal ağa b sanal eşleme oluşturmak için gereken ağ bir sanal ağ (Resource Manager) olmalıdır          |
| Microsoft.Network/virtualNetworks/peer/action                   | Bir sanal ağ B (Resource Manager) bir sanal ağ eşlemesi oluşturmak için gerekli                                                       |
| Microsoft.ClassicNetwork/virtualNetworks/peer                   | Bir sanal ağ B (Klasik) bir sanal ağ eşlemesi oluşturmak için gerekli                                                                |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read   | Bir sanal ağ eşlemesi okuyun   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Bir sanal ağ eşlemesini Sil |

## <a name="next-steps"></a>Sonraki adımlar

- Aynı veya farklı aboneliklerde, aynı veya farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında, bir sanal ağ eşlemesi oluşturulur. Aşağıdaki senaryolardan biri için öğreticiyi tamamlayın:

  |Azure dağıtım modeli             | Abonelik  |
  |---------                          |---------|
  |Her ikisi de Resource Manager              |[Aynı](tutorial-connect-virtual-networks-portal.md)|
  |                                   |[Farklı](create-peering-different-subscriptions.md)|
  |Biri Resource Manager, diğeri klasik  |[Aynı](create-peering-different-deployment-models.md)|
  |                                   |[Farklı](create-peering-different-deployment-models-subscriptions.md)|

- [Merkez ve uç ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmayı öğrenin
- Kullanarak bir sanal ağ eşlemesi oluşturma [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için
