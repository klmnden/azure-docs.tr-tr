---
title: Bulma ve eklenmemiş Azure NIC'ler silme | Microsoft Docs
description: Bul ve Azure CLI 2.0 olan VM'ler için bağlı olmayan Azure NIC'ler silme
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: iainfou
ms.openlocfilehash: c730866fe73305a37b37038699a7f729085a16aa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-find-and-delete-unattached-network-interface-cards-nics-for-azure-vms"></a>Azure VM'ler için nasıl bulacağınızı ve eklenmemiş ağ arabirimini silmek kartları (NIC)
Azure'da sanal makine (VM) sildiğinizde, ağ arabirimi kartlarıyla (NIC) varsayılan olarak silinmez. Oluşturma ve birden çok VM silme kullanılmayan NIC'ler iç IP adresi kiraları kullanmaya devam edin. Diğer VM NIC'ler oluştururken, alt ağın adres alanındaki IP kira alınamıyor olabilir. Bu makalede bulma ve eklenmemiş NIC'ler silme gösterilmektedir.

## <a name="find-and-delete-unattached-nics"></a>Bulma ve eklenmemiş NIC'ler silme

*VirtualMachine* özelliği için bir NIC NIC eklendiği VM kimliği ve kaynak grubu depolar. Aşağıdaki komut dosyasını bir Abonelikteki tüm NIC'ler döngü ve denetler *virtualMachine* özelliği null. Bu özellik null ise, NIC bir VM öğesine bağlı değil.

Tüm eklenmemiş NIC'ler görüntülemek için sahip yüksek oranda önerilir ilk çalıştırmada komut dosyasıyla *deleteUnattachedNics* değişkenini *0*. Listesi çıkışı gözden geçirdikten sonra tüm eklenmemiş NIC'ler silmek için komut dosyası ile çalıştırma *deleteUnattachedNics* için *1*.

```azurecli
# Set deleteUnattachedNics=1 if you want to delete unattached NICs
# Set deleteUnattachedNics=0 if you want to see the Id(s) of the unattached NICs
deleteUnattachedNics=0

unattachedNicsIds=$(az network nic list --query '[?virtualMachine==`null`].[id]' -o tsv)
for id in ${unattachedNicsIds[@]}
do
   if (( $deleteUnattachedNics == 1 ))
   then

       echo "Deleting unattached NIC with Id: "$id
       az network nic delete --ids $id
       echo "Deleted unattached NIC with Id: "$id
   else
       echo $id
   fi
done
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve Azure sanal ağları yönetme hakkında daha fazla bilgi için bkz: [oluşturun ve VM ağlarını yönetmek](tutorial-virtual-network.md).
