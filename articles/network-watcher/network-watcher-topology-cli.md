---
title: "Azure Ağ İzleyicisi topolojisi - Azure CLI görüntülemek | Microsoft Docs"
description: "Bu makalede, Azure CLI ağ topolojinizi sorgulamak için nasıl kullanılacağını anlatmaktadır."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 654c23e567d81bff1cb0c3091ba3d8f96f0a3eda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="view-network-watcher-topology-with-azure-cli"></a>Ağ İzleyicisi topolojisi Azure CLI ile görüntüleme

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

Ağ İzleyicisi'nin topoloji özelliği görsel bir abonelik ağ kaynaklarında sağlar. Portalda, bu görselleştirme için otomatik olarak sunulur. Portal topoloji görünümünde ardındaki bilgiler PowerShell aracılığıyla alınabilir.
Bu özelliği olan topoloji bilgisi veri görselleştirmesi oluşturmak için diğer araçları tarafından kullanılabilecek daha verimli hale getirir.

Bu makalede, platformlar arası Azure CLI 1.0, Windows, Mac ve Linux için kullanılabilir olduğu kullanır. Ağ İzleyicisi, CLI desteği şu anda Azure CLI 1.0 kullanır.

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

Bu senaryoda, kullandığınız `network watcher topology` topoloji bilgilerini almak üzere. Olduğundan ayrıca bir makale nasıl [ağ topolojisi REST API ile almak](network-watcher-topology-rest.md).

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo belirtilen kaynak grubu için topoloji yanıtı alır.

## <a name="retrieve-topology"></a>Topoloji alma

`network watcher topology` Cmdlet'i, belirtilen kaynak grubu için topoloji alır. Bağımsız değişken Ekle "--json" json biçiminde oput görüntülemek için

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a>Sonuçlar

Döndürülen sonuçların bir özellik "json yanıt gövdesi için içeren adı" kaynaklarınız `network watcher topology` cmdlet'i.  Yanıt ağ güvenlik grubu ve ilişkilendirmelerinin (diğer bir deyişle, içerir, ilişkilendirilmiş) kaynakları içerir.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ kaynaklarınıza ziyaret ederek uygulanan güvenlik kuralları hakkında daha fazla bilgi [güvenlik grubu Görünümü'ne genel bakış](network-watcher-security-group-view-overview.md)
