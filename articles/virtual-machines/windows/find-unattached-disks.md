---
title: Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme | Microsoft Docs
description: Nasıl bulmak ve Azure PowerShell kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD'ler/sayfa bloblarını) diskleri silin.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ramankum
ms.openlocfilehash: 17262978c9600b75a1ddf945cf170fea6ac4f8ce
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34756941"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme
Bir sanal makine (VM) Azure, varsayılan olarak, sildiğinizde VM'ye bağlı diskler silinmez. Bu özellik VM'ler yanlışlıkla silinmesini nedeniyle veri kaybını önlemeye yardımcı olur. Bir VM silindikten sonra eklenmemiş diskler için ödeme devam eder. Bu makalede bulmak ve eklenmemiş tüm diskleri silin ve gereksiz maliyetleri azaltmak nasıl gösterir. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskleri: bulma ve eklenmemiş diskleri silme 

Aşağıdaki komut dosyası arar eklenmemiş [yönetilen diskleri](managed-disks-overview.md) değerini inceleyerek tarafından **şirketiniz tarafından** özelliği. Yönetilen bir diske bir VM öğesine bağlı olduğu **şirketiniz tarafından** özelliği VM kaynak Kimliğini içerir. Yönetilen bir disk eklenmemiş, olduğunda **şirketiniz tarafından** özelliği null. Komut dosyası Azure aboneliği içindeki tüm yönetilen disklerin inceler. Ne zaman betik bulur yönetilen bir diskle **şirketiniz tarafından** betik null olarak ayarlayın özelliği, diskin eklenmemiş olup olmadığını belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak komut dosyasını çalıştırmak **deleteUnattachedDisks** 0 değişken. Bu eylem, bulma ve tüm eklenmemiş yönetilen diskleri görüntüleme sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve ayarlama **deleteUnattachedDisks** değişken 1. Bu eylem tüm eklenmemiş yönetilen diskleri silmenize olanak sağlar.
>

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

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: bulma ve eklenmemiş diskleri silme 

Yönetilmeyen disklerdir olarak depolanan VHD dosyaları [sayfa blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası değerini inceleyerek eklenmemiş yönetilmeyen diskleri (sayfa bloblarını) görünüyor **LeaseStatus** özelliği. Yönetilmeyen bir diski bir VM öğesine bağlı olduğu **LeaseStatus** özelliği ayarlanmış **kilitli**. Yönetilmeyen bir disk eklenmemiş, olduğunda **LeaseStatus** özelliği ayarlanmış **kilitli değil**. Komut dosyası Azure depolama hesaplarında bir Azure aboneliği bulunan tüm yönetilmeyen diskleri inceler. Ne zaman betik bulur yönetilmeyen bir diskle bir **LeaseStatus** özelliğini **kilitli değil**, komut dosyası diskin eklenmemiş olup olmadığını belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak komut dosyasını çalıştırmak **deleteUnattachedVHDs** 0 değişken. Bu eylem, bulma ve tüm eklenmemiş yönetilmeyen VHD'leri görüntüleme sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve ayarlama **deleteUnattachedVHDs** değişken 1. Bu eylem tüm eklenmemiş yönetilmeyen VHD'leri silmenize olanak sağlar.
>

```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=0

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

Daha fazla bilgi için bkz: [depolama hesabı Sil](../../storage/common/storage-create-storage-account.md) ve [tanımlamak yalnız bırakılmış diskler PowerShell kullanılarak](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)



