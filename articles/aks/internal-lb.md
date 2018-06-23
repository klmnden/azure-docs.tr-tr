---
title: Bir Azure Kubernetes hizmet (AKS) iç yük dengeleyici oluşturma
description: Bir iç yük dengeleyici Azure Kubernetes hizmet (AKS) kullanın.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 3/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 910a5c85d16cb46465598a77d5321cc0eed99744
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319257"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Bir iç yük dengeleyici Azure Kubernetes hizmet (AKS) kullanın

İç Yük Dengeleme Kubernetes hizmet Kubernetes kümesi olarak aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Bu belge, Azure Kubernetes hizmet (AKS) bir iç yük dengeleyici oluşturma ayrıntıları. İçinde iki SKU'ları Azure yük dengeleyici kullanılabilir: temel ve standart. AKS temel SKU kullanır.

## <a name="create-internal-load-balancer"></a>İç yük dengeleyici oluşturma

Bir iç yük dengeleyici oluşturmak için hizmet türü ile bir hizmet bildirimi yapı `LoadBalancer` ve `azure-load-balancer-internal` aşağıdaki örnekte görüldüğü gibi ek açıklama.

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

Uygulama dağıtıldıktan sonra bir Azure yük dengeleyici oluşturulur ve AKS kümesi olarak aynı sanal ağda kullanılabilir.

![AKS iç yük dengeleyici görüntüsü](media/internal-lb/internal-lb.png)

Ne zaman hizmeti alma ayrıntıları, IP adresi `EXTERNAL-IP` sütun iç yük dengeleyicisi IP adresidir.

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Bir IP adresi belirtin

İç yük dengeleyici ile belirli bir IP adresi kullanmak istiyorsanız, ekleme `loadBalancerIP` yük dengeleyici spec özelliğine. Belirtilen IP adresi AKS küme aynı alt ağda bulunmaları gerekir ve zaten kaynak atanmamaış olmalıdır.

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

Hizmet Ayrıntıları alırken, IP adresi `EXTERNAL-IP` belirtilen IP adresi yansıtmalıdır.

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="delete-the-load-balancer"></a>Yük Dengeleyici Sil

İç yük dengeleyicisi kullanarak tüm hizmetlerde sildikten sonra Yük Dengeleyici de silinir.

## <a name="next-steps"></a>Sonraki adımlar

Adresinde Kubernetes hizmetleri hakkında daha fazla bilgi [Kubernetes Hizmetleri belgeleri][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
