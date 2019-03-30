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
ms.date: 7/02/2017
ms.author: atsenthi
ms.openlocfilehash: c9b2f9ac131e71b7c6b37ed85568adc0c3978dc2
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668254"
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric uygulama senaryoları
Azure Service Fabric sayesinde yazmak ve çeşitli iş uygulamaları ve hizmetleri çalıştırmak güvenilir ve esnek bir platform sunar. Durum bilgisi olan veya bu uygulamalar ve mikro hizmetler olabilir ve bunlar verimliliğini en üst düzeye çıkarmak için sanal makineler arasında kaynak dengeli. Service Fabric benzersiz mimarisi, neredeyse gerçek zamanlı veri analizi, bellek içi hesaplama, paralel işlemleri ve olay işleme, uygulamalarınızda gerçekleştirmenizi sağlar. Kolayca uygulamalarınızı yukarı veya aşağı (gerçekten iç veya dış), değişen kaynak gereksinimlerinize bağlı olarak ölçeklendirebilirsiniz.

Azure Service Fabric platform uygulamaları aşağıdaki kategoriler için idealdir:

* **Yüksek oranda kullanılabilir hizmetler**: Service Fabric Hizmetleri birden fazla ikincil service çoğaltma oluşturarak hızlı yük devretmeyi sağlar. Bir düğüm, işlem veya bireysel hizmet donanım veya diğer hatası nedeniyle arıza yaparsa, ikincil çoğaltmalardan birine bir birincil çoğaltmaya hizmetinin en az düzeyde kayıp ile yükseltilir.
* **Ölçeklenebilir Hizmetler**: Küme genelinde ölçeği durumuna izin verme tek başına hizmetlerinden bölümlenebilir. Ayrıca, tek başına hizmetlerinden oluşturulabilir ve çalışma sırasında kaldırıldı. Hizmetleri hızla ve kolayca birkaç düğüm üzerinde birkaç örneklerinden birçok düğüm örneklerinde binlerce ölçeği ve ardından yeniden kaynak gereksinimlerinize bağlı olarak ölçeği. Service Fabric, bu hizmetleri oluşturmak ve tam döngülerini yönetmek için kullanabilirsiniz.
* **Statik olmayan veri çubuğunda hesaplama**: Service Fabric, yapılandırma verilerini, giriş/çıkış ve işlem yoğun durum bilgisi olan uygulamalar sağlar. Service Fabric uygulamaları düzenleme işlemi (hesaplama) ve veri sağlar. Normalde, uygulama verilerine erişim gerektirdiğinde, bir dış veri önbelleği veya depolama katmanı ile ilişkili ağ gecikmesini yoktur. Durum bilgisi olan Service Fabric Hizmetleri ile daha fazla yüksek performanslı etkinleştirme okur ve yazar, gecikme süresi çıkarıldı. Örneğin, 100 milisaniyeden gidiş dönüş süresi gereksinimi ile müşteriler gerçek zamanlı öneri seçimlerini yakın gerçekleştiren bir uygulama olduğunu düşünelim. Gecikme süresi ve performans özelliklerini (burada öneri seçimi hesaplama veri ve kuralları ile birlikte bulunan) Service Fabric Hizmetleri, standart uygulama modeli ile karşılaştırıldığında kullanıcıya hızlı yanıt veren bir deneyim sağlar Uzaktaki depolama biriminden gerekli verileri getirmek zorunda.  
* **Oturum tabanlı etkileşimli uygulamalar**: Service Fabric uygulamalarınızın çevrimiçi oyun veya anlık Mesajlaşma gibi düşük gecikme süreli okuma ve yazma işlemleri gerektiriyorsa yararlı olur. Service Fabric, ayrı bir depo veya önbellek, durum bilgisiz uygulamalar için gerekli olarak oluşturmaya gerek olmadan bu etkileşimli, durum bilgisi olan uygulamalar oluşturmanıza olanak tanır. (Bu gecikme süresi artar ve büyük olasılıkla tutarlılık sorunlar açıklanır.).
* **Veri analizi ve iş akışları**: Hızlı Okuma ve yazma işlemleri, Service Fabric olayları veya veri akışları güvenilir bir şekilde işlemesi gerekir uygulamaları etkinleştirin. Service Fabric ayrıca işleme komut zincirini, güvenilir ve sonraki işleme aşamasına kaybı olmadan oturum başarılı sonuçları nerede olmalıdır açıklayan uygulamalar sağlar. Bunlar, veri tutarlılığı ve hesaplama garantisi önemli olduğu, işlemsel ve mali sistemlerini içerir.
* **Veri toplama, işleme ve IOT**: Service Fabric, büyük ölçekli işleme ve düşük gecikme süresi, durum bilgisi olan hizmetler aracılığıyla sahip olduğundan, cihaz ve hesaplama için veri birlikte bulunduğu yere milyonlarca verileri işlemek için idealdir.
Service Fabric da dahil olmak üzere IOT sistemlerinde yerleşik çeşitli müşteriler gördük [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider elektrik](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) ve [Mesh sistemleri](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Uygulama tasarım örnek olay incelemeleri
Service Fabric uygulamaları tasarlamak için nasıl kullanıldığını gösteren örnek olay incelemeleri sayısı yayımlanan [Service Fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) ve [mikro hizmet çözümlerini site](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Durum bilgisiz ve durum bilgisi olan mikro hizmetlerden oluşan uygulamalar tasarlayın
Azure bulut hizmeti çalışan rollerini içeren uygulamalar oluşturan bir durum bilgisi olmayan hizmet örneğidir. Buna karşılık, durum bilgisi olan mikro hizmetler, isteğin ve yanıtının ötesinde yetkili durumlarına korur. Bu, yüksek kullanılabilirlik ve tutarlılık çoğaltma tarafından desteklenen işlem garanti sağlayan basit API'ler aracılığıyla durumu sağlar. Service Fabric'in durum bilgisi olan hizmetler yüksek kullanılabilirliğe sahip uygulamalar, yalnızca veritabanları ve diğer veri depolarına tüm türleri için çerçevesinde, herkesin. Doğal bir ilerleme budur. Uygulamalar, NoSQL veritabanları için yüksek kullanılabilirlik için yalnızca ilişkisel veritabanları kullanan zaten taşıdık. Uygulamalar artık "sıcak" durumu ve bunların içinde ek performans artışları için güvenilirlik, tutarlılık veya kullanılabilirlikten ödün vermeden yönetilen verileri olabilir.

Mikro oluşan uygulamalar oluştururken, durum bilgisi olmayan web uygulamaları (ASP.NET, Node.js vb.) birleşimine normalde sahip durum bilgisiz ve durum bilgisi olan iş orta katman Hizmetleri'ni arama, tüm Service Fabric kümesi kullanarak aynı içine dağıtılan Service Fabric dağıtım komutları. Bu hizmetlerin her biri, çevikliği geliştirmeyi ve yaşam döngüsü yönetimini büyük ölçüde geliştirme, Ölçek, güvenilirlik ve kaynak kullanımı ile ilgili bağımsızdır.

Durum bilgisi olan mikro hizmetler, bunlar ek kuyruklar ve geleneksel olarak yalnızca durum bilgisiz uygulamaların kullanılabilirlik ve gecikme süresi gereksinimlerini karşılamak için gerekli önbellekler için ihtiyacını ortadan kaldırıyor çünkü uygulama tasarımları basitleştirin. Durum bilgisi olan hizmetler doğal olarak yüksek oranda kullanılabilir ve düşük gecikme süreli olduğundan, bu, uygulamanızı bir bütün olarak yönetmek için daha az sayıda hareketli parçadan olduğu anlamına gelir. Aşağıdaki diyagramlar, durum bilgisi olmayan bir uygulama ve durum bilgisi olan bir tasarlama arasındaki farklar gösterilmektedir. Yararlanarak [Reliable Services](service-fabric-reliable-services-introduction.md) ve [Reliable Actors](service-fabric-reliable-actors-introduction.md) programlama modellerini, durum bilgisi olan hizmetler uygulama yüksek aktarım hızı ve düşük gecikme süresi elde edin karmaşıklığı ortadan kaldırın.

## <a name="an-application-built-using-stateless-services"></a>Durum bilgisi olmayan hizmetler kullanılarak oluşturulmuş bir uygulama
![Durum bilgisi olmayan hizmet kullanarak uygulama][Image1]

## <a name="an-application-built-using-stateful-services"></a>Durum bilgisi olan hizmetler kullanılarak oluşturulmuş bir uygulama
![Durum bilgisi olmayan hizmet kullanarak uygulama][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [müşteri örnek olay incelemeleri](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* Daha fazla bilgi edinin [modeller ve senaryolar](service-fabric-patterns-and-scenarios.md)

* Service Fabric ile durum bilgisiz ve durum bilgisi olan hizmetler oluşturmaya başlama [güvenilir hizmetler](service-fabric-reliable-services-quick-start.md) ve [reliable actors](service-fabric-reliable-actors-get-started.md) programlama modeli.
* Ayrıca aşağıdaki konulara bakın:
  * [Mikro hizmetler hakkında bilgi ver](service-fabric-overview-microservices.md)
  * [Tanımlama ve hizmet durumunu yönetme](service-fabric-concepts-state.md)
  * [Service Fabric hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
  * [Service Fabric Hizmetleri ölçeklendirme](service-fabric-concepts-scalability.md)
  * [Partition Service Fabric Hizmetleri](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
