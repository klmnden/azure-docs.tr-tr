---
title: Azure PowerShell Betiği Örneği - NGINX | Microsoft Docs
description: Azure PowerShell Betiği Örneği - NGINX
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d957b366a2926750e593e497f953fc57b0c160e0
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888371"
---
# <a name="create-an-nginx-vm-with-powershell"></a>PowerShell ile NGINX sanal makinesi oluşturma

Bu betik bir Azure Sanal Makinesi oluşturur ve Azure Sanal Makinesi Özel Betik Uzantısını kullanarak NGINX yükler. Betiği çalıştırdıktan sonra sanal makinenin genel IP adresi üzerindeki bir tanıtım web sitesine erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.ps1 "Create VM NGINX")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Sanal ağ oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Ağ güvenlik grubu kuralı yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Ağ güvenlik grubu oluşturur. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Alt ağ bilgilerini alır. Bu bilgiler, bir ağ arabirimi oluşturulurken kullanılır. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Ağ arabirimi oluşturur. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine oluşturur. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Sanal makineye bir VM uzantısı ekleyin. Bu örnekte NGINX yüklemek için özel betik uzantısı kullanılır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
