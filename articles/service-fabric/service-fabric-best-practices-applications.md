---
title: Azure Service Fabric uygulama tasarım en iyi yöntemler | Microsoft Docs
description: Service Fabric uygulamaları geliştirmek için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: markfussell
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/26/2019
ms.author: msfussell
ms.openlocfilehash: 55f043effc7cdb102acea856e89c58f660d0cde5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65237755"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric uygulama tasarım en iyi uygulamaları

Bu makalede, Service Fabric üzerinde uygulamalar ve hizmetler oluşturmaya yönelik en iyi uygulama Kılavuzu sağlanmaktadır.
 
## <a name="get-familiar-with-service-fabric"></a>Service Fabric ile hakkında bilgi edinin
* [Bu nedenle, Service Fabric hakkında öğrenmek ister misiniz?](service-fabric-content-roadmap.md)
* Hakkında bilgi edinin [Service Fabric uygulama senaryoları](service-fabric-application-scenarios.md)
* Ardından programlama modeli seçenekleri anlamak [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md)



## <a name="application-design-guidance"></a>Uygulama Tasarım Kılavuzu
Sahibi [genel mimari](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) bir Service Fabric uygulamasının ve kendi [tasarım konuları](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Bir API ağ geçidi seçin
Arka uç hizmetlerine dışa Genişletilebilir iletişim kuran bir API ağ geçidi hizmeti kullanın. Kullanılan en yaygın API ağ geçidi hizmetler şunlardır:

- [Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), olduğu [Service Fabric ile tümleşik](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management)
- [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/) veya [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) kullanarak [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) olay hub'ı bölümleri okumak için
- [Ters proxy Træfik](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/) kullanarak [Azure Service Fabric sağlayıcısı](https://docs.traefik.io/configuration/backends/servicefabric/)
- [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) Not: Bu doğrudan Service Fabric ile tümleşik olmayan ve Azure API Management, genellikle tercih edilen bir seçim
- Kendi uzantınızı oluşturun [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) web uygulama ağ geçidi

### <a name="choose-stateless-services"></a>Durum bilgisi olmayan hizmetler seçin
Derleme durum bilgisi olmayan hizmetleri kullanarak her zaman başlatın önerilir [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) durumu bir Azure veritabanı, Azure Cosmosdb'ye veya Azure depolama içinde depolamak. Te dış durumuna sahip çoğu geliştirici için daha tanıdık bir yaklaşımdır ve ayrıca mağaza sorgu özelliklerinden yararlanmanıza olanak sağlar.  

### <a name="when-to-choose-stateful-services"></a>Ne zaman durum bilgisi olan hizmetler seçin
Bir senaryo için düşük gecikme süresi olan ve verilerin işlem yakınında tutmak mı, durum bilgisi olan hizmetler göz önünde bulundurun. IOT dijital ikizini cihazları, oyun durumunu, oturum durumu, bir veritabanındaki verileri önbelleğe alma ve diğer hizmetlere yönelik çağrıları izlemek için iş akışı uzun süre çalışan örnek verilebilir.

Veri bekletme zaman çerçevesi karar verin:

- Önbelleğe alınan veriler - önbelleğe alma gecikme harici depolar için bir sorun olduğu kullanırsınız. Kendi verilerinizi önbelleğe alın ya da kullanmayı gibi durum bilgisi olan bir hizmet kullanın [açık kaynağı SoCreate service-fabric-dağıtılmış-önbelleği](https://github.com/SoCreate/service-fabric-distributed-cache). Bu senaryoda, önbellekteki tüm verileri kaybedebilir ve önemli değildir.
- Zamana bağlı veriler - gecikme süresi, işlem bir süreliğine verileriniz yakınınızda tutmak mı, ancak bu verileri, kayıp kabul edilebildiği bir *olağanüstü durum* senaryo. Örneğin, birçok IOT çözümlerinde veri yakın olması gerekir ancak bu veriler kaybolursa, belirli veri işaret eden son birkaç günde, örneğin ortalama sıcaklık hesaplama işlem, kaydedilen bu önemli değildir. Ayrıca bu senaryoda, genellikle dış depolama birimine düzenli aralıklarla yazılan yalnızca hesaplanan ortalama değer tek tek veri noktalarının bir yedekleme hakkında planlandığını önemsemez.  
- Uzun süreli verileri - güvenilir koleksiyonlar verilerinizi kalıcı olarak depolayabilirsiniz. Ancak, bu durumda, gerekli [olağanüstü durum kurtarmasına hazırlanma](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery) dahil olmak üzere [düzenli yedekleme ilkelerini yapılandırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) kümeleriniz için. Aslında, kümenizin bir olağanüstü durum senaryosunda edildiğinde ne olur, burada, yeni bir küme oluşturmanız gerekir ve yeni uygulama örnekleri dağıtma ve en son yedekten kurtarma yapılandırın.

Maliyet tasarrufu sağlayan ve kullanılabilirliği geliştirmek:
- Azure Redis gibi başka bir hizmet kullanmaya gerek yoktur ve uzak Mağazası'ndan veri erişim ve işlem maliyetleri tabi durum bilgisi olan hizmetler ile maliyetleri azaltabilirsiniz.
- Birincil depolama ve işlem için durum bilgisi olan hizmetler kullanarak, pahalı ve önerilmez. Durum bilgisi olan hizmetler ucuz yerel depolama ile işlem olarak göz önünde bulundurun.
- Bağımlı diğer hizmetleri kaldırarak, hizmet kullanılabilirliği iyileştirebilir. Kümede ile HA yönetilen duruma sahip diğer hizmet kapalı kalma süreleri veya gecikme sorunlarını yalıtır.

## <a name="how-to-properly-work-with-reliable-services"></a>Reliable Services özelliğiyle düzgün çalışması nasıl
Güvenilir Hizmetleri durum bilgisiz ve durum bilgisi olan hizmetler kolayca oluşturmanıza olanak sağlar. Okuma [Reliable Services giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction)
- Her zaman dikkate [iptal belirteci](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) içinde `RunAsync()` durum bilgisiz ve durum bilgisi olan hizmetler için yöntemi ve `ChangeRole()` durum bilgisi olan hizmetler için yöntemi. Bu olmadan, Service Fabric hizmetinizi Kapalı durumunda bilmez. Örneğin, iptal belirteci uygularken değil, çok daha uzun uygulama yükseltme kez neden olabilir.
-   Açma ve kapama [iletişim dinleyicileri](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) zamanında ve iptal belirteçlerini uygulanır.
-   Hiçbir zaman eşitleme kodu zaman uyumsuz kod ile karıştırma. Örneğin, kullanmayın `.GetAwaiter().GetResult()` zaman uyumsuz olması gereken, zaman uyumsuz çağrı; *tüm* çağrı yığını aracılığıyla.

## <a name="how-to-properly-work-with-reliable-actors"></a>Reliable Actors hizmetini kullanmaya düzgün çalışması nasıl
Reliable Actors aktör, durum bilgisi olan, sanal kolayca oluşturmanıza olanak sağlar. Okuma [Reliable Actors hizmetine giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction)

- Kesin pub/sub arasında aktör uygulaması ölçeklendirme için Mesajlaşma kullanmayı düşünün. Örneğin, [açık kaynağı SoCreate pub/sub](https://service-fabric-pub-sub.socreate.it/) veya [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Aktör durumu olarak olun [mümkün olduğunca ayrıntılı](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Yönetme [aktörün yaşam döngüsü](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Aktör, hiç olmadığı kadar yeniden kullanılacak etmeyecekseniz silin. Bu özellikle doğrudur kullanıldığında [VolatileState sağlayıcısı](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication) gibi tüm durumu bellekte depolanır.
- Şu nedenle kendi [Aç tabanlı eşzamanlılık](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency) aktörler nesneleri bağımsız en iyi şekilde kullanılır. Grafikler (büyük olasılıkla, her biri ayrı ağ çağrısı olur) çok aktör, zaman uyumlu yöntem çağrılarının oluşturmamaları veya döngüsel aktör istekleri; Bu, performansı ve ölçeği önemli ölçüde etkiler.
- Eşitleme kodu zaman uyumsuz kodu ile karıştırmayın; Bu performans sorunlarını önlemek için tüm zaman uyumsuz olması gerekir.
- Uzun süre çalışan çağrıları düzenindeki aktörler yapmayın, diğer çağrıları sırayla oynadıkları eşzamanlılık nedeniyle aynı aktör engeller.
- Kullanarak diğer hizmetlerle iletişim kurarken [remoting Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) ve oluşturmakta olduğunuz bir `ServiceProxyFactory`, fabrikada oluşturup [actor hizmetinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) düzeyi ve *değil*aktör düzeyinde.


## <a name="application-diagnostics"></a>Uygulama tanılama
- Ekleme kapsamlı [uygulama günlüğü](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) hizmet çağrılarındaki. Bu, burada hizmetler birbirini çağıran senaryoları Tanılama'da yardımcı olur. Örneğin, A ' -> zaman B -> C çağrı işlemi başarısız olabilir herhangi bir yerde; D -> günlüğe kaydetme yeterli değilse tanılamak zordur. En az Hizmetleri çok büyük çağrı hacimlerini nedeniyle oturum açıyorsanız, hataları ve uyarıları günlüğe emin olun.

## <a name="iot-and-messaging-applications"></a>IOT ve mesajlaşma uygulamaları
Gelen iletileri okurken [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/) veya [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) kullanın [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) Service Fabric güvenilir korumak için Hizmetler ile tümleşir Olay Hub'ından okuma durumu bölümler ve hizmetlerinizde yeni iletiler gönderir `IEventProcessor::ProcessEventsAsync()` yöntemi.


## <a name="design-guidance-on-azure"></a>Azure'da Tasarım Kılavuzu
* Ziyaret [Azure Mimari Merkezi](https://docs.microsoft.com/azure/architecture/microservices/) tasarım kılavuzu için [Azure'da mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/)

* Ziyaret [oyunlar için Azure ile çalışmaya başlama](https://docs.microsoft.com/gaming/azure/) tasarım kılavuzu için [oyun Hizmetleri Service Fabric kullanma](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)
