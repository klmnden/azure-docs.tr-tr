---
title: Azure geliştirme alanları ile kubectl kullanma | Microsoft Docs
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
ms.openlocfilehash: 978274a6059a327c8596df6776eaa47a27ea4a34
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056343"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Bir Azure geliştirme boşluk ile kubectl kullanma

Bir Azure geliştirme alanı içindeki bir Kubernetes kümesi erişebilir ve gibi mevcut Kubernetes Araçları `kubectl`.

Çalışan `az aks use-dev-spaces` komutu otomatik olarak ekleyecek bir `kubectl` , kubectl zaten Azure geliştirme alanları Kubernetes kümenize bağlanması gereken şekilde yapılandırma bağlamı. Örnekler:
- Geçerli bağlam onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları listesinde: `kubectl config get-contexts`. 
- Bağlam değiştirme: `kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüle: çalıştırma `kubectl proxy`, ardından bu komutu yayan adresine tarayıcınızı açın (ekleme `/ui` Kubernetes panosuna gitmek için URL'ye).
- Çalışan hizmetleri adlı varsayılan Azure geliştirme alanları alanı listesi *varsayılan*: `kubectl get services --namespace=default`

