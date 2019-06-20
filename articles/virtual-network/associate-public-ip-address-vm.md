---
title: Bir sanal makine için genel bir IP adresi ilişkilendirme
titlesuffix: Azure Virtual Network
description: Bir sanal makineye bir genel IP adresini ilişkilendirmek öğrenin.
services: virtual-network
documentationcenter: ''
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: kumud
ms.openlocfilehash: 1b201957a33acd609eed8a2373c8201bdefe9d7d
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "64692000"
---
# <a name="associate-a-public-ip-address-to-a-virtual-machine"></a>Bir sanal makine için genel bir IP adresi ilişkilendirme

Bu makalede, mevcut bir sanal makine (VM) için genel bir IP adresi ilişkilendirme öğrenin. İnternet'ten bir sanal makineye bağlanmak istiyorsanız, VM ile ilişkili bir genel IP adresi olmalıdır. Genel bir IP adresi ile yeni bir VM oluşturmak istiyorsanız, bunu kullanarak yapabilirsiniz [Azure portalında](virtual-network-deploy-static-pip-arm-portal.md), [Azure komut satırı arabirimi (CLI)](virtual-network-deploy-static-pip-arm-cli.md), veya [PowerShell](virtual-network-deploy-static-pip-arm-ps.md). Genel IP adreslerinin nominal bir ücreti vardır. Ayrıntılar için bkz [fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses/). Abonelik başına kullanabileceğiniz genel IP adresleri sayısına bir sınır yoktur. Ayrıntılar için bkz [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#publicip-address).

Kullanabileceğiniz [Azure portalında](#azure-portal), Azure [komut satırı arabirimi](#azure-cli) (CLI) veya [PowerShell](#powershell) VM'ye bir genel IP adresini ilişkilendirmek için.

## <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Göz atın veya genel IP adresine eklemek ve ardından seçmek istediğiniz sanal makine için arama yapın.
3. Altında **ayarları**seçin **ağ**ve ardından istediğiniz için genel IP adresini eklemek için aşağıdaki resimde gösterildiği gibi ağ arabirimini seçin:

   ![Ağ arabirimi seçin](./media/associate-public-ip-address-vm/select-nic.png)

   > [!NOTE]
   > Genel IP adresleri, bir VM'ye ağ arabirimlerine ilişkilendirilir. Önceki resimde, VM'nin yalnızca bir ağ arabirimi bulunur. VM'nin birden çok ağ arabirimi varsa, tüm görünür ve ağ arabirimi için genel IP adresini ilişkilendirmek istediğiniz seçersiniz.

4. Seçin **IP yapılandırmaları** ve ardından aşağıdaki resimde gösterildiği gibi bir IP yapılandırması seçin:

   ![IP yapılandırması](./media/associate-public-ip-address-vm/select-ip-configuration.png)

   > [!NOTE]
   > Genel IP adresleri için bir ağ arabirimi IP yapılandırması için ilişkilendirilir. Önceki resimde, ağ arabirimi bir IP yapılandırmasına sahip. Ağ arabirimi birden fazla IP yapılandırması varsa, tüm listesinde görünür ve IP yapılandırması için genel IP adresini ilişkilendirmek istediğiniz seçersiniz.

5. Seçin **etkin**, ardından **IP adresi (*gerekli ayarları Yapılandır*)** . Otomatik olarak kapanır bir varolan genel IP adresi Seç **genel IP adresi seçin** kutusu. Listelenen tüm kullanılabilir genel IP adresleri yoksa, oluşturmanız gerekir. Bilgi edinmek için bkz [genel IP adresi oluşturma](virtual-network-public-ip-address.md#create-a-public-ip-address). Seçin **Kaydet**, izler ve IP yapılandırması için kutusunu kapatın resimde gösterildiği gibi.

   ![Genel IP adresini etkinleştir](./media/associate-public-ip-address-vm/enable-public-ip-address.png)

   > [!NOTE]
   > Görüntülenen genel IP adreslerini VM ile aynı bölgede mevcut olanlardır. Bir bölgede oluşturulan birden çok genel IP adresi varsa, tüm burada görünür. Gri herhangi, adresi zaten başka bir kaynak ilişkili olduğundan olur.

6. Aşağıdaki resimde gösterildiği gibi IP yapılandırması için atanan genel IP adresini görüntüleyin. Bu, bir IP adresi görünmesi birkaç saniye sürebilir.

   ![Genel IP adresi atanmış görüntüle](./media/associate-public-ip-address-vm/view-assigned-public-ip-address.png)

   > [!NOTE]
   > Her Azure bölgesi içinde kullanılan adreslerinden oluşan bir havuzdan adresi atanır. Her bölgede kullanılan adres havuzları listesini görmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Atanmış olan adresi havuzlarındaki bölge için kullanılan herhangi bir adres olabilir. Belirli bir havuzundan bölgesinde atanmış adresine ihtiyacınız varsa, bir [genel IP adresi ön eki](public-ip-address-prefix.md).

7. [VM ağ trafiğine izin veren](#allow-network-traffic-to-the-vm) bir ağ güvenlik grubu güvenlik kuralları ile.

## <a name="azure-cli"></a>Azure CLI

Yükleme [Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), veya Azure Cloud Shell'i kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Seçin **deneyin** düğmesi CLI komutları anlatılmaktadır. Seçme **deneyin** , Azure hesabınızla oturum açarak bir Cloud Shell çağırır.

1. CLI'yi yerel olarak Bash'te kullanıyorsanız, Azure ile oturum açın `az login`.
2. Bir VM'ye ağ arabirimi IP yapılandırması için genel bir IP adresi ilişkilidir. Kullanım [az ağ NIC IP yapılandırmasını Güncelleştir](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update) bir IP yapılandırmasına bir genel IP adresini ilişkilendirmek için komutu. Aşağıdaki örnekte adlı varolan genel IP adresini ilişkilendirir *myVMPublicIP* adlı IP yapılandırmasını *ipconfigmyVM* var olan bir ağ arabiriminin adlı *myVMVMNic* adlı bir kaynak grubunda mevcut olan *myResourceGroup*.
  
   ```azurecli-interactive
   az network nic ip-config update \
     --name ipconfigmyVM \
     --nic-name myVMVMNic \
     --resource-group myResourceGroup \
     --public-ip-address myVMPublicIP
   ```

   - Var olan bir genel IP adresi yoksa kullanın [az network public-IP oluşturma](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) oluşturmak için komutu. Örneğin, aşağıdaki komut adlı bir genel IP adresi oluşturur *myVMPublicIP* adlı bir kaynak grubu içinde *myResourceGroup*.
  
     ```azurecli-interactive
     az network public-ip create --name myVMPublicIP --resource-group myResourceGroup
     ```

     > [!NOTE]
     > Önceki komut özelleştirmek için isteyebileceğiniz birkaç ayar için varsayılan değeri olan bir genel IP adresi oluşturur. Tüm genel IP adresi ayarları hakkında daha fazla bilgi için bkz: [genel IP adresi oluşturma](virtual-network-public-ip-address.md#create-a-public-ip-address). Adresi, her Azure bölgesi için kullanılan genel IP adresi havuzundan atanır. Her bölgede kullanılan adres havuzları listesini görmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

   - VM'nize bağlanacak bir ağ arabirimi adını bilmiyorsanız, kullanın [az vm nic listesini](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-list) bunları görmek için komutu. Örneğin, aşağıdaki komut adlı bir sanal Makineye bağlı ağ arabirimleri adlarını listeler *myVM* adlı bir kaynak grubu içinde *myResourceGroup*:

     ```azurecli-interactive
     az vm nic list --vm-name myVM --resource-group myResourceGroup
     ```

     Çıktı aşağıdaki örneğe benzer bir veya daha fazla satır içerir:
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

     Önceki örnekte, *myVMVMNic* ağ arabirimini adıdır.

   - Bir ağ arabirimi IP yapılandırması adını bilmiyorsanız, kullanın [az ağ NIC IP-config listesi](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-list) bunları almak için komutu. Örneğin, aşağıdaki komut adlı bir ağ arabirimi için IP yapılandırma adlarını listeler *myVMVMNic* adlı bir kaynak grubu içinde *myResourceGroup*:

     ```azurecli-interactive
     az network nic ip-config list --nic-name myVMVMNic --resource-group myResourceGroup --out table
     ```

3. IP yapılandırması ile atanan genel IP adresini görüntüleyin [az vm-IP-adreslerini](/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) komutu. Aşağıdaki örnek, mevcut bir VM'ye atanan IP adresleri adlı gösterir *myVM* adlı bir kaynak grubu içinde *myResourceGroup*.

   ```azurecli-interactive
   az vm list-ip-addresses --name myVM --resource-group myResourceGroup --out table
   ```

   > [!NOTE]
   > Her Azure bölgesi içinde kullanılan adreslerinden oluşan bir havuzdan adresi atanır. Her bölgede kullanılan adres havuzları listesini görmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Atanmış olan adresi havuzlarındaki bölge için kullanılan herhangi bir adres olabilir. Belirli bir havuzundan bölgesinde atanmış adresine ihtiyacınız varsa, bir [genel IP adresi ön eki](public-ip-address-prefix.md).

4. [VM ağ trafiğine izin veren](#allow-network-traffic-to-the-vm) bir ağ güvenlik grubu güvenlik kuralları ile.

## <a name="powershell"></a>PowerShell

Yükleme [PowerShell](/powershell/azure/install-az-ps), veya Azure Cloud Shell'i kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir kabuktur. Hesabınızla birlikte kullanmak için önceden yüklenmiş ve yapılandırılmış PowerShell var. Seçin **deneyin** düğmesi PowerShell komutları anlatılmaktadır. Seçme **deneyin** , Azure hesabınızla oturum açarak bir Cloud Shell çağırır.

1. PowerShell'i yerel olarak kullanıyorsanız, Azure ile oturum açın `Connect-AzAccount`.
2. Bir VM'ye ağ arabirimi IP yapılandırması için genel bir IP adresi ilişkilidir. Kullanım [Get-AzVirtualNetwork](/powershell/module/Az.Network/Get-AzVirtualNetwork) ve [Get-AzVirtualNetworkSubnetConfig](/powershell/module/Az.Network/Get-AzVirtualNetworkSubnetConfig) sanal ağ ve ağ arabiriminin bulunduğu alt ağ için komutları. Ardından, [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) bir ağ arabirimi almak için komut ve [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) var olan bir genel IP adresini almak için komutu. Ardından [kümesi AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/Set-AzNetworkInterfaceIpConfig) IP yapılandırması için genel IP adresini ilişkilendirmek için komut ve [kümesi AzNetworkInterface](/powershell/module/Az.Network/Set-AzNetworkInterface) komutu yeni IP yapılandırmasına yazma Ağ arabirimi.

   Aşağıdaki örnekte adlı varolan genel IP adresini ilişkilendirir *myVMPublicIP* adlı IP yapılandırmasını *ipconfigmyVM* var olan bir ağ arabiriminin adlı *myVMVMNic* adlı bir alt ağ içinde mevcut olan *myVMSubnet* adlı bir sanal ağdaki *myVMVNet*. Adlı bir kaynak grubundaki tüm kaynakları olan *myResourceGroup*.
  
   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name myVMVNet -ResourceGroupName myResourceGroup
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name myVMSubnet -VirtualNetwork $vnet
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $pip = Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup
   $nic | Set-AzNetworkInterfaceIpConfig -Name ipconfigmyVM -PublicIPAddress $pip -Subnet $subnet
   $nic | Set-AzNetworkInterface
   ```

   - Var olan bir genel IP adresi yoksa kullanın [yeni AzPublicIpAddress](/powershell/module/Az.Network/New-AzPublicIpAddress) oluşturmak için komutu. Örneğin, aşağıdaki komut oluşturur bir *dinamik* adlı ortak IP adresi *myVMPublicIP* adlı bir kaynak grubu içinde *myResourceGroup* içinde  *eastus* bölge.
  
     ```azurepowershell-interactive
     New-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup -AllocationMethod Dynamic -Location eastus
     ```

     > [!NOTE]
     > Önceki komut özelleştirmek için isteyebileceğiniz birkaç ayar için varsayılan değeri olan bir genel IP adresi oluşturur. Tüm genel IP adresi ayarları hakkında daha fazla bilgi için bkz: [genel IP adresi oluşturma](virtual-network-public-ip-address.md#create-a-public-ip-address). Adresi, her Azure bölgesi için kullanılan genel IP adresi havuzundan atanır. Her bölgede kullanılan adres havuzları listesini görmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

   - VM'nize bağlanacak bir ağ arabirimi adını bilmiyorsanız, kullanın [Get-AzVM](/powershell/module/Az.Compute/Get-AzVM) bunları görmek için komutu. Örneğin, aşağıdaki komut adlı bir sanal Makineye bağlı ağ arabirimleri adlarını listeler *myVM* adlı bir kaynak grubu içinde *myResourceGroup*:

     ```azurepowershell-interactive
     $vm = Get-AzVM -name myVM -ResourceGroupName myResourceGroup
     $vm.NetworkProfile
     ```

     Çıktı aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda *myVMVMNic* ağ arabirimini adıdır.
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

   - Sanal ağ veya ağ arabiriminin bulunduğu alt ağ adını bilmiyorsanız, kullanın `Get-AzNetworkInterface` bilgileri görmek için komutu. Örneğin, aşağıdaki komutu adlı bir ağ arabirimi için sanal ağ ve alt ağ bilgilerini alır *myVMVMNic* adlı bir kaynak grubu içinde *myResourceGroup*:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $ipConfigs = $nic.IpConfigurations
     $ipConfigs.Subnet | Select Id
     ```

     Çıktı aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda *myVMVNET* sanal ağ adı ve *myVMSubnet* alt adıdır.
  
     ```
     "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVMVNET/subnets/myVMSubnet",
     ```

   - Bir ağ arabirimi IP yapılandırması adını bilmiyorsanız, kullanın [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) bunları almak için komutu. Örneğin, aşağıdaki komut adlı bir ağ arabirimi için IP yapılandırma adlarını listeler *myVMVMNic* adlı bir kaynak grubu içinde *myResourceGroup*:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $nic.IPConfigurations
     ```

     Çıktı aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda *ipconfigmyVM* IP yapılandırmasının adı.
  
     ```
     Id     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM
     ```

3. IP yapılandırması ile atanan genel IP adresini görüntüleyin [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) komutu. Aşağıdaki örnekte adlı bir genel IP adresi için atanmış olan adresi *myVMPublicIP* adlı bir kaynak grubu içinde *myResourceGroup*.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup | Select IpAddress
   ```

   Bir IP yapılandırması için atanan genel IP adresini adını bilmiyorsanız, almak için aşağıdaki komutları çalıştırın:

   ```azurepowershell-interactive
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $nic.IPConfigurations
   $address = $nic.IPConfigurations.PublicIpAddress
   $address | Select Id
   ```

   Çıktı aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda *myVMPublicIP* IP yapılandırması için atanan genel IP adresini adıdır.

   ```
   "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP"
   ```

   > [!NOTE]
   > Her Azure bölgesi içinde kullanılan adreslerinden oluşan bir havuzdan adresi atanır. Her bölgede kullanılan adres havuzları listesini görmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Atanmış olan adresi havuzlarındaki bölge için kullanılan herhangi bir adres olabilir. Belirli bir havuzundan bölgesinde atanmış adresine ihtiyacınız varsa, bir [genel IP adresi ön eki](public-ip-address-prefix.md).

4. [VM ağ trafiğine izin veren](#allow-network-traffic-to-the-vm) bir ağ güvenlik grubu güvenlik kuralları ile.

## <a name="allow-network-traffic-to-the-vm"></a>VM ağ trafiğine izin vermek

İnternet'ten genel IP adresine bağlanabilmeleri için gerekli bağlantı noktaları, ağ arabirimi, ağ arabiriminin içinde bulunduğu alt ağa veya her ikisi de ilişkili herhangi bir ağ güvenlik grubuyla açık olduğundan emin olun. Güvenlik grupları filtresi trafiği gelen internet trafiğini Azure genel IP adresinde geldikten ağ arabiriminin özel IP adresine çevrilir rağmen genel özel IP adresine adres, bunu bir ağ güvenlik grubu engelliyorsa trafik akışını, genel IP adresi ile iletişimi başarısız olur. Bir ağ arabirimi ve alt ağı kullanmak için geçerli güvenlik kuralları görüntüleyebileceğiniz [portalı](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-portal), [CLI](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-cli), veya [PowerShell](diagnose-network-traffic-filter-problem.md#diagnose-using-powershell).

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenize gelen internet trafiği bir ağ güvenlik grubu izin. Bir ağ güvenlik grubu oluşturmayı öğrenmek için bkz: [ağ güvenlik gruplarıyla çalışma](manage-network-security-group.md#work-with-network-security-groups). Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [güvenlik grupları](security-overview.md).
