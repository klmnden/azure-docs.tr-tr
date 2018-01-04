---
title: "Azure kotaları vCPU | Microsoft Docs"
description: "Azure için vCPU kotaları hakkında bilgi edinin."
keywords: 
services: virtual-machines-linux
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: 8b99fd92d9031b7172e698cf5db1f776387bdfb9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

Sanal makineler ve sanal makine ölçek kümeleri vCPU kotaları her bölgede her abonelik için iki katmanlarda düzenlenir. İlk katmanı toplam bölgesel Vcpu'lar ve çeşitli VM boyutu ailesi çekirdek standart D Ailesi Vcpu'lar gibi ikinci katman şeklindedir. Yeni VM dilediğiniz zaman Vcpu'lar dağıtılan yeni dağıtılan VM belirli VM boyutu ailesi vCPU kotası veya toplam bölgesel vCPU kota aşmamalıdır. Kotalarda birini aşılırsa, VM dağıtımı verilmez. Sanal makine bölgede genel sayısı için bir kota yoktur. Her bu kotalar ayrıntıları görülebilir **kullanım + kotaları** bölümünü **abonelik** sayfasındaki [Azure portal](https://portal.azure.com), ya da Azure CLI kullanarak değerleri için sorgu .


## <a name="check-usage"></a>Kullanımını denetleyin

Kota kullanımı kullanarak denetleyebilirsiniz [az vm listesi-kullanım](/cli/azure/vm#az_vm_list_usage).

```azurecli-interactive
az vm list-usage --location "East US"
[
  …
  {
    "currentValue": 4,
    "limit": 260,
    "name": {
      "localizedValue": "Total Regional vCPUs",
      "value": "cores"
    }
  },
  {
    "currentValue": 4,
    "limit": 10000,
    "name": {
      "localizedValue": "Virtual Machines",
      "value": "virtualMachines"
    }
  },
  {
    "currentValue": 1,
    "limit": 2000,
    "name": {
      "localizedValue": "Virtual Machine Scale Sets",
      "value": "virtualMachineScaleSets"
    }
  },
  {
    "currentValue": 1,
    "limit": 10,
    "name": {
      "localizedValue": "Standard B Family vCPUs",
      "value": "standardBFamily"
    }
  },
```
## <a name="reserved-vm-instances"></a>Ayrılmış VM Örnekleri
Tek bir aboneliği kapsamlıdır, ayrılmış VM örnekleri yeni en boy vCPU kotaları ekler. Bu değerler, abonelikte dağıtılabilir belirtilen boyut örnekleri sayısını tanımlar. Bu kota ayrılmış örnekler abonelikte dağıtılabilir olduğundan emin olmak için ayrılmış emin olmak için kota sistemindeki yer tutucu olarak çalışırlar. Örneğin, belirli bir aboneliği 10 Standard_D1 ayrılmış varsa kullanımlar için ayrılmış örnekler Standard_D1 sınırlamak örnekleri 10 olacaktır. Bu, her zaman en az 10 Vcpu'lar Standard_D1 örnekleri için kullanılacak toplam bölgesel Vcpu'lar kota bulunan ve en az 10 Vcpu'lar Standard_D1 örnekleri için kullanılacak standart D Ailesi vCPU kota bulunan emin olmak Azure neden olur.

Kota artışı ya da tek bir abonelik RI satın almanız gerekiyorsa, yapabilecekleriniz [kota artışı isteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) aboneliğinizi üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

Faturalama ve kotaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
