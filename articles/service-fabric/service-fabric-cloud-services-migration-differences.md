---
title: Bulut Hizmetleri ve Service Fabric arasındaki farklar | Microsoft Docs
description: Geçiş için kavramsal bir genel bulut hizmetlerinden Service Fabric uygulamaları.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 8b486e617389e1611dfebf3d347d2d64df088593
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258647"
---
# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Geçiş yapmadan önce Cloud Services ve Service Fabric arasındaki farklar hakkında bilgi edinin uygulamalar.
Microsoft Azure Service Fabric, yüksek düzeyde ölçeklenebilir, yüksek oranda güvenilir dağıtılmış uygulamalar için tasarlanan yeni nesil bulut uygulama platformudur. Bu, paketleme, dağıtma, yükseltme ve dağıtılan bulut uygulamaları yönetmek için birçok yeni özellik sunar. 

Bu geçiş için Tanıtım amaçlı bir kılavuz olan bulut hizmetlerinden Service Fabric uygulamaları. Öncelikle mimari odaklanır ve Cloud Services ve Service Fabric arasındaki farklar tasarlayın.

## <a name="applications-and-infrastructure"></a>Uygulamalar ve altyapı
Cloud Services ve Service Fabric arasındaki temel bir fark, VM'ler, iş yüklerini ve uygulamaları arasındaki ilişkidir. Buraya bir iş yükü, belirli bir görevi gerçekleştirme veya bir hizmet sağlamak için yazdığınız kodu olarak tanımlanır.

* **Cloud Services, sanal makineleri olarak uygulamaları dağıtma hakkında.** Yazdığınız kodu bir Web veya çalışan rolü gibi bir sanal makine örneğiyle sıkı şekilde bağlı. Bulut Hizmetleri iş yükünü dağıtmak için iş yükü çalıştıran bir veya daha fazla VM örneği dağıtmaktır. Uygulamalar ve VM'lerin hiçbir ayrım yoktur ve bu nedenle yoktur uygulamanın resmi tanımı yok. Bir uygulama, ardından bir bulut Hizmetleri dağıtımı içinde Web veya çalışan rolü örnekleri kümesi veya bütün bir Cloud Services dağıtım olarak düşünülebilir. Bu örnekte, bir uygulama rolü örnekleri bir dizi gösterilir.

![Cloud Services uygulamalarını ve topolojisi][1]

* **Service Fabric uygulamaları mevcut Vm'lere veya Service Fabric Windows veya Linux üzerinde çalışan makineler dağıtma hakkında ' dir.** Yazdığınız tamamen ayrılmış bir uygulama birden çok ortama dağıtılacak şekilde Service Fabric uygulama platformu tarafından soyutlanır uzakta temel altyapısından hizmetleridir. Service Fabric iş yükünü bir "hizmet" adı verilir ve bir veya daha fazla hizmet Service Fabric uygulama platformunda çalışan resmi olarak tanımlanan bir uygulama içinde gruplandırılır. Çoklu uygulamaları tek bir Service Fabric kümesine dağıtılabilir.

![Service Fabric uygulamaları ve topolojisi][2]

Cloud Services, Azure tarafından yönetilen sanal makineleri dağıtma bağlı iş yükleriyle sistemidir ise Service Fabric kendisi Windows veya Linux üzerinde çalışan bir uygulama platformu katmanıdır.
Service Fabric uygulama modelini, çok sayıda avantaj vardır:

* Hızlı Dağıtım süreleri. VM örnekleri oluşturma, zaman alıcı olabilir. Service Fabric'te bir küme oluşturmak için Service Fabric uygulama platformu sunan sonra VM'ler yalnızca dağıtılır. Bu noktadan itibaren kümeye çok hızlı bir şekilde uygulama paketleri dağıtılabilir.
* Yüksek yoğunluklu barındırma. Bulut Hizmetleri'nde çalışan rolü sanal makine bir iş yükü barındırır. Service Fabric'te, çok sayıda küçük bir sayı daha büyük dağıtımlar için genel maliyeti düşürmenizi sanal makinelerin uygulamaları dağıtabileceğiniz anlamına gelir, çalıştıran Vm'leri ayrı uygulamalardır.
* Azure veya şirket içinde olup olmadığını Windows Server veya Linux makineler, Service Fabric platform, her yerde çalıştırılabilecek sahiptir. Uygulamanızın farklı ortamlarda çalıştırabilmeniz için platform altyapının üzerinde bir soyutlama katmanı sağlar. 
* Dağıtılmış uygulama yönetimi. Service Fabric yalnızca ana dağıtılmış uygulamalar aynı barındırma VM bağımsız olarak yaşam döngüleri veya makine yaşam döngüsü de yönetilmesine yardımcı olur platformudur.

## <a name="application-architecture"></a>Uygulama mimarisi
Bir Cloud Services uygulamasının mimarisi genellikle, Service Bus, Azure tablosu ve Blob Depolama, SQL, Redis ve diğer durum ve veriler bir uygulama ve Web arasındaki iletişim, yönetilecek gibi çok sayıda dış Hizmet bağımlılıkları içerir ve Bulut hizmetlerini dağıtımında çalışan rolleri. Eksiksiz bir bulut Hizmetleri Uygulaması örneği şuna benzeyebilir:  

![Bulut Hizmetleri mimarisi][9]

Service Fabric uygulamaları, aynı dış hizmetlere tam kullanmak üzere de seçebilirsiniz. Bu örneği bulut Hizmetleri mimarisi kullanarak, en basit geçiş bulut hizmetlerinden Service fabric'e genel mimarisi aynı kalmasını bir Service Fabric uygulaması ile yalnızca bulut Hizmetleri dağıtım değiştirilecek yoludur. Web ve çalışan rolleri Service Fabric durum bilgisi olmayan hizmetler çok az kod değişikliğiyle unity'nin.

![Basit geçişten sonra Service Fabric mimarisi][10]

Bu aşamada, sistemin bir önceden olduğu gibi çalışmaya devam etmesi gerekir. Uygun olduğunda, durum bilgisi olan hizmetler, Service Fabric'in durum bilgisi olan özellikleri, dış durumlarını depoları avantajlarından yararlanmaya internalized. Bu dış hizmetler önce yaptığınız gibi uygulamanıza eşdeğer bir işlevselliği sağlayan bir özel hizmetler yazma gerektirdiğinden daha basit bir Web ve çalışan rolleri geçişini Service Fabric durum bilgisi olmayan hizmetler için daha karmaşıktır. Bunu yapmanın avantajları şunları içerir: 

* Dış bağımlılıklar kaldırılıyor 
* Dağıtım, yönetim ve yükseltme modeli altyapınızı kurun. 

Bu hizmetler internalizing, bir örneği elde edilen mimari şöyle görünebilir:

![Tam geçişten sonra Service Fabric mimarisi][11]

## <a name="communication-and-workflow"></a>İletişim ve iş akışı
Çoğu bulut hizmeti uygulamaları birden fazla katmanını oluşur. Benzer şekilde, Service Fabric uygulaması birden fazla hizmeti (genellikle birçok Hizmetleri) oluşur. İki ortak iletişim modelleridir doğrudan iletişim ve bir dış dayanıklı depolama aracılığıyla.

### <a name="direct-communication"></a>Doğrudan iletişim
İle doğrudan iletişim, katmanları doğrudan her katman tarafından kullanıma sunulan uç noktası üzerinden iletişim kurabilir. Cloud Services, bu rastgele bir VM rol örneği ya da seçmek anlamına gelir veya hepsini bir kez deneme yük dengeleme ve kendi uç noktasına doğrudan bağlanma gibi durum bilgisi olmayan ortamlarda.

![Bulut Hizmetleri doğrudan iletişim][5]

 Doğrudan iletişim, Service fabric'te yaygın bir iletişim modelidir. Service Fabric'te bir hizmete bağlanabilir ancak Service Fabric ve Cloud Services arasındaki temel fark VM'ye bağlanın, içinde bulut Hizmetleri ' dir. Bu önemli bir ayrımdır birkaç nedenleri verilmiştir:

* Service fabric'te Hizmetleri, kendilerini barındıran sanal makinelere bağlı değil; Hizmetleri kümedeki yerleri ve çeşitli nedenlerden dolayı değiştirmemiz aslında, beklenen: Kaynak Dengeleme, yük devretme, uygulama ve altyapı yükseltmeleri ve yerleştirme veya yük kısıtlamaları. Başka bir deyişle, bir hizmet örneği adresi herhangi bir zamanda değişebilir. 
* Service fabric'te bir VM, her benzersiz uç noktaları ile birden çok hizmeti barındırabilirsiniz.

Service Fabric adlandırma uç nokta adresleri Hizmetleri çözümlemek için kullanılan hizmeti adlı bir hizmet bulma mekanizması sağlar. 

![Service Fabric doğrudan iletişim][6]

### <a name="queues"></a>Kuyruklar
Bulut Hizmetleri gibi durum bilgisi olmayan ortamlarda katmanlar arasında ortak bir iletişim mekanizması, iş görevleri bir katmandan diğerine arızaya depolamak için bir dış depolama kuyruğu kullanmaktır. İşleri bir Azure kuyruğu veya Service burada çalışan rolü örnekleri sıradan çıkarma ve işleri işlemek Bus'a gönderir bir web katmanı buna yaygın bir senaryodur.

![Bulut Hizmetleri kuyruk iletişimi][7]

Service Fabric'te aynı iletişim modelini kullanılabilir. Service Fabric mevcut bir Cloud Services uygulamasına geçirirken bu yararlı olabilir. 

![Service Fabric doğrudan iletişim][8]

## <a name="parity"></a>Eşlik
[Bulut Hizmetleri, kullanım kolaylığı ve kontrol derecesi Service fabric'e benzerdir, ancak artık eski bir hizmet olduğundan ve Service Fabric yeni geliştirme projeleri için önerilen](https://docs.microsoft.com/azure/app-service/overview-compare); bir API karşılaştırması aşağıda verilmiştir:


| **Bulut hizmeti API'si** | **Service Fabric API** | **Notlar** |
| --- | --- | --- |
| RoleInstance.GetID | FabricRuntime.GetNodeContext.NodeId veya. NodeName | Kimliği bir NodeName özelliğidir. |
| RoleInstance.GetFaultDomain | FabricClient.QueryManager.GetNodeList | NodeName üzerinde filtreleme ve FD özelliğini kullanın |
| RoleInstance.GetUpgradeDomain | FabricClient.QueryManager.GetNodeList | Üzerinde NodeName filtrelemek ve yükseltme özelliğini kullanın |
| RoleInstance.GetInstanceEndpoints | FabricRuntime.GetActivationContext veya (ResolveService) adlandırma | FabricRuntime.GetActivationContext hem içinde yineleme sırasında sağlanan ServiceInitializationParameters.CodePackageActivationContext aracılığıyla sağlanan CodePackageActivationContext. Başlatma |
| RoleEnvironment.GetRoles | FabricClient.QueryManager.GetNodeList | Aynı sıralama listesini alabilirsiniz türüne göre filtreleme yapmak istiyorsanız kümeden düğüm türleri FabricClient.ClusterManager.GetClusterManifest bildirim ve buradan rol/düğüm türleri alın. |
| RoleEnvironment.GetIsAvailable | Connect WindowsFabricCluster veya belirli bir düğüme işaret eden bir FabricRuntime oluşturun | * |
| RoleEnvironment.GetLocalResource | CodePackageActivationContext.Log/Temp/Work | * |
| RoleEnvironment.GetCurrentRoleInstance | CodePackageActivationContext.Log/Temp/Work | * |
| LocalResource.GetRootPath | CodePackageActivationContext.Log/Temp/Work | * |
| Role.GetInstances | FabricClient.QueryManager.GetNodeList veya ResolveService | * |
| RoleInstanceEndpoint.GetIPEndpoint | FabricRuntime.GetActivationContext veya (ResolveService) adlandırma | * |

## <a name="next-steps"></a>Sonraki Adımlar
En basit geçiş yolunun bulut Hizmetleri'nden Service Fabric uygulamanızı genel mimarisi yaklaşık aynı kalmasını bir Service Fabric uygulaması ile yalnızca bulut Hizmetleri dağıtım değiştirmektir. Aşağıdaki makalede bir Web veya çalışan rolü bir Service Fabric durum bilgisi olmayan hizmet dönüştürme yardımcı olacak bir kılavuz sağlar.

* [Basit bir geçiş: bir Web veya çalışan rolü bir Service Fabric durum bilgisi olmayan hizmet Dönüştür](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
