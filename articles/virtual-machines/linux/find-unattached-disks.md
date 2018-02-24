---
title: "Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen diskleri silme | Microsoft Docs"
description: "Nasıl bulmak ve Azure CLI kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD'ler/sayfa bloblarını) diskleri silin."
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
ms.openlocfilehash: 281e51783af05e02346b537f0abccdb2def38b31
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
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

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: bulma ve eklenmemiş diskleri silme 

Yönetilmeyen disklerdir olarak depolanan VHD dosyaları [sayfa blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası değerini inceleyerek eklenmemiş yönetilmeyen diskleri (sayfa bloblarını) görünüyor **LeaseStatus** özelliği. Yönetilmeyen bir diski bir VM öğesine bağlı olduğu **LeaseStatus** özelliği ayarlanmış **kilitli**. Yönetilmeyen bir disk eklenmemiş, olduğunda **LeaseStatus** özelliği ayarlanmış **kilitli değil**. Komut dosyası Azure depolama hesaplarında bir Azure aboneliği bulunan tüm yönetilmeyen diskleri inceler. Ne zaman betik bulur yönetilmeyen bir diskle bir **LeaseStatus** özelliğini **kilitli değil**, komut dosyası diskin eklenmemiş olup olmadığını belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak komut dosyasını çalıştırmak **deleteUnattachedVHDs** 0 değişken. Bu eylem, bulma ve tüm eklenmemiş yönetilmeyen VHD'leri görüntüleme sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve ayarlama **deleteUnattachedVHDs** değişken 1. Bu eylem tüm eklenmemiş yönetilmeyen VHD'leri silmenize olanak sağlar.
>

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



