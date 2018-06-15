---
title: Azure Service Bus ileti kullanım süresi sonu | Microsoft Docs
description: Geçerlilik süresi ve Azure Service Bus iletilerin yaşam süresi
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
ms.openlocfilehash: 6e1f6177ccacf24955763982189bcdb1ef69c788
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28196777"
---
# <a name="message-expiration-time-to-live"></a>İleti kullanım süresi sonu (yaşam süresi)

Bir ileti veya bir komut veya bir alıcıya, ileti ilettiği sorgulama içinde yükü neredeyse her zaman bazı uygulama düzeyi sona erme tarihi tabi biçimidir. Bu tür bir son tarihten sonra içeriği artık teslim edilir veya istenen işlem artık yürütülür.

Geliştirme ve test ortamları, kuyruklar ve konu başlıkları genellikle uygulamaları veya uygulama bölümleri kısmi çalıştırılan bağlamında kullanılan için de otomatik olarak sonraki test çalışabilmesi toplanacak olabilir kenarda kalmış test ileti almak için tercih edilir temiz başlatın.

Tek bir ileti için sona erme ayarlayarak denetlenebilir [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) göreli süreyi belirten sistem özelliği. İleti sıraya alınan varlık içine süre sonu mutlak anlık olur. Bu sırada, [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) özelliği değeri temel alır [(**EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**)](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive).

Son **ExpiresAtUtc** anlık, iletileri alma işlemi için uygun hale gelir. Sona erme teslim için şu anda kilitli iletileri etkilemez; Bu iletiler hala normal şekilde işlenir. Kilit süresi veya iletinin terk, sona erme hemen etkinleşir.

İleti kilidi altında olsa da, uygulama süresi dolmuş bir ileti elinde olabilir. Uygulamanın işlemeyle devam etmeye hazır veya iletinin abandon seçti kadar uygulayan ' dir.

## <a name="entity-level-expiration"></a>Varlık düzeyi süre sonu

Bir kuyruk veya konu gönderilen tüm iletiler varlıkta ayarlanmış varsayılan süre sonu ile düzeyi tabidir [defaultMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues) özelliği ve bu da ayarlanabilir Portalı'nda oluşturma sırasında ve daha sonra ayarlanır. Varsayılan süre sonu varlığa gönderilen tüm iletiler için kullanılan nerede [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) açıkça ayarlı değil. Varsayılan süre sonu için bir tavan olarak da işler **TimeToLive** değeri. Bir uzun olan iletiler **TimeToLive** sona erme varsayılan değerinden sessizce için ayarlanmış **defaultMessageTimeToLive** sıraya alınan olan önce değeri.

Süresi dolan iletileri isteğe bağlı olarak taşınabileceği bir [sahipsiz sırayı](service-bus-dead-letter-queues.md) ayarlayarak [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) özelliği veya ilgili portalında kutuyu işaretleyin. Seçeneği devre dışı bırakılırsa, süresi dolan iletileri bırakılır. Süresi dolan iletileri teslim edilemeyen sıraya ayırt edilen diğer atılacak lettered iletilerden değerlendirerek [DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) Aracısı kullanıcı özellikleri bölümünde; depolar özelliği değerdir[TTLExpiredException](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) bu durumda.

Kilit terk veya süresi içinde ileti sona erme kilit ve varlıkta bayrağı ayarlanmış olup olmadığını altında gelen korumalı daha önce bahsedilen durumunda sahipsiz sıraya ileti taşınır. İleti başarıyla kapatılır, daha sonra uygulama başarıyla, tüm nominal sona erme işlediği olduğunu varsayar ancak bu taşınmaz.

Birleşimi [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) ve otomatik (ve işlem) ulaşmayan posta geçerlilik sonu için bir işleyici veya bir son tarih altında işleyicileri grubu verilen bir iş olup alınan güvenini oluşturma için değerli bir araç Son olarak işleme ulaşıldı.

Örneğin, güvenilir bir ölçek kısıtlı arka ucu işleri yürütmek gereken ve bazen deneyimleri trafiği ani veya bu arka uç kullanılabilirlik bölümlerini karşı yalıtımlı istediği bir web sitesi göz önünde bulundurun. Normal durumda, sunucu tarafı işleyicisi gönderilen kullanıcı verileri için bir kuyruğa bilgi iter ve daha sonra bir yanıt sırası harekete başarılı işlenmesini onaylayan bir yanıt alır. Bir trafik ani ve arka uç işleyici biriktirme listesi öğeleri zamanında işleyemez, süresi dolan işleri sahipsiz sıraya döndürülür. Etkileşimli bir kullanıcı, istenen işlem normalden biraz daha uzun sürer ve istek daha sonra bir işleme yol için farklı bir sıra üzerinde burada son işlem sonucu kullanıcıya e-posta ile gönderilen yerleştirilebileceği bildirilebilir. 

## <a name="temporary-entities"></a>Geçici varlıklar

Service Bus kuyrukları, konu başlıkları ve Abonelikleri, belirtilen bir süre için kullanılmamış bağlandığınızda otomatik olarak kaldırılır geçici varlıklar olarak oluşturulabilir.
 
Otomatik temizleme varlıkları dinamik olarak oluşturulur ve hata ayıklama çalıştırma ya da test bazı kesintisi nedeniyle kullandıktan sonra temizlenmesini değil geliştirme ve test senaryolarda kullanışlıdır. Dinamik varlıklar, web sunucusu işlemine geri yükleme ya da güvenilir bir şekilde bu varlıkların temiz zor olduğu başka bir görece kısa süreli nesnesine yanıtları almak için bir yanıt sırası gibi bir uygulama oluşturduğunda, de kullanışlıdır zaman nesnesi Örnek kaybolur.

Kullanarak özelliği etkinleştirilmişse [autoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues) otomatik olarak silinmeden önce bir varlık olmalıdır süresini ayarlama özelliği (kullanılmayan) boş. En düşük süre 5 dakikadır.
 
**AutoDeleteOnIdle** özelliği ayarlanmalıdır, .NET Framework istemci aracılığıyla veya bir Azure Resource Manager işlemi [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'leri. Portalı aracılığıyla ayarlanamaz.


## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)