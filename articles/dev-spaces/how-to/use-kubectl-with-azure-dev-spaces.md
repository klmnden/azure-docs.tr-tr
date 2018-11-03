---
title: Azure geliştirme alanları ile kubectl kullanma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 05/11/2018
ms.topic: article
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
ms.openlocfilehash: 6af96556d816e8773c3419b4545198148f6eca3a
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978341"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Bir Azure geliştirme boşluk ile kubectl kullanma

Bir Azure geliştirme alanı içindeki bir Kubernetes kümesi erişebilir ve gibi mevcut Kubernetes Araçları `kubectl`.

Çalışan `az aks use-dev-spaces` komutu otomatik olarak ekleyecek bir `kubectl` , kubectl zaten Azure geliştirme alanları Kubernetes kümenize bağlanması gereken şekilde yapılandırma bağlamı. Örnekler:
- Geçerli bağlam onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları listesinde: `kubectl config get-contexts`. 
- Bağlam değiştirme: `kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüle: çalıştırma `kubectl proxy`, ardından bu komutu yayan adresine tarayıcınızı açın (ekleme `/ui` Kubernetes panosuna gitmek için URL'ye).
- Çalışan hizmetleri adlı varsayılan Azure geliştirme alanları alanı listesi *varsayılan*: `kubectl get services --namespace=default`

