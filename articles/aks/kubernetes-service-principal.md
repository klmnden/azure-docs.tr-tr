---
title: Azure Kubernetes Service’in (AKS) hizmet sorumluları
description: Azure Kubernetes Service’te bir küme için Azure Active Directory hizmet sorumlusu oluşturma ve yönetme
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 4af4cae07f4e02bc8306c0b317da3a58e4586494
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578358"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) ile hizmet sorumluları

Bir AKS kümesi, Azure API’leri ile etkileşim kurmak için [Azure Active Directory (AD) hizmet sorumlusu][aad-service-principal] gerektirir. Azure yük dengeleyicisi veya kapsayıcı kayıt defteri (ACR) gibi diğer Azure kaynaklarını dinamik olarak oluşturmak ve yönetmek için hizmet sorumlusuna ihtiyaç vardır.

Bu makalede, AKS kümeleriniz için bir hizmet sorumlusunun nasıl oluşturulacağı ve kullanılacağı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure AD hizmet sorumlusu oluşturmak için, Azure AD kiracınızla bir uygulamayı kaydetme ve uygulamanızı aboneliğinizdeki bir role atama izinlerinizin olması gerekir. Gerekli izinlere sahip değilseniz Azure AD veya abonelik yöneticinizden gerekli izinleri atamasını istemeniz veya AKS kümesiyle kullanmanız için bir hizmet sorumlusu oluşturma ön işlemlerini tamamlamanız gerekebilir.

Ayrıca Azure CLI sürüm 2.0.46 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Otomatik olarak bir hizmet sorumlusu oluşturma ve kullanma

Azure portalda bir AKS kümesi oluşturduğunuzda veya [az aks create][az-aks-create] komutunu kullanarak, Azure otomatik olarak bir hizmet sorumlusu oluşturabilir.

Aşağıdaki Azure CLI örneğinde bir hizmet sorumlusu belirtilmemiştir. Bu senaryoda Azure CLI, AKS kümesi için bir hizmet sorumlusu oluşturur. İşlemi başarıyla tamamlamak için Azure hesabınızın gerekli hizmet sorumlusu oluşturma haklarına sahip olması gerekir.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>El ile hizmet sorumlusu oluşturma

El ile hizmet sorumlusunu Azure CLI ile oluşturmak için [az ad sp create-for-rbac][az-ad-sp-create] komutunu kullanın. Aşağıdaki örnekte, `--skip-assignment` parametresi, ek varsayılan atamaların atanmasını engellemektedir:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzerdir. Kendi `appId` ve `password`’lerinizi not edin. Bu değerler bir sonraki bölümde AKS kümesi oluşturduğunuzda kullanılır.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Bir AKS kümesi için hizmet sorumlusu belirtin

[az aks create][az-aks-create] komutunu kullanarak bir AKS kümesi oluşturduğunuzda var olan bir hizmet sorumlusunu kullanmak üzere, [az ad sp create-for-rbac][az-ad-sp-create] komutunun çıktısından `appId` ve `password`’i belirtmek için `--service-principal` ve `--client-secret` parametrelerini kullanın:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Azure portalı kullanarak bir AKS kümesi dağıtırsanız, **Kubernetes kümesi oluştur** iletişim kutusunun *Kimlik doğrulaması* sayfasında **Hizmet sorumlusu yapılandır**’ı seçin. **Var olanı kullan**’ı seçin ve aşağıdaki değerleri belirtin:

- **Hizmet sorumlusu istemci kimliği**, *appId*’nizdir
- **Hizmet sorumlusu istemci parolası**, *parola* değeridir

![Azure Vote’a göz atma görüntüsü](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="delegate-access-to-other-azure-resources"></a>Diğer Azure kaynakları için temsilci erişimi

Hizmet sorumlusu AKS kümesi, diğer kaynaklarına erişmek için kullanılabilir. Örneğin, Gelişmiş ağ için var olan sanal ağları birbirine bağlama veya Azure Container Registry (ACR) bağlanmak için kullanmak istiyorsanız, hizmet sorumlusu erişimi devretmek gerekir.

Temsilci izinleri için rol atama kullanarak oluşturduğunuz [az rol ataması oluşturma] [ az-role-assignment-create] komutu. Atadığınız `appId` bir kaynak grubu ya da sanal ağ kaynağı gibi belirli bir kapsamda. Bir rol, ardından aşağıdaki örnekte gösterildiği gibi kaynak, hizmet sorumlusu sahip izinleri tanımlar:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

`--scope` Bir tam kaynak kimliği gibi olacak şekilde bir kaynak ihtiyaçları için */subscriptions/\<GUID\>/resourceGroups/myResourceGroup* veya */subscriptions/\<GUID \>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet*

Aşağıdaki bölümlerde yapmanız gerekebilir genel temsilcileri ayrıntılı olarak açıklanmaktadır.

### <a name="azure-container-registry"></a>Azure Container Registry

Kapsayıcı görüntüsünü deponuz olarak Azure Container Registry (ACR) kullanırsanız, okumak ve görüntüleri çekmek AKS kümenizin izinleri vermeniz gerekir. Hizmet sorumlusu AKS kümesi, temsilci *okuyucu* kayıt defterindeki rol. Ayrıntılı adımlar için bkz. [ACR erişimi verme AKS][aks-to-acr].

### <a name="networking"></a>Ağ

Gelişmiş Ağ, sanal ağ ve alt ağı veya genel IP adresleri başka bir kaynak grubunda olduğu kullanabilir. Rol izinleri aşağıdaki kümesinin atayın:

- Oluşturma bir [özel rol] [ rbac-custom-role] ve aşağıdaki rol izinleri tanımlayın:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
- Veya atama [ağ Katılımcısı] [ rbac-network-contributor] sanal ağ içindeki alt ağ üzerinde yerleşik rol

### <a name="storage"></a>Depolama

Başka bir kaynak grubunda mevcut Disk kaynaklarına erişmek gerekebilir. Rol izinleri aşağıdaki kümesinin atayın:

- Oluşturma bir [özel rol] [ rbac-custom-role] ve aşağıdaki rol izinleri tanımlayın:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- Veya atama [depolama hesabı Katılımcısı] [ rbac-storage-contributor] kaynak grubunda yerleşik rol

## <a name="additional-considerations"></a>Diğer konular

AKS ve Azure AD hizmet sorumlularını kullanılırken aşağıdaki noktalara dikkat edin.

- Kubernetes için hizmet sorumlusu, küme yapılandırmasının bir parçasıdır. Ancak, kümeyi dağıtmak için kimlik kullanmayın.
- Her hizmet sorumlusunun bir Azure AD uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure AD uygulama adıyla ilişkilendirilebilir (örneğin: *https://www.contoso.org/example*). Uygulama URL'sinin gerçek bir uç nokta olması gerekmez.
- Hizmet sorumlusu **İstemci kimliğini** belirttiğinizde `appId` değerini kullanın.
- Kubernetes kümesinin ana ve düğüm VM’lerinde, hizmet sorumlusu kimlik bilgileri `/etc/kubernetes/azure.json` dosyasında saklanır
- Hizmet sorumlusunu otomatik olarak oluşturmak için [az aks create][az-aks-create] komutunu kullandığınızda, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı makinede `~/.azure/aksServicePrincipal.json` dosyasına yazılır.
- [az aks create][az-aks-create] komutu tarafından oluşturulan bir AKS kümesini sildiğinizde, otomatik olarak oluşturulan hizmet sorumlusu silinmez.
    - Hizmet sorumlusunu silmek için, önce [az ad app list][az-ad-app-list]komutuyla hizmet sorumlusunun kimliğini alın. Aşağıdaki örnekte, *myAKSCluster* adlı küme sorgulanır ve [az ad app delete][az-ad-app-delete] ile uygulama kimliği silinir. Şu adları kendi değerlerinizle değiştirin:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory hizmet sorumluları hakkında daha fazla bilgi için, bkz. [Uygulama ve hizmet sorumlusu nesneleri][service-principal]

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
[rbac-network-contributor]: ../role-based-access-control/built-in-roles.md#network-contributor
[rbac-custom-role]: ../role-based-access-control/custom-roles.md
[rbac-storage-contributor]: ../role-based-access-control/built-in-roles.md#storage-account-contributor
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[aks-to-acr]: ../container-registry/container-registry-auth-aks.md?toc=%2fazure%2faks%2ftoc.json#grant-aks-access-to-acr
