---
title: "Azure Service Bus ileti aktarımları, kilitler ve kapatma | Microsoft Docs"
description: "Hizmet veri yolu ileti aktarımları ve kapatma işlemleri genel bakış"
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
ms.date: 09/27/2017
ms.author: sethm
ms.openlocfilehash: edb6e207852fa59d5828906c891693f367739c9c
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="message-transfers-locks-and-settlement"></a>İleti aktarımı, kilitler ve kapatma

En merkezi bir ileti aracısı hizmet veri yolu gibi kuyruk veya konu iletileri kabul edip sonraki alınamayabilir tutmak için bir özelliktir. *Gönderme* bir ileti aktarımı içine ileti aracısı için yaygın olarak kullanılan bir terimdir. *Alma* olan bir ileti transfer alınırken bir istemci için yaygın olarak kullanılan terim.

Bir istemci bir ileti gönderdiğinde, genellikle olup ileti olduğundan düzgün bir şekilde aktarılır ve aracısı tarafından kabul veya hata çeşit oluşup oluşmadığını bilmek ister. Bu olumlu veya olumsuz bildirim istemcisi ve ileti aktarım durumu hakkında anlama Aracısı kapatır ve bu nedenle olarak adlandırılır *kapatma*.

Aracısı için bir istemci bir ileti aktardığında, benzer şekilde, aracısı ve istemci ileti başarıyla işlendi ve bu nedenle kaldırılabilir veya olup ileti teslimi veya işlem başarısız oldu, bir anlayış oluşturmak istediğiniz ve bu nedenle ileti yeniden teslim gerekebilir.

## <a name="settling-send-operations"></a>Gönderme işlemleri kapatma

Desteklenen hizmet veri yolu API'sini istemcilerden herhangi biri kullanarak, Service Bus işlemlere her zaman açıkça kapatılır API işlemi ulaşması için Service Bus kabul sonucundan bekler, yani gönderin ve gönderme işlemini tamamlar.

İletinin hizmet veri yolu tarafından reddedilirse, reddetme bir hata göstergesi ve bir "izleme-id" içindeki metinle içerir. Reddetme ayrıca işlemi başarı herhangi Beklenti denenebilir hakkında bilgiler içerir. İstemcide, bu bilgileri bir özel durum açık ve gönderme işlemi çağırana oluşturulur. İleti onayladığınızda işlemi sessizce tamamlar.

Özel protokol .NET standart istemci ve Java istemcisi için AMQP protokolünü kullanırken ve [.NET Framework istemci için bir seçenek olan](service-bus-amqp-dotnet.md), ileti aktarımları ve kapatmaları ardışık ve tamamen zaman uyumsuz ve zaman uyumsuz programlama modeli API çeşitler kullanmanız önerilir.

Bir gönderici çeşitli iletiler kablo hızlı art arda Doğrulamanızın kabul edilmesi, her ileti için HTTP 1.1 veya SBMP protokolü ile çalışması Aksi durumda olacak şekilde beklemek zorunda kalmadan koyabilirsiniz. Bu zaman uyumsuz gönderme işlemleri karşılık gelen iletileri kabul edilir ve, bölümlenmiş varlıklarını depolanan gibi tamamlayın veya işlemi farklı varlıkları çakışma gönderdiğinizde. Tamamlamalar özgün gönderme sıralama dışında da oluşabilir.

Gönderme işlemleri sonucunu işlemek için stratejisi, uygulamanız için anında ve önemli performans etkisi olabilir. Bu bölümdeki örnekler C# dilinde yazılmıştır ve eşdeğer için Java vadeli uygulayın.

Uygulama iletilerinin WINS'e üretir, düz bir döngü ile burada gösterilen ve tamamlanmasını beklemek için olan her bir zaman uyumlu sonraki iletiyi göndermeden önce işlemi göndermek veya zaman uyumsuz API şekiller aynı şekilde, 10 iletileri gönderme yalnızca sonra 10 tamamlar sıralı tam gidiş dönüş kapatma için.

Bir varsayılan 70 milisaniyelik Itanium tabanlı sistemler için TCP gidiş dönüş gecikme uzaklığı bir şirket içi siteden kabul edin ve her iletinin depolamak için hizmet veri yolu ve hizmet veri yolu için yalnızca 10 ms vermek, en az 8 yükü aktarım süresini veya potansiyel saymaz saniyeleri, aşağıdaki döngü alır Rota tıkanıklık etkiler:

```csharp
for (int i = 0; i < 100; i++)
{
  // creating the message omitted for brevity
  await client.SendAsync(…);
}
```

Uygulama 10 zaman uyumsuz gönderme işlemleri hemen art arda başlar ve bunların ilgili tamamlama ayrı olarak bekler, bu 10 gönderme işlemleri için gidiş dönüş süresi ile çakışıyor. 10 iletiler, büyük olasılıkla bile TCP çerçeveler, paylaşımı hemen sırayla aktarılır ve genel aktarım süresini Aracısı aktarılan iletileri almak için gereken ağ ilgili süreyi büyük ölçüde bağlıdır.

Önceki döngü için olduğu gibi aynı varsayımlar yapmadan, aşağıdaki döngü için toplam çakışan yürütme süresini de altında bir saniye kalabilir:

```csharp
var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  tasks.Add(client.SendAsync(…));
}
await Task.WhenAll(tasks.ToArray());
```

Tüm zaman uyumsuz programlama modelleri bekleyen işlemler tutan bellek tabanlı, gizli çalışma sırası çeşit kullanmak dikkate almak önemlidir. Zaman [SendAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.sendasync#Microsoft_Azure_ServiceBus_QueueClient_SendAsync_Microsoft_Azure_ServiceBus_Message_) (C#) veya **Gönder** (Java) iade, gönderme görev kuyruğa alınır o iş kuyruğundaki ancak çalıştırmak için görev sırası geldi sonra Protokolü hareketi yalnızca gönderir. Bunlar factually kablo konmuş kadar bellekte gönderilen tüm iletiler almak için çok fazla ileti "Uçuş modunda" aynı anda put WINS'e iletilerinin ve güvenilirlik ilgili bir sorun olduğu itme eğilimindedir kodunu dikkatli olunmalıdır.

Semafor, C# ' ta, aşağıdaki kod parçacığında gösterildiği gibi bu tür uygulama düzeyi gerektiğinde azaltmayı etkinleştirmek eşitleme nesneleridir. Bu kullanımı semafor, en fazla 10 iletileri aynı anda uçuş modunda olmasını sağlar. 10 kullanılabilir semafor kilitler birini send önce alınır ve gönderme tamamladıkça serbest bırakılır. Önceki en az biri gönderene kadar döngü bekler 11 geçiş tamamladı ve ardından, kilit kullanılabilir hale getirir:

```csharp
var semaphore = new SemaphoreSlim(10);

var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  await semaphore.WaitAsync();

  tasks.Add(client.SendAsync(…).ContinueWith((t)=>semaphore.Release()));
}
await Task.WhenAll(tasks.ToArray());
```

Uygulamaları gereken **hiçbir zaman** işlemi sonucunu alma olmadan bir "yangın ve unut" şekilde bir zaman uyumsuz gönderme işlemi başlatın. Bunun yapılması bellek tükendi kadar iç ve görünmez görev sırası yüklemek ve gönderme hataları algılama uygulamanın engelle:

```csharp
for (int i = 0; i < 100; i++)
{

  client.SendAsync(message); // DON’T DO THIS
}
```

Alt düzey bir AMQP istemcisi ile Service Bus "önceden kapatılmış" aktarımları de kabul eder. Önceden kapatılmış aktarım gönderildiğinde kapatılan kendisi için sonucu, her iki durumda da geri istemci ve ileti bildirilmedi yangın ve unut işlemi kabul yöntemidir. İstemciye geri bildirim eksikliği hiçbir veri bu modu değil nitelemek için Azure Destek aracılığıyla Yardım anlamına gelir tanılama için kullanılabilir olduğunu anlamına gelir.

## <a name="settling-receive-operations"></a>Alma işlemlerinin kapatma

Alma işlemlerinin, hizmet veri yolu API'sini istemciler iki farklı açık modunu etkinleştirme: *alma ve silme* ve *gözlem kilidinin*.

[Alma ve silme](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu bildiren tüm iletileri gönderir alıcı istemci olarak kapatılan ne zaman dikkate alınması gereken Aracısı gönderilir. İleti Aracısı kablo getirdi hemen tüketilen değerlendirilir anlamına gelir. İleti aktarımı başarısız olursa, ileti kaybolur.

Bu mod baş alıcı ileti üzerinde daha fazla eylem gerekmez ve ayrıca kapatma sonucu için bekleyerek yavaş değil ' dir. Tek bir ileti bulunan verileri düşük değere sahip ve/veya yalnızca çok kısa bir süre için anlamlı, bu mod makul bir seçimdir.

[Gözlem kilidinin](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu alıcı istemci alınan iletiler açıkça kapatılacak istediği Aracısı söyler. İleti alıcı, hizmet özel bir kilit altında rakip, diğer alıcılar görmemesi böylece tutulan sırada işlemek kullanılabilir hale getirilir. Kilit süresi kuyruk veya abonelik düzeyinde başlangıçta tanımlanır ve aracılığıyla kilit sahibi olan istemci tarafından Genişletilmiş [RenewLock](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_) işlemi.

Bir ileti kilitlendiğinde aynı sıra ya da abonelik alan diğer istemcilerin kilit alabilir ve etkin kilit altında değil sonraki kullanılabilir iletileri almak. Bir ileti kilidi açıkça serbest bırakıldığında veya kilit süresi dolduğunda, ileti veya yeniden teslim alma siparişi önüne yakın POP yedekleyin.

İleti alıcı tarafından sürekli olarak yayımlanan veya tanımlı kaç kez geçmesini kilit izin olduğunda ([maxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount)), ileti otomatik olarak sıra veya abonelik kaldırılır ve ilişkili yerleştirilir sahipsiz sıra.

Çağırdığında alıcı istemci pozitif bildirim ile alınan iletinin kapatma başlatır [tam](/dotnet/api/microsoft.servicebus.messaging.queueclient.complete#Microsoft_ServiceBus_Messaging_QueueClient_Complete_System_Guid_) API düzeyinde. Bu iletiyi başarıyla işledi ve ileti sıra veya abonelik kaldırılır Aracısı gösterir. Alıcının kapatma hedefi kapatma gerçekleştirilebilir olup olmadığını belirten bir yanıt ile Aracısı yanıtlar.

Alıcı istemcisi bir iletiyi işlemek tamamlanamazsa, ancak ileti yeniden teslim edilebilir istediği zaman, açıkça serbest ve anında çağırarak kilidi iletiye isteyebilir [Abandon](/dotnet/api/microsoft.servicebus.messaging.queueclient.abandon) veya hiçbir şey yapabilirsiniz ve geçmesini kilit izin verin.

Bir alıcı istemcisi bir iletiyi işlemek başarısız olur ve bu ileti redelivering bilir ve işlemi yeniden denemeden değil yardımcı olur, çağırarak sahipsiz sıraya taşır ileti reddedebilirsiniz [sahipsiz](/dotnet/api/microsoft.servicebus.messaging.queueclient.deadletter), hangi de Teslim edilemeyen kuyruğundan ileti ile alınabilmesi için bir neden kodu da dahil olmak üzere özel bir özellik ayarlama sağlar.

Özel bir kapatma ayrı bir makalede açıklanan erteleme durumdur.

**Tam** veya **sahipsiz** işlemlerinin yanı sıra **RenewLock** operations tutulan kilit süresi dolmuş ya da var olan diğer ağ sorunları nedeniyle başarısız olabilir kapatma önlemek Hizmet tarafı koşulları. İkinci durumda her birinde hizmeti olumsuz bildirim bu yüzeyleri API istemciler bir özel durum olarak gönderir. Bunun nedeni bozuk ağ bağlantısı ise, hizmet veri yolu üzerinde farklı bir bağlantı mevcut AMQP bağlantılar kurtarılmasını desteklemediğinden kilidi bırakılır.

Varsa **tam** genellikle ileti işleme ve bazı durumlarda en sonunda işleme iş dakika sonra alma işlemini yapan uygulamanın iş durumunu saklayan ve aynı yoksayar olup olmadığını karar oluşur başarısız oluyor ikinci kez teslim edilir veya olup iş sonuç tosses ve iletiyi olarak yeniden deneme yeniden teslim iletisi.

Yinelenen ileti teslimleri tanımlamak için tipik ileti-ve muhtemelen kaynak işleminden bir tanımlayıcıyla hizalı benzersiz bir değere, gönderen tarafından ayarlanmalıdır kimliği kontrol ederek mekanizmadır. İş Zamanlayıcı bir olasılıkla ileti kimliği verilen çalışan ile çalışan atamak için çalışıyor işinin tanıtıcısı ayarlayın ve bu iş zaten yapıldığında çalışan iş atama ikinci oluşum yoksay.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
