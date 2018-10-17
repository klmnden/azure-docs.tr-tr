---
title: Hızlı Başlangıç - Linux için Azure Kubernetes kümesi
description: Azure CLI ile AKS'de Linux kapsayıcıları için Kubernetes kümesi oluşturmayı hızlı bir şekilde öğrenin.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: iainfou
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: caf3607dbd33d75916ff65b0ab498fa228e2a823
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068928"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster"></a>Hızlı Başlangıç: Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Bu hızlı başlangıçta, Azure CLI kullanılarak AKS kümesi dağıtılır. Ardından web ön ucu ve bir Redis örneğinden oluşan çok kapsayıcılı bir uygulama küme üzerinde çalıştırılır. Tamamlandığında, uygulamaya İnternet üzerinden erişilebilir.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

Bu hızlı başlangıçta temel Kubernetes kavramlarını bildiğiniz varsayılmıştır. Kubernetes hakkında ayrıntılı bilgi için bkz. [Kubernetes belgeleri][kubernetes-documentation].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.46 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynaklarınızın Azure'da çalıştığı yerdir.

Aşağıdaki örnekte *eastus* konumunda *myAKSCluster* adlı bir kaynak grubu oluşturulur.

```azurecli-interactive
az group create --name myAKSCluster --location eastus
```

Çıktı:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myAKSCluster",
  "location": "eastus",
  "managedBy": null,
  "name": "myAKSCluster",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma

AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnekte, bir düğüm ile *myAKSCluster* adlı bir küme oluşturulmuştur. *--enable-addons monitoring* parametresiyle Kapsayıcılar için Azure İzleyici de etkinleştirilmiştir. Kapsayıcı durumu izleme çözümünü etkinleştirme hakkında daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti durumunu izleme][aks-monitor].

```azurecli-interactive
az aks create --resource-group myAKSCluster --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON tarafından biçimlendirilmiş bilgiler gösterilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi [kubectl][kubectl]’i kullanın.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. Yerel olarak yüklemek istiyorsanız [az aks install-cli][az-aks-install-cli] komutunu kullanın.


```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu adım kimlik bilgilerini indirir ve Kubernetes CLI’yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın. Düğümlerin görünmesi birkaç dakika sürebilir.

```azurecli-interactive
kubectl get nodes
```

Çıktı:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası, hangi kapsayıcı görüntülerinin çalıştırılması gerektiği de dahil olmak üzere, küme için istenen durumu tanımlar. Bu örnekte, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirim, biri Azure Vote Python uygulamaları için, diğeri de Redis örneği için olmak üzere iki [Kubernetes dağıtımı][kubernetes-deployment] içerir. Ayrıca, Redis örneği için bir iç hizmet ve İnternet’ten Azure Vote uygulamasına erişmek için bir dış hizmet olmak üzere iki [Kubernetes Hizmeti][kubernetes-service] oluşturulur.

`azure-vote.yaml` adlı bir dosya oluşturun ve dosyayı aşağıdaki YAML koduna kopyalayın. Azure Cloud Shell'de çalışıyorsanız, bu dosya bir sanal veya fiziksel sistemde olduğu gibi vi veya Nano kullanılarak oluşturulabilir.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Uygulamayı çalıştırmak için [kubectl apply][kubectl-apply] komutunu kullanın.

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Çıktı:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştırıldığında, uygulama ön ucunu İnternet üzerinden kullanıma sunan bir [Kubernetes hizmeti][kubernetes-service] oluşturulur. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* için *EXTERNAL-IP* durumu *pending* olarak görünür.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*EXTERNAL-IP* adresi *pending* durumundan *IP address* değerine değiştiğinde kubectl izleme işlemini durdurmak için `CTRL-C` komutunu kullanın.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Artık Azure Vote Uygulamasını görmek için dış IP adresine göz atabilirsiniz.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Sistem durumunu ve günlükleri izleme

AKS kümesi oluşturulduğunda hem küme düğümleri hem de pod'lar için sistem durumu ölçümlerini yakalamak için izleme özellikleri etkinleştirilmiştir. Bu sistem durumu ölçümleri Azure portaldan kullanılabilir. Küme durumu izleme hakkında daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti durumunu izleme][aks-monitor].

Azure Vote pod'larınızın geçerli durum, çalışma süresi ve kaynak kullanımı bilgilerini görmek için aşağıdaki adımları uygulayın:

1. Web tarayıcısından Azure portalı [https://portal.azure.com][azure-portal] açın.
1. *myResourceGroup* gibi bir kaynak grubunu ve *myAKSCluster* gibi bir AKS kümesini seçin.
1. Sol taraftaki **İzleme** bölümünde **İçgörüler (önizleme)** öğesini seçin
1. Üst taraftan **+ Filtre Ekle**'yi seçin
1. *Ad alanı* özelliğini ve ardından *\<kube-system hariç tümünü\> seçin*
1. **Kapsayıcılar** bölümünü görüntüleyin.

Aşağıdaki örnekte olduğu gibi *azure-vote-back* ve *azure-vote-front* kapsayıcıları görüntülenir:

![AKS'de çalışan kapsayıcıların durumunu görüntüleme](media/kubernetes-walkthrough-portal/monitor-containers.png)

`azure-vote-front` pod'unun günlüklerini görmek için kapsayıcı listesinin sağ tarafındaki **Kapsayıcı günlüklerini görüntüle** bağlantısını seçin. Bu günlükler, kapsayıcıdaki *stdout* ve *stderr* akışlarını içerir.

![AKS'deki kapsayıcı günlüklerini görüntüleme](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Kümeyi silme

Kümeye artık ihtiyacınız yoksa [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırın.

```azurecli-interactive
az group delete --name myAKSCluster --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanılır. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
