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
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: f9cc157f25e70a8154614b659fb7e59b7fd06ed1
ms.contentlocale: tr-tr
ms.lasthandoff: 06/01/2017


---
<a id="azure-vm-scale-sets-and-managed-disks" class="xliff"></a>

# Azure VM ölçek kümeleri ve yönetilen diskler

Azure [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/), artık yönetilen disklere sahip sanal makineleri desteklemektedir. Ölçek kümeleri ile birlikte yönetilen disklerin kullanılması aşağıdakiler gibi birçok avantaj sunar:

* Ölçek kümesi VM’lerine ait işletim sistemi disklerini depolamak amacıyla önceden depolama hesapları oluşturmanıza ve bunları yönetmenize artık gerek kalmaz.

* Yönetilen veri disklerini ölçek kümesine ekleyebilirsiniz.

* Yönetilen diskler sayesinde ölçek kümesinin kapasitesi, platform görüntüsü tabanlıysa 1.000 VM'e veya özel bir görüntü tabanlıysa 100 VM'e kadar çıkabilir.

<a id="get-started" class="xliff"></a>

## Başlarken

Yönetilen disk ölçek kümelerini kullanmaya başlamanın basit bir yolu bunların birini Azure portalından dağıtmaktır. Daha fazla bilgi için [bu makaleye](./virtual-machine-scale-sets-portal-create.md) bakın. Başlamak için başka bir basit yol da [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) ile bir ölçek kümesi dağıtmaktır. Aşağıdaki örnekte, her biri 50 GB ve 100 GB’lık veri disklerine sahip 10 VM içeren Ubuntu tabanlı bir ölçek kümesinin nasıl oluşturulacağı gösterilmektedir:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Alternatif olarak, [Azure Hızlı Başlangıç Şablonları GitHub deposunda](https://github.com/Azure/azure-quickstart-templates) `vmss` içeren klasörlere bakarak ölçek kümesi dağıtan önceden oluşturulmuş şablon örneklerini inceleyebilirsiniz. Hangi şablonların yönetilen diskler kullanmakta olduğunu [bu listede](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) görebilirsiniz.

<a id="api-versions" class="xliff"></a>

## API sürümleri

Yönetilen diskler içeren ölçek kümeleri için Microsoft.Compute API'sinin `2016-04-30-preview` veya sonraki bir sürümü gerekir. Yönetilmeyen diskler içeren ölçek kümeleri, yönetilen disk desteğine sahip yeni API sürümlerinde bile şu andaki gibi çalışmaya devam edecektir. Ancak, yönetilmeyen disk içeren ölçek kümeleri bu yeni API sürümlerinde bile yönetilen disklerin avantajlarından yararlanamayacaktır.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Yönetilen diskler hakkında daha fazla genel bilgi edinmek için [bu makaleye](../storage/storage-managed-disks-overview.md) bakın.

Bir Resource Manager şablonunu yönetilen diskler içeren ölçek kümeleri sağlayacak şekilde dönüştürmeyi öğrenmek için [bu makaleye](./virtual-machine-scale-sets-convert-template-to-md.md) bakın. Resource Manager şablonlarında yapılan değişikliklerin aynısı Azure REST API için de geçerlidir.

Ölçek kümeleri ile yönetilen veri diskleri kullanma hakkında daha fazla bilgi edinmek için [bu makaleye](./virtual-machine-scale-sets-attached-disks.md) bakın.

Büyük ölçek kümeleri ile çalışmaya başlamak için [bu makaleye](./virtual-machine-scale-sets-placement-groups.md) başvurun.



