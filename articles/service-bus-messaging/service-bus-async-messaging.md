---
title: Service Bus zaman uyumsuz Mesajlaşma | Microsoft Docs
description: Azure Service Bus Mesajlaşma hizmetinin zaman uyumsuz açıklaması.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 0ff2fbf8ddfdd191c72cfdb36a9462076f8dec5b
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657306"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Zaman uyumsuz mesajlaşma modelleri ve yüksek kullanılabilirlik

Zaman uyumsuz Mesajlaşma, çeşitli yöntemlerle uygulanabilir. Azure Service Bus, kuyruklar, konular ve abonelikler ile asynchronism depola ve İlet mekanizması aracılığıyla destekler. Normal (zaman uyumlu) işlemi, kuyruklar ve konular için iletiler gönderin ve ileti kuyrukları ve aboneliklerinden iletiler alabilir. Yazdığınız uygulamalar her zaman kullanılabilir olmasını bu varlıklara bağımlı. Varlık durumu değiştiğinde koşullar, çeşitli nedeniyle çoğu gereksinimlerini karşılayan bir azaltılmış özelliği varlık sağlamak için bir yönteme ihtiyacınız vardır.

Uygulamalar, zaman uyumsuz Mesajlaşma desenleri genellikle birkaç iletişimi senaryoları etkinleştirmek için kullanın. Bile hizmeti çalışmadığı zaman, istemciler iletileri hizmetine gönderebilir uygulamalar oluşturabilirsiniz. Uygulamalar için iletişim, bir sıra ani artışlara yardımcı olabilir, bu deneyimi düzeyi yük arabellek iletişimler için bir yer sağlayarak. Son olarak, iletileri birden çok makine arasında dağıtmak için basit ancak etkili yük dengeleyici alabilirsiniz.

Bu varlıkların herhangi birinin kullanılabilirliğini sürdürmek için birkaç farklı yol bu varlıklar için kalıcı bir Mesajlaşma sistemi kullanılamaz görünebilir göz önünde bulundurun. Genel olarak bakıldığında, biz aşağıdaki farklı yollarla yazma uygulamaları için kullanılamaz hale varlık bakın:

* İleti gönderilemiyor.
* İleti almak yüklenemiyor.
* Varlıkları Yönetme kurulamıyor (oluşturma, alma, güncelleştirme veya varlıklarını silme).
* Hizmet ile iletişim kurulamıyor.

Bu başarısızlıkların her biri için farklı hata modları bir uygulamanın iş düzeyde azaltılmış yeteneğinin gerçekleştirmeye devam etmek sağlayan mevcut. Örneğin, ileti göndermek ancak bunları alma sistemi siparişler müşterilerden hala alabilir ancak bu siparişleri işleme alınamıyor. Bu konuda ortaya çıkabilecek olası sorunları ele alır ve bu sorunların nasıl azaltıldığından. Service Bus katılmayı gerekir risk azaltmalarını bir dizi kullanıma sundu ve bu konuda, bu katılımı azaltmaları kullanımını yöneten kurallar da ele alınmaktadır.

## <a name="reliability-in-service-bus"></a>Hizmet veri yolu güvenilirlik
İletisi ve varlık sorunlarını gidermek için birkaç yolu vardır ve bu risk azaltma işlemleri uygun kullanımını yöneten kuralları vardır. Kuralları anlamak için önce hizmet veri yolundaki başarısız olabilir anlamanız gerekir. Azure sistemleri tasarımı nedeniyle, bu sorunların tümü, kısa süreli olma eğilimindedir. Yüksek bir düzeyde kullanılamazlık farklı nedenleri şu şekilde görünür:

* Service Bus bağımlı olduğu bir dış sistemden azaltma. Azaltma, depolama ve işlem kaynakları ile etkileşim gelen gerçekleşir.
* Service Bus bağımlı olduğu bir sistem sorunu. Örneğin, belirli bir depolama parçası sorunlarla karşılaşabilir.
* Tek alt sisteminde hata hizmet veri yolu. Bu durumda, bir işlem düğümünde tutarsız bir duruma koyar alabilir ve kendisini Yük Dengelemesi diğer düğümlere sunduğu tüm varlıkları neden yeniden başlatmalısınız. Bu sırayla kısa bir süre yavaş ileti işleme neden olabilir.
* Azure veri merkezindeki hatası hizmet veri yolu. Bu "sırasında sistem birkaç dakika veya birkaç saat ulaşılamaz olduğu bir arıza" dir.

> [!NOTE]
> Terim **depolama** hem Azure depolama ve SQL Azure anlamına gelebilir.
> 
> 

Service Bus bu sorunlara yönelik risk azaltma işlemleri içerir. Aşağıdaki bölümlerde, her sorun ve bunların ilgili risk azaltma işlemleri açıklanmaktadır.

### <a name="throttling"></a>Azaltma
Service Bus ile azaltma, ortak ileti oranı yönetimi sağlar. Her bir Service Bus düğümün birçok varlıklar barındırır. Bu varlıkların her biri sistemdeki CPU, bellek, depolama ve diğer özellikleri açısından taleplerini yapar. Bu modellerin hiçbirini algıladığında kullanım, tanımlanmış eşikleri aştığında, Service Bus, belirtilen bir isteğin reddedebilirsiniz. Çağıranın alır bir [ServerBusyException] [ ServerBusyException] ve 10 saniye sonra yeniden deneme sayısı.

Bir risk azaltma, kod okuma hatası ve en az 10 saniye boyunca iletinin herhangi bir yeniden deneme durdurmak gerekir. Müşteri uygulamasının parçaları arasında hata oluşabilir olduğundan, her parça bağımsız olarak yeniden deneme mantığı yürütür bekleniyor. Kod, bir kuyruk veya konuda bölümleme etkinleştirerek aşarak olasılığını azaltabilirsiniz.

### <a name="issue-for-an-azure-dependency"></a>Sorun için bir Azure bağımlılığı
Azure'daki diğer bileşenleri, zaman zaman hizmet sorun yaşayabilir. Örneğin, Service Bus kullanan bir sistemi yükseltilirken, sistemin geçici olarak azaltılmış özellikleri oluşabilir. Bu tür sorunları geçici olarak çözmek için Service Bus düzenli olarak araştırdıktan ve risk azaltma işlemleri uygular. Bu risk azaltma işlemleri yan etkilerini görünür. Örneğin, geçici bir sorun depolama ile işlemek için Service Bus ileti gönderme işlemleri tutarlı bir şekilde çalışmaya olanak sağlayan bir sistemi uygular. Risk azaltma yapısı nedeniyle, gönderilen bir iletinin etkilenen kuyruk veya abonelik içinde görünür ve bir alma işlemi için hazır olması 15 dakika sürebilir. Genel olarak bakıldığında, çoğu varlık bu sorunla karşılaşmazsınız. Ancak, varlıkların sayısı, azure'da hizmet veri yolu verildiğinde, bu risk azaltma bazen Service Bus müşterilerinin küçük bir kısmı için gereklidir.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus hata tek bir alt sistem durumunda
Herhangi bir uygulama ile koşullarda bir iç bileşen Service Bus'ın tutarsız hale gelmesine neden olabilir. Service Bus bu algıladığında, uygulamadaki ne tanılamaya yardımcı olmak için verileri toplar. Veriler toplandıktan sonra uygulama girişimi, tutarlı bir duruma döndürmek için yeniden başlatılır. Bu işlem oldukça hızlı bir şekilde gerçekleşir ve kapalı kaldıkları süreleri tipik ancak birkaç dakika kadar için kullanılamaz olarak görünen bir varlıkta sonuçları çok daha kısa.

Bu durumda, istemci uygulamanın oluşturduğu bir [System.TimeoutException] [ System.TimeoutException] veya [Istransient] [ MessagingException] özel durum. Service Bus, bu sorunun otomatik istemci yeniden deneme mantığı biçiminde bir risk azaltma içerir. Yeniden deneme süresi dolana ve iletiyi teslim sonra gibi diğer özellikleri kullanarak keşfedebilirsiniz [eşleştirilmiş ad alanları][paired namespaces]. Eşleştirilen ad alanları, bu makalede ele alınan diğer uyarılar var.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Azure veri merkezindeki Service Bus hatası
Azure veri merkezindeki bir hatanın en olası nedeni bir Service Bus veya bağımlı bir sistem, yükseltme başarısız dağıtımıdır. Platform olgunlaştığından gibi bu tür hataları olasılığını yayınladıklarını. Bir veri merkezi hatalarına de içeren aşağıdaki nedenlerle oluşabilir:

* Elektrik kesintisi (güç kaynağı ve oluşturma power kaybolur).
* Bağlantı (istemcilerinizi ve Azure arasında internet kesme).

Her iki durumda da, doğal ya da modelimiz olağanüstü bir soruna neden. Bu sorunu çözmek ve iletileri göndermeye devam edebilir, kullanabileceğiniz emin olmak için [eşleştirilmiş ad alanları] [ paired namespaces] birincil konum tekrar sağlıklı yaptığınız sırada ikinci bir konuma gönderilecek iletilerin etkinleştirmek için. Daha fazla bilgi için [insulating hizmet veri yolu kesintilerini ve olağanüstü durumları yönetme kullanan uygulamalar için en iyi yöntemler][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Eşleştirilen ad alanları
[Eşleştirilmiş ad alanları] [ paired namespaces] özelliğini destekleyen senaryoları, Service Bus varlık veya bir veri merkezi dağıtımında kullanılamaz. Bu olay sık gerçekleşirken, en kötü durum senaryoları işlemek için Dağıtılmış sistemler yine de hazırlanması gerekir. Genellikle, Service Bus bağlı olduğu bazı öğesi kısa süreli bir sorunla karşılaştığı için bu olay olur. Kesinti sırasında uygulama kullanılabilirliği sürdürmek için Service Bus kullanıcıların iki ayrı ad alanları, tercihen ayrı veri merkezlerinde, Mesajlaşma varlıkları barındırmak için kullanabilirsiniz. Bu bölümün geri kalanında aşağıdaki terimler kullanılmaktadır:

* Birincil ad alanı: Hangi uygulamanız, etkileşim için gönderme ve alma işlemleri ad alanı.
* İkincil ad alanı: Birincil ad alanı için bir yedekleme görevi gören ad alanı. Uygulama mantığının bu ad alanı ile etkileşime girmez.
* Yük devretme aralığı: Uygulama birincil ad alanından ikincil ad alanına geçiş yapmadan önce normal hataları kabul etmek için süre miktarı.

Eşleştirilmiş ad alanları Destek *kullanılabilirlik Gönder*. Kullanılabilirlik iletileri gönderme olanağı korur gönderin. Gönderme kullanılabilirlik kullanmak için uygulamanızı aşağıdaki gereksinimleri karşılaması gerekir:

1. İletiler, yalnızca birincil ad alanından alınır.
2. Belirli bir kuyruğa gönderilen iletileri veya konu sıralamaya gelebilir.
3. Bir oturum iletilerinde sıralamaya gelebilir. Oturumlarının normal işlevselliğe aradan budur. Bu, uygulamanızın mantıksal Grup iletilerini oturumları kullandığı anlamına gelir.
4. Oturum durumu, yalnızca birincil ad alanı üzerinde korunur.
5. Birincil kuyruk, çevrimiçi ve ikincil kuyruktan tüm iletileri birincil kuyruğa gönderir. önce iletileri kabul etmeye başlayın.

Aşağıdaki bölümlerde API'leri, nasıl API'leri uygulanır ve örnek kodunu gösterir özelliğini kullanır. Bu özellik ile ilgili faturalandırma etkileri olduğunu unutmayın.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>' % S ' MessagingFactory.PairNamespaceAsync API
Eşleştirilen ad alanları özelliğini içeren [PairNamespaceAsync] [ PairNamespaceAsync] metodunda [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] sınıfı:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Görev tamamlandığında, ad alanı ayrıca eksiksiz ve bağlı tüm için hareket hazır eşleşmesidir [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient] , veya [TopicClient] [ TopicClient] ile oluşturulan [MessagingFactory] [ MessagingFactory] örneği. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] is the base class for the different types of pairing that are available with a [MessagingFactory][MessagingFactory] object. Şu anda yalnızca türetilmiş sınıf adlı biridir [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], gönderme kullanılabilirlik gereksinimlerini uygular. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] birbirleri üzerinde derlenir oluşturucular kümesi vardır. Oluşturucu çoğu parametrelerle bakarak, diğer oluşturucular davranışını anlayabilirsiniz.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Bu parametreler, aşağıdaki anlamlara sahiptir:

* *secondaryNamespaceManager*: Başlatılan bir [NamespaceManager] [ NamespaceManager] örneği ikincil ad alanı, [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi ayarlamak için kullanabilirsiniz İkincil ad alanı. Ad alanı yöneticisi, gerekli biriktirme listesi kuyrukları bulunduğundan emin olun ve ad alanındaki kuyrukları listesini almak için kullanılır. Sıraların mevcut değilse oluşturulur. [NamespaceManager] [ NamespaceManager] sahip bir belirteç oluşturma olanağı gerektirir **Yönet** talep.
* *messagingFactory*: [MessagingFactory] [ MessagingFactory] ikincil ad alanı için örneği. [MessagingFactory] [ MessagingFactory] nesnesi göndermek için kullanılır ve [EnableSyphon] [ EnableSyphon] özelliği **true**, biriktirme listesi kuyruklardan iletileri alacak.
* *backlogQueueCount*: Oluşturmak için biriktirme listesi sıraların sayısı. Bu değer, en az 1 olmalıdır. Biriktirme listesine iletileri gönderirken bu kuyruklar birini rastgele seçilir. Değer 1 olarak ayarlarsanız, yalnızca bir sıra hiç olmadığı kadar kullanılabilir. Böyle bir biriktirme listesi sırası hatası veriyorsa, istemci farklı biriktirme listesi sırası denemek mümkün değildir ve ileti göndermek başarısız olabilir. Bu değer bazı büyük değer ve varsayılan değer 10 ayarı öneririz. Gün başına uygulamanızın gönderdiği veri miktarını bağlı olarak daha yüksek veya düşük bir değere değiştirebilirsiniz. Her biriktirme listesi kuyruk iletileri için en fazla 5 GB barındırabilir.
* *failoverInterval*: Bu sırada, birincil ad alanı hatalarında tek bir varlık ikincil ad alanına geçmeden önce kabul süre miktarı. Yük devretme, bir varlık tarafından varlık temelinde oluşur. Service Bus içinde farklı düğümleri sık Canlı tek bir ad alanındaki varlıklar. Bir varlık içinde bir hata, başka bir hata göstermez. Bu değeri ayarlamak [System.TimeSpan.Zero] [ System.TimeSpan.Zero] yük devretme ikincil hemen sonra ilk, geçici olmayan hata. Yük devretme Zamanlayıcı tetikleyicisi gösterilenlere herhangi [Istransient] [ MessagingException] hangi [IsTransient] [ IsTransient] özelliktir bir veyayanlış[ System.TimeoutException][System.TimeoutException]. Diğer özel durumlar gibi [UnauthorizedAccessException] [ UnauthorizedAccessException] istemci yanlış yapılandırıldığını belirttiğinden yük devretme, neden olmaz. A [ServerBusyException] [ ServerBusyException] doğru düzeni 10 saniye bekleyin olduğundan değil neden yük devretme sonra iletinin yeniden gönderir.
* *enableSyphon*: Bu belirli eşleştirme iletileri ikincil ad alanından geri birincil ad alanı da syphon olduğunu gösterir. İleti gönderme uygulamalar genel olarak, bu değeri ayarlamak **false**; iletiler alan uygulamalar bu değer ayarlanmalıdır **true**. Bunun nedeni genelde ileti gönderenler daha az sayıda ileti alıcılar yoktur. Alıcılar sayısına bağlı olarak, bir tek bir uygulama örneği Sifon görevlerini işlemek seçebilirsiniz. Birçok alıcılar kullanarak her biriktirme listesi sırası için fatura etkilere sahiptir.

Kodu kullanmak için birincil oluşturma [MessagingFactory] [ MessagingFactory] örnek, bir ikincil [MessagingFactory] [ MessagingFactory] örnek, bir ikincil [ NamespaceManager] [ NamespaceManager] örneği ve bir [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] örneği. Çağrı aşağıdaki gibi basit olabilir:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Ne zaman tarafından döndürülen görev [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi tamamlandığında, her şeyi ayarlamak ve kullanılmaya hazır ayarlanır. Görev döndürülmeden önce tüm doğru çalışması için gerektiği bir eşleştirme için arka plan işleri tamamlanmamış olabilir. Sonuç olarak, görev dönene kadar ileti gönderme başlamamalıdır. Hatalı kimlik bilgileri veya biriktirme listesi sıraları oluşturma hatası gibi hatalar oluştuğunda görev tamamlandıktan sonra bu özel durum oluşturulur. Görev döndüğünde, kuyrukları bulundu inceleyerek oluşturulmuş olduğunu doğrulayın veya [BacklogQueueCount] [ BacklogQueueCount] özelliği, [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] örneği. Önceki kod için bu işlem aşağıdaki gibidir:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Sonraki adımlar
Service Bus içinde zaman uyumsuz Mesajlaşma temel bilgileri öğrendiniz, daha fazla bilgi okuyun [eşleştirilmiş ad alanları][paired namespaces].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN
[paired namespaces]: service-bus-paired-namespaces.md
