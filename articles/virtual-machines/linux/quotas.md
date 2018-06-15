---
title: Azure kotaları vCPU | Microsoft Docs
description: Azure için vCPU kotaları hakkında bilgi edinin.
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: a880ee18bb13b2cd8471cc58157469555397b872
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716525"
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

Sanal makineler ve sanal makine ölçek kümeleri vCPU kotaları her bölgede her abonelik için iki katmanlarda düzenlenir. İlk katmanı toplam bölgesel Vcpu'lar ve çeşitli VM boyutu ailesi çekirdek D-serisi Vcpu'lar gibi ikinci katman şeklindedir. Yeni VM dilediğiniz zaman Vcpu'lar dağıtılmış VM VM boyutu ailesi vCPU kotası veya toplam bölgesel vCPU kota aşmamalıdır. Kotalarda birini aşılırsa, VM dağıtımı izin verilmiyor. Sanal makine bölgede genel sayısı için bir kota yoktur. Her bu kotalar ayrıntıları görülebilir **kullanım + kotaları** bölümünü **abonelik** sayfasındaki [Azure portal](https://portal.azure.com), ya da Azure kullanarak değerleri için sorgu CLI.


## <a name="check-usage"></a>Kullanımını denetleyin

Kota kullanımı kullanarak denetleyebilirsiniz [az vm listesi-kullanım](/cli/azure/vm#az_vm_list_usage).

```azurecli-interactive
az vm list-usage --location "East US" -o table
```

Çıktı aşağıdakine benzer görünmelidir:


```
Name                                CurrentValue    Limit
--------------------------------  --------------  -------
Availability Sets                              0     2000
Total Regional vCPUs                          29      100
Virtual Machines                               7    10000
Virtual Machine Scale Sets                     0     2000
Standard DSv3 Family vCPUs                     8      100
Standard DSv2 Family vCPUs                     3      100
Standard Dv3 Family vCPUs                      2      100
Standard D Family vCPUs                        8      100
Standard Dv2 Family vCPUs                      8      100
Basic A Family vCPUs                           0      100
Standard A0-A7 Family vCPUs                    0      100
Standard A8-A11 Family vCPUs                   0      100
Standard DS Family vCPUs                       0      100
Standard G Family vCPUs                        0      100
Standard GS Family vCPUs                       0      100
Standard F Family vCPUs                        0      100
Standard FS Family vCPUs                       0      100
Standard Storage Managed Disks                 5    10000
Premium Storage Managed Disks                  5    10000
```

## <a name="reserved-vm-instances"></a>Ayrılmış VM Örnekleri
Tek bir aboneliği kapsamlıdır, ayrılmış VM örnekleri yeni en boy vCPU kotaları ekler. Bu değerler, abonelikte dağıtılabilir belirtilen boyut örnekleri sayısını tanımlar. Bu kota ayrılmış örnekler abonelikte dağıtılabilir olduğundan emin olmak için ayrılmış emin olmak için kota sistemindeki yer tutucu olarak çalışırlar. Örneğin, belirli bir aboneliği 10 Standard_D1 ayrılmış varsa kullanımlar için ayrılmış örnekler Standard_D1 sınırlamak örnekleri 10 olacaktır. Bu, her zaman en az 10 Vcpu'lar Standard_D1 örnekleri için kullanılacak toplam bölgesel Vcpu'lar kota bulunan ve en az 10 Vcpu'lar Standard_D1 örnekleri için kullanılacak standart D Ailesi vCPU kota bulunan emin olmak Azure neden olur.

Kota artışı ya da tek bir abonelik RI satın almanız gerekiyorsa, yapabilecekleriniz [kota artışı isteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) aboneliğinizi üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

Faturalama ve kotaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
