---
title: Bulma ve Azure eklenmemiş NIC'leri silme | Microsoft Docs
description: Bulma ve Azure CLI ile VM ekli değil Azure NIC Sil
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: dd4fcfe80818bd8e1e87851f4b5131aac73ceeb5
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671517"
---
# <a name="how-to-find-and-delete-unattached-network-interface-cards-nics-for-azure-vms"></a>Azure Vm'leri için nasıl bulup silmesine eklenmemiş ağ arabirimi kartları (NIC)
Ağ arabirim kartı (NIC), Azure sanal makineler'de (VM) sildiğinizde, varsayılan olarak silinmez. Oluşturma ve birden çok VM'yi silin, kullanılmayan NIC iç IP adresi kiraları kullanmaya devam edin. Diğer VM NIC oluşturma gibi bir alt ağın adres alanındaki IP kira alınamıyor olabilir. Bu makalede bulup silmesine eklenmemiş NIC'leri gösterilmektedir.

## <a name="find-and-delete-unattached-nics"></a>Eklenmemiş NIC’leri bulma ve silme

*VirtualMachine* özelliği için bir NIC kimliği ve kaynak grubu VM'nin NIC eklendiği depolar. Aşağıdaki betik, bir Abonelikteki tüm NIC'lerin döngü ve denetler *virtualMachine* özelliği null. Bu özellik null ise, NIC bir VM'ye bağlı değil.

Eklenmemiş NIC'leri görüntülemek için olan yüksek oranda öneririz ilk çalışmaya betiğiyle *deleteUnattachedNics* değişkenini *0*. Liste çıkışı gözden geçirdikten sonra tüm eklenmemiş NIC'leri silmek için komut dosyasını Çalıştır *deleteUnattachedNics* için *1*.

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

Oluşturma ve Azure sanal ağlarını yönetme hakkında daha fazla bilgi için bkz. [oluşturmak ve VM ağlarını yönetmek](tutorial-virtual-network.md).
