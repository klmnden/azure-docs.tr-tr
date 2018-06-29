---
title: Bir Azure Kubernetes hizmet (AKS) küme silme
description: CLI veya Azure portal ile küme silme ve AKS.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 66dcebb702695a6601f6ed17b85a04d5bb4e01f6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100052"
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