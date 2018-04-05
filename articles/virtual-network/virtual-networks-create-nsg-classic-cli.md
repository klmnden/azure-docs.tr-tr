---
title: Azure CLI 1.0 kullanarak bir ağ güvenlik grubu (Klasik) oluşturun | Microsoft Docs
description: Azure CLI 1.0 kullanarak bir ağ güvenlik grubu (Klasik) oluşturup öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 67b959378521e5a456a2e464d6dd04bbdcab4f30
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-network-security-group-classic-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak bir ağ güvenlik grubu (Klasik) oluşturun
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde Nsg'leri oluşturma](tutorial-filter-network-traffic-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları zaten senaryoyu temel alarak oluşturulan basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce test ortamı tarafından yapı [bir VNet oluşturma](virtual-networks-create-vnet-classic-cli.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Ön uç alt ağı için bir NSG oluşturma

1. Azure CLI hiç kullanmadıysanız bkz [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md).
2. Klasik moduna geçin:

    ```azurecli
    azure config mode asm
    ```   

3. Bir NSG oluşturma::
   
    ```azurecli   
     azure network nsg create -l uswest -n NSG-FrontEnd
    ```
   
4. Bağlantı noktası 3389 (RDP) internet'ten erişim veren bir güvenlik kuralı oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   ```

5. İnternet'ten bağlantı noktası 80 (HTTP) erişim sağlayan bir kural oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
    ```   

6. Ön uç alt ağı için NSG ilişkilendirin:
   
    ```azurecli
    azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   ```

## <a name="create-the-nsg-for-the-back-end-subnet"></a>Arka uç alt ağı için NSG oluşturma

1. NSG oluşturun:
   
    ```azurecli
    azure network nsg create -l uswest -n NSG-BackEnd
   ```

2. Ön uç alt ağından bağlantı noktası 1433 (SQL) erişim sağlayan bir kural oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   ```

3. İnternet'e erişimini engellediği bir kural oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   ```

4. Arka uç alt ağı için NSG ilişkilendirin:
   
    ```azurecli
    azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
    ```