---
title: Azure için vCPU kotaları | Microsoft Docs
description: VCPU kotaları hakkında bilgi edinmek için Azure.
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 8912387583c24945de22bcb029d40e4d4766b1fb
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719869"
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

VCPU kotaları sanal makineler ve sanal makine ölçek kümeleri için her bölgede her abonelik için iki katmanda düzenlenir. İlk katmandır toplam bölgesel vcpu sayısı ve çeşitli VM boyutu ailesi çekirdek D serisi Vcpu gibi ikinci katmandır. Dilediğiniz zaman yeni bir VM Vcpu dağıtılmış VM vCPU kotası VM boyutu ailesi veya toplam bölgesel vCPU kotası aşmamalıdır. Kotalarda biri aşılırsa, VM dağıtımı izin verilmiyor. Genel bölgesindeki sanal makine sayısı kotasının yoktur. Bu kotalar ayrıntı görülebilir **kullanım ve kotalar** bölümünü **abonelik** sayfasını [Azure portalı](https://portal.azure.com), ya da değerlerini kullanarak sorgulayabilirsiniz. PowerShell.

 [!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)] 
 
## <a name="check-usage"></a>Kullanımı denetleme

Kullanabileceğiniz [Get-AzVMUsage](https://docs.microsoft.com/powershell/module/az.compute/get-azvmusage) cmdlet'ini kota kullanımınızı kontrol edin.

```azurepowershell-interactive
Get-AzVMUsage -Location "East US"
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
VM boyutu esneklik olmadan tek bir abonelik için kapsamlı ayrılmış VM örneklerini yeni bir boyut için vCPU kotaları ekler. Bu değerler, abonelikte dağıtılabilir belirtilen boyut örnek sayısını tanımlar. Ayrılmış VM örnekleri abonelikte dağıtılabilir emin olmak için bu kota ayrılmış emin olmak için kota sisteminde yer tutucu olarak çalışırlar. Örneğin, belirli bir aboneliğe ayrılmış VM örnekleri için kullanım sınırı 10 işler için standart_d1 sahipse işler için standart_d1 ayrılmış VM örnekleri, 10 olacaktır. Bu, kullanılabilir her zaman en az 10 Vcpu işler için standart_d1 örnekleri için kullanılacak toplam bölgesel Vcpu kotası, ve en az 10 Vcpu işler için standart_d1 örnekleri için kullanılacak standart D Ailesi vCPU kotası kullanılabilir sağlamak için Azure'da neden olur.

Tek bir abonelik RI satın almak için bir kota artırım gerekiyorsa yapabilecekleriniz [bir kota artırım talebinde](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) aboneliğinizde.

## <a name="next-steps"></a>Sonraki adımlar

Faturalama ve kotalar hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
