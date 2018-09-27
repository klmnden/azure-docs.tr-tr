---
title: Azure Service Bus ad alanları eşleştirilmiş | Microsoft Docs
description: Eşleştirilmiş ad alanı uygulama ayrıntılarını ve maliyet
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: spelluru
ms.openlocfilehash: ac663cc382fcacd4960843c25aa6c95191210116
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395211"
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Eşleştirilmiş ad alanı uygulama ayrıntılarını ve etkileri maliyet

[PairNamespaceAsync] [ PairNamespaceAsync] yöntemini kullanarak bir [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] örneği, görünür görevler gerçekleştirir sizin adınıza. Bir maliyeti yoktur çünkü konuları özelliğini kullanırken, böyle durumlarda davranış beklediğiniz. böylece, bu görevleri anlamak kullanışlıdır. API, sizin adınıza otomatik aşağıdaki davranış yönetir:

* Biriktirme listesi kuyruk oluşturma.
* Oluşturma bir [MessageSender] [ MessageSender] nesnesini kuyruklar veya konular hakkında konuşuyor.
* Bir Mesajlaşma varlığı kullanılamaz hale geldiğinde, varlığın yeniden kullanılabilir duruma geldiğinde algılama girişimi varlık iletileri gönderir ping işlemi yapın.
* İsteğe bağlı olarak "iletisi pompalara" bir dizi oluşturur, iletileri taşıma biriktirme listesi kuyruklardan için birincil sıralar.
* Koordinatları kapanış/hatalı birincil ve ikincil [MessagingFactory] [ MessagingFactory] örnekleri.

Yüksek düzeyde, bu özellik şu şekilde çalışır: hiçbir davranış değişiklikleri birincil varlık iyi durumda olduğunda oluşur. Zaman [FailoverInterval] [ FailoverInterval] süresi dolduktan ve Hayır başarılı birincil varlık görür gönderen bir geçici olmayan sonra [Istransient] [ MessagingException] veya [TimeoutException][TimeoutException], aşağıdaki davranış oluşur:

1. Birincil varlık için işlemleri devre dışı bırakıldı ve ping başarıyla teslim edilebilir kadar sistem birincil varlık ping gönderin.
2. Bir rastgele biriktirme listesi sırası seçilir.
3. [BrokeredMessage] [ BrokeredMessage] nesneleri, seçilen kapsam kuyruğa yönlendirilir.
4. Seçilen kapsam kuyruğa bir gönderme işlemi başarısız olursa, döndürme bu kuyruğa alındığından ve yeni bir kuyruk seçilir. Tüm Gönderenler üzerinde [MessagingFactory] [ MessagingFactory] örneği hatasını öğrenin.

Aşağıdaki şekil dizisi kullanılırlar. İlk olarak, gönderici iletileri gönderir.

![Eşleştirilen ad alanları][0]

Birincil kuyruğa göndermek için başarısızlık durumunda, rastgele seçilen kapsam kuyruğa ileti gönderme gönderen başlar. Eşzamanlı olarak ping görevi başlatır.

![Eşleştirilen ad alanları][1]

Bu noktada iletiler yine de ikincil kuyrukta ve birincil kuyruğa teslim edilemedi. Birincil kuyruk yeniden sağlıklı hale geldikten sonra en az bir işlem Sifon çalıştırıyor olmalıdır. Sifon iletileri farklı biriktirme listesi sıralar tüm uygun hedef varlıkları (kuyruklar ve konular) sunar.

![Eşleştirilen ad alanları][2]

Bu konunun geri kalanı belirli ayrıntılarını parçaların nasıl çalıştığı açıklanır.

## <a name="creation-of-backlog-queues"></a>Sıraların biriktirme listesi oluşturma
[SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] geçirilen nesne [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi sayısını belirtir kullanmak istediğiniz biriktirme listesini sıralar. Her biriktirme listesi sırası sonra aşağıdaki özelliklerle açıkça oluşturulduğunda ayarlayın (diğer tüm değerler kümesine [QueueDescription] [ QueueDescription] varsayılan):

| Yol | [birincil ad alanı] / x-servicebus-aktarım / [[dizin] [0, BacklogQueueCount) bir değer olduğu dizin] |
| --- | --- |
| Maxsizeınmegabytes |5120 |
| MaxDeliveryCount |tamsayı MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| LockDuration |1 dakika |
| EnableDeadLetteringOnMessageExpiration |true |
| EnableBatchedOperations |true |

Örneğin, ad alanı için oluşturulan ilk biriktirme listesi sırası **contoso** adlı `contoso/x-servicebus-transfer/0`.

Kuyrukların oluştururken, kod önce böyle bir sıra olup olmadığını denetler. Kuyruk yoksa, daha sonra kuyruk oluşturulur. Kodu daha önceden temizleme "fazladan" biriktirme listesini sıralar. Özellikle, uygulama birincil ad alanı ile **contoso** beş biriktirme listesi sırası ancak bir biriktirme listesi kuyruk yoluyla istekleri `contoso/x-servicebus-transfer/7` yoksa, bu ek biriktirme listesi sırası hala mevcut olduğu ancak kullanılmaz. Sistem, kullanılmayan mevcut ek biriktirme listesi sıralarına açıkça izin verir. Ad alanı sahibi olarak, tüm biriktirme listesi kullanılmayan/istenmeyen sıralarını temizleme işlemi için sorumlu olursunuz. Bu karar nedeni, Service Bus ad alanınızdaki tüm kuyrukları için hangi amacıyla mevcut bilemezsiniz olmasıdır. Ayrıca, bir kuyruk verilen ada sahip mevcut, ancak varsayılan karşılamıyor [QueueDescription][QueueDescription], ardından için kendi varsayılan davranışını değiştirme, nedenleri. Garanti varsayılan olarak, kodunuz tarafından yapılan değişiklikler biriktirme listesi kuyruklar için yapılır. Yaptığınız değişiklikleri sınamanız emin olun.

## <a name="custom-messagesender"></a>Özel MessageSender
Tüm iletileri gönderirken, bir iç Git [MessageSender] [ MessageSender] ne zaman her şey çalışır ve biriktirme listesine yönlendirir normal şekilde davranır nesne kuyruklar şey "böldüğünüzde." Geçici olmayan hata alındıktan sonra bir süreölçer başlatır. Sonra bir [TimeSpan] [ TimeSpan] oluşan süresi [FailoverInterval] [ FailoverInterval] boyunca herhangi bir başarılı iletisi gönderilir, özellik değeri Yük devretme gerçekleştiriliyor. Bu noktada, her varlık için aşağıdakiler gerçekleşir:

* Ping görevi yürüten her [PingPrimaryInterval] [ PingPrimaryInterval] varlık kullanılabilir olup olmadığını denetlemek için. Bu görev başarılı olduktan sonra birincil ad alanı için yeni bir ileti gönderme hemen varlığını kullanan tüm istemci kodu başlatır.
* Diğer kullanıcılardan, aynı varlığa göndermek için sonraki istekler sonucunda [BrokeredMessage] [ BrokeredMessage] biriktirme listesi kuyrukta oturmak değiştirilecek gönderilen. Bazı özellikleri değişikliği kaldırır [BrokeredMessage] [ BrokeredMessage] nesnesini ve başka bir yerde depolar. Aşağıdaki özellikleri temizlenir ve Service Bus ve SDK'sı iletileri aynı şekilde işlemeye izin vererek yeni bir ad altında eklendi:

| Eski özellik adı | Yeni özellik adı |
| --- | --- |
| oturum kimliği |x-ms-oturum kimliği |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-path |

Özgün hedef yolu, ayrıca ileti içinde x-ms-path adında bir özellik olarak depolanır. Bu tasarım, tek bir biriktirme listesi kuyrukta bulunabilmesi birçok varlık için iletileri sağlar. Özellikleri Sifon tarafından geri çevrilir.

Özel [MessageSender] [ MessageSender] iletileri 256 KB sınıra yaklaşmak ve yük devretme ilgisini hep Canlı nesne sorunları karşılaşabilir. Özel [MessageSender] [ MessageSender] nesnesi tüm kuyrukları ve konuları biriktirme listesi kuyruklarda birlikte iletileri kaydeder. Bu nesne, biriktirme listesi kuyrukların birlikte içinde birçok seçimlerine iletilerden karıştırır. Birbirine bilmiyorsanız, birden çok istemci arasında yük dengelemeyi işlemek için SDK'sı rastgele bir biriktirme listesi sırası her biri için seçer [QueueClient] [ QueueClient] veya [TopicClient] [ TopicClient] kod içinde oluşturun.

## <a name="pings"></a>Ping isteği
Boş bir ping iletisidir [BrokeredMessage] [ BrokeredMessage] ile kendi [ContentType] [ ContentType] uygulama/vnd.ms-servicebus-ping özelliği ve bir [TimeToLive] [ TimeToLive] değeri 1 saniye. Bu ping, Service Bus bir özellik vardır: her arayana istediğinde sunucunun bir ping hiçbir zaman sunar. bir [BrokeredMessage][BrokeredMessage]. Bu nedenle, hiçbir zaman alır ve bu iletiler yoksay öğrenin zorunda. Her varlık (benzersiz bir kuyruk veya konu) başına [MessagingFactory] [ MessagingFactory] örnek istemci başına işten durumlarda kullanılamaz olarak kabul edilir. Varsayılan olarak, bu kez dakikada gerçekleşir. Ping iletilerine normal Service Bus iletileri olarak değerlendirilir ve bant genişliği ve iletileri ücretleri neden olabilir. İstemciler sistemin kullanılabilir olduğunu tespit hemen sonra iletileri durdurun.

## <a name="the-syphon"></a>Sifon
Uygulamada en az bir yürütülebilir programı Sifon etkin bir şekilde çalışmalıdır. Sifon gerçekleştiren bir uzun yoklama almak, 15 dakika sürer. Tüm varlıkları kullanılabilir ve 10 biriktirme listesi kuyrukları Sifon barındıran uygulaması 40 kat saatlik, günlük 960 kez ve 28800 kez 30 gün içinde alma işlemi çağırır. Sifon etkin bir şekilde iletileri biriktirme listesinden birincil kuyruğa taşınıyor, her iletinin (ileti boyutu ve bant genişliği için standart ücretler tüm aşamalar halinde uygulanır) aşağıdaki ücretler görür:

1. Biriktirme listesine gönderin.
2. Biriktirme listesinden alırsınız.
3. Birincil siteye gönderin.
4. Birincil sunucudan alma.

## <a name="closefault-behavior"></a>Kapat/hata davranışı
Sifon, bir kez birincil veya ikincil barındıran uygulama içinde [MessagingFactory] [ MessagingFactory] hataları ya da hatalı, iş ortağı kapalı veya kapalı ve Sifon bu durumu algılar. Sifon işlevi görür. Varsa diğer [MessagingFactory] [ MessagingFactory] kapalı 5 saniye içinde hala açık Sifon hataları [MessagingFactory][MessagingFactory].

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [zaman uyumsuz desenleri ve yüksek kullanılabilirlik Mesajlaşma] [ Asynchronous messaging patterns and high availability] ayrıntılı bir irdelemesi ve Service Bus Mesajlaşma hizmetinin zaman uyumsuz. 

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
