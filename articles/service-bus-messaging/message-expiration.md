---
title: Azure Service Bus ileti süre | Microsoft Docs
description: Geçerlilik süresi ve Azure Service Bus iletileri yaşam süresi
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
ms.openlocfilehash: fdfd7794961b0254526b124525c6e978d13b0114
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65800272"
---
# <a name="message-expiration-time-to-live"></a>İleti süre sonu (Yaşam Süresi)

Bir ileti bir komut veya bir alıcıya, ileti ilettiği sorgulama neredeyse her zaman uygulama düzeyi sona erme tarihi çeşit tabi yüktür. Böyle bir son tarihine ulaşıldıktan sonra içerik artık teslim veya istenen işlem artık yürütülür.

Geliştirme ve test ortamları, kuyruklar ve konular genellikle uygulama veya uygulama bölümleri kısmi çalıştırmaları bağlamında kullanılan için de otomatik olarak sonraki test çalıştırın böylece atık olabilir kenarda kalmış test iletileri için tercih edilir temiz başlatın.

Herhangi bir ileti için süre sonu ayarlayarak denetlenebilir [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) göreli süre belirten sistem özelliği. İleti kuyruğa alınan varlık içine süre sonu mutlak anlık olur. O zaman [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) özellik değerini alır [(**EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**)](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive). Hiçbir istemci etkin bir şekilde dinlerken, aracılı ileti yaşam süresi (TTL) ayarı zorlanmaz.

Son **ExpiresAtUtc** anında, iletileri alma işlemi için uygun olur. Sona erme için teslim şu anda kilitli iletileri etkilemez. Bu iletileri hala normal şekilde işlenir. Kilit süresi dolana veya sözleşme ileti bırakıldı, sona erme hemen etkili olur.

İleti kilidi altında olsa da, uygulama süresi dolmuş bir ileti elinde olabilir. Uygulama işlemeyle devam konusunda istekli mi veya iletiyi bırak seçtiğinde kadar uygulayan ' dir.

## <a name="entity-level-expiration"></a>Varlık düzeyinde süre sonu

Bir kuyruk veya konuda gönderilen tüm iletilerin varlıkta ayarlanmış varsayılan süre sonu ile düzeyi tabidir [defaultMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues) özelliği ve bu da ayarlanabilir portalda oluşturma sırasında ve daha sonra ayarlanır. Varsayılan süre sonu varlığa gönderilen tüm iletiler için kullanılan burada [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) açıkça ayarlı değil. Varsayılan süre sonu için bir tavan olarak da işler **TimeToLive** değeri. Uzun olan iletiler **TimeToLive** sona erme varsayılan değerinden sessizce için ayarlanmış **defaultMessageTimeToLive** sıraya alınıyor önce değeri.

> [!NOTE]
> Varsayılan [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) aracılı ileti değeri [TimeSpan.Max](https://docs.microsoft.com/dotnet/api/system.timespan.maxvalue) belirtilmişse Aksi takdirde.
>
> Varlıkları (kuyruklar ve konular) Mesajlaşma için varsayılan sona erme süresini de olduğundan [TimeSpan.Max](https://docs.microsoft.com/dotnet/api/system.timespan.maxvalue) Service Bus standart ve premium katmanları için.  Temel katmanı için varsayılan süre 14 gündür.

Süresi dolan iletileri isteğe bağlı olarak taşınabileceği bir [eski ileti sırası](service-bus-dead-letter-queues.md) ayarlayarak [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) özelliği veya portaldaki ilgili kutuyu işaretleyerek. Süresi dolan iletileri seçeneği devre dışı bırakılırsa, bırakılır. Süresi dolan iletileri teslim edilemeyen kuyruğa taşındı ayırt edilebilir diğer eski lettered iletilerden değerlendirerek [DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) Aracısı kullanıcı özellikleri bölümünde; depolar özelliği değerdir[TTLExpiredException](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) böyle bir durumda.

Kilit edilmeden veya süresi içinde iletinin kilit ve varlıkta bayrağı ayarlanmış olup olmadığını altında süre sonu gelen korumalı yukarıda sözü edilen durumda eski ileti sırası için ileti taşınır. Bununla birlikte, iletinin başarılı bir şekilde kapatılır, daha sonra uygulama başarıyla, artma nominal sona erme işlediği olduğunu varsayar, taşınmaz.

Birleşimi [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) ve otomatik (ve işlem) ulaşmayan geçerlilik sonu için bir işleyici veya bir son tarih altında işleyicileri gruba verilen bir iş olup alınır, güven oluşturma için değerli bir araç Son olarak işleme ulaşıldı.

Örneğin, bir web sitesi, güvenilir bir şekilde bir ölçek kısıtlı arka uç işleri yürütmek gereken ve bazen deneyimleri trafik ani veya bu arka uç kullanılabilirlik bölümlerini karşı yalıtılmış istediği göz önünde bulundurun. Normal durumda, sunucu tarafı işleyici gönderilen kullanıcı verileri için bilgileri bir kuyruğa gönderir ve daha sonra başarılı bir yanıt kuyruğu harekete işlenmesini onaylayan bir yanıt alır. Bir trafik ani ve arka uç işleyici kendi biriktirme listesi öğelerini zamanında işleyemez, süresi dolmuş işler üzerinde eski ileti sırası döndürülür. Etkileşimli kullanıcı, istenen işlem normalden biraz daha uzun sürer ve istek sonra işleme yolu için farklı bir sırada nerede işleme nihai sonucu kullanıcıya e-posta ile gönderilen konulabilir bildirilebilir. 


## <a name="temporary-entities"></a>Geçici varlıklar

Service Bus kuyrukları, konular ve abonelikler, belirtilen bir süre için kullanılmamış bağlandığınızda otomatik olarak kaldırılır geçici varlıklar olarak oluşturulabilir.
 
Otomatik temizleme varlıklarını dinamik olarak oluşturulur ve kullanımı, test veya hata ayıklama çalışma bazı kesinti nedeniyle sonra temizlenir değil geliştirme ve test senaryolarında yararlıdır. Web sunucusu işlemine geri yükleme ya da güvenilir bir şekilde kişilikleri temizlemek zor olduğu başka bir görece kısa süreli nesnesine yanıtlar almak için bir yanıt kuyruğu gibi dinamik varlıkları bir uygulama oluşturduğunda de kullanışlıdır, nesne Örnek kaybolur.

Bu özellik kullanılarak etkinleştirilir [autoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues) özelliği. Bu özellik için bir varlık gerekir olması boşta (kullanılmayan) otomatik olarak silinmeden önce süresi için ayarlanır. Bu özellik için en düşük değer 5'tir.
 
**AutoDeleteOnIdle** özelliği ayarlanmalıdır, .NET Framework istemci aracılığıyla veya bir Azure Resource Manager işlem [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'leri. Portalda ayarlanamaz.

## <a name="idleness"></a>Modu boşta kalma oranı

İşte ne kabul modu boşta kalma oranı varlıkları (kuyruklar, konular ve abonelikler):

- Kuyruklar
    - Hiçbir gönderir  
    - Hayır alır  
    - Kuyruğa güncelleştirme yok  
    - Zamanlanmış ileti yok  
    - Hiçbir gözatma/göz atma 
- Konu başlıkları  
    - Hiçbir gönderir  
    - Konuya güncelleştirme yok  
    - Zamanlanmış ileti yok 
- Subscriptions
    - Hayır alır  
    - Abonelik için güncelleştirme yok  
    - Abonelik için eklenen yeni kural  
    - Hiçbir gözatma/göz atma  
 


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)