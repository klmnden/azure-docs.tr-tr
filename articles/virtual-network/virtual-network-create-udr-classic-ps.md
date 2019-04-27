---
title: Denetim içinde bir Azure sanal ağ - PowerShell - Klasik yönlendirme | Microsoft Docs
description: PowerShell kullanarak sanal ağ içinde yönlendirme denetlemeyi öğrenin | Klasik
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: 1441ee9a3d4a563ab35cd9b01e8347d8f51b827a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743403"
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Yönlendirilmesini denetlemesini ve PowerShell kullanarak sanal gereçler (Klasik) kullanma

> [!div class="op_single_selector"]
> * [PowerShell](tutorial-create-route-table-powershell.md)
> * [Azure CLI](tutorial-create-route-table-cli.md)
> * [PowerShell (Klasik)](virtual-network-create-udr-classic-ps.md)
> * [CLI (Klasik)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure'da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-resource-manager/resource-manager-deployment-model.md) iyice anladığınızdan emin olun. Bu makalenin üst kısmındaki bir seçenek belirleyerek, farklı araçlarla belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Örnek Azure PowerShell önceden oluşturulmuş basit bir ortam aşağıdaki komutları beklediğiniz, yukarıdaki senaryo temel alınarak. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, gösterilen ortam oluşturma [PowerShell kullanarak bir sanal ağa (Klasik) oluşturmak](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Ön uç alt ağı için UDR oluşturun
Yol tablosu ve yukarıdaki senaryo temel alınarak ön uç alt ağı için rota oluşturmak için aşağıdaki adımları izleyin.

1. Bir ön uç alt ağı için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. İçin bir yol arka uç alt ağına (192.168.2.0/24) hedefleyen tüm trafiği göndermek için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **ön uç** alt ağı:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>Arka uç alt ağı için UDR oluşturun
Yol tablosu ve senaryo temel alınarak arka uç alt ağı için rota oluşturmak için aşağıdaki adımları tamamlayın:

1. Arka uç alt ağı için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. İçin bir yol (192.168.1.0/24) ön uç alt ağı hedefleyen tüm trafiği göndermek için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **arka uç** alt ağı:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>FW1 VM'de IP iletimini etkinleştir

IP FW1 VM'yi iletmeyi etkinleştirmek için aşağıdaki adımları tamamlayın:

1. IP iletimi durumunu denetlemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. IP için iletmeyi etkinleştirmek için aşağıdaki komutu çalıştırın *FW1* VM:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
