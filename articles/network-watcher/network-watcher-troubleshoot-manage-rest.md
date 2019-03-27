---
title: Sanal ağ geçidi ve Azure Ağ İzleyicisi - REST kullanarak bağlantı sorunlarını giderme | Microsoft Docs
description: Bu sayfa, sanal ağ geçitleri ve REST kullanarak Azure Ağ İzleyicisi bağlantı sorunlarını gidermek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 1276d1e581caf477449ce9a4c928d4493a6354d3
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487609"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi'ni kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ İzleyicisi, ağ kaynaklarınıza azure'da anlamak için bağlantılı olarak çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorunlarını giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi, bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulguları döndürür.

Bu makalede kaynak sorun giderme için şu anda kullanılabilir olan farklı yönetim görevleri alır.

- [**Bir sanal ağ geçidi sorunlarını giderme**](#troubleshoot-a-virtual-network-gateway)
- [**Bir bağlantı sorunlarını giderme**](#troubleshoot-connections)

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient, PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulunur, chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesini [desteklenen ağ geçidi türleri](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Ağ İzleyicisi sorun giderme olanağı sağlayan bağlantılar ve sanal ağ geçitleri ile ortaya çıkan sorunları giderme. Sorun giderme kaynak için bir istek yapıldığında, günlükleri sorgulamak ve inceledi. İnceleme işlemi tamamlandıktan sonra sonuçlar döndürülür. Sorun giderme API istek uzun süren istekleri, bir sonuç döndürmek için birden çok dakika sürebilir. Günlükler, depolama hesabında bir kapsayıcıda depolanır.

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum açın

```powershell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a>Bir sanal ağ geçidi sorunlarını giderme


### <a name="post-the-troubleshoot-request"></a>Sorun giderme isteği gönderin

Aşağıdaki örnek, bir sanal ağ geçidi durumunu sorgular.

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$vnetGatewayName = "ContosoVNETGateway"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @"
{
'TargetResourceId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/virtualNetworkGateways/${vnetGatewayName}',
'Properties': {
'StorageId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}',
'StoragePath': 'https://${storageAccountName}.blob.core.windows.net/${containerName}'
}
}
"@


armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30" $requestBody -verbose
```

Bu işlem uzun olduğundan çalıştıran, URI aşağıdaki yanıtında gösterilen yanıt üst bilgisinde sonuç döndürülmeden için işlemi ve URI'nı sorgulamak için:

**Önemli değerler**

* **Azure-AsyncOperation** -URI sorgu zaman uyumsuz işlemi sorun giderme için bu özelliği içeriyor
* **Konum** -bu özellik, işlem tamamlandığında sonuçları nerede URI içerir

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a>Sorgu tamamlama için zaman uyumsuz işlem

Sorgu işlemleri URI işlemin ilerlemesi için aşağıdaki örnekte görüldüğü gibi kullanın:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

İşlem devam ederken, yanıt gösterilmiştir **Inprogress** aşağıdaki örnekte görüldüğü gibi:

```json
{
  "status": "InProgress"
}
```

İşlem tamamlandığında, durum değişikliklerini **başarılı**.

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-the-results"></a>Sonuçları alma

Durum döndürülür sonra **başarılı**, sonuçları almak için URI operationResult üzerinde bir GET yöntemi çağırın.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

Aşağıdaki yanıtlar bir ağ geçidi sorunlarını giderme sonuçlarını sorgulanırken döndürülen tipik düzeyi düşürülmüş bir yanıt bir örnektir. Bkz: [sonuçlarını anlama](#understanding-the-results) yanıt özelliklerinde anlamı hakkında açıklama almak için.

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by the expected resolution time, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN gateway is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```


## <a name="troubleshoot-connections"></a>Bağlantı sorunlarını giderme

Aşağıdaki örnek, bir bağlantının durumunu sorgular.

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$connectionName = "VNET2toVNET1Connection"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @{
"TargetResourceId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/connections/${connectionName}",
"Properties": {
"StorageId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}",
"StoragePath": "https://${storageAccountName}.blob.core.windows.net/${containerName}"
}

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 $requestBody"
```

> [!NOTE]
> Sorun giderme işlemi paralel olarak bir bağlantı ve karşılık gelen, ağ geçitleri üzerinde çalıştırılamaz. İşlem, önceki kaynak çalıştırılmadan önce tamamlamanız gerekir.

Bu yanıt üst bilgisi bir uzun süre çalışan işlemde olduğundan işlemi ve sonuç için URI'nı sorgulamak için URI şu yanıt olarak gösterildiği gibi verilir:

**Önemli değerler**

* **Azure-AsyncOperation** -URI sorgu zaman uyumsuz işlemi sorun giderme için bu özelliği içeriyor
* **Konum** -bu özellik, işlem tamamlandığında sonuçları nerede URI içerir

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a>Sorgu tamamlama için zaman uyumsuz işlem

Sorgu işlemleri URI işlemin ilerlemesi için aşağıdaki örnekte görüldüğü gibi kullanın:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

İşlem devam ederken, yanıt gösterilmiştir **Inprogress** aşağıdaki örnekte görüldüğü gibi:

```json
{
  "status": "InProgress"
}
```

İşlem tamamlandığında, durum değişikliklerini **başarılı**.

```json
{
  "status": "Succeeded"
}
```

Aşağıdaki yanıtlar bağlantı sorunlarını giderme sonuçlarını sorgulanırken döndürülen yanıt tipik örnekleridir.

### <a name="retrieve-the-results"></a>Sonuçları alma

Durum döndürülür sonra **başarılı**, sonuçları almak için URI operationResult üzerinde bir GET yöntemi çağırın.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

Aşağıdaki yanıtlar bağlantı sorunlarını giderme sonuçlarını sorgulanırken döndürülen yanıt tipik örnekleridir.

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by the expected resolution time, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN Connection is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-the-results"></a>Sonuçlarını anlama

Eylem metin sorunu hakkında genel rehberlik sağlar. Bir eylem sorununu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Bu durumda hiçbir ek yönergeler olduğunda, bir destek olayı açmak için URL'yi isteğin yanıtını verir.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için başvurmak [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilen başka bir Depolama Gezgini aracıdır. Depolama Gezgini hakkında daha fazla bilgi aşağıdaki bağlantıda burada bulunabilir: [Depolama Gezgini](https://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

Ayarları durdurma VPN bağlantısının değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/manage-network-security-group.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.
