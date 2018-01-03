---
title: "Sanal ağ geçidi ve Azure Ağ İzleyicisi - REST kullanarak bağlantı sorunlarını giderme | Microsoft Docs"
description: "Bu sayfayı sanal ağ geçitleri ve Azure Ağ İzleyicisi'ni kullanarak REST bağlantılarla nasıl giderileceği açıklanmaktadır"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 086a853d0849ee22f992c9d3265f6988bcc7bd83
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi'ni kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ kaynaklarınızı Azure anlamak için ilgili olarak Ağ İzleyicisi çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorun giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulduklarını döndürür.

Bu makalede kaynak sorun giderme için şu anda kullanılabilir farklı yönetim görevleri yoluyla alır.

- [**Bir sanal ağ geçidi sorun giderme**](#troubleshoot-a-virtual-network-gateway)
- [**Bir bağlantı sorunlarını giderme**](#troubleshoot-connections)

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulundu üzerinde adresindeki chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesi [desteklenen ağ geçidi türleri](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Ağ İzleyicisi sorunlarını giderme özelliği sağlar sanal ağ geçitlerini ve bağlantılarla ortaya çıkan sorunları giderme. Sorun giderme kaynak için bir istek yapıldığında, günlükleri sorguladığınızda ve sahip denetlenir. İnceleme tamamlandığında, sonuçları döndürülür. Sorun giderme API istekleri uzun çalışan bir sonuca dönmek için birden çok dakika sürebilir isteklerin. Günlükler, depolama hesabındaki bir kapsayıcıda depolanır.

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a>Bir sanal ağ geçidi sorun giderme


### <a name="post-the-troubleshoot-request"></a>Sorun giderme isteği gönderme

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


armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30" -verbose
```

Bu işlem uzun olduğundan çalıştıran, sonuç yanıt üst bilgi aşağıdaki yanıtta gösterildiği gibi döndürülür için işlemi ve URI sorgulamak için URI:

**Önemli değerleri**

* **Azure AsyncOperation** -URI sorgu zaman uyumsuz işlemi gidermek için bu özelliği içeriyor
* **Konum** -bu özellik, işlem tamamlandığında, sonuçları nerede URI içerir.

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

### <a name="query-the-async-operation-for-completion"></a>Sorgu tamamlanması için zaman uyumsuz işlemi

Aşağıdaki örnekte görüldüğü gibi işlemlerini URI sorgu işlemi ilerleme durumu için kullanın:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

İşlemi sürerken yanıtı gösterir **devam ediyor** aşağıdaki örnekte görüldüğü gibi:

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

### <a name="retrieve-the-results"></a>Sonuçları Al

Durumu döndürülür sonra **başarılı**, sonuçları almak için URI operationResult üzerinde bir GET yöntemini çağırın.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

Aşağıdaki yanıtlar bir ağ geçidi sorun giderme sonuçlarını sorgulanırken döndürülen tipik bir düşürülmüş yanıtında bir örnektir. Bkz: [sonuçlarını anlama](#understanding-the-results) yanıt özelliklerinde anlamı hakkında açıklama almak için.

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
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by the expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
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
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
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
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

> [!NOTE]
> Sorun giderme işlemi, bağlantı ve karşılık gelen alt ağ geçitleri üzerinde paralel olarak çalıştırılamaz. İşlemi, önceki kaynağı çalıştırılmadan önce tamamlamanız gerekir.

Bu yanıt üstbilgisinde uzun süre çalışan bir işlem olduğundan işlem ve sonuç URI'sini sorgulamak için URI aşağıdaki yanıtta gösterildiği gibi verilir:

**Önemli değerleri**

* **Azure AsyncOperation** -URI sorgu zaman uyumsuz işlemi gidermek için bu özelliği içeriyor
* **Konum** -bu özellik, işlem tamamlandığında, sonuçları nerede URI içerir.

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

### <a name="query-the-async-operation-for-completion"></a>Sorgu tamamlanması için zaman uyumsuz işlemi

Aşağıdaki örnekte görüldüğü gibi işlemlerini URI sorgu işlemi ilerleme durumu için kullanın:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

İşlemi sürerken yanıtı gösterir **devam ediyor** aşağıdaki örnekte görüldüğü gibi:

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

Aşağıdaki yanıtlar bağlantı sorunlarını giderme sonuçlarını sorgulanırken döndürülen tipik bir yanıt gösterilebilir.

### <a name="retrieve-the-results"></a>Sonuçları Al

Durumu döndürülür sonra **başarılı**, sonuçları almak için URI operationResult üzerinde bir GET yöntemini çağırın.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

Aşağıdaki yanıtlar bağlantı sorunlarını giderme sonuçlarını sorgulanırken döndürülen tipik bir yanıt gösterilebilir.

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
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by the expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
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
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-the-results"></a>Sonuçları anlama

Eylem metin sorunun nasıl çözümleneceği hakkında genel kılavuzluk sağlar. Bir eylem için sorunu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Durumda hiçbir ek yönergeler bulunduğu bir destek servis talebi açmaya url yanıt sağlar.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için bkz [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilir başka bir Depolama Gezgini aracıdır. Aşağıdaki bağlantıda Depolama Gezgini hakkında daha fazla bilgi şurada bulunabilir: [Depolama Gezgini](http://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

Bu Dur VPN bağlantısı ayarları değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.
