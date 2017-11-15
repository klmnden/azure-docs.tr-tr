---
title: "Kubernetes için Azure Container Service’e Giriş | Microsoft Docs"
description: "Kubernetes için Azure Container Service, Azure üzerinde kapsayıcı tabanlı uygulamaları dağıtmayı ve yönetmeyi kolaylaştırır."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Kubernetes, Docker, Kapsayıcılar, Mikro Hizmetler, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 9fba9fdda3503ec80fede845466858825e3677a5
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) giriş

Azure kapsayıcı hizmeti (AKS) oluşturmak, yapılandırmak ve sanal makinelerin kapsayıcılı uygulamaları çalıştırmak için yapılandırılmış bir kümeyi yönetmek kolaylaştırır. Bu sayede Microsoft Azure’daki kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için mevcut becerilerinizi kullanabilir veya kapsamlı ve gelişmeye devam eden topluluk uzmanlığından faydalanabilirsiniz.

AKS kullanarak, Azure, kurumsal düzeyde özelliklerini hala Kubernetes ve Docker görüntü biçimi aracılığıyla uygulama taşınabilirliği korurken yararlanabilirsiniz.

## <a name="managed-kubernetes-in-azure"></a>Azure'da Kubernetes yönetilen

AKS Azure bu sorumluluğu çoğunu boşaltarak Kubernetes küme yönetme işlem yükünü ve karmaşıklığını azaltır. Barındırılan bir Kubernetes hizmeti, sistem durumu izleme ve sizin için bakım gibi kritik görevleri Azure tanıtıcıları. Ayrıca, yalnızca Aracısı düğümleri için yönetici olmayan kümelerinizi içinde ödersiniz. Yönetilen bir Kubernetes hizmet olarak AKS sağlar:

> [!div class="checklist"]
> * Otomatik Kubernetes sürüm yükseltme ve düzeltme eki uygulama
> * Kolay küme ölçeklendirme
> * Kendini onarma barındırılan denetim düzlemi (Yöneticileri)
> * Maliyet tasarrufu - aracı havuzu düğümleri yalnızca çalıştırmak için ödeme

AKS kümenizdeki düğümlerin yönetim işleme Azure ile artık birçok el ile Küme yükseltme gibi görevleri gerekmez. Bu kritik bakım görevlerini sizin yerinize Azure işlemesi nedeniyle AKS doğrudan erişim sağlamaz (gibi SSH ile) kümeye.

## <a name="using-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) kullanma
AKS amacı barındırma ortamı açık kaynaklı araçları ve bugün müşterileri arasında popüler bir teknoloji kullanarak bir kapsayıcı sağlamaktır. Bu işlem için standart Kubernetes API uç noktalarını kullanıma sunacağız. Bu standart uç noktaları kullanarak, bir Kubernetes kümesiyle iletişim kurma özelliğine sahip olan tüm yazılımlardan faydalanabilirsiniz. Örneğin [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) veya [draft](https://github.com/Azure/draft) arasından seçim yapabilirsiniz.

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) kullanarak Kubernetes küme oluşturma
AKS'ı kullanmaya başlamak için bir AKS kümeyle dağıtmak [Azure CLI](./kubernetes-walkthrough.md) veya portal aracılığıyla (Market arama **Azure kapsayıcı hizmeti**). Azure Resource Manager şablonları hakkında daha fazla denetime gereksinim duyan İleri düzey bir kullanıcıysanız açık kaynak [acs-engine](https://github.com/Azure/acs-engine) projesini kullanarak kendi özel Kubernetes kümenizi oluşturup `az` CLI’si ile dağıtabilirsiniz.

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

Azure kapsayıcı hizmeti (AKS) - Azure Cuma Ekim 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Geliştirmek ve Kubernetes - Azure OpenDev Haziran 2017 uygulamaları dağıtmak için Araçlar:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Dağıtma ve AKS AKS Hızlı Başlangıç ile yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi](./kubernetes-walkthrough.md)