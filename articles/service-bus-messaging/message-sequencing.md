---
title: Azure Service Bus ileti sıralama ve zaman damgaları | Microsoft Docs
description: Service Bus ileti dizisi ve zaman damgalı sipariş koru
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
ms.openlocfilehash: 8665d0a1fccecf5521a553a894e2a55e52384ec3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60402730"
---
# <a name="message-sequencing-and-timestamps"></a>İleti sıralama ve zaman damgaları

Sıralama ve zaman damgası olan tüm Service Bus varlıklarının her zaman etkin ve aracılığıyla yüzey iki özellik [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) ve [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc) alınan ya da taranan özellikleri iletileri.

Mutlak iletilerin sırasını önemli olduğu durumlara yönelik ve/veya, bir tüketici güvenilir benzersiz bir tanımlayıcı iletileri, bir boşluk içermeyen ile Aracısı Damgalar iletileri için sıra numarası kuyruğuna veya konusuna göre artan gerekir. Bölümlenen varlıklar için sıra numarasına göre bölüm verilir.

**SequenceNumber** olarak kabul edilir ve işlevleri ve Aracısı kendi iç tanımlayıcı olarak depolanan bir ileti atanmış bir benzersiz bir 64-bit tamsayı değeridir. Bölümlenen varlıklar için bölüm tanımlayıcısı üst 16 bit yansıtır. Sıra numaraları 48/64 bit aralığı kaldığında sıfır olarak geçir.

Orta ve bağımsız bir yetkili tarafından ve istemciler tarafından atandıktan sonra sıra numarası benzersiz bir tanımlayıcı olarak güvenilir olabilir. Ayrıca varış true sırayı temsil eder ve zaman damgaları yeterince yüksek bir çözünürlük ileti olağanüstü fiyatlarıyla olmayabilir ve (ancak en düşük) saat eğriltme durumlarda tabi olabilir çünkü zaman damgası bir sipariş ölçütü olarak daha kesin burada Aracısı sahipliği düğümler arasında geçiş yapar.

Mutlak varış sırasındaki önemlidir, örneğin, iş'te, sınırlı sayıda mal satışa senaryoları kaynakları sırasında ilk-gelen-ilk-hizmet olarak son sunulur. Konser bileti satış buna örnek verilebilir.

Zaman damgası özelliği, yansıtılan bir ileti geliş UTC zamanını doğru bir şekilde yakalar nötr ve güvenilir yetkili görür **EnqueuedTimeUtc** özelliği. Gibi belirli bir tarihte gece yarısından önce bir iş öğesi olup gönderilen son tarihleri, bir iş senaryosuna bağlıdır, değeri kullanışlıdır ancak şu ana kadar kuyruk biriktirme listesi işlemesidir.

## <a name="scheduled-messages"></a>Zamanlanan mesajlar

Bir kuyruk veya konuda Gecikmeli işleme iletiler gönderebilirsiniz; Örneğin, belirli bir zamanda bir sistem tarafından işlenmek üzere kullanılabilir hale gelmesi için bir iş zamanlama. Bu özellik zaman tabanlı güvenilir dağıtılmış Zamanlayıcı fark etti.

Zamanlanmış iletileri tanımlanmış kuyruğa zamana kadar kuyrukta depolanabildiği değil. Bu tarihten önce zamanlanmış iletileri iptal edilebilir. İptal iletisini siler.

Zamanlayabilirsiniz ayarlayarak ya da iletileri [ScheduledEnqueueTimeUtc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc) normal gönderme yolundan veya açıkça ile ileti gönderilirken özelliği [ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) API. İkinci hemen zamanlanmış iletinin döndürür **SequenceNumber**, daha sonra zamanlanmış iletileri gerekirse iptal etmek için kullanabileceğiniz. Zamanlanan mesajlar ve kendi sıra numaraları ayrıca bulunması kullanarak [iletilere gözatma](message-browsing.md).

**SequenceNumber** için zamanlanmış bir ileti yalnızca geçerli sırasında bu durumdayken iletisidir. Etkin duruma ileti geçişleri ileti kuyruğuna sıraya içeren yeni bir atama geçerli anlık olsaydı gibi eklenir **SequenceNumber**.

Service Bus özelliği tek bir ileti sabitlenmiştir ve iletileri yalnızca sıraya alınan bir kez olabilir çünkü yinelenen zamanlamalar iletileri desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)