---
title: "Kubernetes üzerinde Azure Öğreticisi - uygulamayı Ölçeklendir"
description: "AKS Öğreticisi - uygulamayı Ölçeklendir"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: ff8cf813f9c932f867413dbf7e76f949e0de2f26
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="scale-application-in-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) uygulamayı Ölçeklendir

Öğreticiler takip, çalışan bir Kubernetes küme içinde AKS sahip ve Azure oylama uygulaması dağıtılır.

Bu öğreticide parçası beş sekiz, uygulama pod'ları ölçeğini ve pod otomatik ölçeklendirmeyi deneyin. Ayrıca iş yüklerini barındırmak için kümenin kapasite değiştirmek için Azure VM düğüm sayısının ölçeğini öğrenin. Tamamlanan görevler aşağıdakileri içerir:

> [!div class="checklist"]
> * Kubernetes Azure düğümleri ölçeklendirme
> * Kubernetes pod'ları el ile ölçeklendirme
> * Otomatik ölçeklendirme uygulaması ön ucu çalıştıran pod'ları yapılandırma

Sonraki öğreticilerde, Azure oy uygulama güncelleştirilir ve Kubernetes küme izlemek için Operations Management Suite yapılandırılmış.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine bir uygulama bir kapsayıcı görüntü, Azure kapsayıcı kayıt defterine karşıya bu görüntü ve oluşturulan Kubernetes küme paketlenmiştir. Uygulama sonra Kubernetes kümede çalıştırıldı.

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri][aks-tutorial-prepare-app].

## <a name="scale-aks-nodes"></a>Ölçek AKS düğümler

Önceki öğreticide komutları kullanarak Kubernetes kümenize oluşturduysanız, tek bir düğüme sahip. Daha fazla veya daha az sayıda kapsayıcı iş yükleri kümenizde düşünüyorsanız, düğüm sayısını el ile ayarlayabilirsiniz.

Aşağıdaki örnek üç adlı Kubernetes kümedeki düğüm sayısını artırır *myK8sCluster*. Komut birkaç tamamlamak için dakika sürer.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myK8SCluster --node-count 3
```

Çıktı şuna benzer olacaktır:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myK8sCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Pod'ları el ile ölçeklendirin

Bu nedenle şimdiye kadar Azure oy ön uç ve Redis örnek silinmiş dağıtıldı, her tek bir çoğaltma ile. Doğrulamak için çalıştırın [kubectl almak] [ kubectl-get] komutu.

```azurecli
kubectl get pods
```

Çıktı:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

El ile pod'ları içinde sayısını değiştirme `azure-vote-front` dağıtım kullanarak [kubectl ölçek] [ kubectl-scale] komutu. Bu örnek 5 sayısını artırır.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Çalıştırma [kubectl pod'ları alma] [ kubectl-get] Kubernetes pod'ları oluşturmakta olduğunu doğrulayın. Bir dakika veya bunu sonra ek pod'ları çalıştırıyorsanız:

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

## <a name="autoscale-pods"></a>Otomatik ölçeklendirme pod'ları

Kubernetes destekleyen [yatay pod otomatik ölçeklendirmeyi] [ kubernetes-hpa] ayarlamak için pod'ları CPU kullanımına bağlı olarak bir dağıtımda sayısı veya diğer ölçümleri seçin.

Autoscaler kullanmak için pod'ları CPU istekleri ve tanımlanan sınırları olması gerekir. İçinde `azure-vote-front` dağıtım, ön uç kapsayıcı 0,5 sınırına sahip istekleri 0,25 CPU CPU. Ayarları gibi görünür:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnek kullanır [kubectl otomatik ölçeklendirme] [ kubectl-autoscale] pod'ları içinde sayısı için otomatik ölçeklendirme komutu `azure-vote-front` dağıtım. Burada, CPU kullanımı % 50 aşarsa, en fazla 10 için pod'ları autoscaler artırır.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Autoscaler durumunu görmek için aşağıdaki komutu çalıştırın:

```azurecli
kubectl get hpa
```

Çıktı:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Azure oy uygulama üzerinde minimum yük ile birkaç dakika sonra pod çoğaltmaların sayısı 3'e otomatik olarak azaltır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, farklı ölçekleme özelliklerini Kubernetes kümenizdeki kullanılır. Görevleri dahil ele:

> [!div class="checklist"]
> * Kubernetes pod'ları el ile ölçeklendirme
> * Otomatik ölçeklendirme uygulaması ön ucu çalıştıran pod'ları yapılandırma
> * Kubernetes Azure düğümleri ölçeklendirme

Kubernetes uygulamada güncelleştirmek hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Bir uygulamada Kubernetes güncelleştir][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
