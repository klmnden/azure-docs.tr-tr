---
title: (KULLANIM DIŞI) Azure Kubernetes kümesi - Sysdig ile izleme
description: Sysdig kullanarak Azure Container Service Kubernetes kümesini izleme
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 4aef241e2c86e4016c3c468fcdcfdfc620fc7aa9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309269"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>(KULLANIM DIŞI) Sysdig kullanarak bir Azure Container Service Kubernetes kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu izlenecek yol, sahibi olduğunuzu varsayar [Azure Container Service kullanan bir Kubernetes kümesi oluşturuldu](container-service-kubernetes-walkthrough.md).

Ayrıca, azure CLI ve kubectl araçlarının yüklü olduğunu varsayar.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```console
$ az --version
```

Öğeniz yoksa `az` yüklü aracı yönergeler sunulmaktadır [burada](https://github.com/azure/azure-cli#installation).

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```console
$ kubectl version
```

Öğeniz yoksa `kubectl` yüklü çalıştırabilirsiniz:

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a>Sysdig
Sysdig, bir dış hizmet şirket, bir Kubernetes kümenize Azure'da çalışan kapsayıcılar izleyebilirsiniz İzleme ' dir. Sysdig kullanarak etkin bir Sysdig hesabınızın gerektirir.
İçin bir hesap üzerinde kaydolabilirsiniz kendi [site](https://app.sysdigcloud.com).

Sysdig bulut web sitesinde oturum açtıktan sonra kullanıcı adınıza tıklayın, sayfada “Erişim Anahtarınızı” göreceksiniz. 

![Sysdig API anahtarı](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Kubernetes için Sysdig aracıları yükleme
Sysdig kapsayıcılarınızı izleyecek şekilde bir Kubernetes kullanarak her bir makinede bir işlem çalıştırır `DaemonSet`.
DaemonSets kapsayıcı makine başına tek bir örneğini çalıştıran Kubernetes API nesneleridir.
Bunlar Sysdig'ın İzleme Aracısı gibi araçları yüklemek için mükemmeldir.

Sysdig daemonset yüklemek için indirmeniz [şablon](https://github.com/draios/sysdig-cloud-scripts/tree/master/agent_deploy/kubernetes) sysdig öğesinden. Bu dosyayı farklı Kaydet `sysdig-daemonset.yaml`.

Linux ve OS X üzerinde çalıştırabilirsiniz:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

PowerShell'de:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Sonra Sysdig hesabınızdan aldığınız erişim anahtarına, eklemek için bu dosyayı düzenleyin.

Son olarak, DaemonSet oluşturun:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>İzleme işleminiz görüntüleyin
Yüklü ve çalışır sonra aracılar verileri Sysdig pompa.  Geri Git [sysdig Pano](https://app.sysdigcloud.com) ve kapsayıcılarınızı hakkında bilgiler görmeniz gerekir.

Ayrıca Kubernetes özel panoları da yükleyebilirsiniz [yeni pano sihirbazıyla](https://app.sysdigcloud.com/#/dashboards/new).
