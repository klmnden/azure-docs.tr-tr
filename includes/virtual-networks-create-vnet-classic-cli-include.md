---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: cd686e1bf62bbd7f37f61ced767e92918edf919c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60597063"
---
## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Azure CLI kullanarak klasik bir VNet oluşturma
Windows, Linux veya OSX çalıştıran herhangi bir bilgisayarın komut isteminden Azure kaynaklarınızı yönetmek için Azure CLI’yi kullanabilirsiniz.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../articles/cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. VNet ve alt ağ oluşturmak için çalıştırın **azure ağ sanal ağ oluşturma** komutu:
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Beklenen çıktı:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Oluşturulacak VNet’in adı. Senaryo için *TestVNet*
   * **-e (veya--adres alanı)**. VNet adres alanı. Senaryo için *192.168.0.0*
   * **-i (veya - CIDR)**. CIDR biçiminde ağ maskesi. Senaryo için *16*.
   * **-n (veya--alt ağ adı**). İlk alt ağ adı. Senaryo için *ön uç*.
   * **-p (veya--alt başlangıç IP)**. Başlangıç IP adresi alt ağ veya alt ağ adres alanı. Senaryo için *192.168.1.0*.
   * **-r (veya--CIDR alt ağ)**. Alt ağ için CIDR biçiminde ağ maskesi. Senaryo için *24*.
   * **-l (veya --konum)**. VNet oluşturulduğu azure bölgesi. Senaryo için *Orta ABD*.
3. Bir alt ağ oluşturmak için çalıştırılması **azure ağ sanal ağ alt ağı oluşturmanız** komutu:
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Önceki komut için beklenen çıktı:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (veya--vnet-ad**. Alt ağın oluşturulacağı VNet’in adı. Senaryo için *TestVNet*.
   * **-n (veya --name)**. Yeni alt ağın adı. Senaryo için *arka uç*.
   * **-a (veya--adres-önek)**. Alt ağ CIDR bloğu. Senaryo için *192.168.2.0/24*.
4. Yeni vnet'in özelliklerini görüntülemek için çalıştırın **azure ağı vnet show** komutu:
   
            azure network vnet show
   
    Önceki komut için beklenen çıktı:
   
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

