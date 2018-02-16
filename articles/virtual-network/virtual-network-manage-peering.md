---
title: "Oluşturma, değiştirme veya bir Azure sanal ağ eşlemesi silme | Microsoft Docs"
description: "Oluşturma, değiştirme veya bir sanal ağ eşlemesi silme öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial;anavin
ms.openlocfilehash: 0dc19abcf010f5c0bc50534941e7b80a9665869e
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Oluşturma, değiştirme veya bir sanal ağ eşlemesi silme

Oluşturma, değiştirme veya bir sanal ağ eşlemesi silme öğrenin. Sanal Ağ eşlemesi, sanal ağlar Azure omurga ağ üzerinden bağlanmasına olanak sağlar. Eşlendikten sonra sanal ağlar hala ayrı kaynaklar olarak yönetilir. Sanal Ağ eşlemesi ile bilmiyorsanız okuma öneririz [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) tamamlayarak [bir sanal ağ eşleme öğretici oluşturma](virtual-network-create-peering.md), bu makaledeki görevleri tamamlamadan önce.

Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Sanal ağlar farklı bölgelerde şu anda önizlemede eşleme. Bkz: [sanal ağı güncelleştirmelerini](https://azure.microsoft.com/en-us/updates/?product=virtual-network) bölgeleri için kullanılabilir. Yapmanız gerekenler [aboneliğiniz Önizleme için kaydetmek](virtual-network-create-peering.md).

> [!WARNING]
> Bu senaryoda oluşturulan sanal ağ eşlemeleri genel kullanım sürümünde mevcut olan senaryolarla aynı kullanılabilirlik ve güvenilirlik seviyesine sahip değildir. Sanal ağ eşlemeleri sınırlı özelliklere sahip olabilir ve tüm Azure bölgelerinde kullanılamayabilir. Bu özelliğin kullanılabilirliği ve durumuyla ilgili en güncel bildirimler için, [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasına bakın.
>

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, https://portal.azure.com açın ve Azure hesabınızla oturum açın.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.2.0 gerektirir veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.26 gerektirir veya sonraki bir sürümü. Çalıştırma `az --version` yüklü olan sürümü bulunamıyor. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="create-a-peering"></a>Bir eşleme oluşturma

>[!NOTE]
>Bir eşleme oluşturmadan önce familiarized kendinizle olun [gereksinimleri ve kısıtlamaları](#requirements-and-constraints) ve [gerekli izinleri](#permissions).
>

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür. Seçmeyin **sanal ağları (Klasik)** Klasik dağıtım modeli aracılığıyla dağıtılan bir sanal ağ eşlemesi oluşturulamıyor gibi listesinde görünüp görünmeyeceğini.
2. Sanal ağ için bir eşleme oluşturmak istediğiniz listesinden seçin.
3. Sanal ağlar listesinden için eşliği oluşturmak istediğiniz sanal ağı seçin.
4. Altında **ayarları**seçin **eşlemeler**.
5. Seçin **+ Ekle**. 
6. <a name="add-peering"></a>Girin veya aşağıdaki ayarları için değerleri seçin:
    - **Ad:** eşleme için adı sanal ağ içinde benzersiz olmalıdır.
    - **Sanal ağ dağıtım modeli:** hangi dağıtım modeli ile eş istediğiniz sanal ağı seçin aracılığıyla dağıtıldı.
    - **Kaynak Kimliğimi biliyorum:** ile eş istediğiniz sanal ağına yönelik okuma erişimi varsa, bu onay kutusunun işaretini kaldırın. Sanal ağ veya ile eş istediğiniz aboneliği okuma erişimi yoksa, bu kutuyu işaretleyin. Sanal ağ içinde eşe istediğiniz tam kaynak Kimliğini girin **kaynak kimliği** kutusu işaretlendiğinde, görünen kutusu. Girdiğiniz kaynak kimliği için aynı Azure mevcut bir sanal ağ olmalıdır [bölge](https://azure.microsoft.com/regions) bu sanal ağ olarak. Farklı bir bölgede bir sanal ağ seçmek istiyorsanız [Önizleme aboneliğinizin kaydedin.](virtual-network-create-peering.md) Tam kaynak kimliği için /subscriptions/ benzer<Id>/resourceGroups/ < kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/ < sanal ağ-adı >. Bir sanal ağ özelliklerini görüntüleyerek kaynak kimliği için bir sanal ağ elde edebilirsiniz. Bir sanal ağ özelliklerini görüntüleme hakkında bilgi edinmek için [sanal ağlarını yönetmeleri](virtual-network-manage-network.md#view-vnet).
    - **Abonelik:** seçin [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ile eş istediğiniz sanal ağ. Kaç tane abonelikleri hesabınızın okuma erişimine sahip bağlı olarak bir veya daha fazla abonelik listelenir. İşaretlendiğinde, **kaynak kimliği** onay kutusu, bu ayar kullanılamaz.
    - **Sanal ağ:** ile eş sanal ağı seçin. Ya da Azure dağıtım modeliyle oluşturulan bir sanal ağ seçebilirsiniz. Farklı bir bölgede bir sanal ağ seçmek istiyorsanız [Önizleme aboneliğinizin kaydedin.](virtual-network-create-peering.md) Sanal ağ listesinde görünür olması için okuma erişimi olmalıdır. Bir sanal ağ listelenmiş, ancak gri, sanal ağın adres alanı bu sanal ağın adres alanı ile çakıştığından olabilir. Sanal ağ adres alanları, çakışma varsa, bunlar eşlenen olamaz. İşaretlendiğinde, **kaynak kimliği** onay kutusu, bu ayar kullanılamaz.
    - **Sanal ağ erişimine izin ver:** seçin **etkin** (iki sanal ağlar arasındaki iletişimi etkinleştirmek istiyorsanız, varsayılan). Sanal ağlar arasındaki iletişimi etkinleştirmek aynı sanal ağa bağlıymış gibi aynı bant genişliği ve gecikme birbirleriyle iletişim ya da sanal ağa bağlı kaynaklar sağlar. İki sanal ağlarda bulunan kaynaklar arasındaki tüm iletişimi Azure özel ağdır. **VirtualNetwork** eşlenmiş sanal ağ ve sanal ağın ağ güvenlik grupları için varsayılan etiket kapsar. Ağ güvenlik grubu varsayılan etiketleri hakkında daha fazla bilgi için okuma [ağ güvenlik gruplarını genel bakış](virtual-networks-nsg.md#default-tags) makalesi.  Seçin **devre dışı** eşlenmiş sanal ağa akışına istemiyorsanız. Seçtiğiniz **devre dışı** başka bir sanal ağ ile sanal ağ eşlenen ancak bazen iki sanal ağlar arasındaki trafik akışını devre dışı bırakmak istiyorsanız. Etkinleştirme/devre dışı bırakma silinmesi ve yeniden eşlemeleri oluşturma daha rahat bulabilirsiniz. Bu ayarı devre dışı bırakıldığında, eşlenmiş sanal ağlar arasında trafiği akışı değil.
    - **İletilen trafiğe izin:** trafiğine izin vermek için bu kutuyu işaretleyin *iletilen* (sanal ağdan kaynaklanan olmadı) bir sanal ağ içinde ağ sanal gereç olarak bir eşleme aracılığıyla bu sanal ağa akışı . Örneğin, üç sanal ağlar Spoke1, Spoke2 ve Hub adlı göz önünde bulundurun. Her bağlı sanal ağ ve Hub sanal ağ arasında bir eşleme var, ancak bağlı sanal ağlar arasında eşlemeler mevcut değil. Ağ sanal gereç Hub sanal ağda dağıtılmış ve kullanıcı tanımlı yollar ağ sanal gereç aracılığıyla alt ağlar arasında trafiği yönlendirmek her spoke sanal ağ için uygulanır. Her bağlı sanal ağ ve hub sanal ağ arasında eşleme için bu onay kutusu işaretli değilse, hub sanal ağlar arasında trafiği yönlendirme çünkü trafiği spoke sanal ağlar arasında akış değil. Bu özellik etkinleştirme eşleme aracılığıyla iletilen trafiği sağlar, ancak herhangi bir kullanıcı tanımlı yollar veya ağ sanal Gereçleri oluşturmaz. Kullanıcı tanımlı yollar ve ağ sanal Gereçleri ayrı olarak oluşturulur. Hakkında bilgi edinin [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined). Bu ayar bir Azure VPN ağ geçidi üzerinden sanal ağlar arasında trafiği iletilirse denetlemeniz gerekmez.
    - **Ağ geçidi transit izin ver:** bu sanal ağa bağlı bir sanal ağ geçidi varsa, bu onay kutusunu işaretleyin ve ağ geçidi üzerinden akmasını eşlenen sanal ağ trafiğinden izin vermek istiyor. Örneğin, bu sanal ağ bir şirket içi ağınıza bir sanal ağ geçidi aracılığıyla bağlı. Ağ geçidi, bir ExpressRoute veya VPN ağ geçidi olabilir. Bu kutuyu işaretleyerek bu sanal ağ şirket ağına bağlı ağ geçidi üzerinden akmasını eşlenen sanal ağ gelen trafiğe izin verir. Bu kutuyu işaretleyin, eşlenmiş sanal ağ yapılandırılmış bir ağ geçidi sahip olamaz. Eşlenen sanal ağın olmalıdır **uzak ağ geçitlerini kullanan** onay kutusu işaretli diğer sanal ağdan bu sanal ağ eşleme ayarlama ayarlarken. Bırakır Bu kutu işaretli (varsayılan), bu sanal ağa eşlenmiş sanal ağ hala akış yaptığı trafiği, ancak bu sanal ağa bağlı bir sanal ağ geçidi üzerinden geçirilemez. 
    
    Bir şirket içi ağ trafiğini iletme ek olarak, bir VPN ağ geçidi ağ geçidi, sanal ağlar birbirleri ile eşlenen gerek olmadan sanal ağ ile eşlenen sanal ağlar arasında ağ trafiğini iletebilirsiniz. Bir hub bir VPN ağ geçidi kullanmak istediğinizde bu kullanışlıdır (için açıklanan hub ve bağlı bileşen örneğe bakın **iletilen trafiğe izin ver**) birbirleri ile eşlenen olmayan spoke sanal ağlar arasında trafiği yönlendirmek için sanal ağ. Daha fazla bilgi edinmek [sanal ağ geçitlerini](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). Bu senaryoda, sanal ağ geçidi sonraki atlama türünü belirtin, kullanıcı tanımlı yollar uygulama gerekir. Hakkında bilgi edinin [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined). Bir sonraki atlama türü kullanıcı tanımlı bir yol olarak yalnızca bir VPN ağ geçidi belirtebilirsiniz, bir ExpressRoute ağ geçidi sonraki atlama türü kullanıcı tanımlı bir yol olarak belirtilemez.

        You cannot enable this option if you're peering a virtual network (Resource Manager) with a virtual network (classic). Though the traffic flows between the two virtual networks, the virtual network (classic) traffic cannot flow through a network gateway attached to the virtual network (Resource Manager). 

    - **Uzak ağ geçitleri kullanın:** eşliği ile sanal ağa bağlı bir sanal ağ geçidi akışına bu sanal ağ trafiğine izin vermek için bu onay kutusunu işaretleyin. Örneğin, ile eşliği sanal ağ bağlı bir VPN ağ geçidi için bir şirket içi ağ iletişimi sağlayan sahiptir.  Bu kutuyu işaretleyerek bu sanal ağa eşlenmiş sanal ağa bağlı VPN ağ geçidi üzerinden akış gelen trafiğe izin verir. Bu kutuyu işaretleyin, eşlenmiş sanal ağ bağlı bir sanal ağ geçidi olmalıdır ve olmalıdır **ağ geçidi transit izin** onay kutusu işaretli. Bu kutu işaretli (varsayılan) bırakın, eşlenmiş sanal ağ trafiğinden hala bu sanal ağa akış, ancak olamaz bir sanal ağ geçidi üzerinden akmasını iliştirilmiş bu sanal ağ. 
Bu sanal ağ için yalnızca bir eşleme Bu ayar etkin olabilir.
Sanal ağınızda yapılandırılmış bir ağ geçidi zaten varsa bu ayarı kullanamazsınız.
        (Resource Manager) bir sanal ağ (Klasik) ile bir sanal ağ eşlemesi varsa bu seçeneği etkinleştirilemiyor. İki sanal ağlar arasında trafiği akan de, sanal ağ (Resource Manager) trafiği (Klasik) sanal ağa bağlı bir ağ geçidi üzerinden geçirilemez.

7. Tıklatın **Tamam** seçtiğiniz sanal ağ alt ağı eklemek için düğmesi.

### <a name="commands"></a>Komutlar

- Azure CLI: [az ağ vnet eşlemesi oluşturma](/cli/azure/network/vnet/peering#create)
- PowerShell: [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering)

### <a name="scenarios"></a>Senaryolar

Aynı veya farklı aboneliklerde, aynı veya farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında, bir sanal ağ eşlemesi oluşturulur. Aşağıdaki senaryolardan biri için adım adım öğretici tamamlayın:
 
|Azure dağıtım modeli  | Abonelik  |
|---------|---------|
|Her ikisi de Resource Manager |[Aynı](virtual-network-create-peering.md)|
| |[Farklı](create-peering-different-subscriptions.md)|
|Biri Resource Manager, diğeri klasik     |[Aynı](create-peering-different-deployment-models.md)|
| |[Farklı](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Eşleme ayarlarını görüntülemek veya değiştirmek

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür. Seçmeyin **sanal ağları (Klasik)** Klasik dağıtım modeli aracılığıyla dağıtılan bir sanal ağ eşlemesi oluşturulamıyor gibi listesinde görünüp görünmeyeceğini.
2. Sanal ağ eşleme ayarlarını değiştirmek için istediğiniz listesinden seçin.
3. Sanal ağlar listesinden eşleme ayarlarını değiştirmek için istediğiniz sanal ağı seçin.
4. Altında **ayarları**seçin **eşlemeler**.
5. Görüntülemek veya ayarlarını değiştirmek istediğiniz eşleme'yi tıklatın.
6. Uygun ayarını değiştirin. Her ayar için seçenekleri hakkında bilgi [adım 6](#add-peering) bir eşleme oluştur. 

    >[!NOTE]
    >Bir eşleme oluşturmadan önce familiarized kendinizle olun [gereksinimleri ve kısıtlamaları](#requirements-and-constraints) ve [gerekli izinleri](#permissions).
    >

7. **Kaydet**’e tıklayın.

**Komutları**

Azure CLI: [az ağ vnet eşleme listesi](/cli/azure/network/vnet/peering#az_network_vnet_peering_list) sanal bir ağ için liste eşlemeleri için [az ağ vnet eşleme Göster](/cli/azure/network/vnet/peering#az_network_vnet_peering_show) bir özel eşliği için ayarları göstermek için ve [az ağ vnet eşleme güncelleştirmesi ](/cli/azure/network/vnet/peering#az_network_vnet_peering_update) eşleme ayarlarını değiştirmek için. |
- PowerShell: [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering) to retrieve view peering settings and [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering) to change settings.

## <a name="delete-a-peering"></a>Bir eşleme Sil

Bir eşleme silindiğinde, bir sanal ağ trafiğinden artık eşlenmiş sanal ağa akar. Resource Manager aracılığıyla dağıtılan sanal ağlar eşlendikleri, her sanal ağ bir diğer sanal ağ eşlemesi vardır. Bir sanal ağdan eşliği silme sanal ağlar arasındaki iletişimi devre dışı bırakır. ancak, diğer sanal ağdan eşliği silmez. Diğer sanal ağda mevcut eşleme durumu eşleme için **bağlantı kesildi**. İlk sanal ağ ve iki sanal ağ değişiklikleri eşleme durumunu eşliği yeniden oluşturduğunuz kadar eşliği yeniden oluşturulamaz *bağlı*. 

Bazen iletişim kurmak için sanal ağlar istiyor, ancak her zaman, bir eşleme silme yerine ayarlayabileceğiniz **sanal ağ erişimine izin ver** ayarını **devre dışı** yerine. Bilgi edinmek için nasıl, 6. adımını okuma [bir eşlemesi oluşturmak](#create-peering) bu makalenin. Devre dışı bırakma ve ağ erişimi silme ve eşlemeler yeniden daha kolay bulabilirsiniz.

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür. Seçmeyin **sanal ağları (Klasik)** Klasik dağıtım modeli aracılığıyla dağıtılan bir sanal ağ eşlemesi oluşturulamıyor gibi listesinde görünüp görünmeyeceğini.
2. Sanal ağ için eşlemesini silmek istediğiniz listesinden seçin.
3. Sanal ağlar listesinden için eşlemesini silmek istediğiniz sanal ağı seçin.
4. Altında **ayarları**seçin **eşlemeler**.
5. Silmek istediğiniz eşliği, sağdaki, seçin **...** seçin **silmek**seçeneğini belirleyip **Evet** ilk sanal ağdan eşlemesini silmek için.
6. Eşlemesi içindeki diğer sanal ağ eşlemesini silmek için önceki adımları tamamlayın.

**Komutları**

- Azure CLI: [az ağ vnet eşleme Sil](/cli/azure/network/vnet/peering#az_network_vnet_peering_delete)
- PowerShell: [Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Gereksinimleri ve kısıtlamaları 

- Eş sanal ağlar, IP adresi alanları çakışmayan olması gerekir.
- Adres aralıklarına ekleyemez veya başka bir sanal ağ ile sanal ağ eşlendikten sonra bir sanal ağın adres alanından adres aralıklarını silin. Adres aralıklarını kaldırmak, eşlemesini silmek, eklediğinizde veya adres aralıklarını kaldırmak için ardından eşliği yeniden oluşturun. Adres aralıklarını ekleyin ya da sanal ağlardan adres aralıklarını kaldırmak için bkz: [sanal ağlarını yönetmeleri](virtual-network-manage-network.md).
- Resource Manager veya Klasik dağıtım modeli aracılığıyla dağıtılan bir sanal ağ ile Resource Manager aracılığıyla dağıtılan bir sanal ağ üzerinden dağıtılan iki sanal ağ eş. Klasik dağıtım modeliyle oluşturulan iki sanal ağ eş olamaz. Azure dağıtım modelleri bilmiyorsanız okuma [anlamak Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi. Klasik dağıtım modeliyle oluşturulan iki sanal ağı bağlamak için [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
- Resource Manager ile oluşturulmuş olan iki sanal ağı eşlerken eşlemedeki her sanal ağ için bir eşleme yapılandırılması gerekir. 
    - *Başlatılan:* ilk sanal ağdan ikinci sanal ağ eşlemesi oluşturduğunuzda, eşleme durumudur *başlatılan*. 
    - *Bağlı:* ikinci sanal ağla olan ilk sanal ağ eşlemesi oluşturduğunuzda, eşleme durumundadır *bağlı*. İlk sanal ağ eşleme durumunu görüntülediğinizde, değiştirildi durumu görürsünüz *başlatılan* için *bağlı*. Her iki sanal ağ eşlemesi bulunabilir eşleme durumu kadar eşlemeyi başarıyla kurulmaz *bağlı*.
- Resource Manager aracılığıyla Klasik dağıtım modeliyle oluşturulan bir sanal ağ ile oluşturulan bir sanal ağ eşlemesi olduğunda, yalnızca bir Resource Manager aracılığıyla dağıtılan sanal ağ eşlemesini yapılandırın. Bir sanal ağ (Klasik) için ya da Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağlar arasında eşleme yapılandıramazsınız. Sanal ağdan (Resource Manager) sanal ağ (Klasik) eşlemesi oluşturduğunuzda, eşleme durumudur *güncelleştirme*, için kısa bir süre içinde değişiklikler *bağlı*.
- Bir eşleme iki sanal ağ arasında oluşturulur. Eşlemeler geçişli değildir. Arasındaki eşlemeler oluşturursanız:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Hiçbir VirtualNetwork1 ve VirtualNetwork3 VirtualNetwork2 aracılığıyla arasında eşleme yoktur. VirtualNetwork1 ve VirtualNetwork3 arasında eşleme sanal ağ oluşturmak istiyorsanız, bir VirtualNetwork1 ve VirtualNetwork3 arasında eşleme oluşturmanız gerekir.
- Varsayılan Azure ad çözümlemesi kullanarak eşlenen sanal ağları adlarında çözümlenemiyor. Diğer sanal ağlar adları çözümlemek için özel bir DNS sunucusu kullanmanız gerekir. Kendi DNS sunucusunu ayarlama hakkında bilgi edinmek için okuma [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) makalesi.
- Aynı sanal ağda değilmiş gibi eşlemesindeki her iki sanal ağ kaynaklarında birbirleri ile aynı bant genişliği ve gecikme süresi ile iletişim kurabilir. Her sanal makine boyutu ancak kendi maksimum ağ bant genişliği vardır. Farklı sanal makine boyutlarına yönelik ağ bant genişliği üst sınırları hakkında daha fazla bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları makalelerini okuyun.
- Aynı veya farklı Aboneliklerde bulunan sanal ağlar Resource Manager aracılığıyla dağıtılan eş.
- Sanal ağlar aynı ya da farklı Aboneliklerde bulunan farklı dağıtım modeli aracılığıyla dağıtılan eş. 
- Her iki sanal ağlar aboneliklerin aynı Azure Active Directory Kiracı ilişkilendirilmiş olması gerekir. Bir AD kiracısıyla zaten sahip değilseniz, hızlı bir şekilde yapabilecekleriniz [oluşturmak](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Kullanabileceğiniz bir [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) farklı Active Directory kiracılar ilişkilendirilen farklı Aboneliklerde bulunan iki sanal ağlara bağlanma.
- Bir sanal ağ, başka bir sanal ağa eşlenen ve ayrıca bir Azure sanal ağ geçidi ile başka bir sanal ağa bağlı olması. Sanal ağlar eşliği ve ağ geçidi bağlandığında, sanal ağlar arasında trafiği ağ geçidi yerine eşleme yapılandırmasını akar.
- Sanal ağ eşlemesi kullanan girdi ve çıktı trafiği için nominal bir ücret uygulanır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="permissions"></a>İzinler

Bir sanal ağ eşlemesi oluşturmak için kullandığınız hesaplarının gerekli rol veya izinleri olması gerekir. Örneğin, myVnetA ve myVnetB adlı iki sanal ağ eşlemesi, hesabınızı aşağıdaki en düşük rolü veya her sanal ağ izinlerini atanmalıdır:
    
|Sanal ağ|Dağıtım modeli|Rol|İzinler|
|---|---|---|---|
|myVnetA|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Yok|
|myVnetB|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Daha fazla bilgi edinmek [yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ve belirli izinler atama [özel roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (yalnızca Resource Manager).

## <a name="next-steps"></a>Sonraki adımlar

[Merkez ve uç ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) oluşturmayı öğrenin
