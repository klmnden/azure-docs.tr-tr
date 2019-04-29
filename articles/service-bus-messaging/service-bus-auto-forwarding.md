---
title: Azure Service Bus Mesajlaşma varlıkları otomatik iletme | Microsoft Docs
description: Bir Service Bus kuyruğu veya başka bir kuyruk veya konu için abonelik zincir şeklinde nasıl.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 86fa7f62230c0ae0530b67ff2384942c876083d4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098601"
---
# <a name="chaining-service-bus-entities-with-autoforwarding"></a>Hizmet veri yolu varlıkları autoforwarding zincirleme

Service Bus *autoforwarding* özelliği bir kuyruk veya başka bir kuyruk veya aynı ad parçası olan konu için abonelik zincir olanak sağlar. Autoforwarding etkin olduğunda, Service Bus otomatik olarak ilk kuyruk ya da abonelik (kaynak) yerleştirildiğinden iletileri kaldırır ve ikinci kuyruğuna veya konusuna (hedef) geçirir. Hedef varlık, doğrudan bir ileti göndermek yine de mümkündür.

## <a name="using-autoforwarding"></a>Autoforwarding kullanma

Ayarlayarak autoforwarding etkinleştirebilirsiniz [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] veya [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] özelliklerde [QueueDescription] [ QueueDescription] veya [SubscriptionDescription] [ SubscriptionDescription] olarak kaynak için nesneleri Aşağıdaki örnekte:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Hedef varlık, kaynak varlık oluşturulduğunda mevcut olması gerekir. Hedef varlık yok, Service Bus kaynak varlık oluşturmak için sorulduğunda bir özel durum verir.

Tek bir konuyu ölçeklendirme autoforwarding kullanabilirsiniz. Service Bus sınırları [belirli bir konuya abonelik sayısı](service-bus-quotas.md) 2.000 için. İkinci düzey konuları oluşturarak ek abonelikler barındırabilir. Service Bus sınırlama tarafından abonelik sayısına bağlı olmayan olsa da, ikinci düzey konular bir ekleme Konunuza genel verimini artırabilir.

![Otomatik iletme senaryosu][0]

Autoforwarding, ileti gönderenler alıcılarından ayrıştırmak için de kullanabilirsiniz. Örneğin, üç modülden oluşur bir ERP sistemi göz önünde bulundurun: sipariş işleme, envanter yönetimi ve müşteri ilişkileri yönetimi. Bu modüllerin her biri, karşılık gelen bir konu içine sıraya alınan iletileri oluşturur. Alice ve Bob, müşterilerine ilişkili tüm iletileri, ilgileniyor satış temsilcilerinin var. Bu iletileri almak için Alice ve Bob kişisel bir kuyruk ve bir abonelik her biri kendi sıra tüm iletileri otomatik olarak iletme ERP konuları üzerinde oluşturun.

![Otomatik iletme senaryosu][1]

Alice, tatil kendi kişisel kuyruk, yerine, ERP konu kalırsa, dolar. Bir satış temsilcisi herhangi bir iletisi almadı, çünkü bu senaryoda, ERP konuları hiçbiri hiç olmadığı kadar kota ulaşın.

> [!NOTE]
> Autoforwarding ayarlandığında, hedef AutoDeleteOnIdle değeri otomatik olarak veri türünün maksimum değerine ayarlanır.
> Bu, olduğundan her zaman bir hedef iletinin iletileceği emin olmak için gerçekleştirilir.

## <a name="autoforwarding-considerations"></a>Autoforwarding konuları

Hedef varlık, çok fazla ileti toplanır ve kotasını aşıyor veya hedef varlık devre dışı bırakıldı, kaynak varlık iletileri ekler, [eski ileti sırası](service-bus-dead-letter-queues.md) oluncaya kadar hedef (veya varlık alanı yeniden etkin). Bu iletiler, açıkça almak ve bunları edilemeyen kuyruktan işlemek için teslim edilemeyen kuyrukta Canlı devam edin.

Bileşik bir konuya birden fazla aboneliğin bulunduğu elde etmek için tek tek ilgili konulara birbirine zincirleme zaman birinci düzey konuyu Aboneliklerde ve ikinci düzey konularda daha fazla abonelik normal bir sayıda olması önerilir. Örneğin, her biri bir ikinci düzey konuya 200 abonelikleriyle zincirleme 20 abonelikleri ile birinci düzey konu sağlar bir birinci düzey konu abonelikleriyle 200'den daha yüksek performans için her 20 abonelikleriyle ikinci düzey konu zincirleme.

Service Bus yönlendirilmiş her ileti için tek bir işlem düzenler. Birinci düzey abonelikler iletinin bir kopyasını alır. Örneğin, bunları başka bir kuyruk veya konu, Otomatik İlet iletiler için yapılandırılmış her 20 aboneliklerine sahip bir konu başlığına ileti gönderme 21 işlem olarak faturalandırılır.

Başka bir kuyruğa veya konuya zincirleme bir abonelik oluşturmak için aboneliğin oluşturucusu olmalıdır **Yönet** hem kaynak hem de hedef varlık üzerindeki izinleri. Kaynak konu başlığına ileti gönderme yalnızca gerektirir **Gönder** izinleri kaynak konusunda.

## <a name="next-steps"></a>Sonraki adımlar

Autoforwarding hakkında ayrıntılı bilgi için aşağıdaki başvuru konularına bakın:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Service Bus performans iyileştirmeleri hakkında daha fazla bilgi için bkz: 

* [Service Bus Mesajlaşması kullanarak performans geliştirmek için en iyi yöntemler](service-bus-performance-improvements.md)
* [Bölümlenmiş Mesajlaşma varlıkları][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
