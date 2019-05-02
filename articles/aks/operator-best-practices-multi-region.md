---
title: İşleç en iyi uygulamalar - yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure Kubernetes Hizmetleri (AKS)
description: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma durumlar Azure Kubernetes Hizmetleri (AKS) için hazırlamak, uygulamalarınız için en fazla çalışma süresi için küme işleci en iyi uygulamaları öğrenin
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 5cac42505cd015cb018664b765e88f40667b1759
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920458"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>İş sürekliliği ve olağanüstü durum kurtarma Azure Kubernetes Service (AKS) için en iyi uygulamalar

Azure Kubernetes Service (AKS) kümeleri yönetme gibi uygulama çalışma süresini önemli hale gelir. AKS, bir kullanılabilirlik kümesindeki birden çok düğüm kullanarak yüksek kullanılabilirlik sağlar. Bu birden çok düğüm bir bölge arızasına karşı korumak yok. Uygulamanızın çalışma süresini en üst düzeye çıkarmak için bazı iş sürekliliği ve olağanüstü durum kurtarma özelliklerinin uygulayın.

İş sürekliliği ve olağanüstü durum kurtarma aks'deki makale yardımcı konularına odaklanır. Bu en iyi planlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Birden çok bölgede AKS kümeleri planlama
> * Azure Traffic Manager ile birden fazla küme arasındaki trafiği yönlendirme
> * Kapsayıcı görüntü kayıt defterleri için coğrafi çoğaltma kullanma
> * Uygulama durumu arasında birden çok kümeleri planlama
> * Birden çok bölgede depolama çoğaltma

## <a name="plan-for-multi-region-deployment"></a>Çok bölgeli dağıtımını planlama

**En iyi uygulama kılavuzunu** - birden çok AKS kümesi dağıtırken AKS kullanılabildiği bölgeleri seçin ve eşleştirilmiş bölgelerin kullanın.

Bir AKS kümesi, tek bir bölgeye dağıtılır. Kendiniz bölge arızasına karşı korumak için farklı bölgeler arasında birden çok AKS kümesi uygulamanıza dağıtın. Hangi bölgeler, AKS kümesi dağıtmayı planlarken aşağıdaki maddeler geçerlidir:

* [AKS bölge kullanılabilirliği](https://docs.microsoft.com/azure/aks/quotas-skus-regions#region-availability)
  * Kullanıcılarınıza yakın bir bölge seçin. AKS, yeni bölgelere sürekli genişliyor.
* [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
  * Bulunduğunuz coğrafi bölge için birbirleri ile eşleştirilmiş iki bölgeleri seçin. Bu bölgeler platformu güncelleştirmeleri koordine etmek ve kurtarma çalışmalarınızı öncelik gerektiğinde.
* Kullanılabilirlik hizmet düzeyine (sık erişimli ve sık sık erişimli/yarı etkin, sıcak/soğuk)
  * Tek bir bölge ile aynı anda iki bölgeleri çalıştırmak istiyor musunuz *hazır* trafiği veya zaman trafiğe hizmet verir hazır hale getirmek için gereken tek bir bölge sunan başlatmak için.

AKS bölge kullanılabilirliği ve eşleştirilmiş bölgelerin birleşik göz önünde bulundurarak var. Bölge olağanüstü durum kurtarma birlikte yönetmek için tasarlanan eşleştirilmiş bölgeler, AKS kümeye dağıtın. Örneğin, AKS kullanılabilir *Doğu ABD* ve *Batı ABD*. Bu bölgeler de eşleştirilmiş. Bu iki bölgeleri, bir AKS BC/DR stratejisi oluştururken önerilen.

Uygulamanızı dağıttığınızda, bu birden çok AKS kümeye dağıtmak için CI/CD ardışık düzeninizi için de başka bir adım eklemeniz gerekir. Dağıtım işlem hatlarınızı güncelleştirmezseniz, uygulama dağıtımları yalnızca birine bölgeler ve AKS küme dağıtılabilir. Bir ikincil bölgeye yönlendirilir müşteri trafiğini en son kod güncelleştirmeleri almaz.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Azure Traffic Manager trafiği yönlendirmek için kullanın.

**En iyi uygulama kılavuzunu** -Azure Traffic Manager, müşteriler kendi yakın AKS kümesi ve uygulama örneği için doğrudan. En iyi performansı ve yedeklilik için AKS kümenizi geçmeden önce tüm uygulama trafik Traffic Manager aracılığıyla doğrudan.

Farklı bölgelerdeki birden fazla AKS kümeleriyle trafiği için her kümede çalışan uygulamaları nasıl Yönlendirilme biçimini denetlemek gerekir. [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) bölgeler arasında ağ trafiğini dağıtmak bir DNS tabanlı trafik yük dengeleyicidir. Küme yanıt saatini temel alan ya da geography üzerinde temel kullanıcı yönlendirebilirsiniz.

![Azure Traffic Manager ile AKS](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Tek bir AKS kümesi ile bağlanmak genellikle müşterilerin *hizmet IP* veya DNS adı verilen bir uygulama. Bir çoklu küme dağıtımında, müşterilerin her AKS kümesinde hizmetlere işaret eden bir Traffic Manager DNS adı bağlanmanız gerekir. Bu hizmetler, Traffic Manager uç noktaları kullanılarak tanımlanır. Her uç nokta *hizmeti yük dengeleyici IP*. Bu yapılandırma, farklı bir bölgede uç bir bölgede Traffic Manager uç noktasından ağ trafiğini doğrudan sağlar.

![Azure Traffic Manager ile coğrafi yönlendirme](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager DNS araması gerçekleştirmek ve bir kullanıcı için en uygun uç noktaya döndürmek için kullanılır. Birincil bir konum için verilen önceliğe sahip iç içe profiller kullanılabilir. Örneğin, bir kullanıcı için en yakın coğrafi bölgede öncelikli olarak bağlanmanız gerekir. Bu bölge, bir sorun varsa, Traffic Manager bunları bunun yerine bir ikincil bölgeye yönlendirir. Bu yaklaşım, bunların en yakın coğrafi bölgede kullanılamaz durumda olsa bile müşterilerin her zaman bir uygulama örneği bağlanabildiğinden emin yapar.

Bu uç noktaları ayarlama ve yönlendirmeyi, bkz. adımları için [Traffic Manager'ı kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door"></a>7. Katman Azure ön kapısı ile uygulama yönlendirme

Azure Traffic Manager DNS (Katman 3), Şekil trafiği kullanır. [Azure ön kapısı](https://docs.microsoft.com/azure/frontdoor/front-door-overview) HTTP/HTTPS (katman 7) yönlendirme seçeneği sunar. SSL sonlandırma, özel etki alanı, Web uygulaması güvenlik duvarı, URL yeniden yazma ve oturum benzeşimi ön kapısı ek özellikleri içerir.

Hangi çözümünün en uygun olduğunu anlamak için uygulama trafiğinizi gereksinimlerini gözden geçirin.

## <a name="enable-geo-replication-for-container-images"></a>Kapsayıcı görüntüleri için coğrafi çoğaltmayı etkinleştirin

**En iyi uygulama kılavuzunu** -kapsayıcı görüntülerinizi Azure Container Registry ve kayıt defteri her bir AKS bölgeye coğrafi olarak çoğaltma Store.

Dağıtıp uygulamalarınızı, AKS içinde çalıştırmak için depolamak ve kapsayıcı görüntüleri çekmek için bir yol gerekir. Azure Container Registry (ACR), kapsayıcı görüntülerini veya Helm grafikleri güvenli bir şekilde depolamak için AKS ile tümleştirebilirsiniz. ACR çok yöneticili coğrafi otomatik olarak dünyanın dört bir yanındaki Azure bölgeleri için kendi görüntülerinizi çoğaltma çoğaltmayı destekler. ACR coğrafi çoğaltma performansı ve kullanılabilirliği geliştirmek için bir AKS kümesi bulunduğu her bölgede bir kayıt defteri oluşturmak için kullanın. Her bir AKS kümesi, ardından aynı bölgede yerel ACR kayıt defterinden kapsayıcı görüntülerini çeker:

![Azure Container Registry coğrafi çoğaltma için kapsayıcı görüntüleri](media/operator-best-practices-bc-dr/acr-geo-replication.png)

ACR coğrafi çoğaltma kullanmanın avantajları şunlardır:

* **Aynı bölgeden görüntüleri çekiliyor daha hızlıdır.** Ağ bağlantıları aynı Azure bölgesinde yüksek hızlı, düşük gecikme süreli görüntüleri çekin.
* **Aynı bölgeden görüntüleri çekiliyor daha güvenilir oldu.** Bir bölge kullanılamıyorsa, AKS kümenizi kullanılabilir kalan bir farklı ACR kayıt defterinden görüntü çeker.
* **Aynı bölgeden görüntüleri çekiliyor ucuz.** Veri Merkezi arasında ağ çıkışı ücret alınmaz.

Coğrafi çoğaltma özelliğidir *Premium* SKU ACR kayıt defterleri. Çoğaltma yapılandırma adımları için bkz: [Azure Container Registry coğrafi çoğaltma](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

## <a name="remove-service-state-from-inside-containers"></a>Hizmeti Kaldır kapsayıcıların içinde durum

**En iyi uygulama kılavuzunu** - mümkün olduğunda, hizmet durumunu kapsayıcı içinde depolamayın. Bunun yerine, çok bölgeli çoğaltma destekleyen Azure PaaS Hizmetleri kullanın.

Hizmet durumu işlevi için bir hizmet gerektiren, bellek veya disk üzerindeki verileri ifade eder. Durum Hizmeti okuyan ve yazan üye değişkenleri ve veri yapıları içerir. Hizmet nasıl geliştirilmiştir bağlı olarak, dosyaları ve diskte depolanan diğer kaynakları da durumu içerebilir. Örneğin, dosyalar, bir veritabanı veri ve işlem günlüklerini depolamak için kullanırsınız.

Durum te dış veya durumu düzenleme koduyla birlikte. Externalization durumu genellikle bir veritabanı veya ağ üzerinden veya aynı makinede işlemi dışında farklı makinelerde çalışan başka bir veri deposunda kullanılarak gerçekleştirilir.

Kapsayıcılar ve mikro hizmet içinde çalıştırılan işlemlerin durumu korumaz, en esnek olan. Uygulamalarınıza bazı durumları neredeyse her zaman içermesi gibi bir platformu Azure veritabanı gibi bir hizmet çözümü olarak Azure SQL veya MySQL/Postgres kullanın.

Hakkında ayrıntılı bilgi için nasıl daha taşınabilir uygulamalar oluşturmak için aşağıdakilere bakın:

* [On iki Faktörlü uygulama metodolojiyi](https://12factor.net/).
* [Bir web uygulaması Azure birden çok bölgede çalıştırın](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Depolama geçiş planı oluşturma

**En iyi uygulama kılavuzunu** - Azure Depolama'yı kullanırsanız, hazırlama ve test depolama alanınızı birincil sunucudan yedekleme bölgesine geçirme.

Uygulamalarınızı kendi veri için Azure depolama kullanabilir. Uygulamalarınızı birden çok AKS kümesi farklı bölgelerde arasında yayılır gibi eşitlenen depolama tutmanız gerekir. Depolama çoğaltma, iki genel yolu aşağıdaki yaklaşımlardan şunlardır:

* Uygulama tabanlı zaman uyumsuz çoğaltma
* Zaman uyumsuz çoğaltma alt yapısı tabanlı

### <a name="infrastructure-based-asynchronous-replication"></a>Zaman uyumsuz çoğaltma alt yapısı tabanlı

Bir pod bile silindikten sonra uygulamalarınızın kalıcı depolama alanı gerektirebilir. Kubernetes'te, kalıcı birimleri, veri depolama kalıcı hale getirmek için kullanabilirsiniz. Bu kalıcı birimler düğümü VM'sine bağlanmış ve ardından pod'ların kullanıma sunulan. Pod aynı küme içinde farklı bir düğüme taşınır bile pod'ları, kalıcı birimleri izleyin.

Depolama çözümü kullanımı bağlı olarak, çoğaltma stratejileri farklı olabilir. Gibi genel depolama çözümleri [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [CEPH](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [Kale](https://rook.io/docs/rook/master/disaster-recovery.html), ve [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) tüm kendi yönergelerimiz vardır.

Merkezi bir yaklaşım, uygulamaların verilerini yazmak ortak bir depolama noktasıdır. Bu veriler daha sonra bölgede çoğaltılan ve yerel olarak erişilebilir.

![Zaman uyumsuz çoğaltma alt yapısı tabanlı](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Azure yönetilen diskler kullanıyorsanız, aşağıdaki yaklaşımlardan birini kullanarak kullanılabilir çoğaltma ve DR çözümleri şunlardır:

* [Azure'da ark](https://github.com/heptio/ark/blob/master/docs/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Uygulama tabanlı zaman uyumsuz çoğaltma

Şu anda Kubernetes yerel-tabanlı zaman uyumsuz çoğaltma uygulaması. Kapsayıcılar ve Kubernetes gevşek yapısı ile geleneksel uygulama veya dil yaklaşım çalışması gerekir. Merkezi bir yaklaşım sonra her kümenin yazılır depolama istekleri çoğaltmak uygulamaların kendileri için veri depolama arka plandaki olduğu.

![Uygulama tabanlı zaman uyumsuz çoğaltma](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, iş sürekliliği ve olağanüstü durum kurtarma değerlendirmeleri AKS kümelerinde odaklı. AKS kümesi işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi bakın:

* [Çok kiracılılık ve küme ayırma][aks-best-practices-cluster-isolation]
* [Temel Kubernetes Zamanlayıcı Özellikleri][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
