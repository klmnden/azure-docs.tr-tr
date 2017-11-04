---
title: Azure Service Bus ileti erteleme | Microsoft Docs
description: "Hizmet veri yolu İleti teslimini erteleme"
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
ms.date: 09/29/2017
ms.author: sethm
ms.openlocfilehash: f84b870de4b79399d5edc90284c9c56222156b5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="message-deferral"></a>İleti erteleme

Ne zaman bir sıra veya abonelik istemci işlemek istekli olup, ancak hangi işleme uygulama içinde özel durumlar nedeniyle şu anda olası değil için "bir noktaya iletiye alınmasını ertelemeyi" seçeneğine sahip bir ileti alır. İleti sıraya veya abonelik kalır ancak kenara ayarlanır.

Erteleme işleme senaryolarına iş akışı için özel olarak oluşturulmuş bir özelliktir. İş akışı çerçeveler belirli bir sırada işlenmesi için belirli işlemleri gerektirebilir ve bazı alınan iletileri işlemeyi diğer iletileri ile bilgilendirilir belirlenen önceki iş tamamlanana kadar erteleyin gerekebilir.

Bu basit bir görsel örnek eşleşen satın alma siparişi yayılmadan önce ödeme bildirim dış ödeme sağlayıcısından gelen bir sistemde deposu Önden yerine getirme sistemine göründüğü sırası işleme bir sırasıdır. Bu durumda, yerine getirme sistem oluncaya kadar bir sıra ile ilişkilendirmek için ödeme bildirim işleme erteleyebilirsiniz. Burada farklı kaynaklardan gelen iletileri bir iş akışı İleri sürücü, randevu senaryolarda gerçek zamanlı yürütme sırası gerçekten doğru olabilir, ancak sonuçlar yansıtma iletileri bozuk gelebilir.

Sonuç olarak, erteleme varış sipariş iletilerden, bunlar, bu iletileri güvenli bir şekilde izin hangi işleme ihtiyaçları için ileti deposundaki bırakarak işlenebilir siparişe yeniden sıralamada yardımcı olur.

## <a name="message-deferral-apis"></a>İleti erteleme API'leri

API [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) veya [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) .NET Framework istemci [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) .NET standart istemci ve **mesageReceiver.defer** veya **messageReceiver.deferSync** Java istemci. 

Diğer etkin iletileri (aksine, bir alt sırada Canlı teslim edilemeyen iletiler) yanı sıra ana sırasındaki ertelenmiş iletileri kalır, ancak bunlar artık normal alma/ReceiveAsync işlevler kullanılarak alınabilir. Ertelenmiş iletileri keşfedilecek aracılığıyla [ileti gözatma](message-browsing.md) uygulamanın bunları izini kaybederse.

Ertelenmiş ileti almak için "sahibi" hatırlamak için sorumludur [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) onu erteler gibi. Bilir alıcı **SequenceNumber** ertelenmiş bir ileti iletinin açıkça Receive(sequenceNumber) daha sonra alabilirsiniz.

Bir ileti işlenemedi, ileti işleme değil summarily askıya alınır ancak bu iletiyi işlemek için belirli bir kaynak geçici olarak kullanılamıyor çünkü tarafında için bir kaç dakika bu iletiyi almaya zarif bir şekilde anımsamasıvarsa,**SequenceNumber** içinde bir [zamanlanmış ileti](message-sequencing.md) birkaç dakika içinde gönderilen ve zamanlanmış ileti geldiğinde ertelenmiş ileti yeniden almak için. Tüm işlemler için bir veritabanı bir ileti işleyicisini bağlıdır ve bu veritabanı geçici olarak devre dışı ise, bu erteleme kullanmaz, bunun yerine iletileri alma askıya olduğunu not veritabanını yeniden kullanılabilir hale gelene kadar değerlerinin.

İletileri ertelemeyi öyle yapılandırılmışsa, ertelenmiş iletileri hala başlangıçta zamanlanan saatte sona ve ardından sahipsiz sıraya taşınır anlamına ileti sona erme etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)