---
title: PowerShell ile bir Azure VM için takas işletim sistemi diski | Microsoft Docs
description: Bir Azure PowerShell kullanarak sanal makine tarafından kullanılan işletim sistemi diski olarak değiştirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: caa8fe2088995e3d30c9b808f639b9280e3a74be
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32196199"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>Bir Azure PowerShell kullanarak bir VM tarafından kullanılan işletim sistemi diski değiştirme

Mevcut bir VM'yi var ancak bir yedek diski veya başka bir işletim sistemi diski için disk takas etmek istediğiniz işletim sistemi diski değiştirmek için Azure PowerShell'i kullanabilirsiniz. VM silip gerekmez. Zaten kullanımda olmadığı sürece, yönetilen bir diski başka bir kaynak grubunda bile kullanabilirsiniz.

VM stopped\deallocated olmasına gerek yoktur ve farklı bir yönetilen disk kaynak kimliği ile kaynak kimliği yönetilen diskin değiştirilebilir.

VM boyutu ve depolama türü eklemek için kullanmak istediğiniz disk ile uyumlu olduğundan emin olun. Premium depolama alanına kullanmak istediğiniz disk ise, örneğin, daha sonra VM (DS serisi boyutu gibi) Premium depolama özelliğine sahip olması gerekir. 

Kullanarak bir kaynak grubu içinde disklerin listesini almak [Get-AzureRmDisk](/powershell/module/azurerm.compute/get-azurermdisk)

```azurepowershell-interactive
Get-AzureRmDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Kullanmak istediğiniz disk adı varsa, VM için işletim sistemi diski olarak ayarlayın. Bu örnek stop\deallocates adlı VM *myVM* ve adlı disk atar *newDisk* yeni işletim sistemi diski olarak. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzureRmDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzureRmVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Sonraki adımlar**

Bir disk bir kopyasını oluşturmak için bkz: [bir disk anlık görüntü](snapshot-copy-managed-disk.md).