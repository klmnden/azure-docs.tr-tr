---
title: "Azure’da Kubernetes öğreticisi - Uygulamayı güncelleştirme"
description: "AKS öğreticisi - Uygulamayı güncelleştirme"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/24/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 82a6b6580fbe69b11fdb8a47e2ca09c19b341bbc
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="update-an-application-in-azure-container-service-aks"></a>Azure Container Service’te (AKS) bir uygulamayı güncelleştirme

Bir uygulama Kubernetes’te dağıtıldıktan sonra, yeni bir kapsayıcı görüntüsü veya görüntü sürümü belirtilerek güncelleştirilebilir. Bu yapıldığında, güncelleştirme, dağıtımın yalnızca bir kısmı eşzamanlı olarak güncelleştirilecek şekilde hazırlanılır. Hazırlanan bu güncelleştirme, uygulamanın güncelleştirme sırasında çalışmaya devam etmesini sağlar. Ayrıca bir dağıtım hatası oluşursa, bir geri alma mekanizması sağlar. 

Bu yedi parçalı öğreticinin altıncı bölümünde, örnek Azure Vote uygulaması güncelleştirilir. Tamamladığınız görevler şunları içerir:

> [!div class="checklist"]
> * Ön uç uygulaması kodunu güncelleştirme
> * Güncelleştirilmiş kapsayıcı görüntüsü oluşturma
> * Kapsayıcı görüntüsünü Azure Container Registry’ye gönderme
> * Güncelleştirilmiş kapsayıcı görüntüsünü dağıtma

Sonraki öğreticilerde Kubernetes kümesini izlemek için Operations Management Suite yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı. 

Bu öğreticide ayrıca uygulama kaynak kodunu içeren bir uygulama deposu da kopyalandı ve önceden oluşturulmuş bir Docker Compose dosyası kullanıldı. Deponun bir kopyasını oluşturduğunuzu ve dizinleri kopyalanmış dizine değiştirdiğinizi doğrulayın. İçeride `azure-vote` adlı bir dizin ve `docker-compose.yaml` adlı bir dosya bulunuyor.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün. 

## <a name="update-application"></a>Uygulamayı güncelleştirme

Bu öğreticide, uygulamada bir değişiklik yapıldı ve güncelleştirilmiş uygulama Kubernetes kümesine dağıtıldı. 

Uygulama kaynak kodu `azure-vote` dizininde bulunabilir. `config_file.cfg` dosyasını herhangi bir kod ya da metin düzenleyici ile açın. Bu örnekte `vi` kullanıldı.

```console
vi azure-vote/azure-vote/config_file.cfg
```

`VOTE1VALUE` ile `VOTE2VALUE` değerlerini değiştirin ve sonra dosyayı kaydedin.

```console
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Dosyayı kaydedin ve kapatın.

## <a name="update-container-image"></a>Kapsayıcı görüntüsünü güncelleştirme

Ön uç görüntüsünü yeniden oluşturmak için [docker-compose][docker-compose]’u kullanın ve güncelleştirilmiş uygulamayı çalıştırın. `--build` bağımsız değişkeni, uygulama görüntüsünü yeniden oluşturmak üzere Docker Compose'a komut vermek için kullanılır.

```console
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Uygulamayı yerel olarak test etme

Güncelleştirilmiş uygulamayı görüntülemek için http://localhost:8080 konumuna göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Görüntüleri etiketleme ve gönderme

`azure-vote-front` görüntüsünü, kapsayıcı kayıt defterinin loginServer’ıyla etiketleyin. 

[az acr list](/cli/azure/acr#az_acr_list) komutuyla oturum açma sunucusu adını alın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Görüntüyü etiketlemek için [docker tag][docker-tag]’i kullanın. `<acrLoginServer>` değerini, Azure Container Registry oturum açma sunucu adınız veya genel kayıt defteri ana bilgisayar adınız ile değiştirin. Ayrıca görüntü sürümünün `v2` değerine güncelleştirildiğine dikkat edin.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v2
```

Görüntüyü kayıt defterinize yüklemek için [docker push][docker-push]’u kullanın. `<acrLoginServer>` değerini, Azure Container Registry oturum açma sunucu adınız ile değiştirin.

```console
docker push <acrLoginServer>/azure-vote-front:v2
```

## <a name="deploy-update-application"></a>Güncelleştirilmiş uygulamayı dağıtma

En uzun çalışma süresini sağlamak için uygulama podunun birden çok örneğinin çalıştırılması gerekir. Bu yapılandırmayı [kubectl get pod][kubectl-get] komutu ile doğrulayın.

```
kubectl get pod
```

Çıktı:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

azure-vote-front görüntüsünü çalıştıran birden çok podunuz yoksa, `azure-vote-front` dağıtımını ölçeklendirin.


```azurecli
kubectl scale --replicas=3 deployment/azure-vote-front
```

Uygulamayı güncelleştirmek için [kubectl set][kubectl-set] komutunu kullanın. `<acrLoginServer>` öğesini, kapsayıcı kayıt defterinizin oturum açma sunucusu veya ana bilgisayar adıyla güncelleştirin.

```azurecli
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

Dağıtımı izlemek için [kubectl get pod][kubectl-get] komutunu kullanın. Güncelleştirilmiş uygulama dağıtıldığında, podlarınız sonlandırılır ve yeni kapsayıcı görüntüsüyle yeniden oluşturulur.

```azurecli
kubectl get pod
```

Çıktı:

```
NAME                               READY     STATUS        RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running       0          5m
azure-vote-front-1297194256-tpjlg  1/1       Running       0          1m
azure-vote-front-1297194256-tptnx  1/1       Running       0          5m
azure-vote-front-1297194256-zktw9  1/1       Terminating   0          1m
```

## <a name="test-updated-application"></a>Güncelleştirilmiş uygulamayı test etme

`azure-vote-front` hizmetinin dış IP adresini alın.

```azurecli
kubectl get service azure-vote-front
```

Güncelleştirilmiş uygulamayı görüntülemek için IP adresine göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulamayı güncelleştirdiniz ve bu güncelleştirmeyi bir Kubernetes kümesine sundunuz. Şu görevler tamamlandı:

> [!div class="checklist"]
> * Ön uç uygulaması kodu güncelleştirildi
> * Güncelleştirilmiş kapsayıcı görüntüsü oluşturuldu
> * Kapsayıcı görüntüsü Azure Container Registry’ye gönderildi
> * Güncelleştirilmiş uygulama dağıtıldı

Operations Management Suite ile Kubernetes’in nasıl izlenileceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Log Analytics ile Kubernetes’i izleme][aks-tutorial-monitor]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-monitor]: ./tutorial-kubernetes-monitor.md