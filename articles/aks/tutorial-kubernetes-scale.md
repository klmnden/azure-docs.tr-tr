---
title: Azure’da Kubernetes öğreticisi - Uygulamayı Ölçeklendirme
description: Bu Azure Kubernetes Service (AKS) öğreticisinde Kubernetes içindeki düğümleri ve podları ölçeklendirmenin yanı sıra yataya pod otomatik ölçeklendirme uygulamasını yapmayı öğreneceksiniz.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 5a942aa10f36df55ac232defa610102700e3995b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614189"
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

Bu öğretici, Azure CLI Sürüm 2.0.53 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="manually-scale-pods"></a>Pod’ları el ile ölçeklendirme

Önceki öğreticilerde Azure Vote ön ucu ve Redis örneği dağıtıldığında tek bir çoğaltma oluşturulmuştu. Sayısı ve kümenize pod'ların durumunu görmek için [kubectl alma][kubectl-get] komutuyla şu şekilde:

```console
kubectl get pods
```

Aşağıdaki örnek çıktıda bir ön uç podu ve bir arka uç podu gösterilmektedir:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Pod'ların sayısını el ile olarak değiştirmek için *azure-vote-front* dağıtım, kullanım [kubectl ölçek][kubectl-scale] komutu. Aşağıdaki örnekte ön uç podlarının sayısı *5*'e çıkarılmaktadır:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Çalıştırma [kubectl pod'ları alma][kubectl-get] yeniden AKS ek pod'ları oluşturduğunu doğrulayın. Yaklaşık bir dakika sonra ek podlar kümenizde kullanılabilir duruma gelir:

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

Kubernetes destekler [yatay pod otomatik ölçeklendirmeyi][kubernetes-hpa] to adjust the number of pods in a deployment depending on CPU utilization or other select metrics. The [Metrics Server][metrics-server] Kubernetes için kaynak kullanımını sağlamak için kullanılır ve AKS küme 1.10 ve üzeri sürümleri otomatik olarak dağıtılır. AKS kümenizi sürümünü görmek için [az aks show][az aks show] aşağıdaki örnekte gösterildiği gibi komut:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query kubernetesVersion
```

AKS kümesi *1.10* sürümünden eskiyse Ölçüm Sunucusu'nu yükleyin. Aksi takdirde bu adımı atlayın. Yüklemek için kopyalama `metrics-server` GitHub deposunu ve örnek kaynak tanımları yükleyin. Bu YAML tanımları içeriğini görüntülemek için bkz: [Kuberenetes 1.8 + ölçümleri sunucusu][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Ölçeklendiriciyi kullanmak için pod ve pod tüm kapsayıcıların CPU istekleri ve sınırları tanımlanmış olmalıdır. İçinde `azure-vote-front` dağıtım, ön uç kapsayıcısı 0,5 ile 0,25 CPU zaten CPU. Bu kaynak isteklerini ve sınırları aşağıdaki örnek kod parçacığında gösterildiği gibi tanımlanır:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnekte [kubectl autoscale][kubectl-autoscale] pod'ların sayısını otomatik olarak ölçeklendirmek için komut *azure-vote-front* dağıtım. CPU kullanımı % 50 aşarsa, en çok pod'ları otomatik ölçeklendiricinin artırır *10* örnekleri. En az *3* örnekleri sonra dağıtım için tanımlanır:

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
[metrics-server]: https://v1-12.docs.kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
