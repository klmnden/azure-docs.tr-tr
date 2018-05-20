---
title: Ağ güvenlik grubu akış günlükleri Azure Ağ İzleyicisi - Azure CLI ile yönetme | Microsoft Docs
description: Bu sayfa, Azure CLI ile Azure Ağ İzleyicisi ağ güvenlik grubu akış günlüklerine yönetmek açıklanmaktadır
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
ms.openlocfilehash: fa90d406313d756f3dd852305a6d61db29815192
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a>Azure CLI ile ağ güvenlik grubu akış günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ güvenlik grubu akış günlükleri, giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek izin veren bir Ağ İzleyicisi özelliğidir. Bu akış günlükleri json biçiminde yazılır ve Kural başına temelinde, akış uygulanır, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini NIC giden ve gelen akışları gösterir ve trafiğe izin verilen veya reddedilen.

Bu makalede, Windows, Mac ve Linux için kullanılabilir olduğu, kaynak yönetimi için dağıtım modelini, Azure CLI 2.0 bizim nesil CLI kullanılmaktadır.

Bu makaledeki adımları gerçekleştirmek için gerek [Mac, Linux ve Windows (Azure CLI) için Azure komut satırı arabirimini yükleyin](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="register-insights-provider"></a>Öngörüler sağlayıcısını Kaydet

Başarılı bir şekilde çalışması için günlük akışı sırayla **Microsoft.ınsights** sağlayıcı kaydedilmelidir. Emin değilseniz, **Microsoft.ınsights** sağlayıcısı kayıtlı, aşağıdaki komut dosyasını çalıştırın.

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Ağ güvenlik grubu etkinleştirmek akış günlükleri

Akış günlükleri etkinleştirmek için komutu aşağıdaki örnekte gösterilmiştir:

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
```

Yalnızca Microsoft Hizmetleri veya belirli sanal ağlar için ağ erişimi kısıtlamak için yapılandırılmış ağ kuralları olamaz belirttiğiniz depolama hesabı. Depolama hesabı, aynı veya akış günlüğü etkinleştirmek NSG daha farklı bir Azure aboneliği olabilir. Farklı Aboneliklerde kullanırsanız, bunların her ikisi de aynı Azure Active Directory Kiracı ilişkili olması gerekir. Her abonelik için kullandığınız hesap olmalıdır [gerekli izinleri](required-rbac-permissions.md).

## <a name="disable-network-security-group-flow-logs"></a>Ağ güvenlik grubu devre dışı akış günlükleri

Akış günlükleri devre dışı bırakmak için aşağıdaki örneği kullanın:

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a>Akış günlüğü indirin

Akış günlüğü depolama konumunu, oluşturma sırasında tanımlanır. Burada indirilebilir Microsoft Azure Storage Gezgini, bir depolama hesabına kaydedilen bu akış günlüklerine erişmek için uygun bir araçtır:  http://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Günlük yapısı hakkında bilgi için ziyaret [ağ güvenlik grubu akış günlüğüne genel bakış](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Sonraki Adımlar

Bilgi edinmek için nasıl [NSG akış günlüklerinizi Powerbı ile Görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Bilgi edinmek için nasıl [NSG akış günlüklerinizi açık kaynaklı araçları ile Görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
