---
title: "Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme | Microsoft Docs"
description: "Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen (VHD'ler/sayfa bloblarını) diskleri, Azure CLI kullanarak silme"
services: virtual-machines-linux
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: 9ada768cd4128b9dd6949b5a96c557496c6bb11c
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme
Azure'da sanal makine sildiğinizde, varsayılan olarak bağlı disk silinmez. Sanal makineler yanlışlıkla silinen kaynaklanan veri kaybının engeller, ancak eklenmemiş diskler için gereksiz yere ödeme geçin. Bul ve eklenmemiş tüm diskleri silin ve maliyet kaydetmek için bu makaleyi kullanın. 


## <a name="find-and-delete-unattached-managed-disks"></a>Bulma ve eklenmemiş yönetilen diskleri silme 

Aşağıdaki komut dosyası şirketiniz tarafından özelliğini kullanarak eklenmemiş yönetilen diskleri Bul gösterilmiştir.  Tüm yönetilen diskleri bir abonelikte döngü ve denetler *şirketiniz tarafından* eklenmemiş yönetilen diskleri bulmak için özellik NULL'dur. *Şirketiniz tarafından* özelliği, yönetilen bir diske bağlı sanal makine kaynak Kimliğini depolar. 

Yüksek oranda ilk çalıştırmada betik ayarlayarak öneririz *deleteUnattachedDisks* değişken eklenmemiş tüm diskleri görüntülemek için 0. Komut dosyasını ayarlayarak çalıştırmak eklenmemiş diskleri gözden geçirdikten sonra *deleteUnattachedDisks* eklenmemiş tüm diskleri silmek için 1.

 ```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done

```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Bulma ve eklenmemiş yönetilmeyen diskleri silme 

Yönetilmeyen disklerdir olarak depolanan VHD dosyaları [sayfa blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası LeaseStatus özelliğini kullanarak eklenmemiş yönetilmeyen diskleri (sayfa bloblarını) bulmak nasıl gösterir. Tüm depolama hesapları bir abonelikte ve denetler yönetilmeyen tüm diskleri aracılığıyla döngü *LeaseStatus* özelliği kilidi eklenmemiş yönetilmeyen disk bulunamadı. *LeaseStatus* yönetilmeyen bir diski bir sanal makineye bağlıysa özelliği için kilitli ayarlanır. 

Yüksek oranda ilk çalıştırmada betik ayarlayarak öneririz *deleteUnattachedVHDs* değişken eklenmemiş tüm diskleri görüntülemek için 0. Komut dosyasını ayarlayarak çalıştırmak eklenmemiş diskleri gözden geçirdikten sonra *deleteUnattachedVHDs* eklenmemiş tüm diskleri silmek için 1.


 ```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=1

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 

```

## <a name="next-steps"></a>Sonraki adımlar

[Depolama hesabını silme](../../storage/common/storage-create-storage-account.md)



