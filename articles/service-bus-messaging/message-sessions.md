---
title: "Azure Service Bus ileti oturumları | Microsoft Docs"
description: "Oturumları ile Azure Service Bus iletilerinin dizileri işleyin."
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
ms.date: 01/02/2018
ms.author: sethm
ms.openlocfilehash: 7a594e5951f6e90c9151fbaf231675d6ed091d1f
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="message-sessions-first-in-first-out-fifo"></a>İleti oturumları: ilk çıkar (FIFO) ilk olarak, 

Microsoft Azure Service Bus oturumları ilgili iletiler ve sınırsız dizilerini birleşik ve sıralı işlenmesi etkinleştirin. Service Bus içinde FIFO garantisi hayata geçirmek için oturumları kullanın. Hizmet veri yolu iletileri arasındaki ilişkiyi doğası hakkında Düzenleyici değil ve ayrıca burada bir ileti sırası başlangıç veya bitiş belirlemek için belirli bir model tanımlamıyor.

Gönderen zaman gönderme iletileri bir konu veya sıra içine ayarlayarak bir oturumu oluşturabilirsiniz [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) oturuma benzersiz bazı uygulama tanımlı bir tanımlayıcı özellik. Bu değer eşlendiği AMQP 1.0 protokol düzeyinde *Grup Kimliği* özelliği.

Oturumun ile en az bir ileti olduğunda oturum algılayan sıraları ya da abonelik üzerinde oturumları varlığı gelen [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Bir oturumu var olduğunda, tanımlı zaman veya yoktur API ne zaman oturum süresi veya kaybolur. Teorik olarak, bir ileti bir oturum için bugün, bir yılın zaman sıradaki ilk iletiye alınabilir ve **SessionID** eşleşir, Service Bus açısından aynı oturumdur.

Genellikle, ancak, bir uygulama burada ilgili iletiler kümesini başlar ve biter düz bir kavram vardır. Hizmet veri yolu, herhangi bir özel kuralın ayarlı değil.

Bir dosya aktarma ayarlamak için bir dizi ayırmak nasıl bir örnek **etiket** özelliği ilk iletinin için **Başlat**, Ara iletileri için **içerik**, ve son iletiye **son**. İçerik iletileri göreli konumunu geçerli iletisi olarak hesaplanabilir *SequenceNumber* delta **Başlat** ileti *SequenceNumber*.

Belirli bir alma işlemi, biçiminde Service Bus etkinleştirir oturum özelliğinde [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) C# ve Java API. Ayarlayarak özelliği etkinleştirmek [requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) sıra veya abonelik aracılığıyla Azure Resource Manager veya portalda bayrağını ayarlayarak özelliği. İlgili API işlemleri kullanmaya çalışmadan önce bu gereklidir.

Portalda, aşağıdaki onay kutusunu bayrağıyla ayarlayın:

![][2]

API oturumları için kuyruk ve abonelik istemcilerde mevcut. Oturumlar ve iletileri alındığında denetimleri kesinlik temelli bir model ve bir işleyici tabanlı modeli benzer *Onmessageoptions*alma yönetme karmaşıklığını gizler döngü,.

## <a name="session-features"></a>Oturum özellikleri

Oturumları eşzamanlı araya eklemeli ileti akışları koruma ve sıralı teslim güvence altına almak XML'deki çoğullama sağlar.

![][1]

A [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) alıcı, bir oturum kabul istemci tarafından oluşturulur. İstemci çağrılarını [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) veya [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) C#. Geriye dönük geri çağırma modelinde, bir oturum işleyici kaydeder.

Zaman [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesi tarafından kabul edilir ve istemci tarafından tutulan karşın, istemci o oturumunun sahip tüm iletiler üzerinde özel bir Kilit tutan [SessionID](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) sıra veya abonelik, mevcut ve Ayrıca, sahip tüm iletiler üzerinde **SessionID** , hala gelmesini oturum kilitli durumdayken.

Kilidi serbest zaman **kapatmak** veya **CloseAsync** olarak da bilinen veya kilidi uygulama olduğu kapatma işlemini gerçekleştiremiyor durumlarda süresi dolduğunda. Oturum kilidi uygulama, artık gerekli ve/veya herhangi başka ileti beklemiyor hemen oturumunu kapatmalısınız anlamına gelen bir özel kullanım kilidi gibi bir dosya değerlendirilmelidir.

Birden çok eşzamanlı alıcıları sıradan çıkarmak, belirli bir oturuma ait iletileri şu anda bu oturum için kilidi tutan belirli bir alıcının gönderilir. Bu işlem ile bir sıra veya abonelik bulunan bir araya eklemeli ileti akışı düzgün bir şekilde farklı alıcıya içermiyordu çoğaltıldı ve Kilit Yönetimi içinde Hizmet tarafı yapıldığından bu alıcılar farklı istemci bilgisayarlarında de canlı Hizmet veri yolu.

Bir kuyruk, ancak hala bir sıra olup: rastgele erişimi yoktur. Bir oturum alıcı talep kadar belirli oturumları kabul etmek veya belirli oturumları iletilerden bekleyin için birden çok eşzamanlı alıcıları bekleyin ve bir ileti alıcı yok henüz istemiş durumda bir oturuma ait olan bir sıra üstündeki ise teslimler tutun oturumu.

Önceki çizimde tümü etkin ilerleme her alıcı için sırasından ileti almalıdır üç eşzamanlı oturum alıcıları gösterilmektedir. Önceki oturumla `SessionId` = 4, bu iletiyi duruma kadar hiçbir ileti herkese teslim edilir anlamına gelir hiçbir etkin, sahibi olan istemci sahip bir yeni oluşturulan tarafından oturum alıcı yapamaz.

Sınırlama olması görünebilir, ancak özellikle kesinlikle zaman uyumsuz kod ile yazılan, tek alıcı işlem fazla eşzamanlı oturum kolayca işleyebilir; birkaç düzine eşzamanlı oturum dengede geri çağırma modeliyle etkili bir şekilde otomatik olarak yapılır.

Çok sayıda eşzamanlı oturum yapabildiği her oturum yalnızca düzensiz aralıklarla hataya işlemek için stratejisi iletileri alan, boş bir süre sonra oturumu bırakın ve bir sonraki oturumu ulaştığında oturum kabul edildiğinde işleme devam etmek için işleyici.

Tarafından kullanılan ileti kilit için bir şemsiyesi oturum alıcı tarafından tutulan oturum kilidi olan *gözlem kilidinin* kapatma modu. Bir alıcı iki ileti eşzamanlı olarak "Uçuş modunda" olamaz, ancak iletileri sırayla işlenmelidir. Yeni bir ileti, yalnızca önceki iletiyi tamamlandı ya da eski lettered alınabilir. Aynı ileti sunulacak bir ileti nedenler bırakıp yeniden ile ileri alma işlemi.

## <a name="message-session-state"></a>İleti oturum durumu

İş akışları ne zaman işleneceğini büyük ölçekli, yüksek kullanılabilirlik bulut sistemleri, belirli bir oturum ile ilişkili iş akışı işleyici beklenmeyen arızalardan kurtarmak ve ayrıca farklı bir kısmen tamamlanmış çalışmaları sürdürme yeteneğine sahip olmalıdır işlemin veya iş yeri başlangıcından makinenin.

Bu oturuma göre kaydedilen işleme durumu oturum yeni işlemcisi tarafından alındığında hemen kullanılabilir hale oturum durumu tesis Aracısı içinde ileti oturumunun uygulama tanımlı ek açıklama sağlar.

Hizmet veri yolu açısından bakıldığında, ileti oturum durumu, hizmet veri yolu Standart için 256 KB ve Service Bus Premium için 1 MB bir iletinin boyutunu veri tutabilen bir donuk ikili nesnesidir. Bir oturum göreli işleme durumu içinde oturum durumu tutulabilir veya oturum durumu bazı depolama konumu veya gibi bilgileri tutan veritabanı kaydını işaret edebilir.

Oturum durumu yönetmek için API'ler [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) ve [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), bulunabilir [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) C# ve Java API nesnesi. Daha önce hiç oturum durumu olan bir oturum döndürür ayarlayın bir **null** için başvuru **GetState**. Önceden ayarlanmış oturum durumu temizlenmesi ile yapılır [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Oturum durumu, temizlenmez sürece kalmasını unutmayın (döndürme **null**) bir oturumdaki tüm iletilerin tüketilen olsa bile.

Bir kuyruk veya abonelik tüm mevcut oturumları ile numaralandırılabilecek **SessionBrowser** yöntemi Java API ve ile [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) üzerinde [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) ve [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) .NET istemci.

Oturum durumu bir sıraya veya bir abonelik bu varlığın depolama kotası doğru sayar tutulan. Uygulamayı oturum bittiğinde, bu nedenle tutulan durumuna temizlemek uygulama için dış yönetim maliyeti önlemek için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Tam bir örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) hizmet yolundan oturum tabanlı ileti alma ve gönderme, kuyruklar .NET standart kitaplığını kullanarak.
- [Bir örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) , .NET Framework istemci oturumu algılayan iletileri işlemek için kullanır. 

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png