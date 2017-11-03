---
title: "Azure PowerShell Betiği örnek - Docker | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - Docker"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c8b700d13e4645d408e4e752a541e521ef93a6e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-docker-host-with-powershell"></a>PowerShell ile bir Docker ana oluşturma

Bu komut dosyası etkin Docker ile bir sanal makine oluşturur ve NGINX çalıştıran bir kapsayıcı başlatır. Betiği çalıştırdıktan sonra NGINX web sunucusuna Azure sanal makinesini FQDN üzerinden erişebilirsiniz. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-docker-host/create-docker-host.ps1 "Create Docker host")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası dağıtımı oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [AzureRmVirtualNetworkSubnetConfig yeni](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma sanal ağ oluşturma işlemine kullanılır. |
| [Yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Sanal ağ oluşturur. |
| [AzureRmPublicIpAddress yeni](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Bir ortak IP adresi oluşturur. |
| [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Bir ağ güvenlik grubu kural yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır. |
| [AzureRmNetworkSecurityGroup yeni](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Bir ağ güvenlik grubu oluşturur. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Alt ağ bilgilerini alır. Bu bilgiler, bir ağ arabirimi oluşturulurken kullanılır. |
| [AzureRmNetworkInterface yeni](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir ağ arabirimi oluşturur. |
| [AzureRmVMConfig yeni](/powershell/module/azurerm.compute/new-azurermvmconfig) | Bir VM yapılandırması oluşturur. Bu yapılandırma VM adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma VM oluşturma sırasında kullanılır. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturun. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | VM uzantısı sanal makineye ekleyin. Bu örnekte, Docker uzantısı Docker yapılandırmak ve bir NGINX Docker kapsayıcısı çalıştırmak için kullanılır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
