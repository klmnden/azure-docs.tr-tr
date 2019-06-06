---
title: Azure Kubernetes Service (AKS) kubeconfig'i denetleyin erişimi sınırlayın
description: Küme yöneticileri ve küme kullanıcılar için Kubernetes yapılandırma dosyası (kubeconfig'i denetleyin) erişimi denetleme hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: b55cc226cfbb462cdccd73b3b80cfb0d56c10711
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475601"
---
# <a name="use-azure-role-based-access-controls-to-define-access-to-the-kubernetes-configuration-file-in-azure-kubernetes-service-aks"></a>Azure rol tabanlı erişim denetimleri Kubernetes yapılandırma dosyasının Azure Kubernetes Service (AKS) erişim tanımlamak için kullanın

Kullanarak Kubernetes kümeleriyle etkileşim kurma `kubectl` aracı. Azure CLI'yı erişim kimlik bilgilerini almak için kolay bir yol sağlar ve, AKS için bağlanmak için yapılandırma bilgileri kümeleri kullanarak `kubectl`. Kubernetes yapılandırma alabilirsiniz sınırla (*kubeconfig'i denetleyin*) bilgilerini ve ardından sahip izinleri sınırlamak için Azure rol tabanlı erişim denetimleri (RBAC) kullanabilirsiniz.

Bu makalede bir AKS kümesi yapılandırma bilgilerini alabilirsiniz bu sınırı RBAC rollerini atama işlemini gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Bu makalede, ayrıca Azure CLI Sürüm 2.0.65 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="available-cluster-roles-permissions"></a>Kullanılabilir küme rol izinleri

Ne zaman etkileşim kullanarak bir AKS kümesi `kubectl` aracı, yapılandırma dosyasının küme bağlantı bilgileri tanımlayan kullanılır. Bu yapılandırma dosyası genellikle depolanan *~/.kube/config*. Birden fazla küme bu tanımlanabilir *kubeconfig'i denetleyin* dosya. Kullanarak küme arasında geçiş [kubectl config kullanım bağlam] [ kubectl-config-use-context] komutu.

[Az aks get-credentials] [ az-aks-get-credentials] komut bir AKS kümesi için erişim kimlik bilgilerini almanıza olanak tanır ve bunları birleştirir *kubeconfig'i denetleyin* dosya. Azure rol tabanlı erişim denetimleri (RBAC), bu kimlik bilgileri erişimi denetlemek için kullanabilirsiniz. Bu Azure RBAC rolleri kimin alabilirsiniz tanımlamanıza olanak sağlar *kubeconfig'i denetleyin* dosya ve hangi ardından sahip oldukları küme içinde izinleri.

İki yerleşik roller bulunmaktadır:

* **Azure Kubernetes hizmeti Küme Yöneticisi rolü**  
    * Erişime izin *Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action* API çağrısı. Bu API çağrısı [Küme Yöneticisi kimlik bilgileri listeler][api-cluster-admin].
    * İndirmeler *kubeconfig'i denetleyin* için *clusterAdmin* rol.
* **Azure Kubernetes hizmeti küme kullanıcı rolü**
    * Erişime izin *Microsoft.ContainerService/managedClusters/listClusterUserCredential/action* API çağrısı. Bu API çağrısı [küme kullanıcı kimlik bilgilerini listeler][api-cluster-user].
    * İndirmeler *kubeconfig'i denetleyin* için *clusterUser* rol.

Bu RBAC rolleri, bir Azure Active Directory (AD) kullanıcı veya gruba uygulanabilir.

## <a name="assign-role-permissions-to-a-user-or-group"></a>Bir kullanıcı veya grup rolü izinleri atama

Kullanılabilir rollerden biri atamak için kaynak Kimliğini AKS kümesi ve Azure AD kullanıcı hesabı veya grup Kimliğini almanız gerekir. Aşağıdaki örnek komutlar:

* Küme kaynak Kimliğini kullanarak alma [az aks show] [ az-aks-show] adlı Küme için komutu *myAKSCluster* içinde *myResourceGroup* kaynak grubu. Gerektiğinde kendi küme ve kaynak grubu adı belirtin.
* Kullanan [az hesabı show] [ az-account-show] ve [az ad kullanıcı show] [ az-ad-user-show] kullanıcı kimliğinizi almak için komutları
* Son olarak, bir rolü kullanarak atar [az rol ataması oluşturma] [ az-role-assignment-create] komutu.

Aşağıdaki örnek atar *Azure Kubernetes hizmeti Küme Yöneticisi rolüne* bireysel bir kullanıcı hesabı için:

```azurecli-interactive
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
ACCOUNT_UPN=$(az account show --query user.name -o tsv)
ACCOUNT_ID=$(az ad user show --upn-or-object-id $ACCOUNT_UPN --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $ACCOUNT_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"
```

> [!TIP]
> Azure AD grubu için izinleri atamak istiyorsanız, güncelleştirme `--assignee` parametresi için nesne Kimliğine sahip bir önceki örnekte gösterilen *grubu* yerine *kullanıcı*. Bir grubun nesne Kimliğini almak için kullanın [az ad Grup show] [ az-ad-group-show] komutu. Aşağıdaki örnekte adlı bir Azure AD grubu nesne kimliği alır *appdev*: `az ad group show --group appdev --query objectId -o tsv`

Önceki atama için değiştirebileceğiniz *küme kullanıcı rolünü* gerektiğinde.

Rol ataması başarıyla oluşturuldu, aşağıdaki örnek çıktı gösterilmektedir:

```
{
  "canDelegate": null,
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/providers/Microsoft.Authorization/roleAssignments/b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "name": "b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "principalId": "946016dd-9362-4183-b17d-4c416d1f8f61",
  "resourceGroup": "myResourceGroup",
  "roleDefinitionId": "/subscriptions/<guid>/providers/Microsoft.Authorization/roleDefinitions/0ab01a8-8aac-4efd-b8c2-3ee1fb270be8",
  "scope": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-and-verify-the-configuration-information"></a>Alma ve yapılandırma bilgilerini doğrulayın

Atanan RBAC rolleri ile [az aks get-credentials] [ az-aks-get-credentials] almak için komut *kubeconfig'i denetleyin* AKS kümenizin tanımı. Aşağıdaki örnekte *--yönetici* kullanıcı verilmişse, düzgün çalışması kimlik bilgilerini *Küme Yöneticisi rolüne*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Ardından [kubectl config görünümü] [ kubectl-config-view] doğrulamak için komut *bağlam* Küme Yöneticisi yapılandırma bilgileri uygulandığını gösterir:

```
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://myaksclust-myresourcegroup-19da35-4839be06.hcp.eastus.azmk8s.io:443
  name: myAKSCluster
contexts:
- context:
    cluster: myAKSCluster
    user: clusterAdmin_myResourceGroup_myAKSCluster
  name: myAKSCluster-admin
current-context: myAKSCluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_myResourceGroup_myAKSCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: e9f2f819a4496538b02cefff94e61d35
```

## <a name="remove-role-permissions"></a>Rol izinleri Kaldır

Rol atamalarını kaldırmak için [az rol atamasını Sil] [ az-role-assignment-delete] komutu. Hesap Kimliği ve küme kaynağı kimliği, önceki komutlarda elde edilmiş olarak belirtin. Uygun grup nesne kimliği yerine hesap nesnesi kimliği için kullanıcı yerine bir grup rolü atanmışsa belirtin `--assignee` parametresi:

```azurecli-interactive
az role assignment delete --assignee $ACCOUNT_ID --scope $AKS_CLUSTER
```

## <a name="next-steps"></a>Sonraki adımlar

Gelişmiş Güvenlik AKS kümelerine erişim [Azure Active Directory kimlik doğrulamasını tümleştirmek][aad-integration].

<!-- LINKS - external -->
[kubectl-config-use-context]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config
[kubectl-config-view]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[api-cluster-admin]: /rest/api/aks/managedclusters/listclusteradmincredentials
[api-cluster-user]: /rest/api/aks/managedclusters/listclusterusercredentials
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-role-assignment-delete]: /cli/azure/role/assignment#az-role-assignment-delete
[aad-integration]: azure-ad-integration.md
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
