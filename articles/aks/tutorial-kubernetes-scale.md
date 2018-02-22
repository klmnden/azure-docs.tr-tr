---
title: "Azure’da Kubernetes öğreticisi - Uygulamayı Ölçeklendirme"
description: "AKS öğreticisi - Uygulamayı Ölçeklendirme"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 993a8b71b29952394a2ab6a2bdddd0fc5fd241ae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="scale-application-in-azure-container-service-aks"></a>Azure Container Service’te (AKS) uygulama ölçeklendirme

Öğreticileri takip ediyorsanız, AKS’de çalışan bir Kubernetes kümesine sahipsinizdir ve Azure Voting uygulamasını dağıtmışsınızdır.

Sekiz öğreticinin beşinci parçası olan bu öğreticide, uygulamada pod’ları ölçeklendirirsiniz ve otomatik pod ölçeklendirmeyi denersiniz. Ayrıca, barındırılan iş yükleri için kümenin kapasitesini değiştirmek üzere Azure VM düğümlerinin sayısını ölçeklendirmeyi de öğrenirsiniz. Tamamlanan görevler şunları içerir:

> [!div class="checklist"]
> * Kubernetes Azure düğümlerini ölçeklendirme
> * Kubernetes pod’larını el ile ölçeklendirme
> * Uygulama ön ucunda çalışan Otomatik pod ölçeklendirmeyi yapılandırma

Sonraki öğreticilerde Azure Vote uygulaması güncelleştirilir ve Operations Management Suite, Kubernetes kümesini izlemek için yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, bu görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

## <a name="scale-aks-nodes"></a>AKS düğümlerini ölçeklendirme

Önceki öğreticide Kubernetes kümenizi komutları kullanarak oluşturduysanız, kümenin bir düğümü vardır. Kümenizde daha fazla veya daha az kapsayıcı iş yükü planlıyorsanız, düğüm sayısını el ile ayarlayabilirsiniz.

Aşağıdaki örnek, *myAKSCluster* adlı Kubernetes kümesinde düğümlerin sayısını üçe yükseltir. Komutun tamamlanması birkaç dakika sürer.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

Çıktı şuna benzer olacaktır:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Pod’ları el ile ölçeklendirme

Şu ana kadar hem Azure Vote ön ucu hem de Redis örneği tek bir çoğaltma dağıtıldı. Doğrulamak için [kubectl get][kubectl-get] komutunu çalıştırın.

```azurecli
kubectl get pods
```

Çıktı:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

[kubectl scale][kubectl-scale] komutunu kullanarak `azure-vote-front` dağıtımında pod’ların sayısını el ile değiştirin. Bu örnek, sayısı 5’e yükseltir.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

[kubectl get pods][kubectl-get] komutunu çalıştırarak Kubernetes’in pod’ları oluşturduğunu doğrulayın. Yaklaşık bir dakika sonra ek pod’lar çalışır:

```azurecli
kubectl get pods
```

Çıktı:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Pod’ları otomatik ölçeklendirme

Kubernetes, bir dağıtımdaki pod’ların sayısını CPU kullanımı ve diğer seçili ölçümleri temel alarak ayarlamak için [yatay pod otomatik ölçeklendirmeyi][kubernetes-hpa] destekler.

Otomatik ölçeklendiriciyi kullanmak için pod’larınızın CPU istekleri olmalı ve sınırları tanımlanmış olmalıdır. `azure-vote-front` dağıtımında, ön uç kapsayıcısı 0,5 sınırlı, 0,25 CPU kullanımı ister. Ayarları şöyle görünür:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnek, `azure-vote-front` dağıtımındaki pod’ların sayısını otomatik olarak ölçeklendirmek için [kubectl autoscale][kubectl-autoscale] komutunu kullanır. Burada CPU kullanımı %50’yi aşarsa, otomatik ölçeklendirici, pod’ların sayısını üst sınır olan 10’a yükseltir.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Otomatik ölçeklendiricinin durumunu görmek için aşağıdaki komutu çalıştırın:

```azurecli
kubectl get hpa
```

Çıktı:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Birkaç dakika sonra Azure Vote uygulamasında en az yük ile, pod çoğaltmalarının sayısı otomatik olarak 3’e azalır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Kubernetes kümenizde farklı ölçeklendirme özellikleri kullandınız. Dahil edilen görevler:

> [!div class="checklist"]
> * Kubernetes pod’larını el ile ölçeklendirme
> * Uygulama ön ucunda çalışan Otomatik pod ölçeklendirmeyi yapılandırma
> * Kubernetes Azure düğümlerini ölçeklendirme

Kubernetes’te uygulama güncelleştirme hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes'te uygulama güncelleştirme][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
