---
title: Azure PowerShell Betik Örneği - Yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma | Microsoft Docs
description: Azure PowerShell Betik Örneği - Yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 4e1695eb85988b74bf968ebe08602164c94f01dd
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696073"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a>PowerShell ile yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma

Bu betik, mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. Yukarıdaki senaryolarda bu betiği kullanın:
* Farklı abonelikteki bir yönetilen diskten kopyalanmış mevcut bir yönetilen işletim sistemi diskinden VM oluşturma
* Özelleştirilmiş bir VHD dosyasından oluşturulmuş mevcut bir yönetilen diskten VM oluşturma 
* Anlık görüntüden oluşturulmuş mevcut bir yönetilen işletim sistemi diskinden VM oluşturma 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik yönetilen disk özelliklerini almak, yeni bir VM’ye yönetilen bir disk eklemek ve bir VM oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzureRmDisk](/powershell/module/azurerm.compute/Get-AzureRmDisk) | Diskin adına ve kaynak grubuna göre disk nesnesini alır. Döndürülen disk nesnesinin Id özelliği, diski yeni VM'ye bağlamak için kullanılır |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) | Diskin Id özelliğini kullanarak yönetilen diski yeni sanal makineye işletim sistemi diski olarak ekler |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Ağ arabirimi oluşturur. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine oluşturur. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

Market görüntülerinde plan bilgilerini ayarlamak için [Set-AzureRmVMPlan](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmplan?view=azurermps-6.7.0) komutunu kullanın
```powershell
Set-AzureRmVMPlan -VM $VirtualMachine -Publisher $Publisher -Product $Product -Name $Name
```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
