---
title: Kubectl Azure Dev alanları ile kullanma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Kapsayıcılar ve Azure üzerinde mikro ile hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcıları
manager: douge
ms.openlocfilehash: 38a433a14ab977fb56a8331a057d27241f1d9783
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Bir Azure Dev alanıyla kubectl kullanın

Kubernetes küme Azure Dev alanı içinde erişim ve kullanım varolan Kubernetes Araçlar `kubectl`.

Çalışan `azds resource create` veya `azds resource select` otomatik olarak ekleyecek bir `kubectl` , kubectl Azure Dev alanları Kubernetes kümenize zaten bağlı şekilde yapılandırma bağlamı. Örnekler:
- Geçerli bağlam onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları listesinde: `kubectl config get-contexts`. 
- Bağlam değiştirin: `kubectl config use-context <context-name>`
- Kubernetes Panoyu görebilmek: çalıştırmak `kubectl proxy`, ardından bu komut yayar adresine tarayıcınızı açın (append `/ui` Kubernetes panoya gitmek için URL).
- Çalışan hizmetleri adlı varsayılan Azure Dev alanları alanı listesi *varsayılan*: `kubectl get services --namespace=default`

