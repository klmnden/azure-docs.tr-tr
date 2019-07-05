---
title: Kavramlar - ağ Azure Kubernetes hizmeti (AKS)
description: Azure Kubernetes Service (kubernetes ve Azure CNI ağ, giriş denetleyicileri, yük Dengeleyiciler ve statik IP adresleri de dahil olmak üzere AKS), ağ oluşturma hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: afb7acda67eb5818ace8169dc4e98fb86bdbeaa7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442012"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) uygulamaları için ağ kavramları

Uygulama geliştirme için kapsayıcı tabanlı bir mikro hizmetler yaklaşımı uygulama bileşenleri, kendi görevlerini işlemek için birlikte çalışmalıdır. Kubernetes, bu uygulamanın iletişim sağlayan çeşitli kaynaklar sağlar. Bağlanın ve uygulamaları, dahili veya harici olarak kullanıma sunar. Yüksek kullanılabilirliğe sahip uygulamalar oluşturmak için yükleyebilirsiniz uygulamalarınızı dengeleyin. SSL/TLS sonlandırılmasına veya birden çok bileşenden yönlendirme için daha karmaşık uygulamalar giriş trafik yapılandırma gerektirebilir. Güvenlik nedenleriyle, ya da pod'ların ve düğümler arasındaki ağ trafiği akışını kısıtlamak gerekebilir.

Bu makalede aks'deki uygulamalarınıza ağ sağlayan temel kavramlar açıklanmaktadır:

- [Hizmetler](#services)
- [Azure sanal ağları](#azure-virtual-networks)
- [Giriş denetleyicileri](#ingress-controllers)
- [Ağ ilkeleri](#network-policies)

## <a name="kubernetes-basics"></a>Kubernetes hakkında temel bilgiler

Uygulamalarınıza veya uygulama bileşenleri, birbirleriyle iletişim kurmak için erişime izin vermek için Kubernetes sanal ağ için bir özet düzeyi sağlar. Kubernetes düğümleri, bir sanal ağa bağlı ve pod'ları için gelen ve giden bağlantı sağlar. *Kube-proxy* bileşen bu ağ özellikleri sağlamak için her bir düğümde çalışır.

Kubernetes, *Hizmetleri* pod'ları, bir IP adresi veya DNS adı aracılığıyla ve belirli bir bağlantı noktasında doğrudan erişim sağlamak amacıyla gruplandırıp. Trafik kullanarak dağıtabileceğiniz bir *yük dengeleyici*. Daha karmaşık uygulama trafiği yönlendirme de ile gerçekleştirilebilir *giriş denetleyicileri*. Güvenlik ve pod'ların ağ trafiğini filtreleme Kubernetes ile olası *ağ ilkelerini* (önizlemede aks'deki).

Azure platformu, AKS kümeleri için sanal ağ basitleştirmek için de yardımcı olur. Kubernetes yük dengeleyici oluşturduğunuzda, temel alınan Azure yük dengeleyici kaynağı oluşturulur ve yapılandırılır. Pod'ların ağ bağlantı noktalarını açın, ilgili Azure ağ güvenlik grubu kuralları yapılandırılır. HTTP uygulama yönlendirme için Azure da yapılandırabilirsiniz *dış DNS* yollar yeni giriş yapılandırılır.

## <a name="services"></a>Hizmetler

Uygulama iş yükleri için ağ yapılandırmasını basitleştirmek için Kubernetes kullanan *Hizmetleri* mantıksal olarak gruplamak pod'ları kümesi ve ağ bağlantısı sağlamak için. Aşağıdaki hizmet türleri kullanılabilir:

- **Küme IP** -AKS kümesi içinde kullanmak için bir iç IP adresi oluşturur. Küme içindeki diğer iş yükleri destekleyen yalnızca dahili uygulamalar için uygundur.

    ![Bir AKS kümesinde Küme IP trafik akışını gösteren diyagram][aks-clusterip]

- **NodePort** -düğüm IP adresi ve bağlantı noktası ile erişilmesini sağlar temel alınan düğüme bir bağlantı noktası eşlemesi oluşturur.

    ![Bir AKS kümesinde NodePort trafik akışını gösteren diyagram][aks-nodeport]

- **LoadBalancer** - bir Azure yük dengeleyici kaynağı oluşturur, dış IP adresi yapılandırır ve istenen pod'ların yük dengeleyici arka uç havuzuna bağlanır. Müşteriler trafiğine izin vermek için erişim uygulama Yük Dengeleme kuralları oluşturulur istenen bağlantı noktalarında. 

    ![Bir AKS kümesinde yük dengeleyici trafik akışını gösteren diyagram][aks-loadbalancer]

    Ek denetimi ve gelen trafiği yönlendirme için bunun yerine kullanabilirsiniz bir [giriş denetleyicisine](#ingress-controllers).

- **ExternalName** -daha kolay uygulama erişimi için belirli bir DNS girişi oluşturur.

Yük Dengeleyiciler ve hizmetler için IP adresi dinamik olarak atanabilir ya da varolan statik IP adresini kullanmak için belirtebilirsiniz. İç ve dış statik IP adresleri atayabilirsiniz. Bu var olan bir statik IP adresi için bir DNS girişi genellikle bağlıdır.

Her ikisi de *iç* ve *dış* yük Dengeleyiciler oluşturulabilir. İç yük Dengeleyiciler yalnızca Internet'ten erişilmeyecek şekilde özel bir IP adresi atanır.

## <a name="azure-virtual-networks"></a>Azure sanal ağları

AKS, aşağıdaki iki ağ modelleri birini kullanan bir küme dağıtabilirsiniz:

- *Kubernetes* ağ - ağ kaynaklarının genellikle oluşturulup yapılandırılması AKS kümesi dağıtılır.
- *Azure kapsayıcı ağ arabirimi (CNI)* ağ - AKS kümesini mevcut sanal ağ kaynakları ve yapılandırmaların bağlı.

### <a name="kubenet-basic-networking"></a>Kubernetes (Temel) ağ

*Kubernetes* olan AKS kümesi oluşturma için varsayılan yapılandırma seçeneği ağ. İle *kubernetes*, düğümler, Azure sanal ağ alt ağından bir IP adresi alın. Pod'ların, düğümler Azure sanal ağ alt ağına mantıksal olarak farklı adres alanından bir IP adresi alır. Ağ adresi çevirisi (NAT), ardından pod'ları Azure sanal ağındaki kaynaklara erişebilmesi için yapılandırılır. Trafiği kaynak IP adresini, NAT düğümün birincil IP başvuracağını ' dir.

Düğümleri kullanma [kubernetes][kubenet] Kubernetes eklentisi. Azure platformu oluşturun ve sanal ağları yapılandırmanız veya varolan bir sanal ağ alt ağı, AKS kümesi dağıtmayı tercih izin verebilirsiniz. Yeniden yönlendirilebilir bir IP adresi yalnızca alır ve AKS küme dışındaki diğer kaynaklarla iletişim için NAT pod'ları kullanın. Bu yaklaşım ağ alanınıza kullanmak pod'ları ayırmanız gerekir IP adresi sayısını önemli ölçüde azaltır.

Daha fazla bilgi için [kubernetes AKS kümesi için ağ yapılandırma][aks-configure-kubenet-networking].

### <a name="azure-cni-advanced-networking"></a>Azure CNI (Gelişmiş) ağ

Azure CNI her pod alt ağdan bir IP adresi alır ve doğrudan erişilebilir. Bu IP adresleri, ağ alanı benzersiz olmalıdır ve önceden hazırlıklı olmak gerekir. Her düğümü destekliyorsa pod'ların sayısı için bir yapılandırma parametresi vardır. Düğüm başına IP adreslerinin sayısı, bu düğüm için önden ayrılmıştır. Bu yaklaşım daha planlama, aksi takdirde IP adresi tükenmesi veya uygulama arttıkça daha büyük bir alt ağ kümelerini yeniden oluşturmak için gereken neden olabileceğinden gerektirir.

Düğümleri kullanma [Azure kapsayıcı ağ arabirimi (CNI)][cni-networking] Kubernetes eklentisi.

![Her tek bir Azure sanal ağa bağlanma köprüleri ile iki düğümü gösteren diyagram][advanced-networking-diagram]

Daha fazla bilgi için [yapılandırın, bir AKS kümesi için Azure CNI][aks-configure-advanced-networking].

### <a name="compare-network-models"></a>Ağ modelleri karşılaştırın

Hem kubernetes hem de Azure CNI AKS kümeleriniz için ağ bağlantısı sağlar. Ancak, avantajlar ve dezavantajlar her vardır. Yüksek düzeyde, aşağıdaki maddeler geçerlidir:

* **Kubernetes**
    * IP adres alanı tasarrufu sağlar.
    * Kubernetes iç veya dış yük dengeleyici, gelen bir pod küme dışındaki erişmek için kullanır.
    * El ile yönetin ve kullanıcı tanımlı yollar (Udr) korumak gerekir.
    * 400 düğüm Küme başına en fazla.
* **Azure CNI**
    * Pod'ların tam sanal ağ bağlantısını alın ve doğrudan gelen kümesi dışında erişilebilir.
    * Daha fazla IP adres alanı gerektirir.

Kubernetes ile Azure CNI arasında davranışı aşağıdaki farklar mevcuttur:

| Özellik                                                                                   | Kubernetes   | Azure CNI |
|----------------------------------------------------------------------------------------------|-----------|-----------|
| Mevcut veya yeni bir sanal ağda küme dağıtma                                            | Desteklenen - Udr el ile uygulanan | Desteklenen |
| Pod pod bağlantısı                                                                         | Desteklenen | Desteklenen |
| Pod VM bağlantısı; Aynı sanal ağda VM                                          | Pod tarafından başlatıldığında çalışır | Her iki şekilde de çalışır |
| Pod VM bağlantısı; Eşlenen sanal ağdaki VM                                            | Pod tarafından başlatıldığında çalışır | Her iki şekilde de çalışır |
| VPN veya Express Route kullanarak şirket içi erişim                                                | Pod tarafından başlatıldığında çalışır | Her iki şekilde de çalışır |
| Hizmet uç noktaları tarafından güvenli kaynaklara erişim                                             | Desteklenen | Desteklenen |
| Bir yük dengeleyici hizmet, uygulama ağ geçidi veya giriş denetleyicisine kullanarak Kubernetes Hizmetleri kullanıma sunma | Desteklenen | Desteklenen |
| Varsayılan Azure DNS ve özel bölgeleri                                                          | Desteklenen | Desteklenen |

### <a name="support-scope-between-network-models"></a>Ağ modelleri arasında destek kapsamı

Ağ modeli kullandığınız bağımsız olarak hem kubernetes hem de Azure CNI aşağıdaki yollardan biriyle dağıtabilir:

* Azure platformu, otomatik olarak oluşturun ve bir AKS kümesi oluşturduğunuzda, sanal ağ kaynaklarını yapılandırın.
* El ile oluşturabilir ve da sanal ağ kaynaklarını yapılandırmak ve AKS kümenizi oluştururken bu kaynaklara ekleyin.

Hizmet uç noktalarına veya Udr'ler gibi özellikler hem kubernetes hem de Azure CNI ile desteklenir ancak [destek ilkeleri için AKS][support-policies] yapabilirsiniz hangi değişiklikleri tanımlayın. Örneğin:

* Bir AKS kümesi için sanal ağ kaynakları el ile oluşturursanız, kendi Udr veya hizmet uç noktaları yapılandırılırken desteklenir.
* Azure platformu AKS kümenizin bir sanal ağ kaynakları otomatik olarak oluşturursa, kendi Udr ya da hizmet uç noktaları yapılandırmak için bu AKS tarafından yönetilen kaynaklara el ile olarak değiştirmek için desteklenmiyor.

## <a name="ingress-controllers"></a>Giriş denetleyicileri

LoadBalancer türü hizmet oluşturduğunuzda, bir temel alınan Azure yük dengeleyici kaynağı oluşturulur. Yük Dengeleyici, pod'larını hizmetinizdeki belirli bir bağlantı noktasında trafiği dağıtmak için yapılandırılır. 4 - katmanında LoadBalancer yalnızca çalışır hizmet gerçek uygulamaları farkında değildir ve herhangi hususlar yönlendirme yapamazsınız.

*Giriş denetleyicileri* 7 katmanında çalışır ve daha akıllı kurallarını uygulama trafiği dağıtmak için kullanabilirsiniz. Bir ortak giriş denetleyicisine gelen URL'sini temel alarak farklı uygulamalar için HTTP trafiği yönlendirmek için kullanılır.

![Bir AKS kümesinde giriş trafik akışını gösteren diyagram][aks-ingress]

AKS, NGINX gibi kullanarak bir giriş kaynağı oluşturun veya AKS HTTP uygulama yönlendirme özelliğini kullanın. Bir AKS kümesi için uygulama HTTP yönlendirme etkinleştirdiğinizde, Azure platformu giriş denetleyicisine oluşturur ve bir *dış DNS* denetleyicisi. Yeni giriş kaynakları Kubernetes'te oluşturuldukça gerekli DNS A kayıtlarını kümeye özgü bir DNS bölgesinde oluşturulur. Daha fazla bilgi için [HTTP uygulama yönlendirme dağıtma][aks-http-routing].

Başka bir ortak giriş SSL/TLS sonlandırma özelliğidir. Giriş kaynağı yerine uygulamanın kendisinden, HTTPS erişilen büyük web uygulamaları, TLS sonlandırma işlenebilir. Otomatik TLS sertifika oluşturma ve yapılandırma sağlamak üzere, giriş kaynak sağlayıcılarını şimdi şifreleme gibi kullanacak şekilde yapılandırabilirsiniz. Şimdi şifreleme ile bir NGINX giriş denetleyicisine yapılandırma hakkında daha fazla bilgi için bkz. [giriş ve TLS][aks-ingress-tls].

Ayrıca, istemci kaynak IP AKS kümenizde kapsayıcıları isteklerinde korumak için giriş denetleyicisini yapılandırabilirsiniz. Bir istemcinin isteği bir kapsayıcıda, AKS kümesi, giriş denetleyicisine aracılığıyla yönlendirilir, bu isteğin özgün kaynak IP'sini hedef kapsayıcı için kullanılamaz. Etkinleştirdiğinizde *istemci kaynak IP korunması*, istemci için kaynak IP istek üstbilgisindeki altında kullanılabilir *X-iletilen-için*. Giriş denetleyicinizde istemci kaynak IP koruma kullanıyorsanız, SSL doğrudan kullanamazsınız. İstemci kaynak IP korunması ve SSL doğrudan kullanılabilir diğer hizmetlerle gibi *LoadBalancer* türü.

## <a name="network-security-groups"></a>Ağ güvenlik grupları

Bir ağ güvenlik grubu, AKS düğümleri gibi VM'ler için trafiği filtreler. Bir yük dengeleyici gibi hizmetleri oluştururken Azure platformu, gerekli olan tüm ağ güvenlik grubu kuralları otomatik olarak yapılandırır. El ile bir AKS kümesi pod'ların trafiğini filtrelemek için ağ güvenlik grubu kuralları yapılandırmayın. Tüm gerekli bağlantı noktaları ve iletme Kubernetes hizmeti bildirimlerinizi bir parçası olarak tanımlayın ve Azure platform oluşturma veya güncelleştirme olarak uygun kuralları sağlar. Ağ ilkeleri, sonraki bölümde açıklandığı gibi otomatik olarak pod'ların trafik filtresi kuralları uygulamak için kullanabilirsiniz.

## <a name="network-policies"></a>Ağ ilkeleri

Varsayılan olarak, bir AKS kümesindeki tüm pod'ların gönderebilir ve trafiği kısıtlama olmadan alabilirsiniz. Gelişmiş güvenlik için trafik akışını denetleyen kuralları tanımlamak isteyebilirsiniz. Arka uç uygulamaları genellikle yalnızca gerekli ön uç Hizmetleri için sunulan ve veritabanı bileşenlerini yalnızca bunlara uygulama katmanları tarafından erişilebilir.

Ağ İlkesi kullanılabilir pod'ları arasındaki trafik akışını denetlemenizi sağlayan aks'deki bir Kubernetes özelliğidir. İzin verme veya reddetme ayarları gibi atanan etiketleri, ad alanı veya trafiği, bağlantı noktası trafiğini seçebilirsiniz. Ağ güvenlik grupları değil pod'ların AKS düğümleri için daha fazla. Ağ ilkeleri kullanımı, trafik akışını denetlemek için daha uygun, bulutta yerel bir yoludur. Pod'ların bir AKS kümesinde dinamik olarak oluşturulmuş gibi gerekli ağ ilkeleri otomatik olarak uygulanabilir.

Daha fazla bilgi için [güvenli ağ ilkelerini Azure Kubernetes Service (AKS) kullanarak pod'ları arasındaki trafiği][use-network-policies].

## <a name="next-steps"></a>Sonraki adımlar

Ağ AKS ile çalışmaya başlama, oluşturun ve kendi IP adresi aralıkları kullanarak bir AKS kümesi yapılandırma [kubernetes][aks-configure-kubenet-networking] or [Azure CNI][aks-configure-advanced-networking].

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için ağ bağlantısını ve güvenlik aks'deki][operator-best-practices-network].

Çekirdek Kubernetes hakkında daha fazla bilgi ve AKS kavramlar için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümesi ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS erişim ve kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeklendirin][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[aks-configure-advanced-networking]: configure-azure-cni.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[use-network-policies]: use-network-policies.md
[operator-best-practices-network]: operator-best-practices-network.md
[support-policies]: support-policies.md
