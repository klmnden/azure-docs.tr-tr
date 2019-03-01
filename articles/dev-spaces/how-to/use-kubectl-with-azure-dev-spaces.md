---
title: Azure geliştirme alanları ile kubectl kullanma
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s '
ms.openlocfilehash: 328c67b04f8e582fcd7d953c7cd8b6c5312bada5
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194323"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Bir Azure geliştirme boşluk ile kubectl kullanma

Bir Azure geliştirme alanı içindeki bir Kubernetes kümesi erişebilir ve gibi mevcut Kubernetes Araçları `kubectl`.

Çalışan `az aks use-dev-spaces` komutu otomatik olarak ekleyecek bir `kubectl` , kubectl zaten Azure geliştirme alanları Kubernetes kümenize bağlanması gereken şekilde yapılandırma bağlamı. Örnekler:
- Geçerli bağlam onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları listesinde: `kubectl config get-contexts`. 
- Bağlam değiştirme: `kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüle: çalıştırma `kubectl proxy`, ardından bu komutu yayan adresine tarayıcınızı açın (ekleme `/ui` Kubernetes panosuna gitmek için URL'ye).
- Çalışan hizmetleri adlı varsayılan Azure geliştirme alanları alanı listesi *varsayılan*: `kubectl get services --namespace=default`

