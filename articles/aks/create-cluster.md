---
title: Azure Kubernetes Service (AKS) kümesi oluşturma
description: CLI veya Azure portalı ile bir AKS kümesi oluşturun.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6bfe6f9b76693ded79aa9b9d21ddcac4e1a0733e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43110313"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesi oluşturma

Azure Kubernetes Service (AKS) kümesini Azure CLI veya Azure portalı ile oluşturulabilir.

## <a name="azure-cli"></a>Azure CLI

Kullanım [az aks oluşturma] [ az-aks-create] AKS kümesi oluşturmak için komutu.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Aşağıdaki seçenekler kullanılabilen `az aks create` komutu. Bkz: [Azure CLI başvurusu] [ az-aks-create] AKS bu değişkenin her biri hakkında daha fazla bilgi için.

| Bağımsız değişken | Açıklama | Gerekli |
|---|---|:---:|
| `--name` `-n` | Yönetilen küme kaynağı adı. | evet |
| `--resource-group` `-g` | Azure Kubernetes hizmeti kaynak grubunun adı. | evet |
| `--admin-username` `-u` | Kullanıcı adı için Linux sanal makineleri.  Varsayılan: azureuser. | hayır |
| `--aad-client-app-id` | (ÖNİZLEME) "Yerel" türünde bir Azure Active Directory istemci uygulaması kimliği. | hayır |
| `--aad-server-app-id` | (ÖNİZLEME) "Web uygulaması/API'si" türünde bir Azure Active Directory sunucu uygulama kimliği. | hayır |
| `--aad-server-app-secret` | (ÖNİZLEME) Bir Azure Active Directory sunucu uygulaması gizli anahtarı. | hayır |
| `--aad-tenant-id` | (ÖNİZLEME) Bir Azure Active Directory kiracısının kimliği. | hayır |
| `--admin-username` `-u` | Düğüm VM'ler için SSH erişimini oluşturmak için kullanıcı hesabı'nı tıklatın.  Varsayılan: azureuser. | hayır |
| ` --client-secret` | Hizmet sorumlusuyla ilişkili gizli dizi. | hayır |
| `--dns-name-prefix` `-p` | Kümeleri genel IP adresi için DNS öneki. | hayır |
| `--dns-service-ip` | Kubernetes DNS hizmetine atanan bir IP adresi. | hayır |
| `--docker-bridge-address` | Bir IP adresi ve Docker köprüsüne atanan ağ maskesi. | hayır |
| `--enable-addons` `-a` | Virgülle ayrılmış bir liste olarak Kubernetes eklentiler sağlar. | hayır |
| `--enable-rbac` `-r` | Kubernetes rol tabanlı erişim denetimini etkinleştirin. | hayır |
| `--generate-ssh-keys` | SSH ortak ve özel anahtar dosyaları eksikse oluşturur. | hayır |
| `--kubernetes-version` `-k` | '1.7.9' veya '1.9.6' gibi bir küme oluşturmak için kullanılacak Kubernetes sürümü. | hayır |
| `--location` `-l` | Otomatik olarak oluşturulmuş bir kaynak grubu konumu | hayır |
| `--max-pods` `-m` | Pod'ların bir düğüme dağıtılabilir maksimum sayısı. | hayır |
| `--network-plugin` | Kubernetes eklentisini kullanmak için ağ. | hayır |
| `--no-ssh-key` `-x` | Kullanmayın veya yerel bir SSH anahtarı oluşturun. | hayır |
| `--no-wait` | Uzun süre çalışan işlemin tamamlanmasını bekleyin değil. | hayır |
| `--node-count` `-c` | Düğüm düğüm havuzları için varsayılan sayısı.  Varsayılan: 3. | hayır |
| `--node-osdisk-size` | Düğüm havuzu sanal makinesi GB osDisk boyutu. | hayır |
| `--node-vm-size` `-s` | Sanal makine boyutu.  Varsayılan: işler için standart_d1_v2. | hayır |
| `--pod-cidr` | Bir CIDR notasyonu IP aralığı kubernetes kullanıldığında pod IP'ler atanacak. | hayır |
| `--service-cidr` | Bir CIDR notasyonu IP aralığı hizmet kümesi IP'leri atanacak. | hayır |
| `--service-principal` | Hizmet sorumlusu, küme kimlik doğrulaması için kullanılır. | hayır |
| `--ssh-key-value` | SSH anahtar dosyası değer veya anahtar dosya yolu.  Varsayılan: ~ /.ssh/id_rsa.pub. | hayır |
| `--tags` | Boşluk ayrılmış etiketler 'anahtarındaki [= değer]' biçimi. Kullanım '' mevcut etiketleri temizleyin. | hayır |
| `--vnet-subnet-id` | Hangi kümeyi dağıtmak mevcut bir VNet içindeki alt ağ kimliği. | hayır |
| `--workspace-resource-id` | İzleme verilerini depolamak için kullanılacak mevcut bir Log Analytics çalışma kaynak kimliği. | hayır |

## <a name="azure-portal"></a>Azure portal

Azure Kubernetes Service (AKS) Azure portalı ile bir AKS kümesi dağıtma hakkında yönergeler için bkz. [Azure portalı hızlı başlangıcı][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
