---
title: Görünüm kubelet günlüklerini Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) düğümlerden kubelet günlüklerinde sorun giderme bilgilerini görüntüle öğrenin
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 03/05/2019
ms.date: 04/08/2019
ms.author: v-yeche
ms.openlocfilehash: b381145fef7e6fb399fac3387ab01fdc9a51b154
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465028"
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Azure Kubernetes Service (AKS) kümesi düğümlerinden kubelet günlüklerini alma

Bir AKS kümesi çalışan bir parçası olarak bir sorunu gidermek için günlükleri gözden geçirmek gerekebilir. Günlüklerini görüntüleme yeteneği Azure portalında yerleşiktir [AKS ana bileşenleri] [ aks-master-logs] veya [bir AKS kümesindeki kapsayıcılar][azure-container-logs]. Bazen, almanız gerekebilir *kubelet* bir AKS düğümü sorun giderme amacıyla günlükleri.

Bu makalede, nasıl kullanabileceğinizi gösterir. `journalctl` görüntülemek için *kubelet* bir AKS düğümde günlüğe kaydeder.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

## <a name="create-an-ssh-connection"></a>Bir SSH bağlantısı oluşturun

İlk olarak, bir SSH bağlantısı üzerinde görüntülemek için ihtiyacınız olan düğüm oluşturma *kubelet* günlükleri. Bu işlem, ayrıntılı [Azure Kubernetes Service (AKS) kümesi düğümleri içine SSH] [ aks-ssh] belge.

## <a name="get-kubelet-logs"></a>Kubelet günlüklerini alma

Düğüme bağlandıktan sonra çekmek için aşağıdaki komutu çalıştırın *kubelet* günlükleri:

```console
sudo journalctl -u kubelet -o cat
```

Aşağıdaki örnek çıktı gösterilmektedir *kubelet* oturum verileri:

```
I0508 12:26:17.905042    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:27.943494    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:28.920125    8672 server.go:796] GET /stats/summary: (10.370874ms) 200 [[Ruby] 10.244.0.2:52292]
I0508 12:26:37.964650    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:47.996449    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:58.019746    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:05.107680    8672 server.go:796] GET /stats/summary/: (24.853838ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:27:08.041736    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:18.068505    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:28.094889    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:38.121346    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:44.015205    8672 server.go:796] GET /stats/summary: (30.236824ms) 200 [[Ruby] 10.244.0.2:52588]
I0508 12:27:48.145640    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:58.178534    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:05.040375    8672 server.go:796] GET /stats/summary/: (27.78503ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:28:08.214158    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:18.242160    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:28.274408    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:38.296074    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:48.321952    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:58.344656    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes yöneticisinden ek sorun giderme bilgisi gerekirse bkz [Kubernetes AKS ana düğüm günlüklerini görüntüleyin][aks-master-logs].

<!-- LINKS - internal -->
[aks-ssh]: ssh.md
[aks-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-master-logs]: view-master-logs.md
[azure-container-logs]: ../azure-monitor/insights/container-insights-overview.md