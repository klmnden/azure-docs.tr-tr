---
title: "Ağ güvenlik grubunu yönetme akış günlükleri Azure Ağ İzleyicisi - REST API ile | Microsoft Docs"
description: "Bu sayfa, Azure Ağ İzleyicisi REST API ile ağ güvenlik grubu akış günlüklerine yönetmek açıklanmaktadır"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: c790fa5348ab8f945c40e2a941a31ba6064f1627
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a>REST API kullanarak ağ güvenlik grubu yapılandırma akış günlükleri

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ güvenlik grubu akış günlükleri, giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek izin veren bir Ağ İzleyicisi özelliğidir. Bu akış günlükleri json biçiminde yazılır ve Kural başına temelinde, akış uygulanır, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini NIC giden ve gelen akışları gösterir ve trafiğe izin verilen veya reddedilen.

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulundu üzerinde adresindeki chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

> [!Important]
> Ağ İzleyicisi'ni içeren kaynak grubunu istek URI'SİNDEKİ kaynak grubu adı olan Ağ İzleyicisi REST API çağrıları için kaynaklar üzerinde tanılama işlemleri yapıyorsunuz.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo etkinleştir, devre dışı bırakın ve REST API kullanarak akış günlükleri sorgu gösterilmektedir. Ağ güvenlik grubu akışı loggings hakkında daha fazla bilgi için [ağ güvenlik grubu akışı günlüğü - genel bakış](network-watcher-nsg-flow-logging-overview.md).

Bu senaryoda, şunları yapacaksınız:

* Akış günlükleri etkinleştir
* Akış günlükleri devre dışı bırak
* Sorgu akış günlükleri durumu

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum

Armclient Azure kimlik bilgilerinizle oturum açın.

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a>Öngörüler sağlayıcısını Kaydet

Başarılı bir şekilde çalışması için günlük akışı sırayla **Microsoft.ınsights** sağlayıcı kaydedilmelidir. Emin değilseniz, **Microsoft.ınsights** sağlayıcısı kayıtlı, aşağıdaki komut dosyasını çalıştırın.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a>Ağ güvenlik grubu etkinleştirmek akış günlükleri

Akış günlükleri etkinleştirmek için komutu aşağıdaki örnekte gösterilmiştir:

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
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Önceki örnekten döndürülen yanıt aşağıdaki gibidir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a>Ağ güvenlik grubu devre dışı akış günlükleri

Akış günlükleri devre dışı bırakmak için aşağıdaki örneği kullanın. Çağrı dışında akış günlükleri, etkinleştirme aynıdır **false** etkin özelliğini ayarlayın.

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
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Önceki örnekten döndürülen yanıt aşağıdaki gibidir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="query-flow-logs"></a>Sorgu akış günlükleri

Aşağıdaki REST çağrısı sorguları bir ağ güvenlik grubu akışı durumunu kaydeder.

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
    }
  }
}
```

## <a name="download-a-flow-log"></a>Akış günlüğü indirin

Akış günlüğü depolama konumunu, oluşturma sırasında tanımlanır. Bir depolama hesabına kaydedilen bu akış günlüklerine erişmek için kullanışlı bir burada indirilebilir Microsoft Azure Storage Gezgini araçtır: http://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [NSG akış günlüklerinizi Powerbı ile Görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Bilgi edinmek için nasıl [NSG akış günlüklerinizi açık kaynaklı araçları ile Görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
