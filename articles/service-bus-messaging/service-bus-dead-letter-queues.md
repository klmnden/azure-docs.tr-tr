---
title: Service Bus edilemeyen | Microsoft Docs
description: Azure Service Bus edilemeyen genel bakış
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 0364304a203e03faf69868174a45cb41850ce112
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60713971"
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Service Bus edilemeyen genel bakış

Azure Service Bus kuyrukları ve konu abonelikleri olarak adlandırılan bir ikincil bir alt kuyruk sağlayan bir *eski ileti sırası* (DLQ). Eski ileti sırası açıkça oluşturulması ve silinemez veya aksi halde yönetilen ana varlık bağımsız gerek yoktur.

Bu makalede hizmet veri yolu kuyrukları atılacak. Tartışma çoğunu gösterilmiştir [geçerliliğini yitirmiş kuyruk örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) GitHub üzerinde.
 
## <a name="the-dead-letter-queue"></a>Eski ileti sırası

Eski ileti sırası amacı, herhangi bir alıcıya teslim edilemeyen iletiler veya işlenemedi iletileri basmaktır. İletileri DLQ kaldırılmış ve inceledi. Bir uygulama operatörün yardımıyla sorunları düzeltin ve ileti yeniden gönderin, bir hata oluştu olgu oturum açmanız ve düzeltici. 

Üzerinden üst varlık teslim edilemeyen işlem iletileri yalnızca gönderilebilir dışında bir API ve protokol açısından bakıldığında, DLQ çoğunlukla diğer herhangi bir sıra için benzerdir. Ayrıca, yaşam süresi uyulmuyor, ve edilemeyen bir iletiyi sıradan bir DLQ olamaz. Eski ileti sırası tam gözlem kilidi dağıtmayı ve işlemsel çalışmaları destekler.

Otomatik temizlik DLQ, olduğuna dikkat edin. İletileri DLQ içinde kalır, açıkça bunları DLQ ve çağrı almak kadar [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) geçerliliğini yitirmiş ileti üzerinde.

## <a name="moving-messages-to-the-dlq"></a>İletiler için DLQ taşınıyor

Service Bus iletileri DLQ Mesajlaşma altyapısı içinde itilir neden birkaç etkinliği vardır. Bir uygulama da açıkça iletileri için DLQ taşıyın. 

İleti aracısı tarafından taşındı olarak aracının kendi iç sürümünü çağrısı iki özellik iletiye eklenir [teslim edilemeyen iletiler](/dotnet/api/microsoft.azure.servicebus.queueclient.deadletterasync) iletideki yöntemi: `DeadLetterReason` ve `DeadLetterErrorDescription`.

Uygulamalar için kendi kodları tanımlayabilirsiniz `DeadLetterReason` özelliği ancak sistem, aşağıdaki değerleri ayarlar.

| Koşul | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Her Zaman |HeaderSizeExceeded |Bu akış için boyut kotasını aştı. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing ve SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |özel durum. GetType(). Adı |özel durum. İleti |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |İleti süresi doldu ve kullanılmayan lettered. |
| SubscriptionDescription.RequiresSession |Oturum kimliği null. |Etkin oturum varlık, bir oturum tanımlayıcısı null bir ileti izin vermez. |
| ! Sahipsiz Sıra |MaxTransferHopCountExceeded |Null |
| Kullanılmayan lettering açıkça uygulama |Uygulama tarafından belirtilen |Uygulama tarafından belirtilen |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount aşan

Kuyrukları ve aboneliklerinden sahip bir [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) ve [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount) özelliği sırasıyla; varsayılan değer 10'dur. Her bir ileti teslim kilidi altında ([ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)), ya da açıkça olmuştur ancak terk veya kilit süresi sona erdi, ileti [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) olduğu artırılır. Zaman [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) aşıyor [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount), ileti DLQ için taşınır belirtme `MaxDeliveryCountExceeded` neden kodu.

Bu davranışı devre dışı bırakılamaz, ancak ayarlayabileceğiniz [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) çok büyük bir sayı.

## <a name="exceeding-timetolive"></a>TimeToLive aşan

Zaman [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription) veya [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) özelliği **true** (varsayılan değer **false**), tüm süresi dolan iletileri DLQ için taşınır belirtme `TTLExpiredException` neden kodu.

Süresi dolan iletileri olduğunu yalnızca temizlenir ve en az bir etkin alıcı ana kuyruk veya abonelikten çekme olduğunda DLQ için taşınabilir unutmayın; Bu davranış tasarım gereğidir.

## <a name="errors-while-processing-subscription-rules"></a>Abonelik kurallar işlenirken hatalar

Zaman [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) özelliği için bir abonelik etkin olduğunda, bir aboneliğin SQL filtresi kuralı çalışırken oluşabilecek hatalar DLQ kaydedilir soruna neden olan sorunları var.

## <a name="application-level-dead-lettering"></a>Uygulama düzeyi ulaşmayan

Sistem tarafından sağlanan ulaşmayan özelliklerine ek olarak, açıkça kabul edilemez iletilerini reddetmeye DLQ uygulamaları kullanabilir. Bu, her tür bir sistem sorunu nedeniyle düzgün işlenemiyor iletileri, hatalı biçimlendirilmiş yüklerini tutun iletilerini veya bazı ileti düzeyi güvenliği düzeni kullanılırken, kimlik doğrulaması başarısız iletileri içerebilir.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>ForwardTo veya SendVia senaryolarda ulaşmayan

İleti aktarımı atılacak aşağıdaki koşullarda gönderilir:

- Bir ileti, 4'ten fazla kuyruklar veya olan konular geçirir [birbirine zincirlenmiş](service-bus-auto-forwarding.md).
- Hedef kuyruk veya konuda devre dışı veya silinmiş.
- Hedef kuyruk veya konu başlığı en fazla varlık boyutunu aşıyor.

Bu eski lettered iletileri almak için kullanarak bir alıcı oluşturabilirsiniz [FormatTransferDeadletterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formattransferdeadletterpath) yardımcı yöntemi.

## <a name="example"></a>Örnek

Aşağıdaki kod parçacığı, ileti alıcısı oluşturur. Ana kuyruğa alma döngüde kod iletiyi alır [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver), anında kullanılabilir herhangi bir ileti döndürür ya da hiçbir sonuç ile Aracısı sorar. Kod bir ileti alırsa, bu hemen, hangi artışlarla vazgeçer `DeliveryCount`. Sistem için DLQ ileti geçtiğinde, ana sıranın boş olduğundan ve döngüden çıkılıp, olarak [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) döndürür **null**.

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

Service Bus kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Azure kuyrukları ve Service Bus kuyrukları karşılaştırma](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

