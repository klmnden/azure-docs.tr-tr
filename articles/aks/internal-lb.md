---
title: Azure Kubernetes Service (AKS) bir iç yük dengeleyici oluşturma
description: Oluşturma ve hizmetlerinizi Azure Kubernetes Service (AKS) ile kullanıma sunmak için iç yük dengeleyici kullanma hakkında bilgi edinin.
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 03/04/2019
ms.date: 04/08/2019
ms.author: v-yeche
ms.openlocfilehash: a26eab83f567a46f613e3bfda95fd99aba2b79c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465555"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile iç yük dengeleyici kullanın

Azure Kubernetes Service (AKS) ve uygulamalarınıza erişimi kısıtlamak için oluşturabilir ve iç yük dengeleyici kullanın. İç yük dengeleyici, bir Kubernetes hizmeti yalnızca Kubernetes kümesi aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Bu makalede oluşturma ve Azure Kubernetes Service (AKS) ile iç yük dengeleyici kullanma gösterilmektedir.

> [!NOTE]
> Azure Load Balancer iki SKU ile - kullanılabilir *temel* ve *standart*. AKS tarafından desteklenen *temel* SKU. Kullanmak istiyorsanız *standart* SKU, kullanabileceğiniz Yukarı Akış [aks altyapısı][aks-engine]. Daha fazla bilgi için [Azure yük dengeleyici SKU karşılaştırma][azure-lb-comparison].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

İç yük dengeleyici oluşturmak için adlandırılmış bir hizmet bildirimi oluşturma `internal-lb.yaml` hizmet türü ile *LoadBalancer* ve *azure-load-balancer-iç* aşağıda gösterildiği gibi ek açıklaması Örnek:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

Kullanarak iç yük dengeleyici dağıtma [kubectl uygulamak] kubectl-uygulama] ve YAML bildiriminizi adını belirtin:

```console
kubectl apply -f internal-lb.yaml
```

Azure load balancer düğüm kaynak grubunda oluşturulur ve AKS kümesi ile aynı sanal ağa bağlanır.

Hizmet ayrıntılarını görüntülediğinizde, iç yük dengeleyici IP adresi gösterilen *EXTERNAL-IP* sütun. Bu bağlamda *dış* yük dengeleyicinin dış arabirimi ile ilgili bir genel, dış IP adresi alan değil. Bir veya değiştirmek IP adresi için iki dakika sürebilir *\<bekleyen\>* gerçek iç IP adresine, aşağıdaki örnekte gösterildiği gibi:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   2m
```

## <a name="specify-an-ip-address"></a>Bir IP adresi belirtin

Belirli bir IP adresi ile iç yük dengeleyici kullanmak istiyorsanız, ekleme *loadBalancerIP* yük dengeleyici YAML bildirim özelliği. Belirtilen IP adresi, AKS kümesi aynı alt ağda bulunması gerekir ve zaten kaynak atanmamaış olmalıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: internal-app
```

Dağıtıldığında ve hizmet ayrıntılarını, IP adresini *EXTERNAL-IP* sütun belirtilen IP adresiniz yansıtır:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Özel ağları kullanın

AKS kümenizi yeniden oluşturduğunuzda, Gelişmiş Ağ ayarları belirtebilirsiniz. Bu yaklaşım küme var olan bir Azure sanal ağı ve alt ağlar dağıtmanıza olanak tanır. Şirket içi ortamınıza bağlı özel bir ağ, AKS kümesi dağıtmayı ve hizmetler yalnızca erişilebilir dahili olarak çalıştırmak için tek bir senaryodur. Daha fazla bilgi için kendi sanal ağ alt ağları ile yapılandırma [Kubernetes] [ use-kubenet] veya [Azure CNI][advanced-networking].

Önceki adımları herhangi bir değişiklik, özel bir ağ kullanan bir AKS kümesi içinde iç yük dengeleyici dağıtmak için gereklidir. Yük Dengeleyici, AKS kümenizin aynı kaynak grubunda oluşturulur, ancak aşağıdaki örnekte gösterildiği gibi özel sanal ağ ve alt ağına bağlı:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Hizmet sorumlusu AKS kümenizin iznine ihtiyaç duyabilirsiniz *ağ Katılımcısı* rolü için Azure sanal ağı kaynaklarınızın dağıtıldığı kaynak grubu. Hizmet sorumlusuyla görüntülemek [az aks show][az-aks-show], gibi `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Bir rol ataması oluşturmak için kullanın [az rol ataması oluşturma] [ az-role-assignment-create] komutu.

## <a name="specify-a-different-subnet"></a>Farklı bir alt ağ belirtin.

Load balancer'ınız için bir alt ağ belirtmek için *azure load-balancer-iç-alt* hizmetiniz için ek açıklama. Belirtilen alt ağ, AKS kümenizin aynı sanal ağda olması gerekir. Dağıtıldığında, yük dengeleyici *EXTERNAL-IP* adresi belirtilen alt ağın bir parçasıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

## <a name="delete-the-load-balancer"></a>Yük Dengeleyiciyi Sil

İç yük dengeleyici kullanan tüm hizmetleri silindiğinde, yük dengeleyici de silinir.

Hizmet olarak herhangi bir Kubernetes kaynak ile gibi doğrudan silme `kubectl delete service internal-app`, ardından da silen temel Azure load balancer.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes hizmetleri hakkında daha fazla bilgi [Kubernetes Hizmetleri belgeleri][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[az-aks-show]: https://docs.azure.cn/zh-cn/cli/aks?view=azure-cli-latest#az-aks-show
[az-role-assignment-create]: https://docs.azure.cn/zh-cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-create
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[use-kubenet]: configure-kubenet.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest