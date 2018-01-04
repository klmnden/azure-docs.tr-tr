---
title: "Azure Ağ İzleyicisi topolojisi - REST API görüntülemek | Microsoft Docs"
description: "Bu makalede, REST API ağ topolojinizi sorgulamak için nasıl kullanılacağını anlatmaktadır."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: fefeae4e816994d3ee69d6ac1c1cbe6cf8bbd06e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a>Ağ İzleyicisi topolojisi REST API ile görüntüleme

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

Ağ İzleyicisi'nin topoloji özelliği görsel bir abonelik ağ kaynaklarında sağlar. Portalda, bu görselleştirme için otomatik olarak sunulur. Portal topoloji görünümünde ardındaki bilgiler PowerShell aracılığıyla alınabilir.
Bu özelliği olan topoloji bilgisi veri görselleştirmesi oluşturmak için diğer araçları tarafından kullanılabilecek daha verimli hale getirir.

Bağlantısı altında iki ilişki modellenir.

- **Kapsama** -örnek: VNet bir alt ağ içeren bir NIC içerir
- **İlişkili** -örnek: NIC VM ile ilişkili

Topoloji REST API'si sorgulanırken döndürülen özellikler listelenmiştir.

* **ad** -kaynağın adı
* **Kimliği** -kaynak URI'si.
* **Konum** -kaynağın bulunduğu konum.
* **ilişkileri** -başvurulan nesne ilişkilerini listesi.
    * **ad** -başvurulan kaynağın adı.
    * **ResourceId** -ResourceId association'ında başvurulan kaynak URI'si değil.
    * **associationType** -bu değer alt nesne ve üst arasındaki ilişkiyi başvuruyor. Geçerli değerler **içerir** veya **ilişkilendirilmiş**.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, topoloji bilgilerini alır. ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulundu üzerinde adresindeki chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo belirtilen kaynak grubu için topoloji yanıtı alır.

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum

Armclient Azure kimlik bilgilerinizle oturum açın.

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a>Topoloji alma

Aşağıdaki örnek topoloji REST API istekleri.  Örnek oluşturma esneklik sağlamak amacıyla örnek parametreli.  Tüm değerlerle değiştirin \< \> bunları çevreleyen.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name to run topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

Aşağıdaki yanıtı zaman döndürülür kısaltılmış bir yanıt örneği almak için bir kaynak grubu topolojisi şöyledir:

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek NSG akış günlüklerinizi Power BI ile görselleştirme öğrenin [görselleştirmek NSG akar Power BI ile günlükleri](network-watcher-visualize-nsg-flow-logs-power-bi.md)

