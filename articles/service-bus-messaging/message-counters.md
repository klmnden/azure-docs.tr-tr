---
title: Azure Service Bus ileti sayısı | Microsoft Docs
description: Azure Service Bus ileti sayısı alınamıyor.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: adfd8c5849cfee69805715378a3f56ec9f685b00
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60403967"
---
# <a name="message-counters"></a>İleti sayaçları

Azure Resource Manager ve Service Bus kuyrukları ile aboneliklerinden içinde tutulan ileti sayısı alabilirsiniz [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) .NET Framework SDK'sı API'leri.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell ile sayısı şu şekilde elde edebilirsiniz:

```powershell
(Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>İleti sayısı ayrıntıları

Etkin mesaj sayısı bilmek, bir sıra yukarı ne şu anda dağıtılmış olan daha işlemek için daha fazla kaynak gerektiren bir biriktirme listesi yapıları olup olmadığını belirlemede yararlıdır. Aşağıdaki sayacı ayrıntılarını kullanılabilir [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) sınıfı:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): Etkin Durum ve teslim için hazır olan iletiler bir kuyrukta veya abonelik.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): Geçerliliğini yitirmiş kuyruk iletileri.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): Zamanlanmış durum iletileri.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): Başka bir kuyruk veya konuda aktarımı başarısız oldu ve aktarım sahipsiz sıraya taşınmış iletileri.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): Başka bir kuyruk veya konuda aktarımı bekleyen iletileri.

Kaynakları kuyruk uzunluğuna göre ölçeklendirmek bir uygulama isterse, bunu bir ölçülen yapmanız gerektiğini uygun bir hızda. İleti sayaçları alımını ileti Aracısı içinde pahalı bir işlemdir ve sık sık doğrudan ve olumsuz yürütme varlık performansını etkiler.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
