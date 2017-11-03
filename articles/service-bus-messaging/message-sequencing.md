---
title: "Azure Service Bus ileti sıralama ve zaman damgaları | Microsoft Docs"
description: "Hizmet veri yolu ileti sırası ve zaman damgalı sırası koru"
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
ms.date: 09/28/2017
ms.author: sethm
ms.openlocfilehash: e97bdd645ef2a3efba83e3f87114c998f9a9e344
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="message-sequencing-and-timestamps"></a>İleti sıralama ve zaman damgaları

Sıralama ve zaman damgası olan tüm hizmet veri yolu varlıklarını her zaman etkin ve aracılığıyla yüzey iki özellik [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) ve [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc) alınan ya da taranan özellikleri iletileri.

Bu durumlarda iletilerinin mutlak sırası önemlidir ve/veya, bir tüketici güvenilir benzersiz bir tanımlayıcı iletileri, bir boşluk içermeyen ile Aracısı Damgalar iletileri için sıra numarası kuyruk veya konu göre artan gerekir. Bölümlenen varlıklar için sıra numarasına göre bölüm verilir.

**SequenceNumber** olarak kabul edilir ve işlevleri ve Aracısı kendi iç tanımlayıcı olarak depolanan ileti atanmış benzersiz bir 64-bit tamsayı değerdir. Bölümlenen varlıklar için en üstteki 16 bit bölüm tanımlayıcısı yansıtır. 48/64 bit aralığı kaldığında, sıra numaraları sıfır olarak UTC'ye.

Merkezi ve dilden bağımsız bir yetkili tarafından ve istemciler tarafından atanan bu yana sıra numarası benzersiz bir tanımlayıcı olarak güvenilir olabilir. Ayrıca varış doğru sırayı temsil eder ve zaman damgaları çözünürlüğü yeterince yüksek aşırı ileti oranlarda sahip değil ve (ancak en az) saat durumlarda eğme tabi olabilir çünkü bir sipariş ölçütü olarak zaman damgası daha kesin burada Aracısı Sahiplik düğümler arasında geçiş yapar.

Mutlak varış sırası önemlidir, örneğin, business senaryoları sınırlı sayıda mal sunulan kaynakları sırasında ilk-gelen-first-hizmet temelinde son sunulan; birlikte bilet satış bir örnektir.

Zaman damgası özelliği doğru bir şekilde yansıtılan bir iletinin varış UTC saati yakalar nötr ve güvenilir bir yetkilisi olarak davranan **EnqueuedTimeUtc** özelliği. Değeri bir iş öğesi, yarısı önce belirli bir tarihte olup gönderildiği gibi son tarihleri, bir iş senaryosu bağlıdır yararlıdır, ancak şu ana kadar kuyruk biriktirme listesi işlemesidir.

## <a name="scheduled-messages"></a>Zamanlanan mesajlar

Bir kuyruk veya konu Gecikmeli işleme iletileri gönderebilirsiniz; Örneğin, belirli bir zamanda bir sistem tarafından işlenmek üzere kullanılabilir hale gelmesi işini zamanlamak için. Bu özellik, bir güvenilir dağıtılmış zaman tabanlı Zamanlayıcı gerçekleştirir.

Zamanlanmış iletiler kuyrukta tanımlı enqueue zamana kadar gerçekleştirmeye değil. Bundan önce zamanlanan iletileri iptal edilebilir. İptal iletiyi siler.

Zamanlayabilirsiniz ayarlayarak ya da iletileri [ScheduledEnqueueTimeUtc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc) normal gönderme yolundaki aracılığıyla veya açıkça ile ileti gönderirken özelliği [ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) API. İkinci hemen zamanlanmış ileti döndürür **SequenceNumber**, daha sonra gerekirse zamanlanmış iletiyi iptal etmek için kullanabileceğiniz. Zamanlanmış ileti ve sıra numaraları de keşfedilecek kullanarak [ileti gözatma](message-browsing.md).

**SequenceNumber** zamanlanmış bir ileti yalnızca çalışırken geçerlidir bu durumda iletisidir. Etkin duruma ileti geçişler ileti kuyruğuna sıraya alınan yeni bir atama içeren geçerli anlık olsaydı gibi eklenir **SequenceNumber**.

Özellik üzerinde tek bir ileti bağlantılı ve iletileri yalnızca sıraya alınan bir kez olabilir çünkü Service Bus iletileri için yinelenen zamanlamaları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)