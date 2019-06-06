---
title: Bir Azure Kubernetes Service (AKS) kümesi içinde Virtual Kubelet çalıştırın
description: Azure Container Instances üzerinde Linux ve Windows kapsayıcılarını çalıştırmaya yönelik, Virtual Kubelet Azure Kubernetes Service (AKS) kullanmayı öğrenin.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: cc0c3becf21cb54b97a88e9ba35b38308af81a85
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475414"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'i (AKS) ile sanal Kubelet kullanın

Azure Container Instances'a (ACI) Azure'da kapsayıcıları çalıştırmak için barındırılan bir ortam sağlar. ACI kullanırken, temel alınan bilgi işlem altyapısını yönetme gerek yoktur, bu yönetim, her şeyi Azure gerçekleştirir. ACI çalışan kapsayıcılar, her çalışmakta olan kapsayıcıyı saniye olarak ücretlendirilir.

Standart bir Kubernetes düğümü ise gibi hem Linux hem de Windows kapsayıcıları, Virtual Kubelet sağlayıcısı için Azure Container Instances kullanarak, bir kapsayıcı örneği üzerinde zamanlanabilir. Bu yapılandırma, Kubernetes yeteneklerini ve container Instances yönetim değeri ve maliyet avantajı yararlanmasına olanak sağlar.

> [!NOTE]
> AKS adlı ACI, kapsayıcıları zamanlamak için yerleşik destek sunuyor *sanal düğümü*. Bu sanal düğümü şu anda Linux kapsayıcı örneklerini destekler. Windows container Instances zamanlamak gerekiyorsa, Virtual Kubelet kullanmaya devam edebilirsiniz. Aksi takdirde, bu makalede belirtilen el ile Virtual Kubelet yönergeler yerine sanal düğümü kullanmanız gerekir. Sanal düğümleri kullanarak oluşturabileceğinize dair [Azure CLI] [ virtual-nodes-cli] veya [Azure portalında][virtual-nodes-portal].
>
> Sanal Kubelet, Deneysel bir açık kaynak bir projedir ve bu nedenle kullanılmalıdır. Katkıda bulunmak için dosya sorunları ve okuma sanal kubelet hakkında daha fazla bilgi bkz [sanal Kubelet GitHub projesini][vk-github].

## <a name="before-you-begin"></a>Başlamadan önce

Bu belge, bir AKS kümesi olduğunu varsayar. Bir AKS kümesi gerekirse bkz [Azure Kubernetes Service (AKS) hızlı başlangıç][aks-quick-start].

Ayrıca Azure CLI Sürüm ihtiyacınız **2.0.65** veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Virtual Kubelet yüklemek için yükleme ve yapılandırma [Helm] [ aks-helm] AKS kümenizde. Tiller olduğundan emin olun [Kubernetes RBAC ile kullanılmak üzere yapılandırılmış](#for-rbac-enabled-clusters), gerekirse.

### <a name="register-container-instances-feature-provider"></a>Container Instances özellik sağlayıcısını Kaydet

Azure Container örneği (ACI) hizmeti daha önce kullanmadıysanız, hizmet sağlayıcısı, aboneliğiniz ile kaydedin. ACI sağlayıcı kaydı kullanarak durumu denetleyebilirsiniz [az sağlayıcı listesi] [ az-provider-list] aşağıdaki örnekte gösterildiği gibi komut:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

*Microsoft.ContainerInstance* sağlayıcısı olarak raporlamalıdır *kayıtlı*aşağıdaki örnek çıktıda gösterildiği gibi:

```console
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Sağlayıcı olarak gösteriliyorsa *NotRegistered*, kullanarak sağlayıcısını kaydedin [az provider register] [ az-provider-register] aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>Kümeler için RBAC etkin

AKS kümenizi RBAC etkinse, hizmet hesabını ve kullanmak için rol bağlama Tiller ile oluşturmanız gerekir. Daha fazla bilgi için [Helm rol tabanlı erişim denetimi][helm-rbac]. Hizmet hesabını ve rol bağlama oluşturmak için adlı bir dosya oluşturun. *rbac sanal-kubelet.yaml* aşağıdaki tanımını yapıştırın:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Hizmet hesabını uygular ve ile bağlama [kubectl uygulamak] [ kubectl-apply] ve belirtin, *rbac sanal-kubelet.yaml* aşağıdaki örnekte gösterildiği gibi dosya:

```console
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Helm tiller hizmet hesabı kullanacak şekilde yapılandırın:

```console
helm init --service-account tiller
```

Şimdi, AKS kümenizi Virtual Kubelet yüklemeye devam edebilirsiniz.

## <a name="installation"></a>Yükleme

Kullanım [az aks yükleme-connector] [ aks-install-connector] Virtual Kubelet yüklemek için komutu. Aşağıdaki örnek Linux ve Windows bağlayıcı dağıtır.

```azurecli-interactive
az aks install-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

Bu bağımsız değişkenler kullanılabilir [az aks yükleme-connector] [ aks-install-connector] komutu.

| Bağımsız değişkeni: | Açıklama | Gerekli |
|---|---|:---:|
| `--connector-name` | ACI Bağlayıcısı adıdır.| Evet |
| `--name``-n` | Yönetilen kümesinin adı. | Evet |
| `--resource-group``-g` | Kaynak grubunun adı. | Evet |
| `--os-type` | Kapsayıcı örnekleri işletim sistemi türü. İzin verilen değerler: Both, Linux, Windows. Varsayılan: Linux. | Hayır |
| `--aci-resource-group` | ACI kapsayıcı grubu oluşturulacağı kaynak grubu. | Hayır |
| `--location``-l` | ACI kapsayıcı grubu oluşturulacağı konum. | Hayır |
| `--service-principal` | Hizmet sorumlusu kimlik doğrulaması için Azure API'leri için kullanılır. | Hayır |
| `--client-secret` | Hizmet sorumlusuyla ilişkili gizli anahtarı. | Hayır |
| `--chart-url` | ACI Bağlayıcısı yükleyen bir Helm grafiği URL'si. | Hayır |
| `--image-tag` | Sanal kubelet kapsayıcı görüntüsünün resim etiketi. | Hayır |

## <a name="validate-virtual-kubelet"></a>Sanal Kubelet doğrula

Virtual Kubelet yüklendiğini doğrulamak için Kubernetes düğümleri kullanarak listesini döndürmek [kubectl alma düğümleri] [ kubectl-get] komutu:

```console
$ kubectl get nodes

NAME                                             STATUS   ROLES   AGE   VERSION
aks-nodepool1-56577038-0                         Ready    agent   11m   v1.12.8
virtual-kubelet-virtual-kubelet-linux-eastus     Ready    agent   39s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
virtual-kubelet-virtual-kubelet-windows-eastus   Ready    agent   37s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
```

## <a name="run-linux-container"></a>Linux kapsayıcı çalıştırma

Adlı bir dosya oluşturun `virtual-kubelet-linux.yaml` aşağıdaki YAML'ye kopyalayın. Bu Not bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] düğümdeki kapsayıcı zamanlamak için kullanılır.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
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
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Uygulamayı çalıştırın [kubectl oluşturma] [ kubectl-create] komutu.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` zamanlanmış düğümle pod'ların bir listesini çıkarmak için bağımsız değişken. Dikkat `aci-helloworld` pod zamanlandı `virtual-kubelet-virtual-kubelet-linux` düğümü.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
aci-helloworld-7b9ffbf946-rx87g   1/1     Running   0          22s     52.224.147.210   virtual-kubelet-virtual-kubelet-linux-eastus
```

## <a name="run-windows-container"></a>Windows kapsayıcısı çalıştırma

Adlı bir dosya oluşturun `virtual-kubelet-windows.yaml` aşağıdaki YAML'ye kopyalayın. Bu Not bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] düğümdeki kapsayıcı zamanlamak için kullanılır.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Uygulamayı çalıştırın [kubectl oluşturma] [ kubectl-create] komutu.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` zamanlanmış düğümle pod'ların bir listesini çıkarmak için bağımsız değişken. Dikkat `nanoserver-iis` pod zamanlandı `virtual-kubelet-virtual-kubelet-windows` düğümü.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
nanoserver-iis-5d999b87d7-6h8s9   1/1     Running   0          47s     52.224.143.39    virtual-kubelet-virtual-kubelet-windows-eastus
```

## <a name="remove-virtual-kubelet"></a>Sanal Kubelet Kaldır

Kullanım [az aks remove-connector] [ aks-remove-connector] Virtual Kubelet kaldırmak için komutu. Bağımsız değişken değerlerini bağlayıcı, AKS kümesi ve AKS küme kaynak grubu adıyla değiştirin.

```azurecli-interactive
az aks remove-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

> [!NOTE]
> Her iki işletim sistemi Bağlayıcıları kaldırma hatalarla ya da yalnızca Windows veya Linux işletim sistemi bağlayıcıyı kaldırmak istediğiniz işletim sistemi türü el ile belirtebilirsiniz. Ekleme `--os-type` parametresi önceki `az aks remove-connector` komutunu ve belirtin `Windows` veya `Linux`.

## <a name="next-steps"></a>Sonraki adımlar

Virtual Kubelet olası sorunlar için bkz: [bilinen quirks ve geçici çözümler][vk-troubleshooting]. Virtual Kubelet ile yaşayabileceğiniz sorunları [açık bir GitHub sorunu][vk-issues].

Virtual Kubelet hakkında daha fazla bilgiyi [sanal Kubelet GitHub projesini][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md
[aks-helm]: kubernetes-helm.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
