---
title: "Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme | Microsoft Docs"
description: "Nasıl bulmak ve eklenmemiş Azure yönetilen ve yönetilmeyen (VHD'ler/sayfa bloblarını) diskleri, Azure PowerShell kullanarak siler."
services: virtual-machines-windows
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: a846d3578d40b19762f185381c92bdf8e225b185
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme
Azure'da sanal makine sildiğinizde, varsayılan olarak bağlı disk silinmez. Sanal makineler yanlışlıkla silinen kaynaklanan veri kaybının engeller, ancak eklenmemiş diskler için gereksiz yere ödeme geçin. Bul ve eklenmemiş tüm diskleri silin ve maliyet kaydetmek için bu makaleyi kullanın. 


## <a name="find-and-delete-unattached-managed-disks"></a>Bulma ve eklenmemiş yönetilen diskleri silme 

Aşağıdaki komut dosyasını bulmak gösterilmiştir eklenmemiş [yönetilen diskleri](managed-disks-overview.md) kullanarak *şirketiniz tarafından* özelliği. Tüm yönetilen diskleri bir abonelikte ve denetimleri aracılığıyla döngü *şirketiniz tarafından* eklenmemiş yönetilen diskleri bulmak için özellik NULL'dur. *Şirketiniz tarafından* özelliği, yönetilen bir diske bağlı sanal makine kaynak Kimliğini depolar.

Yüksek oranda ilk çalıştırmada betik ayarlayarak öneririz *deleteUnattachedDisks* değişken eklenmemiş tüm diskleri görüntülemek için 0. Komut dosyasını ayarlayarak çalıştırmak eklenmemiş diskleri gözden geçirdikten sonra *deleteUnattachedDisks* eklenmemiş tüm diskleri silmek için 1.

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Bulma ve eklenmemiş yönetilmeyen diskleri silme 

Yönetilmeyen disklerdir VHD dosyaları [Sayfa bloblarını] olarak depolanan (/ rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası kullanarak eklenmemiş yönetilmeyen diskleri (sayfa bloblarını) bulmak gösterilmiştir *LeaseStatus* özelliği. Tüm depolama hesapları bir abonelikte ve denetler yönetilmeyen tüm diskleri aracılığıyla döngü *LeaseStatus* özelliği kilidi eklenmemiş yönetilmeyen disk bulunamadı. *LeaseStatus* yönetilmeyen bir diski bir sanal makineye bağlıysa özelliği için kilitli ayarlanır.

Yüksek oranda ilk çalıştırmada betik ayarlayarak öneririz *deleteUnattachedVHDs* değişken eklenmemiş tüm diskleri görüntülemek için 0. Komut dosyasını ayarlayarak çalıştırmak eklenmemiş diskleri gözden geçirdikten sonra *deleteUnattachedVHDs* eklenmemiş tüm diskleri silmek için 1.


```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}

```

## <a name="next-steps"></a>Sonraki adımlar

[Depolama hesabını silme](../../storage/common/storage-create-storage-account.md)




