---
title: "Azure Service Bus ileti gözatma | Microsoft Docs"
description: "Gözat ve Service Bus iletilere göz"
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
ms.date: 09/27/2017
ms.author: sethm
ms.openlocfilehash: b0bc1ef7570ccac07975e2560a1d0501d3cde2b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="message-browsing"></a>İleti gözatma

("Atmayı") gözatma ileti bir kuyruk veya genellikle tanılama için abonelik bulunan ve hata ayıklama amacıyla tüm iletileri numaralandırmak bir istemci sağlar.

Peek operations mevcut tüm iletileri kuyruğa veya abonelik iletisi günlükte değil yalnızca ile hemen alım için kullanılabilir dönmek *Receive()* veya *OnMessage()* döngü. *Durumu* her iletinin özelliği bildirir, ileti etkin olup (alınabilmesi kullanılabilir), ertelenmiş (erteleme [henüz Belirlenmedi bağlantı] bakın) veya zamanlanmış (zamanlanmış iletileri [henüz Belirlenmedi bağlantı] bakın).

Tüketilen ve süresi dolan iletileri Temizlenen bir zaman uyumsuz "Çöp toplama" çalıştırın ve mutlaka tam iletileri ne zaman sona ve bu nedenle gözlem süresi zaten dolmuş olan ve veya eski-lettered alma işlemi sırasında kaldırılır iletiler gerçekten döndürebilir işlemi, kuyruk veya abonelik sonraki çağrılır.

Bu, özellikle ertelenmiş iletileri sıradan kurtarmayı denediğinizde göz önünde bulundurmanız önemlidir. Kendisi için bir ileti [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) anlık geçtikten gözlem tarafından bile döndürülmüyor olduğunda artık normal alma diğer herhangi bir yöntem için uygun değil. Peek günlük geçerli durumunu yansıtan bir tanılama aracı olduğundan bu iletilerini döndürmek öğrenirken kalır.

Peek ayrıca kilitlenmiş ve diğer alıcılar tarafından işlenmekte, ancak henüz tamamlanmamış iletileri döndürür. Peek bağlantısı kesilmiş bir anlık görüntü döndürdüğünden, ancak, bir ileti kilit durumunu Aranan iletilerde uyulması olamaz ve [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) ve [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) özellikleri throw bir [ InvalidOperationException](/dotnet/api/system.invalidoperationexception) çalıştığında uygulama bunları okuyun.

## <a name="peek-apis"></a>Peek API'leri

[Gözlem/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) ve [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) yöntemleri mevcut tüm .NET ve Java istemci kitaplıkları ve tüm alıcı nesnelerde: **MessageReceiver**, **MessageSession**, **QueueClient**, ve **SubscriptionClient**. Tüm Kuyruklar ve abonelikleri ve bunların ilgili sıralarındaki gözlem çalışır.

Peek yöntemi art arda çağrıldığında, kuyruk veya abonelik günlüğünde, en yüksek için en düşük kullanılabilir sıra numarasından sıra numarası sırada mevcut tüm iletileri numaralandırır. İletilerin sıraya alındığı sırada budur; hangi iletileri sonunda alınması sırası değildir.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) birden fazla ileti alır ve bunları sabit olarak döndürür. Hiçbir ileti varsa, numaralandırma nesnesi boş, boş değil.

Bir aşırı yüklemesini yöntemiyle oluşturmak bir [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) veren başlatın ve sonra daha fazla Numaralandırılacak parametresiz yöntemi aşırı yüklemesini çağırın. **PeekBatch** eşdeğer çalışır, ancak aynı anda bir dizi ileti alır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)