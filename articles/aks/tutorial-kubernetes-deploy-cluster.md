---
title: Azure’da Kubernetes öğreticisi - Kümeyi Dağıtma
description: AKS öğreticisi - Kümeyi Dağıtma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8698f16138e9baeb9c9c1142a5d0c8937a69d1b
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341408"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Hızlı Başlangıç: Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. AKS ile üretim için hazır bir Kubernetes kümesini hızlıca sağlayabilirsiniz. Yedi parçalık bu öğreticinin üçüncü kısmında, AKS içinde bir Kubernetes kümesi dağıtılır. Tamamlanan adımlar:

> [!div class="checklist"]
> * Kaynak etkileşimleri için hizmet sorumlusu oluşturma
> * Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yüklemesi
> * Kubectl yapılandırması

Sonraki öğreticilerde Azure Vote uygulaması kümeye dağıtılacak, ölçeklendirecek ve güncelleştirilecektir.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir kapsayıcı görüntüsü oluşturuldu ve Azure Container Registry örneğine yüklendi. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Bu hizmet sorumlusu Azure CLI veya portal ile otomatik olarak oluşturulabilir veya kendiniz önceden bir tane oluşturup ek izinler atayabilirsiniz. Bu öğreticide bir hizmet sorumlusu oluşturacak, önceki öğreticide oluşturulan Azure Container Registry (ACR) örneğine erişim verecek ve ardından bir AKS kümesi oluşturacaksınız.

[az ad sp create-for-rbac][] ile bir hizmet sorumlusu oluşturun. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller.

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

İlk olarak [az acr show][] komutuyla ACR kaynağı kimliğini alın. `<acrName>` kayıt defteri adını ACR örneğinin adıyla ve ACR örneğinizin bulunduğu kaynak grubuyla güncelleştirin.

```azurecli
az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv
```

AKS kümesine ACR içinde depolanan görüntüleri kullanmak üzere doğru erişimi vermek için [az role assignment create][] ile bir rol ataması oluşturun. `<appId`> ve `<acrId>` yerine önceki iki adımda topladığınız değerleri yazın.

```azurecli
az role assignment create --assignee <appId> --role Reader --scope <acrId>
```

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Şimdi [az aks create][] ile bir AKS kümesi oluşturun. Aşağıdaki örnek, *myResourceGroup* adlı kaynak grubunda *myAKSCluster* adlı bir küme oluşturur. Bu kaynak grubu, [bir önceki öğreticide][aks-tutorial-prepare-acr] oluşturulmuştur. `<appId>` ve `<password>` yerine hizmet sorumlusu oluşturduğunuz bir önceki adımdan aldığınız değerlerinizi girin.

```azurecli
az aks create \
    --name myAKSCluster \
    --resource-group myResourceGroup \
    --node-count 1 \
    --generate-ssh-keys \
    --service-principal <appId> \
    --client-secret <password>
```

Birkaç dakika sonra dağıtım tamamlanır ve AKS dağıtımı hakkında JSON tarafından biçimlendirilmiş bilgiler döndürür.

## <a name="install-the-kubectl-cli"></a>kubectl CLI yükleme

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanın.

Azure Cloud Shell kullanıyorsanız kubectl zaten yüklüdür. [az aks install-cli][] komutuyla yerel olarak da yükleyebilirsiniz:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

kubectl istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][] komutunu kullanın. Aşağıdaki örnek *myResourceGroup* içindeki *myAKSCluster* adlı AKS kümesinin kimlik bilgilerini alır:

```azurecli
az aks get-credentials --name myAKSCluster --resource-group myResourceGroup
```

Kümenize yönelik bağlantıyı doğrulamak için [kubectl get nodes][kubectl-get] komutunu çalıştırın.

```azurecli
kubectl get nodes
```

Çıktı:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-66427764-0   Ready     agent     9m        v1.9.6
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, AKS içinde bir Kubernetes kümesi dağıtıldı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Kaynak etkileşimleri için hizmet sorumlusu oluşturuldu
> * Kubernetes AKS kümesi dağıtıldı
> * Kubernetes CLI (kubectl) yüklendi
> * Kubectl yapılandırıldı

Kümede uygulama çalıştırma hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin.

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
