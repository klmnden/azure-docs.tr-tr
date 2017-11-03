---
title: "Azure CLI 1.0 kullanarak yönlendirme ve sanal gereçler denetlemek | Microsoft Docs"
description: "Azure CLI 1.0 kullanarak yönlendirme ve sanal gereçler kontrol öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 5f21bc7a4fcd9507ea9d6b2b752a2328a7b834f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-10"></a>Kullanıcı tanımlı yolları (UDR) Azure CLI 1.0 kullanarak oluşturma

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Şablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (Klasik)](virtual-network-create-udr-classic-ps.md)
> * [CLI (Klasik)](virtual-network-create-udr-classic-cli.md)

Özel Yönlendirme ve Azure CLI kullanarak sanal gereçler oluşturun.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri 

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz: 

- [Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](virtual-network-create-udr-arm-cli.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları yukarıda senaryo tabanlı oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce test ortamında dağıtarak yapı [bu şablonu](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), tıklatın **Azure'a Dağıt**, gerekirse varsayılan parametre değerlerini değiştirin ve Portalı'ndaki yönergeleri izleyin.


## <a name="create-the-udr-for-the-front-end-subnet"></a>Ön uç alt ağ için UDR oluşturma
Yol tablosu ve yukarıdaki senaryoyu temel ön uç alt ağ için gereken rota oluşturmak için aşağıdaki adımları izleyin.

1. Ön uç alt ağ için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Çıktı:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Parametreler:
   
   * **-g (veya --resource-group)**. UDR oluşturulacağı kaynak grubunun adı. Bizim senaryomuz için bu *TestRG* ’dir.
   * **-l (veya --location)**. Yeni UDR oluşturulacağı azure bölgesi. Bizim senaryomuz için *westus*.
   * **-n (veya --name)**. Yeni UDR adı. Bizim senaryomuz için *UDR ön uç*.
2. İçin bir yol (192.168.2.0/24) arka uç alt ağa giden tüm trafiği göndermek için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Çıktı:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Parametreler:
   
   * **-r (veya--yol tablosu adı)**. Rota ekleneceği yol tablosu adı. Bizim senaryomuz için *UDR ön uç*.
   * **-a (veya--adres-önek)**. Paketler için burada hedefleyen alt ağ adresi öneki. Bizim senaryomuz için *192.168.2.0/24*.
   * **-y (veya--sonraki atlama türü)**. Nesne trafik türü için gönderilir. Olası değerler şunlardır: *değerinin VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, veya *hiçbiri*.
   * **-p (veya--sonraki atlama IP adresi**). Sonraki atlama IP adresi. Bizim senaryomuz için *192.168.0.4*.
3. Yukarıda oluşturulan yol tablosu ilişkilendirmek için aşağıdaki komutu çalıştırın **ön uç** alt ağ:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Çıktı:
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Parametreler:
   
   * **-e (veya--vnet-ad)**. Alt ağ bulunduğu Vnet'in adı. Bizim senaryomuz için bu *TestVNet* ’tir.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Arka uç alt ağ için UDR oluşturma
Yol tablosu ve yukarıdaki senaryoyu temel arka uç alt ağ için gereken rota oluşturmak için aşağıdaki adımları tamamlayın:

1. Arka uç alt ağ için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. İçin bir yol (192.168.1.0/24) ön uç alt ağa giden tüm trafiği göndermek için yol tablosu oluşturmak için aşağıdaki komutu çalıştırın **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Yol tablosu ile ilişkilendirmek için aşağıdaki komutu çalıştırın **arka uç** alt ağ:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>FW1 üstünde IP iletimini etkinleştirme
Tarafından kullanılan NIC IP iletme etkinleştirmek için **FW1**, aşağıdaki adımları tamamlayın:

1. İzler ve değeri fark komutu çalıştırmak **etkinleştirmek IP iletimini**. Kümesine *false*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Çıktı:
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. IP iletmeyi etkinleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Çıktı:
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Parametreler:
   
   * **-f (veya--etkinleştir IP İletimi)**. *doğru* veya *false*.

