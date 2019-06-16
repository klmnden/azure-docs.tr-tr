---
title: Azure Kubernetes Service’in (AKS) hizmet sorumluları
description: Azure Kubernetes Service’te bir küme için Azure Active Directory hizmet sorumlusu oluşturma ve yönetme
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: iainfou
ms.openlocfilehash: 82ceb332ca377da1953908abba3f7c52874b995e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066800"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) ile hizmet sorumluları

Bir AKS kümesi, Azure API’leri ile etkileşim kurmak için [Azure Active Directory (AD) hizmet sorumlusu][aad-service-principal] gerektirir. Azure yük dengeleyicisi veya kapsayıcı kayıt defteri (ACR) gibi diğer Azure kaynaklarını dinamik olarak oluşturmak ve yönetmek için hizmet sorumlusuna ihtiyaç vardır.

Bu makalede, AKS kümeleriniz için bir hizmet sorumlusunun nasıl oluşturulacağı ve kullanılacağı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure AD hizmet sorumlusu oluşturmak için, Azure AD kiracınızla bir uygulamayı kaydetme ve uygulamanızı aboneliğinizdeki bir role atama izinlerinizin olması gerekir. Gerekli izinlere sahip değilseniz Azure AD veya abonelik yöneticinizden gerekli izinleri atamasını istemeniz veya AKS kümesiyle kullanmanız için bir hizmet sorumlusu oluşturma ön işlemlerini tamamlamanız gerekebilir.

Bir hizmet sorumlusunu kullanıyorsanız farklı bir Azure AD Kiracı, kullanılabilir izinleri etrafında ek hususlar küme dağıttığınızda. Okuma ve yazma dizin bilgileri için uygun izinlere sahip değil. Daha fazla bilgi için [varsayılan kullanıcı izinleri Azure Active Directory nelerdir?][azure-ad-permissions]

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Otomatik olarak bir hizmet sorumlusu oluşturma ve kullanma

Azure portalda bir AKS kümesi oluşturduğunuzda veya [az aks create][az-aks-create] komutunu kullanarak, Azure otomatik olarak bir hizmet sorumlusu oluşturabilir.

Aşağıdaki Azure CLI örneğinde bir hizmet sorumlusu belirtilmemiştir. Bu senaryoda Azure CLI, AKS kümesi için bir hizmet sorumlusu oluşturur. İşlemi başarıyla tamamlamak için Azure hesabınızın gerekli hizmet sorumlusu oluşturma haklarına sahip olması gerekir.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup
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
  "displayName": "azure-cli-2019-03-04-21-35-28",
  "name": "http://azure-cli-2019-03-04-21-35-28",
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

Hizmet sorumlusu AKS kümesi, diğer kaynaklarına erişmek için kullanılabilir. Örneğin, mevcut bir Azure sanal ağ alt ağı, AKS kümesi dağıtmayı veya Azure Container Registry (ACR) bağlanmak istiyorsanız, hizmet sorumlusu için bu kaynaklara temsilci erişimi gerekir.

Kullanarak bir rol atama izinleri için temsilci seçme için oluşturma [az rol ataması oluşturma] [ az-role-assignment-create] komutu. Ata `appId` bir kaynak grubu ya da sanal ağ kaynağı gibi belirli bir kapsamda. Bir rol, ardından aşağıdaki örnekte gösterildiği gibi kaynak, hizmet sorumlusu sahip izinleri tanımlar:

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
  - *Microsoft.Network/virtualNetworks/subnets/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
- Veya atama [ağ Katılımcısı] [ rbac-network-contributor] sanal ağ içindeki alt ağ üzerinde yerleşik rol

### <a name="storage"></a>Depolama

Başka bir kaynak grubunda mevcut Disk kaynaklarına erişmek gerekebilir. Rol izinleri aşağıdaki kümesinin atayın:

- Oluşturma bir [özel rol] [ rbac-custom-role] ve aşağıdaki rol izinleri tanımlayın:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- Veya atama [depolama hesabı Katılımcısı] [ rbac-storage-contributor] kaynak grubunda yerleşik rol

### <a name="azure-container-instances"></a>Azure Container Instances

AKS ile tümleştirin ve kaynak grubunda AKS kümesi için ayrı Azure Container Instances'a (ACI) çalıştırmayı tercih Virtual Kubelet kullanırsanız, hizmet sorumlusu AKS verilmelidir *katkıda bulunan* ACI kaynakta izinleri Grup.

## <a name="additional-considerations"></a>Diğer konular

AKS ve Azure AD hizmet sorumlularını kullanılırken aşağıdaki noktalara dikkat edin.

- Kubernetes için hizmet sorumlusu, küme yapılandırmasının bir parçasıdır. Ancak, kümeyi dağıtmak için kimlik kullanmayın.
- Varsayılan olarak, hizmet sorumlusu kimlik bilgileri bir yıl süreyle geçerlidir. Yapabilecekleriniz [güncelleştirme veya hizmet sorumlusu kimlik bilgilerini döndürme] [ update-credentials] dilediğiniz zaman.
- Her hizmet sorumlusunun bir Azure AD uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure AD uygulama adıyla ilişkilendirilebilir (örneğin: *https://www.contoso.org/example* ). Uygulama URL'sinin gerçek bir uç nokta olması gerekmez.
- Hizmet sorumlusu **İstemci kimliğini** belirttiğinizde `appId` değerini kullanın.
- Aracı düğümde Vm'leri Kubernetes kümesinde hizmet sorumlusu kimlik bilgileri dosyasında depolanır. `/etc/kubernetes/azure.json`
- Hizmet sorumlusunu otomatik olarak oluşturmak için [az aks create][az-aks-create] komutunu kullandığınızda, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı makinede `~/.azure/aksServicePrincipal.json` dosyasına yazılır.
- [az aks create][az-aks-create] komutu tarafından oluşturulan bir AKS kümesini sildiğinizde, otomatik olarak oluşturulan hizmet sorumlusu silinmez.
    - Sorgu kümeniz için hizmet sorumlusu silmek için *servicePrincipalProfile.clientId* ve delete ile [az ad app delete][az-ad-app-delete]. Aşağıdaki kaynak grubu ve küme adlarını kendi değerlerinizle değiştirin:

        ```azurecli
        az ad sp delete --id $(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
        ```

## <a name="troubleshoot"></a>Sorun giderme

Azure CLI ile bir AKS kümesi için hizmet sorumlusu kimlik bilgileri önbelleğe alınır. Bu kimlik bilgilerinin kullanım süresi dolduğunda, AKS kümelerini dağıtma hatalarla karşılaşırsınız. Çalıştırırken aşağıdaki hata iletisini [az aks oluşturma] [ az-aks-create] önbelleğe alınan hizmet sorumlusu kimlik bilgileri bir sorun olduğunu gösteriyor olabilir:

```console
Operation failed with status: 'Bad Request'.
Details: The credentials in ServicePrincipalProfile were invalid. Please see https://aka.ms/aks-sp-help for more details.
(Details: adal: Refresh request failed. Status Code = '401'.
```

Aşağıdaki komutu kullanarak kimlik bilgileri dosyası yaşını denetleyin:

```console
ls -la $HOME/.azure/aksServicePrincipal.json
```

Hizmet sorumlusu kimlik bilgileri için varsayılan zaman aşımı süresi bir yıldır. Varsa, *aksServicePrincipal.json* dosyasıdır bir yıldan eski, dosyayı silin ve yeniden bir AKS kümesi dağıtmayı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory Hizmet sorumluları hakkında daha fazla bilgi için bkz. [uygulama ve hizmet sorumlusu nesneleri][service-principal].

Kimlik bilgilerini güncelleştirme hakkında daha fazla bilgi için bkz: [güncelleştirin veya aks'deki bir hizmet sorumlusu kimlik bilgilerini döndürme][update-credentials].

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
[update-credentials]: update-credentials.md
[azure-ad-permissions]: ../active-directory/fundamentals/users-default-permissions.md
