---
title: Bir Azure Kubernetes hizmet (AKS) kümesi oluşturma
description: CLI veya Azure portal ile bir AKS kümesi oluşturun.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 304f3807a70179e4aab2ede80dc08a1aa85a2e51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098915"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Bir Azure Kubernetes hizmet (AKS) kümesi oluşturma

Azure CLI veya Azure portal ile bir Azure Kubernetes hizmet (AKS) küme oluşturulabilir.

## <a name="azure-cli"></a>Azure CLI

Kullanım [az aks oluşturma] [ az-aks-create] AKS küme oluşturmak için komutu.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Aşağıdaki seçenekler ile kullanılabilen `az aks create` komutu. Bkz: [Azure CLI başvuru] [ az-aks-create] AKS bu değişkenin her biri hakkında daha fazla bilgi için.

| Bağımsız değişken | Açıklama | Gerekli |
|---|---|:---:|
| `--name` `-n` | Yönetilen küme kaynağı adı. | evet |
| `--resource-group` `-g` | Azure Kubernetes hizmeti kaynak grubunun adı. | evet |
| `--admin-username` `-u` | Kullanıcı adı için Linux sanal makineleri.  Varsayılan: azureuser. | hayır |
| `--aad-client-app-id` | (ÖNİZLEME) Bir Azure Active Directory istemci uygulaması türü "Yerel" kimliği. | hayır |
| `--aad-server-app-id` | (ÖNİZLEME) "Web uygulaması/API" türünde bir Azure Active Directory sunucu uygulaması kimliği. | hayır |
| `--aad-server-app-secret` | (ÖNİZLEME) Bir Azure Active Directory sunucu uygulama gizli anahtarı. | hayır |
| `--aad-tenant-id` | (ÖNİZLEME) Bir Azure Active Directory Kiracı kimliği. | hayır |
| `--admin-username` `-u` | SSH erişim VM'ler düğümde oluşturmak için kullanıcı hesabı.  Varsayılan: azureuser. | hayır |
| ` --client-secret` | Hizmet sorumlusuyla ilişkili gizli dizi. | hayır |
| `--dns-name-prefix` `-p` | Kümeleri genel IP adresi için DNS öneki. | hayır |
| `--dns-service-ip` | Kubernetes DNS hizmete atanan bir IP adresi. | hayır |
| `--docker-bridge-address` | Bir IP adresi ve Docker köprüsüne atanan ağ maskesi. | hayır |
| `--enable-addons` `-a` | Virgülle ayrılmış bir listede Kubernetes alabilecekleri etkinleştirin. | hayır |
| `--enable-rbac` `-r` | Kubernetes rol tabanlı erişim Denetimi'ni etkinleştirin. | hayır |
| `--generate-ssh-keys` | SSH ortak ve özel anahtar dosyaları eksikse oluşturur. | hayır |
| `--kubernetes-version` `-k` | '1.7.9' veya '1.9.6' gibi bir küme oluşturmak için kullanılacak Kubernetes sürümü. | hayır |
| `--locaton` `-l` | Otomatik oluşturulan kaynak grubu konumu | hayır |
| `--max-pods` `-m` | Pod'ları bir düğüme dağıtılabilir maksimum sayısı. | hayır |
| `--network-plugin` | Kullanılacak Kubernetes ağ eklentisi. | hayır |
| `--no-ssh-key` `-x` | Kullanmayın veya yerel bir SSH anahtarı oluşturun. | hayır |
| `--no-wait` | Tamamlanması uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--node-count` `-c` | Düğüm düğüm havuzları için varsayılan sayısı.  Varsayılan: 3. | hayır |
| `--node-osdisk-size` | Sanal makine düğümünü havuzunun GB osDisk boyutu. | hayır |
| `--node-vm-size` `-s` | Sanal makine boyutu.  Varsayılan: Standard_D1_v2. | hayır |
| `--pod-cidr` | Bir CIDR gösterimi IP aralığı kubenet kullanıldığında pod IP'leri atanacak. | hayır |
| `--service-cidr` | Bir CIDR gösterimi IP aralığı hizmeti küme IP'leri atanacak. | hayır |
| `--service-principal` | Küme kimlik doğrulaması için kullanılan hizmet sorumlusu. | hayır |
| `--ssh-key-value` | SSH anahtar dosyası değer veya anahtar dosya yolu.  Varsayılan: ~ /.ssh/id_rsa.pub. | hayır |
| `--tags` | Boşluk [= değer]'key ' etiketleri ayrılan biçimi. Kullanım '' varolan etiketleri temizleyin. | hayır |
| `--vnet-subnet-id` | Var olan bir VNet içindeki bir alt küme dağıtmak için içine kimliği. | hayır |
| `--workspace-resource-id` | İzleme verilerini depolamak için kullanmak için mevcut bir günlük analizi çalışma kaynak kimliği. | hayır |

## <a name="azure-portal"></a>Azure portalına

Azure Kubernetes hizmet (AKS) Azure portal ile bir AKS kümesi dağıtma ile ilgili yönergeler için bkz [Azure portalı Hızlı Başlangıç][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
