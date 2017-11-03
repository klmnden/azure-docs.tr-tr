---
title: "Azure Ağ İzleyicisi güvenlik grubu görünümü - Azure CLI 1.0 ile ağ güvenliği çözümleme | Microsoft Docs"
description: "Bu makalede, Azure CLI 1.0 güvenlik grubu görünümü ile sanal makineleri güvenliği çözümlemek için nasıl kullanılacağını anlatmaktadır."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: f27157129bea4e47a2e0e6cc1169b9e4887bdd78
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a>Güvenlik grubu Azure CLI 1.0 kullanarak görünümü ile sanal makine güvenliğinizi Çözümle

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Güvenlik grubu görünümü bir sanal makineye uygulanan yapılandırılmış ve etkili ağ güvenlik kuralları döndürür. Bu denetim ve ağ güvenlik grupları ve trafik yükleniyor emin olmak için bir VM üzerinde yapılandırılmış kurallarını tanılamak yararlı bir yetenektir doğru şekilde izin verilen veya reddedilen. Bu makalede, Azure CLI kullanarak bir sanal makine için yapılandırılmış ve etkili güvenlik kuralları almak nasıl gösteriyoruz

Bu makalede, platformlar arası Azure CLI 1.0, Windows, Mac ve Linux için kullanılabilir olduğu kullanır. Ağ İzleyicisi, CLI desteği şu anda Azure CLI 1.0 kullanır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo, belirli bir sanal makine için yapılandırılmış ve etkili güvenlik kurallarını alır.

## <a name="get-a-vm"></a>VM Al

Bir sanal makineyi çalıştırmak için gerekli `vm list` cmdlet'i. Aşağıdaki komut bir kaynak grubunda sanal machinese listeler:

```azurecli
azure vm list -g resourceGroupName
```

Sanal makine öğrendikten sonra kullanabileceğiniz `vm show` kendi kaynak kimliği almak için cmdlet:

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümü alma

Güvenlik grubu Görünüm sonucu almak için sonraki adımdır bakın. Ekleme "--json" json sonuçları bayrağı biçimlendirecek.

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-the-results"></a>Sonuçları görüntüleme

Döndürülen sonuçlar kısaltılmış yanıtın örnektir. Gruplar halinde ayrılmış sanal makine üzerinde etkili ve uygulanan güvenlik kuralları sonuçları göster **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, ve **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [denetim ağ güvenlik grupları (NSG) Ağ İzleyicisi ile](network-watcher-nsg-auditing-powershell.md) ağ güvenlik grupları doğrulanması otomatikleştirmek öğrenmek için.

Ağ kaynaklarınıza ziyaret ederek uygulanan güvenlik kuralları hakkında daha fazla bilgi [güvenlik grubu Görünümü'ne genel bakış](network-watcher-security-group-view-overview.md)
