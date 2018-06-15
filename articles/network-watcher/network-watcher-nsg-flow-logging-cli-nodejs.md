---
title: Ağ güvenlik grubu akış günlükleri Azure Ağ İzleyicisi - Azure CLI 1.0 ile yönetme | Microsoft Docs
description: Bu sayfa, Azure Ağ İzleyicisi Azure CLI 1.0 ile ağ güvenlik grubu akış günlüklerine yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 1cdef3454a326fbdbdef5ab51ac93cc8a558f34d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
ms.locfileid: "26774497"
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli-10"></a>Azure CLI 1.0 ile ağ güvenlik grubu akış günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure portalı](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ güvenlik grubu akış günlükleri, giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek izin veren bir Ağ İzleyicisi özelliğidir. Bu akış günlükleri json biçiminde yazılır ve Kural başına temelinde, akış uygulanır, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini NIC giden ve gelen akışları gösterir ve trafiğe izin verilen veya reddedilen.

Bu makalede, platformlar arası Azure CLI 1.0, Windows, Mac ve Linux için kullanılabilir olduğu kullanır. Ağ İzleyicisi, CLI desteği şu anda Azure CLI 1.0 kullanır.

## <a name="register-insights-provider"></a>Öngörüler sağlayıcısını Kaydet

Başarılı bir şekilde çalışması için günlük akışı sırayla **Microsoft.ınsights** sağlayıcı kaydedilmelidir. Emin değilseniz, **Microsoft.ınsights** sağlayıcısı kayıtlı, aşağıdaki komut dosyasını çalıştırın.

```azurecli
azure provider register --namespace Microsoft.Insights --subscription <subscriptionid>
```

## <a name="enable-network-security-group-flow-logs"></a>Ağ güvenlik grubu etkinleştirmek akış günlükleri

Akış günlükleri etkinleştirmek için komutu aşağıdaki örnekte gösterilmiştir:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a>Ağ güvenlik grubu devre dışı akış günlükleri

Akış günlükleri devre dışı bırakmak için aşağıdaki örneği kullanın:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
```

## <a name="download-a-flow-log"></a>Akış günlüğü indirin

Akış günlüğü depolama konumunu, oluşturma sırasında tanımlanır. Bir depolama hesabına kaydedilen bu akış günlüklerine erişmek için kullanışlı bir burada indirilebilir Microsoft Azure Storage Gezgini araçtır: http://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Günlük yapısı hakkında bilgi için ziyaret [ağ güvenlik grubu akış günlüğüne genel bakış](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Sonraki Adımlar

Bilgi edinmek için nasıl [NSG akış günlüklerinizi Powerbı ile Görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Bilgi edinmek için nasıl [NSG akış günlüklerinizi açık kaynaklı araçları ile Görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
