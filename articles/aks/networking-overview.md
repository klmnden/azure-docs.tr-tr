---
title: Ağ yapılandırması Azure Kubernetes hizmet (AKS)
description: Temel ve Gelişmiş ağ yapılandırması Azure Kubernetes hizmet (AKS) hakkında bilgi edinin.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: 207accc30e10c4e2bed5b713fc59e2f9ad86a876
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309851"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Ağ yapılandırması Azure Kubernetes hizmet (AKS)

Bir Azure Kubernetes hizmet (AKS) kümesi oluşturduğunuzda, iki ağ seçenekler arasından seçim yapabilirsiniz: **temel** veya **Gelişmiş**.

## <a name="basic-networking"></a>Temel ağ iletişimi

**Temel** seçeneği ağ varsayılan yapılandırmadır AKS küme oluşturma. Küme ve onun pod'ları ağ yapılandırmasını tamamen Azure tarafından yönetilen ve özel sanal ağ yapılandırma gerektirmeyen dağıtımlar için uygundur. Alt ağ veya IP adresi aralıklarını temel ağ seçtiğinizde kümeye atanan gibi ağ yapılandırması üzerinde denetime sahip değilsiniz.

Temel ağ kullanılmak üzere yapılandırılmış bir AKS kümedeki düğümler [kubenet] [ kubenet] Kubernetes eklentisi.

## <a name="advanced-networking"></a>Gelişmiş Ağ iletişimi

**Gelişmiş** ağ bir Azure sanal ağındaki sanal ağ kaynaklarına otomatik bağlantı sağlama yapılandırdığınız (VNet), pod'ları yerleştirir ve zengin ile tümleştirme, sanal ağlar teklif özelliklerini ayarlayın.
Gelişmiş Ağ olduğunda kullanılabilir AKS dağıtma ile kümeleri [Azure portal][portal], Azure CLI veya Resource Manager şablonu ile.

Gelişmiş Ağ kullanılmak üzere yapılandırılmış bir AKS kümedeki düğümler [Azure kapsayıcı ağ arabirimi (CNI)] [ cni-networking] Kubernetes eklentisi.

![Her tek bir Azure sanal ağa bağlanma köprüleri ile iki düğüm gösteren diyagram][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Gelişmiş ağ özellikleri

Gelişmiş Ağ aşağıdaki avantajları sağlar:

* AKS kümenizi olan bir VNet içine dağıtmak veya yeni bir VNet ve alt ağ, kümeniz için oluşturun.
* Kümedeki her pod VNet içindeki bir IP adresi atanır ve kümedeki diğer pod'ları ve diğer düğümlere de VNet ile doğrudan iletişim kurabilir.
* Bir pod vnet'teki diğer hizmetler ve şirket içi ağlar ExpressRoute ve siteden siteye (S2S) VPN bağlantıları bağlanabilir. Pod'ları, ayrıca şirket içi erişilebilir.
* Kubernetes hizmeti Azure yük dengeleyici üzerinden harici veya dahili olarak kullanıma sunar. Ayrıca temel ağ özelliğidir.
* Hizmet uç noktaları etkin olan pod'ları bir alt ağda güvenli bir şekilde Azure hizmetlerine, örneğin Azure Storage ve SQL DB bağlanabilir.
* Kullanıcı tanımlı yolları (UDR) trafiğini yönlendirmek için bir ağ sanal gerece pod'ları kullanın.
* Pod'ları ortak Internet üzerindeki kaynaklara erişebilir. Ayrıca temel ağ özelliğidir.

> [!IMPORTANT]
> Gelişmiş Ağ en fazla barındırabilir için yapılandırılan bir AKS kümedeki her düğümde **30 pod'ları** Azure portalını kullanarak yapılandırıldığında.  En büyük değer Resource Manager şablonu ile bir kümede dağıtırken maxPods özelliği yalnızca değiştirerek değiştirebilirsiniz. Azure CNI eklentisi ile kullanmak için sınırlı için sağlanan her bir Vnet'teki **4096 yapılandırılmış IP adresleri**.

## <a name="advanced-networking-prerequisites"></a>Gelişmiş Ağ önkoşulları

* VNet AKS küme için giden internet bağlantısına izin vermesi gerekir.
* Birden fazla AKS küme aynı alt ağdaki oluşturmayın.
* Gelişmiş Ağ AKS için Azure özel DNS bölgeleri kullanan sanal ağlar desteklemez.
* AKS kümeleri kullanamazsınız `169.254.0.0/16`, `172.30.0.0/16`, veya `172.31.0.0/16` Kubernetes için hizmet adres aralığı.
* AKS kümesi için kullanılan hizmet sorumlusu olmalıdır `Contributor` mevcut VNet içeren kaynak grubunu izinleri.

## <a name="plan-ip-addressing-for-your-cluster"></a>Kümeniz için IP adresleme planlama

Gelişmiş ağ ile yapılandırılmış kümeleri ek planlama gerektirir. Sanal ağınızı ve kendi alt ağı hem çalıştırmayı planladığınız pod'ları sayısı, hem de küme düğümleri sayısı boyutuna uygun olmalıdır.

VNet içinde belirtilen alt ağdan pod'ları ve küme düğümleri için IP adresi atanır. Her düğüm, düğüm ve düğüme zamanlanmış pod'ları atanan Azure CNI tarafından önceden yapılandırılmış 30 ek IP adresleri IP olduğu birincil IP ile yapılandırılır. Kümenizin ölçeklendirdiğinizde her düğümün alt ağdan IP adresleriyle benzer şekilde yapılandırılır.

En az bir alt düğümleri ve pod'ları, bir VNet AKS küme IP adresini planlama oluşur ve bir Kubernetes hizmet adres aralığı.

| Adres aralığı / Azure kaynak | Sınırları ve boyutlandırma |
| --------- | ------------- |
| Sanal ağ | Azure sanal ağı /8 kadar büyük olabilir, ancak yalnızca 4096 yapılandırılmış IP adresine sahip. |
| Alt ağ | Düğümler ve pod'ları uyabilecek kadar büyük olmalıdır. En düşük alt ağ boyutunu hesaplamak için: (düğüm sayısı) + (düğüm sayısı * her düğüm pod'ları). 50 düğümlü bir küme için: (50) + (50 * 30) 1.550, =, alt ağ /21 bir veya daha büyük olması gerekir. |
| Kubernetes hizmet adres aralığı | Bu aralık herhangi bir ağ öğesi tarafından kullanılan veya bu sanal ağa bağlı. Hizmeti adresi CIDR /12 küçük olması gerekir. |
| Kubernetes DNS hizmeti IP adresi | IP adresi Kubernetes içinde hizmet Küme hizmetini bulma (kube-dns) tarafından kullanılan adres aralığı. |
| Docker köprüsü adresi | IP adresi (CIDR gösteriminde) Docker köprü olarak düğümler üzerinde IP adresi kullanılır. 172.17.0.1/16 varsayılan. |

Azure CNI eklentisi ile kullanmak için sınırlı için sağlanan her bir Vnet'teki daha önce belirtildiği gibi **4096 yapılandırılmış IP adresleri**. Gelişmiş Ağ en fazla barındırabilir için yapılandırılan bir kümedeki her düğümde **30 pod'ları**.

## <a name="deployment-parameters"></a>Dağıtım parametreleri

AKS Küme oluşturduğunuzda, aşağıdaki parametreleri Gelişmiş Ağ iletişimi için yapılandırılabilir:

**Sanal ağ**: içine Kubernetes küme dağıtmak istediğiniz VNet. Kümeniz için yeni bir VNet oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları *sanal ağ oluştur* bölümü.

**Alt ağ**: küme dağıtmak istediğiniz sanal ağ içindeki alt ağı. VNet kümeniz için yeni bir alt ağ oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları *alt ağ oluşturmak* bölümü.

**Kubernetes hizmet adres aralığı**: *Kubernetes hizmet adres aralığı* içinden adresleri atanır kümenizdeki Kubernetes Hizmetleri IP aralığı ( Kuberneteshizmetlerihakkındadahafazlabilgiiçinbkz.[ Hizmetleri] [ services] Kubernetes belgelerinde).

Kubernetes hizmeti IP adresi aralığı:

* Kümenizi VNet IP adres aralığı içinde olmamalıdır
* İle VNet küme eşlere diğer Vnet ile çakışmaması gerekir
* Tüm şirket içi IP'leri ile çakışmaması gerekir

Çakışan IP aralıkları kullanılıyorsa beklenmeyen davranışlara neden olabilir. Örneğin, bir pod küme dışındaki bir IP erişmeyi dener ve IP ayrıca bir hizmet IP olmasını olur, beklenmeyen davranışlara ve hataları görebilirsiniz.

**Kubernetes DNS hizmeti IP adresi**: kümenin DNS hizmeti için IP adresi. Bu adres içinde olmalıdır *Kubernetes hizmet adres aralığı*.

**Docker köprüsü adresi**: IP adresi ve Docker köprüsüne atamak için ağ maskesi. Bu IP adresi kümenizin VNet IP adres aralığı içinde olmaması gerekir.

## <a name="configure-networking---cli"></a>Ağ - CLI yapılandırın

Azure CLI ile bir AKS kümesi oluşturduğunuzda, Gelişmiş Ağ de yapılandırabilirsiniz. Gelişmiş ağ özellikleri etkin yeni bir AKS kümesi oluşturmak için aşağıdaki komutları kullanın.

İlk olarak, alt ağ kaynak kimliği AKS küme katılması varolan alt ağı için alın:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Kullanım [az aks oluşturma] [ az-aks-create] komutunu `--network-plugin azure` ile Gelişmiş Ağ bir küme oluşturmak için bağımsız değişken. Güncelleştirme `--vnet-subnet-id` alt ağ kimliği değeri önceki adımda toplanır:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Ağ - yapılandırma portalı

Aşağıdaki ekran görüntüsünde Azure portalından AKS küme oluşturma sırasında bu ayarları yapılandırma örneği gösterilir:

![Gelişmiş Azure Portalı'ndaki Ağ Yapılandırması][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki sorular ve yanıtlar uygulamak **Gelişmiş** ağ yapılandırması.

* *Gelişmiş Ağ Azure CLI ile yapılandırabilir miyim?*

  Hayır. Gelişmiş Ağ yalnızca zaman AKS dağıtma Azure portalında veya Resource Manager şablonu ile kümeleri şu anda kullanılabilir değil.

* *My Küme alt ağda VM'ler dağıtabilir miyim?*

  Hayır. Sanal makineleri Kubernetes küme tarafından kullanılan alt ağ dağıtımı desteklenmiyor. Sanal makineleri aynı sanal ağda ancak farklı bir alt ağ dağıtılmış.

* *Pod başına ağ ilkeleri yapılandırabilir miyim?*

  Hayır. Pod başına ağ ilkeleri şu anda desteklenmiyor.

* *Pod'ları en fazla bir düğüme yapılandırılabilir dağıtılabilir mi?*

  Varsayılan olarak, her düğümün en fazla 30 pod'ları barındırabilir. En büyük değer yalnızca değiştirerek değiştirebilirsiniz `maxPods` Resource Manager şablonu ile bir kümede dağıtırken özelliği.

* *AKS küme oluşturma sırasında oluşturulan alt ağ için ek özellikler nasıl yapılandırırım? Örneğin, hizmet uç noktaları.*

  AKS küme oluşturma sırasında oluşturduğunuz alt ağlar ve sanal ağ için özelliklerin tam listesini standart VNet yapılandırma sayfasında Azure Portalı'nda yapılandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="networking-in-aks"></a>AKS'de ağ iletişimi

Aşağıdaki makalelerde AKS de ağ oluşturmayla ilgili daha fazla bilgi edinin:

[Statik bir IP adresi olan Azure Kubernetes hizmet (AKS) yük dengeleyici kullanın](static-ip.md)

[HTTPS giriş üzerinde Azure kapsayıcı hizmeti (AKS)](ingress.md)

[Azure kapsayıcı hizmeti (AKS) bir iç yük dengeleyici kullanın](internal-lb.md)

### <a name="acs-engine"></a>ACS altyapısı

[Azure kapsayıcı hizmeti altyapısı (ACS altyapısı)] [ acs-engine] Azure Docker etkin kümelerinde dağıtmak için kullanabileceğiniz Azure Resource Manager şablonları oluşturan bir açık kaynak projesidir. Kubernetes, DC/OS, Swarm modu ve Swarm orchestrators ACS altyapısıyla dağıtılabilir.

ACS altyapısıyla oluşturulan Kubernetes kümeleri destekleyen her ikisi de [kubenet] [ kubenet] ve [Azure CNI] [ cni-networking] eklentileri. Bu nedenle, temel ve Gelişmiş Ağ senaryoları ACS altyapısı tarafından desteklenir.

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
