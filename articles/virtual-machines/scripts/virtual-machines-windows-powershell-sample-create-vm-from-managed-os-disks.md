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
ms.openlocfilehash: a608db9d806f9b0ed69eec3ce4dfb69adc5a5ea3
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449085"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a>PowerShell ile yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma

Bu betik, mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. Yukarıdaki senaryolarda bu betiği kullanın:
* Farklı abonelikteki bir yönetilen diskten kopyalanmış mevcut bir yönetilen işletim sistemi diskinden VM oluşturma
* Özelleştirilmiş bir VHD dosyasından oluşturulmuş mevcut bir yönetilen diskten VM oluşturma 
* Anlık görüntüden oluşturulmuş mevcut bir yönetilen işletim sistemi diskinden VM oluşturma 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik yönetilen disk özelliklerini almak, yeni bir VM’ye yönetilen bir disk eklemek ve bir VM oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/Get-AzDisk) | Diskin adına ve kaynak grubuna göre disk nesnesini alır. Döndürülen disk nesnesinin Id özelliği, diski yeni VM'ye bağlamak için kullanılır |
| [Yeni AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) | Diskin Id özelliğini kullanarak yönetilen diski yeni sanal makineye işletim sistemi diski olarak ekler |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [Yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) | Ağ arabirimi oluşturur. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Sanal makine oluşturur. |
|[Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

Market görüntülerini kullanma [kümesi AzVMPlan](https://docs.microsoft.com/powershell/module/az.compute/set-azvmplan) plan bilgisini ayarlamak için.

```powershell
Set-AzVMPlan -VM $VirtualMachine -Publisher $Publisher -Product $Product -Name $Bame
```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
