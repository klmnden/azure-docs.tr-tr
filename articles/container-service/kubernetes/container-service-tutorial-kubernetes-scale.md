---
title: "Azure kapsayıcı hizmeti Öğreticisi - uygulamayı Ölçeklendir"
description: "Azure kapsayıcı hizmeti Öğreticisi - uygulamayı Ölçeklendir"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a748e15abbc01f260349fba2678c03a40c4d7713
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Ölçek Kubernetes pod'ları ve Kubernetes altyapısı

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Öğreticiler takip, Azure kapsayıcı Hizmeti'nde çalışan Kubernetes küme sahip olmanız ve Azure oylama uygulaması dağıtılır. 

Bu öğreticide parçası beş yedi, uygulama pod'ları ölçeğini ve pod otomatik ölçeklendirmeyi deneyin. Ayrıca iş yüklerini barındırmak için kümenin kapasite değiştirmek için Azure VM Aracısı düğüm sayısının ölçeğini öğrenin. Tamamlanan görevler aşağıdakileri içerir:

> [!div class="checklist"]
> * Kubernetes pod'ları el ile ölçeklendirme
> * Otomatik ölçeklendirme uygulaması ön ucu çalıştıran pod'ları yapılandırma
> * Kubernetes Azure Aracısı düğümleri ölçeklendirme

Sonraki öğreticilerde, Azure oy uygulama güncelleştirilir ve Kubernetes küme izlemek için Operations Management Suite yapılandırılmış.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine bir uygulama bir kapsayıcı görüntü, Azure kapsayıcı kayıt defterine karşıya bu görüntü ve oluşturulan Kubernetes küme paketlenmiştir. Uygulama sonra Kubernetes kümede çalıştırıldı. 

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="manually-scale-pods"></a>Pod'ları el ile ölçeklendirin

Bu nedenle şimdiye kadar Azure oy ön uç ve Redis örnek silinmiş dağıtıldı, her tek bir çoğaltma ile. Doğrulamak için çalıştırın [kubectl almak](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) komutu.

```azurecli-interactive
kubectl get pods
```

Çıktı:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

El ile pod'ları içinde sayısını değiştirme `azure-vote-front` dağıtım kullanarak [kubectl ölçek](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) komutu. Bu örnek 5 sayısını artırır.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Çalıştırma [kubectl pod'ları alma](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) Kubernetes pod'ları oluşturmakta olduğunu doğrulayın. Bir dakika veya bunu sonra ek pod'ları çalıştırıyorsanız:

```azurecli-interactive
kubectl get pods
```

Çıktı:

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Otomatik ölçeklendirme pod'ları

Kubernetes destekleyen [yatay pod otomatik ölçeklendirmeyi](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) ayarlamak için pod'ları CPU kullanımına bağlı olarak bir dağıtımda sayısı veya diğer ölçümleri seçin. 

Autoscaler kullanmak için pod'ları CPU istekleri ve tanımlanan sınırları olması gerekir. İçinde `azure-vote-front` dağıtım, ön uç kapsayıcı 0,5 sınırına sahip istekleri 0,25 CPU CPU. Ayarları gibi görünür:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Aşağıdaki örnek kullanır [kubectl otomatik ölçeklendirme](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) pod'ları içinde sayısı için otomatik ölçeklendirme komutu `azure-vote-front` dağıtım. Burada, CPU kullanımı % 50 aşarsa, en fazla 10 için pod'ları autoscaler artırır.


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Autoscaler durumunu görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
kubectl get hpa
```

Çıktı:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Azure oy uygulama üzerinde minimum yük ile birkaç dakika sonra pod çoğaltmaların sayısı 3'e otomatik olarak azaltır.

## <a name="scale-the-agents"></a>Aracıları ölçeklendirme

Önceki öğreticide varsayılan komutlarını kullanarak Kubernetes kümenize oluşturduysanız, üç Aracısı düğüm yok. Daha fazla veya daha az sayıda kapsayıcı iş yükleri kümenizde düşünüyorsanız, aracı sayısı el ile ayarlayabilirsiniz. Kullanım [az acs ölçeklendirme](/cli/azure/acs#scale) komut ve aracılarla sayısını belirtin `--new-agent-count` parametresi.

Aşağıdaki örnek 4 adlı Kubernetes kümedeki Aracısı düğüm sayısını artırır *myK8sCluster*. Komut birkaç tamamlamak için dakika sürer.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Komut çıktısı aracı sayısı düğümleri değerinde gösterir `agentPoolProfiles:count`:

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, farklı ölçekleme özelliklerini Kubernetes kümenizdeki kullanılır. Görevleri dahil ele:

> [!div class="checklist"]
> * Kubernetes pod'ları el ile ölçeklendirme
> * Otomatik ölçeklendirme uygulaması ön ucu çalıştıran pod'ları yapılandırma
> * Kubernetes Azure Aracısı düğümleri ölçeklendirme

Kubernetes uygulamada güncelleştirmek hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Bir uygulamada Kubernetes güncelleştir](./container-service-tutorial-kubernetes-app-update.md)

