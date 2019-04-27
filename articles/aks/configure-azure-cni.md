---
title: Azure Kubernetes Service (AKS) Azure CNI ağı yapılandırma
description: Azure Kubernetes Service (AKS kümesi bir var olan sanal ağı ve alt ağ içinde dağıtımı dahil olmak üzere AKS), Azure CNI (Gelişmiş) ağ yapılandırmayı öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 4bd934c710d6300e95c60742d5873f5b71bdae59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466555"
---
# <a name="configure-azure-cni-networking-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) Azure CNI ağı yapılandırma

Varsayılan olarak, AKS kümesi kullanım [kubernetes][kubenet], ve bir sanal ağ ve alt sizin için oluşturulur. İle *kubernetes*, düğümler, bir sanal ağ alt ağından bir IP adresi alın. Ağ adresi çevirisi (NAT) sonra düğümler üzerinde yapılandırılır ve pod'ların "Gizli düğüm IP'sini" bir IP adresi alır. Bu yaklaşım ağ alanınıza kullanmak pod'ları ayırmanız gerekir IP adresi sayısını azaltır.

İle [Azure kapsayıcı ağ arabirimi (CNI)][cni-networking], her pod alt ağdan bir IP adresi alır ve doğrudan erişilebilir. Bu IP adresleri, ağ alanı benzersiz olmalıdır ve önceden hazırlıklı olmak gerekir. Her düğümü destekliyorsa pod'ların sayısı için bir yapılandırma parametresi vardır. Düğüm başına IP adreslerinin sayısı, bu düğüm için önden ayrılmıştır. Bu yaklaşım, daha fazla planlama gerektirir ve genellikle IP adresi tükenmesi veya uygulama arttıkça daha büyük bir alt ağ kümelerini yeniden gereksinimini doğurur.

Bu makalede nasıl kullanılacağını gösterir *Azure CNI* oluşturmak ve bir AKS kümesi için bir sanal ağ alt ağı kullanmak için ağ. Ağ seçenekleri ve konuları hakkında daha fazla bilgi için bkz. [ağ kavramları Kubernetes ve AKS için][aks-network-concepts].

## <a name="prerequisites"></a>Önkoşullar

* AKS kümesi için sanal ağ, giden internet bağlantısına izin vermelidir.
* Aynı alt ağda birden fazla AKS kümesi oluşturma.
* AKS kümeleri kullanamazsınız `169.254.0.0/16`, `172.30.0.0/16`, veya `172.31.0.0/16` için Kubernetes hizmeti adres aralığı.
* AKS kümesi tarafından kullanılan hizmet sorumlusunun en az olmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor) sanal ağınızdaki bir alt ağ üzerindeki izinleri. Tanımlamak istiyorsanız bir [özel rol](../role-based-access-control/custom-roles.md) yerleşik ağ Katılımcısı rolü kullanmak yerine, aşağıdaki izinler gereklidir:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Kümeniz için IP adresini planlama

Azure CNI ağ ile yapılandırılan kümeler için ek planlama gerektirir. Sanal ağınız ile onun alt ağ boyutunu çalıştırmayı planladığınız pod'ların sayısını ve kümenin düğüm sayısını, uyum sağlamak gerekir.

Pod'ların ve küme düğümleri için IP adresleri, sanal ağ içinde belirtilen alt ağından atanır. Her düğüm, birincil bir IP adresi ile yapılandırılır. Varsayılan olarak 30 ek IP adresleri düğümde zamanlanmış pod'ların atanan Azure CNI tarafından önceden yapılandırılmış. Kümenizin ölçeğini daralttığınızda, her düğüm alt ağdaki IP adresleriyle benzer şekilde yapılandırılır. Ayrıca görüntüleyebilirsiniz [düğüm başına en fazla pod'ların](#maximum-pods-per-node).

> [!IMPORTANT]
> Gerekli IP adresi sayısı, yükseltme ve ölçeklendirme işlemlerinin dikkate alınacak noktalar içermelidir. Yalnızca düğümlerin sabit sayıda desteklemek için IP adresi aralığı ayarlarsanız, yükseltin veya kümenizi ölçeklendirme.
>
> - Olduğunda, **yükseltme** AKS kümenizi, yeni bir düğüm kümesine dağıtılır. Hizmetleri ve iş yüklerini yeni düğüm üzerinde çalışmaya başlar ve daha eski bir düğümü kümeden kaldırılır. Bu sıralı yükseltme işlemi en az bir ek kullanılabilir olması için IP adres bloğunu gerektirir. Bu durumda, düğüm sayısını `n + 1`.
>
> - Olduğunda, **ölçek** AKS kümesi, yeni bir düğüm kümesine dağıtılır. Hizmetleri ve iş yüklerini yeni düğüm üzerinde çalışmaya başlar. IP adresi aralığınızı düğümleri ve kümenize destekleyebilir pod'ların sayısını ölçeklendirmek nasıl isteyebileceğiniz noktaları olması gerekir. Yükseltme işlemleri için ek bir düğümü da bulunması gerekir. Bu durumda, düğüm sayısını `n + number-of-additional-scaled-nodes-you-anticipate + 1`.

Pod'ların, en fazla sayısını çalıştırın ve düzenli olarak yok et ve pod'ları dağıtmak için düğümlerinizin bekliyorsanız, ayrıca bazı ek IP adresleri düğüm başına hesaba katmanız. Bu ek IP adresleri silinecek bir hizmet için birkaç saniye sürebilir dikkate alın ve IP adresi serbest dağıtılması ve adresini almak için yeni bir hizmet.

IP adresi planı bir AKS kümesi bir sanal oluşur için ağ, düğümler, pod'ların ve bir Kubernetes hizmeti adres aralığı için en az bir alt ağ.

| Adres aralığı / Azure kaynağı | Limitler ve boyutlandırma |
| --------- | ------------- |
| Sanal ağ | Azure sanal ağı /8 büyük olabilir, ancak 65.536 yapılandırılmış IP adresleri için sınırlıdır. |
| Alt ağ | Düğümler, pod'ların ve kümenizde sağlanan tüm Kubernetes ile Azure kaynaklarını tutabilecek kadar büyük olmalıdır. Örneğin, bir iç Azure yük dengeleyici dağıtırsanız, Küme alt ağından değil genel IP'ler, ön uç IP'ler ayrılır. Alt ağ boyutunu da hesap yükseltme işlemleri veya gelecekteki ölçeklendirme gereksinimlerinizi almanız gerekir.<p />Hesaplamak için *minimum* yükseltme işlemleri için ek bir düğümüne dahil alt ağ boyutu: `(number of nodes + 1) + ((number of nodes + 1) * maximum pods per node that you configure)`<p/>Örneğin, 50 düğümlü bir küme: `(51) + (51  * 30 (default)) = 1,581` (/ 21 veya daha büyük)<p/>Örneğin, ek bir 10 düğümlerini ölçeklendirmek için sağlama da içeren bir 50 düğüm kümesi: `(61) + (61 * 30 (default)) = 1,891` (/ 21 veya daha büyük)<p>Kümenizi oluştururken pod'ların düğüm başına en fazla belirtmezseniz, pod'ların düğüm başına en fazla sayısını kümesine *30*. IP adresleri gerekli en düşük sayısı bu değere göre belirlenir. En düşük IP adresi gereksinimlerinize göre farklı bir maksimum değer hesaplamak olup [pod'ların düğüm başına en fazla sayısını yapılandırmak nasıl](#configure-maximum---new-clusters) kümenizi dağıttığınızda bu değeri ayarlamak için. |
| Kubernetes hizmeti adres aralığı | Bu aralık herhangi bir ağ öğe tarafından kullanılan veya bu sanal ağa bağlı. Hizmeti adresi CIDR /12 küçük olmalıdır. |
| Kubernetes DNS hizmeti IP adresi | Kubernetes içinde IP adresi (kube-dns) Küme hizmetini bulma tarafından kullanılan adres aralığı hizmeti. .1 gibi adres aralığındaki ilk IP adresini kullanmayın. İlk adres, alt ağ aralığında için kullanılan *kubernetes.default.svc.cluster.local* adresi. |
| Docker köprü adresi | IP adresi (CIDR gösteriminde) Docker köprü düğümlerinde IP adresi kullanılır. Varsayılan değer 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Düğüm başına en fazla pod'ları

Pod'ların bir AKS kümesindeki düğüm başına en fazla sayısını 110 ' dir. *Varsayılan* pod'ların düğüm başına en fazla sayısını arasında değişir *kubernetes* ve *Azure CNI* ağ ve küme dağıtım yöntemi.

| Dağıtım yöntemi | Kubernetes varsayılan | Azure CNI varsayılan | Dağıtım sırasında yapılandırılabilir |
| -- | :--: | :--: | -- |
| Azure CLI | 110 | 30 | Evet (en fazla 110) |
| Resource Manager şablonu | 110 | 30 | Evet (en fazla 110) |
| Portal | 110 | 30 | Hayır |

### <a name="configure-maximum---new-clusters"></a>Maksimum - yeni kümeleri yapılandırma

Pod'ların düğüm başına en fazla sayısını yapılandırabilirsiniz *küme dağıtım sırasında yalnızca*. Azure CLI veya Resource Manager şablonu ile dağıtırsanız, en fazla pod'ların her 110 yüksek olan düğüm değeri ayarlayabilirsiniz.

* **Azure CLI**: Belirtin `--max-pods` ile bir küme dağıtılırken bağımsız değişken [az aks oluşturma] [ az-aks-create] komutu. En fazla 110 değerdir.
* **Resource Manager şablonu**: Belirtin `maxPods` özelliğinde [ManagedClusterAgentPoolProfile] bir Resource Manager şablonu ile bir küme dağıtılırken nesne. En fazla 110 değerdir.
* **Azure portalında**: Azure portalı ile bir küme dağıtılırken, pod'ların düğüm başına en fazla sayısını değiştiremezsiniz. Azure portalını kullanarak dağıttığınızda azure CNI ağ kümelerine düğüm başına 30 pod'ların sınırlıdır.

### <a name="configure-maximum---existing-clusters"></a>Maksimum - var olan kümeleri yapılandırma

Mevcut bir AKS kümesindeki düğüm başına en fazla pod'ların değiştiremezsiniz. Yalnızca ilk kümesi dağıtırken sayısını ayarlayabilirsiniz.

## <a name="deployment-parameters"></a>Dağıtım parametreleri

Bir AKS kümesi oluşturduğunuzda, aşağıdaki parametreleri için Azure CNI ağ yapılandırılabilir:

**Sanal ağ**: Kubernetes kümesini dağıtmak istediğiniz sanal ağı. Kümeniz için yeni bir sanal ağ oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları izleyerek *sanal ağ oluştur* bölümü. Bir Azure sanal ağı için kotaları ve sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Alt ağ**: Kümeyi dağıtmak istediğiniz sanal ağ içindeki alt ağ. Kümenizin bir sanal ağdaki yeni bir alt ağ oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları izleyerek *alt ağ oluşturma* bölümü. Karma bağlantı için adres aralığı ortamınızdaki diğer sanal ağlara ile çakışma olmaması gerekir.

**Kubernetes hizmeti adres aralığı**: Kubernetes için iç atar sanal IP'ler kümesidir [Hizmetleri] [ services] kümenizdeki. Aşağıdaki gereksinimleri karşılayan herhangi bir özel adres aralığını kullanabilirsiniz:

* Kümeniz sanal ağ IP adresi aralığında olmamalıdır.
* Küme sanal ağ ile eşleri diğer sanal ağlara ile çakışmaması gerekir
* Tüm şirket içi IP'leri ile çakışmaması gerekir
* Aralıklar olmamalıdır `169.254.0.0/16`, `172.30.0.0/16`, veya `172.31.0.0/16`

Bunun yapılması hizmeti adres aralığı aynı sanal ağ içinde küme olarak belirtmek teknik olarak mümkün olsa da, bu nedenle önerilmez. Çakışan IP aralıkları kullanılıyorsa öngörülemeyen davranışlara neden olabilir. Daha fazla bilgi için [SSS](#frequently-asked-questions) bu makalenin. Kubernetes hizmetleri hakkında daha fazla bilgi için bkz. [Hizmetleri] [ services] Kubernetes belgelerinde.

**Kubernetes DNS hizmeti IP adresi**:  Küme DNS hizmeti IP adresi. İçinde bu adresi olmalıdır *Kubernetes hizmeti adres aralığı*. .1 gibi adres aralığındaki ilk IP adresini kullanmayın. İlk adres, alt ağ aralığında için kullanılan *kubernetes.default.svc.cluster.local* adresi.

**Docker köprü adresi**: Docker köprüsüne atamak için ağ maskesi ve IP adresi. Docker köprü AKS düğümleri temel alınan yönetim platformu ile iletişim kurmasına olanak tanır. Bu IP adresi kümenizin sanal ağ IP adresi aralığında olmamalıdır ve ağınızda kullanımda başka adres aralıklarıyla çakışıyor olmamalıdır.

## <a name="configure-networking---cli"></a>Ağ oluşturma - CLI yapılandırma

Azure CLI ile bir AKS kümesi oluşturduğunuzda, Azure CNI ağ da yapılandırabilirsiniz. Azure CNI ağ ile yeni bir AKS kümesi oluşturmak için aşağıdaki komutları kullanın.

İlk olarak, AKS kümesi katılması mevcut alt ağı için alt ağ kaynak Kimliğini alın:

```console
$ az network vnet subnet list \
    --resource-group myVnet \
    --vnet-name myVnet \
    --query [].id --output tsv

/subscriptions/<guid>/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Kullanım [az aks oluşturma] [ az-aks-create] komutunu `--network-plugin azure` ile Gelişmiş Ağ bir küme oluşturmak için bağımsız değişken. Güncelleştirme `--vnet-subnet-id` önceki adımda toplanan alt ağ kimliği değeri:

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Ağ - portal

Aşağıdaki ekran görüntüsünde Azure portalında AKS kümesi oluşturulurken bu ayarları yapılandırma örneği gösterilmektedir:

![Gelişmiş ağ yapılandırma Azure portalında][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki sorular ve yanıtlar uygulamak **Azure CNI** ağ yapılandırması.

* *VM'ler kümenin alt dağıtabilir miyim?*

  Hayır. Kubernetes kümeniz tarafından kullanılan alt ağ içindeki Vm'leri dağıtma desteklenmiyor. Sanal makineleri aynı sanal ağda, ancak farklı bir alt ağ içinde dağıtılabilir.

* *Pod başına ağ ilkeleri yapılandırabilirim?*

  Kubernetes Ağ İlkesi şu anda aks'deki bir önizleme özelliği olarak kullanılabilir. Başlamak için bkz: [güvenli ağ ilkeleri kullanarak AKS pod'ları arasındaki trafiği][network-policy].

* *Pod'ların sayısı yapılandırılabilir bir düğüme dağıtılabilir mi?*

  Azure CLI veya Resource Manager şablonu ile bir küme dağıtılırken, Evet. Bkz: [düğüm başına en fazla pod](#maximum-pods-per-node).

  Pod'ların var olan bir kümede düğüm başına en fazla sayısını değiştiremezsiniz.

* *AKS kümesi oluşturulurken oluşturduğum alt ağ için ek özelliklerini nasıl yapılandırırım? Örneğin, hizmet uç noktaları.*

  AKS küme oluşturma sırasında oluşturduğunuz alt ağlar ve sanal ağ için özelliklerinin tam listesini standart sanal ağ yapılandırma sayfasında Azure Portalı'nda yapılandırılabilir.

* *Benim için küme sanal ağ içindeki farklı bir alt kullanabilirim* **Kubernetes hizmeti adres aralığı**?

  Önerilmez, ancak bu yapılandırma mümkündür. Hizmeti adres aralığı, Kubernetes kümenizde iç Hizmetleri atar sanal IP'ler (VIP) kümesidir. Azure ağı yok görünürlük bir Kubernetes kümesinin hizmet IP aralığı vardır. Küme hizmeti adres aralığı görünürlük eksikliği nedeniyle, daha sonra hizmet adres aralığıyla çakışıyor küme sanal ağda yeni bir alt ağ oluşturmak mümkündür. Böyle bir çakışma ortaya çıkarsa, Kubernetes hizmet öngörülemeyen davranışlara veya hatalara neden alt ağdaki başka bir kaynak tarafından kullanımda bir IP atayabilirsiniz. Küme sanal ağ dışındaki bir adres aralığı kullandığınız sağlayarak bu çakışma risk önleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="networking-in-aks"></a>AKS ağ

Aşağıdaki makalelerde AKS de ağ oluşturmayla ilgili daha fazla bilgi edinin:

- [Azure Kubernetes Service (AKS) yük dengeleyiciyle bir statik IP adresi kullanın](static-ip.md)
- [Azure Container Service (AKS) ile iç yük dengeleyici kullanın](internal-lb.md)

- [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
- [Dinamik genel IP ile bir giriş denetleyicisi oluşturmak ve TLS sertifikalarını otomatik olarak oluşturmak için şimdi şifreleme yapılandırma][aks-ingress-tls]
- [Giriş denetleyicisine statik bir genel IP oluşturun ve otomatik olarak TLS sertifikalarını oluşturmak için şimdi şifreleme yapılandırma][aks-ingress-static-tls]

### <a name="aks-engine"></a>AKS altyapısı

[Azure Kubernetes Service Engine (AKS altyapısı)] [ aks-engine] azure'da Kubernetes kümelerini dağıtmak için kullanabileceğiniz Azure Resource Manager şablonları oluşturan bir açık kaynak projesidir.

AKS altyapıyla oluşturulmuş Kubernetes kümeleri destekleyen hem de [kubernetes] [ kubenet] ve [Azure CNI] [ cni-networking] eklentiler. Bu nedenle, her iki ağ senaryoları AKS altyapısı tarafından desteklenir.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
[network-policy]: use-network-policies.md
