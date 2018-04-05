---
title: Ağ trafiği - Azure PowerShell filtre | Microsoft Docs
description: Bu makalede, bir alt ağ PowerShell kullanarak bir ağ güvenlik grubu ile ağ trafiğini filtrelemek öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 53406150cfc2ec4229ecd9cf3356ad03d60f8e7e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="filter-network-traffic-with-a-network-security-group-using-powershell"></a>PowerShell kullanarak bir ağ güvenlik grubu ile ağ trafiği filtreleme

Ağ güvenlik grubu ile ağ trafiği için gelen ve giden sanal ağ alt ağından filtreleyebilirsiniz. Ağ güvenlik grupları, IP adresi, bağlantı noktası ve protokol ağ trafiğinin filtre güvenlik kuralları içerir. Güvenlik kuralları, bir alt ağda dağıtılan kaynaklara uygulanır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Ağ güvenlik grubu ve güvenlik kuralları oluşturma
> * Bir sanal ağ oluşturun ve bir alt ağ için ağ güvenlik grubu ilişkilendirin
> * Sanal makineler (VM) bir alt ağ dağıtma
> * Test trafik filtreleri

Tercih ederseniz, bu makalede kullanarak tamamlayabilirsiniz [Azure CLI](tutorial-filter-network-traffic-cli.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Ağ güvenlik grubu güvenlik kuralları içerir. Güvenlik kuralları, bir kaynak ve hedef belirtin. Kaynakları ve hedeflere uygulama güvenlik grupları olabilir.

### <a name="create-application-security-groups"></a>Uygulama güvenlik grupları oluşturma

Önce bu makalede ile oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubunda oluşturur *eastus* konumu: 


```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Uygulama güvenlik grubu ile oluşturma [yeni AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup). Uygulama güvenlik grubu grubu sunucuları için gereksinimler filtreleme benzer bağlantı noktası ile sağlar. Aşağıdaki örnek, iki uygulama güvenlik grubu oluşturur.

```azurepowershell-interactive
$webAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgWebServers `
  -Location eastus

$mgmtAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgMgmtServers `
  -Location eastus
```

### <a name="create-security-rules"></a>Güvenlik kuralları oluşturma

Bir güvenlik kuralıyla oluşturma [yeni AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Aşağıdaki örnek, trafiğe izin veren bir kural oluşturur Internet'ten gelen *myWebServers* 80 ve 443 numaralı bağlantı noktaları üzerinden uygulama güvenlik grubu:

```azurepowershell-interactive
$webRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-Web-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $webAsg.id `
  -DestinationPortRange 80,443

The following example creates a rule that allows traffic inbound from the internet to the *myMgmtServers* application security group over port 3389:

$mgmtRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-RDP-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 110 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $mgmtAsg.id `
  -DestinationPortRange 3389
```

Bu makalede, RDP (3389 numaralı bağlantı noktası) için internet kullanıma sunulan *myAsgMgmtServers* VM. 3389 numaralı bağlantı noktasına Internet gösterme yerine üretim ortamları için kullanarak yönetmek istediğiniz Azure kaynaklarına bağlanmak önerilir bir [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [özel](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ağ bağlantısı.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu oluşturma [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNsg*: 

```powershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myNsg `
  -SecurityRules $webRule,$mgmtRule
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek, bir sanal adlı oluşturur *myVirtualNetwork*:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig)ve ardından sanal ağ ile alt ağ yapılandırması yazma [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). Aşağıdaki örnek adlı bir alt ağ, *mySubnet* ilişkilendirir ve sanal ağ için *myNsg* ona ağ güvenlik grubu:

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -VirtualNetwork $virtualNetwork `
  -AddressPrefix "10.0.2.0/24" `
  -NetworkSecurityGroup $nsg
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal makineleri oluşturmadan önce alt ağ ile sanal ağ nesnesiyle almak [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell-interactive
$virtualNetwork = Get-AzureRmVirtualNetwork `
 -Name myVirtualNetwork `
 -Resourcegroupname myResourceGroup
```
Her VM için bir ortak IP adresi oluşturma [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

$publicIpWeb = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` -ResourceGroupName myResourceGroup `
  -Location eastus ` -Name myVmWeb

$publicIpMgmt yeni AzureRmPublicIpAddress = `
  -AllocationMethod Dynamic ` - ResourceGroupName myResourceGroup `
  -Location eastus ` -Name myVmMgmt


İki ağ arabirimi ile oluşturmak [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)ve ağ arabirimi genel bir IP adresi atayın. Aşağıdaki örnek, bir ağ arabirimi, ilişkilendirilmiş bir oluşturur *myVmWeb* , genel IP adresi ve bir üyesi yapar *myAsgWebServers* uygulama güvenlik grubu:

```powershell-interactive
$webNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $webAsg.Id `
  -PublicIpAddressId $publicIpWeb.Id
```

Aşağıdaki örnek, bir ağ arabirimi, ilişkilendirilmiş bir oluşturur *myVmMgmt* , genel IP adresi ve bir üyesi yapar *myAsgMgmtServers* uygulama güvenlik grubu:

```powershell-interactive
$mgmtNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $mgmtAsg.Id `
  -PublicIpAddressId $publicIpMgmt.Id
```

Trafik bir sonraki adımda filtreleme doğrulamak için iki VM sanal ağ oluşturun. 

Bir VM yapılandırması oluşturma [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), VM oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, bir web sunucusu olarak davranacak bir VM oluşturur. `-AsJob` Seçeneği bir sonraki adıma devam etmek için bu VM arka planda oluşturur: 

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$webVmConfig = New-AzureRmVMConfig `
  -VMName myVmWeb `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmWeb `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $webNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $webVmConfig `
  -AsJob
```

Bir yönetim sunucusu hizmet için bir VM oluşturun:

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create the web server virtual machine configuration and virtual machine.
$mgmtVmConfig = New-AzureRmVMConfig `
  -VMName myVmMgmt `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmMgmt `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $mgmtNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $mgmtVmConfig
```

Sanal makine oluşturmak için birkaç dakika sürer. Azure VM oluşturma tamamlanana kadar sonraki adıma devam yok.

## <a name="test-traffic-filters"></a>Test trafik filtreleri

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) VM genel IP adresi dönün. Aşağıdaki örnek genel IP adresi döndürür *myVmMgmt* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* yerel bilgisayarınızdan VM. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.

Kullanıcı adını ve VM oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için), ardından **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için. 
   
Bağlantı başarılı olur, Internet'ten gelen 3389 numaralı bağlantı noktasına izin verilmediğinden *myAsgMgmtServers* ağ arabiriminin bağlı uygulama güvenlik grubu *myVmMgmt* VM konusu.

Bir Uzak Masaüstü bağlantısı oluşturmak için aşağıdaki komutu kullanın *myVmWeb* VM, gelen *myVmMgmt* powershell'den aşağıdaki komutu kullanarak VM:

``` 
mstsc /v:myvmWeb
```

Varsayılan güvenlik kuralı her ağ güvenlik grubu içinde bir sanal ağ içindeki tüm IP adresleri arasındaki tüm bağlantı noktaları üzerinden trafiğe izin verdiğinden bağlantı başarılı olur. Bir Uzak Masaüstü bağlantısı oluşturulamıyor *myVmWeb* VM internet'ten güvenlik kuralı için çünkü *myAsgWebServers* bağlantı noktası izin vermeyen 3389 internet'ten gelen.

Microsoft IIS yüklemek için aşağıdaki komutu kullanın *myVmWeb* PowerShell VM'den:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

IIS yüklemesi tamamlandıktan sonra bağlantısını kesmek *myVmWeb* size bırakır VM *myVmMgmt* VM Uzak Masaüstü Bağlantısı. IIS Hoş Geldiniz ekranında görüntülemek için bir Internet tarayıcısı açın ve göz atın http://myVmWeb.

Bağlantısını kesmek *myVmMgmt* VM.

Bilgisayarınızda Powershell'den genel IP adresi almak için aşağıdaki komutu girin *myVmWeb* sunucu:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Erişebildiğinizi onaylamak için *myVmWeb* web sunucusundan Azure dışında bilgisayarınızdaki bir Internet tarayıcısı açın ve `http://<public-ip-address-from-previous-step>`. Bağlantı başarılı olur, Internet'ten gelen bağlantı noktası 80 izin verilmediğinden *myAsgWebServers* ağ arabiriminin bağlı uygulama güvenlik grubu *myVmWeb* VM konusu.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ağ güvenlik grubu oluşturulur ve sanal ağ alt ağına ilişkilendirilmiş. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [ağ güvenlik grubu genel bakış](security-overview.md) ve [bir ağ güvenlik grubunu yönetme](virtual-network-manage-nsg-arm-ps.md).

Varsayılan alt ağlar arasında trafiği Azure yollar. Bunun yerine, bir güvenlik duvarı hizmet veren bir VM ile alt ağlar arasında trafiği yönlendirmek için seçebilirsiniz örneğin. Bir yol tablosu oluşturmayı öğrenmek için sonraki makalede ilerleyin.

> [!div class="nextstepaction"]
> [Rota tablosu oluşturma](./tutorial-create-route-table-portal.md)