---
title: Bir Azure Kubernetes Service (AKS) kümesi içinde Virtual Kubelet çalıştırın
description: Azure Container Instances üzerinde Linux ve Windows kapsayıcılarını çalıştırmaya yönelik, Virtual Kubelet Azure Kubernetes Service (AKS) kullanmayı öğrenin.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: iainfou
ms.openlocfilehash: 2730ab1d909ead0431f0dd7fd0061d3080834296
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443741"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'i (AKS) ile sanal Kubelet kullanın

Azure Container Instances'a (ACI) Azure'da kapsayıcıları çalıştırmak için barındırılan bir ortam sağlar. ACI kullanırken, temel alınan bilgi işlem altyapısını yönetme gerek yoktur, bu yönetim, her şeyi Azure gerçekleştirir. ACI çalışan kapsayıcılar, her çalışmakta olan kapsayıcıyı saniye olarak ücretlendirilir.

Standart bir Kubernetes düğümü ise gibi hem Linux hem de Windows kapsayıcıları, Virtual Kubelet sağlayıcısı için Azure Container Instances kullanarak, bir kapsayıcı örneği üzerinde zamanlanabilir. Bu yapılandırma, Kubernetes yeteneklerini ve container Instances yönetim değeri ve maliyet avantajı yararlanmasına olanak sağlar.

> [!NOTE]
> Sanal Kubelet, Deneysel bir açık kaynak bir projedir ve bu nedenle kullanılmalıdır. Katkıda bulunmak için dosya sorunları ve okuma sanal kubelet hakkında daha fazla bilgi bkz [sanal Kubelet GitHub projesini][vk-github].

Bu belge, bir AKS Virtual Kubelet container Instances için yapılandırma ayrıntıları.

## <a name="prerequisite"></a>Önkoşul

Bu belge, bir AKS kümesi olduğunu varsayar. Bir AKS kümesi gerekirse bkz [Azure Kubernetes Service (AKS) hızlı başlangıç][aks-quick-start].

Ayrıca Azure CLI Sürüm ihtiyacınız **2.0.33** veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Virtual Kubelet yüklemek için [Helm](https://docs.helm.sh/using_helm/#installing-helm) de gereklidir.

### <a name="for-rbac-enabled-clusters"></a>Kümeler için RBAC etkin

AKS kümenizi RBAC etkinse, hizmet hesabını ve kullanmak için rol bağlama Tiller ile oluşturmanız gerekir. Daha fazla bilgi için [Helm rol tabanlı erişim denetimi][helm-rbac].

A *ClusterRoleBinding* için Virtual Kubelet oluşturulmalıdır. Bir bağlamayı oluşturmak için adlı bir dosya oluşturun. *rbac virtualkubelet.yaml* aşağıdaki tanımını yapıştırın:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: virtual-kubelet
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: default
  namespace: default
```

Bağlama ile uygulama [kubectl uygulamak] [ kubectl-apply] ve belirtin, *rbac virtualkubelet.yaml* aşağıdaki örnekte gösterildiği gibi dosya:

```
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/virtual-kubelet created
```

Şimdi, AKS kümenizi Virtual Kubelet yüklemeye devam edebilirsiniz.

## <a name="installation"></a>Yükleme

Kullanım [az aks yükleme-connector] [ aks-install-connector] Virtual Kubelet yüklemek için komutu. Aşağıdaki örnek Linux ve Windows bağlayıcı dağıtır.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Bu bağımsız değişkenler kullanılabilir `aks install-connector` komutu.

| Bağımsız değişkeni: | Açıklama | Gerekli |
|---|---|:---:|
| `--connector-name` | ACI Bağlayıcısı adıdır.| Evet |
| `--name` `-n` | Yönetilen kümesinin adı. | Evet |
| `--resource-group` `-g` | Kaynak grubunun adı. | Evet |
| `--os-type` | Kapsayıcı örnekleri işletim sistemi türü. İzin verilen değerler: her ikisi de, Linux, Windows. Varsayılan: Linux. | Hayır |
| `--aci-resource-group` | ACI kapsayıcı grubu oluşturulacağı kaynak grubu. | Hayır |
| `--location` `-l` | ACI kapsayıcı grubu oluşturulacağı konum. | Hayır |
| `--service-principal` | Hizmet sorumlusu kimlik doğrulaması için Azure API'leri için kullanılır. | Hayır |
| `--client-secret` | Hizmet sorumlusuyla ilişkili gizli anahtarı. | Hayır |
| `--chart-url` | ACI Bağlayıcısı yükleyen bir Helm grafiği URL'si. | Hayır |
| `--image-tag` | Sanal kubelet kapsayıcı görüntüsünün resim etiketi. | Hayır |

## <a name="validate-virtual-kubelet"></a>Sanal Kubelet doğrula

Virtual Kubelet yüklendiğini doğrulamak için Kubernetes düğümleri kullanarak listesini döndürmek [kubectl alma düğümleri] [ kubectl-get] komutu.

```
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Linux kapsayıcı çalıştırma

Adlı bir dosya oluşturun `virtual-kubelet-linux.yaml` aşağıdaki YAML'ye kopyalayın. Değiştirin `kubernetes.io/hostname` Linux Virtual Kubelet düğümün adı ile değeri. Bu Not bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] düğümdeki kapsayıcı zamanlamak için kullanılır.

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

Uygulamayı çalıştırın [kubectl oluşturma] [ kubectl-create] komutu.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` zamanlanmış düğümle pod'ların bir listesini çıkarmak için bağımsız değişken. Dikkat `aci-helloworld` pod zamanlandı `virtual-kubelet-virtual-kubelet-linux` düğümü.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Windows kapsayıcı çalıştırma

Adlı bir dosya oluşturun `virtual-kubelet-windows.yaml` aşağıdaki YAML'ye kopyalayın. Değiştirin `kubernetes.io/hostname` Windows Virtual Kubelet düğümün adı ile değeri. Bu Not bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] düğümdeki kapsayıcı zamanlamak için kullanılır.

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

Uygulamayı çalıştırın [kubectl oluşturma] [ kubectl-create] komutu.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` zamanlanmış düğümle pod'ların bir listesini çıkarmak için bağımsız değişken. Dikkat `nanoserver-iis` pod zamanlandı `virtual-kubelet-virtual-kubelet-win` düğümü.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Sanal Kubelet Kaldır

Kullanım [az aks remove-connector] [ aks-remove-connector] Virtual Kubelet kaldırmak için komutu. Bağımsız değişken değerlerini bağlayıcı, AKS kümesi ve AKS küme kaynak grubu adıyla değiştirin.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Sonraki adımlar

Virtual Kubelet hakkında daha fazla bilgiyi [sanal Kubelet Github projet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
