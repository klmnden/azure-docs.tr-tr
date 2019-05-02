---
title: Bulup silmesine eklenmemiş Azure yönetilen ve yönetilmeyen disk | Microsoft Docs
description: Nasıl bulun ve Azure CLI kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD/sayfa BLOB'ları) diskleri silin.
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 21c15a943974b80469eb9bd71cbaf11a7bc34b4a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64721841"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen disk silme
Bir sanal makine (VM), azure'daki varsayılan olarak, sildiğinizde VM'ye bağlı tüm diskleri silinmez. Bu özellik VM'ler yanlışlıkla silinmesini nedeniyle veri kaybını önlemeye yardımcı olur. VM silindikten sonra eklenmemiş disk için ödeme yapmaya devam edersiniz. Bu makalede bulup eklenmemiş tüm diskleri silin ve gereksiz maliyetleri azaltın gösterilmektedir. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskler: Bulma ve eklenmemiş disk silme 

Aşağıdaki komut dosyasını arar eklenmemiş [yönetilen diskler](managed-disks-overview.md) değerini inceleme tarafından **ManagedBy** özelliği. Bir VM'ye yönetilen disk eklendiğinde **ManagedBy** özelliği, VM kaynak Kimliğini içerir. Yönetilen disk eklenmemiş, olduğunda **ManagedBy** özelliği null. Betik bir Azure aboneliğindeki tüm yönetilen diskler inceler. Betik bir yönetilen disk ile ne zaman bulur **ManagedBy** özelliği null, betik disk eklenmemiş belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak betiği çalıştırmak **deleteUnattachedDisks** 0 değişken. Bu eylem, bulma ve görüntüleme eklenmemiş tüm yönetilen diskler sağlar.
>
>Tüm eklenmemiş disk gözden geçirdikten sonra betiğini yeniden çalıştırın ve ayarlama **deleteUnattachedDisks** 1 değişken. Bu eylem eklenmemiş tüm yönetilen diskler silmenize olanak sağlar.
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

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: Bulma ve eklenmemiş disk silme 

Yönetilmeyen diskler olarak depolanmış VHD dosyaları, [sayfa blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası değerini inceleyerek eklenmemiş yönetilmeyen diskler (sayfa blobları) arar **LeaseStatus** özelliği. Yönetilmeyen disk bir sanal makineye bağlı olduğu **LeaseStatus** özelliği **kilitli**. Yönetilmeyen disk eklenmemiş, olduğunda **LeaseStatus** özelliği **kilitli değil**. Betik bir Azure aboneliğindeki tüm Azure depolama hesaplarındaki yönetilmeyen tüm diskler inceler. Betik bulur yönetilmeyen disk ile ne zaman bir **LeaseStatus** özelliğini **kilitli değil**, komut dosyası disk eklenmemiş olduğunu belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak betiği çalıştırmak **deleteUnattachedVHDs** 0 değişken. Bu eylem, bulma ve görüntüleme tüm eklenmemiş yönetilmeyen VHD'ler sağlar.
>
>Tüm eklenmemiş disk gözden geçirdikten sonra betiğini yeniden çalıştırın ve ayarlama **deleteUnattachedVHDs** 1 değişken. Bu eylem tüm eklenmemiş yönetilmeyen VHD'ler silmenize olanak sağlar.
>

```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=0

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

[Depolama hesabını Sil](../../storage/common/storage-create-storage-account.md)



