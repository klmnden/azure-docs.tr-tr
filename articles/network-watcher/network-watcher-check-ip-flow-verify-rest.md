---
title: "Trafik doğrulayın ile Azure Ağ İzleyicisi IP akışını doğrulamak - REST | Microsoft Docs"
description: "Bu makalede, trafik için veya bir sanal makineden izin verilen veya reddedilen denetlemek açıklar"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 3ccef9ef521b86ffc1eb6047174f4f9e5d9e4296
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Trafik izin verilen ya da IP akışla reddedildi onay Azure Ağ İzleyicisi'nin bir bileşeni doğrulayın

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST API'si](network-watcher-check-ip-flow-verify-rest.md)


IP akış doğrulayın özelliğidir Ağ İzleyicisi, trafik için veya bir sanal makineden izin verilip verilmediğini doğrulamanızı sağlar. Doğrulama gelen veya giden trafiği için çalıştırılabilir. Bu senaryo, bir sanal makine için bir dış kaynağa veya arka uç iletişim kurabilirsiniz geçerli durumunu almak yararlıdır. IP akış doğrulayın, ağ güvenlik grubu (NSG) kurallarınızı düzgün şekilde yapılandırıldığından ve NSG kuralları tarafından engellenen akışları sorun giderme doğrulamak için kullanılabilir. IP kullanan başka bir nedeni akış durumda engellenen istediğiniz trafiği engellenmiş düzgün tarafından NSG emin olmak için doğrulayın.

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulundu üzerinde adresindeki chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

## <a name="scenario"></a>Senaryo

Bu senaryo, bir sanal makine bağlantı noktası 443 üzerinden başka bir makineye konuşun olmadığını doğrulamak için IP akış doğrulama kullanır. Trafiği reddedilirse, bu trafiği engelleyen güvenlik kuralı döndürür. IP akışı doğrulama hakkında daha fazla bilgi için ziyaret [IP akış doğrulayın genel bakış](network-watcher-ip-flow-verify-overview.md)

Bu senaryoda:

* Bir sanal makine alma
* Çağrı IP akış doğrulayın
* Sonuçları doğrulayın

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Bir sanal makine alma

Bir sanal makine döndürmek için aşağıdaki betiği çalıştırın. Aşağıdaki kod değerleri değişkenleri gerekir:

* **Subscriptionıd** -abonelik kimliği kullanın.
* **resourceGroupName** -sanal makine içeren bir kaynak grubu adı.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Gerekli bilgileri kimliği türü'nün altında: `Microsoft.Compute/virtualMachines`. Sonuçları aşağıdaki kod örneği benzer olmalıdır:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a>Çağrı IP akış doğrulayın

Aşağıdaki örnek belirtilen bir sanal makine için trafiği doğrulamak için bir istek oluşturur. Yanıt trafiğe izin verilip verilmediğini veya trafiği reddedilirse döndürür. Ayrıca, trafiği reddedilirse hangi kural trafiği engeller. döndürür.

> [!NOTE]
> IP akış doğrulayın gerektirir VM kaynak ayrılır.

Komut dosyası kaynak kimliği bir sanal makine ve sanal makinedeki ağ arabirim kartı gerektirir. Bu değerler önceki çıktı tarafından sağlanır.

> [!Important]
> Tüm Ağ İzleyicisi REST için istek URI'SİNDEKİ kaynak grubu adı çağrıdır, Ağ İzleyicisi örneği içeren bir tanılama eylemleri gerçekleştirdiğiniz kaynakları değil.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-the-results"></a>Sonuçları anlama

Geri alma yanıt trafiğe izin verilen veya reddedilen bildirir. Yanıt aşağıdaki örneklerde biri gibi görünür:

**İzin verilen**

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

**Engellendi**

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Trafik engelleniyor ve olmamalıdır, bkz: [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) ağ güvenlik grupları hakkında daha fazla bilgi için.












