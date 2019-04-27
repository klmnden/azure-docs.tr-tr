---
title: (KULLANIM DIŞI) Azure Container Service Öğreticisi - uygulamayı güncelleştirme
description: Azure Container Service öğreticisi - Uygulamayı güncelleştirme
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 3c0be935a4ffb51c03d2f63b14ab7c0c713dd2ae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576722"
---
# <a name="deprecated-update-an-application-in-kubernetes"></a>(KULLANIM DIŞI) Kubernetes'te uygulama güncelleştirme

> [!TIP]
> Azure Kubernetes hizmeti kullanan Bu öğretici için güncelleştirilmiş sürümü görmek [Öğreticisi: Azure Kubernetes Service (AKS) uygulama güncelleştirme](../../aks/tutorial-kubernetes-app-update.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Bir uygulama Kubernetes’te dağıtıldıktan sonra, yeni bir kapsayıcı görüntüsü veya görüntü sürümü belirtilerek güncelleştirilebilir. Bu yapıldığında, güncelleştirme, dağıtımın yalnızca bir kısmı eşzamanlı olarak güncelleştirilecek şekilde hazırlanılır. Hazırlanan bu güncelleştirme, uygulamanın güncelleştirme sırasında çalışmaya devam etmesini sağlar. Ayrıca bir dağıtım hatası oluşursa, bir geri alma mekanizması sağlar. 

Bu yedi parçalı öğreticinin altıncı bölümünde, örnek Azure Vote uygulaması güncelleştirilir. Tamamladığınız görevler şunları içerir:

> [!div class="checklist"]
> * Ön uç uygulaması kodunu güncelleştirme
> * Güncelleştirilmiş kapsayıcı görüntüsü oluşturma
> * Kapsayıcı görüntüsünü Azure Container Registry’ye gönderme
> * Güncelleştirilmiş kapsayıcı görüntüsünü dağıtma

Sonraki öğreticilerde Kubernetes kümesini izlemek için Log Analytics yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı. 

Bu öğreticide ayrıca uygulama kaynak kodunu içeren bir uygulama deposu da kopyalandı ve önceden oluşturulmuş bir Docker Compose dosyası kullanıldı. Deponun bir kopyasını oluşturduğunuzu ve dizinleri kopyalanmış dizine değiştirdiğinizi doğrulayın. İçeride `azure-vote` adlı bir dizin ve `docker-compose.yml` adlı bir dosya bulunuyor.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma](./container-service-tutorial-kubernetes-prepare-app.md) konusuna dönün. 

## <a name="update-application"></a>Uygulamayı güncelleştirme

Bu öğreticide, uygulamada bir değişiklik yapıldı ve güncelleştirilmiş uygulama Kubernetes kümesine dağıtıldı. 

Uygulama kaynak kodu `azure-vote` dizininde bulunabilir. `config_file.cfg` dosyasını herhangi bir kod ya da metin düzenleyici ile açın. Bu örnekte `vi` kullanıldı.

```bash
vi azure-vote/azure-vote/config_file.cfg
```

`VOTE1VALUE` ile `VOTE2VALUE` değerlerini değiştirin ve sonra dosyayı kaydedin.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Dosyayı kaydedin ve kapatın.

## <a name="update-container-image"></a>Kapsayıcı görüntüsünü güncelleştirme

Ön uç görüntüsünü yeniden oluşturmak için [docker-compose](https://docs.docker.com/compose/)’u kullanın ve güncelleştirilmiş uygulamayı çalıştırın. `--build` bağımsız değişkeni, uygulama görüntüsünü yeniden oluşturmak üzere Docker Compose'a komut vermek için kullanılır.

```bash
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Uygulamayı yerel olarak test etme

Güncelleştirilmiş uygulamayı görüntülemek için `http://localhost:8080` adresine göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Görüntüleri etiketleme ve gönderme

`azure-vote-front` görüntüsünü, kapsayıcı kayıt defterinin loginServer’ıyla etiketleyin. 

[az acr list](/cli/azure/acr#az-acr-list) komutuyla oturum açma sunucusu adını alın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Görüntüyü etiketlemek için [docker tag](https://docs.docker.com/engine/reference/commandline/tag/)’i kullanın. `<acrLoginServer>` değerini, Azure Container Registry oturum açma sunucu adınız veya genel kayıt defteri ana bilgisayar adınız ile değiştirin. Ayrıca görüntü sürümünün `redis-v2` değerine güncelleştirildiğine dikkat edin.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Görüntüyü kayıt defterinize yüklemek için [docker push](https://docs.docker.com/engine/reference/commandline/push/)’u kullanın. `<acrLoginServer>` değerini, Azure Container Registry oturum açma sunucu adınız ile değiştirin.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Güncelleştirilmiş uygulamayı dağıtma

En uzun çalışma süresini sağlamak için uygulama podunun birden çok örneğinin çalıştırılması gerekir. Bu yapılandırmayı [kubectl get pod](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) komutu ile doğrulayın.

```bash
kubectl get pod
```

Çıkış:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

azure-vote-front görüntüsünü çalıştıran birden çok podunuz yoksa, `azure-vote-front` dağıtımını ölçeklendirin.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

Uygulamayı güncelleştirmek için [kubectl set](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set) komutunu kullanın. `<acrLoginServer>` öğesini, kapsayıcı kayıt defterinizin oturum açma sunucusu veya ana bilgisayar adıyla güncelleştirin.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Dağıtımı izlemek için [kubectl get pod](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) komutunu kullanın. Güncelleştirilmiş uygulama dağıtıldığında, podlarınız sonlandırılır ve yeni kapsayıcı görüntüsüyle yeniden oluşturulur.

```azurecli-interactive
kubectl get pod
```

Çıkış:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Güncelleştirilmiş uygulamayı test etme

`azure-vote-front` hizmetinin dış IP adresini alın.

```azurecli-interactive
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

Log Analytics ile Kubernetes’in nasıl izlenileceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Log Analytics ile Kubernetes’i izleme](./container-service-tutorial-kubernetes-monitor.md)