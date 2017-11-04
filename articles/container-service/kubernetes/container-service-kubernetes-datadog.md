---
title: "İzleyici Azure Kubernetes Datadog ile küme | Microsoft Docs"
description: "Kubernetes Datadog kullanarak Azure kapsayıcı hizmeti kümesinde izleme"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: b2f88f663745f1caef856e9b2b5803000c284cb9
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Azure kapsayıcı hizmeti kümesi DataDog ile izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Ön koşullar
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
