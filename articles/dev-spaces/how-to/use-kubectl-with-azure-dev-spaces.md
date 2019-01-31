---
title: Azure geliştirme alanları ile kubectl kullanma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: article
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
ms.openlocfilehash: b0ceccc4f8b16c21e8a66a5f1b8cf0e7b6f47a4f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469320"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Bir Azure geliştirme boşluk ile kubectl kullanma

Bir Azure geliştirme alanı içindeki bir Kubernetes kümesi erişebilir ve gibi mevcut Kubernetes Araçları `kubectl`.

Çalışan `az aks use-dev-spaces` komutu otomatik olarak ekleyecek bir `kubectl` , kubectl zaten Azure geliştirme alanları Kubernetes kümenize bağlanması gereken şekilde yapılandırma bağlamı. Örnekler:
- Geçerli bağlam onaylayın: `kubectl config current-context`
- Tüm kullanılabilir bağlamları listesinde: `kubectl config get-contexts`. 
- Bağlam değiştirme: `kubectl config use-context <context-name>`
- Kubernetes panosunu görüntüle: çalıştırma `kubectl proxy`, ardından bu komutu yayan adresine tarayıcınızı açın (ekleme `/ui` Kubernetes panosuna gitmek için URL'ye).
- Çalışan hizmetleri adlı varsayılan Azure geliştirme alanları alanı listesi *varsayılan*: `kubectl get services --namespace=default`

