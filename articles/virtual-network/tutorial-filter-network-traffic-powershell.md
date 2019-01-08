---
title: -Azure PowerShell olan ağ trafiğini filtreleme | Microsoft Docs
description: Bu makalede, PowerShell kullanarak bir ağ güvenlik grubu ile bir alt ağ için ağ trafiğini filtreleme öğrenin.
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
ms.custom: mvc
ms.openlocfilehash: 92c1e706d3cbcff8bb0d66ddf4f2e73362a96af6
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064534"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-powershell"></a>PowerShell kullanarak bir ağ güvenlik grubu ile ağ trafiğini filtreleme

Bir sanal ağ alt ağına gelen ve sanal ağ alt ağından giden ağ trafiğini, bir ağ güvenlik grubu ile filtreleyebilirsiniz. Ağ güvenlik grupları, ağ trafiğini IP adresi, bağlantı noktası ve protokole göre filtreleyen güvenlik kuralları içerir. Güvenlik kuralları bir alt ağda dağıtılmış kaynaklara uygulanır. Bu makalede şunları öğreneceksiniz:

* Ağ güvenlik grubu ve güvenlik kuralları oluşturma
* Bir sanal ağ oluşturma ve ağ güvenlik grubunu alt ağ ile ilişkilendirme
* Sanal makineleri (VM) bir alt ağa dağıtma
* Trafik filtrelerini test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell modülü sürüm 6.2.1 gerekir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu, güvenlik kuralları içerir. Güvenlik kuralları, bir kaynak ve hedefi belirtir. Kaynaklar ve hedefler, uygulama güvenlik grupları olabilir.

### <a name="create-application-security-groups"></a>Uygulama güvenlik grupları oluşturma

Önce bu makalede oluşturulan tüm kaynakları için bir kaynak grubu oluşturma [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnekte *eastus* konumunda bir kaynak grubu oluşturulmaktadır: 


```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup) ile bir uygulama güvenlik grubu oluşturun. Uygulama güvenlik grubu, benzer bağlantı noktası filtreleme gereksinimlerine sahip sunucuları gruplandırmanızı sağlar. Aşağıdaki örnek iki uygulama güvenlik grubu oluşturur.

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

[New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) ile bir güvenlik kuralı oluşturun. Aşağıdaki örnek, internetten gelen trafiğin 80 ve 443 numaralı bağlantı noktaları üzerinden *myWebServers* uygulama güvenlik grubuna gitmesine izin veren bir kural oluşturur:

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

Bu makalede, RDP (bağlantı noktası 3389) için İnternet'e kullanıma sunulan *myAsgMgmtServers* VM. Üretim ortamlarında 3389 numaralı bağlantı noktasını internette kullanıma sunmak yerine [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [özel](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ağ bağlantısı kullanarak yönetmek istediğiniz Azure kaynaklarına bağlamanız önerilir.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek *myNsg* adlı bir ağ güvenlik grubu oluşturur: 

```powershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myNsg `
  -SecurityRules $webRule,$mgmtRule
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek *myVirtualNetwork* adlı bir sanal ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ile bir alt ağ yapılandırması oluşturun ve sonra alt ağ yapılandırmasını [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) ile sanal ağa yazın. Aşağıdaki örnek, sanal ağa *mySubnet* adlı bir alt ağ ekler ve *myNsg* ağ güvenlik grubunu onunla ilişkilendirir:

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -VirtualNetwork $virtualNetwork `
  -AddressPrefix "10.0.2.0/24" `
  -NetworkSecurityGroup $nsg
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’leri oluşturmadan önce [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) ile alt ağı içeren sanal ağ nesnesini alın:

```powershell-interactive
$virtualNetwork = Get-AzureRmVirtualNetwork `
 -Name myVirtualNetwork `
 -Resourcegroupname myResourceGroup
```
[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) ile her VM için bir genel IP adresi oluşturun:

```powershell-interactive
$publicIpWeb = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myVmWeb

$publicIpMgmt = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myVmMgmt
```

[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) ile iki ağ arabirimi oluşturun ve ağ arabirimine bir genel IP adresi atayın. Aşağıdaki örnek bir ağ arabirimi oluşturur, *myVmWeb* genel IP adresini onunla ilişkilendirir ve *myAsgWebServers* uygulama güvenlik grubunun bir üyesi yapar:

```powershell-interactive
$webNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $webAsg.Id `
  -PublicIpAddressId $publicIpWeb.Id
```

Aşağıdaki örnek bir ağ arabirimi oluşturur, *myVmMgmt* genel IP adresini onunla ilişkilendirir ve *myAsgMgmtServers* uygulama güvenlik grubunun bir üyesi yapar:

```powershell-interactive
$mgmtNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $mgmtAsg.Id `
  -PublicIpAddressId $publicIpMgmt.Id
```

Daha sonraki bir adımda trafik filtrelemesini doğrulayabilmek için sanal ağda iki VM oluşturun. 

[New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) ile bir VM yapılandırması oluşturun, ardından [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun. Aşağıdaki örnek, web sunucusu olarak görev yapacak bir VM oluşturur. `-AsJob` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur: 

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

Yönetim sunucusu olarak görev yapacak bir VM oluşturun:

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

Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM oluşturma işlemini tamamlayana kadar sonraki adıma geçmeyin.

## <a name="test-traffic-filters"></a>Trafik filtrelerini test etme

Bir sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Aşağıdaki örnek,*myVmMgmt* sanal makinesinin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Yerel bilgisayarınızdan *myVmMgmt* ile bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin. 
   
*myVmMgmt* sanal makinesine bağlı ağ arabiriminin içinde bulunduğu *myAsgMgmtServers* uygulama güvenlik grubuna 3389 numaralı bağlantı noktasının internetten gelmesine izin verildiği için bağlantı başarılı olur.

PowerShell üzerinden *myVmWeb* sanal makinesi ile *myVmMgmt* sanal makinesi arasında bir uzak masaüstü bağlantısı oluşturmak için aşağıdaki komutu kullanın:

``` 
mstsc /v:myvmWeb
```

Her bir ağ güvenlik grubu içindeki varsayılan güvenlik kuralı bir sanal ağ içindeki tüm IP adresleri arasında tüm bağlantı noktaları üzerinden trafiğe izin verdiği için bağlantı başarılı olur. *myAsgWebServers* güvenlik kuralı internetten 3389 numaralı gelen bağlantı noktasına izin vermediği için *myVmWeb* sanal makinesi ile bir uzak masaüstü bağlantısı oluşturamazsınız.

PowerShell’den *myVmWeb* sanal makinesine Microsoft IIS yüklemek için aşağıdaki komutu kullanın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

IIS yüklemesi tamamlandıktan sonra *myVmWeb* sanal makinesinin bağlantısını kesmeniz durumunda *myVmMgmt* VM uzak masaüstü bağlantısında kalırsınız. IIS giriş ekranını görüntülemek için bir internet tarayıcısı açın ve http://myVmWeb sayfasına göz atın.

*myVmMgmt* sanal makinesiyle bağlantıyı kesin.

Bilgisayarınızda *myVmWeb* sunucusundan genel IP adresini almak için Powershell'den aşağıdaki komutu girin:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

*myVmWeb* web sunucusuna Azure dışından erişebildiğinizi onaylamak için bilgisayarınızda bir internet tarayıcısı açın ve `http://<public-ip-address-from-previous-step>` sayfasına göz atın. *myVmWeb* sanal makinesine bağlı ağ arabiriminin içinde bulunduğu *myAsgWebServers* uygulama güvenlik grubuna 80 numaralı bağlantı noktasının internetten gelmesine izin verildiği için bağlantı başarılı olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ağ güvenlik grubu oluşturdunuz ve bir sanal ağ alt ağ ile ilişkilendirilmiş. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek bkz. [Ağ güvenlik grubuna genel bakış](security-overview.md) ve [Ağ güvenlik grubunu yönetme](manage-network-security-group.md).

Azure, varsayılan olarak trafiği alt ağlar arasında yönlendirir. Bunun yerine, alt ağlar arasındaki trafiği, örneğin, güvenlik duvarı olarak görev yapan bir VM aracılığıyla yönlendirmeyi seçebilirsiniz. Bilgi edinmek için bkz. nasıl [yönlendirme tablosu oluşturma](tutorial-create-route-table-powershell.md).
