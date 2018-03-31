---
title: Azure kotaları vCPU | Microsoft Docs
description: Azure için vCPU kotaları hakkında bilgi edinin.
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: Drewm3
manager: timlt
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: 05d84e4b8c0ac84db5d0784e64071931408ec1d6
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

Sanal makineler ve sanal makine ölçek kümeleri vCPU kotaları her bölgede her abonelik için iki katmanlarda düzenlenir. İlk katmanı toplam bölgesel Vcpu'lar ve çeşitli VM boyutu ailesi çekirdek standart D Ailesi Vcpu'lar gibi ikinci katman şeklindedir. Yeni VM dilediğiniz zaman Vcpu'lar dağıtılan yeni dağıtılan VM belirli VM boyutu ailesi vCPU kotası veya toplam bölgesel vCPU kota aşmamalıdır. Kotalarda birini aşılırsa, VM dağıtımı verilmez. Sanal makine bölgede genel sayısı için bir kota yoktur. Her bu kotalar ayrıntıları görülebilir **kullanım + kotaları** bölümünü **abonelik** sayfasındaki [Azure portal](https://portal.azure.com), veya değerlerini kullanarak sorgulama yapabilirsiniz PowerShell.

 
## <a name="check-usage"></a>Kullanımını denetleyin

Kullanabileceğiniz [Get-AzureRmVMUsage](/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet, kota kullanımını denetleyin.

```azurepowershell-interactive
Get-AzureRmVMUsage -Location "East US"
```

Çıktı aşağıdakine benzer görünecektir:

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count

```


## <a name="reserved-vm-instances"></a>Ayrılmış VM Örnekleri
Tek bir aboneliği kapsamlıdır, ayrılmış VM örnekleri yeni en boy vCPU kotaları ekler. Bu değerler, abonelikte dağıtılabilir belirtilen boyut örnekleri sayısını tanımlar. Bu kota ayrılmış örnekler abonelikte dağıtılabilir olduğundan emin olmak için ayrılmış emin olmak için kota sistemindeki yer tutucu olarak çalışırlar. Örneğin, belirli bir aboneliği 10 Standard_D1 ayrılmış varsa kullanımlar için ayrılmış örnekler Standard_D1 sınırlamak örnekleri 10 olacaktır. Bu, her zaman en az 10 Vcpu'lar Standard_D1 örnekleri için kullanılacak toplam bölgesel Vcpu'lar kota bulunan ve en az 10 Vcpu'lar Standard_D1 örnekleri için kullanılacak standart D Ailesi vCPU kota bulunan emin olmak Azure neden olur.

Tek bir abonelik RI satın almak için kota artışı gerekirse, yapabilecekleriniz [kota artışı isteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) aboneliğinizi üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

Faturalama ve kotaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
