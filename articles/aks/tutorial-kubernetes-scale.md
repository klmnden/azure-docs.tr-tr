---
title: Azure’da Kubernetes öğreticisi - Uygulamayı Ölçeklendirme
description: Bu Azure Kubernetes Service (AKS) öğreticisinde Kubernetes içindeki düğümleri ve podları ölçeklendirmenin yanı sıra yataya pod otomatik ölçeklendirme uygulamasını yapmayı öğreneceksiniz.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: 884797d7a81418b7a57659df9f5075b82205aa9c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757720"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Service (AKS) uygulamaları ölçeklendirme

Öğreticileri takip ediyorsanız, AKS'de çalışan bir Kubernetes kümesine sahip ve Azure Voting örnek uygulamasını dağıtmışsınızdır. Yedi öğreticinin beşinci parçası olan bu öğreticide, uygulamada pod’ları ölçeklendirirsiniz ve otomatik pod ölçeklendirmeyi denersiniz. Ayrıca, barındırılan iş yükleri için kümenin kapasitesini değiştirmek üzere Azure VM düğümlerinin sayısını ölçeklendirmeyi de öğrenirsiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kubernetes düğümlerini ölçeklendirme
> * Uygulamanızı çalıştıran Kubernetes podlarını el ile ölçeklendirme
> * Uygulama ön ucunu çalıştıran otomatik ölçeklendirme podlarını yapılandırma

Ek öğreticilerde Azure Vote uygulamasını yeni sürüme güncelleştirilir.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi. Bu görüntü Azure Container Registry'ye yüklendi ve bir AKS kümesi oluşturulmakta. Uygulama ardından için AKS kümesi dağıtıldı. Bu adımları bu işlemi yapmadıysanız ve örneği takip etmek istiyorsanız, başlayın [öğretici 1 – kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app].

Bu öğretici, Azure CLI Sürüm 2.0.53 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

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

Çalıştırma [kubectl pod'ları alma] [ kubectl-get] yeniden AKS ek pod'ları oluşturduğunu doğrulayın. Yaklaşık bir dakika sonra ek podlar kümenizde kullanılabilir duruma gelir:

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

Kubernetes, bir dağıtımdaki pod’ların sayısını CPU kullanımı ve diğer seçili ölçümleri temel alarak ayarlamak için [yatay pod otomatik ölçeklendirmeyi][kubernetes-hpa] destekler. [Ölçüm Sunucusu][metrics-server], Kubernetes'e kaynak kullanımı sağlamak için kullanılır ve AKS kümesi 1.10 ve üzeri sürümlerde otomatik olarak dağıtılır. AKS kümenizin sürümünü görmek için aşağıdaki örnekte gösterildiği gibi [az aks show][az-aks-show] komutunu kullanın:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query kubernetesVersion
```

AKS kümesi *1.10* sürümünden eskiyse Ölçüm Sunucusu'nu yükleyin. Aksi takdirde bu adımı atlayın. Yüklemek için kopyalama `metrics-server` GitHub deposunu ve örnek kaynak tanımları yükleyin. Bu YAML tanımlarının içeriğini görüntülemek için bkz. [Kuberenetes 1.8+ için Ölçüm Sunucusu][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Otomatik ölçeklendiriciyi kullanmak için pod’larınızın CPU istekleri olmalı ve sınırları tanımlanmış olmalıdır. İçinde `azure-vote-front` dağıtım, ön uç kapsayıcısı 0,5 ile 0,25 CPU zaten CPU. Bu kaynak isteklerini ve sınırları aşağıdaki örnek kod parçacığında gösterildiği gibi tanımlanır:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnek, *azure-vote-front* dağıtımındaki podların sayısını otomatik olarak ölçeklendirmek için [kubectl autoscale][kubectl-autoscale] komutunu kullanır. CPU kullanımı % 50 aşarsa, en çok pod'ları otomatik ölçeklendiricinin artırır *10* örnekleri. En az *3* örnekleri sonra dağıtım için tanımlanır:

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
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
```

Küme başarıyla Ölçeklendirildi, çıktı aşağıdaki örneğe benzer olacaktır:

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
> * Uygulamanızı çalıştıran Kubernetes podlarını el ile ölçeklendirme
> * Uygulama ön ucunu çalıştıran otomatik ölçeklendirme podlarını yapılandırma
> * El ile Kubernetes düğümlerini ölçeklendirme

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
[az-aks-show]: /cli/azure/aks#az-aks-show
