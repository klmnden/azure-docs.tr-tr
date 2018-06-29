---
title: Bir Azure Kubernetes hizmet (AKS) küme sanal kubelet çalıştırın
description: Sanal kubelet Kubernetes kapsayıcıları Azure kapsayıcı örneklerini çalıştırmak için kullanın.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: iainfou
ms.openlocfilehash: 04fdb1620dc6e7147ed10ae6eeeaeb3eeae14b62
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097368"
---
# <a name="virtual-kubelet-with-aks"></a>AKS ile sanal Kubelet

Azure kapsayıcı örnekleri (ACI) Azure'da çalışan kapsayıcılar için barındırılan bir ortamda sağlar. ACI kullanırken, temel alınan işlem altyapısını yönetmek için gerek yoktur, Azure sizin için bu yönetim işler. Kapsayıcı içinde ACI çalıştırırken, çalışan her kapsayıcı için saniye olarak ücretlendirilirsiniz.

Bir standart Kubernetes düğümü ise gibi sanal Kubelet sağlayıcı Azure kapsayıcı örnekleri için kullanırken, Linux ve Windows kapsayıcıları bir kapsayıcı örneğinde zamanlanabilir. Bu yapılandırma, yönetim değeri ve maliyet avantajı kapsayıcı örnekleri ve Kubernetes özellikleri yararlanmak sağlar.

> [!NOTE]
> Sanal Kubelet Deneysel açık kaynaklı proje ve bu nedenle kullanılmalıdır. Dosya sorunları ve okuma sanal kubelet hakkında daha fazla katkıda bulunmak için bkz [sanal Kubelet GitHub proje][vk-github].

Bu belge üzerinde bir AKS kapsayıcı örnekleri için sanal Kubelet yapılandırma ayrıntıları.

## <a name="prerequisite"></a>Önkoşul

Bu belgede bir AKS kümesi olduğunu varsayar. AKS küme gerekirse bkz [Azure Kubernetes hizmet (AKS) hızlı başlangıç][aks-quick-start].

Azure CLI Sürüm etmeniz **2.0.33** veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

[Helm](https://docs.helm.sh/using_helm/#installing-helm) sanal Kubelet yüklemek için de gereklidir.

## <a name="installation"></a>Yükleme

Kullanım [az aks yükle-bağlayıcı] [ aks-install-connector] sanal Kubelet yüklemek için komutu. Aşağıdaki örnek Linux ve Windows bağlayıcı dağıtır.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Bu bağımsız değişkenler kullanılabilir `aks install-connector` komutu.

| Bağımsız değişken: | Açıklama | Gerekli |
|---|---|:---:|
| `--connector-name` | ACI bağlayıcı adı.| Evet |
| `--name` `-n` | Yönetilen küme adıdır. | Evet |
| `--resource-group` `-g` | Kaynak grubunun adı. | Evet |
| `--os-type` | Kapsayıcı örnekleri işletim sistemi türü. İzin verilen değerler: her ikisi de, Linux, Windows. Varsayılan: Linux. | Hayır |
| `--aci-resource-group` | Kaynak grubunu ACI kapsayıcı grupları oluşturun. | Hayır |
| `--location` `-l` | ACI kapsayıcı grupları oluşturmak için konum. | Hayır |
| `--service-principal` | Azure API kimlik doğrulaması için kullanılan hizmet sorumlusu. | Hayır |
| `--client-secret` | Hizmet sorumlusu ilişkili gizli anahtarı. | Hayır |
| `--chart-url` | URL bir Helm grafiğin ACI Bağlayıcısı yüklenir. | Hayır |
| `--image-tag` | Sanal kubelet kapsayıcı görüntü resim etiketi. | Hayır |

## <a name="validate-virtual-kubelet"></a>Sanal Kubelet doğrula

Sanal Kubelet yüklendiğini doğrulamak için kullanarak Kubernetes düğümler listesini döndürmek [kubectl alma düğümleri] [ kubectl-get] komutu.

```console
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Linux kapsayıcı çalıştırın

Adlı bir dosya oluşturun `virtual-kubelet-linux.yaml` ve aşağıdaki YAML kopyalayın. Değiştir `kubernetes.io/hostname` Linux sanal Kubelet düğümün adı ile değer. Not alın bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] düğümde kapsayıcı zamanlamak için kullanılır.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-linux
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Uygulamayla çalıştırmak [kubectl oluşturma] [ kubectl-create] komutu.

```azurecli-interactive
kubectl create -f virtual-kubelet-linux.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` pod'ları zamanlanmış düğümle listesini çıkarmak için bağımsız değişken. Dikkat `aci-helloworld` pod zamanlandı `virtual-kubelet-virtual-kubelet-linux` düğümü.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Windows kapsayıcı çalıştırın

Adlı bir dosya oluşturun `virtual-kubelet-windows.yaml` ve aşağıdaki YAML kopyalayın. Değiştir `kubernetes.io/hostname` değeri ile Windows sanal Kubelet düğümün adı. Not alın bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] düğümde kapsayıcı zamanlamak için kullanılır.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: nanoserver/iis
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Uygulamayla çalıştırmak [kubectl oluşturma] [ kubectl-create] komutu.

```azurecli-interactive
kubectl create -f virtual-kubelet-windows.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` pod'ları zamanlanmış düğümle listesini çıkarmak için bağımsız değişken. Dikkat `nanoserver-iis` pod zamanlandı `virtual-kubelet-virtual-kubelet-win` düğümü.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Sanal Kubelet Kaldır

Kullanım [az aks Kaldır-bağlayıcı] [ aks-remove-connector] sanal Kubelet kaldırmak için komutu. Bağımsız değişken değerleri bağlayıcı, AKS küme ve AKS küme kaynak grubu adı ile değiştirin.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal Kubelet hakkında daha fazla bilgiyi [sanal Kubelet Github projet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az_aks_list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
