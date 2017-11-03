---
title: "Hızlı Başlangıç - Linux için Azure Kubernetes kümesi | Microsoft Docs"
description: "Azure CLI ile AKS Linux kapsayıcıları için Kubernetes küme oluşturmak hızlı bir şekilde öğrenin."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: a25f91d092c2f72ea1cbc174d1bf8bf48885788a
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Bir Azure kapsayıcı hizmeti (AKS) kümeyi dağıtma

Bu Hızlı Başlangıç, Azure CLI kullanarak bir AKS küme dağıtılır. Web ön uç ve bir Redis örneği oluşan çok kapsayıcı uygulama sonra küme üzerinde çalıştırın. Tamamlandığında, uygulamaya İnternet üzerinden erişilebilir.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

Bu hızlı başlangıç Kubernetes kavramlar, Kubernetes bakın hakkında ayrıntılı bilgi için temel bir anlayış varsayar [Kubernetes belgelerine]( https://kubernetes.io/docs/home/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI Sürüm 2.0.20 çalıştırıyorsanız bu hızlı başlangıç yükleyip CLI yerel olarak kullanmak seçerseniz, gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Azure aboneliğiniz için AKS Önizleme etkinleştirme
AKS önizlemede olsa da, yeni küme oluşturma aboneliğinizi bir özellik bayrağı gerektirir. Bu özellik için kullanmak istediğiniz abonelikleri herhangi bir sayıda isteyebilir. Kullanım `az provider register` komutu AKS sağlayıcısını kaydetmek için:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Kaydolduktan sonra artık ile AKS Kubernetes küme oluşturmak hazırsınız.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur.

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *westus2* konumu.

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Çıktı:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westus2",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma

Aşağıdaki örnek adlı bir küme oluşturur *myK8sCluster* bir düğümü.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8sCluster --agent-count 1 --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında json tarafından biçimlendirilmiş bilgiler gösterilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi [kubectl](https://kubernetes.io/docs/user-guide/kubectl/)’i kullanın.

Azure bulut Kabuk kullanıyorsanız, kubectl zaten yüklü. Yerel olarak yüklemek istiyorsanız, aşağıdaki komutu çalıştırın.


```azurecli
az aks install-cli
```

Kubernetes kümenize bağlanmak için kubectl yapılandırmak için aşağıdaki komutu çalıştırın. Bu adım kimlik bilgilerini indirir ve Kubernetes CLI’yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Çıktı:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası, hangi kapsayıcı görüntülerinin çalıştırılması gerektiği de dahil olmak üzere, küme için istenen durumu tanımlar. Bu örnekte, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır.

`azure-vote.yml` adlı bir dosya oluşturun ve dosyayı aşağıdaki YAML’ye kopyalayın. Azure Cloud Shell'de çalışıyorsanız, bu dosya bir sanal veya fiziksel sistemde olduğu gibi vi veya Nano kullanılarak oluşturulabilir.

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
        image: microsoft/azure-vote-front:redis-v1
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

Uygulamayı çalıştırmak için [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) komutunu kullanın.

```azurecli-interactive
kubectl create -f azure-vote.yml
```

Çıktı:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştırıldığında, uygulama ön ucunu İnternet üzerinden kullanıma sunan bir [Kubernetes hizmeti](https://kubernetes.io/docs/concepts/services-networking/service/) oluşturulur. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* için *EXTERNAL-IP* durumu *pending* olarak görünür.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Bir kez *dış IP* adresi değiştiğini *bekleyen* için bir *IP adresi*, kullanın `CTRL-C` kubectl izleme işlemi durdurmak için.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.175.236.185   80:30572/TCP   2m
```

Artık Azure Vote Uygulamasını görmek için dış IP adresine göz atabilirsiniz.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Kümeyi silme
Kümeye artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanılır. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam bir kod yol Kubernetes küme Öğreticisine devam.

> [!div class="nextstepaction"]
> [AKS küme yönetme](./tutorial-kubernetes-prepare-app.md)
