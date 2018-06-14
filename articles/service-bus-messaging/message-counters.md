---
title: Azure Service Bus ileti sayısı | Microsoft Docs
description: Azure Service Bus iletilerinin sayısını alır.
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: sethm
ms.openlocfilehash: e6524fe056ee2a1d81c9cccf257008b2369352b1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28197740"
---
# <a name="message-counters"></a>İleti sayaçları

Azure Resource Manager ve Service Bus kullanarak kuyruklar ve abonelikler tutulan ileti sayısını alabilir [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) .NET Framework SDK API'leri.

PowerShell ile aşağıdaki gibi sayımı elde edebilirsiniz:

```powershell
(Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>İleti sayısı ayrıntıları

Etkin ileti sayısı bilerek ne şu anda dağıtılmış daha işlemek için daha fazla kaynak gerektiren bir biriktirme listesi bir kuyruk oluşturur olup olmadığını belirlerken yararlıdır. Aşağıdaki sayacı ayrıntılarını kullanılabilir [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) sınıfı:

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