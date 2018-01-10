---
title: "Kubernetes için Azure Container Service’e Giriş"
description: "Kubernetes için Azure Container Service, Azure üzerinde kapsayıcı tabanlı uygulamaları dağıtmayı ve yönetmeyi kolaylaştırır."
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: overview
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 2980d4733dcf138c3a9aa1713cff994bbf5a296c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Azure Container Service’e (AKS) Giriş

Azure Container Service (AKS), kapsayıcı uygulamalarda çalışmak üzere önceden yapılandırılmış sanal makine kümesi oluşturma, yapılandırma ve yönetim süreçlerini basitleştirir. Bu sayede Microsoft Azure’daki kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için mevcut becerilerinizi kullanabilir veya kapsamlı ve gelişmeye devam eden topluluk uzmanlığından faydalanabilirsiniz.

AKS’yi kullanarak Azure’un kuruluş düzeyindeki özelliklerinden faydalanırken Kubernetes ve Docker görüntü biçimi aracılığıyla uygulama taşınabilirliğini koruyabilirsiniz.

## <a name="managed-kubernetes-in-azure"></a>Azure’da Yönetilen Kubernetes

AKS, sorumluluğun çoğunu Azure’a devrederek bir Kubernetes kümesi yönetmenin karmaşıklığı ve işlemsel yükünü azaltır. Barındırılan bir Kubernetes hizmeti olarak, Azure sistem durumu izleme ve bakım gibi kritik görevleri sizin için gerçekleştirir. Ayrıca, ana düğümler değil yalnızca kümelerinizdeki aracı düğümler için ücret ödersiniz. Yönetilen bir Kubernetes hizmeti olarak AKS aşağıdakileri sağlar:

> [!div class="checklist"]
> * Otomatik Kubernetes sürüm yükseltmeleri ve düzeltme eki uygulama
> * Kolay küme ölçeklendirme
> * Kendi kendini onaran barındırılan denetim düzlemi (yöneticiler)
> * Maliyet tasarrufu Yalnızca çalışan aracı havuz düğümleri için ücret ödeyin

AKS kümenizde düğümleri Azure yönetirken, küme yükseltme gibi görevleri el ile gerçekleştirmeniz gerekmez. Azure bu kritik bakım görevlerini sizin için gerçekleştirdiğinden, AKS kümeye doğrudan erişim (SSH ile olduğu gibi) sağlamaz.

## <a name="using-azure-container-service-aks"></a>Azure Container Service’i (AKS) kullanma
AKS’nin amacı, günümüzde müşteriler arasında popüler olan açık kaynak araçları ve teknolojileri kullanan bir kapsayıcı barındırma ortamı sunmaktır. Bu işlem için standart Kubernetes API uç noktalarını kullanıma sunacağız. Bu standart uç noktaları kullanarak, bir Kubernetes kümesiyle iletişim kurma özelliğine sahip olan tüm yazılımlardan faydalanabilirsiniz. Örneğin [kubectl][kubectl-overview], [helm][helm] veya [draft][draft] arasından seçim yapabilirsiniz.

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Azure Container Service (AKS) kullanan bir Kubernetes kümesi oluşturma
AKS'yi kullanmaya başlamak için portal aracılığıyla (Market'te **Azure Container Service** ifadesini aratın) veya [Azure CLI][aks-quickstart] ile bir AKS kümesi dağıtın. Azure Resource Manager şablonları hakkında daha fazla denetime gereksinim duyan İleri düzey bir kullanıcıysanız açık kaynak [acs-engine][acs-engine] projesini kullanarak kendi özel Kubernetes kümenizi oluşturup `az` CLI'si ile dağıtabilirsiniz.

### <a name="using-kubernetes"></a>Kubernetes kullanma
Kubernetes, kapsayıcılı uygulamaların dağıtımını, ölçeklendirmesini ve yönetimini otomatikleştirir. Aşağıdaki zengin özelliklere sahiptir:
* Otomatik bin paketleme
* Kendi kendini iyileştirme
* Yatay ölçekleme
* Hizmet bulma ve yük dengeleme
* Otomatik piyasaya çıkarma ve geri alma işlemleri
* Gizli dizi ve yapılandırma yönetimi
* Depolama düzenleme
* Toplu iş yürütme

## <a name="videos"></a>Videolar

Azure Container Service (AKS) - Azure Friday, Ekim 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Kubernetes’te Uygulama Geliştirme ve Dağıtma Araçları - Azure OpenDev, Haziran 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

AKS hızlı başlangıçları ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi][aks-quickstart]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md

