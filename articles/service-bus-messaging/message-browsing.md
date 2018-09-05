---
title: Azure Service Bus ileti tarama | Microsoft Docs
description: Göz atma ve Service Bus iletiye göz atın
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
ms.date: 01/25/2018
ms.author: spelluru
ms.openlocfilehash: bafc08eae4a32f803f485097401a586a662f64e9
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700416"
---
# <a name="message-browsing"></a>İletilere göz atma

("Gözatma") gözatma ileti bulunan bir kuyrukta veya abonelikte, genellikle için tanılama ve hata ayıklama amacıyla tüm iletileri numaralandırmak bir Service Bus istemci sağlar.

Göz atma işlemleri mevcut tüm iletileri kuyruk veya abonelik ileti günlüğünde, yalnızca bu ile hemen alım için kullanılabilir dönüş `Receive()` veya `OnMessage()` döngü. `State` Her iletinin özelliği bildirir, ileti etkin olup (alınabilmesi kullanılabilir), [ertelenmiş](message-deferral.md), veya [zamanlanmış](message-sequencing.md).

Tüketilen ve süresi dolan iletileri Temizlenen bir zaman uyumsuz "Çöp toplama Çalıştır" ve mutlaka tam olarak ne zaman iletilerin süresi dolar ve bu nedenle `Peek` gerçekten de kaldırılacak ve süresi zaten dolmuş olan iletileri ya da eski lettered olduğunda döndürebilir alma işlemi, kuyruk veya abonelik sonra çağrılır.

Bu, özellikle bir kuyruktan ertelenmiş ileti kurtarmayı denediğinizde göz önünde bulundurmanız önemlidir. Kendisi için bir ileti [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) anında sonlanana gözlem tarafından bile döndürülüyor olduğunda artık diğer yollarla normal alma için uygun değil. Özet günlüğü geçerli durumunu yansıtan bir tanılama aracı olduğundan bu iletiler döndüren öğrenirken değerindedir.

Özet ayrıca kilitlenmiş ve diğer alıcılar tarafından işlenmekte, ancak henüz tamamlanmadı iletileri döndürür. Gözlem bağlantısı kesilmiş bir anlık görüntü döndürdüğünden, ancak iletinin kilit durumu baktı iletilerde nebyly nalezeny instance ve [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) ve [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) özellikleri throw bir [ InvalidOperationException](/dotnet/api/system.invalidoperationexception) çalıştığında uygulama bunları okuyun.

## <a name="peek-apis"></a>Özet API'leri

[Gözlem/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) ve [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) yöntemleri mevcut tüm .NET ve Java istemci kitaplıkları ve tüm alıcı nesneleri: **MessageReceiver**, **MessageSession**, **QueueClient**, ve **SubscriptionClient**. Tüm Kuyruklar, abonelikler ve bunların ilgili edilemeyen gözlem çalışır.

Art arda çağrıldığında, Peek metodunu dizisi sayısı, sırası en yüksek kullanılabilir dizisi en düşük sayıya kuyruk veya abonelik günlüğünde kayıtlı tüm iletileri numaralandırır. Bu, sıraya alınan iletileri olan sırasıdır; hangi iletileri sonunda alınması sırası değildir.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) birden çok ileti alır ve bunları bir sabit listesi döndürür. İleti kullanılabilir olduğunda sabit listesi nesnesi boş, null olur.

Ayrıca bir aşırı yüklemesini yöntemiyle çekirdeğini oluşturabileceğiniz bir [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) veren başlatın ve sonra daha fazla numaralandırma parametresiz yöntemi aşırı yüklemesini çağırın. **PeekBatch** eşdeğer işlevleri, ancak tek seferde bir dizi ileti alır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)