---
title: "Bulut Hizmetleri ve Service Fabric arasındaki farklar | Microsoft Docs"
description: "Geçiş için kavramsal genel bulut hizmetlerinden Service Fabric uygulamaları."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 26c0256f6fa299551d92e9bcd058ca359d8c85b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Bulut Hizmetleri ve Service Fabric arasındaki farklar hakkında bilgi geçirmeden önce uygulamaları.
Microsoft Azure Service Fabric yüksek düzeyde ölçeklenebilir, yüksek oranda güvenilir dağıtılmış uygulamalar için yeni nesil bulut uygulama platformudur. Paketleme, dağıtma, yükseltme ve dağıtılmış bulut uygulamalarını yönetmek için birçok yeni özellik sunar. 

Bu geçiş için bir giriş kılavuzdur bulut hizmetlerinden Service Fabric uygulamaları. Öncelikle mimari odaklanır ve bulut Hizmetleri ve Service Fabric arasındaki farklar tasarlayın.

## <a name="applications-and-infrastructure"></a>Uygulamalar ve altyapı
Bulut Hizmetleri ve Service Fabric arasındaki temel fark, VM'ler, iş yüklerini ve uygulamaları arasındaki ilişkidir. Buraya bir iş yükü, belirli bir görevi gerçekleştirmek ya da bir hizmet sağlamak için yazdığınız kodu tanımlanır.

* **Bulut Hizmetleri ' dir. vm'lerle uygulamaları dağıtma hakkında** Yazdığınız kodları Web veya çalışan rolü gibi bir VM örneğine sıkı şekilde bağlı. Bulut Hizmetleri iş yükünü dağıtmak için iş yükünü çalıştırmak bir veya daha fazla VM örnekleri dağıtmaktır. Uygulamalar ve sanal makineleri hiçbir ayrımı yoktur ve bu nedenle yoktur uygulamanın resmi tanımı yok. Bir uygulama, ardından bir bulut Hizmetleri dağıtımı içindeki Web veya çalışan rolü örnekleri bir dizi veya tam bir bulut Hizmetleri dağıtımını olarak düşünülebilir. Bu örnekte, bir uygulama rolü örnekleri bir küme olarak gösterilir.

![Bulut Hizmetleri uygulamalar ve topolojisi][1]

* **Service Fabric, var olan sanal makineleri veya Service Fabric Windows veya Linux çalıştıran makineler uygulamaları dağıtma hakkında ' dir.** Yazdığınız tamamen ayrılmış bir uygulama için birden çok ortamları dağıtılabilmesi amacıyla Service Fabric uygulama platformu tarafından hemen soyutlanır temel altyapısından hizmetleridir. Service Fabric iş yükünü bir "hizmet" adı verilir ve bir veya daha fazla hizmet Service Fabric uygulaması platformunda çalışan resmi olarak tanımlanan bir uygulama içinde gruplandırılır. Birden çok uygulama için tek bir Service Fabric kümesi dağıtılabilir.

![Service Fabric uygulamaları ve topolojisi][2]

Bulut Hizmetleri ile bağlı iş yüklerini Azure yönetilen sanal makineleri dağıtmak için bir sistem iken Service Fabric kendisi Windows veya Linux çalıştıran bir uygulama platformu katmanıdır.
Service Fabric uygulama modeli, çok sayıda avantaj vardır:

* Hızlı Dağıtım zamanları. VM örnekleri oluşturmak zaman alabilir. Service Fabric içinde sanal makineleri yalnızca Service Fabric uygulaması platformu barındıran bir küme oluşturmak için bir kez dağıtılır. Bu noktadan itibaren kümeye çok hızlı bir şekilde uygulama paketleri dağıtılabilir.
* Yüksek yoğunlukta barındırma. Bulut Hizmetleri'nde bir iş yükü çalışan rolü VM barındırır. Service Fabric içinde çok sayıda büyük dağıtımlar için genel maliyeti düşürebilirsiniz VM'ler az sayıda uygulamaları dağıtabileceğiniz anlamına gelir, çalışan sanal makineleri ayrı uygulamalardır.
* Azure veya şirket içi olup Windows Server veya Linux makineler platform herhangi bir yerden çalıştırabilirsiniz Service Fabric sahiptir. Uygulamanızın üzerinde farklı ortamlarda çalıştırabilmeniz için platform altyapının bir soyutlama katmanı sağlar. 
* Dağıtılmış uygulama yönetimi. Service Fabric yalnızca ana dağıtılmış uygulamalar aynı yaşam döngüleri barındırma VM bağımsız olarak veya makine yaşam döngüsü de yönetmeye yardımcı olur platformudur.

## <a name="application-architecture"></a>Uygulama mimarisi
Bulut Hizmetleri Uygulama Mimarisi genellikle Service Bus, Azure Table ve Blob Storage, SQL, Redis ve diğerleri gibi durum ve verileri bir uygulama ve Web arasındaki iletişimi yönetmek için çok sayıda dış Hizmet bağımlılıklarını içerir ve Bulut Hizmetleri dağıtımında çalışan rolleri. Tam bir bulut Hizmetleri uygulamanın örnek şuna benzeyebilir:  

![Bulut Hizmetleri mimarisi][9]

Service Fabric uygulamaları, aynı dış hizmetler tam bir uygulamada kullanmak de seçebilirsiniz. Bu örnek bulut Hizmetleri mimarisi kullanarak, en basit geçiş yolunu bulut hizmetlerinden Service Fabric yalnızca bulut Hizmetleri dağıtımı genel mimarisi aynı kalmasını bir Service Fabric uygulaması ile değiştirmektir. Web ve çalışan rolleri çok az kod değişikliklerle Service Fabric durum bilgisi olmayan hizmetler için bağlantı noktası kurulmuş.

![Basit geçişten sonra Service Fabric mimarisi][10]

Bu aşamada, sistem bir önceki ile aynı çalışmaya devam etmesi gerekir. Durum bilgisi olan uygunsa Hizmetleri gibi Service Fabric'ın durum bilgisi olan özellikleri, dış durumu depoları yararlanarak internalized. Bu dış hizmetler önce yaptığınız gibi uygulamanız için eşdeğer işlevsellik sağlayan özel hizmetler yazma gerektirdiğinden daha basit bir Web ve çalışan rolleri geçişini Service Fabric durum bilgisi olmayan hizmetler için daha karmaşıktır. Bunun yapılması avantajları şunlardır: 

* Dış bağımlılıkları kaldırma 
* Dağıtım, yönetim ve yükseltme modeli birleştirin. 

Bu hizmetler internalizing bir örneği elde edilen mimarisi şöyle:

![Tam geçişten sonra Service Fabric mimarisi][11]

## <a name="communication-and-workflow"></a>İletişim ve iş akışı
Çoğu bulut hizmeti uygulamaları birden fazla katmanı oluşur. Benzer şekilde, Service Fabric uygulaması birden fazla hizmeti (genellikle birçok Hizmetleri) oluşur. İki ortak iletişim modelleri olan doğrudan iletişim ve dış dayanıklı bir depolama aracılığıyla.

### <a name="direct-communication"></a>Doğrudan iletişim
İle doğrudan iletişim, katmanları doğrudan her katmanı tarafından kullanıma sunulan bitiş noktası ile iletişim kurabilir. Bulut Hizmetleri, bu rastgele bir VM rol örneği ya da seçerek anlamına gelir veya hepsini Bakiye yükü ve kendi uç noktasına doğrudan bağlanma gibi durum bilgisiz ortamlarda.

![Bulut Hizmetleri doğrudan iletişim][5]

 Doğrudan iletişim Service Fabric ortak bir iletişim modelidir. Service Fabric bir hizmete bağlanmak ancak Service Fabric ve Cloud Services arasındaki temel farklılık bir VM'ye bağlanın, içinde bulut Hizmetleri ' dir. Bu, birkaç nedenlerle önemli bir fark oluşur:

* Service Fabric Hizmetleri'nde kendilerini barındıran sanal makineleri bağlı değildir; Hizmetleri kümeye taşıyabilirsiniz ve çeşitli nedenlerle hareket etmek için aslında, beklenen: kaynak Dengeleme, yük devretme, uygulama ve altyapı yükseltmeleri ve yerleştirme ya da yük kısıtlamaları. Başka bir deyişle, bir hizmet örneğinin Adres dilediğiniz zaman değiştirebilirsiniz. 
* Service Fabric bir VM'de her benzersiz uç noktaları birden çok hizmet barındırabilir.

Service Fabric adlandırma bitiş noktası adreslerini Hizmetleri çözümlemek için kullanılan hizmet adlı bir hizmet bulma mekanizma sağlar. 

![Service Fabric doğrudan iletişim][6]

### <a name="queues"></a>Kuyruklar
Bulut Hizmetleri gibi durum bilgisiz ortamlarda katmanları arasında ortak bir iletişim mekanizması işlemi başka bir iş görevleri bir katmanından depolamak için bir dış depolama kuyruğu kullanmaktır. Bir Azure kuyruk veya hizmet burada çalışan rolü örnekleri dequeue ve işleri işlemek veri yolu işleri gönderir web katmanı buna ortak bir senaryodur.

![Bulut Hizmetleri sıra iletişimi][7]

Aynı iletişim modelini Service Fabric içinde kullanılabilir. Service Fabric varolan bir bulut Hizmetleri uygulamaya geçirirken bu yararlı olabilir. 

![Service Fabric doğrudan iletişim][8]

## <a name="next-steps"></a>Sonraki Adımlar
En basit geçiş yolunu bulut hizmetlerinden Service Fabric uygulamanızı genel mimarisi kabaca aynı kalmasını bir Service Fabric uygulaması ile yalnızca bulut Hizmetleri dağıtımı değiştirmektir. Aşağıdaki makalede bir Web veya çalışan rolü bir Service Fabric durum bilgisiz hizmetine dönüştürme yardımcı olacak bir kılavuz sağlar.

* [Basit geçiş: bir Service Fabric durum bilgisiz hizmetine Web veya çalışan rolü Dönüştür](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

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
