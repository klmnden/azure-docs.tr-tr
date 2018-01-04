---
title: "Azure Öğreticisi - güncelleştirme uygulaması üzerinde Kubernetes"
description: "AKS Öğreticisi - güncelleştirme uygulaması"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6de5173aedc836f7a2d56370ea8e54ad6e77ab5e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="update-an-application-in-azure-container-service-aks"></a>Bir uygulamayı Azure kapsayıcı hizmeti (AKS) güncelleştir

Bir uygulama içinde Kubernetes dağıtıldıktan sonra yeni kapsayıcı resim veya görüntü sürümü belirterek güncelleştirilebilir. Yalnızca bir kısmını dağıtım eşzamanlı olarak güncelleştirilebilmesi için Bunun yapılması, güncelleştirme hazırlanır. Hazırlanan bu güncelleştirme güncelleştirme sırasında çalışmaya devam uygulama sağlar. Bir dağıtım hatası oluşursa, ayrıca bir geri alma mekanizması sağlar. 

Bu öğreticide, bir parçası altı sekiz, örnek Azure oylama uygulaması güncelleştirilir. Tamamlamanız görevler aşağıdakileri içerir:

> [!div class="checklist"]
> * Ön uç uygulamasındaki kod güncelleştiriliyor
> * Güncelleştirilmiş kapsayıcı görüntü oluşturma
> * Azure kapsayıcı kayıt defterine kapsayıcı görüntü iletme
> * Güncelleştirilmiş kapsayıcı görüntü dağıtma

Sonraki öğreticilerde Kubernetes küme izlemek için Operations Management Suite yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine bir uygulama bir kapsayıcı görüntü, Azure kapsayıcı kayıt defterine yüklenen görüntü ve oluşturulan Kubernetes küme paketlenmiştir. Uygulama sonra Kubernetes kümede çalıştırıldı. 

Bir uygulama havuzu da uygulamanın kaynak koduna ve Bu öğreticide kullanılan önceden oluşturulmuş bir Docker Compose dosya içeren kopyalandı. Depodaki bir kopyasını oluşturduktan ve kopyalanan dizine dizinleri değiştirilmediğini doğrulayın. İç olan adlı bir dizin `azure-vote` ve adlı bir dosya `docker-compose.yaml`.

Bu adımları tamamladıysanız henüz ve izlemek istediğiniz dönmek [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri][aks-tutorial-prepare-app]. 

## <a name="update-application"></a>Uygulamayı güncelleştirme

Bu öğreticide, bir değişiklik uygulama ve Kubernetes kümeye dağıtılan güncelleştirilmiş uygulama yapılır. 

Uygulama kaynak koduna içinde bulunabilir `azure-vote` dizin. Açık `config_file.cfg` herhangi bir kod ya da metin düzenleyicisi ile dosya. Bu örnekte `vi` kullanılır.

```console
vi azure-vote/azure-vote/config_file.cfg
```

Değerlerini değiştirmek `VOTE1VALUE` ve `VOTE2VALUE`ve ardından dosyayı kaydedin.

```console
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Dosyasını kaydedin ve kapatın.

## <a name="update-container-image"></a>Güncelleştirme kapsayıcı resmi

Kullanım [docker compose'u] [ docker-compose] ön uç görüntüyü yeniden oluşturun ve güncelleştirilmiş uygulamayı çalıştırın. `--build` Bağımsız değişkeni uygulama görüntüsünü yeniden oluşturmak için Docker Compose'u istemek için kullanılır.

```console
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Uygulamayı yerel olarak test etme

Güncelleştirilmiş uygulamayı görmek için http://localhost: 8080 için göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Etiket ve anında iletme görüntüleri

Etiket `azure-vote-front` kapsayıcı kayıt defteri loginServer görüntüsüyle. 

Oturum açma sunucu adıyla alma [az acr listesi](/cli/azure/acr#list) komutu.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kullanım [docker etiketi] [ docker-tag] görüntü etiketlemek için. Değiştir `<acrLoginServer>` Azure kapsayıcı kayıt defteri oturum açma sunucu adı veya ortak kayıt defteri ana bilgisayar adı. Ayrıca Image sürüm için güncelleştirilen bildirim `redis-v2`.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Kullanım [docker itme] [ docker-push] kayıt defterine görüntüyü karşıya yüklemek için. Değiştir `<acrLoginServer>` , Azure kapsayıcı kayıt defteri oturum açma sunucu adına sahip.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Güncelleştirme uygulaması dağıtma

En fazla çalışma süresi emin olmak için uygulama pod birden çok örneğini çalıştırması gerekir. Bu yapılandırma ile doğrulayın [kubectl alma pod] [ kubectl-get] komutu.

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

Birden çok pod'ları azure oy ön görüntüsünü çalıştıran yoksa ölçeklendirme `azure-vote-front` dağıtım.


```azurecli
kubectl scale --replicas=3 deployment/azure-vote-front
```

Uygulamayı güncelleştirmek için [kubectl kümesi] [ kubectl-set] komutu. Güncelleştirme `<acrLoginServer>` kapsayıcı kaydınız oturum açma sunucusu veya ana bilgisayar adı.

```azurecli
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Dağıtımını izlemek için kullanmak [kubectl alma pod] [ kubectl-get] komutu. Güncelleştirilmiş uygulamayı dağıtılan gibi pod'ları sonlandırıldı ve yeni kapsayıcı görüntüsü ile yeniden oluşturulacak.

```azurecli
kubectl get pod
```

Çıktı:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Güncelleştirilmiş uygulamayı test etme

Dış IP adresi al `azure-vote-front` hizmet.

```azurecli
kubectl get service azure-vote-front
```

Güncelleştirilmiş uygulamayı görmek için IP adresine göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulamanın güncelleştirilmiş ve bu güncelleştirmeyi Kubernetes kümeye alındı. Aşağıdaki görevler tamamlandı:

> [!div class="checklist"]
> * Ön uç uygulamasındaki kod güncelleştirildi
> * Güncelleştirilmiş kapsayıcı görüntüyü oluşturuldu
> * Azure kapsayıcı kayıt defterine kapsayıcı görüntü gönderilir
> * Dağıtılan güncelleştirilmiş uygulama

Operations Management Suite ile Kubernetes izleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Günlük analizi ile İzleyici Kubernetes][aks-tutorial-monitor]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-monitor]: ./tutorial-kubernetes-monitor.md