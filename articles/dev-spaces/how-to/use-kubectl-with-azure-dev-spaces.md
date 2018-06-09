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
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: 3dc0dd4b571f716bcabb67c4cbef1ea6d762eb94
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248667"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Bir Azure Dev alanıyla kubectl kullanın

Kubernetes küme Azure Dev alanı içinde erişim ve kullanım varolan Kubernetes Araçlar `kubectl`.

Çalışan `az aks use-dev-spaces` komutu otomatik olarak ekleyecek bir `kubectl` , kubectl Azure Dev alanları Kubernetes kümenize zaten bağlı şekilde yapılandırma bağlamı. Örnekler:
- Geçerli bağlam onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları listesinde: `kubectl config get-contexts`. 
- Bağlam değiştirin: `kubectl config use-context <context-name>`
- Kubernetes Panoyu görebilmek: çalıştırmak `kubectl proxy`, ardından bu komut yayar adresine tarayıcınızı açın (append `/ui` Kubernetes panoya gitmek için URL).
- Çalışan hizmetleri adlı varsayılan Azure Dev alanları alanı listesi *varsayılan*: `kubectl get services --namespace=default`

