---
title: Azure Cloud Shell kullanarak Azure geliştirme alanları için etkin bir Kubernetes kümesi oluşturma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: article
description: Hızlı bir şekilde Azure geliştirme alanları için doğrudan tarayıcınızdan herhangi bir şey yüklemeden etkin bir Kubernetes kümesi oluşturmayı öğrenin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
ms.openlocfilehash: d806607eb3e46d0bd04deb18756021adec59601d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466583"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Azure Cloud Shell kullanarak bir Kubernetes kümesi oluşturma

Kullanabileceğiniz [Azure Cloud Shell](/azure/cloud-shell) kullanarak Azure geliştirme alanları için bir küme oluşturmak için **deneyin** bu sayfadan düğmesi. Oturum açmadıysanız, bir Azure hesabıyla oturum açmak için istemleri takip edin, ardından göründüğünde Azure Cloud Shell isteminde komutları yazın.

## <a name="create-the-cluster"></a>Kümeyi oluşturma

İlk olarak, kaynak grubu oluşturun. Şu an için desteklenen bölgelerden birini (EastUS, EastUS2, CentralUS, WestUS2, WestEurope, SoutheastAsia, CanadaCentral veya CanadaEast) kullanın.

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.10.9 --enable-addons http_application_routing
```

Kümenin oluşturulması birkaç dakika sürer.  Tamamlandığında, çıkış JSON biçiminde gösterilir. Aranacak `provisioningState` ve bunu doğrulayın `Succeeded`.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure geliştirme alanları](/azure/dev-spaces/) bağlantıları için tam öğreticiler.
