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
ms.openlocfilehash: cf518fb0062a44619894059a1b7369fc92ba4f5d
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597177"
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
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

Kümenin oluşturulması birkaç dakika sürer.  Tamamlandığında, çıkış JSON biçiminde gösterilir. Aranacak `provisioningState` ve bunu doğrulayın `Succeeded`.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure geliştirme alanları](/azure/dev-spaces/) bağlantıları için tam öğreticiler.
