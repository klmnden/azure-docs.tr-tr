---
title: "Azure Öğreticisi - dağıtmak küme üzerinde Kubernetes | Microsoft Docs"
description: "AKS Öğreticisi - küme dağıtma"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 69dea4ab748d88d18cf01dc9b3fc1bdddd562681
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Bir Azure kapsayıcı hizmeti (AKS) kümeyi dağıtma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. AKS ile basit ve hızlı bir üretim hazır Kubernetes kümenin sağlama. Bu öğreticide, bir parçası üç sekiz, Kubernetes küme AKS içinde dağıtılır. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * Kubectl yapılandırması

Sonraki öğreticilerde, Azure oy uygulama olduğundan kümeye dağıtılan, ölçeği, güncelleştirilmiş ve Operations Management Suite Kubernetes küme izlemek için yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine bir kapsayıcı görüntüsü oluşturuldu ve Azure kapsayıcı kayıt defteri örneğine yüklenir. Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./tutorial-kubernetes-prepare-app.md).

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Azure aboneliğiniz için AKS Önizleme etkinleştirme
AKS önizlemede olsa da, yeni küme oluşturma aboneliğinizi bir özellik bayrağı gerektirir. Bu özellik için kullanmak istediğiniz abonelikleri herhangi bir sayıda isteyebilir. Kullanım `az provider register` komutu AKS sağlayıcısını kaydetmek için:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Kaydolduktan sonra artık ile AKS Kubernetes küme oluşturmak hazırsınız.

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Aşağıdaki örnek adlı bir küme oluşturur `myK8sCluster` adlı bir kaynak grubunda `myResourceGroup`. Bu kaynak grubunun oluşturulduğu [önceki Öğreticisi](./tutorial-kubernetes-prepare-acr.md).

```azurecli
az aks create --resource-group myResourceGroup --name myK8sCluster --node-count 1 --generate-ssh-keys
```

Birkaç dakika sonra dağıtım tamamlar ve AKS dağıtımı hakkında bilgi döndürür json biçimli.

## <a name="install-the-kubectl-cli"></a>CLI kubectl yükleyin

İstemci bilgisayarından Kubernetes kümeye bağlanmak için [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), Kubernetes komut satırı istemcisi.

Azure CloudShell kullanıyorsanız kubectl zaten yüklüdür. Yerel olarak yüklemek istiyorsanız, aşağıdaki komutu çalıştırın:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

Kubernetes kümenize bağlanmak için kubectl yapılandırmak için aşağıdaki komutu çalıştırın:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Kümenize bağlantıyı doğrulamak için Çalıştır [kubectl alma düğümleri](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) komutu.

```azurecli
kubectl get nodes
```

Çıktı:

```
NAME                          STATUS    AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     49m       v1.7.7
```

Eğitmen tamamlandığında, iş yükleri için hazır bir AKS kümesine sahip. Sonraki öğreticilerde, bir çok kapsayıcı uygulama bu kümeye dağıtılan, çıkışı ölçeği, güncelleştirilmiş ve izlenen.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Kubernetes küme içinde AKS dağıtıldı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Bir Kubernetes AKS kümesi dağıtılmış
> * Kubernetes CLI (kubectl) yüklü
> * Yapılandırılmış kubectl

Uygulama küme üzerinde çalışan hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes uygulamasında dağıtma](./tutorial-kubernetes-deploy-application.md)
