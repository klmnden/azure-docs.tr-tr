---
title: Azure sanal ağında - Kaynak Yöneticisi şablonu (Önizleme) IPv6 ikili yığını uygulama dağıtma
titlesuffix: Azure Virtual Network
description: Bu makalede nasıl dağıtılacağı bir Azure Resource Manager VM şablonları kullanarak Azure sanal ağına IPv6 ikili yığını uygulamada gösterir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: ae90bc4a12763803f38224d917c4644a68ae7d6b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62131036"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---template-preview"></a>Azure - şablonu (Önizleme) IPv6 ikili yığını uygulama dağıtma

Bu makalede, Azure Resource Manager VM şablonun uygulandığı kısmı ile IPv6 yapılandırma görevleri bir listesini sağlar. Bu makalede açıklanan şablonu bir ikili yığın (IPv4 + IPv6) uygulaması içeren bir ikili yığın sanal ağ (IPv4 + IPv6) çift ön uç yapılandırmaları olan bir çift IP NIC'leri olan Vm'leri bir yük dengeleyici alt ağlar, IPv4 ve IPv6 ile azure'da dağıtım yapma yapılandırma, ağ güvenlik grubu ve genel IP'ler. 

## <a name="required-configurations"></a>Gerekli yapılandırmalar

Şablon bölümleri nerede yapılacağını görmek için şablonu arayın.

### <a name="ipv6-addressspace-for-the-virtual-network"></a>Sanal ağ için IPv6 addressspace değeri

Şablon Bölümü'eklemek için:

```JSON
        "addressSpace": {
          "addressPrefixes": [
            "[variables('vnetv4AddressRange')]",
            "[variables('vnetv6AddressRange')]"    
```

### <a name="ipv6-subnet-within-the-ipv6-virtual-network-addressspace"></a>IPv6 sanal ağ addressSpace içindeki IPv6 alt ağ

Şablon Bölümü'eklemek için:
```JSON
          {
            "name": "V6Subnet",
            "properties": {
              "addressPrefix": "[variables('subnetv6AddressRange')]"
            }

```

### <a name="ipv6-configuration-for-the-nic"></a>IPv6 Yapılandırması için NIC

Şablon Bölümü'eklemek için:
```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

### <a name="ipv6-network-security-group-nsg-rules"></a>IPv6 ağ güvenlik grubu (NSG) kuralları

```JSON
          {
            "name": "default-allow-rdp",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "33819-33829",
              "destinationPortRange": "5000-6000",
              "sourceAddressPrefix": "ace:cab:deca:deed::/64",
              "destinationAddressPrefix": "cab:ace:deca:deed::/64",
              "access": "Allow",
              "priority": 1003,
              "direction": "Inbound"
            }
```

## <a name="conditional-configuration"></a>Koşullu yapılandırma

Bir ağ sanal Gereci kullanıyorsanız, rota tablosunda IPv6 yolları ekleyin. Aksi takdirde, bu yapılandırma isteğe bağlıdır.

```JSON
    {
      "type": "Microsoft.Network/routeTables",
      "name": "v6route",
      "apiVersion": "[variables('ApiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "routes": [
          {
            "name": "v6route",
            "properties": {
              "addressPrefix": "ace:cab:deca:deed::/64",
              "nextHopType": "VirtualAppliance",
              "nextHopIpAddress": "deca:cab:ace:f00d::1"
            }
```

## <a name="optional-configuration"></a>İsteğe bağlı yapılandırma

### <a name="ipv6-internet-access-for-the-virtual-network"></a>Sanal ağ için IPv6 Internet erişimi

```JSON
{
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-public-ip-addresses"></a>IPv6 genel IP adresleri

```JSON
    {
      "apiVersion": "[variables('ApiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "lbpublicip-v6",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "publicIPAddressVersion": "IPv6"
      }
```

### <a name="ipv6-front-end-for-load-balancer"></a>Load Balancer için IPv6 ön uç

```JSON
          {
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-back-end-address-pool-for-load-balancer"></a>Load Balancer için IPv6 arka uç adres havuzu

```JSON
              "backendAddressPool": {
                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', 'loadBalancer'), '/backendAddressPools/LBBAP-v6')]"
              },
              "protocol": "Tcp",
              "frontendPort": 8080,
              "backendPort": 8080
            },
            "name": "lbrule-v6"
```

### <a name="ipv6-load-balancer-rules-to-associate-incoming-and-outgoing-ports"></a>IPv6 yük dengeleyici kuralları, gelen ve giden bağlantı noktaları ilişkilendirmek için

```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

## <a name="sample-vm-template-json"></a>VM şablonu JSON örneği
Tıklayın [burada](https://azure.microsoft.com/resources/templates/ipv6-in-vnet/) bir Azure Resource Manager şablonu kullanarak Azure sanal ağında IPv6 ikili yığını uygulamayı dağıtmak için.

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma hakkında ayrıntılı bilgi bulabilirsiniz [genel IP adresleri](https://azure.microsoft.com/pricing/details/ip-addresses/), [ağ bant genişliği](https://azure.microsoft.com/pricing/details/bandwidth/), veya [yük dengeleyici](https://azure.microsoft.com/pricing/details/load-balancer/).
