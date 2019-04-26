---
title: Bulup silmesine eklenmemiş Azure yönetilen ve yönetilmeyen disk | Microsoft Docs
description: Nasıl bulun ve Azure PowerShell kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD/sayfa BLOB'ları) diskleri silin.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: cb52956afe085c076f0a9a7c2d6810f3def32e3f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325791"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen disk silme

Bir sanal makine (VM), azure'daki varsayılan olarak, sildiğinizde VM'ye bağlı tüm diskleri silinmez. Bu özellik VM'ler yanlışlıkla silinmesini nedeniyle veri kaybını önlemeye yardımcı olur. VM silindikten sonra eklenmemiş disk için ödeme yapmaya devam edersiniz. Bu makalede bulup eklenmemiş tüm diskleri silin ve gereksiz maliyetleri azaltın gösterilmektedir.

## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskler: Bulma ve eklenmemiş disk silme

Aşağıdaki komut dosyasını arar eklenmemiş [yönetilen diskler](managed-disks-overview.md) değerini inceleme tarafından **ManagedBy** özelliği. Bir VM'ye yönetilen disk eklendiğinde **ManagedBy** özelliği, VM kaynak Kimliğini içerir. Yönetilen disk eklenmemiş, olduğunda **ManagedBy** özelliği null. Betik bir Azure aboneliğindeki tüm yönetilen diskler inceler. Betik bir yönetilen disk ile ne zaman bulur **ManagedBy** özelliği null, betik disk eklenmemiş belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak betiği çalıştırmak **deleteUnattachedDisks** 0 değişken. Bu eylem, bulma ve görüntüleme eklenmemiş tüm yönetilen diskler sağlar.
>
>Tüm eklenmemiş disk gözden geçirdikten sonra betiğini yeniden çalıştırın ve ayarlama **deleteUnattachedDisks** 1 değişken. Bu eylem eklenmemiş tüm yönetilen diskler silmenize olanak sağlar.

```azurepowershell-interactive
# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0
$managedDisks = Get-AzDisk
foreach ($md in $managedDisks) {
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){
        if($deleteUnattachedDisks -eq 1){
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"
            $md | Remove-AzDisk -Force
            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "
        }else{
            $md.Id
        }
    }
 }
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: Bulma ve eklenmemiş disk silme

Yönetilmeyen diskler olarak depolanmış VHD dosyaları, [sayfa blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası değerini inceleyerek eklenmemiş yönetilmeyen diskler (sayfa blobları) arar **LeaseStatus** özelliği. Yönetilmeyen disk bir sanal makineye bağlı olduğu **LeaseStatus** özelliği **kilitli**. Yönetilmeyen disk eklenmemiş, olduğunda **LeaseStatus** özelliği **kilitli değil**. Betik bir Azure aboneliğindeki tüm Azure depolama hesaplarındaki yönetilmeyen tüm diskler inceler. Betik bulur yönetilmeyen disk ile ne zaman bir **LeaseStatus** özelliğini **kilitli değil**, komut dosyası disk eklenmemiş olduğunu belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak betiği çalıştırmak **deleteUnattachedVHDs** 0 değişken. Bu eylem, bulma ve görüntüleme tüm eklenmemiş yönetilmeyen VHD'ler sağlar.
>
>Tüm eklenmemiş disk gözden geçirdikten sonra betiğini yeniden çalıştırın ve ayarlama **deleteUnattachedVHDs** 1 değişken. Bu eylem tüm eklenmemiş yönetilmeyen VHD'ler silmenize olanak sağlar.

```azurepowershell-interactive
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=0
$storageAccounts = Get-AzStorageAccount
foreach($storageAccount in $storageAccounts){
    $storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value
    $context = New-AzStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey
    $containers = Get-AzStorageContainer -Context $context
    foreach($container in $containers){
        $blobs = Get-AzStorageBlob -Container $container.Name -Context $context
        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
                    if($deleteUnattachedVHDs -eq 1){
                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                        $_ | Remove-AzStorageBlob -Force
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

Daha fazla bilgi için [depolama hesabını Sil](../../storage/common/storage-create-storage-account.md) ve [tanımlamak yalnız bırakılmış diskler PowerShell kullanılarak](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)
