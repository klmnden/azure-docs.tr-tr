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
ms.openlocfilehash: 50778ae742c1ec66857a6c2fa6250dc3d67e5601
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60531110"
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
Service Bus ile azaltma, ortak ileti oranı yönetimi sağlar. Her bir Service Bus düğümün birçok varlıklar barındırır. Bu varlıkların her biri sistemdeki CPU, bellek, depolama ve diğer özellikleri açısından taleplerini yapar. Bu modellerin hiçbirini algıladığında kullanım, tanımlanmış eşikleri aştığında, Service Bus, belirtilen bir isteğin reddedebilirsiniz. Çağıranın alır bir [ServerBusyException][ServerBusyException] ve 10 saniye sonra yeniden deneme sayısı.

Bir risk azaltma, kod okuma hatası ve en az 10 saniye boyunca iletinin herhangi bir yeniden deneme durdurmak gerekir. Müşteri uygulamasının parçaları arasında hata oluşabilir olduğundan, her parça bağımsız olarak yeniden deneme mantığı yürütür bekleniyor. Kod, bir kuyruk veya konuda bölümleme etkinleştirerek aşarak olasılığını azaltabilirsiniz.

### <a name="issue-for-an-azure-dependency"></a>Sorun için bir Azure bağımlılığı
Azure'daki diğer bileşenleri, zaman zaman hizmet sorun yaşayabilir. Örneğin, Service Bus kullanan bir sistemi yükseltilirken, sistemin geçici olarak azaltılmış özellikleri oluşabilir. Bu tür sorunları geçici olarak çözmek için Service Bus düzenli olarak araştırdıktan ve risk azaltma işlemleri uygular. Bu risk azaltma işlemleri yan etkilerini görünür. Örneğin, geçici bir sorun depolama ile işlemek için Service Bus ileti gönderme işlemleri tutarlı bir şekilde çalışmaya olanak sağlayan bir sistemi uygular. Risk azaltma yapısı nedeniyle, gönderilen bir iletinin etkilenen kuyruk veya abonelik içinde görünür ve bir alma işlemi için hazır olması 15 dakika sürebilir. Genel olarak bakıldığında, çoğu varlık bu sorunla karşılaşmazsınız. Ancak, varlıkların sayısı, azure'da hizmet veri yolu verildiğinde, bu risk azaltma bazen Service Bus müşterilerinin küçük bir kısmı için gereklidir.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus hata tek bir alt sistem durumunda
Herhangi bir uygulama ile koşullarda bir iç bileşen Service Bus'ın tutarsız hale gelmesine neden olabilir. Service Bus bu algıladığında, uygulamadaki ne tanılamaya yardımcı olmak için verileri toplar. Veriler toplandıktan sonra uygulama girişimi, tutarlı bir duruma döndürmek için yeniden başlatılır. Bu işlem oldukça hızlı bir şekilde gerçekleşir ve kapalı kaldıkları süreleri tipik ancak birkaç dakika kadar için kullanılamaz olarak görünen bir varlıkta sonuçları çok daha kısa.

Bu durumda, istemci uygulamanın oluşturduğu bir [System.TimeoutException][System.TimeoutException] veya [Istransient][MessagingException] özel durum. Service Bus, bu sorunun otomatik istemci yeniden deneme mantığı biçiminde bir risk azaltma içerir. Yeniden deneme süresi dolana ve iletiyi teslim sonra diğer makalesinde belirtilen kullanarak keşfedebilirsiniz [kesintileri ve olağanüstü durumları yönetme][handling outages and disasters].

## <a name="next-steps"></a>Sonraki adımlar
Service Bus içinde zaman uyumsuz Mesajlaşma temel bilgileri öğrendiniz, daha fazla bilgi okuyun [kesintileri ve olağanüstü durumları yönetme][handling outages and disasters].

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
[handling outages and disasters]: service-bus-outages-disasters.md
