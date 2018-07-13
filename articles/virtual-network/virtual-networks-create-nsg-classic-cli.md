---
title: Azure CLI 1.0 kullanarak bir ağ güvenlik grubu (Klasik) oluşturmak | Microsoft Docs
description: Oluşturma ve Azure CLI 1.0 kullanarak bir ağ güvenlik grubu (Klasik) dağıtma hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: 5468801e56849498d712f51e71cfb31bf068398a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696627"
---
# <a name="create-a-network-security-group-classic-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak bir ağ güvenlik grubu (Klasik) oluşturun
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde Nsg'leri oluşturma](tutorial-filter-network-traffic-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutlarını senaryo temel alınarak oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutlarını çalıştırmak isterseniz, test ortamı tarafından derlediğinizden [sanal ağ oluşturma](virtual-networks-create-vnet-classic-cli.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Ön uç alt ağı için bir NSG oluşturma

1. Hiç Azure CLI kullanmadıysanız, [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md).
2. Klasik moda geçin:

    ```azurecli
    azure config mode asm
    ```   

3. Bir NSG oluşturun:
   
    ```azurecli   
     azure network nsg create -l uswest -n NSG-FrontEnd
    ```
   
4. Bağlantı noktası 3389 (RDP) internet'ten erişim veren bir güvenlik kuralı oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   ```

5. Bağlantı noktası 80 (HTTP) internet'ten erişim veren bir kural oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
    ```   

6. NSG ile ön uç alt ağını ilişkilendirin:
   
    ```azurecli
    azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   ```

## <a name="create-the-nsg-for-the-back-end-subnet"></a>Arka uç alt ağı için NSG oluşturma

1. NSG oluşturun:
   
    ```azurecli
    azure network nsg create -l uswest -n NSG-BackEnd
   ```

2. Bağlantı noktası 1433'ü (SQL) ön uç alt ağından erişim veren bir kural oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   ```

3. İnternet'e erişimi engelleyen bir kural oluşturun:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   ```

4. NSG ile arka uç alt ağını ilişkilendirin:
   
    ```azurecli
    azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
    ```