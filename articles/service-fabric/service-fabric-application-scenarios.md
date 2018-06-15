---
title: Uygulama senaryoları ve tasarım | Microsoft Docs
description: Service Fabric bulut uygulamalarında kategorilerini genel bakış. Durum bilgisi olan ve durum bilgisiz hizmetleri kullanan uygulama tasarımı açıklanır.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/02/2017
ms.author: mfussell
ms.openlocfilehash: c0a9b24704a91d6a6893937b4ee03765fb05f092
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207545"
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric uygulama senaryoları
Azure Service Fabric yazma ve iş uygulamaları ve Hizmetleri türlerde çalıştırın olanak tanıyan güvenilir ve esnek bir platform sunar. Bu uygulama ve mikro durum bilgisiz veya durum bilgisi olan olabilir ve bunlar verimliliğini en üst düzeye çıkarmak için sanal makineler arasında kaynak dengeli. Service Fabric benzersiz mimarisi, gerçek zamanlı veri analizi, bellek içi hesaplama, paralel işlemleri ve olay uygulamalarınızda işleme yakın gerçekleştirmenizi sağlar. Kolayca uygulamalarınızı yukarı veya aşağı (gerçekten içeri veya dışarı), değişen kaynak gereksinimlerinize bağlı olarak ölçekleme yapabilirsiniz.

Azure Service Fabric platform, uygulamaları aşağıdaki kategorileri için idealdir:

* **Yüksek oranda kullanılabilir hizmetler**: Service Fabric Hizmetleri birden fazla ikincil service çoğaltma oluşturarak hızlı yük devretme sağlar. Bir düğüm, işlem veya bireysel hizmet donanım veya diğer hatası nedeniyle kullanılamaz hale gelirse ikincil çoğaltmaları biri en az olarak hizmet kaybı ile bir birincil çoğaltmaya yükseltilir.
* **Ölçeklenebilir Hizmetler**: tek tek Hizmetleri bölümlenmiş olması, küme üzerinde ölçeğinin genişletilip durumu için izin verme. Ayrıca, tek tek Hizmetleri oluşturulabilir ve anında kaldırıldı. Hizmetleri hızlı ve kolay bir şekilde kullanıma birkaç düğüm üzerinde birkaç örneklerinden birçok düğümlerde örnekleri binlerce ölçeklendirilmiş ve ardından, yeniden kaynak gereksinimlerinize bağlı olarak ölçeklendirilebilir. Service Fabric, bu hizmetleri oluşturmak ve bunların tam yaşam döngüleri yönetmek için kullanabilirsiniz.
* **Statik olmayan veriler üzerinde hesaplama**: veri, derleme için giriş/çıkış ve yoğun durum bilgisi olan uygulamaların işlem Service Fabric etkinleştirir. Service Fabric uygulamalarda işleme (hesaplama) ve veri birlikte bulundurma özelliğini sağlar. Normalde, uygulamanız verilere erişim gerektirdiğinde, bir dış veri önbelleği ya da depolama katmanı ile ilişkili ağ gecikme süresi yok. Durum bilgisi olan Service Fabric Hizmetleri ile bu gecikme, daha fazla kullanıcı etkinleştirme okuyan ve yazan ortadan kalkar. Örneğin, 100 milisaniyeden gidiş dönüş süresi gereksinimi sahip müşteriler için gerçek zamanlı öneri seçimleri yakın gerçekleştiren bir uygulamaya sahip söyleyin. Service Fabric Hizmetleri (burada öneri seçimi hesaplama kuralları ve veri ile birlikte bulunan) gecikme süresi ve performans özelliklerini gerekli verileri uzaktaki depolama biriminden getirmek zorunda standart uygulama modeli ile karşılaştırıldığında kullanıcı için esnek bir deneyim sağlar.  
* **Oturum tabanlı etkileşimli uygulamalar**: Service Fabric, çevrimiçi oyun veya anlık ileti, gibi uygulamalarınız düşük gecikme süresi okuma ve yazma işlemleri gerektiriyorsa yararlıdır. Service Fabric ayrı depolama veya durum bilgisi olmayan uygulamalar için gerektiği gibi önbellek oluşturmak zorunda kalmadan bu etkileşimli, durum bilgisi olan uygulamalar oluşturmanıza olanak sağlar. (Bu gecikme artar ve büyük olasılıkla tutarlılık sorunları tanıtır.).
* **Veri analizi ve iş akışları**: Hızlı Okuma ve yazma işlemleri Service Fabric, olayları veya veri akışları güvenilir bir şekilde işlemelidir uygulamaları etkinleştirir. Service Fabric ayrıca işleme ardışık düzenleri, güvenilir ve sonraki işleme aşamaya kaybı olmadan açın geçirilen sonuçları burada olmalıdır açıklamak uygulamaları sağlar. Bu, veri tutarlılığı ve hesaplama garantileri gerekli olduğu, işlem ve finansal sistemleri içerir.
* **Veri toplama, işleme ve IOT**: Service Fabric büyük ölçekli işleme ve düşük gecikme süresi, durum bilgisi olan hizmetlere sahip olduğundan, cihaz ve hesaplama için veri birlikte bulunduğu milyonlarca cihaza üzerindeki veri işleme için idealdir.
Service Fabric dahil olmak üzere kullanarak IOT sistemleri yerleşik birkaç müşteriler anlatıldığı [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider elektrik](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) ve [kafes sistemleri](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Uygulama tasarım örnek olay incelemeleri
Service Fabric uygulamaları tasarlamak için nasıl kullanılacağını gösteren örnek çalışmalar sayısı üzerinde yayımlanan [Service Fabric ekip blogu](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) ve [mikro çözümleri site](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Uygulamaları tasarlama durum bilgisiz ve durum bilgisi olan mikro oluşur
Azure bulut hizmeti çalışan rolleri ile uygulamaları oluşturmak, bir durum bilgisi olmayan hizmetin örneğidir. Buna karşılık, durum bilgisi olan mikro yetkili durumlarına istek ve yanıt dışında tutun. Bu, yüksek kullanılabilirlik ve çoğaltma tarafından yedeklenen işlem garanti sağlayan basit API'ler aracılığıyla durumu tutarlılığı sağlar. Service Fabric'ın durum bilgisi olan hizmetler yüksek kullanılabilirlik, uygulamalar, yalnızca veritabanları ve diğer veri depolarına tüm türleri için getiren democratize. Doğal progression budur. Uygulamaları NoSQL veritabanları için yüksek kullanılabilirlik için tamamen ilişkisel veritabanları kullanarak zaten taşınmış. Artık uygulamaların kendileri "etkin" durumu ve bunların içinde güvenilirlik, tutarlılık veya kullanılabilirlikten ödün vermeden ek performans artışları için yönetilen veri olabilir.

Mikro oluşan uygulamaları oluştururken, durum bilgisiz web uygulamaları (ASP.NET, Node.js, vb.) bileşimini genellikle sahip durum bilgisiz ve durum bilgisi olan iş orta katman Hizmetleri'ni çağrılırken, tüm Service Fabric dağıtım komutlarını kullanarak aynı Service Fabric kümesi dağıtılmış. Bu hizmetlerin her birini, Ölçek, güvenilirlik ve kaynak kullanımı, büyük ölçüde geliştirme ve yaşam döngüsü Yönetimi'nde çeviklik geliştirme açısından bağımsızdır.

Ek kuyruklar ve tamamen durum bilgisiz uygulamaların kullanılabilirlik ve gecikme süresi gereksinimlerini karşılayacak şekilde geleneksel gerekli sahip önbellekleri gereksinimini kaldırmak için durum bilgisi olan mikro uygulama tasarımları basitleştirin. Durum bilgisi olan hizmetler doğal olarak yüksek oranda kullanılabilir ve düşük gecikme süresi olduğundan, uygulamanızı bir bütün olarak yönetmek için daha az hareketli parça olduğunu gösterir. Aşağıdaki diyagramlar tasarlama ve durum bilgisi olan bir durum bilgisi olmayan bir uygulama arasındaki farklar gösterilmektedir. Yararlanarak [Reliable Services](service-fabric-reliable-services-introduction.md) ve [Reliable Actors](service-fabric-reliable-actors-introduction.md) modellerini programlama, durum bilgisi olan hizmetler uygulama karmaşıklığını yüksek verimlilik ve düşük gecikme süresi elde etmeye devam ederken azaltın.

## <a name="an-application-built-using-stateless-services"></a>Durum bilgisi olmayan hizmetler kullanılarak oluşturulmuş bir uygulama
![Durum bilgisiz hizmetini kullanarak uygulama][Image1]

## <a name="an-application-built-using-stateful-services"></a>Durum bilgisi olan hizmetleri kullanılarak oluşturulan uygulama
![Durum bilgisiz hizmetini kullanarak uygulama][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

* Dinlemek [müşteri örnek olay incelemeleri](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=qDJnf86yC_5206218965
)
* Hakkında bilgi edinin [müşteri örnek olay incelemeleri](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* Daha fazla bilgi edinmek [modelleri ve senaryoları](service-fabric-patterns-and-scenarios.md)

* Service Fabric ile durum bilgisiz ve durum bilgisi olan hizmetler oluşturmaya başlamak [güvenilir hizmetler](service-fabric-reliable-services-quick-start.md) ve [güvenilir aktörler](service-fabric-reliable-actors-get-started.md) modellerini programlama.
* Ayrıca aşağıdaki konulara bakın:
  * [Mikro hizmetler hakkında bilgi ver](service-fabric-overview-microservices.md)
  * [Hizmetinin durumunu yönetin](service-fabric-concepts-state.md)
  * [Service Fabric hizmetlerin kullanılabilirliğini](service-fabric-availability-services.md)
  * [Ölçek Service Fabric Hizmetleri](service-fabric-concepts-scalability.md)
  * [Bölüm Service Fabric Hizmetleri](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
