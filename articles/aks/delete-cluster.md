---
title: "Bir Azure kapsayıcı hizmeti (AKS) küme silme"
description: "CLI veya Azure portal ile küme silme ve AKS."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fd5d3bd7cb2ce5d8f0999710bfeb6a9514231d8d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="delete-an-azure-container-service-aks-cluster"></a>Bir Azure kapsayıcı hizmeti (AKS) küme silme

Azure kapsayıcı hizmeti kümesi silerken küme dağıtıldığı kaynak grubu kalır, ancak tüm AKS ilgili kaynaklar silinir. Bu belge, Azure CLI ve Azure portalını kullanarak bir AKS kümesini silmek gösterilmiştir. 

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
| `--resource-group` `-g` | Azure kapsayıcı hizmeti kaynak grubunun adı. | evet |
| `--no-wait` | Tamamlanması uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--yes` `-y` | Onay isteme. | hayır |

## <a name="azure-portal"></a>Azure portalına

Azure portalında karşın, Gözat Azure kapsayıcı hizmeti (AKS) kaynağını içeren kaynak grubunu, kaynağı seçin ve tıklatın **silmek**. Silme işlemi onaylamanız istenir.

![AKS küme portal Sil](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: azure-files-dynamic-pv.md