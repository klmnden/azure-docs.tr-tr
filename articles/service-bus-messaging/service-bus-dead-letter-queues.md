---
title: "Hizmet veri yolu sıralarındaki | Microsoft Docs"
description: "Azure Service Bus sıralarındaki genel bakış"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2018
ms.author: sethm
ms.openlocfilehash: a82d70e7bf776bf470d14e7f061774ccbb136316
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Hizmet veri yolu sıralarındaki genel bakış

Azure Service Bus kuyrukları ve konu abonelikleri adlı ikincil alt bir sıra sağlayan bir *sahipsiz sırayı* (DLQ). Sahipsiz Sıra açıkça oluşturulması ve silinmiş veya aksi halde yönetilen ana varlık bağımsız gerekmez.

Bu makalede, Service Bus sahipsiz kuyruklarda açıklanmaktadır. Tartışma çoğunu tarafından gösterilen [sahipsiz sıraları örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) github'da.
 
## <a name="the-dead-letter-queue"></a>Sahipsiz Sıra

Sahipsiz Sıra amacı her alıcı için teslim edilemeyen iletiler veya işlenemeyen ileti basmaktır. İletileri DLQ kaldırıldı ve sahip denetlenir. Bir uygulama bir işleç yardımıyla, sorunları düzeltin ve iletiyi yeniden gönderin, bir hata oluştu olgu oturum ve düzeltme eylemlerini gerçekleştirin. 

Üzerinden üst varlık teslim edilemeyen işlem iletileri yalnızca gönderilebilir bir API ve protokolü açısından DLQ çoğunlukla başka herhangi bir kuyrukta için benzerdir. Ayrıca, yaşam süresi gözlenir değil, ve teslim edilemeyen bir DLQ iletiden olamaz. Sahipsiz Sıra tam olarak gözlem kilidinin teslim ve işlem işlemleri destekler.

DLQ otomatik olarak temizleneceği olduğuna dikkat edin. İletileri kalır DLQ, açıkça bunları DLQ ve çağrı almak kadar [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) teslim edilemeyen ileti üzerinde.

## <a name="moving-messages-to-the-dlq"></a>DLQ iletileri taşıma

Service Bus'ın DLQ Mesajlaşma altyapısı içinde gönderilen için iletilerini neden birkaç aktivitelerde vardır. Uygulamanın, ayrıca açıkça iletileri DLQ taşıyabilirsiniz. 

İleti aracısı tarafından taşınan gibi kendi iç sürümü Aracısı çağırır gibi iki özellik iletiye eklenen [sahipsiz](/dotnet/api/microsoft.azure.servicebus.queueclient.deadletterasync) iletideki yöntemi: `DeadLetterReason` ve `DeadLetterErrorDescription`.

Uygulamalar için kendi kodları tanımlayabilirsiniz `DeadLetterReason` özelliği, ancak sistem aşağıdaki değerleri ayarlar.

| Koşul | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Her zaman |HeaderSizeExceeded |Bu akış boyutu kotası aşıldı. |
| !TopicDescription.<br />EnableFilteringMessagesBeforePublishing ve SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |exception.GetType().Name |özel durum. İleti |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |İleti süresi ve kullanılmayan lettered. |
| SubscriptionDescription.RequiresSession |Oturum kimliği null şeklindedir. |Etkin oturum varlık, bir oturum tanımlayıcısı null bir ileti izin vermez. |
| ! Sahipsiz Sıra |MaxTransferHopCountExceeded |Null |
| Uygulama kullanılmayan lettering açık |Uygulama tarafından belirtilen |Uygulama tarafından belirtilen |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount aşan

Kuyruklar ve abonelikler sahip bir [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) ve [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount) özelliği sırasıyla; varsayılan değer 10'dur. Her bir ileti teslim kilidi altında ([ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)), ya da açıkça olmuştur ancak terk veya kilidi sona erdi, ileti [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) olduğu artırılır. Zaman [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) aşıyor [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount), ileti DLQ için taşınır belirtme `MaxDeliveryCountExceeded` neden kodu.

Bu davranışı devre dışı bırakılamaz, ancak ayarlayabileceğiniz [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) için çok büyük bir sayı.

## <a name="exceeding-timetolive"></a>TimeToLive aşan

Zaman [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) veya [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) özelliği ayarlanmış **true** (varsayılan **false**), tüm süresi dolan iletileri DLQ için taşınır belirtme `TTLExpiredException` neden kodu.

Süresi dolan iletileri yalnızca temizlendi ve ana sıranın veya abonelik çekme en az bir etkin alıcı olduğunda DLQ taşınmış olduğundan unutmayın; Bu davranış tasarım gereğidir.

## <a name="errors-while-processing-subscription-rules"></a>Abonelik kuralları işlenirken hatalar

Zaman [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions) özelliği için bir abonelik etkin olduğunda, bir aboneliğin Web SQL filtre kuralı çalışırken oluşabilecek hatalar DLQ yakalanır soruna neden olan sorunları var.

## <a name="application-level-dead-lettering"></a>Uygulama düzeyi ulaşmayan posta

Sistem tarafından sağlanan ulaşmayan posta özelliklerine ek olarak, uygulamaların DLQ açıkça kabul edilemez iletileri reddetmek için kullanabilirsiniz. Bu, her tür sistem sorunu nedeniyle düzgün işlenemiyor iletileri, hatalı biçimlendirilmiş yüklerini tutun iletileri veya bazı ileti düzeyi güvenlik düzeni kullanıldığında, kimlik doğrulaması başarısız iletileri içerebilir.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>ForwardTo veya SendVia senaryolarda ulaşmayan posta

Aşağıdaki koşullarda aktarımı sahipsiz sırayı iletileri gönderilir:

- Bir ileti 3'ten fazla sıralar veya olan konuları geçirir [birbirine zincirlenmiş](service-bus-auto-forwarding.md).
- Hedef sıra ya da konu devre dışı veya silinmiş.
- Hedef sıra ya da konu maksimum varlık boyutu aşıyor.

Bu eski lettered iletileri almak için kullanarak bir alıcı oluşturabilirsiniz [FormatTransferDeadletterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formattransferdeadletterpath) yardımcı yöntemi.

## <a name="example"></a>Örnek

Aşağıdaki kod parçacığını bir ileti alıcısı oluşturur. Ana sıraya alma döngüde kodunu alır [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_), hemen kullanıma hazır herhangi bir iletisi döndürmek için veya hiç sonucu ile dönmek için Aracısı sorar. Kod bir ileti alırsa, bu hemen, hangi artışlarla kenara bırakır `DeliveryCount`. Sistem iletiyi DLQ taşır. sonra ana sıranın boş olduğundan ve döngüden çıkılıp, olarak [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) döndürür **null**.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus kuyruklarını hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Azure kuyruklar ve hizmet veri yolu sıraları karşılaştırılan](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

