---
title: "Azure Service Bus yinelenen ileti algılama | Microsoft Docs"
description: "Yinelenen hizmet veri yolu iletileri Algıla"
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
ms.date: 01/25/2018
ms.author: sethm
ms.openlocfilehash: efc5608d4812edbb3f477dffbc2b495b331bd787
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="duplicate-detection"></a>Yinelenen algılama

Bir uygulama karşılaşırsa hemen sonra önemli bir hata ileti gönderir ve önceki ileti teslimi değil gerçekleştiğinden, sistemde iki kez görünmesi aynı iletiyi sonraki gönderme neden yeniden başlatılan uygulama örneğinin yanlışlıkla düşündüğü.

Ayrıca bir hata biraz daha önce gerçekleşmesi için istemci veya ağ düzeyinde mümkündür ve bildirim ile kuyruğa başarıyla tamamlanması gönderilen bir ileti için istemciye döndürülen. Bu senaryo, gönderme işlemi sonucuyla ilgili şüpheli istemci bırakır.

Yinelenen algılama gönderen yeniden aynı iletiyi gönderin etkinleştirerek bu durumlar dışında şüpheli alır ve yinelenen kopyalar kuyruk veya konu atar.

Yinelenen algılama etkinleştirme yardımcı olan uygulama tarafından denetlenen izlemek *MessageID* , kuyruk veya konu belirtilen zaman penceresi sırasında gönderilen tüm iletiler. Taşıyan yeni bir iletisi gönderirse bir *MessageID* , zaten günlüğe kaydedildi zaman penceresi sırasında ileti kabul olarak bildirildi (gönderme işlemi başarılı olur), ancak yeni gönderilen ileti hemen göz ardı ve bırakıldı. Herhangi bir iletinin dışında bölümünü *MessageID* olarak kabul edilir.

Uygulama denetimi tanımlayıcının önemlidir, yalnızca o bağlamanın uygulama verdiğinden *MessageID* içinden bunu beklendiği yeniden başarısız olması durumunda iş işlem bağlamı için.

İçinde birden fazla ileti gönderilir bazı uygulama bağlamı işleme sırasında bir iş sürecini için *MessageID* satınalma siparişi numarası gibi uygulama düzeyi bağlam tanımlayıcı bileşimi olabilir ve iletinin konusunu; Örneğin, **12345.2017/ödeme**.

*MessageID* bazı GUID her zaman olabilir, ancak iş sürecini tanımlayıcısına sabitleme yinelenen algılama özelliğini etkili bir şekilde yararlanmak için istenen tahmin edilebilir Yinelenebilirlik verir.

## <a name="enable-duplicate-detection"></a>Yinelenen algılamayı etkinleştir

Portalda, özelliği ile varlık oluşturma sırasında açık **etkinleştirmek yinelenen algılama** onay kutusu, varsayılan olarak kapalıdır. Yeni konular oluşturmak için eşdeğer bir ayardır.

![][1]

Bayrağıyla programlı olarak ayarladığınız [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) tam framework .NET API özelliği. Azure Kaynak Yöneticisi API'si ile değer ile ayarlanır [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği.

Yinelenen algılama süresi geçmiş kuyruklar ve konu başlıkları, 7 günde en çok değeriyle 30 saniye varsayılan olarak ayarlanır. Azure portalında kuyruk ve konu Özellikleri penceresinde bu ayarı değiştirebilirsiniz.

![][2]

Kullanarak hangi sırasında iletisi kimlikleri korunur, yinelenen algılama pencere boyutunu programlı olarak yapılandırabilirsiniz [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) tam .NET Framework API özelliğiyle . Azure Kaynak Yöneticisi API'si ile değer ile ayarlanır [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği.

Tüm kayıtlı iletisi kimlikleri karşı yeni gönderilen ileti tanımlayıcısı eşleştirilmesini beri yinelenen algılama ve pencere boyutunu doğrudan etkinleştirme sıra (ve konu) üretilen iş etkisi unutmayın.

Daha az iletisi kimlikleri korunur ve eşleşen gerekir ve işleme penceresi küçük anlamına tutma daha az etkilenebilir. Yinelenen algılama gerektiren yüksek verimlilik varlıklar için olabildiğince küçük pencereyi tutmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
