---
title: "Azure CLI 1.0 kullanarak bir sanal ağ oluşturma | Microsoft Docs"
description: "Azure CLI 1.0 kullanarak bir sanal ağ oluşturmayı öğrenin | Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f0649c5c8c04dda72d2f147601efb37217f9bade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Azure CLI kullanarak bir sanal ağ oluşturma

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure'un iki dağıtım modeli vardır: Azure Resource Manager ve klasik. Microsoft, kaynakların Resource Manager dağıtım modeliyle oluşturulmasını önerir. İki model arasındaki farkları öğrenmek için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md) makalesini okuyun.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız
- [Azure CLI 1.0](#create-a-virtual-network) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Azure CLI kullanarak bir sanal ağ oluşturmak için aşağıdaki adımları tamamlayın:

1. Yükleme ve Azure CLI içindeki adımları izleyerek yapılandırma [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md) makalesi.

2. VNet ve bir alt ağ oluşturun:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Beklenen çıktı:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Kullanılan parametreler:

   * **--vnet**. Oluşturulacak VNet’in adı. Bizim senaryomuz için bu *TestVNet* ’tir.
   * **-e (veya--adres alanı)**. VNet adres alanı. Bizim senaryomuz için *192.168.0.0*
   * **-i (veya - CIDR)**. Ağ maskesi CIDR biçiminde. Bizim senaryomuz için *16*.
   * **-n (veya--alt ağ adı**). İlk alt ağ adı. Bizim senaryomuz için bu *FrontEnd* ’dir.
   * **-p (veya--alt başlangıç IP'sini)**. Alt ağ veya alt ağ adres alanı için başlangıç IP adresi. Bizim senaryomuz için *192.168.1.0*.
   * **-r (veya--alt ağ CIDR)**. Alt ağ için CIDR biçiminde ağ maskesi. Bizim senaryomuz için *24*.
   * **-l (veya --location)**. VNet oluşturulduğu azure bölgesi. Bizim senaryomuz için *Orta ABD*.

3. Bir alt ağ oluşturun:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Beklenen çıktı:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Kullanılan parametreler:

   * **-t (veya--vnet-ad**. Alt ağın oluşturulacağı VNet’in adı. Bizim senaryomuz için bu *TestVNet* ’tir.
   * **-n (veya --name)**. Yeni alt ağın adı. Bizim senaryomuz için *arka uç*.
   * **-a (veya--adres-önek)**. Alt ağ CIDR bloğu. Dört bizim senaryomuz *192.168.2.0/24*.
   
4. Yeni Vnet'in özelliklerini görüntülemek için:

    ```azurecli
    azure network vnet show
    ```
   
    Beklenen çıktı:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Sonraki adımlar

Nasıl bağlayacağınızı öğrenin:

- Sanal makine (VM) okuyarak sanal bir ağa [bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md) makalesi. Makalelerde yer alan adımlarda sanal ağ ve alt ağ oluşturmak yerine var olan sanal ağı ve alt ağı seçerek VM bağlantısı yapabilirsiniz.
- Bir sanal ağı diğer sanal ağlara bağlamak için [Sanal ağları bağlama](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesini okuyun.
- Bir sanal ağı şirket içi ağa bağlamak için siteden siteye sanal özel ağ (VPN) veya ExpressRoute devresi kullanın. Bilgi okuyarak nasıl [bir siteden siteye VPN kullanarak şirket içi ağı için bir sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) ve [expressroute bağlantı hattına bir VNet bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).