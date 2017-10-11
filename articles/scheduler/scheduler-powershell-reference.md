---
title: "Scheduler PowerShell cmdlet'leri başvurusu"
description: "Scheduler PowerShell cmdlet'leri başvurusu"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 141919ab4506b3de4c4a69670dcf54c60ee6409c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-powershell-cmdlets-reference"></a>Scheduler PowerShell cmdlet'leri başvurusu
Aşağıdaki tabloda açıklar ve her Azure Scheduler ana cmdlet'lerin başvuru sayfası bağlar.

Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). 

Hakkında daha fazla bilgi için [Azure Resource Manager cmdlet'lerini](/powershell/azure/overview), bkz: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

| Cmdlet | Cmdlet açıklaması |
| --- | --- |
| [AzureRmSchedulerJobCollection devre dışı bırak](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |İş koleksiyonu devre dışı bırakır. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |İş koleksiyonu sağlar. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Zamanlayıcı işlerinin alır. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Alır koleksiyonları işi. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Alır geçmişi işi. |
| [AzureRmSchedulerHttpJob yeni](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Bir HTTP işi oluşturur. |
| [AzureRmSchedulerJobCollection yeni](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |İş koleksiyonu oluşturur. |
| [AzureRmSchedulerServiceBusQueueJob yeni](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |Bir hizmet veri yolu kuyruğu işi oluşturur. |
| [AzureRmSchedulerServiceBusTopicJob yeni](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Bir service bus konu işi oluşturur. |
| [AzureRmSchedulerStorageQueueJob yeni](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi oluşturur. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Scheduler işi kaldırır. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |İş koleksiyonu kaldırır. |
| [Set-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Bir zamanlayıcı HTTP işi değiştirir. |
| [Set-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |İş koleksiyonu değiştirir. |
| [Set-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Hizmet veri yolu kuyruğu işi değiştirir. |
| [Set-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Hizmet veri yolu konusu işi değiştirir. |
| [Set-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi değiştirir. |

Daha ayrıntılı bilgi için aşağıdaki cmdlet çalıştırabilirsiniz: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

