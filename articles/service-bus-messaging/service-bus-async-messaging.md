---
title: Hizmet veri yolu zaman uyumsuz Mesajlaşma | Microsoft Docs
description: Azure Service Bus zaman uyumsuz Mesajlaşma açıklaması.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: sethm
ms.openlocfilehash: e48e95d99847e68bdb218b341ad2fbcd44eb31f4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28930002"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Zaman uyumsuz mesajlaşma modelleri ve yüksek kullanılabilirlik

Zaman uyumsuz Mesajlaşma çeşitli farklı şekillerde uygulanabilir. Kuyruklar, konular ve abonelikler ile Azure Service Bus deposu ve iletme mekanizması aracılığıyla asynchronism destekler. Normal (zaman uyumlu) işlemi, kuyrukları ve konularından iletiler göndermek ve kuyrukları ve aboneliklerinden iletiler alırsınız. Yazdığınız uygulamalar her zaman kullanılabilir olan varlıkları bağlıdır. Varlık durumu değiştiğinde durumlarda, çeşitli nedeniyle çoğu gereksinimlerini karşılayabilen bir azaltılmış yetenek varlık sağlamak için bir yönteme ihtiyacınız vardır.

Uygulamaları, zaman uyumsuz Mesajlaşma desenleri genellikle çeşitli iletişim senaryoları etkinleştirmek için kullanın. Bile hizmet çalışmadığı zaman, istemciler iletileri hizmetlerine gönderebilir uygulamalar oluşturabilir. Uygulamalar için iletişim, bir sıra WINS'e yardımcı olabilir, deneyimi düzey yük arabellek iletişimler için bir yer sağlayarak. Son olarak, birden fazla makine arasında iletilerini dağıtmak için basit ancak etkili yük dengeleyici elde edebilirsiniz.

Bu varlıkları kullanılabilirliğini sürdürmek için bu varlıklar için sağlam bir ileti sistemi kullanılamaz görünebilir farklı şekillerde sayısını göz önünde bulundurun. Genel olarak bakıldığında, biz aşağıdaki farklı yollarla yazma uygulamalara kullanılamaz hale varlık bakın:

* İleti gönderilemiyor.
* İletileri alınamadı.
* Varlıkları Yönet kurulamıyor (oluşturma, alma, güncelleştirme veya varlıklarını silme).
* Hizmetiyle iletişim kurulamıyor.

Bu başarısızlıkların her biri için farklı bir hata modları azaltılmış yeteneğinin düzeyinde işlerini yapmak devam etmek bir uygulama sağlayan mevcut. Örneğin, ileti gönderme ancak bunları almamayı bir sistem siparişleri müşterilerden hala alabilir ancak bu siparişleri işleyemiyor. Bu konu, oluşabilecek olası sorunları ele alır ve bu sorunların nasıl azalır. Service Bus içine opt Azaltıcı Etkenler sayısı sunulan ve bu konuda Ayrıca bu katılımı Azaltıcı Etkenler kullanımını yöneten kuralları anlatılmaktadır.

## <a name="reliability-in-service-bus"></a>Service Bus içinde güvenilirliği
İleti ve varlık sorunlarını gidermek için birkaç yolu vardır ve bu Azaltıcı Etkenler uygun kullanımını yöneten yönergeler vardır. Yönergeleri anlamak için önce hizmet veri yolundaki yük devredebilir anlamanız gerekir. Azure sistemleri tasarım nedeniyle, bu sorunların tümü, kısa süreli olma eğilimindedir. Yüksek bir düzeyde kullanılamazlık farklı nedenleri şu şekilde görünür:

* Hizmet veri yolu bağımlı olduğu bir dış sistemden azaltma. Depolama ve işlem kaynakları ile etkileşim gelen azaltma oluşur.
* Hizmet veri yolu bağımlı olduğu bir sistem sorunu. Örneğin, belirli bir depolama parçası sorunlarla.
* Tek alt sisteminde hata hizmet veri yolu. Bu durumda, bir işlem düğümünde tutarsız bir duruma alabilir ve kendisini diğer düğümlere dengelemek için sunduğu tüm varlıklar neden yeniden başlatmalısınız. Bu sırayla yavaş ileti işleme kısa bir süre neden olabilir.
* Azure veri merkezi içinde hatası hizmet veri yolu. Bu "sırasında sistem birçok dakika ya da birkaç saat için ulaşılamaz olduğu yıkıcı bir hatadan" dir.

> [!NOTE]
> Terim **depolama** Azure Storage ve SQL Azure anlamına gelebilir.
> 
> 

Service Bus bu sorunlar için Azaltıcı Etkenler sayısını içerir. Aşağıdaki bölümlerde her sorun ve bunların ilgili Azaltıcı açıklanmaktadır.

### <a name="throttling"></a>Azaltma
Service Bus ile azaltma işbirlikçi ileti oranı yönetimi sağlar. Tek tek her Service Bus düğüm birçok varlık barındırır. Bu varlıkların her CPU, bellek, depolama ve diğer yönleri açısından sistemde taleplerini yapar. Bu modellerle hiçbirini algıladığında, kullanım tanımlı eşiklerini aşan, hizmet veri yolu, belirtilen bir isteğin reddedebilirsiniz. Arayan alan bir [ServerBusyException] [ ServerBusyException] ve 10 saniye sonra yeniden deneme sayısı.

Bir azaltma kodu okuma hatası ve tüm yeniden denemeler iletisinin en az 10 saniye durdurmak gerekir. Hata müşteri uygulaması parçalarını arasında gerçekleşebilir olduğundan, her parça bağımsız olarak yeniden deneme mantığı yürütür beklenir. Kod bir kuyruk veya konu bölümleme etkinleştirerek karşılaşıldığı olasılığını azaltır.

### <a name="issue-for-an-azure-dependency"></a>Bir Azure bağımlılığı sorunu
Azure'daki diğer bileşenleri bazen hizmet sorunları olabilir. Örneğin, Service Bus kullanan bir sistemi yükseltme, sistemin geçici olarak azaltılmış yetenekleri yaşayabilirsiniz. Bu tür sorunların olarak çözmek için Service Bus düzenli olarak araştırır ve bunları azaltmanın yollarını uygular. Bu Azaltıcı Etkenler yan etkileri görünür. Örneğin, depolama ile geçici sorunlarını gidermek için tutarlı bir şekilde çalışması ileti gönderme işlemlere izin veren bir sistem hizmet veri yolu uygular. Azaltma yapısı nedeniyle, gönderilen iletiyi etkilenen sıra veya abonelik görünür ve bir alma işlemi için hazır olması için 15 dakika sürebilir. Genel olarak bakıldığında, bu sorun çoğu varlık karşılaşmazsınız. Ancak, Service Bus Azure içindeki varlıkların sayısı verilen, bu azaltma bazen Service Bus müşteriler küçük bir kısmı için gereklidir.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Tek bir alt sistemi bağlı hizmet veri yolu hatası
Herhangi bir uygulama ile durumlarda bir iç bileşeninin Service Bus'ın tutarsız hale gelmesine neden olabilir. Service Bus bu algıladığında, uygulamanın ne tanılamalarına yardımcı olmak için veri toplar. Veriler toplandıktan sonra uygulama için tutarlı bir duruma dönmek girişimi yeniden başlatılır. Bu işlem oldukça hızlı bir şekilde gerçekleşir ve normal olsa kapalı kaldıkları için birkaç dakika kadar kullanılamaz olacak şekilde görünen bir varlık sonuçlarında çok kısa.

Bu durumda, istemci uygulaması oluşturan bir [System.TimeoutException] [ System.TimeoutException] veya [MessagingException] [ MessagingException] özel durum. Hizmet veri yolu azaltma bu sorunun otomatik istemci yeniden deneme mantığı biçiminde içerir. Yeniden deneme süresi dolana ve ileti teslim edilmedi sonra gibi diğer özellikleri kullanılarak keşfedebilirsiniz [eşleştirilmiş ad alanları][paired namespaces]. Eşleştirilmiş ad alanları bu makalede ele alınan diğer uyarılar var.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Hizmet veri yolu hatası Azure veri merkezi içinde
Azure veri merkezi içinde bir hata en olası nedeni bir hizmet veri yolu veya bağımlı sistem başarısız yükseltme dağıtımıdır. Platform olgunlaştığından gibi bu tür bir hata olasılığını yayınladıklarını. Bir veri merkezi hatası Ayrıca aşağıdakileri içeren nedenlerden kaynaklanabilir:

* Elektrik kesintisi (güç kaynağı ve kayboluyor oluşturma güç).
* Bağlantı (internet kesme istemcileri ve Azure arasında).

Her iki durumda da doğal ya da man-made olağanüstü durum sorunu neden oldu. Bu sorunu çözmek ve iletileri göndermeye devam edebilir, kullanabileceğiniz emin olmak için [eşleştirilmiş ad alanları] [ paired namespaces] birincil konumda yeniden sağlıklı karşın ikinci bir konuma gönderilecek iletilerin etkinleştirmek için. Daha fazla bilgi için bkz: [en iyi uygulamalar için Service Bus kesintileri ve olağanüstü karşı uygulamalar insulating][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Eşleştirilen ad alanları
[Eşleştirilmiş ad alanları] [ paired namespaces] özelliğini destekleyen senaryoları, Service Bus varlık veya dağıtım bir veri merkezi içinde kullanılamaz. Bu olay sık gerçekleşirken hala dağıtılmış sistemlerin en kötü örneği senaryolarının işlemek için hazırlanması gerekir. Genellikle, Service Bus bağımlı olduğu bazı öğesi kısa süreli bir sorunla karşılaştığı için bu olay gerçekleşir. Kesinti sırasında uygulama kullanılabilirliği sürdürmek için Service Bus kullanıcılar iki ayrı ad alanları, tercihen ayrı veri merkezlerinde, Mesajlaşma varlıkları barındırmak için kullanabilirsiniz. Bu bölüm geri kalanı aşağıdaki terimleri kullanır:

* Birincil ad alanı: hangi uygulamanız, etkileşim için gönderme ve alma işlemlerinin, ad alanı.
* İkincil ad alanı: birincil ad alanı için bir yedekleme görevi gören ad alanı. Uygulama mantığını bu ad alanı ile etkileşime girmez.
* Yük devretme aralığı: uygulama birincil ad alanından ikincil ad alanına geçiş yapmadan önce normal hataları kabul etmek için süre miktarı.

Ad alanları destek eşleştirilmiş *kullanılabilirlik Gönder*. Kullanılabilirlik iletileri gönderme olanağı korur gönderin. Gönderme kullanılabilirlik kullanmak için uygulamanızı aşağıdaki gereksinimleri karşılaması gerekir:

1. İletileri yalnızca birincil ad alanından alınır.
2. Belirli bir sıraya gönderilen iletileri veya konu bozuk ulaşır.
3. Bir oturumu içinde iletiler bozuk geldiğinde. Oturumları normal işlevselliğini aradan budur. Bu, uygulamanızın mantıksal Grup iletileri oturumlarını kullandığı anlamına gelir.
4. Oturum durumu yalnızca birincil ad korunur.
5. Birincil kuyruk çevrimiçine ve ikincil sıra tüm iletileri birincil sıraya teslim önce iletileri kabul etmeye başlatın.

Aşağıdaki bölümlerde API'ler açıklanmaktadır, nasıl API'leri uygulanır ve örnek kodunu gösterir özelliğini kullanır. Bu özellik ile ilişkili fatura etkileri olduğunu unutmayın.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>API MessagingFactory.PairNamespaceAsync
Eşleştirilmiş ad alanları özelliği içerir [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] sınıfı:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Görev tamamlandığında ad alanı eşleştirme ayrıca tam ve için görev için hazır [MessageReceiver][MessageReceiver], [QueueClient][QueueClient], veya [TopicClient] [ TopicClient] ile oluşturulan [Eventhubclient] [ MessagingFactory] örneği. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] is the base class for the different types of pairing that are available with a [MessagingFactory][MessagingFactory] object. Şu anda yalnızca türetilmiş sınıf adlı biridir [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], gönderme kullanılabilirlik gereksinimlerini uygular. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] birbirine yapı oluşturucuları kümesi vardır. Oluşturucusu çoğu parametrelerle baktığınızda, diğer oluşturucular davranışını anlayabilirsiniz.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Bu parametreleri şu anlama gelir:

* *secondaryNamespaceManager*: başlatılan bir [NamespaceManager] [ NamespaceManager] örneği ikincil ad alanı için [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi ikincil ad alanı ayarlama için kullanabilirsiniz. Ad alanı manager ad alanında sıraları listesini elde etmek için ve gerekli biriktirme listesi sıraları var olduğundan emin olmak için kullanılır. Sıraların yoksa, oluşturulur. [NamespaceManager] [ NamespaceManager] bir belirteci oluşturma olanağı gerektirir **Yönet** talep.
* *Eventhubclient*: [Eventhubclient] [ MessagingFactory] ikincil ad alanı için örneği. [Eventhubclient] [ MessagingFactory] nesnesi göndermek için kullanılır ve [EnableSyphon] [ EnableSyphon] özelliği ayarlanmış **doğru**, biriktirme listesi sıralarından iletileri alacak.
* *backlogQueueCount*: oluşturmak için biriktirme listesi sıraların sayısı. Bu değer, en az 1 olmalıdır. İletileri biriktirme listesine gönderirken bu sıraların birini rastgele seçilir. Değer 1 olarak ayarlarsanız, yalnızca bir sıra herhangi bir zamanda kullanılabilir. Böyle bir biriktirme listesi sıranın hataları oluşturur, istemci farklı biriktirme listesi sırası deneyemez değil ve iletinizi göndermek başarısız olabilir. Bu değer 10 değerine bazı büyük değer ve varsayılan ayarı öneririz. Bu, uygulamanızın günde gönderdiği ne kadar veri bağlı olarak daha yüksek veya düşük bir değere değiştirebilirsiniz. Her bir biriktirme listesi sırası 5 GB iletilerinin basılı tutabilirsiniz.
* *failoverInterval*: süre boyunca, kabul edeceği hataları birincil ad alanı üzerinde tek bir varlık ikincil ad alanına geçmeden önce. Yük devretme, bir varlık tarafından varlık temelinde oluşur. Tek bir ad alanındaki varlıklara sık Service Bus içinde farklı düğümleri dinamik. Bir varlık içinde bir hata başka bir hata göstermez. Bu değer ayarlamak [System.TimeSpan.Zero] [ System.TimeSpan.Zero] ilk, geçici olmayan hata hemen sonra ikincil bir yük devretme için. Yük devretme Zamanlayıcı tetikleyen hatalarıdır herhangi [MessagingException] [ MessagingException] , [IsTransient] [ IsTransient] özelliği false veya [System.TimeoutException][System.TimeoutException]. Diğer özel durumlar gibi [UnauthorizedAccessException] [ UnauthorizedAccessException] yük devretme, istemci yanlış yapılandırılmış belirttiğinden neden olmaz. A [ServerBusyException] [ ServerBusyException] doğru deseni 10 saniye bekleyin olduğundan değil neden yük devretme sonra ileti yeniden göndermez.
* *enableSyphon*: Bu belirli eşlemeyi iletileri ikincil ad alanından geri birincil ad alanı da syphon olduğunu gösterir. İleti gönderme uygulamalar genel olarak, bu değeri ayarlamak **false**; iletilerini uygulamalar bu değeri ayarlanmalıdır **doğru**. Bunun nedeni, sık sık olduğunu ileti gönderenler daha az ileti alıcıları ' dir. Alıcıları sayısına bağlı olarak, bir tek bir uygulama örneği Sifon görevlerini işlemek sahip olmayı seçebilirsiniz. Birçok alıcıları kullanarak her biriktirme listesi sırası için fatura etkilere sahiptir.

Kodu kullanmak için birincil oluşturma [Eventhubclient] [ MessagingFactory] örneği, ikincil bir [Eventhubclient] [ MessagingFactory] örneği, ikincil bir [NamespaceManager] [ NamespaceManager] örneği ve [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] örneği. Çağrı aşağıdaki gibi basit olabilir:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Görev döndürdü tarafından [PairNamespaceAsync] [ PairNamespaceAsync] yöntemi tamamlayan, her şeyi yukarı ve kullanıma hazır ayarlanır. Görev döndürülmeden önce tüm doğru çalışması gerekli eşleştirme için arka plan iş tamamlanmamış olabilir. Sonuç olarak, görev dönene kadar ileti gönderme başlamamalıdır. Hatalı kimlik bilgileri veya biriktirme listesi sıraları oluşturma hatası gibi hatalar oluştuğunda görev tamamlandıktan sonra bu özel durum. Görev döndüğünde, kuyruklar bulunan inceleyerek oluşturulmuş olduğunu doğrulayın veya [BacklogQueueCount] [ BacklogQueueCount] özelliği, [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] örneği. Önceki kod için bu işlem aşağıdaki gibidir:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Sonraki adımlar
Zaman uyumsuz Service Bus Mesajlaşma temel bilgileri öğrendiğinize göre hakkında daha fazla ayrıntı okuyun [eşleştirilmiş ad alanları][paired namespaces].

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
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md
