---
title: Azure’da Kubernetes öğreticisi - Uygulamayı dağıtma
description: Bu Azure Kubernetes Service (AKS) öğreticisinde, Azure Container Registry'de depolanan özel bir görüntüyü kullanarak kümenizde çok kapsayıcılı bir uygulama dağıtacaksınız.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bf817f553250ead449ec0d5db3d33acc2eff23f3
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "41918701"
---
# <a name="tutorial-run-applications-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Hizmeti’nde (AKS) uygulamaları çalıştırma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. Kendi uygulamalarınızı ve hizmetlerinizi oluşturup bir Kubernetes kümesine dağıtırsınız, kullanılabilirlik ve bağlantı özellikleri de küme tarafından yönetilir. Yedi bölümün dördüncüsü olan bu öğreticide Kubernetes kümesine örnek bir uygulama dağıtılır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kubernetes bildirim dosyalarını güncelleştirme
> * Kubernetes'te uygulama çalıştırma
> * Uygulamayı test etme

Sonraki öğreticilerde bu uygulama ölçeklendirilmekte ve güncelleştirilmektedir.

Bu öğreticide temel Kubernetes kavramlarını bildiğiniz varsayılmıştır. Kubernetes hakkında ayrıntılı bilgi için [Kubernetes belgelerine][kubernetes-documentation] bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu.

Bu öğreticiyi tamamlamak için önceden oluşturulmuş `azure-vote-all-in-one-redis.yaml` Kubernetes bildirim dosyasına ihtiyaç duyarsınız. Bu dosya, önceki öğreticide uygulama kaynak koduyla indirildi. Deponun bir kopyasını oluşturduğunuzu ve dizinleri kopyalanmış depoya göre değiştirdiğinizi doğrulayın. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

Bu öğretici için Azure CLI 2.0.44 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="update-the-manifest-file"></a>Bildirim dosyasını güncelleştirme

Bu öğreticilerde Azure Container Registry (ACR) örneği, örnek uygulamanın kapsayıcı görüntüsünü depolar. Uygulamayı dağıtmak için Kubernetes bildirim dosyası içindeki görüntü adını ACR oturum açma sunucusu adını içerecek şekilde güncelleştirmeniz gerekir.

ACR oturum açma sunucusunun adını almak için [az acr list][az-acr-list] komutunu aşağıda gösterilen şekilde kullanın:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

İlk öğreticide kopyalanan git deposundaki örnek bildirim dosyasında oturum açma sunucusu adı olarak *microsoft* kullanılmıştır. Bu bildirim dosyasını `vi` gibi bir metin düzenleyiciyle açın:

```console
vi azure-vote-all-in-one-redis.yaml
```

*microsoft* yerine ACR oturum açma sunucunuzun adını yazın. Görüntü adı, bildirim dosyasının 47. satırında bulunur. Aşağıdaki örnekte varsayılan görüntü adı gösterilir:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Kendi ACR oturum açma sunucunuzun adını girerek bildirim dosyanızın aşağıdaki örnekte olduğu gibi görünmesini sağlayın:

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Dosyayı kaydedin ve kapatın.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulamanızı dağıtmak için [kubectl apply][kubectl-apply] komutunu kullanın. Bu komut, bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur. Aşağıdaki örnekte gösterildiği gibi örnek bildirim dosyasının adını belirtin:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Aşağıdaki örnekte gösterildiği gibi küme içinde Kubernetes nesneleri oluşturulur:

```
$ kubectl apply -f azure-vote-all-in-one-redis.yaml

deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulamayı İnternet'te kullanıma sunan [Kubernetes hizmeti][kubernetes-service] oluşturulur. Bu işlem birkaç dakika sürebilir. İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın:

```console
kubectl get service azure-vote-front --watch
```

*azure-vote-front* hizmeti için *EXTERNAL-IP* değeri aşağıdaki örnekte gösterildiği gibi başlangıçta *pending* olarak görünür:

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

*EXTERNAL-IP* adresi *pending* durumundan gerçek IP adresine değiştiğinde `CTRL-C` komutunu kullanarak kubectl izleme işlemini durdurun. Aşağıdaki örnekte genel IP adresinin atandığı gösterilmektedir:

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Uygulamayı eylem halinde görmek için bir web tarayıcısı açarak dış IP adresine gidin.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

Uygulama yüklenmediyse, bunun nedeni görüntü kayıt defterinizdeki bir yetkilendirme sorunu olabilir. Kapsayıcılarınızın durumunu görüntülemek için `kubectl get pods` komutunu kullanın. Kapsayıcı görüntüleri çekilemiyorsa [Kubernetes gizli dizisi ile Container Registry erişimine izin verin](https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure oylama uygulaması, AKS'deki bir Kubernetes kümesine dağıtılmıştır. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kubernetes bildirim dosyalarını güncelleştirme
> * Kubernetes'te uygulama çalıştırma
> * Uygulamayı test etme

Kubernetes uygulamasını ve temel alınan Kubernetes altyapısını ölçeklendirme hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes uygulamasını ve altyapısını ölçeklendirme][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
[azure-cli-install]: /cli/azure/install-azure-cli
