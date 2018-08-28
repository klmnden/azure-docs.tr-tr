---
title: Azure’da Kubernetes öğreticisi - Uygulamayı Ölçeklendirme
description: Bu Azure Kubernetes Service (AKS) öğreticisinde Kubernetes içindeki düğümleri ve podları ölçeklendirmenin yanı sıra yataya pod otomatik ölçeklendirme uygulamasını yapmayı öğreneceksiniz.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5ffe7b4c7830500e5eeeeb61c57730d9a0d9df47
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "41918949"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Hizmeti’nde (AKS) uygulamaları ölçeklendirme

Öğreticileri takip ediyorsanız, AKS’de çalışan bir Kubernetes kümesine sahipsinizdir ve Azure Voting uygulamasını dağıtmışsınızdır. Yedi öğreticinin beşinci parçası olan bu öğreticide, uygulamada pod’ları ölçeklendirirsiniz ve otomatik pod ölçeklendirmeyi denersiniz. Ayrıca, barındırılan iş yükleri için kümenin kapasitesini değiştirmek üzere Azure VM düğümlerinin sayısını ölçeklendirmeyi de öğrenirsiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kubernetes düğümlerini ölçeklendirme
> * Uygulamanızı çalıştıran Kubernetes podlarını el ile ölçeklendirme
> * Uygulama ön ucunu çalıştıran otomatik ölçeklendirme podlarını yapılandırma

Sonraki öğreticilerde Azure Vote uygulaması yeni sürüme güncelleştirilecektir.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, bu görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

Bu öğretici için Azure CLI 2.0.38 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="manually-scale-pods"></a>Pod’ları el ile ölçeklendirme

Önceki öğreticilerde Azure Vote ön ucu ve Redis örneği dağıtıldığında tek bir çoğaltma oluşturulmuştu. Kümenizdeki podların sayısını ve durumunu görmek için [kubectl get][kubectl-get] komutunu aşağıdaki şekilde kullanın:

```console
kubectl get pods
```

Aşağıdaki örnek çıktıda bir ön uç podu ve bir arka uç podu gösterilmektedir:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

*azure-vote-front* dağıtımındaki pod sayısını el ile değiştirmek için [kubectl scale][kubectl-scale] komutunu kullanın. Aşağıdaki örnekte ön uç podlarının sayısı *5*'e çıkarılmaktadır:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Kubernetes'in ek podları oluşturduğunu doğrulamak için [kubectl get pods][kubectl-get] komutunu tekrar çalıştırın. Yaklaşık bir dakika sonra ek podlar kümenizde kullanılabilir duruma gelir:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Pod’ları otomatik ölçeklendirme

Kubernetes, bir dağıtımdaki pod’ların sayısını CPU kullanımı ve diğer seçili ölçümleri temel alarak ayarlamak için [yatay pod otomatik ölçeklendirmeyi][kubernetes-hpa] destekler. [Ölçüm Sunucusu][metrics-server], Kubernetes'e kaynak kullanımı sağlamak için kullanılır. Ölçüm Sunucusu'nu yüklemek için, `metrics-server` GitHub deposunu kopyalayın ve örnek kaynak tanımlarını yükleyin. Bu YAML tanımlarının içeriğini görüntülemek için bkz. [Kuberenetes 1.8+ için Ölçüm Sunucusu][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Otomatik ölçeklendiriciyi kullanmak için pod’larınızın CPU istekleri olmalı ve sınırları tanımlanmış olmalıdır. `azure-vote-front` dağıtımında, ön uç kapsayıcısı 0,5 sınırlı, 0,25 CPU kullanımı ister. Ayarları şöyle görünür:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnek, *azure-vote-front* dağıtımındaki podların sayısını otomatik olarak ölçeklendirmek için [kubectl autoscale][kubectl-autoscale] komutunu kullanır. CPU kullanımı %50’yi aşarsa, otomatik ölçeklendirici, podların sayısını üst sınır olan 10 örneğe yükseltir:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Otomatik ölçeklendirme durumunu görmek için `kubectl get hpa` komutunu şu şekilde kullanın:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Birkaç dakika sonra Azure Vote uygulamasında en az yük ile, pod çoğaltmalarının sayısı otomatik olarak üçe iner. `kubectl get pods` komutunu tekrar kullanarak gereksiz podların kaldırıldığını görebilirsiniz.

## <a name="manually-scale-aks-nodes"></a>AKS düğümlerini el ile ölçeklendirme

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

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Kubernetes kümenizde farklı ölçeklendirme özellikleri kullandınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kubernetes düğümlerini ölçeklendirme
> * Uygulamanızı çalıştıran Kubernetes podlarını el ile ölçeklendirme
> * Uygulama ön ucunu çalıştıran otomatik ölçeklendirme podlarını yapılandırma

Kubernetes’te uygulama güncelleştirmeyi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes'te uygulama güncelleştirme][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
