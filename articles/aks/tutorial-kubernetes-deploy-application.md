---
title: "Azure’da Kubernetes öğreticisi - Uygulamayı Dağıtma"
description: "AKS öğreticisi - Uygulamayı Dağıtma"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 4ee59782a0e40f9d09023a75862ab0f8415e6d12
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="run-applications-in-azure-container-service-aks"></a>Azure Container Service’te (AKS) uygulamaları çalıştırma

Sekiz bölümün dördüncüsü olan bu öğreticide Kubernetes kümesine örnek bir uygulama dağıtılır. Tamamlanan adımlar:

> [!div class="checklist"]
> * Kubernetes bildirim dosyalarını güncelleştirme
> * Kubernetes'te uygulama çalıştırma
> * Uygulamayı test etme

Sonraki öğreticilerde bu uygulama ölçeklendirilir, güncelleştirilir ve Operations Management Suite, Kubernetes kümesini izlemek için yapılandırılır.

Bu öğreticide temel Kubernetes kavramlarını bildiğiniz varsayılmıştır. Kubernetes hakkında ayrıntılı bilgi için [Kubernetes belgelerine][kubernetes-documentation] bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. 

Bu öğreticiyi tamamlamak için önceden oluşturulmuş `azure-vote-all-in-one-redis.yaml` Kubernetes bildirim dosyasına ihtiyaç duyarsınız. Bu dosya, önceki öğreticide uygulama kaynak koduyla indirildi. Deponun bir kopyasını oluşturduğunuzu ve dizinleri kopyalanmış depoya göre değiştirdiğinizi doğrulayın.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

## <a name="update-manifest-file"></a>Bildirim dosyasını güncelleştirme

Bu öğreticide kapsayıcı görüntüsü depolamak için Azure Container Registry (ACR) kullanılmıştır. Uygulamayı çalıştırmadan önce, ACR oturum açma sunucusu adının Kubernetes bildirim dosyasında güncelleştirilmesi gerekir.

[az acr list][az-acr-list] komutuyla ACR oturum açma sunucusu adını alın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Bildirim dosyası, `microsoft` oturum açma sunucusu adıyla önceden oluşturulmuştur. Dosyayı herhangi bir metin düzenleyici ile açın. Bu örnekte, dosya `vi` ile açılır.

```console
vi azure-vote-all-in-one-redis.yaml
```

ACR oturum açma sunucu adını `microsoft` ile değiştirin. Bu değer, bildirim dosyasının **47**. satırında bulunur.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Dosyayı kaydedin ve kapatın.

## <a name="deploy-application"></a>Uygulamayı dağıtma

Uygulamayı çalıştırmak için [kubectl create][kubectl-create] komutunu kullanın. Bu komut, bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yaml
```

Çıktı:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Uygulamayı test etme

Uygulamayı İnternet'te kullanıma sunan [Kubernetes hizmeti][kubernetes-service] oluşturulur. Bu işlem birkaç dakika sürebilir. 

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* için *EXTERNAL-IP* durumu *pending* olarak görünür.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

*EXTERNAL-IP* adresi *pending* durumundan *IP address* değerine değiştiğinde kubectl izleme işlemini durdurmak için `CTRL-C` komutunu kullanın. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Uygulamayı görmek için dış IP adresine gözatın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

Uygulama yüklenmediyse, bunun nedeni görüntü kayıt defterinizdeki bir yetkilendirme sorunu olabilir.

Lütfen bu adımları izleyerek [bir Kubernetes gizli dizisi aracılığıyla erişime izin verin](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure oylama uygulaması, AKS'deki bir Kubernetes kümesine dağıtılmıştır. Tamamlanan görevler şunları içerir:  

> [!div class="checklist"]
> * Kubernetes bildirim dosyalarını indirme
> * Uygulamayı Kubernetes'te çalıştırma
> * Uygulamayı test etme

Hem bir Kubernetes uygulamasını hem de temel alınan Kubernetes altyapısını ölçeklendirme hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Kubernetes uygulamasını ve altyapısını ölçeklendirme][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
