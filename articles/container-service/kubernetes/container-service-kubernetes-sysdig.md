---
title: "Azure Kubernetes küme - Sysdig izleme"
description: "Kubernetes Sysdig kullanarak Azure kapsayıcı hizmeti kümesinde izleme"
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 4ff610f72af4e6a750749009f3cd4b4df632a37f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Sysdig kullanarak Azure kapsayıcı hizmeti Kubernetes küme izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuz, sahibi olduğunuzu varsayar [Azure kapsayıcı hizmeti kullanarak Kubernetes küme oluşturulan](container-service-kubernetes-walkthrough.md).

Ayrıca, azure CLI ve kubectl araçlarının yüklü olduğunu varsayar.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```console
$ az --version
```

Sahip değilseniz `az` yüklü, aracı yönergeler vardır [burada](https://github.com/azure/azure-cli#installation).

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```console
$ kubectl version
```

Sahip değilseniz `kubectl` yüklü çalıştırabilirsiniz:

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a>Sysdig
Bir dış Azure'da çalışan Kubernetes kümenizi kapsayıcılarında izleyebilirsiniz bir hizmet şirket olarak izleme Sysdig olur. Sysdig kullanarak etkin bir Sysdig hesabı gerektirir.
İçin bir hesap üzerinde kaydolabilirsiniz kendi [site](https://app.sysdigcloud.com).

Sysdig bulut web sitesinde oturum açtıktan sonra kullanıcı adınıza tıklayın, sayfada “Erişim Anahtarınızı” göreceksiniz. 

![Sysdig API anahtarı](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Kubernetes için Sysdig aracılarını yükleme
Sysdig kapsayıcılarınızı izlemek için bir Kubernetes kullanarak her makine üzerinde bir işlemi çalıştırılan `DaemonSet`.
DaemonSets bir kapsayıcı makine başına tek bir örneğini çalıştıran Kubernetes API nesneleridir.
Bunlar Sysdig'ın İzleme Aracısı gibi araçları yüklemek için mükemmel.

Sysdig daemonset yüklemek için önce yüklemelisiniz [şablon](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) sysdig gelen. Bu dosya olarak kaydetmek `sysdig-daemonset.yaml`.

Linux ve OS X çalıştırabilirsiniz:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

PowerShell içinde:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Sonraki erişim Sysdig hesabınızdan aldığınız anahtarınız, eklemek için bu dosyayı düzenleyin.

Son olarak, DaemonSet oluşturun:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>İzlemenizi görüntüleyin
Yüklü ve çalışan sonra aracılar verileri Sysdig pompa.  Geri dönerek [sysdig Pano](https://app.sysdigcloud.com) ve kapsayıcıları hakkında bilgiler görmeniz gerekir.

Kubernetes özel panolar aracılığıyla da yükleyebilirsiniz [yeni pano Sihirbazı'nı](https://app.sysdigcloud.com/#/dashboards/new).
