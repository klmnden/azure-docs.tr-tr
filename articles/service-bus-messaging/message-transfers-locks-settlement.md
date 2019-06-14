---
title: Azure Service Bus ileti aktarımları, kilitler ve kapatma | Microsoft Docs
description: Service Bus ileti aktarımları ve kapatma işlemleri genel bakış
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
ms.date: 09/25/2018
ms.author: aschhab
ms.openlocfilehash: a78409a15acb4e60fc4200778d0f33b3fb566e85
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60403950"
---
# <a name="message-transfers-locks-and-settlement"></a>İleti aktarımları, kilitler ve kapatma

Service Bus gibi bir ileti Aracısı merkezi yeteneğini, kuyruk veya konuda ileti kabul etmek ve sonraki alınamayabilir tuttuğunuza oluşturmaktır. *Gönderme* iletisinin aktarımı içine ileti aracısı için yaygın olarak kullanılan bir terimdir. *Alma* alınırken bir istemci bir ileti aktarımını için yaygın olarak kullanılan bir terimdir.

Bir istemci bir ileti gönderdiğinde, genellikle olup ileti olduğundan düzgün bir şekilde aktarılır ve aracı tarafından kabul veya hata olup olmadığını oluştu bilmek istemektedir. Bu olumlu veya olumsuz bildirim istemci ile ileti aktarım durumu hakkında anlama Aracısı kapatır ve bu nedenle olarak adlandırılır *kapatma*.

Aracıya bir ileti istemciye aktardığında, benzer şekilde, aracısı ve istemci ileti başarıyla işlendi ve bu nedenle kaldırılabilir veya olup ileti teslimi veya işleme başarısız oldu, bir anlayış oluşturmak istediğiniz ve bu nedenle ileti yeniden teslim edilebilir gerekebilir.

## <a name="settling-send-operations"></a>Gönderme işlemleri sonlandırma

Desteklenen istemciler hizmet veri yolu API'sini kullanarak, Service Bus işlemi her zaman açık kapatılır API işlemi bir Service Bus'ın ulaşması için kabul sonuçtan bekler, yani göndermek ve sonra gönderme işlemi tamamlar.

İletinin hizmet veri yolu tarafından reddedilirse, reddetme bir hata göstergesi ve "İzleme-kimliği ile" içindeki metni içerir. Reddetme ayrıca herhangi bir başarı beklentisiyle işlemi olup olmadığını denenebilir hakkında bilgi içerir. İstemcisinde, bu bilgiler bir özel durum açılır ve gönderme işlemi, çağırana oluşturulur. İletiyi kabul işlem sessiz bir şekilde tamamlar.

.NET Standard istemci hem de Java istemcisi için özel bir protokoldür ve AMQP protokolünü kullanırken ve [.NET Framework istemci için bir seçenek olduğu](service-bus-amqp-dotnet.md), ileti aktarımları ve kapatmaları ardışık ve tamamen zaman uyumsuz ve zaman uyumsuz programlama modeli API çeşitler kullanmanız önerilir.

Bir gönderici birkaç ileti kablo hızlı art arda SBMP protokolü ile veya HTTP 1.1 ile çalışması Aksi durumda olacak şekilde Doğrulamanızın kabul edilmesi her ileti için beklemek zorunda kalmadan koyabilirsiniz. Bu zaman uyumsuz gönderme işlemleri karşılık gelen iletileri kabul edilir ve, bölümlenmiş varlıklarda depolanan olarak tamamlamak veya işlemi farklı varlıklar çakışma için gönderdiğinizde. Tamamlamalar, özgün gönderme sırası dışında de oluşabilir.

Gönderme işlemleri sonucunu işlemek için bir strateji, uygulamanız için hemen ve önemli performans etkisi olabilir. Bu bölümdeki örneklerde, C# dilinde yazılmıştır ve eşdeğer Java vadeli için geçerlidir.

Uygulama iletileri ani artışlara üretir, düz bir döngü ile burada gösterildiği ve tamamlanmasını beklemek için olan her zaman uyumlu sonraki iletiyi göndermeden önce işlem Gönder veya zaman uyumsuz API şekiller aynı şekilde, 10 ileti gönderme yalnızca 10 sonra tamamlar sıralı tam gidiş dönüş için kapatma.

Bir varsayılan 70 milisaniyelik Itanium tabanlı sistemler için TCP gidiş dönüş gecikmesi uzaklığı bir şirket içi siteden Service Bus ve Service Bus için yalnızca 10 ms vererek kabul edin ve her bir iletiyi depolamak için aşağıdaki döngü en az 8 yükü aktarım süresini veya potansiyel sayılmaz saniye, alır. Rota Tıkanıklığı etkiler:

```csharp
for (int i = 0; i < 100; i++)
{
  // creating the message omitted for brevity
  await client.SendAsync(…);
}
```

Gidiş dönüş süresi 10 gönderme işlemleri için uygulamayı hemen art arda 10 zaman uyumsuz gönderme işlemi başlatır ve ayrı olarak ilgili bunların tamamlanmasını bekler, ile çakışıyor. 10 iletinin bile büyük olasılıkla TCP çerçeveler, paylaşım anlık art arda aktarılır ve genel aktarım süresi aracıya aktarılan iletileri almak için ağ ile ilgili süresini büyük ölçüde bağlıdır.

Aşağıdaki döngü çakışan toplam yürütme süresi, önceki döngü olduğu gibi aynı varsayımlar yapmak, iyi altında bir saniye kalabilir:

```csharp
var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  tasks.Add(client.SendAsync(…));
}
await Task.WhenAll(tasks);
```

Tüm zaman uyumsuz programlama modeli bekleyen işlemler tutan bellek tabanlı, gizli iş kuyruğunuzu çeşit kullandığını unutmayın. Zaman [SendAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.sendasync#Microsoft_Azure_ServiceBus_QueueClient_SendAsync_Microsoft_Azure_ServiceBus_Message_) (C#) veya **Gönder** (Java) iade, gönderme görev sıraya koyulur, çalışma sırasında ancak sonra çalıştırılacak görev dönüş Protokolü hareket yalnızca başlar. Bunlar factually kablo koyulmuş kadar gönderilen tüm iletiler belleği çünkü çok fazla ileti "uçuşta" tek seferde put anında iletme iletileri ve güvenilirlik önemli olduğu artışları eğilimindedir kod dikkat edilmelidir.

Semafor, C# ' ta, aşağıdaki kod parçacığında gösterildiği gibi uygulama düzeyinde gerektiğinde azaltmayı etkinleştirmek eşitleme nesnelerdir. Semafor bu kullanımı en fazla 10 iletileri aynı anda uçuş modunda olmasını sağlar. 10 mevcut semafor kilitler birini önce Gönder alınır ve Gönder tamamladıkça serbest bırakılır. Önceki en az biri gönderene kadar döngü bekler 11 geçiş tamamlandı ve sonra onun kilidini kullanılabilir hale getirir:

```csharp
var semaphore = new SemaphoreSlim(10);

var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  await semaphore.WaitAsync();

  tasks.Add(client.SendAsync(…).ContinueWith((t)=>semaphore.Release()));
}
await Task.WhenAll(tasks);
```

Uygulamalar **hiçbir zaman** işlemin sonucunu almadan "Başlat ve unut" bir şekilde bir zaman uyumsuz gönderme işlemi başlatın. Bunun yapılması bellek tükendi kadar iç ve görünmez görev sırası yüklemek ve gönderme hataları algılama uygulamanın engelle:

```csharp
for (int i = 0; i < 100; i++)
{

  client.SendAsync(message); // DON’T DO THIS
}
```

Alt düzey bir AMQP istemcisi ile Service Bus "önceden kapatılmış" aktarımlarına de kabul eder. Önceden kapatılmış aktarım gönderildiğinde kapatılan bir Başlat ve unut işlemi için sonucu, her iki durumda da geri istemci ile ileti bildirilmedi kabul yöntemidir. İstemciye geri bildirim eksikliği hiçbir eyleme dönüştürülebilir veriler Bu mod değil nitelemek için Azure desteği aracılığıyla Yardım anlamına gelir ve tanılama için kullanılabilir olduğunu anlamına gelir.

## <a name="settling-receive-operations"></a>Alma işlemleri sonlandırma

Alma işlemleri için hizmet veri yolu API'sini istemcileri iki farklı açık modunu etkinleştirin: *Alma ve silme* ve *gözlem kilidi*.

[Alma ve silme](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu bildiren tüm iletileri gönderir alıcı istemciye kapatılan olarak ne zaman dikkate alınması gereken aracı gönderilir. İleti Aracısı kablo getirdi hemen sonra tüketilen değerlendirilir anlamına gelir. İleti aktarımı başarısız olursa, ileti kaybolur.

Bu mod, baş alıcı iletide başka bir işlem yapması gerekmez ve ayrıca mutabakat sonucunu bekleyen tarafından yavaş değil ' dir. Tek tek iletilerinde yer alan verileri düşük bir değere sahip ve/veya yalnızca kısa bir süre için anlamlı olan, bu mod makul bir seçimdir.

[Gözlem kilidi](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu alıcı istemci alınan iletiler açıkça kapatılacak istediğini Aracısı söyler. İleti alıcı, diğer, rakip alıcılar tarafından görülmez. böylece, özel bir kilit hizmetinde altında tutulan sırada işlemek kullanılabilir. Kilit süresi kuyruk veya abonelik düzeyinde başlangıçta tanımlanır ve kilit aracılığıyla sahip olan istemci tarafından ilave [RenewLock](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_) işlemi.

Bir ileti kilitliyken aynı kuyruk veya abonelikten alma diğer istemciler kilitler alabilir ve etkin kilidi altında değil bir sonraki kullanılabilir iletileri alma. Bir iletinin kilidini açıkça serbest bırakıldığında veya kilit süresi dolduğunda, ileti veya yeniden teslim alma sırası önündeki yakın POP yedekleyin.

Ne zaman ileti art arda alıcılar tarafından yayımlanan veya tanımlı kaç kez geçmesini kilit sağlarlar ([maxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount)), ileti otomatik olarak kuyruk veya abonelikten kaldırılır ve ilişkili yerleştirilir. eski ileti sırası.

Çağırdığında, pozitif bir bildirim ile alınan iletinin kapatma alıcı istemci başlatır [tam](/dotnet/api/microsoft.servicebus.messaging.queueclient.complete#Microsoft_ServiceBus_Messaging_QueueClient_Complete_System_Guid_) API düzeyinde. Bu, aracıya ileti başarıyla işlendi ve kuyruk veya abonelikten ileti silinir gösterir. Aracı için alıcının kapatma hedefi kapatma gerçekleştirilebilir olup olmadığını gösteren bir yanıt ile yanıtlar.

Alıcı istemcisi bir iletiyi işlemekte başarısız halde ileti yeniden teslim edilebilir istediği, onu açıkça serbest ve anında çağırarak kilidi ileti isteyebilir [Abandon](/dotnet/api/microsoft.servicebus.messaging.queueclient.abandon) veya hiçbir şey yapabilirsiniz ve kilit geçmesini sağlayabilirsiniz.

Alıcı bir istemci bir iletiyi işlemek başarısız olur ve bu iletiyi redelivering bilir ve işlemi yeniden denemeden değil yardımcı olur, çağırarak eski ileti kuyruğuna taşır iletiyi reddedebilir [teslim edilemeyen iletiler](/dotnet/api/microsoft.servicebus.messaging.queueclient.deadletter), ayrıca eski ileti sırası iletiden alınabilir bir neden kodu içeren özel bir özellik ayarlamaya izin verir.

Özel bir durum kapatma, ayrı bir makalede ele erteleme oluşturur.

**Tam** veya **teslim edilemeyen iletiler** işlemlerinin yanı sıra **RenewLock** tutulan bir kilidin süresi doldu veya diğer vardır, işlemleri ağ sorunları nedeniyle başarısız kapatma önlemek Hizmet tarafı koşulları. İkinci durumda her birinde hizmeti olumsuz bildirim bu yüzeyleri bir özel API istemcileri olarak gönderir. Bunun nedeni bozuk ağ bağlantısı, Service Bus üzerinde farklı bir bağlantı mevcut AMQP bağlantıları kurtarılmasını desteklemediğinden kilit bırakılır.

Varsa **tam** genellikle ileti işleme ve bazı durumlarda en sonunda işleme iş dakika sonra alıcı uygulamanın çalışma durumunu korur ve aynı yoksayar olmadığını karar verebilir gerçekleşen başarısız ikinci kez teslim edilir veya olup iş sonucu tosses ve mesaj olarak yeniden deneme yeniden teslim iletisi.

Yinelenen ileti teslimat tanımlamak için tipik ileti gönderen bir tanımlayıcıyla kaynak işlemi muhtemelen hizalanmış, benzersiz bir değere ayarlanması gerekir ve kimliği, kontrol ederek mekanizmadır. İş Zamanlayıcı tanımlayıcı bir çalışan verilen çalışanla atamak için çalışan işin büyük olasılıkla ileti kimliği ayarlamalı ve bu işi zaten yapıldıysa çalışan iş atama ikinci oluşum yok.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
