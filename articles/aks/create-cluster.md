---
title: "Bir Azure kapsayıcı hizmeti (AKS) kümesi oluşturma"
description: "CLI veya Azure portal ile bir AKS kümesi oluşturun."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3e884bc16680d74801911547045deb48246afccd
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="create-an-azure-container-service-aks-cluster"></a>Bir Azure kapsayıcı hizmeti (AKS) kümesi oluşturma

Azure CLI veya Azure portal ile Azure kapsayıcı hizmeti (AKS) küme oluşturulabilir.

## <a name="azure-cli"></a>Azure CLI

Kullanım [az aks oluşturma] [ az-aks-create] AKS kümesini silmek için komutu.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Aşağıdaki seçenekler ile kullanılabilen `az aks create` komutu.

| Bağımsız değişken | Açıklama | Gerekli |
|---|---|:---:|
| `--name` `-n` | Yönetilen küme kaynağı adı. | evet |
| `--resource-group` `-g` | Azure kapsayıcı hizmeti kaynak grubunun adı. | evet |
| `--admin-username` `-u` | Kullanıcı adı için Linux sanal makineleri.  Varsayılan: azureuser. | hayır |
| ` --client-secret` | Hizmet sorumlusu ilişkili gizli anahtar. | hayır |
| `--dns-name-prefix` `-p` | Kümeleri genel IP adresi için DNS öneki. | hayır |
| `--generate-ssh-keys` | SSH ortak ve özel anahtar dosyaları eksikse oluşturur. | hayır |
| `--kubernetes-version` `-k` | '1.7.9' veya '1.8.2' gibi bir küme oluşturmak için kullanılacak Kubernetes sürümü.  Varsayılan: 1.7.7. | hayır |
| `--no-wait` | Tamamlanması uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--node-count` `-c` | Düğüm düğüm havuzları için varsayılan sayısı.  Varsayılan: 3. | hayır |
| `--node-osdisk-size` | Sanal makine düğümünü havuzunun GB osDisk boyutu. | hayır |
| `--node-vm-size` `-s` | Sanal makine boyutu.  Default: Standard_D1_v2. | hayır |
| `--service-principal` | Küme kimlik doğrulaması için kullanılan hizmet sorumlusu. | hayır |
| `--ssh-key-value` | SSH anahtar dosyası değer veya anahtar dosya yolu.  Varsayılan: ~ /.ssh/id_rsa.pub. | hayır |
| `--tags` | Boşluk [= değer]'key ' etiketleri ayrılan biçimi. Kullanım '' varolan etiketleri temizleyin. | hayır |

## <a name="azure-portal"></a>Azure portalına

Azure kapsayıcı hizmeti (AKS) Azure portal ile bir AKS kümesi dağıtma ile ilgili yönergeler için bkz [Azure portalı Hızlı Başlangıç][aks-portal-quickstart]. 

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md