---
title: Azure için vCPU kotaları | Microsoft Docs
description: VCPU kotaları hakkında bilgi edinmek için Azure.
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 40fd91ed39731b95cf01a627e3178ab6cfaaa7d3
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670996"
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

VCPU kotaları sanal makineler ve sanal makine ölçek kümeleri için her bölgede her abonelik için iki katmanda düzenlenir. İlk katmandır toplam bölgesel vcpu sayısı ve çeşitli VM boyutu ailesi çekirdek D serisi Vcpu gibi ikinci katmandır. Dilediğiniz zaman yeni bir VM Vcpu dağıtılmış VM vCPU kotası VM boyutu ailesi veya toplam bölgesel vCPU kotası aşmamalıdır. Kotalarda biri aşılırsa, VM dağıtımı izin verilmiyor. Genel bölgesindeki sanal makine sayısı kotasının yoktur. Bu kotalar ayrıntı görülebilir **kullanım ve kotalar** bölümünü **abonelik** sayfasını [Azure portalı](https://portal.azure.com), ya da Azure kullanarak değerler için sorgu yapabilirsiniz CLI.


## <a name="check-usage"></a>Kullanımı denetleme

Kota kullanımı kullanarak denetleyebilirsiniz [az vm listesi kullanım](/cli/azure/vm).

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
VM boyutu esneklik olmadan tek bir abonelik için kapsamlı ayrılmış VM örneklerini yeni bir boyut için vCPU kotaları ekler. Bu değerler, abonelikte dağıtılabilir belirtilen boyut örnek sayısını tanımlar. Bu kota Azure ayırmaları abonelikte dağıtılabilir emin olmak için ayrılmış emin olmak için kota sisteminde yer tutucu olarak çalışırlar. Örneğin, belirli bir aboneliğe 10 işler için standart_d1 ayırmaları sahipse işler için standart_d1 ayırmalar için kullanım sınırı 10 olacaktır. Bu, kullanılabilir her zaman en az 10 Vcpu işler için standart_d1 örnekleri için kullanılacak toplam bölgesel Vcpu kotası, ve en az 10 Vcpu işler için standart_d1 örnekleri için kullanılacak standart D Ailesi vCPU kotası kullanılabilir sağlamak için Azure'da neden olur.

Kota artışı ya da tek bir abonelik RI satın almanız gerekiyorsa, şunları yapabilirsiniz [bir kota artırım talebinde](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) aboneliğinizde.

## <a name="next-steps"></a>Sonraki adımlar

Faturalama ve kotalar hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
