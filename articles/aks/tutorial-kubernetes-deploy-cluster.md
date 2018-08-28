---
title: Azure’da Kubernetes öğreticisi - Kümeyi dağıtma
description: Bu Azure Kubernetes Service (AKS) öğreticisinde bir AKS kümesi oluşturacak ve kubectl istemcisini kullanarak Kubernetes ana düğümüne bağlanacaksınız.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 80b011f9df389098095f58c02008da891b2aa8a7
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "41917602"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Hızlı Başlangıç: Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. AKS ile üretim için hazır bir Kubernetes kümesini hızlıca sağlayabilirsiniz. Yedi parçalık bu öğreticinin üçüncü kısmında, AKS içinde bir Kubernetes kümesi dağıtılır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak etkileşimleri için hizmet sorumlusu oluşturma
> * Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * kubectl istemcisini AKS kümenize bağlanacak şekilde yapılandırma

Sonraki öğreticilerde Azure Vote uygulaması kümeye dağıtılacak, ölçeklendirecek ve güncelleştirilecektir.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir kapsayıcı görüntüsü oluşturuldu ve Azure Container Registry örneğine yüklendi. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

Bu öğretici için Azure CLI 2.0.44 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Bu hizmet sorumlusu Azure CLI veya portal ile otomatik olarak oluşturulabilir veya kendiniz önceden bir tane oluşturup ek izinler atayabilirsiniz. Bu öğreticide bir hizmet sorumlusu oluşturacak, önceki öğreticide oluşturulan Azure Container Registry (ACR) örneğine erişim verecek ve ardından bir AKS kümesi oluşturacaksınız.

[az ad sp create-for-rbac][] komutunu kullanarak bir hizmet sorumlusu oluşturun. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzer:

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

*appId* ve *password* değerlerini not edin. Bu değerler aşağıdaki adımlarda kullanılacaktır.

## <a name="configure-acr-authentication"></a>ACR kimlik doğrulamasını yapılandırma

ACR'de depolanan görüntülere erişmek için AKS hizmet sorumlusuna ACR'den görüntüleri almak için doğru hakları vermeniz gerekir.

İlk olarak [az acr show][] komutunu kullanarak ACR kaynağı kimliğini alın. `<acrName>` kayıt defteri adını ACR örneğinin adıyla ve ACR örneğinizin bulunduğu kaynak grubuyla güncelleştirin.

```azurecli
az acr show --resource-group myResourceGroup --name <acrName> --query "id" --output tsv
```

AKS kümesine ACR içinde depolanan görüntüleri kullanmak üzere doğru erişimi vermek için [az role assignment create][] komutunu kullanarak bir rol ataması oluşturun. `<appId`> ve `<acrId>` yerine önceki iki adımda topladığınız değerleri yazın.

```azurecli
az role assignment create --assignee <appId> --scope <acrId> --role Reader
```

## <a name="create-a-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

AKS kümeleri Kubernetes rol tabanlı erişim denetimlerini (RBAC) kullanabilir. Bu denetimler, kullanıcılara atanmış olan rollere göre kaynaklara erişim vermenizi sağlayabilir. Bir kullanıcıya birden fazla rolün atanmış olması durumunda izinler birleştirilebilir ve ayrıca izinler için tek bir ad alanı veya kümenin tamamı gibi kapsamlar belirlenebilir. Kubernetes RBAC şu anda AKS kümeleri için önizleme sürümündedir. Bir AKS kümesi oluşturduğunuzda Azure CLI varsayılan ayarlarda RBAC özelliğini otomatik olarak etkinleştirir.

[az aks create][] komutunu kullanarak bir AKS kümesi oluşturun. Aşağıdaki örnek, *myResourceGroup* adlı kaynak grubunda *myAKSCluster* adlı bir küme oluşturur. Bu kaynak grubu, [bir önceki öğreticide][aks-tutorial-prepare-acr] oluşturulmuştur. `<appId>` ve `<password>` yerine hizmet sorumlusunun oluşturulduğu bir önceki adımdan aldığınız değerlerinizi girin.

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --service-principal <appId> \
    --client-secret <password> \
    --generate-ssh-keys
```

Birkaç dakika sonra dağıtım tamamlanır ve AKS dağıtımı hakkında JSON tarafından biçimlendirilmiş bilgiler döndürür.

## <a name="install-the-kubernetes-cli"></a>Kubernetes CLI'yi yükleme

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanmanız gerekir.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

## <a name="connect-to-cluster-using-kubectl"></a>kubectl istemcisini kullanarak kümeye bağlanma

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][] komutunu kullanın. Aşağıdaki örnek *myResourceGroup* içindeki *myAKSCluster* adlı AKS kümesinin kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize yönelik bağlantıyı doğrulamak için [kubectl get nodes][kubectl-get] komutunu çalıştırın:

```
$ kubectl get nodes

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-66427764-0   Ready     agent     9m        v1.9.9
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide AKS'de bir Kubernetes kümesi dağıttınız ve `kubectl` istemcisini ona bağlanacak şekilde yapılandırdınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kaynak etkileşimleri için hizmet sorumlusu oluşturma
> * Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * kubectl istemcisini AKS kümenize bağlanacak şekilde yapılandırma

Kümeye uygulama dağıtmayı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes'te uygulama dağıtma][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-cli-install]: /cli/azure/install-azure-cli
