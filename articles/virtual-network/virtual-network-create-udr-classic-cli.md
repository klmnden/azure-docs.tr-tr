---
title: Denetim içinde bir Azure sanal ağ - CLI - Klasik yönlendirme | Microsoft Docs
description: Klasik dağıtım modelinde Azure CLI kullanarak sanal ağ yönlendirmesini denetlemeyi öğrenin
services: virtual-network
documentationcenter: na
author: genlin
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
ms.openlocfilehash: e1b8bb3544a08b60564ceb5bd7e1666214059e09
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743930"
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Yönlendirilmesini denetlemesini ve Azure CLI kullanarak sanal gereçler (Klasik) kullanma

> [!div class="op_single_selector"]
> * [PowerShell](tutorial-create-route-table-powershell.md)
> * [Azure CLI](tutorial-create-route-table-cli.md)
> * [PowerShell (Klasik)](virtual-network-create-udr-classic-ps.md)
> * [CLI (Klasik)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [denetim Yönlendirme ve Resource Manager dağıtım modelinde sanal gereçler kullanma](tutorial-create-route-table-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları, yukarıdaki senaryo temel alınarak oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, gösterilen ortam oluşturma [Azure CLI kullanarak bir Vnet'e (Klasik) oluşturmak](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Ön uç alt ağı için UDR oluşturun
Yol tablosu ve yukarıdaki senaryo temel alınarak ön uç alt ağı için rota oluşturmak için aşağıdaki adımları izleyin.

1. Klasik moda geçmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure config mode asm
    ```

    Çıkış:

        info:    New mode is asm

2. Bir ön uç alt ağı için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Çıkış:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Parametreler:
   
   * **-l (veya --konum)**. Yeni NSG oluşturulacağı azure bölgesi. Bizim senaryomuz için *westus*.
   * **-n (veya --name)**. Yeni NSG'nin adı. Bizim senaryomuz için *NSG ön uç*.
3. İçin bir yol arka uç alt ağına (192.168.2.0/24) hedefleyen tüm trafiği göndermek için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Çıkış:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Parametreler:
   
   * **-r (veya--yol tablosu adı)**. Rota ekleneceği yol tablosunun adı. Bizim senaryomuz için *UDR ön uç*.
   * **-a (veya--adres-önek)**. Burada paketleri birleştirilmek üzere bir alt ağ için adres ön eki. Bizim senaryomuz için *192.168.2.0/24*.
   * **-t (veya--sonraki atlama türü)**. Nesne trafik türü için gönderilir. Olası değerler *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, veya *yok*.
   * **-p (veya--sonraki atlama IP adresi**). Sonraki atlama IP adresi. Bizim senaryomuz için *192.168.0.4*.
4. Oluşturulan rota tablosunu ilişkilendirmek için aşağıdaki komutu çalıştırın **ön uç** alt ağı:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Çıkış:
   
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
   * **-n (veya--alt ağ adı**. Adı alt ağın yol tablosuna eklenir. Bizim senaryomuz için bu *FrontEnd* ’dir.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Arka uç alt ağı için UDR oluşturun
Yol tablosu ve senaryo temel alınarak arka uç alt ağı için rota oluşturmak için aşağıdaki adımları tamamlayın:

1. Arka uç alt ağı için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. İçin bir yol (192.168.1.0/24) ön uç alt ağı hedefleyen tüm trafiği göndermek için rota tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **arka uç** alt ağı:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

