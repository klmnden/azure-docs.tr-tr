---
title: "Otomatik iletme Azure Service Bus Mesajlaşma | Microsoft Docs"
description: "Hizmet veri yolu kuyruğu ya da başka bir kuyruk veya konu başlığı aboneliği zincir yapma."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: sethm
ms.openlocfilehash: 6c92acee9d7609f4fedcddd40563b1a55fa08fac
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Hizmet veri yolu varlıklarını otomatik iletme ile zincirleme

Hizmet veri yolu *otomatik iletme* özelliği, kuyruk veya başka bir sıraya veya aynı ad parçası olan konu aboneliği zincir olanak tanır. Otomatik iletme etkinleştirildiğinde, Service Bus otomatik olarak ilk sıra ya da abonelik (kaynak) yerleştirilen iletileri kaldırır ve ikinci sıra ya da konu (hedef) koyar. Bir ileti hedef varlık göndermek doğrudan hala mümkün olduğunu unutmayın. Ayrıca, bir sahipsiz sıraya, başka bir kuyruk veya konu gibi bir alt sırasına zincir mümkün değil.

## <a name="using-auto-forwarding"></a>Otomatik iletme kullanma
Otomatik iletme ayarlayarak etkinleştirebilirsiniz [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] veya [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] özellikleri [QueueDescription] [ QueueDescription] veya [SubscriptionDescription] [ SubscriptionDescription] olarak kaynak için nesneleri Aşağıdaki örnek.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Hedef varlık, kaynak varlık oluşturulduğunda mevcut olması gerekir. Hizmet veri yolu, hedef varlık mevcut değilse, kaynak varlık oluşturmak için sorulduğunda bir özel durum döndürür.

Tek bir konuyu ölçeklendirmek için otomatik iletme kullanabilirsiniz. Hizmet veri yolu sınırları [belirli bir konu Aboneliklerde sayısı](service-bus-quotas.md) 2.000 için. İkinci düzey konuları oluşturarak ek abonelikleri barındırabilir. Service Bus sınırlaması tarafından abonelikleri sayısına bağlı olmayan olsa bile, ikinci düzey konuları ekleme Konunuzu, genel üretilen işi artırabilir olduğunu unutmayın.

![Otomatik iletme senaryosu][0]

Otomatik iletme, alıcılar iletiyi göndericilerden ayırırsınız için de kullanabilirsiniz. Örneğin, üç modülden oluşur bir ERP sistemi göz önünde bulundurun: sipariş işleme, Envanter yönetimine ve müşteri ilişkileri yönetimi. Bu modüllerin her biri, karşılık gelen bir konu içine sıraya alınan iletileri oluşturur. Alice ve Bob müşterilerine ilişkili tüm iletileri ilgileniyor satış temsilcisi markalarıdır. Bu ileti almak için Alice ve Bob kişisel bir sıra ve bir abonelik her biri kendi sıra tüm iletileri otomatik olarak iletme ERP konuları oluşturun.

![Otomatik iletme senaryosu][1]

Alice tatil, kendi kişisel kuyruk, yerine ERP konu kalırsa, dolar. Bir satış temsilcisi herhangi bir iletisi aldı değil çünkü bu senaryoda, ERP konuları hiçbiri hiç kota ulaşabilirsiniz.

## <a name="auto-forwarding-considerations"></a>Otomatik iletme konuları

Hedef varlık çok fazla ileti toplanır ve kota aşılıyor veya hedef varlık devre dışıysa, kaynak varlık iletileri ekler, [sahipsiz sırayı](service-bus-dead-letter-queues.md) hedef (veya bir varlık içindeki alan kadar yeniden etkin). Bu iletiler, açıkça almak ve bunları sahipsiz sıradan işlemek için sahipsiz sıraya Canlı devam eder.

Birçok abonelikler ile bileşik bir konu elde etmek için tek tek konuları birlikte zincirleme kullanırken Orta sayıda birinci düzey konu Aboneliklerde ve ikinci düzey konuları birçok Aboneliklerde sahip önerilir. Örneğin, bunların 200 Abonelikleri, ikinci düzey konuyla zincir her birini 20 abonelikleri birinci düzey konuyla sağlar birinci düzey konu 200 aboneliklerle daha yüksek verimlilik için her bir ikinci düzey konuya 20 aboneliklerle zincirleme.

Hizmet veri yolu iletilen her ileti için bir işlem ödemenizi işler. İletinin bir kopyasını birinci düzey abonelikler alırsanız, örneğin, her birine otomatik iletme iletileri başka bir kuyruk veya konu başlığı, için yapılandırılmış 20 abonelikler ile bir konuya ileti gönderme 21 işlemleri olarak faturalandırılır.

Başka bir kuyruk veya konu zincir bir aboneliği oluşturmak için aboneliğin oluşturucusu olmalıdır **Yönet** hem kaynak hem de hedef varlık izinleri. Kaynak konu başlığına ileti gönderme yalnızca gerektirir **Gönder** kaynak konu izinleri.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik iletme hakkında ayrıntılı bilgi için aşağıdaki başvuru konularına bakın:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Hizmet veri yolu performans iyileştirmeleri hakkında daha fazla bilgi için bkz: 

* [Service Bus Mesajlaşma hizmeti kullanarak performans iyileştirmeleri için en iyi yöntemler](service-bus-performance-improvements.md)
* [Bölümlenmiş Mesajlaşma varlıkları][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
