---
title: "En iyi uygulamalar Azure Service Bus kullanarak performansı artırmak için | Microsoft Docs"
description: "Service Bus aracılı ileti alışverişi sırasında performansı iyileştirmek için nasıl kullanılacağını açıklar."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e756c15d-31fc-45c0-8df4-0bca0da10bb2
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2018
ms.author: sethm
ms.openlocfilehash: be702f0b08ce14012db9da10d874031c7a5a562b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Service Bus Mesajlaşma hizmeti kullanarak performans iyileştirmeleri için en iyi yöntemler

Bu makalede nasıl kullanılacağını açıklar [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) aracılı ileti alışverişi sırasında performansı iyileştirmek için. Bu makalenin ilk bölümü performansını artırmaya yardımcı olmak için sunulan farklı mekanizmaları açıklar. İkinci bölümü, belirli bir senaryo en iyi performans sunabilir şekilde Service Bus hizmetini kullanmak hakkında yönergeler sağlar.

Bu konu boyunca terimi "istemci" Service Bus erişen herhangi bir varlığa anlamına gelir. Bir istemci bir gönderici veya bir alıcı rolüne alabilir. "Gönderen" terimi, Service Bus kuyruk veya konu başlığı aboneliği iletileri gönderen bir Service Bus kuyruk veya konu istemci için kullanılır. "Alıcı" terimi, bir hizmet veri yolu kuyruğu ya da abonelik iletileri alan Service Bus kuyruğu veya abonelik istemci anlamına gelir.

Bu bölümler performansı artırmaya yardımcı olmak için hizmet veri yolu kullanır birkaç kavramları tanıtır.

## <a name="protocols"></a>Protokoller

Hizmet veri yolu, istemcilerin üç protokolden birini aracılığıyla iletileri almasına ve göndermesine olanak sağlar:

1. Gelişmiş Message Queuing Protokolü (AMQP)
2. Service Bus Mesajlaşma protokolünü (SBMP)
3. HTTP

Mesajlaşma fabrikası var olduğu sürece, hizmet veri yolu bağlantı korumak için AMQP ve SBMP daha etkili olurlar. Ayrıca, toplu işleme ve prefetching uygular. Açıkça belirtilmediği sürece, bu konudaki tüm içeriğin AMQP veya SBMP kullanımını varsayar.

## <a name="reusing-factories-and-clients"></a>Oluşturucular ve istemcilerin yeniden kullanma

Service Bus istemci nesneleri, gibi [QueueClient] [ QueueClient] veya [MessageSender][MessageSender], aracılığıyla oluşturulan bir [Eventhubclient] [ MessagingFactory] bağlantılarının iç yönetimi de sağlayan nesne. İleti gönderme ve sonraki iletiyi gönderdiğinizde, ardından yeniden oluşturduktan sonra ileti oluşturucuları veya kuyruk, konu ve abonelik istemcileri kapatmalısınız değil. Bir Mesajlaşma fabrikası kapatma Service Bus hizmeti bağlantısı siler ve Fabrika yeniden zaman yeni bir bağlantı kurulur. Bir bağlantı kurarak, aynı Fabrika ve birden çok işlemleri için istemci nesneleri yeniden kullanarak önleyebilirsiniz pahalı bir işlemdir. Güvenli bir şekilde kullanabilirsiniz [QueueClient] [ QueueClient] eşzamanlı zaman uyumsuz işlemleri ve birden çok iş parçacığı ileti göndermek için nesne. 

## <a name="concurrent-operations"></a>Eşzamanlı operasyonlar

Bir işlemi gerçekleştirilirken (gönderme, alma, silme, vb.) biraz zaman alabilir. Bu süre, istek ve yanıt gecikmesi ek olarak Service Bus hizmeti tarafından işlemi işlenmesini içerir. Saat başına işlem sayısını artırmak için işlemler aynı anda yürütmeniz gerekir. Birkaç farklı yolla bunu yapabilirsiniz:

* **Zaman uyumsuz işlemleri**: zaman uyumsuz işlemleri gerçekleştirerek istemci işlemleri zamanlar. Bir sonraki istekte, önceki isteği tamamlanmadan önce başlatılır. Bir zaman uyumsuz gönderme işleminin bir örnek verilmiştir:
  
 ```csharp
  BrokeredMessage m1 = new BrokeredMessage(body);
  BrokeredMessage m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  Bu bir örnektir zaman uyumsuz bir alma işlemi:
  
  ```csharp
  Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  
  Task.WaitAll(receive1, receive2);
  Console.WriteLine("All messages received");
  
  async void ProcessReceivedMessage(Task<BrokeredMessage> t)
  {
    BrokeredMessage m = t.Result;
    Console.WriteLine("{0} received", m.Label);
    await m.CompleteAsync();
    Console.WriteLine("{0} complete", m.Label);
  }
  ```

* **Birden çok oluşturucuları**: bir TCP bağlantı aynı fabrikası tarafından oluşturulan tüm istemciler (göndericiler alıcıları ek olarak) paylaşır. En fazla ileti işleme bu TCP bağlantısı üzerinden gidebilirsiniz işlemlerinin sayısı sınırlıdır. Tek bir Fabrika ile elde edilebilir verimlilik TCP iletim süreleriyle ve ileti boyutu olmadığına göre değişir. Daha yüksek performans oranına elde etmek için birden çok Mesajlaşma oluşturucuları kullanmanız gerekir.

## <a name="receive-mode"></a>Mod alma

Bir kuyruk veya abonelik istemci oluştururken, alma modu belirtebilirsiniz: *gözlem kilidinin* veya *alma ve silme*. Varsayılan alma modu olan [PeekLock][PeekLock]. Bu modda çalışırken, istemci hizmeti yolundan bir ileti almak için bir istek gönderir. İstemci iletiyi aldıktan sonra iletiyi tamamlamak için bir istek gönderir.

Alma modu ayarlarken [ReceiveAndDelete][ReceiveAndDelete], her iki adım tek bir istekte birleştirilir. Bu işlem genel sayısını azaltır ve genel ileti üretilen işi artırabilir. Bu performans kazancı iletileri kaybetme at the risk of gelir.

Hizmet veri yolu, alma ve silme işlemleri için işlemleri desteklemiyor. Ayrıca, gözlem kilidinin semantiği, istemcinin istediği erteleme herhangi senaryoları için gerekli veya [sahipsiz](service-bus-dead-letter-queues.md) bir ileti.

## <a name="client-side-batching"></a>İstemci-tarafı toplu işleme

İstemci-tarafı toplu bir ileti gönderme belirli bir süre geciktirmek bir kuyruk veya konu istemci sağlar. İstemci bu süre içinde ek iletiler gönderirse, tek bir toplu iletileri iletir. İstemci-tarafı toplu işleme ayrıca neden olan birden çok toplu sıra veya abonelik bir istemci **tam** tek bir istek isteklerine. Toplu işleme yüklenebilir yalnızca zaman uyumsuz **Gönder** ve **tam** işlemleri. Zaman uyumlu işlemler hemen Service Bus hizmetine gönderilir. Toplu işleme için gözlem oluşur veya değil alma işlemleri ya da toplu işleme istemciler arasında etmiyorsa.

Varsayılan olarak, istemci bir toplu iş aralığı 20ms kullanır. Toplu iş aralığı ayarlayarak değiştirebileceğiniz [BatchFlushInterval] [ BatchFlushInterval] Mesajlaşma fabrikası oluşturmadan önce özelliği. Bu ayar bu fabrikası tarafından oluşturulan tüm istemcilerini etkiler.

Toplu işleme devre dışı bırakmak için ayarlanmış [BatchFlushInterval] [ BatchFlushInterval] özelliğine **değeri, TimeSpan.Zero**. Örneğin:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Toplu işleme Faturalanabilir Mesajlaşma işlemlerinin sayısını etkilemez ve yalnızca hizmet veri yolu istemci protokolü için kullanılabilir. Toplu işleme HTTP protokolünü desteklemiyor.

## <a name="batching-store-access"></a>Mağaza erişimi toplu işleme

Bir kuyruk, konu veya abonelik verimliliğini artırmak için iç depolama alanına yazdığında birden fazla ileti Service Bus. toplu iş. Bir kuyruk veya konu etkinleştirilirse, ileti deposuna yazma toplu hale. Bir kuyruk veya abonelik etkinleştirilirse, mağaza'dan iletilerini silerken toplu hale. Toplu depolama erişimi için bir varlık etkinleştirilirse, hizmet veri yolu en fazla 20ms tarafından deposu yazma işlemi varlığın ilgili geciktirir. Bu zaman aralığı boyunca gerçekleşen ek depolama işlem toplu eklenir. Toplu depolama erişim yalnızca etkiler **Gönder** ve **tam** işlemleri; alma işlemleri etkilenmez. Toplu depolama erişim bir varlığı üzerinde bir özelliktir. Toplu işleme toplu depolama erişimi etkinleştir tüm varlıklar arasında oluşur.

Yeni Kuyruk, konu veya abonelik oluştururken, toplu depolama erişim varsayılan olarak etkindir. Toplu iş mağazası erişimini devre dışı bırakmak için ayarlanmış [EnableBatchedOperations] [ EnableBatchedOperations] özelliğine **false** varlık oluşturmadan önce. Örneğin:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Toplu depolama erişim Faturalanabilir Mesajlaşma işlemlerinin sayısını etkilemez ve kuyruk, konu veya abonelik bir özelliğidir. Alma modu ve bir istemci ve Service Bus hizmeti arasında kullanılan protokol bağımsızdır.

## <a name="prefetching"></a>Prefetching

[Prefetching](service-bus-prefetch.md) alma işlemi gerçekleştirdiğinde, ek iletiler hizmetinden yüklemek sıra veya abonelik istemci sağlar. İstemci bu iletiler bir yerel önbellekte depolar. Önbellek boyutu tarafından belirlenir [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] veya [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] özellikleri. Prefetching sağlayan her bir istemci kendi önbelleğini korur. Bir önbellek istemcileri arasında paylaşılmaz. İstemci bir alma işlemi başlatır ve önbelleğinde boş ise, hizmet toplu iletiler iletir. Toplu iş boyutu 256 KB ve önbellek boyutunu eşittir, hangisi daha küçüktür. İstemci bir alma işlemi başlatır ve bir ileti önbellek içeriyorsa, ileti önbellekten alınır.

Bir ileti prefetched, hizmet prefetched ileti kilitler. Bunu yaparak, farklı bir alıcı tarafından prefetched ileti alınamıyor. Kilit süresi dolmadan önce alıcı iletiyi tamamlanamazsa, iletinin diğer alıcılar için kullanılabilir hale gelir. İletinin prefetched kopyasını önbellekte kalır. Bu iletiyi tamamlamak çalıştığında, süresi dolan önbelleğe alınan kopya tüketir alıcı bir özel durum alır. Varsayılan olarak, ileti kilidi 60 saniye sonra süresi dolar. Bu değer 5 dakika olarak genişletilebilir. Süresi dolan iletileri kullanımını önlemek için önbellek boyutu her zaman bir istemci tarafından kilit zaman aşımı aralığı içinde kullanılabilecek iletileri sayısından daha küçük olmalıdır.

Varsayılan kilitleme sona erme tarihi 60 saniye kullanırken, iyi bir değer için [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] 20 kez maksimum işliyor Factory tüm alıcıları oranları. Örneğin, bir Fabrika 3 alıcıları oluşturur ve her alıcı saniye başına en fazla 10 iletileri işleyebilir. Önceden getirme sayısı, 20 X 3 X 10 = 600 aşamaz. Varsayılan olarak, [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] hiçbir ek iletiler hizmetinden getirilen yani 0 olarak ayarlayın.

Mesaj işlemleri ya da gidiş dönüş genel sayısını azalttığı iletileri prefetching bir kuyruk veya abonelik için genel üretilen işi artırır. İlk iletinin getirme, ancak, bu kadar (daha fazla ileti boyutu nedeniyle) daha uzun sürer. Bu iletiler istemci tarafından önceden yüklenmiş olan çünkü prefetched iletileri alma daha hızlı olacaktır.

Bir iletinin yaşam süresi (TTL) özelliği, sunucu ileti istemciye zaman sunucu tarafından denetlenir. İleti alındığında, istemci iletinin TTL özelliği denetlemez. Bunun yerine, iletiyi, ileti TTL ileti istemci tarafından önbelleğe olsa bile geçtiyse alınabilir.

Prefetching Faturalanabilir Mesajlaşma işlemlerinin sayısını etkilemez ve yalnızca hizmet veri yolu istemci protokolü için kullanılabilir. HTTP protokolünü prefetching desteklemez. Prefetching zaman uyumlu ve zaman uyumsuz alma işlemleri için kullanılabilir.

## <a name="express-queues-and-topics"></a>Kuyruklar ve konu başlıkları express

İfade varlıkları yüksek verimlilik ve düşük gecikme senaryoları etkinleştirmek ve standart Mesajlaşma katmanında yalnızca desteklenir. Oluşturulan varlık [Premium ad](service-bus-premium-messaging.md) express seçeneğini desteklemez. Bir kuyruk veya konu, bir ileti gönderilirse express varlıklarıyla ileti hemen Mesajlaşma deposunda depolanmaz. Bunun yerine, bellekte önbelleğe alınır. Bir ileti birden çok birkaç saniye boyunca sırada kalırsa, kararlı depolama, böylece bir kesinti nedeniyle kaybına karşı koruma için otomatik olarak yazılır. İleti bir bellek önbelleğine yazma verimliliğini artırır ve iletiyi gönderen zaman kararlı depolama erişimi olduğundan gecikmesini azaltır. Birkaç saniye içinde tüketilen iletileri Mesajlaşma deposuna yazılmaz. Aşağıdaki örnekte, hızlı bir konu oluşturur.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Kayıp olmaması gereken önemli bilgileri içeren bir ileti hızlı bir varlığa gönderilirse, gönderenin hemen ayarlayarak kararlı depolama iletiye kalıcı hale getirmek için Service Bus zorlayabilirsiniz [ForcePersistence] [ ForcePersistence] özelliğine **doğru**.

> [!NOTE]
> İfade varlıkları işlemleri desteklemez.

## <a name="use-of-partitioned-queues-or-topics"></a>Bölümlenmiş sıraları veya konuları

Dahili olarak, Service Bus aynı düğümde kullanır ve iletileri depolamak bir Mesajlaşma varlığıyla (kuyruk veya konu) için tüm iletileri depolamak ve işlemek için. A [bölümlenmiş kuyruk veya konu](service-bus-partitioning.md), diğer yandan, birden çok düğümüne dağıtılmış ve depoları Mesajlaşma. Bölümlenmiş kuyruklar ve konu başlıkları yalnızca normal kuyruklar ve konu başlıkları daha yüksek verimlilik elde etmek, bunlar ayrıca üst düzey kullanılabilirlik sergiler. Bölümlenmiş bir varlık oluşturmak için [EnablePartitioning] [ EnablePartitioning] özelliğine **doğru**, aşağıdaki örnekte gösterildiği gibi. Bölümlenen varlıklar hakkında daha fazla bilgi için bkz: [bölümlenmiş Mesajlaşma varlıkları][Partitioned messaging entities].

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Birden çok sıraya kullanımı

Bölümlenmiş kuyruk veya konu kullanmak mümkün değil veya beklenen yükü tek bölümlenmiş kuyruk veya konu tarafından işlenemez birden çok Mesajlaşma varlıkları kullanmanız gerekir. Birden çok varlık kullanırken, ayrılmış bir istemci tüm varlıklar için aynı istemci kullanmak yerine her varlık için oluşturun.

## <a name="development-and-testing-features"></a>Geliştirme ve test özellikleri

Hizmet veri yolu olan özellikle geliştirme için kullanılan bir özellik olan **üretim yapılandırmalarında hiçbir zaman kullanılmalıdır**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Yeni kurallar veya filtreler konuya eklendiğinde, kullanabileceğiniz [TopicDescription.EnableFilteringMessagesBeforePublishing][] yeni filtre ifadesi beklendiği gibi çalıştığını doğrulayın.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki bölümlerde, tipik Mesajlaşma senaryolar açıklanmaktadır ve tercih edilen hizmet veri yolu ayarlar alır. Verimi sınıflandırılan küçük (1 saniye başına ileti daha azını), Orta (1 saniye başına ileti veya 100'den az ancak büyük ileti/saniye) ve yüksek (100 iletileri/ikinci veya daha büyük). İstemci sayısı küçük sınıflandırılır (5 veya daha az), Orta (5'ten fazla ancak 20 küçük veya buna eşit) ve büyük (birden çok 20).

### <a name="high-throughput-queue"></a>Yüksek verimlilik sırası

Hedef: tek bir sıraya verimini ekranı kaplamasını sağlayın. Göndericiler ile alıcılar küçük sayısıdır.

* Bölümlenmiş bir sıra, Gelişmiş performans ve kullanılabilirlik için kullanın.
* Genel gönderme oranı sıraya artırmak için birden fazla ileti altyapısı Gönderenler oluşturmak için kullanın. Her gönderen için zaman uyumsuz işlemleri veya birden çok iş parçacığı kullanın.
* Kuyruktan genel alma hızı artırmak için birden fazla ileti altyapısı alıcıları oluşturmak için kullanın.
* İstemci-tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemleri kullanın.
* Toplu işleme aralığı Service Bus istemci iletişim kuralı iletimlerini sayısını azaltmak için 50ms için ayarlayın. Birden çok Gönderenler kullandıysanız, 100ms için toplu aralığını artırın.
* Toplu depolama erişim etkin bırakın. Bu, iletileri kuyruğa yazılabilir genel oranı artırır.
* Önceden getirme sayısı 20 kez en yüksek işleme oranları bir Factory tüm alıcılar için ayarlayın. Bu hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltır.

### <a name="multiple-high-throughput-queues"></a>Birden çok yüksek işleme sırası

Hedef: birden çok sıraların genel üretilen işi en üst düzeye çıkarın. Tek bir sıra işleme, Orta veya yüksek.

Birden çok kuyrukta en yüksek verimlilik elde etmek için tek bir sıraya verimliliğini en üst düzeye çıkarmak için ana hatlarıyla ayarlarını kullanın. Ayrıca, farklı oluşturucuları farklı sıralarından gönderip istemciler oluşturmak için kullanın.

### <a name="low-latency-queue"></a>Düşük gecikme süresi sırası

Hedef: bir kuyruk veya konu uçtan uca gecikme süresi en aza indirin. Göndericiler ile alıcılar küçük sayısıdır. Sıranın işleme küçük veya Orta değil.

* Bölümlenmiş bir kuyruk için geliştirilmiş kullanılabilirlik kullanın.
* İstemci-tarafı toplu işleme devre dışı bırakın. İstemci hemen bir ileti gönderir.
* Toplu iş mağazası erişimini devre dışı bırakın. Hizmet hemen deposuna iletisi yazar.
* Tek bir istemci kullanıyorsanız, 20 kez işleme hızı alıcı için hazırlık sayısını ayarlayın. Hizmet veri yolu istemci protokolü birden fazla ileti aynı anda sıraya ulaşırsa, tüm aynı anda iletir. İstemci bir sonraki iletiyi aldığında, bu iletiyi yerel önbellekte zaten var. Önbellek küçük olmalıdır.
* Birden çok istemci kullanıyorsanız, hazırlık sayısı 0 olarak ayarlayın. Bunu yaparak, ilk istemci hala ilk iletiyi işlerken ikinci istemci ikinci bir ileti alabilir.

### <a name="queue-with-a-large-number-of-senders"></a>Çok sayıda Gönderenler sıraya

Hedef: bir kuyruk veya konu çok sayıda göndericiler ile verimini ekranı kaplamasını sağlayın. Her göndereni Orta oranı içeren iletileri gönderir. Alıcıları küçük sayısıdır.

Service Bus Mesajlaşma varlığı en fazla 1000 eşzamanlı bağlantı sağlar (veya 5000 AMQP kullanarak). Bu sınır ad alanı düzeyinde uygulanır ve ad alanı başına eşzamanlı bağlantı sınırını tarafından konuları/sıraları/abonelikleri tutulabilir. Sıralar için bu numara göndericiler ile alıcılar arasında paylaşılır. Göndericiler için tüm 1000 bağlantıları gerekirse, sıra konu ve tek bir abonelik ile değiştirmeniz gerekir. Abonelik alıcıları gelen ek bir 1000 eşzamanlı bağlantıları kabul eder ancak konu gönderenlerden, en fazla 1000 eşzamanlı bağlantı kabul eder. 1000'den fazla eşzamanlı Gönderenler gerekirse, HTTP üzerinden Service Bus kuralına Gönderenler ileti göndermesi gerekir.

Verimliliği en üst düzeye çıkarmak için aşağıdakileri yapın:

* Bölümlenmiş bir sıra, Gelişmiş performans ve kullanılabilirlik için kullanın.
* Farklı bir işlem içinde her göndereni bulunuyorsa, işlem başına yalnızca tek bir Fabrika kullanın.
* İstemci-tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemleri kullanın.
* Hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltmak için 20ms aralığını yığınlama varsayılan kullanın.
* Toplu depolama erişim etkin bırakın. Hangi iletileri kuyruk veya konu yazılabilir genel hızını artırır.
* Önceden getirme sayısı 20 kez en yüksek işleme oranları bir Factory tüm alıcılar için ayarlayın. Bu hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltır.

### <a name="queue-with-a-large-number-of-receivers"></a>Çok sayıda alıcı sırası

Hedef: sıra ya da çok sayıda alıcı abonelikle alma hızı en üst düzeye çıkarın. Her alıcı Orta hızında iletilerini alır. Göndericiler küçük sayısıdır.

Service Bus varlık için en fazla 1000 eşzamanlı bağlantı sağlar. Bir kuyruk 1000'den fazla alıcıları gerektiriyorsa, sıranın bir konu ve birden çok abonelik ile değiştirmeniz gerekir. Her abonelik 1000 en fazla eş zamanlı bağlantıyı destekler. Alternatif olarak, alıcılar sıranın HTTP protokolü aracılığıyla erişebilirsiniz.

Verimliliği en üst düzeye çıkarmak için aşağıdakileri yapın:

* Bölümlenmiş bir sıra, Gelişmiş performans ve kullanılabilirlik için kullanın.
* Farklı bir işlem içinde her alıcı bulunuyorsa, işlem başına yalnızca tek bir Fabrika kullanın.
* Alıcılar, zaman uyumlu veya zaman uyumsuz işlemleri kullanabilirsiniz. Tek bir alıcı Orta alma oranını göz önüne alındığında, istemci-tarafı tam bir istek toplu işleme alıcı verimlilik etkilemez.
* Toplu depolama erişim etkin bırakın. Bu varlık genel yükünü azaltır. Ayrıca, hangi iletileri kuyruk veya konu yazılabilir genel oranı azaltır.
* Önceden getirme sayısı küçük bir değere ayarlayın (örneğin, PrefetchCount = 10). Bu alıcılar çok sayıda önbelleğe alınmış iletiyi diğer alıcılar varken boşta engeller.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Konu Abonelikleri, küçük bir değere sahip

Hedef: abonelikler, küçük bir değere sahip bir konu verimini ekranı kaplamasını sağlayın. Bir ileti, tüm abonelikleri üzerinden birleşik alma hızı gönderme hızından daha büyük olduğu anlamına gelir pek çok abonelik tarafından alınır. Göndericiler küçük sayısıdır. Abonelik başına alıcıları küçük sayısıdır.

Verimliliği en üst düzeye çıkarmak için aşağıdakileri yapın:

* Bölümlenmiş bir konu, Gelişmiş performans ve kullanılabilirlik için kullanın.
* Konu ile genel gönderme oranını artırmak için birden fazla ileti altyapısı Gönderenler oluşturmak için kullanın. Her gönderen için zaman uyumsuz işlemleri veya birden çok iş parçacığı kullanın.
* Bir aboneliğe ilişkin genel alma hızı artırmak için birden fazla ileti altyapısı alıcıları oluşturmak için kullanın. Her alıcı için zaman uyumsuz işlemleri veya birden çok iş parçacığı kullanın.
* İstemci-tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemleri kullanın.
* Hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltmak için 20ms aralığını yığınlama varsayılan kullanın.
* Toplu depolama erişim etkin bırakın. Bu, konu ile hangi iletileri yazılabilir genel oranı artırır.
* Önceden getirme sayısı 20 kez en yüksek işleme oranları bir Factory tüm alıcılar için ayarlayın. Bu hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltır.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Çok sayıda abonelikleri konuyla

Hedef: çok sayıda abonelikleri konuyla verimini ekranı kaplamasını sağlayın. Bir ileti, tüm abonelikleri üzerinden birleşik alma hızı gönderme hızından daha çok daha büyük olduğu anlamına gelir pek çok abonelik tarafından alınır. Göndericiler küçük sayısıdır. Abonelik başına alıcıları küçük sayısıdır.

Tüm iletiler için tüm abonelikleri yönlendirilir, çok sayıda abonelikleri konularda genellikle düşük genel üretilen işi kullanıma sunar. Bu, her ileti birçok kez alınan ve konu başlığında yer alan tüm iletileri ve tüm abonelikleri aynı deposunda saklanır olgu kaynaklanır. Gönderenlerin sayısını ve abonelik başına alıcıları küçük olduğu varsayılır. Service Bus konu başına en fazla 2000 abonelik destekler.

Verimliliği en üst düzeye çıkarmak için aşağıdakileri yapın:

* Bölümlenmiş bir konu, Gelişmiş performans ve kullanılabilirlik için kullanın.
* İstemci-tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemleri kullanın.
* Hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltmak için 20ms aralığını yığınlama varsayılan kullanın.
* Toplu depolama erişim etkin bırakın. Bu, konu ile hangi iletileri yazılabilir genel oranı artırır.
* Önceden getirme sayısı 20 kez beklenen alma hızı saniye cinsinden ayarlayın. Bu hizmet veri yolu istemci iletişim kuralı iletimlerini sayısını azaltır.

## <a name="next-steps"></a>Sonraki adımlar

Hizmet veri yolu performansı en iyi duruma getirme hakkında daha fazla bilgi için bkz: [bölümlenmiş Mesajlaşma varlıkları][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.azure.servicebus.queueclient
[MessageSender]: /dotnet/api/microsoft.azure.servicebus.core.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.azure.servicebus.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.azure.servicebus.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.messagesender.batchflushinterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing
