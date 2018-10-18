---
title: Kavramlar - ağ Azure Kubernetes hizmeti (AKS)
description: Azure Kubernetes hizmeti (temel ve Gelişmiş Ağ, giriş denetleyicileri, yük Dengeleyiciler ve statik IP adresleri de dahil olmak üzere AKS), ağ oluşturma hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: 62ba98f221041d5bbf9bb095a02d052218eb0fd0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381277"
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

Kubernetes, *Hizmetleri* pod'ları, bir IP adresi veya DNS adı aracılığıyla ve belirli bir bağlantı noktasında doğrudan erişim sağlamak amacıyla gruplandırıp. Trafik kullanarak dağıtabileceğiniz bir *yük dengeleyici*. Daha karmaşık uygulama trafiği yönlendirme de ile gerçekleştirilebilir *giriş denetleyicileri*. Güvenlik ve pod'ların ağ trafiğini filtreleme Kubernetes ile olası *ağ ilkelerini*.

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

- *Temel* ağ - ağ kaynaklarının oluşturulup yapılandırılması AKS kümesi dağıtılır.
- *Gelişmiş* ağ - AKS kümesini mevcut sanal ağ kaynakları ve yapılandırmaların bağlı.

### <a name="basic-networking"></a>Temel ağ

*Temel* olan AKS kümesi oluşturma için varsayılan yapılandırma seçeneği ağ. Azure platformu küme ve pod'ların ağ yapılandırmasını yönetir. Temel ağ, sanal özel ağ yapılandırması gerektirmez dağıtımlar için uygundur. Temel ağ ile alt ağ adları gibi ağ yapılandırması veya AKS kümeye atanan IP adresi aralıklarını tanımlayamazsınız.

Temel ağ kullanılmak üzere yapılandırılmış bir AKS kümesindeki düğümlere [kubernetes] [ kubenet] Kubernetes eklentisi.

Temel ağ aşağıdaki özellikleri sağlar:

- Bir Kubernetes hizmeti, Azure Load Balancer harici veya dahili olarak kullanıma sunar.
- Pod'ları genel Internet üzerindeki kaynaklara erişebilir.

### <a name="advanced-networking"></a>Gelişmiş Ağ

*Gelişmiş* ağ yapılandırdığınız bir Azure sanal ağında podlarınız yerleştirir. Bu sanal ağa otomatik bağlantı diğer Azure kaynaklarını ve zengin bir özellik kümesi ile tümleştirme sağlar. Gelişmiş Ağ gerektiren belirli bir sanal ağ yapılandırmaları gibi bir var olan bir alt ağ ve bağlantı kullanmak için dağıtımlar için uygundur. Gelişmiş ağ ile bu alt ağ adları ve IP adresi aralıklarını tanımlayabilirsiniz.

Gelişmiş Ağ kullanılmak üzere yapılandırılmış bir AKS kümesindeki düğümlere [Azure kapsayıcı ağ arabirimi (CNI)] [ cni-networking] Kubernetes eklentisi.

![Her tek bir Azure sanal ağa bağlanma köprüleri ile iki düğümü gösteren diyagram][advanced-networking-diagram]

Gelişmiş ağ üzerinden temel ağ aşağıdaki özellikleri sağlar:

- Mevcut bir Azure sanal ağına, AKS kümesi dağıtmayı veya bir yeni sanal ağ ve alt kümenizin oluşturun.
- Kümedeki her bir pod sanal ağdaki bir IP adresi atanır. Pod'ları, kümedeki diğer pod'ları ve diğer düğümleri sanal ağ ile doğrudan iletişim kurabilir.
- Bir pod ExpressRoute ve siteden siteye (S2S) üzerinden şirket içi ağa VPN bağlantıları dahil eşlenen sanal ağ, diğer hizmetlere bağlanabilir. Pod'ları, ayrıca şirket içinden erişilebilir.
- Bir alt ağdaki hizmet uç noktaları etkinleştirilmiş pod güvenli bir şekilde Azure depolama ve SQL DB gibi Azure hizmetlerine bağlanabilirsiniz.
- Kullanıcı tanımlı yollar (UDR) bir ağ sanal Gereci pod'ların trafiği yönlendirmek için oluşturabilirsiniz.

Daha fazla bilgi için [ağ bir AKS kümesi için Gelişmiş Yapılandırma][aks-configure-advanced-networking].

## <a name="ingress-controllers"></a>Giriş denetleyicileri

LoadBalancer türü hizmet oluşturduğunuzda, bir temel alınan Azure yük dengeleyici kaynağı oluşturulur. Yük Dengeleyici, pod'larını hizmetinizdeki belirli bir bağlantı noktasında trafiği dağıtmak için yapılandırılır. 4 - katmanında LoadBalancer yalnızca çalışır hizmet gerçek uygulamaları farkında değildir ve herhangi hususlar yönlendirme yapamazsınız.

*Giriş denetleyicileri* 7 katmanında çalışır ve daha akıllı kurallarını uygulama trafiği dağıtmak için kullanabilirsiniz. Bir ortak giriş denetleyicisine gelen URL'sini temel alarak farklı uygulamalar için HTTP trafiği yönlendirmek için kullanılır.

![Bir AKS kümesinde giriş trafik akışını gösteren diyagram][aks-ingress]

AKS, NGINX gibi kullanarak bir giriş kaynağı oluşturun veya AKS HTTP uygulama yönlendirme özelliğini kullanın. Bir AKS kümesi için uygulama HTTP yönlendirme etkinleştirdiğinizde, Azure platformu giriş denetleyicisine oluşturur ve bir *dış DNS* denetleyicisi. Yeni giriş kaynakları Kubernetes'te oluşturuldukça gerekli DNS A kayıtlarını kümeye özgü bir DNS bölgesinde oluşturulur. Daha fazla bilgi için [HTTP uygulama yönlendirme dağıtma][aks-http-routing].

Başka bir ortak giriş SSL/TLS sonlandırma özelliğidir. Giriş kaynağı yerine uygulamanın kendisinden, HTTPS erişilen büyük web uygulamaları, TLS sonlandırma işlenebilir. Otomatik TLS sertifika oluşturma ve yapılandırma sağlamak üzere, giriş kaynak sağlayıcılarını şimdi şifreleme gibi kullanacak şekilde yapılandırabilirsiniz. Şimdi şifreleme ile bir NGINX giriş denetleyicisine yapılandırma hakkında daha fazla bilgi için bkz. [giriş ve TLS][aks-ingress-tls].

## <a name="network-security-groups"></a>Ağ güvenlik grupları

Bir ağ güvenlik grubu filtresi trafiği VM'ler için AKS düğümleri gibi. Bir yük dengeleyici gibi hizmetleri oluştururken Azure platformu, gerekli olan tüm ağ güvenlik grubu kuralları otomatik olarak yapılandırır. El ile bir AKS kümesi pod'ların trafiğini filtrelemek için ağ güvenlik grubu kuralları yapılandırmayın. Tüm gerekli bağlantı noktaları ve iletme Kubernetes hizmeti bildirimlerinizi bir parçası olarak tanımlayın ve Azure platform oluşturma veya güncelleştirme olarak uygun kuralları sağlar.

Varsayılan olarak ağ güvenlik grubu kuralları gibi SSH trafiği için mevcut. Bu varsayılan kuralları küme yönetimi ve erişim sorunlarını giderme içindir. Bu varsayılan kuralların silinmesi AKS yönetim sorunlara neden olabilir ve hizmet düzeyi hedefi (SLO) keser.

## <a name="next-steps"></a>Sonraki adımlar

Ağ AKS ile çalışmaya başlama bkz [oluşturma bir AKS kümesi için Gelişmiş Ağ ve][aks-configure-advanced-networking].

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
[aks-configure-advanced-networking]: configure-advanced-networking.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md