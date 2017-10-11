---
title: "Denetim bir Azure Virtual Network - PowerShell - Klasik yönlendirme | Microsoft Docs"
description: "PowerShell kullanarak Vnet'lerde yönlendirilmesini denetlemesini öğrenin | Klasik"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: e9564d223cb85529f1fa97bc398d35c6debcedae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Yönlendirme denetlemek ve PowerShell kullanarak sanal gereçler (Klasik) kullanma

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Şablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (Klasik)](virtual-network-create-udr-classic-ps.md)
> * [CLI (Klasik)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure’da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-resource-manager/resource-manager-deployment-model.md) iyice anladığınızdan emin olun. Bu makalenin üst seçeneklerden birini seçerek, farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Örnek senaryo yukarıdaki Azure PowerShell önceden oluşturulmuş basit bir ortam aşağıdaki komutları beklediğiniz temel. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, gösterildiği ortamı oluşturmak [PowerShell kullanarak bir sanal ağ (Klasik) oluşturmak](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Ön uç alt UDR oluşturma
Yol tablosu ve yukarıdaki senaryoyu temel ön uç alt ağ için gereken rota oluşturmak için aşağıdaki adımları izleyin.

1. Ön uç alt ağ için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. İçin bir yol (192.168.2.0/24) arka uç alt ağa giden tüm trafiği göndermek için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **ön uç** alt ağ:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>Arka uç alt ağ için UDR oluşturma
Yol tablosu ve senaryoyu temel arka uç alt ağ için gereken rota oluşturmak için aşağıdaki adımları tamamlayın:

1. Arka uç alt ağ için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. İçin bir yol (192.168.1.0/24) ön uç alt ağa giden tüm trafiği göndermek için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **arka uç** alt ağ:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>FW1 VM'de IP iletimini etkinleştirme

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
