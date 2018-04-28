---
title: Azure Windows VM yeniden boyutlandırmak için PowerShell kullanın | Microsoft Docs
description: Azure Powershell kullanarak Resource Manager dağıtım modelinde oluşturulmuş bir Windows sanal makine yeniden boyutlandırın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: cynthn
ms.openlocfilehash: d2010ee9017416360069c74118b8ae25e71e1da7
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="resize-a-windows-vm"></a>Bir Windows VM yeniden boyutlandırma
Bu makalede Windows Azure Powershell kullanarak Resource Manager dağıtım modelinde oluşturulan bir VM'yi yeniden boyutlandırın gösterilmiştir.

Bir sanal makine (VM) oluşturduktan sonra VM boyutunu değiştirerek yukarı veya aşağı VM ölçeklendirebilirsiniz. Bazı durumlarda, ilk VM ayırması gerekir. Yeni boyutu VM'i şu anda barındırma donanım kümede kullanılabilir değilse, bu durum oluşabilir.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde olmayan bir Windows VM yeniden boyutlandırma
1. VM barındırıldığı donanım kümede kullanılabilir VM boyutları listeler. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. İstenen boyut listeleniyorsa, VM yeniden boyutlandırmak için aşağıdaki komutları çalıştırın. İstenen boyut listelenmemişse, 3. adıma geçin.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. İstenen boyut, VM serbest bırakma için aşağıdaki komutları çalıştırın listelenmemişse yeniden boyutlandırabilir ve VM'yi yeniden başlatın.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -Name $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> VM serbest bırakma VM'ye atanan dinamik IP adreslerini serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde Windows VM yeniden boyutlandırma
Yeni bir kullanılabilirlik kümesinde bir VM boyutu şu anda VM barındırma donanım kümede kullanılabilir durumda değilse, tüm sanal makineleri kullanılabilirlik kümesindeki VM yeniden boyutlandırmak için serbest gerekecektir. Bir VM yeniden boyutlandırılmış sonra kullanılabilirlik diğer VM'ler boyutunu güncelleştirme gerekebilir. Bir kullanılabilirlik kümesinde bir VM'yi yeniden boyutlandırmak için aşağıdaki adımları gerçekleştirin.

1. VM barındırıldığı donanım kümede kullanılabilir VM boyutları listeler.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. İstenen boyut listeleniyorsa, VM yeniden boyutlandırmak için aşağıdaki komutları çalıştırın. Listelenmiyorsa, 3. adıma gidin.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. İstenen boyut listelenmemişse kullanılabilirlik kümesindeki tüm VM'ler ayırması, sanal makineleri yeniden boyutlandırma ve bunları yeniden başlatmak için aşağıdaki adımlarla devam edin.
4. Kullanılabilirlik kümesindeki tüm VM'ler durdurun.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. Yeniden boyutlandırma ve kullanılabilirlik kümesindeki sanal makineleri yeniden başlatın.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar
* Ek ölçeklenebilirlik için birden çok VM örnekleri çalıştırın ve ölçeğini. Daha fazla bilgi için bkz: [otomatik olarak bir sanal makine ölçek kümesindeki Windows makineler ölçekleme](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

