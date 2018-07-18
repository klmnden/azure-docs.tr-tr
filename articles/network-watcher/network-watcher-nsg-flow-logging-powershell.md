---
title: Ağ güvenlik grubu akış günlüklerini Azure Ağ İzleyicisi - PowerShell ile yönetme | Microsoft Docs
description: Bu sayfa, PowerShell ile Azure Ağ İzleyicisi'nde ağ güvenlik grubu akış günlüklerini yönetmek açıklanmaktadır
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
ms.openlocfilehash: 0b0516eb079dc45c46ce56bd9d2522fc66d45548
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089868"
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>PowerShell ile ağ güvenlik grubu akış günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ güvenlik grubu akış günlüklerini bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntülemek izin veren bir Ağ İzleyicisi'nin bir özelliğidir. Bu akış günlüklerini json biçiminde yazılır ve Kural başına temelinde, akışı uygular, 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) akışla ilgili NIC giden ve gelen akış Göster ve trafiğin izin verilen veya reddedilen.

## <a name="register-insights-provider"></a>Insights sağlayıcısını kaydetme

Oturum başarıyla çalışması için flow için sırada **Microsoft.Insights** sağlayıcısı kayıtlı olmalıdır. Emin değilseniz, **Microsoft.Insights** sağlayıcısı kaydedildiğinde, aşağıdaki betiği çalıştırın.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Ağ güvenlik grubu akış günlüklerini etkinleştirme

Akış günlüklerini etkinleştirmek için komutu aşağıdaki örnekte gösterilmiştir:

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

Depolama hesabı yalnızca Microsoft hizmetlerine veya belirli sanal ağları için ağ erişimini kısıtlama için yapılandırılmış ağ kuralları olamaz belirtin. Depolama hesabı, aynı veya farklı Azure aboneliği için akış günlüğü etkinleştirme NSG daha olabilir. Farklı Aboneliklerde kullanırsanız, bunların her ikisi de aynı Azure Active Directory kiracısı ile ilişkilendirilmesi olması gerekir. Her abonelik için kullandığınız hesap olmalıdır [gerekli izinleri](required-rbac-permissions.md).

## <a name="disable-network-security-group-flow-logs"></a>Ağ güvenlik grubu akış günlüklerini devre dışı bırak

Akış günlüklerini devre dışı bırakmak için aşağıdaki örneği kullanın:

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Akış günlüğü indir

Akış günlüğü depolama konumunu oluşturma sırasında tanımlanır. Burada indirilebilir Microsoft Azure Depolama Gezgini, bir depolama hesabına kaydedilir. Bu akış günlüklerine erişmek için kullanışlı bir araçtır:  http://storageexplorer.com/

Bir depolama hesabı belirttiyseniz, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Günlük yapısı hakkında bilgi için ziyaret [ağ güvenlik grubu akış günlüğü genel bakış](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Sonraki Adımlar

Bilgi edinmek için nasıl [, Power BI ile NSG akış günlüklerini Görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Bilgi edinmek için nasıl [açık kaynak araçlarla, NSG akış günlüklerini Görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
