---
title: Azure Cloud Shell kullanarak Azure geliştirme alanları için etkin bir Kubernetes kümesi oluşturma
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: conceptual
description: Hızlı bir şekilde Azure geliştirme alanları için doğrudan tarayıcınızdan herhangi bir şey yüklemeden etkin bir Kubernetes kümesi oluşturmayı öğrenin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s
ms.openlocfilehash: bfde055c99a1109a8f71c838b44ed1742e3f06aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60911643"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Azure Cloud Shell kullanarak Kubernetes kümesi oluşturma

Kullanabileceğiniz [Azure Cloud Shell](/azure/cloud-shell) kullanarak Azure geliştirme alanları için bir küme oluşturmak için **deneyin** bu sayfadan düğmesi. Oturum açmadıysanız, bir Azure hesabıyla oturum açmak için istemleri takip edin, ardından göründüğünde Azure Cloud Shell isteminde komutları yazın.

## <a name="create-the-cluster"></a>Kümeyi oluşturma

İlk olarak, kaynak grubunu oluşturma bir [Azure geliştirme alanları destekleyen bir bölge](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region>
```

Kümenin oluşturulması birkaç dakika sürer.  Tamamlandığında, çıkış JSON biçiminde gösterilir. Aranacak `provisioningState` ve bunu doğrulayın `Succeeded`.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure geliştirme alanları](/azure/dev-spaces/) bağlantıları için tam öğreticiler.
