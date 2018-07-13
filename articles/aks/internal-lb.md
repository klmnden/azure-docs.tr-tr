---
title: Azure Kubernetes Service (AKS) iç yük dengeleyici oluşturma
description: Oluşturma ve hizmetlerinizi Azure Kubernetes Service (AKS) ile kullanıma sunmak için iç yük dengeleyici kullanma hakkında bilgi edinin.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/12/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 123fc08995416e0ff9c7e12a526deadc34b3a4a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001404"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile iç yük dengeleyici kullanın

İç Yük Dengeleme bir Kubernetes hizmeti olarak Kubernetes kümesi aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Bu makalede oluşturma ve Azure Kubernetes Service (AKS) ile iç yük dengeleyici kullanma gösterilmektedir. Azure Load Balancer iki SKU ile sunulur: temel ve standart. AKS, temel SKU kullanır.

## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

İç yük dengeleyici oluşturmak için hizmet türü ile bir hizmet bildirimi oluşturma *LoadBalancer* ve *azure-load-balancer-iç* aşağıdaki örnekte gösterildiği gibi ek açıklama:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Dağıtıldıktan sonra ile `kubetctl apply`, Azure yük dengeleyici oluşturulur ve AKS kümesi ile aynı sanal ağda kullanılabilir.

![AKS iç yük dengeleyici görüntüsü](media/internal-lb/internal-lb.png)

Hizmet ayrıntılarını görüntülediğinizde, IP adresi *EXTERNAL-IP* sütun, aşağıdaki örnekte gösterildiği gibi iç yük dengeleyici IP adresi olan:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Bir IP adresi belirtin

Belirli bir IP adresi ile iç yük dengeleyici kullanmak istiyorsanız, ekleme *loadBalancerIP* özelliğini yük dengeleyici belirtimi. Belirtilen IP adresi, AKS kümesi aynı alt ağda bulunması gerekir ve zaten kaynak atanmamaış olmalıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Hizmet ayrıntılarını görüntülediğinizde, IP adresi *EXTERNAL-IP* belirtilen IP adresi yansıtır:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Özel ağları kullanın

AKS kümenizi yeniden oluşturduğunuzda, Gelişmiş Ağ ayarları belirtebilirsiniz. Bu yaklaşım küme var olan bir Azure sanal ağı ve alt ağlar dağıtmanıza olanak tanır. Şirket içi ortamınıza bağlı özel bir ağ, AKS kümesi dağıtmayı ve hizmetler yalnızca erişilebilir dahili olarak çalıştırmak için tek bir senaryodur. Daha fazla bilgi için [AKS Gelişmiş ağ yapılandırmasında][advanced-networking].

Önceki adımları herhangi bir değişiklik, özel bir ağ kullanan bir AKS kümesi içinde iç yük dengeleyici dağıtmak için gereklidir. Yük Dengeleyici, AKS kümenizin aynı kaynak grubunda oluşturulur, ancak aşağıdaki örnekte gösterildiği gibi özel sanal ağ ve alt ağına bağlı:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Hizmet sorumlusu AKS kümenizin iznine ihtiyaç duyabilirsiniz *ağ Katılımcısı* rolü için Azure sanal ağı kaynaklarınızın dağıtıldığı kaynak grubu. Hizmet sorumlusuyla görüntülemek [az aks show][az-aks-show], gibi `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Bir rol ataması oluşturmak için kullanın [az rol ataması oluşturma] [ az-role-assignment-create] komutu.

## <a name="specify-a-different-subnet"></a>Farklı bir alt ağ belirtin.

Load balancer'ınız için bir alt ağ belirtmek için *azure load-balancer-iç-alt* hizmetiniz için ek açıklama. Belirtilen alt ağ, AKS kümenizin aynı sanal ağda olması gerekir. Dağıtıldığında, yük dengeleyici *EXTERNAL-IP* adresi belirtilen alt ağın bir parçasıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="delete-the-load-balancer"></a>Yük Dengeleyiciyi Sil

İç yük dengeleyici kullanan tüm hizmetleri silindiğinde, yük dengeleyici de silinir.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes hizmetleri hakkında daha fazla bilgi [Kubernetes Hizmetleri belgeleri][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[advanced-networking]: networking-overview.md
[deploy-advanced-networking]: networking-overview.md#configure-networking---cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create