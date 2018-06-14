---
title: Azure hizmet veri yolu ad alanları eşleştirilmiş | Microsoft Docs
description: Eşleştirilmiş ad uygulama ayrıntılarını ve maliyet
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: f16c65286b0aa079889c9d53e98bf54e3d57c95f
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
ms.locfileid: "27159550"
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Ad alanı uygulama ayrıntılarını eşleştirilmiş ve etkileri maliyet

[PairNamespaceAsync] [ PairNamespaceAsync] yöntemini kullanarak bir [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] örneği, sizin adınıza görünür görevleri gerçekleştirir. Var maliyet çünkü konuları özelliğini kullanırken, böylece bu durum oluştuğunda davranışı beklediğiniz bu görevleri anlamak kullanışlıdır. API aşağıdaki otomatik davranışı, sizin adınıza prosese:

* Biriktirme listesi sıraları oluşturma.
* Oluşturulmasını bir [MessageSender] [ MessageSender] sıralar veya konuları ettiği nesnesi.
* Bir Mesajlaşma varlığı kullanılamaz duruma geldiğinde, varlığın yeniden kullanılabilir duruma geldiğinde algılama girişimi varlık iletileri gönderir ping işlemi uygulayın.
* İsteğe bağlı olarak "iletisi Pompalar" kümesi oluşturur, iletileri taşıma biriktirme listesi kuyruklardan birincil sıralar.
* Koordinatları kapanış/hatalı birincil ve ikincil [Eventhubclient] [ MessagingFactory] örnekleri.

Yüksek düzeyde, bu özellik şu şekilde çalışır: birincil varlık iyi durumda hiçbir davranış değişiklikleri oluşur. Zaman [FailoverInterval] [ FailoverInterval] süresi dolduktan ve birincil varlık görür Hayır başarılı bir geçici olmayan sonra gönderir [MessagingException] [ MessagingException] veya [TimeoutException][TimeoutException], aşağıdaki davranış oluşur:

1. Birincil varlık işlemleri devre dışı bırakılır ve ping başarıyla teslim edilebilir kadar sistem birincil varlık ping gönderin.
2. Rastgele biriktirme listesi sırası seçilir.
3. [BrokeredMessage] [ BrokeredMessage] nesneler, seçtiğiniz biriktirme listesi kuyruğuna yönlendirilir.
4. Seçilen biriktirme listesi kuyruğa gönderme işlemi başarısız olursa, o sıra döndürme çekilir ve yeni bir sıra seçilir. Üzerindeki tüm gönderenlerin [Eventhubclient] [ MessagingFactory] örneği başarısızlığını öğrenin.

Aşağıdaki şekil dizisini tarif. İlk olarak, gönderici iletileri gönderir.

![Eşleştirilmiş ad alanları][0]

Birincil sıraya göndermek için başarısızlık durumunda, rastgele seçilen biriktirme listesi kuyruğa ileti gönderme gönderen başlar. Aynı anda bir ping görev başlatır.

![Eşleştirilmiş ad alanları][1]

Bu noktada iletiler hala ikincil sıraya ve birincil kuyruğuna teslim edilmedi. Birincil kuyruk yeniden sağlıklı olduğunda en az bir işlem Sifon çalıştırıyor olması gerekir. Sifon iletileri çeşitli biriktirme listesi sıraları tüm uygun hedef varlıkları (kuyruklar ve konu başlıkları) sunar.

![Eşleştirilmiş ad alanları][2]

Bu konunun geri kalanında bu bilgilerin nasıl çalıştığını belirli ayrıntılarını açıklanır.

## <a name="creation-of-backlog-queues"></a>Biriktirme listesi sıraların oluşturma
[SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] nesne geçirilen [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi kullanmak istediğiniz biriktirme listesi sıraları sayısını belirtir. Her bir biriktirme listesi sırası sonra aşağıdaki özelliklere sahip açıkça oluşturulur ayarlayın (diğer tüm değerler kümesine [QueueDescription] [ QueueDescription] Varsayılanları):

| Yol | [birincil ad alanı] / x servicebus aktarımı / [[dizin] [0, BacklogQueueCount) değerinde olduğu dizin] |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| maxDeliveryCount |int MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| LockDuration |1 dakika |
| EnableDeadLetteringOnMessageExpiration |doğru |
| EnableBatchedOperations |doğru |

Örneğin, ilk biriktirme listesi sıranın oluşturulan ad alanı için **contoso** adlı `contoso/x-servicebus-transfer/0`.

Kuyruklar oluştururken, kod ilk böyle bir sıra olup olmadığını denetler. Sıranın mevcut değilse, sıranın oluşturulur. Kod desteklemez "ek" biriktirme listesi sıraları temizlenemedi. Özellikle, uygulama birincil ad alanı ile **contoso** beş biriktirme listesi sırası ancak bir biriktirme listesi sıra yolu ile istekleri `contoso/x-servicebus-transfer/7` var, bu ek biriktirme listesi kuyruğa hala var, ancak kullanılmaz. Sistem değil kullanılacak olan mevcut için fazladan biriktirme listesi kuyrukları açıkça izin verir. Ad alanı sahibi olarak, tüm kullanılmayan/istenmeyen biriktirme listesi sıralarını temizleme işlemi için sorumluluğu size aittir. Bu karar hizmet veri yolu ad alanınız içinde tüm sıraları için hangi amacıyla mevcut bilemezsiniz nedeni. Ayrıca, bir sıra verilen ada sahip var, ancak varsayılan karşılamıyor [QueueDescription][QueueDescription], sonra da için kendi varsayılan davranışı değiştirme, nedenleri. Garanti biriktirme listesi sıraları değişiklikler için kodunuz tarafından yapılır. Yaptığınız değişiklikleri sınamanız emin olun.

## <a name="custom-messagesender"></a>Özel MessageSender
Tüm iletileri gönderirken, bir iç Git [MessageSender] [ MessageSender] zaman her şeyi çalışır ve biriktirme listesine yönlendiren normal şekilde davranır nesne kuyruklar şey "böldüğünüzde." Bir geçici olmayan hata alındıktan sonra bir süreölçer başlatır. Sonra bir [TimeSpan] [ TimeSpan] oluşan süresi [FailoverInterval] [ FailoverInterval] sırasında başarılı bir ileti gönderilir, özellik değeri yük devretme gerçekleştiriliyor. Bu noktada, her bir varlık için aşağıdakiler gerçekleşir:

* Bir ping görevi yürütür her [PingPrimaryInterval] [ PingPrimaryInterval] varlık kullanılabilir olup olmadığını denetlemek için. Bu görev başarılı olduktan sonra varlığı hemen kullanan tüm istemci kodu birincil ad alanına yeni iletileri göndermeye başlar.
* Diğer bir göndericilerden aynı varlık göndermek için gelecekteki isteklerin neden olur [BrokeredMessage] [ BrokeredMessage] biriktirme listesi sıraya sit için değiştirilmesi gönderilen. Bazı özelliklerinden değişikliği kaldırır [BrokeredMessage] [ BrokeredMessage] nesne ve başka bir yerde depolar. Aşağıdaki özellikler temizlenir ve Service Bus ve SDK iletileri hep işlemeye izin vererek yeni bir ad altında eklendi:

| Eski özellik adı | Yeni özellik adı |
| --- | --- |
| SessionID |x-ms-SessionID |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-yol |

Özgün hedef yolu, aynı zamanda içindeki ileti x-ms-path adlı bir özellik olarak saklanır. Bu tasarım, bir tek biriktirme listesi kuyruktaki bir arada birçok varlıklar için iletileri sağlar. Özellikler Sifon tarafından geri çevrilir.

Özel [MessageSender] [ MessageSender] nesne sorunlarla iletileri 256 KB sınıra yaklaşmak ve yük devretme gerçekleştiriliyor. Özel [MessageSender] [ MessageSender] nesnesi tüm kuyrukları ve konularından biriktirme listesi kuyruklarda birlikte iletileri saklar. Bu nesne pek çok ana biriktirme listesi sıraları içinde birlikte gelen iletileri karıştırır. Yük Dengeleme birbirine bilmiyorsanız birçok istemciler arasında işlemek için SDK rastgele bir biriktirme listesi sırasına her biri için Çekmeleri [QueueClient] [ QueueClient] veya [TopicClient] [ TopicClient] kodda oluşturabilir.

## <a name="pings"></a>Ping isteği
Boş bir ping iletisidir [BrokeredMessage] [ BrokeredMessage] ile kendi [ContentType] [ ContentType] özelliği uygulama/vnd.ms-servicebus-ping ayarlanmış ve [TimeToLive] [ TimeToLive] 1 saniye değeri. Bu ping Service Bus içinde bir özellik vardır: herhangi bir çağırıcı istediğinde sunucusu hiçbir zaman bir ping sunar bir [BrokeredMessage][BrokeredMessage]. Bu nedenle, hiçbir zaman alır ve bu iletiler yoksay öğrenmek zorunda. Her varlığın (benzersiz kuyruk veya konu) başına [Eventhubclient] [ MessagingFactory] istemci başına örnek ping işlemi kullanılamaz hale gelmesine değerlendirildiğinde. Varsayılan olarak, bu kez dakika başına gerçekleşir. Ping iletilerine normal Service Bus iletiler olduğu kabul edilir ve bant genişliği ve iletileri ilişkin ücretlere neden olabilir. İstemciler sistemin kullanılabilir olduğunu tespit hemen iletileri durdurun.

## <a name="the-syphon"></a>Sifon
Uygulamada en az bir yürütülebilir program etkin olarak Sifon çalıştırıyor olması gerekir. Bir uzun Sifon gerçekleştirir yoklama alırsınız, 15 dakika sürer. Tüm varlıklar kullanılabilir olduğunda ve 10 biriktirme listesi sıraların sahip olduğunda Sifon barındıran uygulama 40 kez günde 960 kez ve 28800 kez 30 gün içinde saat başına alma işlemi çağırır. Sifon etkin olarak iletileri biriktirme listesindeki birincil kuyruğuna olduğu taşırken, her ileti (tüm aşamalarda ileti boyutu ve bant genişliği için standart ücretler geçerlidir) aşağıdaki ücretleri görür:

1. Biriktirme listesine gönderin.
2. Biriktirme listesindeki alırsınız.
3. Birincil gönderin.
4. Birincil sunucudan alma.

## <a name="closefault-behavior"></a>Davranış Kapat/hata
Sifon, bir kez birincil veya ikincil barındıran uygulama içinde [Eventhubclient] [ MessagingFactory] hataları ya da hatalı kendi iş ortağı kapalı olduğu veya kapalı ve bu durum Sifon algılar Sifon yapar. Varsa diğer [Eventhubclient] [ MessagingFactory] kapalı 5 saniye içinde hala açık Sifon hataları [Eventhubclient][MessagingFactory].

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [zaman uyumsuz desenleri ve yüksek kullanılabilirlik Mesajlaşma] [ Asynchronous messaging patterns and high availability] Service Bus zaman uyumsuz Mesajlaşma hakkında ayrıntılı bilgi için. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
