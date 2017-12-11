---
title: "Azure Kubernetes kümesi için hizmet sorumlusu"
description: "AKS içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu oluşturma ve yönetme"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a217f4cc8ac18888de8dfa803b4b8667a566dc0b
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Azure Container Service (AKS) ile hizmet sorumluları

AKS kümesi, Azure API'leri ile etkileşime geçmek için [Azure Active Directory hizmet sorumlusu](../active-directory/develop/active-directory-application-objects.md) gerektirir. Hizmet sorumlusu, [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) ve [4. Katman Azure Load Balancer](../load-balancer/load-balancer-overview.md) gibi kaynakları dinamik olarak yönetmek için gereklidir.

Bu makalede AKS'deki Kubernetes kümeniz için hizmet sorumlusu ayarlamak üzere kullanabileceğiniz farklı seçenekler gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce


Azure AD hizmet sorumlusu oluşturmak için, Azure AD kiracınızla bir uygulamayı kaydetme ve uygulamanızı aboneliğinizdeki bir role atama izinlerinizin olması gerekir. Gerekli izinlere sahip değilseniz Azure AD veya abonelik yöneticinizden gerekli izinleri atamasını istemeniz veya Kubernetes kümesi için bir hizmet sorumlusu oluşturma ön işlemlerini tamamlamanız gerekebilir.

Ayrıca Azure CLI sürüm 2.0.21 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-sp-with-aks-cluster"></a>AKS kümesi ile hizmet sorumlusu oluşturma

`az aks create` komutuyla bir AKS kümesi dağıtırken otomatik olarak bir hizmet sorumlusu oluşturma seçeneğine sahip olursunuz.

Aşağıdaki örnekte bir AKS kümesi oluşturulmakta ve var olan bir hizmet sorumlusu belirtilmediğinden küme için bir hizmet sorumlusu oluşturulmaktadır. Bu işlemi tamamlamak için hesabınızın gerekli hizmet sorumlusu oluşturma haklarına sahip olması gerekir.

```azurecli
az aks create --name myK8SCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Var olan bir hizmet sorumlusunu kullanma

Var olan bir Azure AD hizmet sorumlusu AKS kümesiyle kullanılabilir veya kullanılmak üzere ön oluşturma işlemleri gerçekleştirilebilir. Bu durum hizmet sorumlusu bilgilerini belirtmeniz gereken Azure portalı küme dağıtım süreçleri için faydalıdır. Mevcut bir hizmet sorumlusunu kullanırken, istemci gizli anahtarının bir parola olarak yapılandırılması gerekir.

## <a name="pre-create-a-new-sp"></a>Yeni bir hizmet sorumlusunu önceden oluşturma

Hizmet sorumlusunu Azure CLI ile oluşturmak için [az ad sp create-for-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) komutunu kullanın.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdakine benzer olacaktır. `appId` ve `password` değerlerini not edin. Bu değerler AKS kümesi oluşturulurken kullanılır.

```json
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Var olan bir hizmet sorumlusunu kullanma

Önceden oluşturulmuş bir hizmet sorumlusunu kullanırken `appId` ve `password` değerlerini `az aks create` komutunda bağımsız değişken değerleri olarak girin.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8SCluster --service-principal <appId> --client-secret <password>
```

Azure portalını kullanarak bir AKS kümesi dağıtıyorsanız, **hizmet sorumlusu istemci kimliği** alanındaki `appId` değerini ve AKS küme yapılandırması formunda yer alan **hizmet sorumlusu gizli anahtarı** alanındaki `password` değerini girin.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Diğer konular

AKS ve Azure AD hizmet sorumlularıyla çalışırken aşağıdaki noktalara dikkat edin.

* Kubernetes için hizmet sorumlusu, küme yapılandırmasının bir parçasıdır. Ancak, kümeyi dağıtmak için kimlik kullanmayın.
* Her hizmet sorumlusunun bir Azure AD uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure AD uygulama adıyla ilişkilendirilebilir (örneğin: `https://www.contoso.org/example`). Uygulama URL'sinin gerçek bir uç nokta olması gerekmez.
* Hizmet sorumlusu **İstemci Kimliğini** belirtirken `appId` değerini (bu makalede gösterilen şekilde) veya karşılık gelen hizmet sorumlusu `name` değerini (örneğin, `https://www.contoso.org/example`) kullanabilirsiniz.
* Kubernetes kümesindeki ana VM’lerde ve düğüm VM'lerinde hizmet sorumlusu kimlik bilgileri, `/etc/kubernetes/azure.json` dosyasında depolanır.
* Hizmet sorumlusunu otomatik olarak oluşturmak için `az aks create` komutunu kullandığınızda, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı makinede `~/.azure/acsServicePrincipal.json` dosyasına yazılır.
* `az aks create` komutu ile hizmet sorumlusunu otomatik olarak oluşturduğunuzda, hizmet sorumlusu aynı abonelikte oluşturulan bir [Azure kapsayıcı kayıt defteri](../container-registry/container-registry-intro.md) ile de kimlik doğrulaması yapabilir.
* `az aks create` tarafından oluşturulan bir AKS kümesi silinirken, otomatik olarak oluşturulan hizmet sorumlusu silinmez. Silmek için `az ad sp delete --id $clientID` kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory hizmet sorumluları hakkında daha fazla bilgi için Azure AD uygulamaları belgelerine bakın.

> [!div class="nextstepaction"]
> [Uygulama ve hizmet sorumlusu nesneleri](../active-directory/develop/active-directory-application-objects.md)
