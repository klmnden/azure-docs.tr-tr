---
title: "Azure PowerShell - sanal ağ oluşturma | Microsoft Docs"
description: "PowerShell kullanarak bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a31f4f12-54ee-4339-b968-1a8097ca77d3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e7072ddf51570d46578111e2e392e3cbea53f2aa
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-network-using-powershell"></a>PowerShell kullanarak bir sanal ağ oluşturma

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure'un iki dağıtım modeli vardır: Azure Resource Manager ve klasik. Microsoft, kaynakların Resource Manager dağıtım modeliyle oluşturulmasını önerir. İki model arasındaki farkları öğrenmek için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md) makalesini okuyun.
 
Bu makalede PowerShell kullanarak Resource Manager dağıtım modeli üzerinden bir VNet oluşturma açıklanmaktadır. Resource Manager’da farklı araçlar kullanarak da sanal ağ kurabilir veya aşağıdaki listeden farklı bir seçenek belirleyerek klasik dağıtım modeliyle sanal ağ oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Portal](virtual-networks-create-vnet-arm-pportal.md)
> * [PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [CLI](virtual-networks-create-vnet-arm-cli.md)
> * [Şablon](virtual-networks-create-vnet-arm-template-click.md)
> * [Portal (Klasik)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (Klasik)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [CLI (Klasik)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

PowerShell kullanarak bir sanal ağ oluşturmak için aşağıdaki adımları tamamlayın:

1. Yükleme ve Azure PowerShell içindeki adımları izleyerek yapılandırma [yükleme ve yapılandırma Azure PowerShell](/powershell/azure/overview) makalesi.

2. Gerekirse, yeni bir kaynak grubunu aşağıda gösterildiği gibi oluşturun. Bu senaryoda, bir kaynak grubu oluşturma *TestRG*. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../azure-resource-manager/resource-group-overview.md)’ı ziyaret edin.

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Beklenen çıktı:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. Adlı yeni bir VNet oluşturun *TestVNet*:

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    Beklenen çıktı:

        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"[Id]"
        ProvisioningState          : Succeeded
        Tags                       : 
        AddressSpace               : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                  }
        DhcpOptions                : {}
        Subnets                    : []
        VirtualNetworkPeerings     : []
4. Sanal ağ nesnesini bir değişkende saklayın:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > Adım 3 ve 4 çalıştırarak birleştirebilirsiniz `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.
   > 

5. Bir alt ağ yeni VNet değişkenine ekleyin:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    Beklenen çıktı:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings     : []

6. Yukarıdaki 5. adımı oluşturmak istediğiniz her alt ağ için yineleyin. Aşağıdaki komut oluşturur *arka uç* alt senaryo için:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. Alt ağları oluşturmuş olsanız da, bunlar şu anda yalnızca yukarıdaki 4. adımda oluşturduğunuz VNet’i almak için kullanılan yerel değişkende olacaktır. Azure için değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    Beklenen çıktı:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {
                                  "DnsServers": []
                                }
        Subnets               : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []

## <a name="next-steps"></a>Sonraki adımlar

Nasıl bağlayacağınızı öğrenin:

- Sanal makine (VM) okuyarak sanal bir ağa [bir Windows VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md) makalesi. Makalelerde yer alan adımlarda sanal ağ ve alt ağ oluşturmak yerine var olan sanal ağı ve alt ağı seçerek VM bağlantısı yapabilirsiniz.
- Bir sanal ağı diğer sanal ağlara bağlamak için [Sanal ağları bağlama](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) makalesini okuyun.
- Bir sanal ağı şirket içi ağa bağlamak için siteden siteye sanal özel ağ (VPN) veya ExpressRoute devresi kullanın. Nasıl yapacağınızı öğrenmek için [Siteden siteye VPN kullanarak sanal ağ ile şirket içi ağ arasında bağlantı kurma](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) ve [Bir sanal ağı ExpressRoute devresine bağlama](../expressroute/expressroute-howto-linkvnet-arm.md) makalelerini okuyun.
