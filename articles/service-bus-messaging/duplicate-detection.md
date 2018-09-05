---
title: Azure Service Bus yinelenen ileti algılama | Microsoft Docs
description: Service Bus yinelenen iletileri algılama
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
ms.date: 01/25/2018
ms.author: spelluru
ms.openlocfilehash: 7402fcf01078ea3934d1b6794a9190947fe339c2
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696640"
---
# <a name="duplicate-detection"></a>Yineleme algılama

Bir uygulama ile karşılaşırsa, önemli bir hata hemen sonra bir ileti gönderir ve yeniden uygulama örneğini deneyebileceğinizi düşündüğü önceki ileti teslim oluşmadı olduğunu, sonraki gönderme aynı iletinin sistemde iki kez görünmesine neden olur.

Ayrıca biraz daha önce gerçekleşmesi için istemci veya ağ düzeyinde hata mümkündür ve bildirim ile kuyruğuna başarıyla tamamlanması gönderilen ileti için istemciye döndürülen. Bu senaryoda, istemci gönderme işleminin sonucu hakkında şüpheli bırakır.

Yinelenen algılama gönderen yeniden aynı iletiyi gönderin etkinleştirerek bu durumlar dışında şüpheli alır ve kuyruğun veya konunun tüm yinelenen kopyaları atar.

Yineleme algılamayı etkinleştirmek yardımcı olan uygulama tarafından denetlenen izlemek *MessageID* bir kuyruk veya konuda içinde belirtilen zaman penceresi boyunca gönderilen tüm iletilerin. Taşıyan yeni bir ileti gönderildiğinde bir *MessageID* , zaten günlüğe kaydedildi zaman penceresi boyunca, iletiyi kabul olarak bildirildi (gönderme işlemi başarılı), ancak yeni gönderilen ileti anında yoksayıldı ve bırakıldı. İletinin dışında hiçbir diğer bölümlerini *MessageID* olarak kabul edilir.

Yalnızca bu bağlamanın verdiğinden tanımlayıcısının uygulama denetimi gerekli *MessageID* , bunu tahmin edilebilir bir biçimde reconstructed hata durumunda iş işlem bağlam.

İçinde birden çok ileti gönderilir bazı uygulama içeriği, işleme sırasında bir iş işlemi *MessageID* bileşik bir satınalma siparişi numarası gibi uygulama düzeyinde bağlam tanımlayıcısının olabilir ve iletinin konusu; Örneğin, **12345.2017/ödeme**.

*MessageID* bazı GUID her zaman olabilir, ancak iş süreci tanımlayıcısına sabitleme, yinelenen algılama özelliği etkili bir şekilde yararlanmak için istenen tahmin edilebilir yinelenebilirliği verir.

## <a name="enable-duplicate-detection"></a>Yinelenen algılamayı etkinleştir

Portalda özelliği ile varlık oluşturma sırasında açık **yinelenen algılamayı etkinleştirme** onay kutusu, varsayılan olarak kapalıdır. Yeni konular oluşturmak için bu ayarı eşdeğerdir.

![][1]

Bayrağı ile programlı olarak ayarladığınız [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) tam Framework .NET API özelliği. Azure Resource Manager API'si ile ile değerin ayarlanması [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği.

Yineleme algılama süresi geçmiş varsayılan olarak 30 saniyeye kuyruklar ve konular, en fazla 7 gün değerine sahip. Azure portalında kuyruk ve konu Özellikler penceresinde bu ayarı değiştirebilirsiniz.

![][2]

Kullanarak bu sırada iletisi kimlikleri korunur, yineleme algılama penceresi boyutunu programlı olarak yapılandırabilirsiniz [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) tam .NET Framework API özelliği . Azure Resource Manager API'si ile ile değerin ayarlanması [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği.

Tüm kayıtlı iletisi kimlikleri karşı yeni gönderilen ileti tanımlayıcısı eşlenmesi gereken bu yana yinelenen öğe algılamasını ve pencere boyutunu doğrudan etkinleştirme kuyruk (ve konu) aktarım hızını etkiler unutmayın.

Daha az iletisi kimlikleri korunur ve eşleşmesi gerekir ve üretilen iş penceresi küçük anlamına gelir tutarak daha az etkilenmiş. Yinelenen algılama gerektiren yüksek aktarım hızı varlıklar için olabildiğince küçük pencereyi tutmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
