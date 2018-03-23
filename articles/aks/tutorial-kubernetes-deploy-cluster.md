---
title: Azure’da Kubernetes öğreticisi - Kümeyi Dağıtma
description: AKS öğreticisi - Kümeyi Dağıtma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/24/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d02229739e3f358e4a6510dfbb0585939e947f9c
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Azure Container Service (AKS) kümesini dağıtma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. AKS ile üretime hazır bir Kubernetes kümesinin sağlanması basit ve hızlıdır. Sekiz parçalık bu öğreticinin üçüncü kısmında, AKS içinde bir Kubernetes kümesi dağıtılır. Tamamlanan adımlar:

> [!div class="checklist"]
> * Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yüklemesi
> * Kubectl yapılandırması

Sonraki öğreticilerde, Azure Vote uygulaması kümeye dağıtılır, ölçeklendirilir, güncelleştirilir ve Operations Management Suite, Kubernetes kümesini izlemek için yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir kapsayıcı görüntüsü oluşturuldu ve Azure Container Registry örneğine yüklendi. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

## <a name="enable-aks-preview"></a>AKS önizlemesini etkinleştirme

AKS önizlemedeyken, yeni kümeler oluşturmak aboneliğinizde özellik bayrağı olmasını gerektirir. Bu özelliği kullanmak istediğiniz herhangi bir sayıda abonelik için isteyebilirsiniz. AKS sağlayıcıyı kaydetmek için `az provider register` komutunu kullanın:

```azurecli
az provider register -n Microsoft.ContainerService
```

Kaydettikten sonra, AKS ile bir Kubernetes kümesi oluşturmak için hazırsınız.

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Şu örnek, `myResourceGroup` adlı Kaynak Grubunda `myAKSCluster` adlı bir küme oluşturur. Bu Kaynak Grubu, [bir önceki öğreticide][aks-tutorial-prepare-acr] oluşturuldu.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

Birkaç dakika sonra dağıtım tamamlanır ve AKS dağıtımı hakkında JSON tarafından biçimlendirilmiş bilgiler döndürür.

## <a name="install-the-kubectl-cli"></a>kubectl CLI yükleme

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanın.

Azure CloudShell kullanıyorsanız kubectl zaten yüklüdür. Yerel olarak yüklemek istiyorsanız, aşağıdaki komutu çalıştırın:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

kubectl’i Kubernetes kümenize bağlanacak şekilde yapılandırmak için aşağıdaki komutu çalıştırın:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myAKSCluster
```

Kümenize yönelik bağlantıyı doğrulamak için [kubectl get nodes][kubectl-get] komutunu çalıştırın.

```azurecli
kubectl get nodes
```

Çıktı:

```
NAME                          STATUS    AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     49m       v1.7.9
```

Öğretici tamamlandığında, iş yüklerine hazır bir AKS kümesine sahip olursunuz. Sonraki öğreticilerde bu kümeye birden çok kapsayıcılı uygulama dağıtılır, ölçeklendirilir, güncelleştirilir ve izlenir.

## <a name="configure-acr-authentication"></a>ACR kimlik doğrulamasını yapılandırma

Kimlik doğrulama işleminin AKS kümesi ve ACR kayıt defteri arasında yapılandırılması gerekir. Bu, ACR kayıt defterinden görüntüleri çekmek için AKS kimliğindeki uygun hakları sağlamayı içerir.

İlk olarak, AKS için yapılandırılmış hizmet sorumlusu kimliğini alın. Kaynak grubunun ve AKS kümesinin adını, ortamınızla eşleşecek şekilde güncelleştirin.

```azurecli
CLIENT_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId" --output tsv)
```

ACR kayıt defteri kaynak kimliğini alın. Kayıt defteri adını, ACR kayıt defterinizin adıyla, kaynak grubu adını da ACR kayıt defterinin bulunduğu konumla güncelleştirin.

```azurecli
ACR_ID=$(az acr show --name myACRRegistry --resource-group myResourceGroup --query "id" --output tsv)
```

Uygun erişimi sağlayan rol atamasını oluşturun.

```azurecli
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, AKS içinde bir Kubernetes kümesi dağıtıldı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
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