---
title: "Bir Azure Ağ İzleyicisi örneği oluşturma | Microsoft Docs"
description: "Bu sayfa portal ve Azure REST API'sini kullanarak Ağ İzleyicisi örneği oluşturmak için aşağıdaki adımları sağlar."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 687e5b65e89ae2a79d8e9aa5c4345c91b4943d3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-network-watcher-instance"></a>Bir Azure Ağ İzleyicisi örneği oluşturma

Ağ İzleyicisi İzleme ve koşullar bir ağ düzeyinde senaryo içinde gelen ve giden Azure tanılama sağlayan bölgesel bir hizmettir. Senaryo düzeyi izleme, bir uçtan uca ağ düzey görünümü adresindeki sorunlara tanı koymak sağlar. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

> [!NOTE]
> Ağ İzleyicisi'ni şu anda yalnızca CLI 1.0 destekler gibi CLI 1.0 için yeni bir Ağ İzleyicisi örneği oluşturmak için yönergeler sağlanır.

## <a name="create-a-network-watcher-in-the-portal"></a>Portalda bir Ağ İzleyicisi oluşturma

Gidin **daha fazla hizmet** > **ağ** > **Ağ İzleyicisi**. Ağ İzleyicisi için etkinleştirmek istediğiniz tüm abonelikleri seçebilirsiniz. Bu eylem, kullanılabilir olan her bölgede bir Ağ İzleyicisi oluşturur.

![Ağ İzleyicisi oluşturma][1]

Ağ İzleyicisi'ni Portalı'nı kullanarak etkinleştirdiğinizde, Ağ İzleyicisi örneğinin adını burada region_name örneği etkinleştirdiğiniz Azure bölgesine karşılık gelen NetworkWatcher_region_name otomatik olarak ayarlanır.  Örneğin, Batı Orta ABD bölgesinde etkin bir Ağ İzleyicisi NetworkWatcher_westcentralus adlandırılır

Ayrıca, Ağ İzleyicisi örneği NetworkWatcherRG adlı bir kaynak grubuna otomatik olarak eklenir.  Bu kaynak grubu, zaten yoksa, oluşturulur.

Bir Ağ İzleyicisi örneği ve kaynak grubu adını özelleştirmek istiyorsanız içine yerleştirildiğinde, aşağıda açıklanan Powershell, REST API veya ARMClient yöntemlerini kullanabilirsiniz.  Ağ İzleyicisi'ni içine yerleştirmeden önce her seçenekte, kaynak grubu mevcut olmalıdır.  

## <a name="create-a-network-watcher-with-powershell"></a>PowerShell ile bir Ağ İzleyicisi oluşturma

Ağ İzleyicisi örneği oluşturmak için aşağıdaki örnekte çalıştırın:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Ağ İzleyicisi ile REST API'si oluşturma

ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulundu üzerinde adresindeki chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Oturum ARMClient oturum

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Ağ İzleyicisi oluşturma

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ İzleyicisi örneği sahip olduğunuza göre kullanılabilir özellikleri hakkında bilgi edinin:

* [Topoloji](network-watcher-topology-overview.md)
* [Paket yakalama](network-watcher-packet-capture-overview.md)
* [IP akışı doğrulama](network-watcher-ip-flow-verify-overview.md)
* [Sonraki atlama](network-watcher-next-hop-overview.md)
* [Güvenlik grubu görünümü](network-watcher-security-group-view-overview.md)
* [NSG akış günlüğe kaydetme](network-watcher-nsg-flow-logging-overview.md)
* [Sanal ağ geçidi sorunlarını giderme](network-watcher-troubleshoot-overview.md)

Ağ İzleyicisi örneği oluşturulduktan sonra paket yakalama makaleyi izleyerek yapılandırılabilir: [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png











