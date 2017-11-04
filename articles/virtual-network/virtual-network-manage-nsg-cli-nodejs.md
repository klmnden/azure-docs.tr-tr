---
title: "Ağ güvenlik grupları - Azure CLI 1.0 yönetme | Microsoft Docs"
description: "Ağ güvenlik grupları Azure komut satırı arabirimi (CLI) 1.0 kullanarak yönetmeyi öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e53c3ff2ffbef95d6b72ca6afb3b4de377f0389
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak ağ güvenlik gruplarını yönet

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri 

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz: 

- [Azure CLI 1.0](#View-existing-NSGs): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız 
- [Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) -bizim gelecek nesil CLI kaynak yönetimi dağıtım modeli (Bu makalede)

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a>Bilgi alma
Varolan Nsg'lerinizi görüntülemek için varolan bir NSG kuralları almak ve hangi kaynakların bir NSG için ilişkili olduğunu öğrenin.

### <a name="view-existing-nsgs"></a>Varolan Nsg'ler görüntüleyin
Belirli bir kaynak grubunda Nsg'ler listesini görüntülemek için Çalıştır `azure network nsg list` komutunu aşağıda gösterildiği gibi.

```azurecli
azure network nsg list --resource-group RG-NSG
```

Beklenen çıktı:

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK

### <a name="list-all-rules-for-an-nsg"></a>Bir NSG için tüm kuralları listesinde
Adlı bir NSG kurallarını görüntülemek için **NSG ön uç**, çalışma `azure network nsg show` komutunu aşağıda gösterildiği gibi. 

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd
```

Beklenen çıktı:

    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

> [!NOTE]
> Aynı zamanda `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` kurallardan listelemek için **NSG ön uç** NSG.
>

### <a name="view-nsg-associations"></a>Görünüm NSG ilişkilendirmeleri

Hangi kaynakları görüntülemek için **NSG ön uç** NSG olan ilişkilendirmek, çalışma `azure network nsg show` komutunu aşağıda gösterildiği gibi. Tek fark kullanımını olduğuna dikkat edin **--json** parametresi.

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json
```

Ara **networkInterfaces** ve **alt ağlar** aşağıda gösterildiği gibi özellikleri:

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

Yukarıdaki örnekte, NSG herhangi ağ arabirimlerine (NIC'ler) ilişkili değil ve adlı bir alt ağ ile ilişkilendirilene **ön uç**.

## <a name="manage-rules"></a>Kuralları yönetme
Varolan bir NSG kuralları ekleme, mevcut kuralları düzenlemek ve kuralları kaldırın.

### <a name="add-a-rule"></a>Kural ekleme
İzin verme kuralı eklemek için **gelen** bağlantı noktası trafiği **443** için herhangi bir makineden **NSG ön uç** NSG, aşağıdaki komutu girin:

```azurecli
azure network nsg rule create --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --description "Allow access to port 443 for HTTPS" \
    --protocol Tcp \
    --source-address-prefix * \
    --source-port-range * \
    --destination-address-prefix * \
    --destination-port-range 443 \
    --access Allow \
    --priority 102 \
    --direction Inbound
```

Beklenen çıktı:

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a>Bir kural değiştirme
Öğesinden gelen trafiğe izin vermek için yukarıda oluşturduğunuz kural değiştirmek için **Internet** yalnızca, aşağıdaki komutu çalıştırın:

```azurecli
azure network nsg rule set --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --source-address-prefix Internet
```

Beklenen çıktı:

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a>Kural silme
Yukarıda oluşturduğunuz kural silmek için aşağıdaki komutu çalıştırın:

```azurecli
azure network nsg rule delete --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --quiet
```

> [!NOTE]
> `--quiet` Parametresi sağlar silmeyi onaylamak gerekmez.
>

Beklenen çıktı:

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a>İlişkileri yönetme
Bir NSG'yi alt ağlara ve NIC ilişkilendirebilirsiniz. Bir NSG'yi ilişkili olduğu herhangi bir kaynaktan ilişkisini kaldırın.

### <a name="associate-an-nsg-to-a-nic"></a>Bir NSG'yi bir NIC ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **TestNICWeb1** NIC, aşağıdaki komutu çalıştırın:

```azurecli
azure network nic set --resource-group RG-NSG \
    --name TestNICWeb1 \
    --network-security-group-name NSG-FrontEnd
```

Beklenen çıktı:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a>Bir NSG'yi bir NIC gelen ilişkilendirmesini Kaldır

İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **TestNICWeb1** NIC, aşağıdaki komutu çalıştırın:

```azurecli
azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""
```

> [!NOTE]
> Bildirim "" (boş) değeri `network-security-group-id` parametresi. Bir NSG'yi bir ilişkilendirmesini kaldırmak nasıl olmasıdır. İle aynı yapamayacağı `network-security-group-name` parametresi.
> 

Beklenen sonucu:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a>Bir NSG'yi bir alt ağdan ilişkilendirmesini Kaldır
İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **ön uç** alt ağ, aşağıdaki komutu çalıştırın:

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-id ""
```

Beklenen çıktı:

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a>Bir NSG'yi bir alt ağ ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **FronEnd** alt yeniden, aşağıdaki komutu çalıştırın:

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-name NSG-FronEnd
```

> [!NOTE]
> Yukarıdaki komut, yalnızca çünkü çalışır **NSG ön uç** NSG olan sanal ağ ile aynı kaynak grubunda **TestVNet**. NSG farklı bir kaynak grubu içinde ise, kullanmanıza gerek `--network-security-group-id` parametresi bunun yerine ve tam kimliği için NSG sağlayın. Çalıştırarak kimliği alabilir `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` ve aramakta **kimliği** özelliği. 
> 

Beklenen çıktı:

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a>Bir NSG'yi Sil
Herhangi bir kaynağa ilişkili olmayan bir NSG'yi yalnızca silebilirsiniz. Bir NSG'yi silmek için aşağıdaki adımları izleyin.

1. Bir NSG'yi ilişkili tüm kaynakları denetlemek için çalıştırın `azure network nsg show` gösterildiği gibi [görünüm Nsg'ler ilişkilendirmeleri](#View-NSGs-associations).
2. NSG herhangi NIC'ler ilişkiliyse çalıştırmak `azure network nic set` gösterildiği gibi [bir NSG'yi bir NIC gelen ilişkilendirmesini](#Dissociate-an-NSG-from-a-NIC) her NIC için 
3. NSG herhangi bir alt ağ ile ilişkili ise, çalıştırın `azure network vnet subnet set` gösterildiği gibi [bir NSG bir alt ağdan ilişkilendirmesini](#Dissociate-an-NSG-from-a-subnet) her alt ağ için.
4. NSG silmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet
    ```

    Beklenen çıktı:

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a>Sonraki adımlar
* [Günlük kaydını etkinleştir](virtual-network-nsg-manage-log.md) Nsg'ler için.

