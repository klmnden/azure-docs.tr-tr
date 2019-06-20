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
ms.date: 06/18/2019
ms.author: msfussell
ms.openlocfilehash: 30d696337061ade6b79c7ec0e4c4de67651f0dad
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203447"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric uygulama tasarım en iyi uygulamaları

Bu makalede, Azure Service Fabric üzerinde uygulamalar ve hizmetler oluşturmaya yönelik en iyi uygulama Kılavuzu sağlanmaktadır.
 
## <a name="get-familiar-with-service-fabric"></a>Service Fabric ile hakkında bilgi edinin
* Okuma [Service Fabric hakkında bilgi edinmek, istediğiniz şekilde?](service-fabric-content-roadmap.md) makalesi.
* Hakkında bilgi edinin [Service Fabric uygulama senaryoları](service-fabric-application-scenarios.md).
* Programlama modeli seçimleri okuyarak anlamak [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).



## <a name="application-design-guidance"></a>Uygulama Tasarım Kılavuzu
Sahibi [genel mimari](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) Service Fabric uygulamaları ve bunların [tasarım konuları](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Bir API ağ geçidi seçin
Arka uç hizmetlerine dışa Genişletilebilir iletişim kuran bir API ağ geçidi hizmeti kullanın. Kullanılan en yaygın API ağ geçidi hizmetler şunlardır:

- [Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), olduğu [Service Fabric ile tümleşik](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management).
- [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/) veya [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/)kullanarak [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) olay hub'ı bölümleri okumak için.
- [Ters proxy Træfik](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/)kullanarak [Azure Service Fabric sağlayıcısı](https://docs.traefik.io/configuration/backends/servicefabric/).
- [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/).

   > [!NOTE] 
   > Azure Application Gateway, Service Fabric ile doğrudan tümleştirilmiş değil. Azure API Yönetimi genellikle tercih edilen seçenektir.
- Kendi özel olarak geliştirilmiş [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) web uygulama ağ geçidi.

### <a name="stateless-services"></a>Durum bilgisi olmayan hizmetler
Durum bilgisi olmayan hizmetler kullanılarak oluşturarak her zaman başlatın önerilen [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) ve durumu bir Azure veritabanı, Azure Cosmos DB veya Azure depolama içinde depolamak. Te dış durumu Çoğu geliştirici için daha tanıdık bir yaklaşımdır. Bu yaklaşım, mağaza sorgu özelliklerinden yararlanmanıza olanak sağlar.  

### <a name="when-to-use-stateful-services"></a>Durum bilgisi olan hizmetler kullanıldığı durumlar
Bir senaryo için düşük gecikme süresi olan ve verilerin işlem yakınında tutmak mı, durum bilgisi olan hizmetler göz önünde bulundurun. Bazı örnek senaryolar, IOT dijital ikizini cihazları, oyun durumunu, oturum durumu, veritabanı ve diğer hizmetlere yönelik çağrıları izlemek için uzun süre çalışan iş akışları verileri önbelleğe almayı içerir.

Veri bekletme zaman çerçevesi hakkında karar verin:

- **Önbelleğe alınmış veri**. Harici depolar için gecikme süresi, bir sorun olduğunda önbelleğe almayı kullanın. Durum bilgisi olan hizmet kendi veri önbelleği olarak kullanın veya kullanmayı [açık kaynaklı SoCreate Service Fabric dağıtılmış önbellek](https://github.com/SoCreate/service-fabric-distributed-cache). Bu senaryoda, önbellekteki tüm verileri kaybederseniz önceliğiniz olması gerekmez.
- **Zamana bağlı veriler**. Bu senaryoda, gecikme süresi için bir süre için işlem verileriniz yakınınızda tutmak gerekir, ancak, verileri kaybetmeyi göze bir *olağanüstü durum*. Örneğin, IOT çözümlerinin çoğu, veri işlem, zaman son birkaç gün içindeki ortalama sıcaklık hesaplanırken gibi Kapat olması gerekir ancak bu veriler kaybolursa, kaydedilen belirli veri noktalarını bu önemli değildir. Ayrıca, bu senaryoda, genellikle tek tek veri noktalarının yedekleme konusunda sizin için önemli değil. Yalnızca dış depolama birimine düzenli aralıklarla yazılır hesaplanan ortalama değerleri yedekleyin.  
- **Uzun süreli veri**. Güvenilir koleksiyonlar, verilerinizi kalıcı olarak depolayabilirsiniz. Ancak bu durumda gerekmez [olağanüstü durum kurtarmasına hazırlanma](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery)de dahil olmak üzere [düzenli yedekleme ilkelerini yapılandırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) kümeleriniz için. Aslında, kümenizin bir olağanüstü durumda imha edilirse ne olur, burada, yeni bir küme oluşturmanız gerekir ve yeni uygulama örnekleri dağıtma ve en son yedekten kurtarma yapılandırın.

Maliyet tasarrufu sağlayan ve kullanılabilirliği geliştirmek:
- Uzak depodan veri erişimi oluşmaması için durum bilgisi olan hizmetler kullanarak maliyeti ve işlem maliyetleri azaltmak ve Azure Cache için gibi başka bir hizmet kullanmanız gerekmez çünkü Redis.
- Birincil depolama ve işlem için durum bilgisi olan hizmetler kullanarak ucuzdur ve bunu önermiyoruz. Durum bilgisi olan hizmetler ucuz yerel depolama ile işlem olarak düşünün.
- Bağımlı diğer hizmetleri kaldırarak, hizmet kullanılabilirliği iyileştirebilir. HA durumuyla kümedeki yönetme diğer hizmet kapalı kalma süreleri veya gecikme sorunlarını yalıtır.

## <a name="how-to-work-with-reliable-services"></a>Reliable Services ile çalışma
Service Fabric güvenilir Hizmetleri durum bilgisiz ve durum bilgisi olan hizmetler kolayca oluşturmanıza olanak sağlar. Daha fazla bilgi için [Reliable Services giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction).
- Her zaman dikkate [iptal belirteci](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) içinde `RunAsync()` durum bilgisiz ve durum bilgisi olan hizmetler için yöntemi ve `ChangeRole()` durum bilgisi olan hizmetler için yöntemi. Aksi takdirde, Service Fabric hizmetinizi Kapalı durumunda bilmez. Örneğin, iptal belirteci dikkate yoksa, yükseltme süreleri daha uzun uygulama ortaya çıkabilir.
-   Açma ve kapama [iletişim dinleyicileri](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) zamanında bir şekilde ve iptal belirteçlerini uygulanır.
-   Hiçbir zaman eşitleme kodu zaman uyumsuz kod ile karıştırma. Örneğin, kullanmayın `.GetAwaiter().GetResult()` , zaman uyumsuz çağrılarındaki. Zaman uyumsuz kullanma *tüm* çağrı yığını aracılığıyla.

## <a name="how-to-work-with-reliable-actors"></a>Reliable Actors hizmetini kullanmaya çalışma konusunda
Service Fabric Reliable Actors aktör, durum bilgisi olan, sanal kolayca oluşturmanıza olanak sağlar. Daha fazla bilgi için [Reliable Actors giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction).

- Ciddi pub/sub arasında aktör uygulamanızı ölçeklendirme için Mesajlaşma kullanmayı düşünün. Bu hizmeti sağlamak araçlarda [açık kaynaklı SoCreate Service Fabric Pub/Sub](https://service-fabric-pub-sub.socreate.it/) ve [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Aktör durumu olarak olun [mümkün olduğunca ayrıntılı](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Yönetme [aktörün yaşam döngüsü](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Bunları tekrar kullanabilmek için etmeyecekseniz aktörler silin. Gereksiz aktörler silme, özellikle önemli kullanırken [geçici durumu sağlayıcısı](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication), tüm durumu bellekte depolanır.
- Nedeniyle kendi [sırayla oynadıkları eşzamanlılık](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency), aktör nesneleri bağımsız en iyi şekilde kullanılır. (Büyük olasılıkla, her biri ayrı ağ çağrısı olur) çok aktör, zaman uyumlu yöntem çağrılarının grafikleri oluşturun veya döngüsel aktör istekleri oluşturma kullanmayın. Bu, performansı ve ölçeği önemli ölçüde etkiler.
- Eşitleme kodu zaman uyumsuz kodu ile karıştırmayın. Zaman uyumsuz, performans sorunlarını önlemek için tutarlı bir şekilde kullanın.
- Uzun süre çalışan çağrıları düzenindeki aktörler yapmayın. Uzun süren aramalar bırakma tabanlı eşzamanlılık nedeniyle aynı aktör diğer çağrıları engeller.
- Kullanarak diğer hizmetlerle iletişim kurmaya varsa [remoting Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) ve oluşturmakta olduğunuz bir `ServiceProxyFactory`, fabrikada oluşturma [actor hizmetinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) düzeyi ve *değil* aktör düzeyinde.


## <a name="application-diagnostics"></a>Uygulama tanılama
Ekleme hakkında kapsamlı [uygulama günlüğü](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) hizmet çağrılarındaki. Hangi hizmetler birbirini çağıran senaryoları tanılamanıza yardımcı olur. Örneğin, çağrıları B çağrıları C D çağırdığında, çağrı herhangi bir yerde başarısız olabilir. Yeterli günlük yoksa, hataları tanılamak zordur. Hizmetleri çok büyük çağrı hacimlerini nedeniyle oturum açıyorsanız, en az hataları ve uyarıları günlüğe emin olun.

## <a name="iot-and-messaging-applications"></a>IOT ve mesajlaşma uygulamaları
Sizin okurken iletilerden [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/) veya [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/), kullanın [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor). ServiceFabricProcessor hizmetleriyle tümleştirilebilir Service Fabric güvenilir olay hub'ından okuma durumu bölümler ve hizmetlerinizde yeni iletiler gönderir korumak için `IEventProcessor::ProcessEventsAsync()` yöntemi.


## <a name="design-guidance-on-azure"></a>Azure'da Tasarım Kılavuzu
* Ziyaret [Azure Mimari Merkezi](https://docs.microsoft.com/azure/architecture/microservices/) tasarım kılavuzu için [Azure üzerinde mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/).

* Ziyaret [oyunlar için Azure ile çalışmaya başlama](https://docs.microsoft.com/gaming/azure/) tasarım kılavuzu için [oyun Hizmetleri Service Fabric kullanarak](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).
