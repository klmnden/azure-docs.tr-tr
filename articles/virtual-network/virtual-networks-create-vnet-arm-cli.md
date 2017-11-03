---
title: "Azure CLI 2.0 - sanal ağ oluşturma | Microsoft Docs"
description: "Azure CLI 2.0 kullanarak bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c7d7b3543f488aedff1ea2c68a2b497e0ca744af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanan bir sanal ağ oluşturma

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure'un iki dağıtım modeli vardır: Azure Resource Manager ve klasik. Microsoft, kaynakların Resource Manager dağıtım modeliyle oluşturulmasını önerir. İki model arasındaki farkları öğrenmek için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md) makalesini okuyun.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız
- [Azure CLI 2.0](#create-a-virtual-network) -bizim nesil CLI kaynak yönetimi dağıtım modeli (Bu makalede) için '
 
    Resource Manager’da farklı araçlar kullanarak da sanal ağ kurabilir veya aşağıdaki listeden farklı bir seçenek belirleyerek klasik dağıtım modeliyle sanal ağ oluşturabilirsiniz:

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

Azure CLI 2.0 kullanarak bir sanal ağ oluşturmak için aşağıdaki adımları tamamlayın:

1. Yükleme ve yapılandırma en son [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/#login).

2. VNet kullanarak bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#create) komutunu `--name` ve `--location` bağımsız değişkenler:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. VNet ve bir alt ağ oluşturun:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Beklenen çıktı:
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    Kullanılan parametreler:

    - `--name TestVNet`: Oluşturulacak Vnet'in adı.
    - `--resource-group TestRG`: # Kaynağı denetleyen kaynak grubu adı. 
    - `--location centralus`: Konumuna dağıtmak için.
    - `--address-prefix 192.168.0.0/16`: Adres öneki ve blok.  
    - `--subnet-name FrontEnd`: Alt ağ adı.
    - `--subnet-prefix 192.168.1.0/24`: Adres öneki ve blok.

    Sonraki komut içinde kullanmak için temel bilgileri listelemek için VNet kullanarak Sorgulayabileceğiniz bir [sorgu filtresi](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Bu şu çıkışı üretir:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Bir alt ağ oluşturun:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Beklenen çıktı:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Kullanılan parametreler:

    - `--address-prefix 192.168.2.0/24`: Alt ağ CIDR bloğu.
    - `--name BackEnd`: Yeni bir alt ağ adı.
    - `--resource-group TestRG`: Kaynak grubu.
    - `--vnet-name TestVNet`: Sahibi olan vnet'in adı.

5. Yeni Vnet'in özelliklerini sorgu:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Beklenen çıktı:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Alt ağlar özelliklerini sorgu:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Beklenen çıktı:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Sonraki adımlar

Nasıl bağlayacağınızı öğrenin:

- Sanal makine (VM) okuyarak sanal bir ağa [bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md) makalesi. Makalelerde yer alan adımlarda sanal ağ ve alt ağ oluşturmak yerine var olan sanal ağı ve alt ağı seçerek VM bağlantısı yapabilirsiniz.
- Bir sanal ağı diğer sanal ağlara bağlamak için [Sanal ağları bağlama](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesini okuyun.
- Bir sanal ağı şirket içi ağa bağlamak için siteden siteye sanal özel ağ (VPN) veya ExpressRoute devresi kullanın. Bilgi okuyarak nasıl [bir siteden siteye VPN kullanarak şirket içi ağı için bir sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) ve [expressroute bağlantı hattına bir VNet bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).