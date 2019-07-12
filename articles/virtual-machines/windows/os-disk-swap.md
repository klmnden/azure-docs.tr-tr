---
title: PowerShell ile Azure VM için takas işletim sistemi diski | Microsoft Docs
description: PowerShell kullanarak Azure sanal makinesi tarafından kullanılan işletim sistemi diski olarak değiştirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
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
ms.openlocfilehash: ae3979f7ceae4a854df00b39d9c2b9673f65f987
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720125"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>PowerShell kullanarak Azure VM tarafından kullanılan işletim sistemi diskini değiştirme

Mevcut bir VM'ye sahip, ancak Yedekleme diski veya başka bir işletim sistemi diski için disk takas etmek istediğiniz işletim sistemi diskleri takas etmek için Azure PowerShell kullanabilirsiniz. VM'yi silip yeniden yükleme gerekmez. Zaten kullanımda olmadığı sürece, başka bir kaynak grubunda bile yönetilen disk kullanabilirsiniz.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

VM, stopped\deallocated olmasına gerek yoktur ve sonra yönetilen diskin kaynak kimliği farklı bir yönetilen diskin kaynak kimliği ile değiştirilebilir.

VM boyutu ile depolama türü eklemek için kullanmak istediğiniz disk ile uyumlu olduğundan emin olun. Kullanmak istediğiniz disk bir Premium depolama ise, örneğin, sonra VM (DS serisi boyutu gibi) Premium depolama özelliğine sahip olması gerekir. 

Diskleri kullanarak bir kaynak grubu listesini alma [Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk)

```azurepowershell-interactive
Get-AzDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Kullanmak istediğiniz diskin adını sahip olduğunuzda, VM için işletim sistemi diski olarak ayarlayın. Bu örnek stop\deallocates adlı VM *myVM* ve adlı disk atar *newDisk* yeni işletim sistemi diski olarak. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Sonraki adımlar**

Bir diski bir kopyasını oluşturmak için bkz: [bir diskin anlık görüntüsünü alma](snapshot-copy-managed-disk.md).