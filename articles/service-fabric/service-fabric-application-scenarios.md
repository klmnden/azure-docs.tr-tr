---
title: Uygulama senaryoları ve tasarım | Microsoft Docs
description: Bulut uygulamalarının Service fabric'te kategorileri genel bakış. Durum bilgisi olan ve olmayan hizmetleri kullanan uygulama tasarımı açıklanır.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/24/2019
ms.author: atsenthi
ms.openlocfilehash: 6563d6e7c454f44e1a70d725191e56d3f90315c2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052609"
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric uygulama senaryoları
Azure Service Fabric, yazma ve çeşitli iş uygulamaları ve hizmetleri çalıştırmak güvenilir ve esnek bir platform sunar. Durum bilgisi olan veya bu uygulamalar ve mikro hizmetler olabilir ve bunlar kaynak dengeli verimliliğini en üst düzeye çıkarmak için sanal makineler arasında. 

Service Fabric benzersiz mimarisi, neredeyse gerçek zamanlı veri analizi, bellek içi hesaplama, paralel işlemleri ve olay işleme, uygulamalarınızda gerçekleştirmenizi sağlar. Kolayca uygulamalarınızı yukarı veya aşağı (gerçekten iç veya dış), değişen kaynak gereksinimlerinize bağlı olarak ölçeklendirebilirsiniz.

Uygulamaları oluşturma tasarım kılavuzu için okuma [Azure Service fabric'te mikro hizmet mimarisi](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) ve [Service Fabric kullanarak uygulama tasarımı için en iyi yöntemler](service-fabric-best-practices-applications.md).

Service Fabric platform için aşağıdaki uygulama türlerini kullanarak göz önünde bulundurun:

* **Veri toplama ve işleme IOT**: Service Fabric, büyük ölçekli işleme ve düşük gecikme süresi, durum bilgisi olan hizmetler aracılığıyla sahiptir. Bu işlem verilerini milyonlarca cihaz ve hesaplama için veri birlikte nerede yardımcı olabilir.

    IOT Hizmetleri, Service Fabric kullanılarak oluşturulmuş müşterilerin dahil [Honeywell](https://customers.microsoft.com/story/honeywell-builds-microservices-based-thermostats-on-azure), [PCL Construction](https://customers.microsoft.com/story/pcl-construction-professional-services-azure), [Crestron](https://customers.microsoft.com/story/crestron-partner-professional-services-azure), [BMW](https://customers.microsoft.com/story/bmw-enables-driver-mobility-via-azure-service-fabric/), [ Schneider elektrik](https://customers.microsoft.com/story/schneider-electric-powers-engergy-solutions-on-azure-service-fabric), ve [Mesh sistemleri](https://customers.microsoft.com/story/mesh-systems-lights-up-the-market-with-iot-based-azure-solutions).

* **Oyunlar ve oturum tabanlı etkileşimli uygulamalar**: Service Fabric uygulamanızı düşük gecikme süreli okuma ve yazma işlemleri gibi çevrimiçi oyun veya anlık ileti gerektiriyorsa yararlı olur. Service Fabric, ayrı bir depo veya önbellek oluşturmaya gerek olmadan bu etkileşimli, durum bilgisi olan uygulamalar oluşturmanıza olanak tanır. Ziyaret [Azure oyun çözümlerinin](https://azure.microsoft.com/solutions/gaming/) tasarım kılavuzu için [oyun Hizmetleri Service Fabric kullanarak](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).

    Oyun Hizmetleri geliştirdim müşterilerimiz arasında [Next Games](https://customers.microsoft.com/story/next-games-media-telecommunications-azure) ve [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/). Etkileşimli oturumları geliştirdim müşterilerimiz arasında [Hololens ile Honeywell](https://customers.microsoft.com/story/honeywell-manufacturing-hololens).

* **Veri analizi ve iş akışı işleme**: Olayları ya da akış veri Avantajı'ndan en iyi duruma getirilmiş okuma ve yazma Service fabric'te güvenilir bir şekilde işlemesi gerekir uygulamaları. Service Fabric uygulama işleme komut zincirini, güvenilir ve sonraki işleme aşamaya herhangi bir kayıp olmadan oturum başarılı sonuçları nerede olmalıdır da destekler. Bu işlem hatları veri tutarlılığı ve hesaplama garantisi önemli olduğu, işlemsel ve mali sistemlerini içerir.

    İş akışı hizmetlerini geliştirdim müşterilerimiz arasında [Zeiss grubu](https://customers.microsoft.com/story/zeiss-group-focuses-on-azure-service-fabric-for-key-integration-platform), [çekirdek iş çözümleri](https://customers.microsoft.com/en-us/story/quorum-business-solutions-expand-energy-managemant-solutions-using-azure-service-fabric), ve [Société genel](https://customers.microsoft.com/en-us/story/societe-generale-speeds-real-time-market-quotes-using-azure-service-fabric).

* **Veriler üzerinde bir hesaplama**: Service Fabric, yoğun veri hesaplama yapmak durum bilgisi olan uygulamalar oluşturmanıza olanak tanır. Service Fabric uygulamaları (hesaplama) işleme ve verileri birlikte bulundurma sağlar. 

   Normalde, uygulama verilerine erişim gerektirdiğinde, bir dış veri önbelleği veya depolama katmanı ile ilişkili ağ gecikmesini bilgi işlem süresini sınırlar. Service Fabric durum bilgisi olan hizmetler, gecikme süresini ortadan kaldırmak, daha en iyi duruma getirilmiş etkinleştirme okur ve yazar. 
   
   Örneğin, 100 milisaniyeden gidiş dönüş süresi gereksinimi ile müşteriler gerçek zamanlı öneri seçimlerini yakın gerçekleştiren bir uygulamayı düşünün. Service Fabric Hizmetleri gecikme süresi ve performans özelliklerini, kullanıcıya Uzaktan Depolama kullanarak gerekli verileri getirmek zorunda standart uygulama modeli ile karşılaştırıldığında, hızlı yanıt veren bir deneyim sağlar. Öneri seçimi hesaplama veriler ve kuralları ile birlikte olduğundan daha esnek bir sistemdir.

    Hesaplama Hizmetleri geliştirdim müşterilerimiz arasında [Solidsoft yanıt](https://customers.microsoft.com/story/solidsoft-reply-platform-powers-e-verification-of-pharmaceuticals) ve [Infosupport](https://customers.microsoft.com/story/service-fabric-customer-profile-info-support-and-fudura).

* **Yüksek oranda kullanılabilir hizmetler**: Service Fabric, birden fazla ikincil service çoğaltma oluşturarak hızlı yük devretmeyi sağlar. Bir düğüm, işlem veya bireysel hizmet donanım veya diğer hatası nedeniyle arıza yaparsa, ikincil çoğaltmalardan birine bir birincil çoğaltmaya hizmetinin en az düzeyde kayıp ile yükseltilir.

* **Ölçeklenebilir Hizmetler**: Küme genelinde ölçeği durumuna izin verme tek başına hizmetlerinden bölümlenebilir. Tek başına hizmetlerinden de oluşturulabilir ve çalışma sırasında kaldırıldı. Bazı örnekler birkaç düğüm üzerinde hizmetlerinden birçok düğüm örneklerinde binlerce ölçeği ve bunları gerektiği şekilde yeniden ölçeklendirin. Service Fabric, bu hizmetleri oluşturmak ve bunların tam yaşam döngüsü yönetmek için kullanabilirsiniz.

## <a name="application-design-case-studies"></a>Uygulama tasarım örnek olay incelemeleri
Service Fabric uygulamaları tasarlamak için nasıl kullanıldığını gösteren örnek olay incelemeleri yayımlanır [müşteri hikayeleri](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc/) ve [azure'da mikro Hizmetler](https://azure.microsoft.com/solutions/microservice-applications/) siteleri.

## <a name="designing-applications-composed-of-stateless-and-stateful-microservices"></a>Durum bilgisiz ve durum bilgisi olan mikro hizmetlerden uygulamalar tasarlama
Azure Cloud Services çalışan rolleri ile uygulamalar oluşturan bir durum bilgisi olmayan hizmet örneğidir. Buna karşılık, durum bilgisi olan mikro hizmetler, isteğin ve yanıtının ötesinde yetkili durumlarına korur. Yüksek kullanılabilirlik ve tutarlılık durumu çoğaltma tarafından desteklenen işlem garanti sağlayan basit API'ler aracılığıyla bu işlevselliği sağlar. 

Service fabric'te durum bilgisi olan hizmetler yüksek oranda kullanılabilir uygulamalar, yalnızca veritabanları ve diğer veri depolarına tüm türleri getirin. Doğal bir ilerleme budur. Uygulamalar, NoSQL veritabanları için yüksek kullanılabilirlik için yalnızca ilişkisel veritabanları kullanan zaten taşıdık. Uygulamalar artık "sıcak" durumu ve bunların içinde ek performans artışları için güvenilirlik, tutarlılık veya kullanılabilirlikten ödün vermeden yönetilen verileri olabilir.

Mikro oluşmuş uygulamalar oluştururken, durum bilgisi olmayan web uygulamaları (örneğin, ASP.NET ve Node.js) birleşimine normalde sahip durum bilgisiz ve durum bilgisi olan iş orta katman hizmetleri üzerine çağırma. Uygulama ve hizmetlerden tüm Service Fabric dağıtım komutları aracılığıyla aynı Service Fabric kümesine dağıtılır. Bu hizmetlerin her biri, Ölçek, güvenilirlik ve kaynak kullanımı ile ilgili bağımsızdır. Bu bağımsızlığı çevikliği ve geliştirme ve yaşam döngüsü yönetimi esnekliği artırır.

Durum bilgisi olan mikro hizmetler, bunlar ek kuyruklar ve geleneksel olarak yalnızca durum bilgisiz uygulamaların kullanılabilirlik ve gecikme süresi gereksinimlerini karşılamak için gerekli önbellekler için ihtiyacını ortadan kaldırıyor çünkü uygulama tasarımları basitleştirin. Durum bilgisi olan hizmetler yüksek kullanılabilirlik ve düşük gecikme süresine sahip olduğundan, uygulamanızı yönetmek için daha az ayrıntı yok. 

Aşağıdaki diyagramlarda, durum bilgisi olmayan bir uygulama ve durum bilgisi olan bir tasarlama arasındaki farklar gösterilmektedir. Yararlanarak [Reliable Services](service-fabric-reliable-services-introduction.md) ve [Reliable Actors](service-fabric-reliable-actors-introduction.md) programlama modellerini, durum bilgisi olan hizmetler uygulama yüksek aktarım hızı ve düşük gecikme süresi elde edin karmaşıklığı ortadan kaldırın.

Durum bilgisi olmayan hizmetler kullanan bir örnek uygulamayı şu şekildedir: ![Durum bilgisi olmayan hizmetler kullanan uygulama][Image1]

Durum bilgisi olan hizmetler kullanan bir örnek uygulamayı şu şekildedir: ![Durum bilgisi olmayan hizmetler kullanan uygulama][Image2]

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [modeller ve senaryolar](service-fabric-patterns-and-scenarios.md).

* Service Fabric ile durum bilgisiz ve durum bilgisi olan hizmetler oluşturmaya başlama [Reliable Services](service-fabric-reliable-services-quick-start.md) ve [Reliable Actors](service-fabric-reliable-actors-get-started.md) programlama modeli.
* Yönergeler için Azure Mimari Merkezi ziyaret [Azure üzerinde mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/).
* Git [Azure Service Fabric uygulama ve küme en iyi uygulamalar](service-fabric-best-practices-overview.md) uygulama tasarım kılavuzu için.

* Ayrıca aşağıdaki konulara bakın:
  * [Mikro hizmetler hakkında bilgi ver](service-fabric-overview-microservices.md)
  * [Tanımlama ve hizmet durumunu yönetme](service-fabric-concepts-state.md)
  * [Service Fabric hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
  * [Service Fabric Hizmetleri ölçeklendirme](service-fabric-concepts-scalability.md)
  * [Partition Service Fabric Hizmetleri](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
