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
ms.openlocfilehash: 4b179c50af8b1ffc4313a49da978f178915ec9cc
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59489905"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Azure Scheduler PowerShell cmdlet'leri başvurusu

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Scheduler işleri ve iş koleksiyonları oluşturmak ve yönetmek için betikleri oluşturmak için PowerShell cmdlet'lerini kullanabilirsiniz. Bu makalede, başvuru makalelerin bağlantıları ile Azure Zamanlayıcı için ana PowerShell cmdlet'leri listelenmektedir. Azure aboneliğiniz için Azure PowerShell'i yüklemek için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview). Hakkında daha fazla bilgi için [Azure Resource Manager cmdlet'leri](/powershell/azure/overview), bkz: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

| Cmdlet | Açıklama |
|--------|-------------|
| [AzSchedulerJobCollection devre dışı bırak](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |İş koleksiyonu devre dışı bırakır. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |İş koleksiyonunu etkinleştirir. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Scheduler işleri alır. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |İş koleksiyonları alır. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |İş geçmişini alır. |
| [Yeni AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Bir HTTP işi oluşturur. |
| [Yeni AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |İş koleksiyonu oluşturur. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Bir Service Bus kuyruğu işi oluşturur. |
| [Yeni AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Bir Service Bus konu İş oluşturur. |
| [Yeni AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi oluşturur. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Scheduler işi kaldırır. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |İş koleksiyonu kaldırır. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Bir HTTP Zamanlayıcı işi değiştirir. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |İş koleksiyonu değiştirir. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Bir Service Bus kuyruğu işi değiştirir. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Bir Service Bus konu işi değiştirir. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi değiştirir. |
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
