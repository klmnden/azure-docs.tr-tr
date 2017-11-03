---
title: "Service Fabric Azure ile ilgili genel bakış | Microsoft Docs"
description: "Service Fabric, uygulamalar birçok mikro ölçek ve esneklik sağlamak için burada oluşan genel bakış. Service Fabric bir dağıtılmış sistemler platform ölçeklenebilir, güvenilir oluşturmak için kullanılan ve kolayca yönetilen bulut uygulamalarının ' dir."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: msfussell
ms.custom: mvc
ms.openlocfilehash: 8ff0d38a679b673b148dd808050eda82060cfe80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-azure-service-fabric"></a>Azure Service Fabric genel bakış
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric da geliştirmeye ve bulut yerel uygulamaları yönetme önemli sorunları giderir. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar. Service Fabric oluşturmak ve yönetmek bu kurumsal sınıf, Katman 1, kapsayıcılarında çalışan bulut ölçekli uygulamalar için yeni nesil platform temsil eder.

Bu kısa video Service Fabric ve mikro hizmetler sunar:<center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="applications-composed-of-microservices"></a>Uygulamalar mikro oluşur 
Service Fabric, yapı ve yüksek yoğunluklu bir küme olarak adlandırılan bir paylaşılan havuzu makine çalışan mikro oluşan ölçeklenebilir ve güvenilir uygulamaları yönetmenize olanak sağlar. Bir dağıtılmış, ölçeklenebilir, durum bilgisiz oluşturmak için Gelişmiş ve basit çalışma zamanı ve durum bilgisi olan mikro kapsayıcılarında çalıştıran sağlar. Kapsamlı uygulama yönetim özellikleri sağlamak için de sağlar, dağıtmak, izlemek, yükseltme/düzeltme eki ve dağıtılan uygulamaları kapsayıcılı Hizmetleri dahil olmak üzere silin.

Azure SQL veritabanı dahil olmak üzere birçok Microsoft hizmetlerinin Bugün, Service Fabric çalıştırır, Azure Hizmetleri Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IOT Hub, Dynamics 365, Skype Kurumsal ve birçok çekirdek.

Service Fabric gerektiği gibi küçük, başlatın ve büyük ölçekli yüzlerce veya binlerce makinelerin büyüyebileceği yerel Hizmetleri bulut oluşturmak için özel olarak oluşturulmuştur.

Bugünün Internet ölçekli hizmetler mikro oluşturulur. Protokol ağ geçitleri mikro örnekleri arasında kullanıcı profilleri, alışveriş, işleme, Envanter sıraları, sepetleri ve önbelleğe alır. Service Fabric her mikro hizmet (veya kapsayıcı) durum bilgisiz ya da durum bilgisi olan benzersiz bir ad veren mikro bir platformdur.

Service Fabric oluşan bu mikro uygulamaları için kapsamlı çalışma zamanı ve yaşam döngüsü yönetimi özelliklerini sağlar. Mikro dağıtılan ve Service Fabric kümesi arasında etkinleştirilen kapsayıcılar içinde barındırır. Sanal makinelerden taşıma kapsayıcılara büyüklük sipariş artışı yoğunluğu içinde mümkün kılar. Benzer şekilde, bu kapsayıcılardaki mikro kapsayıcılardan taşıdığınızda başka bir büyüklük yoğunluğu de mümkün olur. Örneğin, Azure SQL veritabanı için tek bir küme on binlerce kapsayıcıları barındıran yüz binlerce veritabanları toplam çalışan makineler yüzlerce oluşur. Her bir Service Fabric durum bilgisi olan mikro hizmet veritabanıdır. 

Mikro yaklaşımı hakkında daha fazla bilgi için okuma [uygulamaları oluşturmak için bir mikro yaklaşım neden?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Kapsayıcı dağıtım ve düzenleme
Service Fabric olan Microsoft'un [kapsayıcı orchestrator](service-fabric-cluster-resource-manager-introduction.md) mikro makine bir kümede dağıtma. Mikro kullanarak birçok yolla geliştirilebilir [Service Fabric modellerini programlama](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), dağıtımı [tercih ettiğiniz herhangi bir kod](service-fabric-deploy-existing-app.md). Önemlisi, hem işlemlerde Hizmetleri ve Hizmetleri aynı uygulamada kapsayıcılardaki karıştırabilirsiniz. Yalnızca istiyorsanız, [dağıtmak ve kapsayıcıları yönetmek](service-fabric-containers-overview.md), Service Fabric kapsayıcı orchestrator olarak mükemmel bir seçim değil.

## <a name="any-os-any-cloud"></a>Herhangi bir işletim sistemi, herhangi bir bulut
Service Fabric her yerde çalışır. Azure dahil olmak üzere birçok ortamda veya şirket içinde Windows Server veya Linux için Service Fabric kümeleri oluşturabilirsiniz. Hatta diğer ortak bulutlarda kümeleri oluşturabilirsiniz. Ayrıca, SDK geliştirme ortamında olduğu **aynı** üretim ortamıyla ilgili hiçbir Öykünücüler için. Diğer bir deyişle, diğer ortamlarda kümelerine ne yerel geliştirme kümenizde çalışan dağıtır.

![Service Fabric platformundan][Image1]

Windows geliştirme için Service Fabric .NET SDK, Visual Studio ve Powershell ile tümleşiktir. Bkz: [Windows geliştirme ortamınızı hazırlama](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md). Linux geliştirmesi için Service Fabric Java SDK Eclipse ile tümleştirilmiştir ve Yeoman Java, .NET Core ve kapsayıcı uygulamaları için şablonları oluşturmak için kullanılır. Bkz: [Linux üzerinde geliştirme ortamınızı hazırlama](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md)

Kümeleri oluşturma hakkında daha fazla bilgi için okuma [Windows Server veya Linux üzerinde bir küme oluşturma](service-fabric-deploy-anywhere.md) veya küme oluşturma Azure [Azure Portalı aracılığıyla](service-fabric-cluster-creation-via-portal.md).

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Durum bilgisiz ve durum bilgisi olan mikro Service Fabric için
Service Fabric mikro veya kapsayıcıları oluşması uygulamalar oluşturmanıza olanak sağlar. Durum bilgisiz mikro (örneğin, protokol ağ geçitleri ve web proxy) dışında bir istek ve yanıt hizmetinden değişebilir bir duruma bulundurmayan. Azure bulut Hizmetleri çalışan rolleri, bir durum bilgisi olmayan hizmetin bir örnektir. Durum bilgisi olan mikro (örneğin, kullanıcı hesapları, veritabanları, aygıtlar, Alışveriş sepetleri ve Kuyruklar) istek ve yanıt ötesinde değişebilir, yetkili bir durumu korurlar. Bugünün Internet ölçekli uygulamalar durum bilgisiz ve durum bilgisi olan mikro birleşiminden oluşur. 

Service Fabric ile önemli bir ayrım ile ya da durum bilgisi olan hizmetler oluşturma, güçlü odaklanmasına olan [yerleşik programlama modelleri ](service-fabric-choose-framework.md) veya kapsayıcılı durum bilgisi olan hizmetler ile. [Uygulama senaryoları](service-fabric-application-scenarios.md) durum bilgisi olan hizmetler kullanıldığı senaryolar açıklanmaktadır.


## <a name="application-lifecycle-management"></a>Uygulama yaşam döngüsü yönetimi
Service Fabric tam uygulama yaşam döngüsü ve CI/CD kapsayıcıları dahil olmak üzere bulut uygulamaları için destek sağlar. Bu yaşam döngüsünün geliştirme aşamasından dağıtım, günlük yönetim ve nihai yetkisini bakım içerir.

Service Fabric uygulama yaşam döngüsü yönetimi özellikleri uygulama yöneticileri ve BT işleçleri sağlamak, dağıtma, düzeltme eki için basit ve düşük dokunma iş akışları kullanmayı etkinleştirin ve uygulamalarını izleyin. Bu yerleşik iş akışları büyük ölçüde sürekli olarak kullanılabilir uygulamaları korumak için BT işleçleri üzerindeki yükü azaltın.

Uygulamaların çoğu durum bilgisiz ve durum bilgisi olan mikro, kapsayıcıları ve birlikte dağıtılan diğer yürütülebilir bir birleşiminden oluşur. Service Fabric uygulamaları güçlü türleri sağlayarak, birden fazla uygulama örneğinin dağıtımını sağlar. Her örnek yönetilen ve bağımsız olarak yükseltme. Önemlisi, Service Fabric kapsayıcıları veya tüm yürütülebilir dosyaları dağıtmak ve bu güvenilir hale getirebilirsiniz. Örneğin, Service Fabric .NET, ASP.NET Core, node.js, Windows kapsayıcıları, Linux kapsayıcıları, Java sanal makineler, komut dosyaları, Angular veya gerçek anlamda her şeyi uygulamanızı yapan dağıtabilirsiniz.

Service Fabric tümleşik CI/CD araçlarıyla gibi [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html), ve [Octopus dağıtmak](https://octopus.com/) ve herhangi diğer popüler CI/CD aracı ile kullanılabilir.

Uygulama yaşam döngüsü yönetimi hakkında daha fazla bilgi için okuma [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md). Herhangi bir kod dağıtma hakkında daha fazla bilgi için bkz: [Konuk yürütülebilir dağıtmak](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>Temel işlevler
Service Fabric kullanarak şunları yapabilirsiniz:

* Azure veya sıfır ile kod değişikliklerini Windows veya Linux çalıştıran şirket içi veri merkezleri dağıtın. Bir kez yazma ve herhangi bir yerden herhangi bir Service Fabric küme dağıtma.
* Programlama modelleri, kapsayıcıları veya herhangi bir kod Service Fabric kullanarak oluşan ölçeklenebilir mikro uygulamaları geliştirin.
* Yüksek oranda güvenilir durum bilgisiz ve durum bilgisi olan mikro geliştirin. Durum bilgisi olan mikro kullanarak uygulamanızı tasarımını basitleştirin. 
* Bulut nesneleri self yer alan kodu ve durum oluşturmak için yeni Reliable Actors programlama modelini kullanın.
* Dağıtma ve Windows kapsayıcıları ve Linux kapsayıcıları dahil kapsayıcıları düzenlemek. Service Fabric veri kullanan, durum bilgisi kapsayıcı orchestrator ' dir.
* Saniye cinsinden yüksek yoğunluklu yüzlerce veya binlerce uygulamalar veya makine başına kapsayıcılar olan uygulamalar dağıtın.
* Aynı uygulama yan yana farklı sürümlerini dağıtmak ve her uygulama bağımsız olarak yükseltin.
* Yaşam döngüsü sonu ve bölünemez yükseltmeleri dahil olmak üzere herhangi bir kapalı kalma süresi olmadan, uygulamalarınızı yönetin.
* Ölçeği genişletme veya ölçeklendirmek bir küme içindeki düğümlerin sayısı. Düğümleri ölçeklendirirken uygulamalarınızı otomatik olarak ölçeklendirin.
* İzleme ve uygulamalarınızı durumunu tanılama ve otomatik onarım gerçekleştirmek için ilkeler ayarlama.
* Gözcü kaynak dengeleyicisi, küme üzerinde uygulamaların dağıtılması yönetirler. Service Fabric hatalarından kurtarır ve kullanılabilir kaynaklara bağlı yük dağıtımını en iyi duruma getirir.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi için:
  * [Neden bir mikro yaklaşımını uygulamaları oluşturmak için?](service-fabric-overview-microservices.md)
  * [Terminolojisi genel bakış](service-fabric-technical-overview.md)
* Ayarlama, [Windows geliştirme ortamı](service-fabric-get-started.md)  
* Ayarlama, [Linux geliştirme ortamı](service-fabric-get-started-linux.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
