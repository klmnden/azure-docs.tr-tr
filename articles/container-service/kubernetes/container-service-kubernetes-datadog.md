---
title: Azure Kubernetes küme Datadog ile izleme
description: Kubernetes Datadog kullanarak Azure kapsayıcı hizmeti kümesinde izleme
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 0a3f0baa4998dbc594023935575d659f7d45bbb9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Azure kapsayıcı hizmeti kümesi DataDog ile izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, sahibi olduğunuzu varsayar [Azure kapsayıcı hizmeti kullanarak Kubernetes küme oluşturulan](container-service-kubernetes-walkthrough.md).

Ayrıca, sahibi olduğunuzu varsayar `az` Azure CLI ve `kubectl` araçları yüklü.

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

## <a name="datadog"></a>DataDog
Datadog, Azure kapsayıcı hizmeti kümesi kapsayıcılara gelen izleme verilerini toplayan izleme bir hizmettir. Datadog Docker tümleştirmesi, kapsayıcılara belirli ölçümleri görebileceğiniz bir Pano vardır. Kapsayıcılardan toplanan ölçümleri CPU, bellek, ağ ve g/ç tarafından düzenlenir. Datadog ölçümleri kapsayıcıları ve görüntüleri halinde ayırır.

Öncelikle [bir hesap oluşturun](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>DaemonSet ile Datadog Aracısı'nı yükleme
DaemonSets Kubernetes tarafından bir kapsayıcı tek bir örneği kümedeki her ana bilgisayarda çalıştırmak için kullanılır.
Bunlar izleme aracıları çalıştırmak için mükemmel.

Datadog oturum açtıktan sonra izleyebilirsiniz [Datadog yönergeleri](https://app.datadoghq.com/account/settings#agent/kubernetes) bir DaemonSet kullanarak kümenizde Datadog aracıları yüklemek için.

## <a name="conclusion"></a>Sonuç
İşte bu kadar! Aracılar ve çalışıyor olduktan sonra birkaç dakika içinde veri konsolunda görmeniz gerekir. Tümleşik ziyaret edebilirsiniz [kubernetes Pano](https://app.datadoghq.com/screen/integration/kubernetes) kümenizi özetini görmek için.
