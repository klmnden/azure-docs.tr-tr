---
title: Ağ yapılandırması Azure Kubernetes hizmet (AKS)
description: Temel ve Gelişmiş ağ yapılandırması Azure Kubernetes hizmet (AKS) hakkında bilgi edinin.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/07/2018
ms.author: marsma
ms.openlocfilehash: 818bae2e05f6a3256ccbf0cbcc901dd337b9a260
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Ağ yapılandırması Azure Kubernetes hizmet (AKS)

Bir Azure Kubernetes hizmet (AKS) kümesi oluşturduğunuzda, iki ağ seçenekler arasından seçim yapabilirsiniz: **temel** veya **Gelişmiş**.

## <a name="basic-networking"></a>Temel ağ iletişimi

**Temel** seçeneği ağ varsayılan yapılandırmadır AKS küme oluşturma. Küme ve onun pod'ları ağ yapılandırmasını tamamen Azure tarafından yönetilen ve özel sanal ağ yapılandırma gerektirmeyen dağıtımlar için uygundur. Alt ağ veya IP adresi aralıklarını temel ağ seçtiğinizde kümeye atanan gibi ağ yapılandırması üzerinde denetime sahip değilsiniz.

Temel ağ kullanılmak üzere yapılandırılmış bir AKS kümedeki düğümler [kubenet] [ kubenet] Kubernetes eklentisi.

## <a name="advanced-networking"></a>Gelişmiş Ağ iletişimi

**Gelişmiş** ağ bir Azure sanal ağındaki sanal ağ kaynaklarına otomatik bağlantı sağlama yapılandırdığınız (VNet), pod'ları yerleştirir ve zengin ile tümleştirme, sanal ağlar teklif özelliklerini ayarlayın.
Gelişmiş ağ kullanılabilir şu anda yalnızca zaman AKS dağıtma içinde kümeleri [Azure portal] [ portal] veya Resource Manager şablonu ile.

Gelişmiş Ağ kullanılmak üzere yapılandırılmış bir AKS kümedeki düğümler [Azure kapsayıcı ağ arabirimi (CNI)] [ cni-networking] Kubernetes eklentisi.

![Her tek bir Azure sanal ağa bağlanma köprüleri ile iki düğüm gösteren diyagram][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Gelişmiş ağ özellikleri

Gelişmiş Ağ aşağıdaki avantajları sağlar:

* AKS kümenizi olan bir VNet içine dağıtmak veya yeni bir VNet ve alt ağ, kümeniz için oluşturun.
* Kümedeki her pod VNet içindeki bir IP adresi atanır ve kümedeki diğer pod'ları ve diğer VM'lerin sanal ağ ile doğrudan iletişim kurabilir.
* Bir pod vnet'teki diğer hizmetler ve şirket içi ağlar ExpressRoute ve siteden siteye (S2S) VPN bağlantıları bağlanabilir. Pod'ları, ayrıca şirket içi erişilebilir.
* Kubernetes hizmeti Azure yük dengeleyici üzerinden harici veya dahili olarak kullanıma sunar. Ayrıca temel ağ özelliğidir.
* Hizmet uç noktaları etkin olan pod'ları bir alt ağda güvenli bir şekilde Azure hizmetlerine, örneğin Azure Storage ve SQL DB bağlanabilir.
* Kullanıcı tanımlı yolları (UDR) trafiğini yönlendirmek için bir ağ sanal gerece pod'ları kullanın.
* Pod'ları ortak Internet üzerindeki kaynaklara erişebilir. Ayrıca temel ağ özelliğidir.

> [!IMPORTANT]
> Gelişmiş Ağ en fazla barındırabilir için yapılandırılan bir AKS kümedeki her düğümde **30 pod'ları**. Azure CNI eklentisi ile kullanmak için sınırlı için sağlanan her bir Vnet'teki **4096 IP adreslerini** (/ 20).

## <a name="configure-advanced-networking"></a>Gelişmiş ağ yapılandırma

Olduğunda, [AKS küme oluşturmak](kubernetes-walkthrough-portal.md) Azure Portal'da aşağıdaki parametreler için Gelişmiş Ağ yapılandırılabilir:

**Sanal ağ**: içine Kubernetes küme dağıtmak istediğiniz VNet. Kümeniz için yeni bir VNet oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları *sanal ağ oluştur* bölümü.

**Alt ağ**: küme dağıtmak istediğiniz sanal ağ içindeki alt ağı. VNet kümeniz için yeni bir alt ağ oluşturmak isteyip istemediğinizi seçin *Yeni Oluştur* ve adımları *alt ağ oluşturmak* bölümü.

**Kubernetes hizmet adres aralığı**: Kubernetes Küme hizmeti IP'leri için IP adres aralığı. Bu aralık, küme sanal IP adres aralığı içinde olmaması gerekir.

**Kubernetes DNS hizmeti IP adresi**: kümenin DNS hizmeti için IP adresi. Bu adres içinde olmalıdır *Kubernetes hizmet adres aralığı*.

**Docker köprüsü adresi**: IP adresi ve Docker köprüsüne atamak için ağ maskesi. Bu IP adresi kümenizin VNet IP adres aralığı içinde olmaması gerekir.

Aşağıdaki ekran görüntüsünde Azure portalından AKS küme oluşturma sırasında bu ayarları yapılandırma örneği gösterilir:

![Gelişmiş Azure Portalı'ndaki Ağ Yapılandırması][portal-01-networking-advanced]

## <a name="plan-ip-addressing-for-your-cluster"></a>Kümeniz için IP adresleme planlama

Gelişmiş ağ ile yapılandırılmış kümeleri ek planlama gerektirir. Sanal ağınızı ve kendi alt ağı, küme, aynı zamanda, ölçeklendirme gereksinimlerinizi aynı anda çalıştırmayı planladığınız pod'ları sayısı boyutuna uygun olmalıdır.

VNet içinde belirtilen alt ağdan pod'ları ve küme düğümleri için IP adresi atanır. Her düğüm düğümün IP ve düğüme zamanlanmış pod'ları atanan Azure CNI tarafından önceden yapılandırılmış 30 ek IP adreslerini bir birincil IP ile yapılandırılır. Kümenizin ölçeklendirdiğinizde her düğümün alt ağdan IP adresleriyle benzer şekilde yapılandırılır.

Azure CNI eklentisi ile kullanmak için sınırlı için sağlanan her bir Vnet'teki daha önce belirtildiği gibi **4096 IP adreslerini** (/ 20). Gelişmiş Ağ en fazla barındırabilir için yapılandırılan bir kümedeki her düğümde **30 pod'ları**.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki sorular ve yanıtlar uygulamak **Gelişmiş** ağ yapılandırması.

* *Gelişmiş Ağ Azure CLI ile yapılandırabilir miyim?*

  Hayır. Gelişmiş Ağ yalnızca zaman AKS dağıtma Azure portalında veya Resource Manager şablonu ile kümeleri şu anda kullanılabilir değil.

* *My Küme alt ağda VM'ler dağıtabilir miyim?*

  Hayır. Sanal makineleri Kubernetes küme tarafından kullanılan alt ağ dağıtımı desteklenmiyor. Sanal makineleri aynı sanal ağda ancak farklı bir alt ağ dağıtılmış.

* *Pod başına ağ ilkeleri yapılandırabilir miyim?*

  Hayır. Pod başına ağ ilkeleri şu anda desteklenmiyor.

* *Pod'ları en fazla bir düğüme yapılandırılabilir dağıtılabilir mi?*

  Varsayılan olarak, her düğümün en fazla 30 pod'ları barındırabilir. Şu anda en büyük değer yalnızca değiştirerek değiştirebileceğiniz `maxPods` Resource Manager şablonu ile bir kümede dağıtırken özelliği.

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
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[aks-ssh]: aks-ssh.md
