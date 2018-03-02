---
title: "Azure kapsayıcı hizmeti (AKS) küme düğümleri içine SSH"
description: "Bir SSH bağlantısı ile bir Azure kapsayıcı hizmeti (AKS) küme düğümleri oluşturun."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/28/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 00affc3d1c02c477826261aeac6e092934037e81
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>Azure kapsayıcı hizmeti (AKS) küme düğümleri içine SSH

Bazen, bir Azure kapsayıcı hizmeti (AKS) düğümü bakım, günlük toplama veya diğer sorun giderme işlemleri için erişim gerekebilir. Azure kapsayıcı hizmeti (AKS) düğümleri Internet'e açık değildir. Bir AKS düğümle bir SSH bağlantısı oluşturmak için bu belgede ayrıntılı adımları kullanın.

## <a name="configure-ssh-access"></a>SSH erişimi yapılandırma

 Belirli bir düğüme içine SSH için bir pod ile oluşturulan `hostNetwork` erişim. Bir hizmet de pod erişim için oluşturulur. Bu yapılandırma, ayrıcalıklı ve kullanıldıktan sonra kaldırılmalıdır.

Adlı bir dosya oluşturun `aks-ssh.yaml` ve bu bildirimde kopyalayın. Düğüm adı hedef AKS düğümün adı ile güncelleştirin.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-ssh
spec:
  selector:
    app: aks-ssh
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 22
    targetPort: 22
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aks-ssh
  labels:
    app: aks-ssh
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-ssh
  template:
    metadata:
      labels:
        app: aks-ssh
    spec:
      containers:
      - name: alpine
        image: alpine:latest
        ports:
        - containerPort: 22
        command: ["/bin/sh", "-c", "--"]
        args: ["while true; do sleep 30; done;"]
      hostNetwork: true
      nodeName: aks-nodepool1-42032720-0
```

Pod ve hizmet oluşturmak için bildirim çalıştırın.

```azurecli-interactive
$ kubectl apply -f aks-ssh.yaml
```

Gösterilen service dış IP adresini alın. IP adresi yapılandırmasını tamamlamak bir dakika sürebilir. 

```azurecli-interactive
$ kubectl get service

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
kubernetes         ClusterIP      10.0.0.1      <none>          443/TCP        1d
aks-ssh            LoadBalancer   10.0.51.173   13.92.154.191   22:31898/TCP   17m
```

Oluşturma ssh bağlantısı. 

Varsayılan kullanıcı adı bir AKS kümesi için `azureuser`. Küme oluşturma sırasında bu hesabı değiştirilmişse, uygun yönetici kullanıcı adı yerine koyun. 

Anahtarınızı adresindeki değilse `~/ssh/id_rsa`, doğru konuma kullanarak sağlamak `ssh -i` bağımsız değişkeni.

```azurecli-interactive
$ ssh azureuser@13.92.154.191

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>SSH erişimini kaldırma

İşiniz bittiğinde, hizmet ve SSH erişim pod silin.

```azurecli-interactive
kubectl delete -f aks-ssh.yaml
```