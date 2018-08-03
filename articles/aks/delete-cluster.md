---
title: Azure Kubernetes Service (AKS) kümesini Sil
description: CLI veya Azure portalı ile silebilir ve AKS kümesi.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8eab17a5c635560d9a5274eb038845238968e02
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439944"
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesini Sil

Bir Azure Kubernetes hizmeti kümesi silinirken, kümenin dağıtıldığı kaynak grubu kalır, ancak tüm AKS ile ilgili kaynaklar silinir. Bu belge, Azure CLI ve Azure portalını kullanarak bir AKS kümesini silme işlemi gösterilmektedir.

Küme silmeye ek olarak, bu dağıtıldığı kaynak grubu, AKS kümesi de silen silinebilir.

## <a name="azure-cli"></a>Azure CLI

Kullanım [az aks Sil] [ az-aks-delete] AKS kümesini silmek için komutu.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

Aşağıdaki seçenekler kullanılabilen `az aks delete` komutu.

| Bağımsız değişken | Açıklama | Gerekli |
|---|---|:---:|
| `--name` `-n` | Yönetilen küme kaynağı adı. | evet |
| `--resource-group` `-g` | Azure Kubernetes hizmeti kaynak grubunun adı. | evet |
| `--no-wait` | Uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--yes` `-y` | Onay için istemde bulunmayın. | hayır |

## <a name="azure-portal"></a>Azure portal

Ancak Azure portalında, Azure Kubernetes Service (AKS) kaynağını içeren kaynak grubunu bulun, kaynağı seçin ve tıklayın **Sil**. Silme işlemini onaylamanız istenir.

![AKS kümesi portalı Sil](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az-aks-delete