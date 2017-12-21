---
title: "Windows Azure VM özelleştirme | Microsoft Docs"
description: "Anahtar kasası ve özel betik uzantısı azure'da Windows VM özelleştirmek için nasıl kullanılacağını öğrenin"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 17a6c243aaf73fcd88261870fbdd9e8c936471b8
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makine özelleştirme
Sanal makineler (VM'ler) hızlı ve tutarlı bir şekilde yapılandırmak için tür Otomasyon genellikle istendiğini. Bir Windows VM özelleştirmek için ortak bir yaklaşım kullanmaktır [için özel betik uzantısı Windows](extensions-customscript.md). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özel betik uzantısının IIS yüklemek için kullanın
> * Özel betik uzantısı kullanan bir VM oluşturma
> * Uzantı uygulandıktan sonra çalışan bir IIS site görüntüleyin

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 


## <a name="custom-script-extension-overview"></a>Özel betik uzantısı genel bakış
Özel betik uzantısının indirir ve Azure Vm'lerinde komut dosyaları çalıştırılır. Bu dağıtım yapılandırmaları, yazılım yükleme veya başka bir yapılandırma için yararlı bir uzantısıdır / yönetim görevi. Komut dosyaları Azure depolama veya GitHub indirilen veya çalışma zamanı uzantısı Azure portalında sağlanmaktadır.

Özel betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalında veya Azure sanal makine REST API'sini kullanarak da çalıştırılabilir.

Özel betik uzantısı, Windows ve Linux VM'ler ile kullanabilirsiniz.


## <a name="create-virtual-machine"></a>Sanal makine oluşturma
Bir VM oluşturmadan önce bir kaynak grubuyla oluşturmanız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAutomate* içinde *EastUS* konumu:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Yönetici olan VM'ler için kullanıcı adı ve parola ayarlayın [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

VM oluşturmak üzere, şimdi [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek gereken sanal ağ bileşenleri, işletim sistemi yapılandırması ve adlı bir VM oluşturur *myVM*:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Oluşturulacak VM ve kaynaklar için birkaç dakika sürer.


## <a name="automate-iis-install"></a>IIS yükleme otomatikleştirme
Kullanım [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) özel betik uzantısı yüklemek için. Uzantı çalışır `powershell Add-WindowsFeature Web-Server` IIS Web sunucusu ve ardından güncelleştirmeleri yüklemek için *Default.htm* sayfası ana bilgisayar VM adını göster:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Sınama web sitesi
Yük Dengeleyici ile genel IP adresi elde [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Aşağıdaki örnek IP adresi alacağı *myPublicIP* daha önce oluşturduğunuz:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Bir web tarayıcısı ortak IP adresi girebilirsiniz. Yük Dengeleyici trafiği için aşağıdaki örnekteki gibi dağıtılmış VM konak adı dahil olmak üzere Web sitesi görüntülenir:

![Çalışan IIS Web sitesi](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir VM IIS yükle otomatik. Şunları öğrendiniz:

> [!div class="checklist"]
> * Özel betik uzantısının IIS yüklemek için kullanın
> * Özel betik uzantısı kullanan bir VM oluşturma
> * Uzantı uygulandıktan sonra çalışan bir IIS site görüntüleyin

Özel VM görüntüleri oluşturma hakkında bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Özel VM görüntüleri oluşturma](./tutorial-custom-images.md)
