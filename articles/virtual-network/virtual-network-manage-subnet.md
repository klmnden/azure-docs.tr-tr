---
title: "Ekleme, değiştirme veya bir Azure sanal ağ alt ağını silmek | Microsoft Docs"
description: "Ekleme, değiştirme veya azure'da bir sanal ağ alt silme öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 27918e1d0b335613ea578a815fb3ae00df73ebaa
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Ekleme, değiştirme veya bir sanal ağ alt ağı silme

Ekleme, değiştirme veya bir sanal ağ alt silme öğrenin. Ekleme, değiştirme veya bir alt ağ silme önce sanal ağlar ile bilmiyorsanız, okumanızı öneririz [Azure Virtual Network'e genel bakış](virtual-networks-overview.md) ve [oluşturma, değiştirme veya silme bir sanal ağ](virtual-network-manage-network.md). Bir sanal ağ içinde dağıtılan tüm Azure kaynaklarını bir sanal ağ içindeki bir alt ağ içinde dağıtılır.
 
## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, https://portal.azure.com açın ve Azure hesabınızla oturum açın.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.2.0 gerektirir veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.26 gerektirir veya sonraki bir sürümü. Çalıştırma `az --version` yüklü olan sürümü bulunamıyor. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden bir alt ağa eklemek istediğiniz sanal ağı seçin.
3. Altında **ayarları**seçin **alt ağlar**.
4. Seçin **+ alt**.
5. Aşağıdaki parametreler için değerler girin:
    - **Ad**: ad sanal ağ içinde benzersiz olmalıdır.
    - **Adres aralığı**: aralık sanal ağın adres alanı içinde benzersiz olmalıdır. Aralık, diğer sanal ağ içinde alt ağ adres aralığı ile örtüşemez. Adres alanı, sınıfsız etki alanları arası yönlendirme (CIDR) gösterimini kullanarak belirtilmelidir. Örneğin, bir sanal ağda adres alanı 10.0.0.0/16, 10.0.0.0/24 alt ağ adres alanının tanımlayabilirsiniz. Belirleyebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralıktır. Azure her alt ağ protokolü uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri Azure hizmetinin kullanım için ayrılmıştır. Sonuç olarak, bir alt ağ/29 ile tanımlama adres alt ağda üç kullanılabilir IP adresleri aralığı sonuçlanır. Bir sanal ağ VPN ağ geçidi bağlanmak istiyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları için belirli bir adresi aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Alt ağ, belirli koşullar altında ekledikten sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adresi aralığını değiştirmek konusunda bilgi almak için bkz: [alt ağ ayarlarını değiştirme](#change-subnet-settings).
    - **Ağ güvenlik grubu**: sıfır veya bir alt ağ için gelen ve giden ağ trafiğini filtrelemek için bir mevcut ağ güvenlik grubunun bir alt ağa ilişkilendirebilirsiniz. Ağ güvenlik grubu aynı abonelikte ve konumda sanal ağ mevcut olmalıdır. Daha fazla bilgi edinmek [ağ güvenlik grubu](security-overview.md) ve [bir ağ güvenlik grubu oluşturmak nasıl](virtual-networks-create-nsg-arm-pportal.md).
    - **Yol tablosu**: ağ trafiği diğer ağlara yönlendirme denetlemek için bir alt ağa sıfır veya bir varolan yol tablosu ilişkilendirebilirsiniz. Yol tablosu, aynı abonelikte ve konumda sanal ağ içinde bulunmalıdır. Daha fazla bilgi edinmek [Azure yönlendirme](virtual-networks-udr-overview.md) ve [bir yol tablosu oluşturma](tutorial-create-route-table-portal.md)
    - **Hizmet uç noktaları:** bir alt ağ için etkin sıfır veya birden çok hizmet uç noktalarına sahip olabilir. Bir hizmet için bir hizmet uç noktası etkinleştirmek için hizmet veya hizmet uç noktalarından etkinleştirmek istediğiniz hizmetleri seçin **Hizmetleri** listesi. Hizmet uç noktası kaldırmak için hizmet uç noktası için kaldırmak istediğiniz hizmet seçimini kaldırın. Hizmet uç noktaları hakkında daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları genel bakış](virtual-network-service-endpoints-overview.md). Hizmet uç noktası bir hizmet için etkinleştirdikten sonra hizmet ile oluşturulan bir kaynak için alt ağ için ağ erişimini etkinleştirmeniz gerekir. Örneğin, hizmet uç noktası için etkinleştirirseniz *Microsoft.Storage*, ağ erişimi vermek istediğiniz tüm Azure depolama hesapları ağ erişimini etkinleştirmeniz gerekir. Hizmet uç noktası için etkin bir alt ağ erişiminin nasıl etkinleştirileceği hakkında daha fazla ayrıntı için hizmet uç noktası için etkin bireysel hizmet belgelerine bakın.
6. Seçtiğiniz sanal ağ alt ağı eklemek için seçin **Tamam**.

**Komutları**

- Azure CLI: [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>Alt ağ ayarlarını değiştirme

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden ayarlarını değiştirmek için istediğiniz alt ağ içeren sanal ağı seçin.
3. Altında **ayarları**seçin **alt ağlar**.
4. Alt ağlar listesinde ayarlarını değiştirmek için istediğiniz alt ağ seçin. Aşağıdaki ayarları değiştirebilirsiniz:

    - **Adres aralığı:** hiçbir kaynak alt ağ içinde dağıtılırsa, adres aralığını değiştirebilirsiniz. Herhangi bir kaynağa alt ağda varsa, başka bir alt ağa kaynakları taşımak gerekir veya önce alt ağdan silin. Taşımak veya bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Taşımak veya alt kaynaklarını silmek öğrenmek için taşımak veya silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Kısıtlamalarını bkz **adres aralığı** adım 5 [bir alt ağ Ekle](#add-a-subnet).
    - **Kullanıcıların**: yerleşik roller veya kendi özel roller kullanarak alt ağa erişimi denetleyebilirsiniz. Rol ve alt ağ erişmek için kullanıcı atama hakkında daha fazla bilgi için bkz: [Azure kaynaklarınıza erişimi yönetmek için rol ataması kullanın](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
    - Değiştirme hakkında bilgi için **ağ güvenlik grubu**, **yol tablosu**, **kullanıcılar**, ve **hizmet uç noktaları**, 5adımdabakın[ Bir alt ağ Ekle](#add-a-subnet).
5. Seçin **kaydetmek**.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Bir alt ağı silme

Alt ağ içindeki kaynak varsa, bir alt ağ silebilirsiniz. Alt ağ içindeki kaynaklar varsa, alt ağı silmeden önce alt ağdaki kaynakları silmeniz gerekir. Bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Alt kaynaklarını silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun.

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden silmek istediğiniz alt ağ içeren sanal ağı seçin.
3. Altında **ayarları**seçin **alt ağlar**.
4. Alt ağlar listesinde seçin **...** , sağ, alt ağ için silmek istediğiniz
5. Seçin **silmek**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ vnet Sil](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_delete)
- PowerShell: [Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>İzinler

Alt ağlardaki görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun izinleri atanmış rolü aşağıdaki tabloda listelenen:

|İşlem                                                                |   İşlem adı                               |
|-----------------------------------------------------------------------  |   -------------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Sanal ağ alt ağını Al                   |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Sanal ağ alt güncelle      |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Sanal ağ alt ağını Sil                |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Sanal ağa                         |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Bir alt ağa Hizmeti'ne Katıl                     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Sanal ağ alt ağı sanal makinelerini Al  |
