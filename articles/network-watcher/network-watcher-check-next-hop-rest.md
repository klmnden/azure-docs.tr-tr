---
title: Azure Ağ İzleyicisi sonraki atlama - REST ile sonraki atlama Bul | Microsoft Docs
description: Bu makalede, sonraki atlama türü nedir nasıl bulabilir ve Azure REST API'sini kullanarak sonraki atlama kullanarak IP adresi anlatmaktadır.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b541cd5cb7e49468af2c522b16c3a3b9fe75fd54
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2018
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a>Hangi bir sonraki atlama türü sonraki atlama yetenek Azure REST API'sini kullanarak işlemleri Ağ İzleyicisi içinde kullandığını bulmak

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST API'si](network-watcher-check-next-hop-rest.md)

Sonraki atlama özelliğini get sonraki atlama türü ve belirtilen bir sanal makineye dayalı IP adresi sağlayan Ağ İzleyicisi özelliğidir. Bu özellik, bir sanal makine trafiğe bir ağ geçidi, internet veya sanal ağlar hedefine almak için geçiyorsa belirlemede yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulundu üzerinde adresindeki chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryosu, sonraki atlama, sonraki atlama türü ve IP adresi için bir kaynak bulur Ağ İzleyicisi özelliğini kullanır. Sonraki atlama hakkında daha fazla bilgi için [sonraki atlama genel bakış](network-watcher-next-hop-overview.md).

Bu senaryoda, şunları yapacaksınız:

* Bir sanal makine için sonraki atlama alın.

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum

Armclient Azure kimlik bilgilerinizle oturum açın.

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Bir sanal makine alma

Bir sanal makine döndürmek için aşağıdaki betiği çalıştırın. Bu bilgiler sonraki atlama çalıştırmak için gereklidir.

Aşağıdaki kod, aşağıdaki değişkenleri değerleri gerekir:

- **Subscriptionıd** -abonelik kimliği kullanın.
- **resourceGroupName** -sanal makine içeren bir kaynak grubu adı.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Aşağıdaki çıkışı, aşağıdaki örnekte sanal makinenin kimliği kullanılır:

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a>Sonraki atlama Al

Authorization üstbilgisi oluşturulduktan sonra bir sanal makineden sonraki atlama alınabilir. Aşağıdaki değerleri kod örneğini çalışması için değiştirilmesi gerekir.

> [!Important]
> Ağ İzleyicisi'ni içeren kaynak grubunu istek URI'SİNDEKİ kaynak grubu adı olan Ağ İzleyicisi REST API çağrıları için kaynaklar üzerinde tanılama işlemleri yapıyorsunuz.

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> Sonraki atlama VM kaynak çalıştırmak için ayrılır gerektirir.

## <a name="results"></a>Sonuçlar

Aşağıdaki kod parçacığında, alınan çıkış örneğidir. Sonuçları aşağıdaki değerleri içerir:

* **nextHopType** -bu değer aşağıdaki değerlerden biridir: Internet değerinin VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway veya yok.
* **Nexthopıpaddress** -sonraki atlamanın IP adresi.
* **routeTableId** - rota ile ilişkili yol tablosu için URI ya da değerdir veya kullanıcı tanımlı Hayır ise rota tanımlanan değerini *sistem yolu* döndürülür.

Sonuçları json biçiminde verilmiştir.

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bir sanal makine için sonraki atlama öğrenmek yaptıktan sonra ağ kaynaklarınızın güvenliğini ziyaret ederek görüntüleyebilirsiniz [güvenlik görünümüne genel bakış](network-watcher-security-group-view-overview.md)














