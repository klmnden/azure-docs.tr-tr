---
title: Azure Ağ İzleyicisi örneği oluşturma | Microsoft Docs
description: Bir Azure bölgesinde Ağ İzleyicisini etkinleştirme hakkında bilgi edinin.
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
ms.openlocfilehash: 601a3f273a8da9100d24dfdbd13bd598b0e48884
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59051572"
---
# <a name="create-an-azure-network-watcher-instance"></a>Azure Ağ İzleyicisi örnek oluşturma

Ağ İzleyicisi, koşulları ağ senaryosu düzeyinde, azure'a veya azure'dan izlemenizi ve tanılamanızı sağlayan bölgesel bir hizmettir. İzleme senaryosu düzeyinde bir uçtan uca ağ düzeyinde görünüm, sorunları tanılamak sağlar. Ağ Tanılama ve görselleştirme araçları Ağ İzleyicisi ile kullanılabilen anlamanıza, tanılamanıza ve ağınıza azure'da Öngörüler elde etmeye yardımcı olur. Ağ İzleyicisi, bir Ağ İzleyicisi kaynağı oluşturulmasını etkinleştirilir. Bu kaynak, Ağ İzleyicisi becerilerinden olanak tanır.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="network-watcher-is-automatically-enabled"></a>Ağ İzleyicisi otomatik olarak etkinleştirildi.
Oluşturun veya aboneliğinizdeki sanal ağ güncelleştirmesi, sanal ağınızın bölgede Ağ İzleyicisi otomatik olarak etkinleştirilecektir. Ağ İzleyicisi’nin otomatik olarak etkinleştirilmesi sırasında kaynaklarınız veya bu hizmete ilişkin ücretler etkilenmez.

#### <a name="opt-out-of-network-watcher-automatic-enablement"></a>Ağ İzleyicisi otomatik etkinleştirme, çevirme
Ağ İzleyicisi otomatik etkinleştirme dışında bırakmak isterseniz, aşağıdaki komutları çalıştırarak bunu yapabilirsiniz:

> [!WARNING]
> Ağ İzleyicisi otomatik etkinleştirmeyi tercih eden-out kalıcı bir değişikliktir. Sonra çevirme, olmadan katılımı olamaz [destekle iletişim kurarak](https://azure.microsoft.com/support/options/)

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName DisableNetworkWatcherAutocreation -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

```azurecli-interactive
az feature register --name DisableNetworkWatcherAutocreation --namespace Microsoft.Network
az provider register -n Microsoft.Network
```



## <a name="create-a-network-watcher-in-the-portal"></a>Portalda bir Ağ İzleyicisi oluşturma

Gidin **tüm hizmetleri** > **ağ** > **Ağ İzleyicisi**. Ağ İzleyicisi için etkinleştirmek istediğiniz abonelikleri seçebilirsiniz. Bu eylem, kullanılabilir olan her bölgede Ağ İzleyicisi oluşturur.

![Ağ İzleyicisi oluşturma](./media/network-watcher-create/figure1.png)

Portalı kullanarak Ağ İzleyicisi'ni etkinleştirdiğinizde, Ağ İzleyicisi örneğinin adını otomatik olarak ayarlanır *NetworkWatcher_region_name* burada *region_name* Azure bölgesine karşılık gelir. burada örnek etkin. Örneğin, bir Ağ İzleyicisi Batı Orta ABD bölgesinde etkin olarak da adlandırılır *NetworkWatcher_westcentralus*.

Ağ İzleyicisi örneği otomatik olarak adlı bir kaynak grubunda oluşturulan *NetworkWatcherRG*. Kaynak grubu, zaten yoksa, oluşturulur.

Bir Ağ İzleyicisi örneği ve kaynak grubu adını özelleştirmek istiyorsanız içine yerleştirilir, Powershell, Azure CLI, REST API veya izleyen bölümlerde açıklanan ARMClient yöntemleri kullanabilirsiniz. Ağ İzleyicisi içinde oluşturmadan önce her seçenekte, kaynak grubunun mevcut olması gerekir.  

## <a name="create-a-network-watcher-with-powershell"></a>PowerShell ile bir Ağ İzleyicisi oluşturma

Ağ İzleyicisi bir örneğini oluşturmak için aşağıdaki örneği çalıştırın:

```powershell
New-AzNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Azure CLI ile bir Ağ İzleyicisi oluşturma

Ağ İzleyicisi bir örneğini oluşturmak için aşağıdaki örneği çalıştırın:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>REST API ile bir Ağ İzleyicisi oluşturma

ARMclient, PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient, chocolatey bulunur [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Oturum ARMClient oturum açın

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

Ağ İzleyicisi örneğini olduğuna göre kullanılabilen özellikler hakkında bilgi edinin:

* [Topoloji](network-watcher-topology-overview.md)
* [Paket yakalama](network-watcher-packet-capture-overview.md)
* [IP akışı doğrulama](network-watcher-ip-flow-verify-overview.md)
* [Sonraki atlama](network-watcher-next-hop-overview.md)
* [Güvenlik grubu görünümü](network-watcher-security-group-view-overview.md)
* [NSG akış günlüğe kaydetme](network-watcher-nsg-flow-logging-overview.md)
* [Sanal ağ geçidi sorunlarını giderme](network-watcher-troubleshoot-overview.md)

Ağ İzleyicisi örneği olduğunda, sanal makineleri içinde paket yakalaması etkinleştirebilirsiniz. Bilgi edinmek için bkz [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)
