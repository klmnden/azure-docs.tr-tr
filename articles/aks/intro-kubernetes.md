---
title: Azure Kubernetes Hizmeti Tanıtımı
description: Azure Kubernetes Hizmeti, Azure üzerinde kapsayıcı tabanlı uygulamaları dağıtmayı ve yönetmeyi kolaylaştırır.
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: overview
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 4936554465fbbed45000f43853a6a77567c3028f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="introduction-to-azure-kubernetes-service-aks-preview"></a>Azure Kubernetes Hizmeti (AKS) önizlemesi tanıtımı

Azure Kubernetes Hizmeti (AKS), kapsayıcı uygulamalarda çalışmak üzere önceden yapılandırılmış sanal makine kümesi oluşturma, yapılandırma ve yönetim süreçlerini basitleştirir. Bu sayede Microsoft Azure’daki kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için mevcut becerilerinizi kullanabilir veya kapsamlı ve gelişmeye devam eden topluluk uzmanlığından faydalanabilirsiniz.

AKS’yi kullanarak Azure’un kuruluş düzeyindeki özelliklerinden faydalanırken Kubernetes ve Docker görüntü biçimi aracılığıyla uygulama taşınabilirliğini koruyabilirsiniz.

> [!IMPORTANT]
> Azure Kubernetes Hizmeti (AKS), şu anda **önizleme** aşamasındadır. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="managed-kubernetes-in-azure"></a>Azure’da Yönetilen Kubernetes

AKS, sorumluluğun çoğunu Azure’a devrederek bir Kubernetes kümesi yönetmenin karmaşıklığı ve işlemsel yükünü azaltır. Barındırılan bir Kubernetes hizmeti olarak, Azure sistem durumu izleme ve bakım gibi kritik görevleri sizin için gerçekleştirir. Ayrıca, ana düğümler değil yalnızca kümelerinizdeki aracı düğümler için ücret ödersiniz. Yönetilen bir Kubernetes hizmeti olarak AKS aşağıdakileri sağlar:

> [!div class="checklist"]
> * Otomatik Kubernetes sürüm yükseltmeleri ve düzeltme eki uygulama
> * Kolay küme ölçeklendirme
> * Kendi kendini onaran barındırılan denetim düzlemi (yöneticiler)
> * Maliyet tasarrufu Yalnızca çalışan aracı havuz düğümleri için ücret ödeyin

AKS kümenizde düğümleri Azure yönetirken, küme yükseltme gibi görevleri el ile gerçekleştirmeniz gerekmez. Azure bu kritik bakım görevlerini sizin için gerçekleştirdiğinden, AKS kümeye doğrudan erişim (SSH ile olduğu gibi) sağlamaz.

## <a name="using-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti’ni (AKS) kullanma
AKS’nin amacı, günümüzde müşteriler arasında popüler olan açık kaynak araçları ve teknolojileri kullanan bir kapsayıcı barındırma ortamı sunmaktır. Bu işlem için standart Kubernetes API uç noktalarını kullanıma sunacağız. Bu standart uç noktaları kullanarak, bir Kubernetes kümesiyle iletişim kurma özelliğine sahip olan tüm yazılımlardan faydalanabilirsiniz. Örneğin [kubectl][kubectl-overview], [helm][helm] veya [draft][draft] arasından seçim yapabilirsiniz.

## <a name="creating-a-kubernetes-cluster-using-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti’ni (AKS) kullanarak Kubernetes kümesi oluşturma
AKS’yi kullanmaya başlamak için portal aracılığıyla (Market’te **Azure Kubernetes Hizmeti** ifadesini aratın) veya [Azure CLI][aks-quickstart] ile bir AKS kümesi dağıtın. Azure Resource Manager şablonlarında daha fazla denetime gereksinim duyan ileri düzey bir kullanıcıysanız açık kaynak [acs-engine][acs-engine] projesini kullanarak kendi özel Kubernetes kümenizi oluşturup `az` CLI'si ile dağıtabilirsiniz.

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

Azure Kubernetes Hizmeti (AKS) - Azure Friday, Ekim 2017:

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

