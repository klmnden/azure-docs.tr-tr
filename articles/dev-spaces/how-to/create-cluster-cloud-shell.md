---
title: Azure Cloud Shell kullanarak Azure geliştirme alanları için etkin bir Kubernetes kümesi oluşturma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 10/04/2018
ms.topic: article
description: Hızlı bir şekilde Azure geliştirme alanları için doğrudan tarayıcınızdan herhangi bir şey yüklemeden etkin bir Kubernetes kümesi oluşturmayı öğrenin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
ms.openlocfilehash: 47c467e020a7a9253daa636352352d9a57dddf28
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978162"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Azure Cloud Shell kullanarak bir Kubernetes kümesi oluşturma

Kullanabileceğiniz [Azure Cloud Shell](/azure/cloud-shell) kullanarak Azure geliştirme alanları için bir küme oluşturmak için **deneyin** bu sayfadan düğmesi. Oturum açmadıysanız, bir Azure hesabıyla oturum açmak için istemleri takip edin, ardından göründüğünde Azure Cloud Shell isteminde komutları yazın.

## <a name="create-the-cluster"></a>Kümeyi oluşturma

İlk olarak, kaynak grubu oluşturun. Şu anda desteklenen bölgeler (EastUS, EastUS2, CentralUS, WestUS2, WestEurope, SoutheastAsia, CanadaCentral veya CanadaEast) birini kullanın.

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.3 --enable-addons http_application_routing
```

Kümenin oluşturulması birkaç dakika sürer.  Tamamlandığında, çıkış JSON biçiminde gösterilir. Aranacak `provisioningState` ve bunu doğrulayın `Succeeded`.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure geliştirme alanları](/azure/dev-spaces/) bağlantıları için tam öğreticiler.
