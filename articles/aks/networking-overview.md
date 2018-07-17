---
title: Ağ yapılandırması Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) temel ve Gelişmiş ağ yapılandırması hakkında bilgi edinin.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: da78d388c8e9fc9684942342f48902c2a248e3b1
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072308"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Ağ yapılandırması Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) kümesini oluştururken, iki ağ seçenekler arasından seçim yapabilirsiniz: **temel** veya **Gelişmiş**.

## <a name="basic-networking"></a>Temel ağ

**Temel** olan AKS kümesi oluşturma için varsayılan yapılandırma seçeneği ağ. Küme ve kendi pod'ların ağ yapılandırmasını tamamen Azure tarafından yönetilen ve özel sanal ağ yapılandırma gerektirmeyen dağıtımlar için uygundur. Alt ağlar ve IP adresi aralıklarını temel ağ seçtiğinizde kümeye atanan gibi ağ yapılandırmanız üzerinde denetimi yoktur.

Temel ağ kullanılmak üzere yapılandırılmış bir AKS kümesindeki düğümlere [kubernetes] [ kubenet] Kubernetes eklentisi.

## <a name="advanced-networking"></a>Gelişmiş Ağ

**Gelişmiş** ağ, bir Azure sanal ağ kaynakları için otomatik bağlantı sağlayarak bunları yapılandırdığınız ağ (VNet) podlarınız yerleştirir ve zengin tümleştirme, sanal ağlar teklif özelliklerini ayarlayın.
Gelişmiş Ağ olduğunda kullanılabilir kümeleri ile AKS dağıtma [Azure portalında][portal], Azure CLI veya Resource Manager şablonu ile.

Gelişmiş Ağ kullanılmak üzere yapılandırılmış bir AKS kümesindeki düğümlere [Azure kapsayıcı ağ arabirimi (CNI)] [ cni-networking] Kubernetes eklentisi.

![Her tek bir Azure sanal ağa bağlanma köprüleri ile iki düğümü gösteren diyagram][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Gelişmiş ağ özellikleri

Gelişmiş Ağ aşağıdaki avantajları sağlar:

* Mevcut bir VNet, AKS kümesi dağıtmayı veya yeni bir VNet ve alt ağ kümenizin oluşturun.
* Kümedeki her bir pod sanal ağda bir IP adresi atanır ve kümedeki diğer pod'ları ve diğer düğümleri sanal ağ ile doğrudan iletişim kurabilir.
* Bir pod eşlenmiş vnet'teki diğer hizmetlere ve şirket içi ağlara ExpressRoute ve siteden siteye (S2S) üzerinden VPN bağlantıları bağlanabilirsiniz. Pod'ları, ayrıca şirket içinden erişilebilir.
* Bir Kubernetes hizmeti, Azure Load Balancer harici veya dahili olarak kullanıma sunar. Ayrıca temel ağ özelliğidir.
* Bir alt ağdaki hizmet uç noktaları etkinleştirilmiş pod Azure Hizmetleri için Azure depolama ve SQL DB güvenli bir şekilde bağlanabilirsiniz.
* Kullanıcı tanımlı yolları (UDR) trafiği yönlendirmek için bir ağ sanal Gereci pod'ların kullanın.
* Pod'ları genel Internet üzerindeki kaynaklara erişebilir. Ayrıca temel ağ özelliğidir.

> [!IMPORTANT]
> Gelişmiş Ağ en fazla barındırabilir için yapılandırılan bir AKS kümesindeki her düğüm **30 pod'ların** Azure portalını kullanarak yapılandırıldığında.  En yüksek değer, bir Resource Manager şablonu ile bir küme dağıtılırken maxPods özelliği yalnızca değiştirerek değiştirebilirsiniz. Her sanal ağ için Azure CNI eklentisi ile kullanımı sınırlı sağlanan **4096 yapılandırılmış IP adresleri**.

## <a name="advanced-networking-prerequisites"></a>Gelişmiş Ağ önkoşulları

* AKS kümesi için sanal ağda giden internet bağlantısına izin vermelidir.
* Aynı alt ağda birden fazla AKS kümesi oluşturma.
* AKS için Gelişmiş Ağ, Azure özel DNS bölgeleri kullanan sanal ağlar desteklemez.
* AKS kümeleri kullanamazsınız `169.254.0.0/16`, `172.30.0.0/16`, veya `172.31.0.0/16` için Kubernetes hizmeti adres aralığı.
* AKS kümesi için kullanılan hizmet sorumlunuz olmalıdır `Contributor` mevcut bir VNet içeren kaynak grubunu izinleri.

## <a name="plan-ip-addressing-for-your-cluster"></a>Kümeniz için IP adresini planlama

Gelişmiş ağ ile yapılandırılan kümeler için ek planlama gerektirir. Vnet'iniz ve onun alt ağ boyutunu hem çalıştırmayı planladığınız pod'ların sayısını, hem de küme için düğüm sayısını, uyum sağlamak gerekir.

Pod'ların ve küme düğümleri için IP adresleri, sanal ağ içindeki belirli alt ağından atanır. Her düğüm düğüm ve düğüme zamanlanmış pod'ların atanan Azure CNI tarafından önceden yapılandırılmış 30 ek IP adreslerini içeren IP olduğu birincil bir IP ile yapılandırılır. Kümenizin ölçeğini daralttığınızda, her düğüm alt ağdaki IP adresleriyle benzer şekilde yapılandırılır.

Sanal ağ, düğümler ve pod'ları, en az bir alt ağ, bir AKS kümesi IP adresi planlama oluşur ve bir Kubernetes hizmeti adres aralığı.

| Adres aralığı / Azure kaynağı | Limitler ve boyutlandırma |
| --------- | ------------- |
| Sanal ağ | Azure sanal ağı /8 büyük olabilir, ancak yalnızca 4096 IP adreslerini yapılandırabilir. |
| Alt ağ | Düğümler ve pod'ların tutabilecek kadar büyük olmalıdır. En az bir alt ağ boyutunu hesaplamak için: (düğüm sayısını) + (düğüm sayısı * pod'ların düğüm başına). 50 düğümlü bir küme için: (50) + (50 * 30) = 1.550, alt ağınızın /21 bir veya daha büyük olması gerekir. |
| Kubernetes hizmeti adres aralığı | Bu aralık herhangi bir ağ öğe tarafından kullanılan veya bu sanal ağa bağlı. Hizmeti adresi CIDR /12 küçük olmalıdır. |
| Kubernetes DNS hizmeti IP adresi | Kubernetes içinde IP adresi (kube-dns) Küme hizmetini bulma tarafından kullanılan adres aralığı hizmeti. |
| Docker köprü adresi | IP adresi (CIDR gösteriminde) Docker köprü düğümlerinde IP adresi kullanılır. Varsayılan değer 172.17.0.1/16. |

Her sanal ağ ile Azure CNI eklentisini kullanmak için sınırlı için sağlanan daha önce de belirtildiği **4096 yapılandırılmış IP adresleri**. Gelişmiş Ağ en fazla barındırabilir için yapılandırılan bir kümedeki her düğümün **30 pod'ların**.

## <a name="deployment-parameters"></a>Dağıtım parametreleri

Bir AKS kümesi oluşturduğunuzda, aşağıdaki parametreleri için Gelişmiş Ağ yapılandırılabilir:

**Sanal ağ**: Kubernetes kümesini dağıtmak istediğiniz sanal ağ. Kümeniz için yeni bir sanal ağ oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları izleyerek *sanal ağ oluştur* bölümü.

**Alt ağ**: kümeyi dağıtmak istediğiniz sanal ağ içindeki alt ağ. Kümeniz için sanal ağda yeni bir alt ağ oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları izleyerek *alt ağ oluşturma* bölümü.

**Kubernetes hizmeti adres aralığı**: *Kubernetes hizmeti adres aralığı* içinden adresleri atanmış Kubernetes kümenizi Hizmetleri'nde IP aralığı ( Kuberneteshizmetlerihakkındadahafazlabilgiiçinbkz.[ Hizmetleri] [ services] Kubernetes belgelerinde).

Kubernetes hizmeti IP adresi aralığı:

* Kümeniz sanal ağ IP adresi aralığında olmamalıdır.
* ' % S'küme sanal ağ ile eşleri diğer Vnet'lere ile çakışmaması gerekir
* Tüm şirket içi IP'leri ile çakışmaması gerekir

Çakışan IP aralıkları kullanılıyorsa öngörülemeyen davranışlara neden olabilir. Örneğin, bir pod küme dışında bir IP erişmeye ve IP de hizmet IP'LERİNDEN biri olur, beklenmeyen davranışla sonuçlanarak ve hataları görebilirsiniz.

**Kubernetes DNS hizmeti IP adresi**: küme DNS hizmeti için IP adresi. İçinde bu adresi olmalıdır *Kubernetes hizmeti adres aralığı*.

**Docker köprü adresi**: IP adresi ve Docker köprüsüne atamak için ağ maskesi. Bu IP adresi kümenizin VNet IP adresi aralığında olmamalıdır.

## <a name="configure-networking---cli"></a>Ağ oluşturma - CLI yapılandırma

Azure CLI ile bir AKS kümesi oluşturduğunuzda, Gelişmiş Ağ da yapılandırabilirsiniz. Gelişmiş Ağ özellikleriyle birlikte yeni bir AKS kümesi oluşturmak için aşağıdaki komutları kullanın.

İlk olarak, AKS kümesi katılması mevcut alt ağı için alt ağ kaynak Kimliğini alın:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Kullanım [az aks oluşturma] [ az-aks-create] komutunu `--network-plugin azure` ile Gelişmiş Ağ bir küme oluşturmak için bağımsız değişken. Güncelleştirme `--vnet-subnet-id` önceki adımda toplanan alt ağ kimliği değeri:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Ağ - portal

Aşağıdaki ekran görüntüsünde Azure portalında AKS kümesi oluşturulurken bu ayarları yapılandırma örneği gösterilmektedir:

![Gelişmiş ağ yapılandırma Azure portalında][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki sorular ve yanıtlar uygulamak **Gelişmiş** ağ yapılandırması.

* *VM'ler kümenin alt dağıtabilir miyim?*

  Hayır. Kubernetes kümeniz tarafından kullanılan alt ağ içindeki Vm'leri dağıtma desteklenmiyor. Sanal makineleri aynı sanal ağda, ancak farklı bir alt ağ içinde dağıtılabilir.

* *Pod başına ağ ilkeleri yapılandırabilirim?*

  Hayır. Pod başına ağ ilkeleri şu anda desteklenmiyor.

* *Pod'ların sayısı yapılandırılabilir bir düğüme dağıtılabilir mi?*

  Varsayılan olarak, her düğüm, en fazla 30 pod'ların barındırabilirsiniz. En büyük değeri yalnızca değiştirerek değiştirebilirsiniz `maxPods` bir Resource Manager şablonu ile bir küme dağıtılırken özelliği.

* *AKS kümesi oluşturulurken oluşturduğum alt ağ için ek özelliklerini nasıl yapılandırırım? Örneğin, hizmet uç noktaları.*

  VNet ve AKS küme oluşturma sırasında oluşturduğunuz alt ağlar için özelliklerin tam listesi, standart sanal ağ yapılandırma sayfasında Azure Portalı'nda yapılandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="networking-in-aks"></a>AKS ağ

Aşağıdaki makalelerde AKS de ağ oluşturmayla ilgili daha fazla bilgi edinin:

[Azure Kubernetes Service (AKS) yük dengeleyiciyle bir statik IP adresi kullanın](static-ip.md)

[Azure Container Service (AKS) üzerindeki HTTPS giriş](ingress.md)

[Azure Container Service (AKS) ile iç yük dengeleyici kullanın](internal-lb.md)

### <a name="acs-engine"></a>ACS altyapısı

[Azure Container Service altyapısı (ACS altyapısı)] [ acs-engine] azure'da Docker özellikli kümeler dağıtmak için kullanabileceğiniz Azure Resource Manager şablonları oluşturan bir açık kaynak projesidir. Kubernetes, DC/OS, Swarm modu ve Swarm düzenleyicileri ile ACS altyapısı dağıtılabilir.

ACS altyapısı ile oluşturulmuş Kubernetes kümeleri destekleyen hem de [kubernetes] [ kubenet] ve [Azure CNI] [ cni-networking] eklentiler. Bu nedenle, temel ve Gelişmiş Ağ senaryoları ACS altyapısı tarafından desteklenir.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
