---
title: Azure Service Bus ileti oturumlarını | Microsoft Docs
description: Azure Service Bus iletileri oturumları içeren dizileri işler.
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
ms.openlocfilehash: d6c46d6ebfa8ae44c9bfac4929d3478f6701758a
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497848"
---
# <a name="message-sessions-first-in-first-out-fifo"></a>İleti oturumları: ilk çıkar (FIFO) ilk olarak, 

Microsoft Azure Service Bus oturumları, ilgili iletilerin sınırsız dizilerinin birleşik ve sıralı işlemeyi etkinleştirin. Hizmet veri yolu FIFO garantisi hayata geçirmek için oturumları kullanın. Service Bus iletileri arasındaki ilişkiyi doğası hakkında belirleyici değildir ve ayrıca burada bir ileti sırası başlangıç veya bitiş belirlemek için belirli bir modelde tanımlamıyor.

> [!NOTE]
> Service Bus'ın temel katman, oturumları desteklemez. Standart ve premium katmanlarda, oturumları desteklemez. Daha fazla bilgi için [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).

Gönderen, gönderme iletilerini konusu veya kuyruğundan ayarlayarak bir oturumu oluşturabilirsiniz [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) özelliğini oturuma benzersiz olan bazı uygulama tanımlı tanımlayıcı. Bu değer AMQP 1.0 protokol düzeyinde eşlenir *Grup Kimliği* özelliği.

Oturumun ile en az bir ileti olduğunda oturum algılayan kuyruklarının veya aboneliklerinin üzerinde oturumları varlığı gelen [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Bir oturum mevcut olduğunda, tanımlanan zaman veya yoktur API ne zaman oturumu sona veya kaybolur. Bir oturum için bugün, bir yılın, zaman içinde sonraki iletiyi bir ileti teorik olarak, alınabilir ve **SessionID** eşleşir, Service Bus açısından aynı oturumdur.

Genellikle, ancak bir uygulamanın bir dizi ilgili ileti başlar ve biter NET bir kavram vardır. Service Bus, herhangi belirli kurallar ayarlı değil.

Bir dosya aktarma ayarlamak için bir dizi ayırmak nasıl bir örnek **etiket** ilk iletinin özelliği **Başlat**, Ara iletileri için **içerik**, ve son iletinin **son**. İçerik iletileri göreli konumunu geçerli iletiyi hesaplanmasını *SequenceNumber* delta **Başlat** ileti *SequenceNumber*.

Belirli bir alma işlemi, biçiminde Service Bus etkinleştirir oturumu özelliği [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) C# ve Java API. Ayarlayarak özelliği etkinleştirmeniz [requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği kuyruk veya Azure Resource Manager aracılığıyla veya portalda gösterilen bayrak ayarlayarak aboneliği. İlgili API işlemleri kullanmaya çalışmadan önce bu gereklidir.

Portalda, aşağıdaki onay kutusunu bayrağıyla ayarlayın:

![][2]

Oturumlarının API'leri istemcilerde, kuyruk ve abonelik yok. Oturumlarının ve iletileri alındığında denetleyen kesinlik temelli bir model ve bir işleyici tabanlı model, benzer *Onmessageoptions*alma yönetmenin karmaşıklığını gizler döngü,.

## <a name="session-features"></a>Oturum özellikleri

Eş zamanlı araya eklemeli ileti akışları koruma ve sıralı teslim güvence altına almak XML'deki çoğullama oturumları sağlar.

![][1]

A [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) alıcı kabul eden bir oturumu istemci tarafından oluşturulur. İstemci çağrıları [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) veya [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) C#. Reaktif geri çağırma modelinde, bir oturum işleyici kaydeder.

Zaman [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesne kabul edilir ve bir istemci tarafından açık tutulduğu sürece, istemci ile bu oturumun tüm iletiler özel bir Kilit tutan [SessionID](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) sıra veya abonelik, mevcut ve Ayrıca, sahip tüm iletiler üzerinde **SessionID** , yine de ulaşır oturumu açık tutulduğu sürece.

Kilidi serbest bırakılır olduğunda **kapatın** veya **CloseAsync** olarak da bilinen veya kilit süresinin sona erdiği durumlarda uygulama olduğu kapatma işlemini gerçekleştiremiyor. Oturum kilidi uygulama artık ihtiyaç duyduğu veya herhangi bir ileti beklemiyor hemen oturum kapatmalısınız anlamı özel bir kilit gibi bir dosya üzerinde düşünülmelidir.

Kuyruktan birden çok eş zamanlı alıcı çekme, belirli bir oturuma ait iletileri şu anda bu oturum için kilidi tutan belirli bir alıcının gönderilir. Bu işlem bir kuyruk veya aboneliğinde bulunan bir araya eklemeli ileti akışının düzgün bir şekilde farklı alıcıya XML'deki çoğaltıldı ve kilidi Yönetimi içinde Hizmet tarafı yapıldığından bu alıcılar farklı istemci makinelerinde, ayrıca Canlı çalıştırabilirsiniz Hizmet veri yolu.

Önceki çizimde, üç eş zamanlı oturum alıcılar gösterilmektedir. Bir oturumla `SessionId` = 4, bu belirli oturumdan ileti teslim edilmesini anlamına gelir hiçbir etkin, sahip olan istemcinin sahiptir. Bir oturumu bir alt kuyruk gibi birçok şekilde davranır.

Oturum kilidi oturumu alıcı tarafından tutulan ileti kilide tarafından kullanılan bir terimdir olan *gözlem kilidi* düzenleme modu. Bir alıcı iki iletileri eşzamanlı olarak "uçuşta" olamaz, ancak sırayla işlenmesi gereken iletiler. Yeni bir ileti, yalnızca önceki iletiyi tamamlandı ya da eski lettered alınabilir. Aynı ileti alınacağı bir ileti nedenleri bırakıp tekrar ile ileri alma işlemi.

## <a name="message-session-state"></a>Oturum durumu iletisi

İş akışları ne zaman işleneceğini yüksek ölçekli, yüksek oranda kullanılabilir bulut sistemleri, belirli bir oturum ile ilişkili iş akışı işleyicisi beklenmeyen hatalardan kurtarma ve ayrıca kısmen tamamlanan iş üzerinde farklı bir yeniden başlatma uyumlu olmalıdır işlem veya makinenin, iş yeri başladı.

Oturumu yeni bir işlemci tarafından alındığında bu oturuma göre kaydedilen işleme durumu anında kullanılabilir hale ileti oturumunun Aracısı içinde uygulama tanımlı ek açıklama oturum durumu olanağı sağlar.

Service Bus açısından bakıldığında, ileti oturum durumu, Service Bus standart için 256 KB ve Service Bus Premium için 1 MB olan bir ileti boyutunun veri tutabilen bir katı ikili nesnedir. Oturum durumu içinde göreli bir oturumu işleme durumu tutulabilir veya oturum durumu bazı depolama konumu veya gibi bilgileri tutan veritabanı kaydını işaret edebilir.

Oturum durumu yönetmek için API'ler [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) ve [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), bulunabilir [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) C# ve Java API nesne. Daha önce hiç oturum durumu olan bir oturum döndürür ayarlanmış bir **null** için başvuru **GetState**. Önceden ayarlanmış bir oturum durumu temizlenmesi ile yapılır [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Oturum durumu, temizlenmiyor sürece kaldığına dikkat edin (döndüren **null**) bile tüm iletileri bir oturumda tüketilir.

Bir kuyrukta veya abonelikte var olan tüm oturumlardaki ile numaralandırılabilir **SessionBrowser** yöntemi ile Java API'si de [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) üzerinde [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) ve [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) .NET istemci.

Oturum durumu, o tüzel kişilik depolama kota hesaplamanıza dahil abonelik sayar bir kuyruk veya tutulan. Uygulama ile bir oturumu sona erdiğinde, bu nedenle tutulan durumunu temizlemek uygulama için dış yönetim maliyeti önlemek için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Tam bir örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) Service Bus'tan oturum tabanlı ileti alma ve gönderme, sıraya .NET Standard kitaplığı kullanarak.
- [Bir örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) oturumu algılayan iletileri işlemek üzere .NET Framework istemci kullanan. 

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png