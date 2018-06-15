---
title: Ağ güvenlik grupları - Azure PowerShell yönetme | Microsoft Docs
description: Ağ güvenlik grupları PowerShell kullanarak yönetmeyi öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 3706ce6c-d9ae-46cb-a048-f0a4e84dc5cc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca7f4926ca4edf9d20612aca74f6ae5f0ed847b3
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2018
ms.locfileid: "30960384"
---
# <a name="manage-network-security-groups-using-powershell"></a>PowerShell kullanarak ağ güvenlik gruplarını yönet

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Bilgi alma
Varolan Nsg'lerinizi görüntülemek için varolan bir NSG kuralları almak ve hangi kaynakların bir NSG için ilişkili olduğunu öğrenin.

### <a name="view-existing-nsgs"></a>Varolan Nsg'ler görüntüleyin
Bir Abonelikteki tüm mevcut Nsg'ler görüntülemek için çalıştırın `Get-AzureRmNetworkSecurityGroup` cmdlet'i.

Beklenen sonucu:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Belirli bir kaynak grubunda Nsg'ler listesini görüntülemek için Çalıştır `Get-AzureRmNetworkSecurityGroup` cmdlet'i.

Beklenen çıktı:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

### <a name="list-all-rules-for-an-nsg"></a>Bir NSG için tüm kuralları listesinde
Adlı bir NSG kurallarını görüntülemek için **NSG ön uç**, aşağıdaki komutu girin:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

Beklenen çıktı:

    Name                     : rdp-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound

    Name                     : web-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

> [!NOTE]
> Aynı zamanda `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` varsayılan kurallar listelemek için **NSG ön uç** NSG.
> 

### <a name="view-nsgs-associations"></a>Nsg'ler ilişkilendirmelerini görüntülemek
Hangi kaynakları görüntülemek için **NSG ön uç** NSG olan ilişkilendirmek, aşağıdaki komutu çalıştırın:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

Ara **NetworkInterfaces** ve **alt ağlar** aşağıda gösterildiği gibi özellikleri:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Önceki örnekte, NSG herhangi ağ arabirimlerine (NIC'ler); ilişkili değil adlı bir alt ağ ile ilişkilendirilene **ön uç**.

## <a name="manage-rules"></a>Kuralları yönetme
Varolan bir NSG kuralları ekleme, mevcut kuralları düzenlemek ve kuralları kaldırın.

### <a name="add-a-rule"></a>Kural ekleme
İzin verme kuralı eklemek için **gelen** bağlantı noktası trafiği **443** için herhangi bir makineden **NSG ön uç** NSG, aşağıdaki adımları tamamlayın:

1. Varolan NSG almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. NSG'yi bir kural eklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. NSG'yi yapılan değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    Yalnızca güvenlik kuralları gösteren beklenen çıktı:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Bir kural değiştirme
Öğesinden gelen trafiğe izin vermek için yukarıda oluşturduğunuz kural değiştirmek için **Internet** yalnızca, aşağıdaki adımları izleyin.

1. Varolan NSG almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Yeni Kural ayarlarıyla aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix Internet `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. NSG'yi yapılan değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Yalnızca güvenlik kuralları gösteren beklenen çıktı:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Kural silme
1. Varolan NSG almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. NSG kuralı kaldırmak için aşağıdaki komutu çalıştırın:

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. Aşağıdaki komutu çalıştırarak NSG'yi, yaptığınız değişiklikleri kaydedin:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Beklenen çıktı bildirimi yalnızca güvenlik kuralları gösteren **https kuralı** artık listelenir:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>İlişkileri yönetme
Bir NSG'yi alt ağlara ve NIC ilişkilendirebilirsiniz. Bir NSG'yi ilişkili olduğu herhangi bir kaynaktan ilişkisini kaldırın.

### <a name="associate-an-nsg-to-a-nic"></a>Bir NSG'yi bir NIC ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **TestNICWeb1** NIC, aşağıdaki adımları tamamlayın:

1. Varolan NSG almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Mevcut NIC'in almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. Ayarlama **NetworkSecurityGroup** özelliği **NIC** değişken değerini **NSG** değişken, aşağıdaki komutu girerek:

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. NIC'ye yapılan değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Çıktı yalnızca gösteren beklenen **NetworkSecurityGroup** özelliği:
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Bir NSG'yi bir NIC gelen ilişkilendirmesini Kaldır
İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **TestNICWeb1** NIC, aşağıdaki adımları tamamlayın:

1. Mevcut NIC'in almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. Ayarlama **NetworkSecurityGroup** özelliği **NIC** değişkenini **$null** aşağıdaki komutu çalıştırarak:

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. NIC'ye yapılan değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Çıktı yalnızca gösteren beklenen **NetworkSecurityGroup** özelliği:
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Bir NSG'yi bir alt ağdan ilişkilendirmesini Kaldır
İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **ön uç** alt ağ, aşağıdaki adımları tamamlayın:

1. Mevcut VNet almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Almak için aşağıdaki komutu çalıştırın **ön uç** alt ağı ve bir değişkende saklayın:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Ayarlama **NetworkSecurityGroup** özelliği **alt** değişkenini **$null** aşağıdaki komutu girerek:

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. Alt ağa yapılan değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Beklenen çıktı yalnızca özelliklerini gösteren **ön uç** alt ağ. Bir özellik için hiç duyuru **NetworkSecurityGroup**:
   
            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Bir NSG'yi bir alt ağ ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **FronEnd** alt yeniden, aşağıdaki adımları tamamlayın:

1. Mevcut VNet almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Almak için aşağıdaki komutu çalıştırın **ön uç** alt ağı ve bir değişkende saklayın:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Varolan NSG almak ve bir değişkeni depolamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. Ayarlama **NetworkSecurityGroup** özelliği **alt** değişkenini **$null** aşağıdaki komutu çalıştırarak:

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. Alt ağa yapılan değişiklikleri kaydetmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Çıktı yalnızca gösteren beklenen **NetworkSecurityGroup** özelliği **ön uç** alt ağ:
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Bir NSG'yi Sil
Herhangi bir kaynağa ilişkili olmayan bir NSG'yi yalnızca silebilirsiniz. Bir NSG'yi silmek için aşağıdaki adımları izleyin.

1. Bir NSG'yi ilişkili tüm kaynakları denetlemek için çalıştırın `azure network nsg show` gösterildiği gibi [görünüm Nsg'ler ilişkilendirmeleri](#View-NSGs-associations).
2. NSG herhangi NIC'ler ilişkiliyse çalıştırmak `azure network nic set` gösterildiği gibi [bir NSG'yi bir NIC gelen ilişkilendirmesini](#Dissociate-an-NSG-from-a-NIC) her NIC için 
3. NSG herhangi bir alt ağ ile ilişkili ise, çalıştırın `azure network vnet subnet set` gösterildiği gibi [bir NSG bir alt ağdan ilişkilendirmesini](#Dissociate-an-NSG-from-a-subnet) her alt ağ için.
4. NSG silmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > `-Force` Parametresi sağlar silmeyi onaylamak gerekmez.
   > 

## <a name="next-steps"></a>Sonraki adımlar
* [Günlük kaydını etkinleştir](virtual-network-nsg-manage-log.md) Nsg'ler için.

