---
title: "Bir IIS VM oluşturmak için yeni AzVM kullanın | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - yeni AzVM kullanarak IIS çalıştıran bir VM oluşturun."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: cynthn
ms.openlocfilehash: 932920483e9196251b125065f8efe68d27dda559
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-iis-vm-using-the-new-azvm-powershell-cmdlet-preview"></a>(Önizleme) yeni AzVM PowerShell cmdlet'ini kullanarak bir IIS VM oluşturma

Bu komut dosyasını Windows Server 2016 çalıştıran bir Azure sanal makine oluşturur ve ardından IIS yüklemek için Azure sanal makine özel betik uzantısı kullanır. Betiği çalıştırdıktan sonra sanal makine genel IP adresi varsayılan IIS Web sitesinin erişebilir. 

Bu örnek önizlemede yeni AzVM cmdlet'ini kullanır. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="sample-script"></a>Örnek komut dosyası

```azurepowershell-interactive

New-AzVM -Name myVM `
    -VirtualNetworkName myVNet `
    -Location westeurope `
    -SecurityGroupName myNSG `
    -PublicIpAddressName myPIP


# Create an inbound network security group rule for port 80

$nsgRuleHTTP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleHTTP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 80 -Access Allow

$nsg = Get-AzureRmNetworkSecurityGroup -Name myNSG -ResourceGroupName myVMResourceGroup
$nsg | Add-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleHTTP -NetworkSecurityGroup myNSG 
$nsg | Set-AzureRmNetworkSecurityGroup

# Install IIS

Set-AzureRmVMExtension -ExtensionName "IIS" -ResourceGroupName myVMResourceGroup -VMName myVM `
  -Publisher "Microsoft.Compute" -ExtensionType "CustomScriptExtension" -TypeHandlerVersion 1.4 `
  -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' -Location westeurope
  
```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası dağıtımı oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.

| Komut | Notlar |
|---|---|
| [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Bir ağ güvenlik grubu kural yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır. |
| [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup) | NSG bilgilerini alır. |
| [Ekleme AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig) | Yeni Kural yapılandırmasına ekler. |
| [Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/Set-AzureRmNetworkSecurityGroup) | NSG yeni kuralı güncelleştirir. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | VM uzantısı sanal makineye ekleyin. Bu örnekte, özel betik uzantısının IIS yüklemek için kullanılır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
