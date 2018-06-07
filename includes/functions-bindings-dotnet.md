---
title: include dosyası
description: include dosyası
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/23/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: a7ed3be6f3161b2152f88956b3eafe92232043aa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661173"
---
Aşağıdaki tabloda tetikleyici ve bir Azure işlevleri sınıf kitaplığında kullanılabilir bağlama öznitelikleri listeler. Ad alanındaki tüm öznitelikleridir `Microsoft.Azure.WebJobs`.

| Tetikleyici | Girdi | Çıktı|
|------   | ------    | ------  |
| [BlobTrigger](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---attributes)| [BLOB](../articles/azure-functions/functions-bindings-storage-blob.md#input---attributes)| [BLOB](../articles/azure-functions/functions-bindings-storage-blob.md#output---attributes)|
| [CosmosDBTrigger](../articles/azure-functions/functions-bindings-cosmosdb.md#trigger---attributes)| [DocumentDB](../articles/azure-functions/functions-bindings-cosmosdb.md#input---attributes)| [DocumentDB](../articles/azure-functions/functions-bindings-cosmosdb.md#output---attributes) |
| [EventHubTrigger](../articles/azure-functions/functions-bindings-event-hubs.md#trigger---attributes)|| [EventHub](../articles/azure-functions/functions-bindings-event-hubs.md#output---attributes) |
| [HTTPTrigger](../articles/azure-functions/functions-bindings-http-webhook.md#trigger---attributes)|||
| [QueueTrigger](../articles/azure-functions/functions-bindings-storage-queue.md#trigger---attributes)|| [Sırası](../articles/azure-functions/functions-bindings-storage-queue.md#output---attributes) |
| [ServiceBusTrigger](../articles/azure-functions/functions-bindings-service-bus.md#trigger---attributes)|| [ServiceBus](../articles/azure-functions/functions-bindings-service-bus.md#output---attributes) |
| [TimerTrigger](../articles/azure-functions/functions-bindings-timer.md#attributes) | ||
| |[ApiHubFile](../articles/azure-functions/functions-bindings-external-file.md)| [ApiHubFile](../articles/azure-functions/functions-bindings-external-file.md)|
| |[MobileTable](../articles/azure-functions/functions-bindings-mobile-apps.md#input---attributes)| [MobileTable](../articles/azure-functions/functions-bindings-mobile-apps.md#output---attributes) | 
| |[Tablo](../articles/azure-functions/functions-bindings-storage-table.md#input---attributes)| [Tablo](../articles/azure-functions/functions-bindings-storage-table.md#output---attributes)  | 
| ||[NotificationHub](../articles/azure-functions/functions-bindings-notification-hubs.md#attributes) |
| ||[SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md#attributes) |
| ||[Twilio](../articles/azure-functions/functions-bindings-twilio.md#attributes)| 