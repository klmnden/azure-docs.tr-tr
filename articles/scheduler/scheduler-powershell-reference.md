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
ms.openlocfilehash: 5c80e86699d671994a0989b99c0f97ebe2680592
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045006"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Azure Scheduler PowerShell cmdlet'leri başvurusu

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Scheduler işleri ve iş koleksiyonları oluşturmak ve yönetmek için betikleri oluşturmak için PowerShell cmdlet'lerini kullanabilirsiniz. Bu makalede ana listeler [için Azure Scheduler PowerShell cmdlet'leri](/powershell/module/azurerm.scheduler) kendi başvuru makalelerin bağlantıları ile. Azure aboneliğiniz için Azure PowerShell'i yüklemek için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview). Hakkında daha fazla bilgi için [Azure Resource Manager cmdlet'leri](/powershell/azure/overview), bkz: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

| Cmdlet | Açıklama |
|--------|-------------|
| [AzSchedulerJobCollection devre dışı bırak](/powershell/module/az.scheduler/disable-azschedulerjobcollection) |İş koleksiyonu devre dışı bırakır. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/az.scheduler/enable-azschedulerjobcollection) |İş koleksiyonunu etkinleştirir. |
| [Get-AzSchedulerJob](/powershell/module/az.scheduler/get-azschedulerjob) |Scheduler işleri alır. |
| [Get-AzSchedulerJobCollection](/powershell/module/az.scheduler/get-azschedulerjobcollection) |İş koleksiyonları alır. |
| [Get-AzSchedulerJobHistory](/powershell/module/az.scheduler/get-azschedulerjobhistory) |İş geçmişini alır. |
| [Yeni AzSchedulerHttpJob](/powershell/module/az.scheduler/new-azschedulerhttpjob) |Bir HTTP işi oluşturur. |
| [Yeni AzSchedulerJobCollection](/powershell/module/az.scheduler/new-azschedulerjobcollection) |İş koleksiyonu oluşturur. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/az.scheduler/new-azschedulerservicebusqueuejob) | Bir Service Bus kuyruğu işi oluşturur. |
| [Yeni AzSchedulerServiceBusTopicJob](/powershell/module/az.scheduler/new-azschedulerservicebustopicjob) |Bir Service Bus konu İş oluşturur. |
| [Yeni AzSchedulerStorageQueueJob](/powershell/module/az.scheduler/new-azschedulerstoragequeuejob) |Bir depolama kuyruğu işi oluşturur. |
| [Remove-AzSchedulerJob](/powershell/module/az.scheduler/remove-azschedulerjob) |Scheduler işi kaldırır. |
| [Remove-AzSchedulerJobCollection](/powershell/module/az.scheduler/remove-azschedulerjobcollection) |İş koleksiyonu kaldırır. |
| [Set-AzSchedulerHttpJob](/powershell/module/az.scheduler/set-azschedulerhttpjob) |Bir HTTP Zamanlayıcı işi değiştirir. |
| [Set-AzSchedulerJobCollection](/powershell/module/az.scheduler/set-azschedulerjobcollection) |İş koleksiyonu değiştirir. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/az.scheduler/set-azschedulerservicebusqueuejob) |Bir Service Bus kuyruğu işi değiştirir. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/az.scheduler/set-azschedulerservicebustopicjob) |Bir Service Bus konu işi değiştirir. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/az.scheduler/set-azschedulerstoragequeuejob) |Bir depolama kuyruğu işi değiştirir. |
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
