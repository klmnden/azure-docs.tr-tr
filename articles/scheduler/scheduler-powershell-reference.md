---
title: PowerShell cmdlet başvuru - Azure Zamanlayıcı
description: Azure Zamanlayıcı için PowerShell cmdlet'leri hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: a06439150ac255e7b436082ecc88702bf0c1dec1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991105"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Azure Scheduler PowerShell cmdlet'leri başvurusu

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) devre dışı bırakılıyor Azure Scheduler değiştiriyor. İşleri zamanlamak için [Azure Logic Apps'i deneyin](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Scheduler işleri ve iş koleksiyonları oluşturmak ve yönetmek için betikleri oluşturmak için PowerShell cmdlet'lerini kullanabilirsiniz. Bu makalede ana listeler [için Azure Scheduler PowerShell cmdlet'leri](/powershell/module/azurerm.scheduler) kendi başvuru makalelerin bağlantıları ile. Azure aboneliğiniz için Azure PowerShell'i yüklemek için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview). Hakkında daha fazla bilgi için [Azure Resource Manager cmdlet'leri](/powershell/azure/overview), bkz: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

| Cmdlet | Açıklama |
|--------|-------------|
| [AzureRmSchedulerJobCollection devre dışı bırak](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |İş koleksiyonu devre dışı bırakır. |
| [AzureRmSchedulerJobCollection etkinleştir](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |İş koleksiyonunu etkinleştirir. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Scheduler işleri alır. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |İş koleksiyonları alır. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |İş geçmişini alır. |
| [Yeni AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Bir HTTP işi oluşturur. |
| [Yeni AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |İş koleksiyonu oluşturur. |
| [Yeni AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Bir Service Bus kuyruğu işi oluşturur. |
| [Yeni AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Bir Service Bus konu İş oluşturur. |
| [Yeni AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi oluşturur. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Scheduler işi kaldırır. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |İş koleksiyonu kaldırır. |
| [Set-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Bir HTTP Zamanlayıcı işi değiştirir. |
| [Set-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |İş koleksiyonu değiştirir. |
| [Set-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Bir Service Bus kuyruğu işi değiştirir. |
| [Set-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Bir Service Bus konu işi değiştirir. |
| [Set-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi değiştirir. |
||| 

Daha fazla ayrıntı için bu cmdlet'lerden herhangi birini çalıştırabilirsiniz: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Scheduler nedir?](scheduler-intro.md)
* [Kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Oluşturma ve zamanlama ilk işinizi - Azure portalı](scheduler-get-started-portal.md)
* [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)
