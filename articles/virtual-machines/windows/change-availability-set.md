---
title: "Sanal makineleri kullanılabilirlik kümesini değiştirme | Microsoft Docs"
description: "Azure PowerShell ve Resource Manager dağıtım modeli kullanarak, sanal makineleriniz için ayarlanmış kullanılabilirlik değiştirmeyi öğrenin."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 44c90f90-bc9a-4260-a36f-5465e2a1ef94
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/15/2016
ms.author: drewm
ms.openlocfilehash: c10c947b6fc0737a7b9fba6b7f3efcae1f96638b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="change-the-availability-set-for-a-windows-vm"></a>Kullanılabilirlik için bir Windows VM kümesini değiştirin
Aşağıdaki adımlar, Azure PowerShell kullanarak bir VM'i kullanılabilirlik kümesini değiştirmek açıklar. Bir VM yalnızca kullanılabilirlik oluşturulduğunda kümesi için eklenebilir. Kullanılabilirlik değiştirmek için ayarlamanız, silmeniz ve sanal makine oluşturmanız gerekir. 

## <a name="change-the-availability-set-using-powershell"></a>Kullanılabilirlik PowerShell kullanarak kümesini değiştirin
1. Değiştirilecek VM'den aşağıdaki anahtar ayrıntılarını yakalayın.
   
    VM adı
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
   
    VM boyutu
   
    ```powershell
    $vm.HardwareProfile.VmSize
    ```
   
    Ağ birincil ağ arabirimi ve VM varsa isteğe bağlı ağ arabirimleri
   
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```
   
    İşletim sistemi diski profili
   
    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```
   
    Her veri diski için disk profilleri 
   
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```
   
    VM uzantıları yüklü 
   
    ```powershell
    $vm.Extensions
    ```
2. VM diskleri veya ağ arabirimleri herhangi birini silmeden silin.
   
    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```
3. Kullanılabilirlik zaten yoksa, kümesini oluşturma
   
    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```
4. Yeni kullanılabilirlik kümesi kullanarak VM yeniden oluşturun
   
    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>
   
    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]
   
    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 
   
    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 
5. Veri diskleri ve uzantıları ekleyin. Daha fazla bilgi için bkz: [Attach veri diski VM'ye](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Resource Manager şablonları uzantılarında](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions). Veri diskleri ve uzantıları PowerShell veya Azure CLI kullanarak VM eklenebilir.

## <a name="example-script"></a>Örnek komut dosyası
Aşağıdaki komut dosyasını gerekli bilgileri toplama, orijinal VM silme ve yeni bir kullanılabilirlik kümesinde yeniden ilişkin bir örnek verilmektedir.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkProfile.NetworkInterfaces) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic.Id
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Sonraki adımlar
Ek depolama alanı, VM için ek bir ekleyerek [veri diski](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

