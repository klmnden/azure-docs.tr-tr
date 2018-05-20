---
title: Service Fabric kümesi Kaynak Yöneticisi giriş | Microsoft Docs
description: Giriş için Service Fabric kümesi Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f25a422385abfcdb7020eb7477c0ae2ee55cd8fb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Service Fabric kümesi Kaynak Yöneticisi'ni Tanıtımı
Geleneksel BT sistemleri veya çevrimiçi hizmetleri yönetmek için kullanılan belirli bir fiziksel veya sanal makineleri bu belirli hizmetleri veya sistemleri ayrılması anlamına gelir. Hizmetleri katmanları tasarlanmış. "Web" bir katman ve "verileri" veya "depolama" katman olacaktır. Uygulamaları burada istekleri giriş ve çıkış önbelleğe alma için ayrılmış makineler kümesi yanı sıra aktarılan bir Mesajlaşma katmanına sahip olması gerekir. Her katman veya iş yükü türünü belirli makineler için ayrılmış vardı: veritabanı, web sunucuları az bir adanmış birkaç makineler aldı. Belirli türde bir iş yükü için olan makineler neden olursa o aynı yapılandırmaya sahip daha fazla makine bu katmana eklenen sonra çok sıcak çalıştırın. Ancak, tüm iş yüklerinin çıkışı kolay ölçeklendirilmesi - özellikle veri katmanı ile genellikle daha büyük makineler makinelerle değiştirirsiniz. Kolay. Bir makine başarısız olursa, genel uygulama kısmı makine geri kadar düşük kapasitede verdi. Hala oldukça kolay (değil mutlaka eğlenceli değilse).

Artık ancak hizmet ve yazılım mimarisi world değiştirilmiştir. Uygulamaları ölçeklendirme tasarım benimseyen daha yaygın bir durumdur. Kapsayıcılar veya mikro uygulamalarla (veya her ikisi de) oluşturma yaygındır. Yalnızca birkaç makineler hala olabilir, ancak artık, bir iş yükü yalnızca tek bir örneğini çalıştırdıkları değil. Bunlar bile birden çok farklı iş yükleri aynı anda çalışıyor olabilir. Artık farklı türlerdeki Hizmetleri (none kaynaklar arasında tam makinenin eşitleyeceğini tüketen) düzinelerce belki farklı örnekleri hizmetlerin yüzlerce sahipsiniz. Her adlandırılmış örnek yüksek kullanılabilirlik (HA için) bir veya daha fazla örneği veya çoğaltmaları sahiptir. Bu iş yüklerini ve ne kadar meşgul olduklarına boyutlarına bağlı olarak, kendiniz yüzlerce veya binlerce makinelerin ile karşılaşabilirsiniz. 

Aniden ortamınızı yönetmek için tek tür iş yüklerinin ayrılmış birkaç makineler yönetilmesi kadar basit değil. Sunucularınızın sanal ve artık adlara sahip (mindsets gelen geçtiyseniz [Evcil Hayvanlar cattle için](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) tüm). Yapılandırma hakkında makineler ve hizmetler hakkında daha fazla küçük. İş yükü tek örnekli bir adanmış donanım büyük ölçüde şey of Geçmiş ' dir. Hizmetleri kendilerini, ticari donanım birden çok daha küçük parçalarını span küçük dağıtılmış sistemlerin hale getirildi.

Uygulamanız artık monoliths birkaç katmanları yayılan bir dizi olduğundan, artık uğraşmanız pek çok daha fazla birleşimleri sahipsiniz. Kimin karar ne tür iş yüklerinin hangi donanımda çalıştırabilirsiniz veya kaç tane? Hangi iş yüklerini de aynı donanımda çalışır ve hangi çakışan? Bir makinede aşağı nasıl yapılacağı gittiğinde ne var. Bu makinede çalışan biliyor? İş yükü sağlamaktan sorumlu kim yeniden çalışmaya başladıktan? Geri dönmeniz için (sanal?) makine bekleyin veya iş yüklerinizi otomatik olarak diğer makineler ve çalışan Canlı yük devredecek? İnsan eli gerekli mi? Bu ortamdaki yükseltmeler nasıldır?

Geliştiriciler ve bu ortamda ilgilenme işleçleri bu karmaşıklık yönetme hakkında Yardım istediğiniz yapacağız. İşe alma binge ve karmaşıklık kişilerle Gizle çalışılırken büyük olasılıkla sağ yanıt, bu nedenle ne yapmak istersiniz?

## <a name="introducing-orchestrators"></a>Orchestrators Tanıtımı
Bir "Orchestrator" Bu tür ortamlarda yönetmesine yardımcı olan yazılım parçası için genel bir terimdir. Orchestrators "Benim ortamında çalışan bu hizmeti beş kopyası istiyorum."gibi isteklerinde ele bileşenlerdir Neler olsun belirtilen istenen duruma eşleşen ortam yapmak deneyin.

Orchestrators (değil insanlar) ne bir makine hata verdiğinde veya beklenmeyen bir nedenden dolayı bir iş yükü sonlandırır eylemde ' dir. Çoğu orchestrators daha fazlasını hata ile ilgilidir. Sahip oldukları diğer özellikleri yükseltmeler işleme ve kaynak tüketimini ve idare ilgilenme yeni dağıtımlar, yönettiğiniz. Tüm orchestrators temelde ortamında yapılandırmasında istenen bazı durumunu korumak hakkında var. Bir orchestrator ne isterseniz ve sahip ağır lifting yapmak bildirmek kullanabilmek ister. Aurora Mesos, veri merkezi/Docker Docker Swarm, Kubernetes ve Service Fabric üstünde orchestrators, tüm örnekler verilmiştir. Bu orchestrators, üretim ortamında gerçek iş yüklerinin gereksinimlerini karşılamak için etkin olarak geliştirilmektedir. 

## <a name="orchestration-as-a-service"></a>Bir hizmet olarak düzenleme
Küme Kaynak Yöneticisi'ni Service Fabric düzenleme işleyen sistem bileşenidir. Küme Resource Manager'ın iş üç bölüme ayrılmıştır:

1. Kural zorlama
2. Ortamınıza en iyi duruma getirme
3. Diğer işlemlerle yardımcı olma

Küme Kaynak Yöneticisi'ni nasıl çalıştığını görmek için aşağıdaki Microsoft Virtual Academy videoyu izleyin: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Ne değil
Geleneksel N katmanlı uygulamalar olduğundan her zaman bir [yük dengeleyici](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Bu genellikle bir ağ yük dengeleyici (NLB) veya bir uygulama yük dengeleyici (burada ağ yığınını sat bağlı olarak ALB) oluştu. Donanım tabanlı F5'ın Bigıp sunumu gibi bazı yük Dengeleyiciler, diğerleri ise, yazılım tabanlı gibi Microsoft NLB. Diğer ortamlarda, bir şey HAProxy, nginx, Istio veya haberci gibi bu rolde görebilirsiniz. Bu mimari, Yük Dengeleme iş durum bilgisiz iş yükleri (kabaca) iş ile aynı miktarda alırsınız emin olmaktır. Dengeleme stratejileri, çeşitli yükleyin. Bazı dengeleyicileri her farklı çağrı farklı bir sunucuya gönderir. Başkalarının sabitleme oturum/sürekliliği sağlanır. Daha gelişmiş dengeleyicileri, beklenen maliyeti ve geçerli makine yükü temel alarak bir çağrı yönlendirmek için gerçek yük tahmin veya Raporlama kullanın.

Web/çalışan katmanı kabaca dengeli kalan emin olmak ağ dengeleyicileri veya ileti yönlendiricileri çalıştı. Veri katmanı Dengeleme stratejileri farklı ve veri depolama mekanizmasını bağımlı. Veri katmanı Dengeleme önbelleğe alma, yönetilen görünümleri, saklı yordamları ve diğer depolama özgü mekanizmaları veri parçalama üzerinde dayanıyordu.

Bu stratejileri bazıları ilginç olsa da, Service Fabric kümesi Kaynak Yöneticisi herhangi bir şey Ağ Yük Dengeleyici veya önbellek gibi değil. Bir ağ yük dengeleyicisi, ön uçlar arasında trafiği yayarak ön uçlar dengeler. Service Fabric kümesi Kaynak Yöneticisi, farklı bir strateji vardır. Temelde, Service Fabric taşır *Hizmetleri* burada bunlar trafiği bekleniyor en, anlamlı veya izlemek için yük. Örneğin, bu hizmetleri vardır Hizmetleri kadar iş yapmamanın olduğundan şu anda soğuk bir düğüme taşıyın. Mevcut Hizmetleri silinmiş veya başka bir yere taşınmış bu yana düğümlerin soğuk olabilir. Başka bir örnek olarak, Küme Kaynak Yöneticisi'ni de bir hizmet bir makine çıktığınızda taşıyabilirsiniz. Belki de hakkında yükseltilmesi için makinenin olduğunu veya bir ani artış tüketimi nedeniyle üzerinde çalışan hizmetleri tarafından aşırı yüklendi. Alernatively, hizmetin kaynak gereksinimlerini artış gösterdi. Sonuç olarak çalıştırmaya devam etmek için bu makine üzerinde yeterli kaynak yok. 

Küme Kaynak Yöneticisi'ni hizmetlerin taşımak için sorumlu olduğu için bir ağ yük dengeleyicisi ne bulur için karşılaştırıldığında farklı özellik kümesi içerir. Bu konum hizmeti çalıştırmak için ideal olsa bile Ağ Yük Dengeleyici ağ trafiğini Hizmetleri zaten olduğu için teslim olmasıdır. Service Fabric kümesi Kaynak Yöneticisi kümedeki kaynaklar verimli bir şekilde kullanıldığını sağlamaya yönelik temelde farklı stratejileri kullanır.

## <a name="next-steps"></a>Sonraki adımlar
- Mimari ve bilgi akışını içinde Küme Kaynak Yöneticisi hakkında daha fazla bilgi için kullanıma [bu makalede ](service-fabric-cluster-resource-manager-architecture.md)
- Küme Kaynak Yöneticisi'ni küme açıklamak için birçok seçeneğiniz vardır. Ölçümler hakkında daha fazla bilgi için bu makalede kontrol [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Service Fabric kümesi Kaynak Yöneticisi, kullanım ve kapasite kümedeki nasıl yönettiğini ölçümleridir. Ölçümleri ve bunların kullanıma nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için [bu makalede](service-fabric-cluster-resource-manager-metrics.md)
- Küme Resource Manager ile Service Fabric'ın yönetim özelliklerini çalışır. Bu tümleştirme hakkında daha fazla bilgi için okuma [bu makalede](service-fabric-cluster-resource-manager-management-integration.md)
- Küme Kaynak Yöneticisi'ni yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için makalesine kontrol [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
