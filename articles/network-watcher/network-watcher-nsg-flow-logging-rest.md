---
title: Azure Ağ İzleyicisi - REST API'si ile yönetme ağ güvenlik grubu akış günlüklerini | Microsoft Docs
description: Bu sayfa, REST API'si ile Azure Ağ İzleyicisi'nde ağ güvenlik grubu akış günlüklerini yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: ab4b283449ec6c0174f380b0231dd2e78dea419d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64688037"
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a>REST API'sini kullanarak ağ güvenlik Grubu'nun yapılandırılmasına akış günlükleri

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ güvenlik grubu akış günlüklerini bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntülemek izin veren bir Ağ İzleyicisi'nin bir özelliğidir. Bu akış günlüklerini json biçiminde yazılır ve Kural başına temelinde, akışı uygular, 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) akışla ilgili NIC giden ve gelen akış Göster ve trafiğin izin verilen veya reddedilen.

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient, PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulunur, chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için.

> [!Important]
> Ağ İzleyicisini içeren kaynak grubunu istek URI'SİNDEKİ kaynak grubu adı olan Ağ İzleyicisi REST API çağrıları için kaynaklar üzerinde tanılama işlemleri yapıyorsunuz.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryoyu etkinleştirebilir, devre dışı bırakın ve REST API kullanarak akış günlüklerini sorgu gösterilmektedir. Ağ güvenlik grubu akış loggings hakkında daha fazla bilgi edinmek için [ağ güvenlik grubu akış günlüğe kaydetme - genel bakış](network-watcher-nsg-flow-logging-overview.md).

Bu senaryoda, şunları yapacaksınız:

* Akış günlüklerini (sürüm 2) etkinleştir
* Akış günlüklerini devre dışı bırak
* Sorgu akış günlükleri durumu

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum açın

Armclient Azure kimlik bilgilerinizle oturum açın.

```powershell
armclient login
```

## <a name="register-insights-provider"></a>Insights sağlayıcısını kaydetme

Oturum başarıyla çalışması için flow için sırada **Microsoft.Insights** sağlayıcısı kayıtlı olmalıdır. Emin değilseniz, **Microsoft.Insights** sağlayıcısı kaydedildiğinde, aşağıdaki betiği çalıştırın.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a>Etkinleştirme ağ güvenlik grubu akış günlüklerini

Akış günlükleri sürüm 2 etkinleştirmek için komutu aşağıdaki örnekte gösterilmiştir. Sürüm 1 'Sürüm' alanı '1' ile değiştirin:

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Önceki örnekten döndürülen yanıtı aşağıdaki gibidir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a>Devre dışı ağ güvenlik grubu akış günlüklerini

Akış günlüklerini devre dışı bırakmak için aşağıdaki örneği kullanın. Dışında akış günlüklerini etkinleştirme aynı çağrıdır **false** için enabled özelliğini ayarlayın.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Önceki örnekten döndürülen yanıtı aşağıdaki gibidir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

## <a name="query-flow-logs"></a>Sorgu akış günlükleri

Aşağıdaki REST çağrısı sorguları bir ağ güvenlik grubu akış durumunu kaydeder.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

Döndürülen yanıt örneği verilmiştir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
   "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

## <a name="download-a-flow-log"></a>Akış günlüğü indir

Akış günlüğü depolama konumunu oluşturma sırasında tanımlanır. Burada indirilebilir Microsoft Azure Depolama Gezgini, bir depolama hesabına kaydedilir. Bu akış günlüklerine erişmek için kullanışlı bir araçtır:  https://storageexplorer.com/

Bir depolama hesabı belirttiyseniz, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [, Power BI ile NSG akış günlüklerini Görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Bilgi edinmek için nasıl [açık kaynak araçlarla, NSG akış günlüklerini Görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
