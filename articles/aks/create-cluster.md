---
title: Bir Azure Kubernetes hizmet (AKS) kümesi oluşturma
description: CLI veya Azure portal ile bir AKS kümesi oluşturun.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 46c2f718911f27ad8f51423589c6fdda100a68d2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Bir Azure Kubernetes hizmet (AKS) kümesi oluşturma

Azure CLI veya Azure portal ile bir Azure Kubernetes hizmet (AKS) küme oluşturulabilir.

## <a name="azure-cli"></a>Azure CLI

Kullanım [az aks oluşturma] [ az-aks-create] AKS küme oluşturmak için komutu.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Aşağıdaki seçenekler ile kullanılabilen `az aks create` komutu.

| Bağımsız değişken | Açıklama | Gerekli |
|---|---|:---:|
| `--name` `-n` | Yönetilen küme kaynağı adı. | evet |
| `--resource-group` `-g` | Azure Kubernetes hizmeti kaynak grubunun adı. | evet |
| `--admin-username` `-u` | Kullanıcı adı için Linux sanal makineleri.  Varsayılan: azureuser. | hayır |
| ` --client-secret` | Hizmet sorumlusuyla ilişkili gizli dizi. | hayır |
| `--dns-name-prefix` `-p` | Kümeleri genel IP adresi için DNS öneki. | hayır |
| `--generate-ssh-keys` | SSH ortak ve özel anahtar dosyaları eksikse oluşturur. | hayır |
| `--kubernetes-version` `-k` | '1.7.9' veya '1.8.2' gibi bir küme oluşturmak için kullanılacak Kubernetes sürümü.  Varsayılan: 1.7.7. | hayır |
| `--no-wait` | Tamamlanması uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--node-count` `-c` | Düğüm düğüm havuzları için varsayılan sayısı.  Varsayılan: 3. | hayır |
| `--node-osdisk-size` | Sanal makine düğümünü havuzunun GB osDisk boyutu. | hayır |
| `--node-vm-size` `-s` | Sanal makine boyutu.  Varsayılan: Standard_D1_v2. | hayır |
| `--service-principal` | Küme kimlik doğrulaması için kullanılan hizmet sorumlusu. | hayır |
| `--ssh-key-value` | SSH anahtar dosyası değer veya anahtar dosya yolu.  Varsayılan: ~ /.ssh/id_rsa.pub. | hayır |
| `--tags` | Boşluk [= değer]'key ' etiketleri ayrılan biçimi. Kullanım '' varolan etiketleri temizleyin. | hayır |

## <a name="azure-portal"></a>Azure portalına

Azure Kubernetes hizmet (AKS) Azure portal ile bir AKS kümesi dağıtma ile ilgili yönergeler için bkz [Azure portalı Hızlı Başlangıç][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
