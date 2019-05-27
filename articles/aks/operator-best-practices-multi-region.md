---
title: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure Kubernetes Service (AKS)
description: Yüksek düzeyde kullanılabilirlik sağladığınızdan ve olağanüstü durum kurtarma Azure Kubernetes Service (AKS) için hazırlama uygulamalarınız için en fazla çalışma süresi elde etmek için bir küme İşlecin en iyi uygulamaları öğrenin.
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 93af2e4c373701383a674c694f7799ba890414dd
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65887431"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>İş sürekliliği ve olağanüstü durum kurtarma Azure Kubernetes Service (AKS) için en iyi uygulamalar

Azure Kubernetes Service (AKS) kümeleri yönetme gibi uygulama çalışma süresini önemli hale gelir. AKS, bir kullanılabilirlik kümesindeki birden çok düğüm kullanarak yüksek kullanılabilirlik sağlar. Ancak, bu birden çok düğüm sisteminizi bölge arızasına karşı koruma. Uygulamanızın çalışma süresini en üst düzeye çıkarmak için korumak iş sürekliliği ve olağanüstü durum kurtarmasına hazırlanmak için önceden planlayın.

Bu makalede, iş sürekliliği ve olağanüstü durum kurtarma aks'deki planlama hakkında odaklanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * AKS kümeleri birden fazla bölgede planlayın.
> * Azure Traffic Manager'ı kullanarak birden fazla küme arasındaki trafiği yönlendirme.
> * Coğrafi çoğaltma, kapsayıcı görüntüsünü kayıt defterleri için kullanın.
> * Uygulama durumu için birden çok kümelerinde planlayın.
> * Depolama, birden çok bölgede çoğaltın.

## <a name="plan-for-multiregion-deployment"></a>Çok bölgeli dağıtımını planlama

**En iyi yöntem**: Birden çok AKS kümesi dağıtırken, AKS kullanılabildiği bölgeleri seçin ve eşleştirilmiş bölgelerin kullanın.

Bir AKS kümesi, tek bir bölgeye dağıtılır. Bölge arızasına karşı sisteminizi korumak için farklı bölgeler arasında birden çok AKS kümesi uygulamanıza dağıtın. AKS kümenizin dağıtılacağı yeri planlarken göz önünde bulundurun:

* [**AKS bölge kullanılabilirliği**](https://docs.microsoft.com/azure/aks/quotas-skus-regions#region-availability): Kullanıcılarınıza yakın bir bölge seçin. AKS, yeni bölgelere sürekli olarak genişletir.
* [**Azure eşleştirilmiş bölgeleri**](https://docs.microsoft.com/azure/best-practices-availability-paired-regions): Bulunduğunuz coğrafi bölge için birbirleri ile eşleştirilmiş iki bölgeleri seçin. Eşleştirilmiş bölgeler platformu güncelleştirmeleri koordine etmek ve kurtarma çalışmalarınızı öncelik gerektiğinde.
* **Hizmet kullanılabilirliği**: Sık erişimli ve seyrek, sık erişimli/sıcak veya sıcak/soğuk, eşleştirilmiş bölgelerin gerekip gerekmediğini belirleyin. Tek bir bölge ile aynı anda iki bölgeleri çalıştırmak istiyor musunuz *hazır* trafik başlatmak için? Veya zamanınız trafiğe hizmet verir hazır hale getirmek için tek bir bölge istiyor musunuz?

AKS bölge kullanılabilirliği ve eşleştirilmiş bölgelerin bir birleşik göz önünde bulundurarak var. Bölge olağanüstü durum kurtarma birlikte yönetmek için tasarlanan eşleştirilmiş bölgeler, AKS kümeye dağıtın. Örneğin, AKS Doğu ABD ve Batı ABD bölgelerinde kullanılabilir. Bu bölgeler ile eşlenirler. Bir AKS BC/DR stratejisi oluştururken bu iki bölgeleri seçin.

Başka bir adım, uygulamanızı dağıtırken bu birden çok AKS kümeye dağıtmak için CI/CD ardışık düzeninizi ekleyin. Dağıtım işlem hatlarınızı güncelleştirmezseniz, uygulamalar tek bölgeler ve AKS küme dağıtılmış olabilir. Bir ikincil bölgeye yönlendirilir müşteri trafiğini en son kod güncelleştirmeleri almaz.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Azure Traffic Manager trafiği yönlendirmek için kullanın.

**En iyi yöntem**: Azure Traffic Manager, müşteriler kendi yakın AKS kümesi ve uygulama örneği için yönlendirebilir. AKS kümenizi geçmeden önce en iyi performans ve yedeklilik için tüm uygulama trafik Traffic Manager aracılığıyla doğrudan.

Farklı bölgelerde birden fazla AKS kümesi varsa, her kümede çalışan uygulamaları için trafiğin nasıl akacağını denetlemek için Traffic Manager'ı kullanın. [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) bölgeler arasında ağ trafiğini dağıtmak bir DNS tabanlı trafik yük dengeleyicidir. Traffic Manager yönlendirme kullanıcılara kullanın küme yanıt saatini temel alan veya üzerinde coğrafi konum tabanlı.

![Traffic Manager ile AKS](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Genellikle tek bir AKS kümesi olan müşteriler, hizmet IP veya DNS adı verilen bir uygulama için bağlanın. Multicluster bir dağıtımda, müşterilerin her AKS kümesinde hizmetlere işaret eden bir Traffic Manager DNS adı bağlanmanız gerekir. Bu hizmetler, Traffic Manager uç noktaları kullanarak tanımlayın. Her uç nokta *yük dengeleyici IP hizmet*. Farklı bir bölgede uç bir bölgede Traffic Manager uç noktasından ağ trafiğini yönlendirmek için bu yapılandırmayı kullanın.

![Coğrafi trafik Yöneticisi aracılığıyla yönlendirme](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager DNS araması gerçekleştirir ve bir kullanıcının en uygun uç noktaya döndürür. İç içe geçmiş profilleri, bir birincil konum öncelik sırasına koyabilirsiniz. Örneğin, kullanıcılar için kendi en yakın coğrafi bölgede genel olarak bağlanmanız gerekir. Traffic Manager, bunun yerine bu bölge, bir sorun varsa, kullanıcıların ikincil bir bölgeye yönlendirir. Bu yaklaşım, müşterilerin kendi en yakın coğrafi bölgede kullanılamaz durumda olsa bile bir uygulama örneği bağlanabilir sağlar.

Uç noktalar ve yönlendirme ayarlama hakkında daha fazla bilgi için bkz: [Traffic Manager'ı kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door-service"></a>7. Katman Azure ön kapısı hizmetle uygulama yönlendirme

Traffic Manager DNS (Katman 3) şekli trafiği kullanır. [Azure ön kapısı hizmet](https://docs.microsoft.com/azure/frontdoor/front-door-overview) HTTP/HTTPS (katman 7) yönlendirme seçeneği sunar. SSL sonlandırma, özel etki alanı, web uygulaması güvenlik duvarı, URL yeniden yazma ve oturum benzeşimi Azure ön kapısı hizmetinin ek özellikler. Hangi çözümünün en uygun olduğunu anlamak için uygulama trafiğinizi gereksinimlerini gözden geçirin.

## <a name="enable-geo-replication-for-container-images"></a>Kapsayıcı görüntüleri için coğrafi çoğaltmayı etkinleştirin

**En iyi yöntem**: Azure Container Registry ve coğrafi çoğaltma her bir AKS bölgeye kayıt defterine kapsayıcı görüntülerinizi Store.

Dağıtıp uygulamalarınızı, AKS içinde çalıştırmak için depolamak ve kapsayıcı görüntüleri çekmek için bir yol gerekir. Kapsayıcı kayıt defteri, kapsayıcı görüntülerini veya Helm grafikleri güvenli bir şekilde depolayabilirsiniz şekilde AKS ile tümleşir. Kapsayıcı kayıt defteri otomatik olarak dünyanın dört bir yanındaki Azure bölgeleri için kendi görüntülerinizi çoğaltma iletimiyle coğrafi-çoğaltmayı destekler. 

Container Registry coğrafi çoğaltma performansı ve kullanılabilirliği geliştirmek için bir AKS kümesi bulunduğu her bölgede bir kayıt defteri oluşturmak için kullanın. Her bir AKS kümesi, ardından aynı bölgede yerel bir kapsayıcı kayıt defterinden kapsayıcı görüntülerini çeker:

![Container Registry coğrafi çoğaltma için kapsayıcı görüntüleri](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Container Registry coğrafi çoğaltma çekme görüntülere aynı bölgeden kullandığınızda karşılaşılır:

* **Daha hızlı**: Aynı Azure bölgesinde yüksek hızlı, düşük gecikmeli ağ bağlantıları görüntüleri çekin.
* **Daha güvenilir**: Bir bölge kullanılamıyorsa, AKS kümenizin bir kullanıma açık kapsayıcı kayıt defterinden görüntüleri çeker.
* **Ucuz**: Veri Merkezi arasında ağ çıkışı ücret alınmaz.

Coğrafi çoğaltma özelliğidir *Premium* SKU kapsayıcı kayıt defterleri. Coğrafi çoğaltmayı yapılandırma hakkında daha fazla bilgi için bkz: [Container Registry coğrafi çoğaltma](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).

## <a name="remove-service-state-from-inside-containers"></a>Hizmeti Kaldır kapsayıcıların içinde durum

**En iyi yöntem**: Mümkün olduğunda, hizmet durumunu kapsayıcı içinde depolamayın. Bunun yerine, bir Azure, çok bölgeli çoğaltma destekleyen bir hizmet olarak Platformu (PaaS) kullanın.

*Hizmet durumu* işlevi için bir hizmet gerektiren, bellek veya disk üzerindeki verileri ifade eder. Durum Hizmeti okuyan ve yazan üye değişkenleri ve veri yapıları içerir. Nasıl geliştirilmiştir bağlı olarak, hizmet durumu dosyalara veya diske depolanmış diğer kaynaklara de bulunabilir. Örneğin, durum veri ve işlem günlüklerini depolamak için bir veritabanı kullanır dosyaları içerebilir.

Durum te dış veya durumu işleyen kod ile birlikte. Genellikle, bir veritabanı veya ağ üzerinden farklı makinelerde çalışan veya işlem dışında aynı makinede çalışan başka bir veri deposunda kullanarak durumu federasyonda.

Kapsayıcılar ve mikro hizmet içinde çalıştırılan işlemlerin durumu korumaz, en esnek olan. Uygulamaları neredeyse her zaman, bazı durum içerdiğinden bir PaaS çözümü gibi Azure veritabanı, MySQL ve Azure veritabanı için PostgreSQL veya Azure SQL veritabanı için kullanın.

Taşınabilir uygulamalar oluşturmak için aşağıdakilere bakın:

* [12 Faktörlü uygulama yöntemi](https://12factor.net/)
* [Birden çok Azure bölgesinde bir web uygulamasını çalıştırma](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Depolama geçiş planı oluşturma

**En iyi yöntem**: Azure depolama hizmetini kullanıyorsanız, hazırlama ve depolama yedekleme bölgeye birincil bölgeden geçirme test edin.

Uygulamalarınızı kendi veri için Azure depolama kullanabilir. Farklı bölgelerdeki birden çok AKS kümesi genelinde uygulamalarınızın yayıldığından, eşitlenen depolama tutmanız gerekir. Depolama çoğaltma için iki ortak yollar şunlardır:

* Zaman uyumsuz çoğaltma alt yapısı tabanlı
* Uygulama tabanlı zaman uyumsuz çoğaltma

### <a name="infrastructure-based-asynchronous-replication"></a>Zaman uyumsuz çoğaltma alt yapısı tabanlı

Uygulamalarınızı, bir pod bile silindikten sonra kalıcı depolama alanı gerektirebilir. Kubernetes'te, kalıcı birimleri, veri depolama kalıcı hale getirmek için kullanabilirsiniz. Kalıcı birimleri bir düğümü VM'sine bağlanmış ve ardından pod'ların kullanıma sunulan. Pod'ların aynı küme içinde farklı bir düğüme taşınır bile kalıcı birimler pod'ların izleyin.

Kullandığınız çoğaltma stratejinizi depolama çözümünüzü bağlıdır. Gibi genel depolama çözümleri [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [Ceph](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [Kale](https://rook.io/docs/rook/master/disaster-recovery.html), ve [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) kendi olağanüstü durum kurtarma hakkında rehberlik ve Çoğaltma.

Uygulamalar, veri nerede yazabilirsiniz ortak bir depolama noktası tipik strateji sağlanmasıdır. Bu veriler daha sonra bölgede çoğaltılan ve yerel olarak erişilebilir.

![Zaman uyumsuz çoğaltma alt yapısı tabanlı](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Azure yönetilen diskler kullanıyorsanız, çoğaltma ve DR çözümler şunlar gibi seçebilirsiniz:

* [Azure'da ark](https://github.com/heptio/ark/blob/master/docs/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Uygulama tabanlı zaman uyumsuz çoğaltma

Kubernetes, şu anda yerel-tabanlı zaman uyumsuz çoğaltma uygulaması sağlar. Kapsayıcılar ve Kubernetes birbirine sıkı şekilde bağlı için geleneksel uygulama veya dil yaklaşım çalışması gerekir. Genellikle, uygulamalar, ardından her kümenin temel alınan veri depolama alanına yazılır depolama isteklerin çoğaltın.

![Uygulama tabanlı zaman uyumsuz çoğaltma](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Sonraki adımlar

İş sürekliliği ve olağanüstü durum kurtarma değerlendirmeleri AKS kümeleri için bu makalede ele alınmaktadır. AKS kümesi işlemleri hakkında daha fazla bilgi için en iyi uygulamalar hakkında şu makalelere bakın:

* [Çok kiracılı mimari ve küme ayırma][aks-best-practices-cluster-isolation]
* [Temel Kubernetes Zamanlayıcı Özellikleri][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
