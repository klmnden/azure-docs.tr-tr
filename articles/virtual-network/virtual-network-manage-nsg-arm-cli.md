---
title: "Ağ güvenlik grupları - Azure CLI 2.0 yönetme | Microsoft Docs"
description: "Ağ güvenlik grupları Azure komut satırı arabirimi (CLI) 2.0 kullanarak yönetmeyi öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed17d314-07e6-4c7f-bcf1-a8a2535d7c14
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11ec0d3d9e33c06d4c0a164f7fba5dd5cca73872
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanan ağ güvenlik gruplarını yönet

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri 

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz: 

- [Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız 
- [Azure CLI 2.0](#View-existing-NSGs) -bizim gelecek nesil CLI kaynak yönetimi dağıtım modeli (Bu makalede)


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a>Önkoşul
Henüz henüz yükleyin ve en son yapılandırırsanız [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/#login). 


## <a name="view-existing-nsgs"></a>Varolan Nsg'ler görüntüleyin
Belirli bir kaynak grubunda Nsg'ler listesini görüntülemek için Çalıştır [az ağ nsg listesi](/cli/azure/network/nsg#list) komutunu bir `-o table` çıktı biçimi:

```azurecli
az network nsg list -g RG-NSG -o table
```

Beklenen çıktı:

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a>Bir NSG için tüm kuralları listesinde
Adlı bir NSG kurallarını görüntülemek için **NSG ön uç**, çalışma [az ağ nsg Göster](/cli/azure/network/nsg#show) komutunu kullanarak bir [JMESPATH sorgu filtresi](/cli/azure/query-az-cli2) ve `-o table` çıktı biçimi:

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

Beklenen çıktı:

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs to all VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs to Internet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> Aynı zamanda [az ağ nsg kural listesi](/cli/azure/network/nsg/rule#list) bir NSG'yi yalnızca özel kurallar listelemek için.
>

## <a name="view-nsg-associations"></a>Görünüm NSG ilişkilendirmeleri

Hangi kaynakları görüntülemek için **NSG ön uç** NSG olan ilişkilendirmek, çalışma `az network nsg show` komutunu aşağıda gösterildiği gibi. 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

Ara **networkInterfaces** ve **alt ağlar** aşağıda gösterildiği gibi özellikleri:

```json
[
  [
    {
      "addressPrefix": null,
      "etag": null,
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
      "ipConfigurations": null,
      "name": null,
      "networkSecurityGroup": null,
      "provisioningState": null,
      "resourceGroup": "RG-NSG",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  null
]
```

Yukarıdaki örnekte, NSG herhangi ağ arabirimlerine (NIC'ler) ilişkili değil ve adlı bir alt ağ ile ilişkilendirilene **ön uç**.

## <a name="add-a-rule"></a>Kural ekleme
İzin verme kuralı eklemek için **gelen** bağlantı noktası trafiği **443** için herhangi bir makineden **NSG ön uç** NSG, aşağıdaki komutu girin:

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access to port 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

Beklenen çıktı:

```json
{
  "access": "Allow",
  "description": "Allow access to port 443 for HTTPS",
  "destinationAddressPrefix": "*",
  "destinationPortRange": "443",
  "direction": "Inbound",
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
  "name": "allow-https",
  "priority": 102,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

## <a name="change-a-rule"></a>Bir kural değiştirme
Öğesinden gelen trafiğe izin vermek için yukarıda oluşturduğunuz kural değiştirmek için **Internet** yalnızca çalıştırmak [az ağ nsg kural güncelleştirmesi](/cli/azure/network/nsg/rule#update) komutu:

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

Beklenen çıktı:

```json
{
"access": "Allow",
"description": "Allow access to port 443 for HTTPS",
"destinationAddressPrefix": "*",
"destinationPortRange": "443",
"direction": "Inbound",
"etag": "W/\"<guid>\"",
"id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
"name": "allow-https",
"priority": 102,
"protocol": "Tcp",
"provisioningState": "Succeeded",
"resourceGroup": "RG-NSG",
"sourceAddressPrefix": "Internet",
"sourcePortRange": "*"
}
```

## <a name="delete-a-rule"></a>Kural silme
Yukarıda oluşturduğunuz kural silmek için aşağıdaki komutu çalıştırın:

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-to-a-nic"></a>Bir NSG'yi bir NIC ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **TestNICWeb1** NIC, kullanım [az ağ NIC güncelleştirmesi](/cli/azure/network/nic#update) komutu:

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

Beklenen çıktı:

```json
{
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": [],
    "internalDnsNameLabel": null,
    "internalDomainNameSuffix": "k0wkaguidnqrh0ud.gx.internal.cloudapp.net",
    "internalFqdn": null
  },
  "enableAcceleratedNetworking": false,
  "enableIpForwarding": false,
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1",
  "ipConfigurations": [
    {
      "applicationGatewayBackendAddressPools": null,
      "etag": "W/\"<guid>\"",
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1",
      "loadBalancerBackendAddressPools": null,
      "loadBalancerInboundNatRules": null,
      "name": "ipconfig1",
      "primary": true,
      "privateIpAddress": "192.168.1.6",
      "privateIpAddressVersion": "IPv4",
      "privateIpAllocationMethod": "Static",
      "provisioningState": "Succeeded",
      "publicIpAddress": null,
      "resourceGroup": "RG-NSG",
      "subnet": {
        "addressPrefix": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
        "ipConfigurations": null,
        "name": null,
        "networkSecurityGroup": null,
        "provisioningState": null,
        "resourceGroup": "RG-NSG",
        "resourceNavigationLinks": null,
        "routeTable": null
      }
    }
  ],
  "location": "centralus",
  "macAddress": "00-0D-3A-91-A9-60",
  "name": "TestNICWeb1",
  "networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "primary": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "resourceGuid": "<guid>",
  "tags": {},
  "type": "Microsoft.Network/networkInterfaces",
  "virtualMachine": null
}
```

## <a name="dissociate-an-nsg-from-a-nic"></a>Bir NSG'yi bir NIC gelen ilişkilendirmesini Kaldır

İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **TestNICWeb1** çalıştırmak NIC [az ağ nsg kural güncelleştirmesi](/cli/azure/network/nsg/rule#update) komutunu yeniden ancak yerini `--network-security-group` boş bir dize değişkeni (`""`).

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

Çıktıda `networkSecurityGroup` anahtarı null.

## <a name="dissociate-an-nsg-from-a-subnet"></a>Bir NSG'yi bir alt ağdan ilişkilendirmesini Kaldır
İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **ön uç** alt yeniden çalıştırın, [az ağ nsg kural güncelleştirmesi](/cli/azure/network/nsg/rule#update) komutunu yeniden ancak yerini `--network-security-group` bağımsız değişkeni boş bir dize ile (`""`).

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

Çıktıda `networkSecurityGroup` anahtarı null.

## <a name="associate-an-nsg-to-a-subnet"></a>Bir NSG'yi bir alt ağ ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **ön uç** alt yeniden, aşağıdaki komutu çalıştırın:

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

Çıktıda `networkSecurityGroup` anahtarı için değer benzer bir şey yok:

```json
"networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  }
  ```

## <a name="delete-an-nsg"></a>Bir NSG'yi Sil
Herhangi bir kaynağa ilişkili olmayan bir NSG'yi yalnızca silebilirsiniz. Bir NSG'yi silmek için aşağıdaki adımları izleyin.

1. Bir NSG'yi ilişkili tüm kaynakları denetlemek için çalıştırın `azure network nsg show` gösterildiği gibi [görünüm Nsg'ler ilişkilendirmeleri](#View-NSGs-associations).
2. NSG herhangi NIC'ler ilişkiliyse çalıştırmak `azure network nic set` gösterildiği gibi [bir NSG'yi bir NIC gelen ilişkilendirmesini](#Dissociate-an-NSG-from-a-NIC) her NIC için 
3. NSG herhangi bir alt ağ ile ilişkili ise, çalıştırın `azure network vnet subnet set` gösterildiği gibi [bir NSG bir alt ağdan ilişkilendirmesini](#Dissociate-an-NSG-from-a-subnet) her alt ağ için.
4. NSG silmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a>Sonraki adımlar
* [Günlük kaydını etkinleştir](virtual-network-nsg-manage-log.md) Nsg'ler için.

