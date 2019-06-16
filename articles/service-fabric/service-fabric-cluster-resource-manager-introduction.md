---
title: Service Fabric Küme Kaynak Yöneticisi ile tanışın | Microsoft Docs
description: Giriş için Service Fabric Küme Kaynak Yöneticisi.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e9b1cc8b66be36a0a77118f4de672c9411433ba5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60743674"
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Service Fabric Küme Kaynak Yöneticisi ile tanışın
Geleneksel BT sistemleri veya çevrimiçi hizmetleri yönetmek için kullanılan belirli bir fiziksel veya sanal makineler bu belirli hizmetler veya sistemleri ayrılması geliyordu. Hizmet katmanları desteklemesi için. Bir "web" katmanı ve bir "veri" veya "alanı" katmanı olacaktır. Uygulamaları nerede istekleri giriş ve çıkış önbelleğe alma için adanmış makineler kümesi yanı sıra aktarılan bir Mesajlaşma katmanına sahip olması gerekir. Her bir katmanı veya iş yükü türü için ayrılmış belirli makinelere vardı: veritabanı, birkaç web sunucuları dedicated birkaç makineler alındı. Belirli türde bir iş yükü için olan makineleri neden olursa söz konusu katman için daha fazla makine, aynı yapılandırmaya sahip eklemek çok sık erişimli çalıştırın. Ancak, tüm iş yükleri şekilde kolayca ölçeği - özellikle veri katmanı ile genellikle daha büyük makinelere makinelerle değiştirirsiniz. Kolay. Bir makine başarısız olursa, genel uygulama bu bölümü makinesi geri yüklenemedi kadar alt kapasitede çalıştı. Yine de oldukça kolay (değil gerekmeyen eğlenceli varsa).

Artık ancak hizmet ve yazılım mimarisi dünyasına değişti. Uygulamaları bir ölçek genişletme tasarım benimseyen daha yaygındır. Kapsayıcıları ve mikro hizmet uygulamaları (veya her ikisi de) derleme yaygındır. Yalnızca birkaç makineler hala olabilir, ancak artık, ister bir iş yükü yalnızca tek bir örneği çalışıyor değil. Bunlar bile birden çok farklı iş yüklerini aynı anda çalışıyor olabilir. Artık düzinelerce farklı türlerdeki Hizmetleri (hiçbiri tam bir makinenin gereken kaynakları kullanan), belki de farklı örnekleri hizmetlerin yüzlerce sahipsiniz. Her adlandırılmış örnek bir veya daha fazla örneği veya çoğaltmalar yüksek kullanılabilirlik (HA için) sahiptir. Bu iş yüklerini ve ne kadar meşgul olduklarını boyutunu bağlı olarak, kendinizi yüzlerce veya binlerce bulabilirsiniz. 

Ortamınızı aniden yönetme birkaç makine iş yükleri tek tür için adanmış yönetme olarak bu kadar basit değil. Sunucularınızın sanal ve artık adlara sahip (gelen mindsets geçtiniz [Evcil Hayvanlar cattle için](https://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) tüm). Yapılandırma hakkında makineleri ve Hizmetleri hakkında daha fazla küçük. Bir iş yükünün tek örnekli bir adanmış donanım büyük ölçüde şey of geçmiş olur. Hizmetleri kendilerini ticari donanım, birden çok daha küçük parçalara span küçük bir dağıtılmış sistemler haline geldi.

Uygulamanız artık hamleye olanak çeşitli katmanlarda yayılmış bir dizi olduğundan, artık çok daha fazla birleşimleri uğraşmanız sahipsiniz. Kimin karar ne tür iş yüklerinin hangi donanımda çalıştırabilirsiniz veya kaç? Hangi iş yüklerini de aynı donanımda çalışır ve hangi çakışan? Bir makine aşağı bunu nasıl geçtiğinde ne var. Bu makinede çalışan biliyor musunuz? Bu iş yükü sağlamaktan sorumlu kim yeniden çalışmaya başlar? (Sanal?) makine döndürülmesini bekleyin ya da iş yüklerinizi otomatik olarak diğer makineler ve canlı çalıştırma için yük devretme? Kullanıcı müdahalesi gerekli mi? Bu ortamda yükseltmeleri hakkında neler diyeceksiniz?

Geliştiriciler ve bu ortamda ilgilenme işleçleri Bu karmaşıklığı yönetme hakkında Yardım istediğiniz oluşturacağız. İşe alma binge ve karmaşıklığı kişilerle gizlemek çalışırken büyük olasılıkla doğru yanıt, bu nedenle ne yapmak istersiniz?

## <a name="introducing-orchestrators"></a>Düzenleyicileri ile tanışın
"Orchestrator", bu tür ortamlarda yönetmesine yardımcı olan bir yazılım parçasıdır için genel bir terimdir. Düzenleyiciler "my ortamında çalışan bu hizmet beş kopyası istiyorum."gibi isteklerde ele bileşenlerdir İstenen durum ne olursa olsun, aynı ortam yapmak deneyin.

Düzenleyiciler (insanlar değil), eylem başarısız bir makine ya da bir iş yükü beklenmeyen bir nedenden dolayı sonlandırır yapın ' dir. Çoğu düzenleyicileri, daha fazlasını hatayla ilgilidir. Sahip oldukları diğer özelliklere, yükseltmeler işlemek ve kaynak tüketimi ve yönetim ile ilgili yeni dağıtımlar yönetiyorsunuz. Bazı istenen durum yapılandırması ortamda bakımı hakkında tüm düzenleyicilerle temelde olan. Bir orchestrator ne isterseniz ve bu kaynaklanan ağır yüklerden yapmak mümkün olmasını istediğiniz. Aurora Mesos ve Docker Datacenter/Docker Swarm, Kubernetes ve Service Fabric üzerinde düzenleyicileri tüm örnekleri mevcuttur. Bu düzenleyicileri, üretim ortamlarında gerçek iş yüklerinin gereksinimlerini karşılamak için etkin bir şekilde Geliştiriliyor. 

## <a name="orchestration-as-a-service"></a>Hizmet olarak düzenleme
Küme Kaynak Yöneticisi düzenleme Service fabric'te işleyen sistem bileşendir. Küme Kaynak Yöneticisi'nin iş üç bölümlere ayrılmıştır:

1. Kuralları zorlama
2. Ortamınızı en iyi duruma getirme
3. Diğer işlemlerle yardımcı olma

### <a name="what-it-isnt"></a>Ne desteklenmez
Geleneksel N katmanlı uygulamalarda olduğu her zaman bir [yük dengeleyici](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Bu genellikle bir ağ yük dengeleyicisi (NLB) veya bir uygulama yük dengeleyici (burada, ağ yığınını CTS bağlı olarak ALB) oluştu. Bazı yük Dengeleyiciler F5 Bigıp teklifi gibi donanım tabanlı, diğerleri ise, yazılım tabanlı gibi Microsoft NLB. Diğer ortamlarda, bir şey HAProxy, ngınx, Istio veya Envoy gibi bu rolde görebilirsiniz. Bu mimaride, durum bilgisiz iş yükleri (yaklaşık) aynı miktarda iş aldığınızdan emin olmak için Yük Dengeleme işi silinir. Dengeleme stratejileri, çeşitli yükler. Bazı Dengeleyiciler farklı bir sunucuya farklı her çağrının gönderir. Diğer oturum sabitleme/sürekliliği sağlanır. Daha gelişmiş Dengeleyiciler, beklenen maliyet ve geçerli makine yükü temel alarak bir çağrı yönlendirmek için gerçek yükü tahmini veya Raporlama kullanın.

Web/çalışan katmanı kabaca dengeli kaldığını emin olmak ağ Dengeleyiciler ya da ileti yönlendiriciler çalıştı. Veri katmanı Dengeleme stratejileri farklı ve kullanıcıda üzerinde veri depolama mekanizmasıdır. Veri katmanı Dengeleme önbelleğe alma, yönetilen görünümleri, saklı yordamlar ve diğer depolama özgü mekanizmaları veri parçalama üzerinde yararlandı.

Bu stratejiler bazı ilginç olsa da, Service Fabric Küme Kaynak Yöneticisi herhangi bir ağ yük dengeleyicisi veya bir önbellek gibi değil. Bir ağ yük dengeleyicisi, ön uçlar arasında trafik yayarak dengeler. Service Fabric Küme Kaynak Yöneticisi, farklı bir strateji vardır. Temelde, Service Fabric taşır *Hizmetleri* nerede bunlar en çok trafiği bekleniyor uygun veya izlemek için yükleme. Örneğin, hizmetleri kadar işi vardır Hizmetleri yapmamanın olduğundan şu anda soğuk düğümlere taşıyabilirsiniz. Düğümleri mevcut hizmetlere silindi veya başka bir yere taşıdı soğuk olabilir. Başka bir örnek olarak, Küme Kaynak Yöneticisi ayrıca uzaktaki bir makine bir hizmetin taşıyabilirsiniz. Belki de yaklaşık yükseltilmek üzere makinedir veya tüketim ani bir değişiklik nedeniyle üzerinde çalışan hizmetleri tarafından aşırı yüklendi. Alternatif olarak, hizmetin kaynak gereksinimlerini artırmış olabilir. Sonuç olarak çalıştırmaya devam etmek için bu makinede yeterli kaynak yok. 

Küme Kaynak Yöneticisi geçici bir çözüm bulmak services taşımak için sorumlu olduğundan, bir ağ yük dengeleyicisi ne bulacağından için karşılaştırıldığında farklı özellik kümesi içerir. Bu konum hizmeti çalıştırmak için ideal olsa bile Ağ Yük Dengeleyiciler ağ trafiğini Hizmetleri zaten olduğu için teslim olmasıdır. Service Fabric Küme Kaynak Yöneticisi kümedeki kaynaklar verimli bir şekilde kullanılan sağlamaya yönelik tamamen farklı stratejiler kullanır.

## <a name="next-steps"></a>Sonraki adımlar
- Mimari ve bilgi akışını içinde Küme Kaynak Yöneticisi hakkında daha fazla bilgi için [bu makalede](service-fabric-cluster-resource-manager-architecture.md)
- Küme Kaynak Yöneticisi kümesi tanımlamak için birçok seçenek vardır. Ölçümler hakkında daha fazla bilgi için bu makalede atın [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Kullanım ve kapasite kümedeki Service Fabric Küme Kaynak Yöneticisi'ni nasıl yönettiğini ölçümleridir. Ölçümler ve bunları kullanıma yapılandırma hakkında daha fazla bilgi edinmek için [bu makalede](service-fabric-cluster-resource-manager-metrics.md)
- Küme Kaynak Yöneticisi, Service Fabric'in yönetimi özellikleri ile çalışır. Bu tümleştirme hakkında daha fazla bilgi için okumaya devam [bu makalede](service-fabric-cluster-resource-manager-management-integration.md)
- Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi almak için makalesine göz atın [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
