---
title: (KULLANIM DIŞI) Azure Kubernetes kümesi Datadog ile izleme
description: Datadog kullanarak Azure Container Service Kubernetes kümesini izleme
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 6a682c199b40035bfd44fc5611a7d44b49f7b3ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712347"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-datadog"></a>(KULLANIM DIŞI) DataDog ile bir Azure Container Service kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu izlenecek yol, sahibi olduğunuzu varsayar [Azure Container Service kullanan bir Kubernetes kümesi oluşturuldu](container-service-kubernetes-walkthrough.md).

Ayrıca, sahibi olduğunuzu varsayar `az` Azure CLI ve `kubectl` araçlarının yüklü.

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

## <a name="datadog"></a>DataDog
Datadog gelen Azure Container Service kümenizdeki kapsayıcıları izleme verilerini toplayan bir izleme hizmetidir. Datadog Docker tümleştirmesi, kapsayıcılarınızın belirli ölçümleri görebileceğiniz bir Pano vardır. Kapsayıcılardan toplanan ölçümler, CPU, bellek, ağ ve g/ç tarafından düzenlenir. Datadog ölçümleri kapsayıcılar ve görüntüleri halinde böler.

Öncelikle [bir hesap oluşturun](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>İle bir DaemonSet Datadog Aracısı'nı yükleme
DaemonSets tarafından Kubernetes kümesindeki her bir konakta bir kapsayıcıyı tek bir örneğini çalıştırmak için kullanılır.
Bunlar, izleme aracılarını çalıştırmak için ideal.

Datadog açtıktan sonra izleyebilirsiniz [Datadog yönergeleri](https://app.datadoghq.com/account/settings#agent/kubernetes) bir DaemonSet kullanarak kümenizde Datadog aracıları yüklemek için.

## <a name="conclusion"></a>Sonuç
İşte bu kadar! Aracılar hazır ve çalışır olduğunda birkaç dakika içinde veri konsolunda görmeniz gerekir. Tümleşik ziyaret edebilirsiniz [kubernetes panosunu](https://app.datadoghq.com/screen/integration/kubernetes) kümenizi özetini görmek için.
