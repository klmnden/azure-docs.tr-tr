---
title: Azure Kubernetes hizmet (AKS) kubelet günlüklerini alma
description: Azure Kubernetes hizmet (AKS) küme düğümlerinden kubelet günlüklerini alma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0467be7e91fdbf4685fc41a375ea86a503e26009
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Azure Kubernetes hizmet (AKS) küme düğümlerinden kubelet günlüklerini alma

Bazen, sorun giderme amacıyla bir Azure Kubernetes hizmet (AKS) düğümden kubelet günlükleri yapmanız gerekebilir. Bu belgede, bu günlükler çekmek için bir seçenek ayrıntıları verilmektedir.

## <a name="create-an-ssh-connection"></a>Bir SSH bağlantısı oluşturma

İlk olarak, bir SSH bağlantısı kubelet günlüklerini gerek düğümle oluşturun. Bu işlem içinde ayrıntılı [Azure Kubernetes hizmet (AKS) küme düğümleri içine SSH] [ aks-ssh] belge.

## <a name="get-kubelet-logs"></a>Kubelet günlüklerini alma

Bir kez kubelet günlükleri çıkarmak için aşağıdaki komutu çalıştırın düğümü bağlandınız.

```azurecli-interactive
docker logs $(docker ps -a | grep "hyperkube kubele" | awk -F ' ' '{print $1}')
```

Örnek çıktı:

```console
2018-03-05 00:04:00.883195 I | proto: duplicate proto type registered: google.protobuf.Any
2018-03-05 00:04:00.883242 I | proto: duplicate proto type registered: google.protobuf.Duration
2018-03-05 00:04:00.883253 I | proto: duplicate proto type registered: google.protobuf.Timestamp
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
I0305 00:04:00.978560    8021 feature_gate.go:144] feature gates: map[Accelerators:true]
I0305 00:04:00.978996    8021 azure.go:174] azure: using client_id+client_secret to retrieve access token
I0305 00:04:00.979041    8021 server.go:439] Successfully initialized cloud provider: "azure" from the config file: "/etc/kubernetes/azure.json"
I0305 00:04:00.979058    8021 server.go:740] cloud provider determined current node name to be aks-nodepool1-42032720-0
```

<!-- LINKS - internal -->
[aks-ssh]: aks-ssh.md