---
title: Bir Azure Ağ İzleyicisi örneği oluşturma | Microsoft Docs
description: Ağ İzleyicisi bir Azure bölgesindeki etkinleştirmeyi öğrenin.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 9d3e579cd58bc6c7d67b29998ea5a48a65548b0a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30904009"
---
# <a name="create-an-azure-network-watcher-instance"></a>Bir Azure Ağ İzleyicisi örneği oluşturma

Ağ İzleyicisi İzleme ve koşullar bir ağ düzeyinde senaryo içinde gelen ve giden Azure tanılama sağlayan bölgesel bir hizmettir. Senaryo düzeyi izleme, bir uçtan uca ağ düzey görünümü adresindeki sorunlara tanı koymak sağlar. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

## <a name="create-a-network-watcher-in-the-portal"></a>Portalda bir Ağ İzleyicisi oluşturma

Gidin **tüm hizmetleri** > **ağ** > **Ağ İzleyicisi**. Ağ İzleyicisi için etkinleştirmek istediğiniz tüm abonelikleri seçebilirsiniz. Bu eylem, kullanılabilir olan her bölgede bir Ağ İzleyicisi oluşturur.

![Ağ İzleyicisi oluşturma](./media/network-watcher-create/figure1.png)

Ağ İzleyicisi'ni Portalı'nı kullanarak etkinleştirdiğinizde, Ağ İzleyicisi örneğinin adını otomatik olarak ayarlanır *NetworkWatcher_region_name* nerede *region_name* bir Azure bölgesine karşılık gelen örneği burada etkin. Örneğin, Batı Orta ABD bölgesinde etkin bir Ağ İzleyicisi adlı *NetworkWatcher_westcentralus*.

Ağ İzleyicisi örneği otomatik olarak adlı bir kaynak grubunda oluşturulan *NetworkWatcherRG*. Kaynak grubu, zaten yoksa, oluşturulur.

Bir Ağ İzleyicisi örneği ve kaynak grubu adını özelleştirmek istiyorsanız içine yerleştirildiğinde, Powershell, Azure CLI, REST API veya izleyen bölümlerde açıklanan ARMClient yöntemlerini kullanabilirsiniz. Ağ İzleyicisi içinde oluşturmadan önce her seçenekte, kaynak grubu mevcut olmalıdır.  

## <a name="create-a-network-watcher-with-powershell"></a>PowerShell ile bir Ağ İzleyicisi oluşturma

Ağ İzleyicisi örneği oluşturmak için aşağıdaki örnekte çalıştırın:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Azure CLI ile bir Ağ İzleyicisi oluşturma

Ağ İzleyicisi örneği oluşturmak için aşağıdaki örnekte çalıştırın:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Ağ İzleyicisi ile REST API'si oluşturma

ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient chocolatey konumunda bulunan [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

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

Ağ İzleyicisi örneği eklendiğinde, sanal makineler içinde paket yakalama etkinleştirebilirsiniz. Bilgi edinmek için bkz [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)
