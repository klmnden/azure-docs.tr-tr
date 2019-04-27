---
title: Ağ güvenlik grubu akış günlüklerini Azure Ağ İzleyicisi - PowerShell ile yönetme | Microsoft Docs
description: Bu sayfa, PowerShell ile Azure Ağ İzleyicisi'nde ağ güvenlik grubu akış günlüklerini yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: ebeebfa4490797493a781bf462d363d1cbcf2d55
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60680378"
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
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs-and-traffic-analytics"></a>Etkinleştirme ağ güvenlik grubu akış günlüklerini ve trafik analizi

Akış günlüklerini etkinleştirmek için komutu aşağıdaki örnekte gösterilmiştir:

```powershell
$NW = Get-AzNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id

#Traffic Analytics Parameters
$workspaceResourceId = "/subscriptions/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb/resourcegroups/trafficanalyticsrg/providers/microsoft.operationalinsights/workspaces/taworkspace"
$workspaceGUID = "cccccccc-cccc-cccc-cccc-cccccccccccc"
$workspaceLocation = "westeurope"

#Configure Version 1 Flow Logs
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 1

#Configure Version 2 Flow Logs, and configure Traffic Analytics
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2

#Configure Version 2 FLow Logs with Traffic Analytics Configured
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -EnableTrafficAnalytics -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceid -WorkspaceLocation $workspaceRegion

#Query Flow Log Status
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
```

Depolama hesabı yalnızca Microsoft hizmetlerine veya belirli sanal ağları için ağ erişimini kısıtlama için yapılandırılmış ağ kuralları olamaz belirtin. Depolama hesabı, aynı veya farklı Azure aboneliği için akış günlüğü etkinleştirme NSG daha olabilir. Farklı Aboneliklerde kullanırsanız, bunların her ikisi de aynı Azure Active Directory kiracısı ile ilişkilendirilmesi olması gerekir. Her abonelik için kullandığınız hesap olmalıdır [gerekli izinleri](required-rbac-permissions.md).

## <a name="disable-traffic-analytics-and-network-security-group-flow-logs"></a>Trafik analizi ve ağ güvenlik grubu akış günlüklerini devre dışı bırak

Trafik analizi devre dışı bırakın ve akış günlükleri için aşağıdaki örneği kullanın:

```powershell
#Disable Traffic Analaytics by removing -EnableTrafficAnalytics property
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Disable Flow Logging
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Akış günlüğü indir

Akış günlüğü depolama konumunu oluşturma sırasında tanımlanır. Burada indirilebilir Microsoft Azure Depolama Gezgini, bir depolama hesabına kaydedilir. Bu akış günlüklerine erişmek için kullanışlı bir araçtır:  https://storageexplorer.com/

Bir depolama hesabı belirttiyseniz, akışın günlük dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Günlük yapısı hakkında bilgi için ziyaret [ağ güvenlik grubu akış günlüğü genel bakış](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Sonraki Adımlar

Bilgi edinmek için nasıl [, Power BI ile NSG akış günlüklerini Görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Bilgi edinmek için nasıl [açık kaynak araçlarla, NSG akış günlüklerini Görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
