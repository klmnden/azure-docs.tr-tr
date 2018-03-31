---
title: "Ağ güvenlik grupları - Azure CLI oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak ağ güvenlik grupları oluşturup öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4827fabf1d8cde366dda8b3a782a2fefe01db8d5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-network-security-groups-using-the-azure-cli"></a>Ağ güvenlik grupları Azure CLI kullanarak oluşturma

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları, önceki senaryoyu temel mevcut basit ortamında bekler. 

## <a name="create-the-nsg-for-the-frontend-subnet"></a>İçin NSG oluşturma `FrontEnd` alt ağ

Adlı bir NSG oluşturmak için *NSG ön uç* önceki senaryoya bağlı olarak, adımları aşağıdaki izleyin.

1. Henüz henüz yükleyin ve en son yapılandırırsanız [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/reference-index#az_login). 

2. Bir NSG kullanarak oluşturduğunuz [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create) komutu. 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    Parametreler:
   
   * `--resource-group`: NSG oluşturulduğu kaynak grubunun adı. Bizim senaryomuz için bu *TestRG* ’dir.
   * `--location`: Yeni NSG oluşturulduğu azure bölgesi. Bizim senaryomuz için *westus*.
   * `--name`: Yeni nsg'nin adı. Bizim senaryomuz için *NSG ön uç*.

    Beklenen çıktı tüm varsayılan kuralları listesi dahil olmak üzere bilgilerinin oldukça bitidir. Aşağıdaki örnek, bir JMESPATH sorgu Filtresi ile kullanarak varsayılan kuralları gösterir `table` çıktı biçimi:

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   Çıktı:

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs to all VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs to Internet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. İle Internet'ten 3389 numaralı bağlantı noktasına (RDP) erişimine izin veren bir kural oluşturmak [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) komutu.

    > [!NOTE]
    > Kullanmakta olduğunuz Kabuk bağlı olarak değiştirmeniz gerekebilir `*` yürütmeden önce bağımsız şekilde genişletmek değil olarak için aşağıdaki bağımsız değişkenleri karakter.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    Beklenen çıktı:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    Parametreler:

    * `--resource-group testrg`: Kullanmak için kaynak grubu. Büyük küçük harf duyarlı olduğunu unutmayın.
    * `--nsg-name NSG-FrontEnd`: Kuralın oluşturulduğu NSG adı.
    * `--name rdp-rule`: Yeni kural için bir ad.
    * `--access Allow`: Kural (Engelle veya izin ver) için erişim düzeyi.
    * `--protocol Tcp`: Protokolü (Tcp, Udp veya *).
    * `--direction Inbound`: (Gelen veya giden) bağlantı yönü.
    * `--priority 100`: Kural için öncelik.
    * `--source-address-prefix Internet`: CIDR veya varsayılan etiketleri kullanılarak kaynak adres ön eki.
    * `--source-port-range "*"`: Bağlantı noktası veya bağlantı noktası aralığı kaynağı. Bağlantı açık bağlantı noktası.
    * `--destination-address-prefix "*"`: CIDR veya varsayılan etiketleri kullanılarak hedef adres ön eki.
    * `--destination-port-range 3389`: Hedef bağlantı noktası veya bağlantı noktası aralığı. Bağlantı isteğini alan bağlantı.



4. Bağlantı noktası 80 (HTTP) Internet'ten erişim sağlayan bir kural oluşturmak **az ağ nsg kuralını** komutu.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    Beklenen putput:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. NSG'yi bağlama **ön uç** alt ağ ile [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) komutu.
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    Beklenen çıktı:
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-the-nsg-for-the-backend-subnet"></a>İçin NSG oluşturma `BackEnd` alt ağ
Adlı bir NSG oluşturmak için *NSG arka uç* önceki senaryoya bağlı olarak, adımları aşağıdaki izleyin.

1. Oluşturma `NSG-BackEnd` NSG ile **az ağ nsg oluşturma**.
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    Adım 2, önceki, olduğu gibi beklenen çıktı varsayılan kuralları da dahil olmak üzere oldukça büyük.
   
2. 1433 numaralı bağlantı noktasına (SQL) erişim sağlayan bir kural oluşturmak `FrontEnd` alt ağ ile **az ağ nsg kuralını** komutu.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    Beklenen çıktı:

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. Kullanarak Internet erişimini engellediği bir kural oluşturmak **az ağ nsg kuralını** komutu.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    Beklenen putput:
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. NSG'yi bağlama `BackEnd` alt ağı kullanarak **az ağ sanal alt ağ kümesi** komutu.
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    Beklenen çıktı:
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```
