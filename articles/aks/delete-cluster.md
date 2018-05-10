---
title: Bir Azure Kubernetes hizmet (AKS) küme silme
description: CLI veya Azure portal ile küme silme ve AKS.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e006466d1450471900a8635c49d3bc6c3a73d476
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Bir Azure Kubernetes hizmet (AKS) küme silme

Azure Kubernetes hizmeti kümesi silerken küme dağıtıldığı kaynak grubu kalır, ancak tüm AKS ilgili kaynaklar silinir. Bu belge, Azure CLI ve Azure portalını kullanarak bir AKS kümesini silmek gösterilmiştir.

Küme silme yanı sıra, dağıtıldığı kaynak grubu, ayrıca AKS küme silen silinebilir.

## <a name="azure-cli"></a>Azure CLI

Kullanım [az aks silme] [ az-aks-delete] AKS kümesini silmek için komutu.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

Aşağıdaki seçenekler ile kullanılabilen `az aks delete` komutu.

| Bağımsız değişken | Açıklama | Gerekli |
|---|---|:---:|
| `--name` `-n` | Yönetilen küme kaynağı adı. | evet |
| `--resource-group` `-g` | Azure Kubernetes hizmeti kaynak grubunun adı. | evet |
| `--no-wait` | Tamamlanması uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--yes` `-y` | Onay isteme. | hayır |

## <a name="azure-portal"></a>Azure portalına

Azure portalında karşın, Gözat Azure Kubernetes hizmet (AKS) kaynağını içeren kaynak grubunu, kaynağı seçin ve tıklatın **silmek**. Silme işlemi onaylamanız istenir.

![AKS küme portal Sil](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az_aks_delete