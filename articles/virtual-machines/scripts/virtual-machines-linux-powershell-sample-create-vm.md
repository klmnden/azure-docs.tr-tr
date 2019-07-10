---
title: Azure PowerShell Betiği Örneği - Linux Sanal Makinesi Oluşturma | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Linux Sanal Makinesi Oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/23/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 95672a2fd26461b5febeb571163673a3c5e8ee83
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708295"
---
# <a name="create-a-fully-configured-virtual-machine-with-powershell"></a>PowerShell ile tam olarak yapılandırılmış bir sanal makine oluşturma

Bu betik, Ubuntu işletim sistemi ile bir Azure Sanal Makinesi oluşturur. Betiği çalıştırdıktan sonra sanal makineye SSH üzerinden erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.ps1 "Create VM detailed")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [Yeni AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur. |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig) | Ağ güvenlik grubu kuralı yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır. |
| [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) | Ağ güvenlik grubu oluşturur. |
| [Get-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Alt ağ bilgilerini alır. Bu bilgiler, bir ağ arabirimi oluşturulurken kullanılır. |
| [Yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) | Ağ arabirimi oluşturur. |
| [Yeni AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) | Bir sanal makine için işletim sistemi özelliklerini ayarlar. |
| [Set-AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) | Sanal makine için görüntüyü belirtir. |
| [AzVMNetworkInterface ekleyin](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) | Sanal makineye bir ağ arabirimi ekler. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Sanal makine oluşturur. |
|[Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
