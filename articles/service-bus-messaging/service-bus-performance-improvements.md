---
title: Azure Service Bus'ı kullanarak performans geliştirme için en iyi yöntemler | Microsoft Docs
description: Service Bus aracılı mesaj alışverişleri sırasında performansı iyileştirmek için nasıl kullanılacağını açıklar.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: aschhab
ms.openlocfilehash: f5ce8a237bc2ba7fe15acfcd6afa0edcda7ef713
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996039"
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Service Bus Mesajlaşma kullanarak performans geliştirme en iyi uygulamalar

Bu makalede, Azure Service Bus aracılı mesaj alışverişleri sırasında performansı iyileştirmek için nasıl kullanılacağını açıklar. Bu makalenin ilk bölümünü performansı artırmaya yardımcı olmak için sunulan farklı mekanizmalar anlatılır. İkinci bölümü, belli bir senaryoda en iyi performans sunabilir şekilde Service Bus'ı kullanma hakkında yönergeler sağlanır.

Bu makale boyunca "istemci" terimi, Service Bus erişen herhangi bir varlık için ifade eder. Bir istemci, bir gönderici veya alıcı rolünün alabilir. "Sender" terimi, bir Service Bus kuyruğuna veya konusuna abonelik iletiler gönderen bir Service Bus kuyruğuna veya konusuna istemci için kullanılır. "Alıcı" terimi, bir Service Bus kuyruk veya abonelikten ileti aldığında bir Service Bus kuyruğu veya abonelik istemciye ifade eder.

Bu bölümlerde, performansı artırmaya yardımcı olmak için Service Bus kullanan çeşitli kavramları tanıtan.

## <a name="protocols"></a>Protokoller

Service Bus, istemcilerin gönderip üç protokolden birine aracılığıyla iletileri almasına olanak sağlar:

1. Gelişmiş ileti sıraya alma Protokolü (AMQP)
2. Service Bus Mesajlaşma Protokolü (SBMP)
3. HTTP

Bağlantı Service Bus Mesajlaşma altyapısını mevcut olduğu sürece kullandıkları için AMQP ve SBMP daha verimlidir. Ayrıca, toplu işleme ve önceden getiriliyor uygular. Açıkça belirtilmediği sürece, bu makaledeki tüm içeriği AMQP veya SBMP kullanımını varsayar.

## <a name="reusing-factories-and-clients"></a>Fabrikalar ve istemcilerin yeniden kullanma

Service Bus istemci nesneler, gibi [QueueClient] [ QueueClient] veya [MessageSender][MessageSender], aracılığıyla oluşturulan bir [ MessagingFactory] [ MessagingFactory] bağlantılarının iç yönetimi de sağlayan bir nesne. Bir ileti gönderin ve sonraki iletiyi gönderdiğinizde, ardından yeniden oluşturduktan sonra Mesajlaşma fabrikaları veya kuyruk, konu ve abonelik istemcileri kapatmayın, önerilir. Service Bus hizmetinin bağlantısı bir Mesajlaşma fabrikası kapatma siler ve yeni bir bağlantı üreteci tekrar oluşturulurken kurulur. Bağlantı kurma aynı Fabrika ve istemci nesneleri birden çok işlem için yeniden kullanarak kaçınabilirsiniz pahalı bir işlemdir. Bu istemci nesneler, eş zamanlı zaman uyumsuz işlemler ve birden çok iş parçacığından güvenli bir şekilde kullanabilirsiniz. 

## <a name="concurrent-operations"></a>Eşzamanlı işlem

Bir işlem gerçekleştirme (gönderme, alma, silme, vb.) biraz zaman alabilir. Bu süre, istek ve yanıt gecikmeye ek olarak Service Bus hizmeti tarafından işlemi işlenmesini içerir. İşlem Saati başına sayıyı artırmak için işlemler aynı anda yürütmeniz gerekir. 

İstemcisi eşzamanlı işlemlerin zaman uyumsuz işlemleri gerçekleştirerek zamanlar. Bir sonraki istekte, önceki isteği tamamlanmadan önce başlatılır. Aşağıdaki kod parçacığı, bir zaman uyumsuz gönderme işlemi örneğidir:
  
 ```csharp
  Message m1 = new BrokeredMessage(body);
  Message m2 = new BrokeredMessage(body);
  
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
  
  Aşağıdaki kod örneği bir zaman uyumsuz bir alma işlemi ' dir. Tam program bkz [burada](https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.Azure.ServiceBus/SendersReceiversWithQueues):
  
  ```csharp
  var receiver = new MessageReceiver(connectionString, queueName, ReceiveMode.PeekLock);
  var doneReceiving = new TaskCompletionSource<bool>();

  receiver.RegisterMessageHandler(...);
  ```

## <a name="receive-mode"></a>Modu alır

Bir kuyruk veya abonelik istemci oluştururken alma modu belirtebilirsiniz: *Gözlem kilidi* veya *alma ve silme*. Varsayılan alma modu olan [PeekLock][PeekLock]. Bu modda çalışırken, istemci Service Bus'tan ileti almak için bir istek gönderir. İstemci iletiyi aldıktan sonra iletiyi tamamlamak için bir istek gönderir.

Alma modu ayarını olduğunda [ReceiveAndDelete][ReceiveAndDelete], her iki adım tek bir istekte birleştirilir. Bu adımlar, genel işlem sayısını azaltın ve genel ileti işleme hızı artırabilir. Bu performans artışı iletileri kaybetme at the risk of gelir.

Service Bus, alma ve silme işlemleri için işlemleri desteklemiyor. Ayrıca, gözlem kilidi semantiği, istemcinin istediği erteleneceği tüm senaryolarda gerekli veya [edilemeyen](service-bus-dead-letter-queues.md) bir ileti.

## <a name="client-side-batching"></a>İstemci tarafı işlem grubu oluşturma

İstemci tarafı toplu işlem, belirli bir süreliğine bir ileti gönderme geciktirmek bir kuyruk veya konu istemci sağlar. İstemci bu süre içinde başka iletiler gönderirse, iletileri tek bir toplu iş olarak gönderir. İstemci tarafı toplu işleme da neden birden çok toplu iş bir kuyruk veya abonelik istemci **tam** istekleri tek bir istek. Toplu işleme yüklenebilir yalnızca zaman uyumsuz **Gönder** ve **tam** operations. Zaman uyumlu işlemler hemen Service Bus hizmetine gönderilir. Toplu işleme için Özet gerçekleşmezse veya alma işlemleri ya da toplu işlem istemci genelinde oluşuyor.

Varsayılan olarak, bir istemci bir toplu iş aralığı 20 MS kullanır. Toplu iş aralığı ayarlayarak değiştirebilirsiniz [BatchFlushInterval] [ BatchFlushInterval] Mesajlaşma fabrikası oluşturmadan önce özelliği. Bu ayar, bu factory tarafından oluşturulan tüm istemcilerin etkiler.

Toplu işleme devre dışı bırakmak için ayarlanmış [BatchFlushInterval] [ BatchFlushInterval] özelliğini **değeri, TimeSpan.Zero**. Örneğin:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Toplu işleme ileti Faturalanabilir işlemlerin sayısı etkilemez ve yalnızca hizmet veri yolu istemci protokolünü kullanarak için kullanılabilir [Microsoft.ServiceBus.Messaging](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kitaplığı. Toplu işleme HTTP protokolünü desteklemiyor.

> [!NOTE]
> BatchFlushInterval ayarı toplu işleme uygulama açısından bakıldığında örtük olmasını sağlar. yani uygulama SendAsync() yapar ve CompleteAsync() çağırır ve belirli toplu işlem çağrıları yapmaz.
>
> Açık istemci tarafı toplu işleme uygulanabilir yararlanarak yöntem çağrısı - aşağıdaki 
> ```csharp
> Task SendBatchAsync (IEnumerable<BrokeredMessage> messages);
> ```
> Burada iletilerin toplam boyutu fiyatlandırma katmanı tarafından desteklenen Maksimum boyuttan daha az olmalıdır.

## <a name="batching-store-access"></a>Depolama erişim toplu işleme

Kendi iç deposuna yazarken kuyruk, konu veya abonelik verimini artırmak için Service Bus birden çok ileti toplu olarak işler. Bir kuyruk veya konuda etkinleştirilirse, ileti deposuna yazma toplu. Bir kuyrukta veya abonelikte etkin ise, mağaza'dan iletilerini silme toplu. Toplu depolama erişimi için bir varlık etkinleştirilirse, Service Bus tarafından en fazla 20 ms mağazası yazma işlemi bu varlığa ilişkin geciktirir. 

> [!NOTE]
> Bir 20ms toplu aralığın sonunda bir Service Bus hata olsa toplu işlem, sahip iletileri kaybetme riski yoktur. 

Bu zaman aralığı boyunca gerçekleşen ek depolama işlemleri toplu olarak eklenir. Toplu depolama erişim yalnızca etkiler **Gönder** ve **tam** işlemleri; alma işlemlerini etkilenmez. Toplu depolama erişimi bir varlık üzerindeki bir özelliktir. Toplu işlem, toplu depolama erişiminizi etkinleştirecek olan tüm varlıklar arasında gerçekleşir.

Yeni Kuyruk, konu veya abonelik oluştururken, toplu depolama erişim varsayılan olarak etkindir. Toplu depolama erişimi devre dışı bırakmak için ayarlanmış [EnableBatchedOperations] [ EnableBatchedOperations] özelliğini **false** varlık oluşturmadan önce. Örneğin:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Toplu depolama erişim Mesajlaşma Faturalanabilir işlemlerin sayısı etkilemez ve bir kuyruk, konu veya abonelik bir özelliğidir. Alma modu ve bir istemci ve hizmet veri yolu hizmeti arasında kullanılan protokol bağımsızdır.

## <a name="prefetching"></a>Önceden getiriliyor

[Önceden getiriliyor](service-bus-prefetch.md) alma işlemi gerçekleştirdiğinde hizmetinden ek iletiler yüklemek kuyruk veya abonelik istemcinin sağlar. İstemci bu iletiler, yerel bir önbellekte depolar. Önbellek boyutu tarafından belirlenir [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] veya [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] özellikleri. Önceden getiriliyor sağlayan her bir istemci kendi önbellek tutar. Bir önbellek istemci genelinde paylaşılmaz. İstemci bir Al işlemi başlatır ve önbelleğinde boş ise, toplu olarak mesaj hizmeti iletir. Toplu iş boyutu 256 KB ve önbellek boyutuna eşittir, hangisi daha küçükse. İstemci bir Al işlemi başlatır ve önbelleği içeren bir ileti, ileti önbellekten alınır.

Bir ileti önceden getirilmiş, hizmeti önceden getirilmiş ileti kilitler. Kilit ile farklı bir alıcı tarafından önceden getirilmiş ileti alınamıyor. Kilit süresi dolmadan önce iletiyi alıcı tamamlanamıyorsa, iletinin diğer alıcılar için kullanılabilir hale gelir. İletinin önceden getirilmiş kopyasının önbellekte kalır. Süresi dolmuş önbelleğe alınmış kopyayı tüketen alıcı bu iletiyi tamamlamak çalıştığında bir özel durum alır. Varsayılan olarak, ileti kilidi 60 saniye sonra süresi dolar. Bu değer 5 dakika olarak genişletilebilir. Süresi dolan iletileri kullanımını önlemek için önbellek boyutu her zaman kilit zaman aşımı aralığı içinde bir istemci tarafından tüketilebilecek iletileri sayısından küçük olmalıdır.

60 saniye varsayılan kilidi sona erme kullanırken iyi bir değer için [PrefetchCount] [ SubscriptionClient.PrefetchCount] olan 20 kez en fazla işleme Factory tüm alıcılar hızları. Örneğin, üç alıcılar bir Üreteç oluşturur ve her alıcı saniye başına en fazla 10 iletileri işleyebilir. Önceden getirme sayısı, 20 X 3 X 10 = 600 aşmamalıdır. Varsayılan olarak, [PrefetchCount] [ QueueClient.PrefetchCount] başka ileti hizmetten getirilen anlamına gelen 0 olarak ayarlanır.

İletileri önceden getiriliyor, genel bir ileti işlemleri veya gidiş dönüş sayısını azaltır çünkü bir kuyrukta veya abonelikte için hizmetin genel performansını artırır. İlk iletinin getiriliyor, ancak bu kadar (daha yüksek ileti boyutu nedeniyle) daha uzun sürer. Bu iletiler, istemci tarafından önceden yüklenmiş olduğundan önceden getirilmiş iletilerini alma daha hızlı olacaktır.

Bir iletinin yaşam süresi (TTL) özelliği, sunucu istemciye iletiyi gönderir zaman sunucu tarafından denetlenir. İleti alındığında istemci, iletinin TTL özelliğine denetlemez. Bunun yerine, ileti iletinin TTL ileti istemci tarafından önbelleğe alınmış olsa bile geçtiyse alınabilir.

Önceden getiriliyor Mesajlaşma Faturalanabilir işlemlerin sayısı etkilemez ve yalnızca hizmet veri yolu istemci protokolü için kullanılabilir. Önceden getiriliyor HTTP protokolünü desteklemiyor. Önceden getiriliyor, zaman uyumlu ve zaman uyumsuz alma işlemleri için kullanılabilir.

## <a name="prefetching-and-receivebatch"></a>Önceden getiriliyor ve ReceiveBatch

Birden çok ileti birlikte önceden getiriliyor kavramları (ReceiveBatch) toplu ileti işleme için benzer semantiğe sahip olsa da, bunların bir araya ne akılda tutulması gereken bazı küçük farklar vardır.

Önceden getirme bir yapılandırma (veya modu) (QueueClient ve SubscriptionClient) istemcide ve ReceiveBatch (istek-yanıt semantiği olan) bir işlemdir.

Bu arada kullanırken, aşağıdaki durumlarda düşünün:

* Önceden getirme büyüktür veya eşittir ReceiveBatch almaya beklediğiniz ileti sayısı olmalıdır.
* Önceden getirme n/3 n varsayılan kilit süresi, saniye başına işlenen iletilerin sayısını süreleri kadar olabilir.

Doyumsuz bir sahip olan bazı zorluklar vardır yaklaşımını (yani önceden getirme sayısı çok yüksek tutma), belirli bir alıcı iletiyi kilitli olduğunu gösterdiğinden. Çıkış değerleri yukarıda belirtilen eşikler arasında önceden getirme ve hangi uygun türü tanımlamak denemek için önerilir.

## <a name="multiple-queues"></a>Birden fazla kuyruk

Beklenen yükü tek bölümlenmiş bir kuyruk veya konuda tarafından işlenemez birden çok Mesajlaşma varlıkları kullanmanız gerekir. Birden çok varlık kullanırken, aynı istemciden tüm varlıklar için kullanmak yerine, her varlık için adanmış bir istemci oluşturun.

## <a name="development-and-testing-features"></a>Geliştirme ve test özellikleri

Service Bus özel geliştirme için kullanılan bir özellik olan, **üretim yapılandırmalarında hiçbir zaman kullanılmamalıdır**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Yeni kurallar veya filtreleri konuya eklendiğinde kullanabileceğiniz [TopicDescription.EnableFilteringMessagesBeforePublishing][] yeni filtre ifadesi beklendiği gibi çalıştığını doğrulayın.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki bölümlerde, tipik bir Mesajlaşma senaryoları açıklar ve tercih edilen hizmet veri yolu ayarları özetler. Hızlarındaki sınıflandırılan küçük (daha az 1 saniye başına ileti çok), Orta (1 ileti/saniye veya daha fazla 100 ancak iletiler/saniye) ve yüksek (100 iletileri/ikinci veya büyük). İstemci sayısı küçük sınıflandırılan (5 veya daha az), Orta (5'ten fazla ancak 20 küçüktür veya eşittir) ve büyük (birden çok 20).

### <a name="high-throughput-queue"></a>Yüksek performanslı sırası

Hedef: Tek bir kuyruk verimini en üst düzeye çıkarın. Göndericiler ve alıcılar küçük sayısıdır.

* Kuyruğa genel gönderme oranını artırmak için birden fazla ileti altyapısı Gönderenler oluşturmak için kullanın. Her bir gönderen için zaman uyumsuz işlemler ya da birden çok iş parçacığı kullanın.
* Kuyruktan genel alma oranını artırmak için birden fazla ileti altyapısı alıcılar oluşturmak için kullanın.
* İstemci tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemler kullanın.
* Service Bus istemci protokolü iletimleri sayısını azaltmak için 50 ms toplu aralığı ayarlayın. Birden çok Gönderenler kullandıysanız, 100 ms toplu aralığını artırın.
* Toplu depolama erişim etkin bırakın. Bu erişim, iletileri kuyruğa yazılabilir genel hızı artar.
* Önceden getirme sayısı 20 kez en yüksek işleme oranlarda fabrikanın tüm alıcılar için ayarlayın. Bu sayı, Service Bus istemci protokolü iletimleri sayısını azaltır.
* Bölümlenmiş bir kuyruk, Gelişmiş performans ve kullanılabilirlik için kullanın.

### <a name="multiple-high-throughput-queues"></a>Birden çok yüksek işleme sırası

Hedef: Birden fazla kuyruk genel verimini en üst düzeye çıkarın. Aktarım hızını ayrı bir kuyruk, Orta veya yüksek.

Birden fazla kuyruk en fazla aktarım hızı elde etmek için tek bir kuyruk verimini en üst düzeye çıkarmak için ana hatlarıyla belirtilen ayarları kullanın. Ayrıca, farklı fabrikaları farklı sıralarından gönderip istemciler oluşturmak için kullanın.

### <a name="low-latency-queue"></a>Düşük gecikme süresi sırası

Hedef: Bir kuyruk veya konuda uçtan uca gecikme süresini en aza indirin. Göndericiler ve alıcılar küçük sayısıdır. Sıranın işleme küçük veya orta.

* İstemci tarafı işlem grubu oluşturma devre dışı bırakın. İstemci hemen bir ileti gönderir.
* Toplu depolama erişimi devre dışı bırakın. Hizmet, hemen ileti deposuna yazar.
* Tek bir istemci kullanıyorsanız, önceden getirme sayısı 20 kez işleme hızı alıcı için ayarlayın. Service Bus istemci protokolü tümü aynı anda birden çok ileti aynı anda kuyruğa geldiğinde, iletir. İstemci bir sonraki iletiyi aldığında, bu iletiyi yerel önbellekte zaten. Önbellek küçük olmalıdır.
* Birden çok istemci kullanıyorsanız, önceden getirme sayısı 0 olarak ayarlayın. İlk istemci ilk iletiyi işlemeye devam ederken sayısı ayarlayarak, ikinci istemci ikinci bir ileti alabilir.
* Bölümlenmiş bir kuyruk, Gelişmiş performans ve kullanılabilirlik için kullanın.

### <a name="queue-with-a-large-number-of-senders"></a>Çok sayıda Gönderenler kuyruğa al

Hedef: Bir kuyruk veya konu çok sayıda göndericiler ile aktarım hızını en üst düzeye çıkarın. Her gönderen Orta oranı olan iletiler gönderir. Alıcılar küçük sayısıdır.

Service Bus Mesajlaşma varlığı için 1000 adede kadar eş zamanlı bağlantı sağlar (veya 5000 AMQP kullanarak). Bu sınır ad alanı düzeyinde uygulanır ve kuyrukları/konular/abonelikler, ad alanı başına eşzamanlı bağlantı sınırını tarafından kapsanır. Kuyruklar için bu sayıyı göndericiler ile alıcılar arasında paylaşılır. Tüm 1000 bağlantıları Gönderenler için gerekliyse, kuyruğa bir konu ve tek bir abonelik ile değiştirin. Abonelik bir ek 1000 eşzamanlı bağlantı alıcılarından kabul ederken bir konu göndericiler, 1000 adede kadar eşzamanlı bağlantılarını kabul eder. 1000'den fazla eş zamanlı Gönderenler gerekiyorsa, Gönderenler HTTP aracılığıyla hizmet veri yolu protokolündeki iletileri göndermesi gerekir.

Aktarım hızını en üst düzeye çıkarmak için aşağıdaki adımları gerçekleştirin:

* Farklı bir işlem içinde her gönderen yer alıyorsa, işlem başına yalnızca tek bir Fabrika kullanın.
* İstemci tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemler kullanın.
* Service Bus istemci protokolü iletimleri sayısını azaltmak için 20 ms, aralık toplu işleme varsayılan kullanın.
* Toplu depolama erişim etkin bırakın. Bu erişim hangi ileti kuyruğuna veya konusuna yazılabilir genel hızı artar.
* Önceden getirme sayısı 20 kez en yüksek işleme oranlarda fabrikanın tüm alıcılar için ayarlayın. Bu sayı, Service Bus istemci protokolü iletimleri sayısını azaltır.
* Bölümlenmiş bir kuyruk, Gelişmiş performans ve kullanılabilirlik için kullanın.

### <a name="queue-with-a-large-number-of-receivers"></a>Çok sayıda alıcı ile kuyruk

Hedef: Bir kuyrukta veya abonelikte bir sayıda alıcılar alma oranını en üst düzeye çıkarın. Her alıcı Orta fiyat iletileri alır. Göndericiler küçük sayısıdır.

Bir varlık için en fazla 1000 eşzamanlı bağlantı Service Bus sağlar. Bir kuyruk 1000'den fazla alıcılar gerektiriyorsa, kuyruğa bir konu ve birden çok abonelik ile değiştirin. Her abonelik en fazla 1000 eşzamanlı bağlantı destekleyebilir. Alternatif olarak, alıcıları, kuyruk HTTP protokolü üzerinden erişebilirsiniz.

Aktarım hızını en üst düzeye çıkarmak için aşağıdakileri yapın:

* Farklı bir işlem içinde her alıcı yer alıyorsa, işlem başına yalnızca tek bir Fabrika kullanın.
* Alıcılar, zaman uyumlu veya zaman uyumsuz işlemleri kullanabilirsiniz. Tek bir alıcı Orta alma oranını göz önünde bulundurulduğunda, tam bir istek istemci tarafı toplu işleme alıcı aktarım hızı etkilemez.
* Toplu depolama erişim etkin bırakın. Bu erişim, varlığın genel yükü azaltır. Ayrıca, ileti kuyruğuna veya konusuna yazılabilir toplam hızı da azaltır.
* Önceden getirme sayısı küçük bir değere ayarlayın (örneğin, PrefetchCount = 10). Bu sayı çok sayıda önbelleğe alınan iletiyi diğer alıcılar varken boş olmaktan alıcılar engeller.
* Bölümlenmiş bir kuyruk, Gelişmiş performans ve kullanılabilirlik için kullanın.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Konu Abonelikleri, küçük bir değere sahip

Hedef: Konu abonelikleri az sayıda ile aktarım hızını en üst düzeye çıkarın. Bir ileti, tüm abonelikler üzerinden birleşik alma hızı gönderme hızından daha büyük olduğu anlamına gelir çok abonelik tarafından alınır. Göndericiler küçük sayısıdır. Abonelik başına alıcılar küçük sayısıdır.

Aktarım hızını en üst düzeye çıkarmak için aşağıdakileri yapın:

* Bu konu başlığında genel gönderme hızı artırmak için Gönderenler oluşturmak için birden fazla ileti altyapısı kullanın. Her bir gönderen için zaman uyumsuz işlemler ya da birden çok iş parçacığı kullanın.
* Bir Abonelikteki toplam alma hızı artırmak için birden fazla ileti altyapısı alıcılar oluşturmak için kullanın. Zaman uyumsuz işlemler ya da birden çok iş parçacığı her alıcı için kullanın.
* İstemci tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemler kullanın.
* Service Bus istemci protokolü iletimleri sayısını azaltmak için 20 ms, aralık toplu işleme varsayılan kullanın.
* Toplu depolama erişim etkin bırakın. Bu erişim, konu başlığında, iletileri yazılabilir genel hızı artar.
* Önceden getirme sayısı 20 kez en yüksek işleme oranlarda fabrikanın tüm alıcılar için ayarlayın. Bu sayı, Service Bus istemci protokolü iletimleri sayısını azaltır.
* Bölümlenmiş bir konu, Gelişmiş performans ve kullanılabilirlik için kullanın.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Konu Abonelikleri, çok sayıda ile

Hedef: Bir konu çok sayıda abonelikler ile aktarım hızını en üst düzeye çıkarın. Bir ileti, tüm abonelikler üzerinden birleşik alma hızı gönderme hızından çok daha büyüktür, yani çok abonelik tarafından alınır. Göndericiler küçük sayısıdır. Abonelik başına alıcılar küçük sayısıdır.

Tüm iletiler için tüm abonelikleri yönlendirilir, çok sayıda abonelikleri konularla genellikle düşük bir genel performansını kullanıma sunar. Bu düşük aktarım hızı, her ileti birden çok kez alındığında ve bir konuda yer alan tüm iletileri ve tüm abonelikler aynı depoda depolanan gerçeği kaynaklanır. Göndericiler ve alıcılar başına abonelik sayısı küçük sayısıdır varsayılır. Service Bus konu başına en fazla 2.000 abonelik destekler.

Aktarım hızını en üst düzeye çıkarmak için aşağıdaki adımları deneyin:

* İstemci tarafı toplu işleme avantajlarından yararlanmak için zaman uyumsuz işlemler kullanın.
* Service Bus istemci protokolü iletimleri sayısını azaltmak için 20 ms, aralık toplu işleme varsayılan kullanın.
* Toplu depolama erişim etkin bırakın. Bu erişim, konu başlığında, iletileri yazılabilir genel hızı artar.
* Önceden getirme sayısı 20 kez beklenen alma hızı, saniye cinsinden ayarlayın. Bu sayı, Service Bus istemci protokolü iletimleri sayısını azaltır.
* Bölümlenmiş bir konu, Gelişmiş performans ve kullanılabilirlik için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Hizmet veri yolu performansını iyileştirme hakkında daha fazla bilgi için bkz: [bölümlenmiş Mesajlaşma varlıkları][Partitioned messaging entities].

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
