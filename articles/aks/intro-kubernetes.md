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
ms.date: 10/24/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: fa46d16e2105c0354cc533c58fb9e0093c129ced
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) giriş

Azure kapsayıcı hizmeti (AKS) oluşturmak, yapılandırmak ve sanal makinelerin kapsayıcılı uygulamaları çalıştırmak için yapılandırılmış bir kümeyi yönetmek kolaylaştırır. Bu sayede Microsoft Azure’daki kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için mevcut becerilerinizi kullanabilir veya kapsamlı ve gelişmeye devam eden topluluk uzmanlığından faydalanabilirsiniz.

AKS kullanarak, Azure, kurumsal düzeyde özelliklerini hala Kubernetes ve Docker görüntü biçimi aracılığıyla uygulama taşınabilirliği korurken yararlanabilirsiniz.

## <a name="using-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) kullanma
Amacımız AKS ile Barındırma ortamı açık kaynaklı araçları ve bugün müşterilerimizin arasında popüler bir teknoloji kullanarak bir kapsayıcı sağlamaktır. Bu işlem için standart Kubernetes API uç noktalarını kullanıma sunacağız. Bu standart uç noktaları kullanarak, bir Kubernetes kümesiyle iletişim kurma özelliğine sahip olan tüm yazılımlardan faydalanabilirsiniz. Örneğin [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) veya [draft](https://github.com/Azure/draft) arasından seçim yapabilirsiniz.

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

Azure Container Services'daki Kubernetes Desteği (Azure Friday, Ocak 2017):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Kubernetes’te Uygulama Geliştirme ve Dağıtma Araçları (Azure OpenDev, Haziran 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Dağıtma ve AKS AKS Hızlı Başlangıç ile yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi](./kubernetes-walkthrough.md)