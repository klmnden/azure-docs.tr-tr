---
title: Denetim bir Azure Virtual Network - CLI - Klasik yönlendirme | Microsoft Docs
description: Azure CLI kullanarak Klasik dağıtım modelinde sanal ağlara yönlendirme denetim öğrenin
services: virtual-network
documentationcenter: na
author: genli
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: f158270df344ebd55601b3f625aaabc284ad1323
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Yönlendirme denetlemek ve Azure CLI kullanarak sanal gereçler (Klasik) kullanma

> [!div class="op_single_selector"]
> * [PowerShell](tutorial-create-route-table-powershell.md)
> * [Azure CLI](tutorial-create-route-table-cli.md)
> * [Şablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (Klasik)](virtual-network-create-udr-classic-ps.md)
> * [CLI (Klasik)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [kontrol Yönlendirme ve Resource Manager dağıtım modelinde sanal gereçler kullanma](tutorial-create-route-table-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları yukarıda senaryo tabanlı oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, gösterildiği ortamı oluşturmak [Azure CLI kullanarak VNet (Klasik) oluşturma](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Ön uç alt UDR oluşturma
Yol tablosu ve yukarıdaki senaryoyu temel ön uç alt ağ için gereken rota oluşturmak için aşağıdaki adımları izleyin.

1. Klasik moduna geçmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure config mode asm
    ```

    Çıktı:

        info:    New mode is asm

2. Ön uç alt ağ için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Çıktı:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Parametreler:
   
   * **-l (veya --konum)**. Yeni NSG oluşturulacağı azure bölgesi. Bizim senaryomuz için *westus*.
   * **-n (veya --name)**. Yeni nsg'nin adı. Bizim senaryomuz için *NSG ön uç*.
3. İçin bir yol (192.168.2.0/24) arka uç alt ağa giden tüm trafiği göndermek için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Çıktı:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Parametreler:
   
   * **-r (veya--yol tablosu adı)**. Rota ekleneceği yol tablosu adı. Bizim senaryomuz için *UDR ön uç*.
   * **-a (veya--adres-önek)**. Paketler için burada hedefleyen alt ağ adresi öneki. Bizim senaryomuz için *192.168.2.0/24*.
   * **-t (veya--sonraki atlama türü)**. Nesne trafik türü için gönderilir. Olası değerler şunlardır: *değerinin VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, veya *hiçbiri*.
   * **-p (veya--sonraki atlama IP adresi**). Sonraki atlama IP adresi. Bizim senaryomuz için *192.168.0.4*.
4. Oluşturulan rota tablosu ilişkilendirmek için aşağıdaki komutu çalıştırın **ön uç** alt ağ:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Çıktı:
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    Parametreler:
   
   * **-t (veya--vnet-ad)**. Alt ağ bulunduğu Vnet'in adı. Bizim senaryomuz için bu *TestVNet* ’tir.
   * **-n (veya--alt ağ adı**. Yol tablosu eklenir alt ağın adı. Bizim senaryomuz için bu *FrontEnd* ’dir.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Arka uç alt ağ için UDR oluşturma
Yol tablosu ve senaryoyu temel arka uç alt ağ için gereken rota oluşturmak için aşağıdaki adımları tamamlayın:

1. Arka uç alt ağ için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. İçin bir yol (192.168.1.0/24) ön uç alt ağa giden tüm trafiği göndermek için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **arka uç** alt ağ:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

