---
title: "Azure CLI kullanarak Klasik modda Nsg'ler oluşturma | Microsoft Docs"
description: "Oluşturma ve Azure CLI kullanarak Klasik modda Nsg'ler dağıtma hakkında bilgi edinin"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Azure CLI Nsg'ler (Klasik) oluşturma
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde Nsg'leri oluşturma](virtual-networks-create-nsg-arm-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları yukarıda senaryo tabanlı oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce test ortamı tarafından yapı [bir VNet oluşturma](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Ön uç alt ağı için NSG oluşturma
Adlı adlı bir NSG oluşturmak için **NSG ön uç** yukarıdaki senaryoya bağlı olarak, aşağıdaki adımları izleyin.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Çalıştırma  **`azure config mode`**  aşağıda gösterildiği gibi Klasik moduna geçmek için komutu.
   
        azure config mode asm
   
    Beklenen çıktı:
   
        info:    New mode is asm
3. Çalıştırma  **`azure network nsg create`**  bir NSG oluşturmak için komutu.
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    Beklenen çıktı:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parametreler:
   
   * **-l (veya --location)**. Yeni NSG oluşturulacağı azure bölgesi. Bizim senaryomuz için *westus*.
   * **-n (veya --name)**. Yeni nsg'nin adı. Bizim senaryomuz için *NSG ön uç*.
4. Çalıştırma  **`azure network nsg rule create`**  Internet'ten (RDP) 3389 numaralı bağlantı noktasına erişim sağlayan bir kural oluşturmak için komutu.
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    Beklenen çıktı:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    Parametreler:
   
   * **-a (veya--nsg-name)**. Kural oluşturulacak NSG adı. Bizim senaryomuz için *NSG ön uç*.
   * **-n (veya --name)**. Yeni kural için bir ad. Bizim senaryomuz için *rdp kural*.
   * **-c (veya--eylem)**. Kural (Engelle veya izin ver) için erişim düzeyi.
   * **-p (veya--Protokolü)**. Protokolü (Tcp, Udp veya *) kuralı için.
   * **-r (veya--türü)**. Bağlantı (gelen veya giden) yönü.
   * **-y (veya--öncelik)**. Kural için öncelik.
   * **-f (veya--kaynak adres öneki)**. Kaynak adres ön eki CIDR veya varsayılan etiketleri kullanarak.
   * **-o (veya--kaynak bağlantı noktası aralığı)**. Kaynak bağlantı noktası veya bağlantı noktası aralığı.
   * **-e (veya--hedef adres öneki)**. Hedef adres ön eki CIDR veya varsayılan etiketleri kullanarak.
   * **-u (veya--hedef bağlantı noktası aralığı)**. Hedef bağlantı noktası veya bağlantı noktası aralığı.
5. Çalıştırma  **`azure network nsg rule create`**  erişim bağlantı noktası 80 (HTTP) Internet'ten sağlayan bir kural oluşturmak için komutu.
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    Beklenen putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. Çalıştırma  **`azure network nsg subnet add`**  NSG ön uç alt ağına bağlamak için komutu.
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    Beklenen çıktı:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Arka uç alt ağı için NSG oluşturma
Adlı adlı bir NSG oluşturmak için *NSG arka uç* yukarıdaki senaryoya bağlı olarak, aşağıdaki adımları izleyin.

1. Çalıştırma  **`azure network nsg create`**  bir NSG oluşturmak için komutu.
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    Beklenen çıktı:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parametreler:
   
   * **-l (veya --location)**. Yeni NSG oluşturulacağı azure bölgesi. Bizim senaryomuz için *westus*.
   * **-n (veya --name)**. Yeni nsg'nin adı. Bizim senaryomuz için *NSG ön uç*.
2. Çalıştırma  **`azure network nsg rule create`**  ön uç alt ağından bağlantı noktası 1433 (SQL) erişim sağlayan bir kural oluşturmak için komutu.
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    Beklenen çıktı:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. Çalıştırma  **`azure network nsg rule create`**  Internet'e erişimini engellediği bir kural oluşturmak için komutu.
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    Beklenen putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. Çalıştırma  **`azure network nsg subnet add`**  NSG arka uç alt ağına bağlamak için komutu.
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    Beklenen çıktı:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

