---
title: Azure Service Bus ileti erteleme | Microsoft Docs
description: Service Bus iletileri teslim ertele
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
ms.openlocfilehash: 11ea10f1deba5a21b98dea875a1b7dc94998aa00
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60402743"
---
# <a name="message-deferral"></a>İleti erteleme

Ne zaman bir kuyruk veya abonelik istemci işlenecek istekli olup, ancak hangi işleme uygulama içinde özel durumlar nedeniyle şu anda mümkün olmayan için "daha ilerideki bir noktaya iletiye alınmasını ertelemek" seçeneğine sahip bir ileti alır. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.

Erteleme senaryoları işleme iş akışı için özel olarak oluşturulmuş bir özelliktir. İş akışı çerçeveler, belirli bir sırayla işlenmesi için belirli işlemleri gerektirebilir ve diğer iletiler tarafından verilmediği için önceden belirlenmiş bir önceki iş tamamlanana kadar bazı alınan iletilerin işlenmesini erteleme gerekebilir.

Basit bir görsel eşleşen satınalma sırası yayılmadan önce bir dış ödeme sağlayıcısından ödeme bildirim sistem deposu önünden yerine getirme sistemine göründüğü sırası işleme bir sırayı örnektir. Bu durumda, bir sıra ile ilişkilendirmek istediğiniz kadar ödeme bildirim işleme yerine getirme sistem erteleyebilirsiniz. Burada farklı kaynaklardaki iletilerin bir iş akışı gelişmesini, randevu senaryolarda gerçek zamanlı yürütme sırası gerçekten doğru olabilir, ancak sonuçlarını yansıtma iletileri sıralamaya gelebilir.

Sonuç olarak, bunlar, bu iletileri güvenli bir şekilde izin hangi işleme ihtiyaçları için ileti deposundaki bırakarak işlenebilir siparişe varış sırasındaki iletilere sipariş erteleme kolaylaştırır.

## <a name="message-deferral-apis"></a>İleti erteleme API'leri

API [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) veya [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) .NET Framework istemci [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) .NET Standard istemci ve **mesageReceiver.defer** veya **messageReceiver.deferSync** Java istemci. 

Ertelenmiş ileti ana kuyruk diğer etkin iletileriniz (farklı bir alt kuyruk Canlı teslim edilemeyen iletiler) ile birlikte kalır, ancak bunlar artık normal alma/ReceiveAsync işlevleri kullanılarak alınabilir. Ertelenmiş ileti aracılığıyla bulunması [iletilere gözatma](message-browsing.md) uygulama bunları izini kaybederse.

Ertelenmiş ileti almak için sahibi hatırlamak için sorumlu [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) olarak bunu erteler. Ertelenmiş ileti sıra numarası bilir herhangi bir alıcıya daha sonra açıkça sahip ileti alabilir `Receive(sequenceNumber)`. Kuyruklar için kullanabileceğiniz [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), konu abonelikleri kullanan [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient).

Bir ileti işlenemiyor, çünkü bu iletiyi işlemek için belirli bir kaynağa geçici olarak kullanılamıyor, ancak ileti işleme değil summarily askıya alınır, bu iletiyi tarafında birkaç dakikalığına koymak için bir unutmayın yöntemdir  **SequenceNumber** içinde bir [zamanlanmış iletileri](message-sequencing.md) birkaç dakika içinde yayınlanabilir ve zamanlanmış ileti geldiğinde, ertelenmiş ileti yeniden almak için. Tüm işlemler için bir veritabanı bir ileti işleyicisi bağlıdır ve bu veritabanı geçici olarak devre dışı ise, bu erteleme kullanmaz, bunun yerine iletileri alma askıya veritabanı yeniden kullanılabilir olana kadar toptan.


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
