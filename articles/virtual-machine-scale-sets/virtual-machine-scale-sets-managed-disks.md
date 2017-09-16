---
title: "Azure Sanal Makine Ölçek Kümeleri ile Yönetilen Diskler Kullanma | Microsoft Belgeleri"
description: "Sanal makine ölçek kümeleri ile yönetilen diskler kullanmanın yararlarını ve bunların nasıl kullanılacağını öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.translationtype: HT
ms.sourcegitcommit: a16daa1f320516a771f32cf30fca6f823076aa96
ms.openlocfilehash: 338144eb103c68c7fff407cbeccce11734c1c34b
ms.contentlocale: tr-tr
ms.lasthandoff: 09/02/2017

---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Azure VM ölçek kümeleri ve yönetilen diskler

Azure [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/), yönetilen disklere sahip sanal makineleri destekler. Ölçek kümeleri ile birlikte yönetilen disklerin kullanılması aşağıdakiler gibi birçok avantaj sunar:

* Ölçek kümesi VM’lerine ait işletim sistemi disklerini depolamak amacıyla önceden depolama hesapları oluşturmanıza ve bunları yönetmenize artık gerek kalmaz.

* Yönetilen veri disklerini ölçek kümesine ekleyebilirsiniz.

* Yönetilen diskler sayesinde ölçek kümesinin kapasitesi, platform görüntüsü tabanlıysa 1.000 VM'e veya özel bir görüntü tabanlıysa 300 VM'e kadar çıkabilir.

## <a name="get-started"></a>başlarken

Yönetilen disk ölçek kümelerini kullanmaya başlamanın basit bir yolu bunların birini Azure portalından dağıtmaktır. Daha fazla bilgi için [bu makaleye](./virtual-machine-scale-sets-portal-create.md) bakın. Başlamak için başka bir basit yol da [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) ile bir ölçek kümesi dağıtmaktır. Aşağıdaki örnekte, her biri 50 GB ve 100 GB’lık veri disklerine sahip 10 VM içeren Ubuntu tabanlı bir ölçek kümesinin nasıl oluşturulacağı gösterilmektedir:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Alternatif olarak, [Azure Hızlı Başlangıç Şablonları GitHub deposunda](https://github.com/Azure/azure-quickstart-templates) `vmss` içeren klasörlere bakarak ölçek kümesi dağıtan önceden oluşturulmuş şablon örneklerini inceleyebilirsiniz. Hangi şablonların yönetilen diskler kullanmakta olduğunu [bu listede](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen diskler hakkında daha fazla genel bilgi edinmek için [bu makaleye](../virtual-machines/windows/managed-disks-overview.md) bakın.

Bir Resource Manager şablonunu yönetilen diskler içeren ölçek kümeleri sağlayacak şekilde dönüştürmeyi öğrenmek için [bu makaleye](./virtual-machine-scale-sets-convert-template-to-md.md) bakın. Resource Manager şablonlarında yapılan değişikliklerin aynısı Azure REST API için de geçerlidir.

Ölçek kümeleri ile yönetilen veri diskleri kullanma hakkında daha fazla bilgi edinmek için [bu makaleye](./virtual-machine-scale-sets-attached-disks.md) bakın.

Büyük ölçek kümeleri ile çalışmaya başlamak için [bu makaleye](./virtual-machine-scale-sets-placement-groups.md) başvurun.



