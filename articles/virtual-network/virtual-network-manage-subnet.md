---
title: Ekleme, değiştirme veya bir Azure sanal ağ alt ağını silmek | Microsoft Docs
description: Ekleme, değiştirme veya azure'da bir sanal ağ alt silme öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 68d4c54b2648dc3b40e69dcde9828d18de318796
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Ekleme, değiştirme veya bir sanal ağ alt ağı silme

Ekleme, değiştirme veya bir sanal ağ alt silme öğrenin. Bir sanal ağ içinde dağıtılan tüm Azure kaynaklarını bir sanal ağ içindeki bir alt ağ içinde dağıtılır. Sanal ağlar yeniyseniz, bunları hakkında daha fazla bilgiyi [sanal ağa genel bakış](virtual-networks-overview.md) veya tamamlayarak bir [Öğreticisi](quick-create-portal.md). Oluşturmak için değiştirmek veya bir sanal ağı silmek, bkz: [sanal ağ yönetme](manage-virtual-network.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.7.0 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.31 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

Hesap oturum açın veya ile azure'a bağlanmak için atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünecek, onu seçin.
2. Sanal ağlar listesinden bir alt ağa eklemek istediğiniz sanal ağı seçin.
3. **AYARLAR** altında **Alt ağlar**’ı seçin.
4. Seçin **+ alt**.
5. Aşağıdaki parametreler için değerler girin:
    - **Ad**: ad sanal ağ içinde benzersiz olmalıdır.
    - **Adres aralığı**: aralık sanal ağın adres alanı içinde benzersiz olmalıdır. Aralık, diğer sanal ağ içinde alt ağ adres aralığı ile örtüşemez. Adres alanı, sınıfsız etki alanları arası yönlendirme (CIDR) gösterimini kullanarak belirtilmelidir. Örneğin, bir sanal ağda adres alanı 10.0.0.0/16, 10.0.0.0/24 alt ağ adres alanının tanımlayabilirsiniz. Belirleyebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralıktır. Azure her alt ağ protokolü uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri Azure hizmetinin kullanım için ayrılmıştır. Sonuç olarak, bir alt ağ/29 ile tanımlama adres alt ağda üç kullanılabilir IP adresleri aralığı sonuçlanır. Bir sanal ağ VPN ağ geçidi bağlanmak istiyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları için belirli bir adresi aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Alt ağ, belirli koşullar altında ekledikten sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adresi aralığını değiştirmek konusunda bilgi almak için bkz: [alt ağ ayarlarını değiştirme](#change-subnet-settings).
    - **Ağ güvenlik grubu**: sıfır veya bir alt ağ için gelen ve giden ağ trafiğini filtrelemek için bir mevcut ağ güvenlik grubunun bir alt ağa ilişkilendirebilirsiniz. Ağ güvenlik grubu aynı abonelikte ve konumda sanal ağ mevcut olmalıdır. Daha fazla bilgi edinmek [ağ güvenlik grubu](security-overview.md) ve [bir ağ güvenlik grubu oluşturmak nasıl](tutorial-filter-network-traffic.md).
    - **Yol tablosu**: ağ trafiği diğer ağlara yönlendirme denetlemek için bir alt ağa sıfır veya bir varolan yol tablosu ilişkilendirebilirsiniz. Yol tablosu, aynı abonelikte ve konumda sanal ağ içinde bulunmalıdır. Daha fazla bilgi edinmek [Azure yönlendirme](virtual-networks-udr-overview.md) ve [bir yol tablosu oluşturma](tutorial-create-route-table-portal.md)
    - **Hizmet uç noktaları:** bir alt ağ için etkin sıfır veya birden çok hizmet uç noktalarına sahip olabilir. Bir hizmet için bir hizmet uç noktası etkinleştirmek için hizmet veya hizmet uç noktalarından etkinleştirmek istediğiniz hizmetleri seçin **Hizmetleri** listesi. Konumu için bir uç nokta otomatik olarak yapılandırılır. Varsayılan olarak, hizmet uç noktaları sanal ağın bölge için yapılandırılır. Azure Storage için bölgesel yük devretme senaryoları desteklemek için uç noktaları otomatik olarak için yapılandırılmış olan [Azure bölgeleri eşleştirilmiş](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

    Hizmet uç noktası kaldırmak için hizmet uç noktası için kaldırmak istediğiniz hizmet seçimini kaldırın. Hizmet uç noktaları ve bunlar etkinleştirilebilir için hizmetleri hakkında daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları genel bakış](virtual-network-service-endpoints-overview.md). Hizmet uç noktası bir hizmet için etkinleştirdikten sonra hizmet ile oluşturulan bir kaynak için alt ağ için ağ erişimini etkinleştirmeniz gerekir. Örneğin, hizmet uç noktası için etkinleştirirseniz *Microsoft.Storage*, ağ erişimi vermek istediğiniz tüm Azure depolama hesapları ağ erişimini etkinleştirmeniz gerekir. Hizmet uç noktası için etkin bir alt ağ erişiminin nasıl etkinleştirileceği hakkında daha fazla ayrıntı için hizmet uç noktası için etkin bireysel hizmet belgelerine bakın.

    Hizmet uç noktası için bir alt ağ etkin olduğunu doğrulamak için görüntülemek [etkili yolları](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-virtual-machine) alt ağdaki herhangi bir ağ arabirimi için. Bir uç nokta yapılandırıldığında, gördüğünüz bir *varsayılan* hizmet adres öneklerini ve nextHopType, rotası **VirtualNetworkServiceEndpoint**. Yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
6. Seçtiğiniz sanal ağ alt ağı eklemek için seçin **Tamam**.

**Komutları**

- Azure CLI: [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [ekleme AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>Alt ağ ayarlarını değiştirme

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünecek, onu seçin.
2. Sanal ağlar listesinden ayarlarını değiştirmek için istediğiniz alt ağ içeren sanal ağı seçin.
3. **AYARLAR** altında **Alt ağlar**’ı seçin.
4. Alt ağlar listesinde ayarlarını değiştirmek için istediğiniz alt ağ seçin. Aşağıdaki ayarları değiştirebilirsiniz:

    - **Adres aralığı:** hiçbir kaynak alt ağ içinde dağıtılırsa, adres aralığını değiştirebilirsiniz. Herhangi bir kaynağa alt ağda varsa, başka bir alt ağa kaynakları taşımak gerekir veya önce alt ağdan silin. Taşımak veya bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Taşımak veya alt kaynaklarını silmek öğrenmek için taşımak veya silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Kısıtlamalarını bkz **adres aralığı** adım 5 [bir alt ağ Ekle](#add-a-subnet).
    - **Kullanıcıların**: yerleşik roller veya kendi özel roller kullanarak alt ağa erişimi denetleyebilirsiniz. Rol ve alt ağ erişmek için kullanıcı atama hakkında daha fazla bilgi için bkz: [Azure kaynaklarınıza erişimi yönetmek için rol ataması kullanın](../role-based-access-control/role-assignments-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
    - **Ağ güvenlik grubu** ve **yol tablosu**: bkz: adım 5 / [bir alt ağ Ekle](#add-a-subnet).
    - **Hizmet uç noktaları**: hizmet uç noktaları adım 5'te bkz [bir alt ağ Ekle](#add-a-subnet). Var olan bir alt ağ için bir hizmet uç noktası etkinleştirilirken hiçbir kritik görev alt ağdaki herhangi bir kaynak üzerinde çalıştığından emin olun. Hizmet uç noktaları olan varsayılan yol kullanarak yolların her alt ağ arabirimindeki geçiş *0.0.0.0/0* adres öneki ve sonraki atlama türü *Internet*, yeni bir rota ile kullanmak için adres öneklerini hizmeti ve bir sonraki atlama türü *VirtualNetworkServiceEndpoint*. Geçiş sırasında herhangi bir açık TCP bağlantısı sonlandırıldı. Trafik akışı için tüm ağ arabirimleri hizmetini yeni bir rota ile güncelleştirilinceye kadar hizmet uç noktası etkin değil. Yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
5. **Kaydet**’i seçin.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)
- PowerShell: [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Bir alt ağı silme

Alt ağ içindeki kaynak varsa, bir alt ağ silebilirsiniz. Alt ağ içindeki kaynaklar varsa, alt ağı silmeden önce alt ağdaki kaynakları silmeniz gerekir. Bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Alt kaynaklarını silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun.

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünecek, onu seçin.
2. Sanal ağlar listesinden silmek istediğiniz alt ağ içeren sanal ağı seçin.
3. **AYARLAR** altında **Alt ağlar**’ı seçin.
4. Alt ağlar listesinde seçin **...** , sağ, alt ağ için silmek istediğiniz
5. Seçin **silmek**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ vnet Sil](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-delete)
- PowerShell: [AzureRmVirtualNetworkSubnetConfig Kaldır](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>İzinler

Alt ağlardaki görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

|Eylem                                                                   |   Ad                                       |
|-----------------------------------------------------------------------  |   -----------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Bir sanal ağ alt okuma              |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Bir sanal ağ alt güncelle  |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Bir sanal ağ alt ağı silme            |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Sanal bir ağa                     |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Bir alt ağ için bir hizmet uç noktası etkinleştirme     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Bir alt ağda sanal makinelerini Al       |

## <a name="next-steps"></a>Sonraki adımlar

- Bir sanal ağ ve alt ağlarını kullanarak oluşturma [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure ilke](policy-samples.md) sanal ağlar için