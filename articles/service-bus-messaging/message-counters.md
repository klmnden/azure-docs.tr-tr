---
title: "Azure Service Bus ileti sayısı | Microsoft Docs"
description: "Azure Service Bus iletilerinin sayısını alır."
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: sethm
ms.openlocfilehash: f5d2aa551bbe77a66459907cf5cd1313bb907981
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="message-counters"></a>İleti sayaçları

Azure Resource Manager ve Service Bus kullanarak kuyruklar ve abonelikler tutulan ileti sayısını alabilir [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) .NET Framework SDK API'leri.

PowerShell ile aşağıdaki gibi sayımı elde edebilirsiniz:

```powershell
(Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>İleti sayısı ayrıntıları

Etkin ileti sayısı bilerek ne şu anda dağıtılmış daha işlemek için daha fazla kaynak gerektiren bir biriktirme listesi bir kuyruk oluşturur olup olmadığını belirlemek kullanışlıdır. Aşağıdaki sayacı ayrıntılarını kullanılabilir [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) sınıfı:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): etkin durum ve teslimat için hazır olan sıra veya abonelik iletilerinde.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): teslim edilemeyen sıradaki iletiler.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): zamanlanmış durum iletileri.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): başka bir kuyruk veya konu aktarımı başarısız oldu ve aktarım sahipsiz sıraya taşınmış iletiler.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): başka bir kuyruk veya konu içine aktarma bekleyen iletiler.

Uygulama kaynakları sırasının uzunluğuna göre ölçeklendirme isterse, bunu bir çok ölçülen yapmanız gerektiğini hızınıza. İleti sayaçları alımını bir ileti Aracısı içinde pahalı bir işlemdir ve sık sık doğrudan ve olumsuz yürütme varlık performansını etkiler.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)