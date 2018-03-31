---
title: 'Rota tabanlı Azure VPN ağ geçidi oluşturmak: CLI | Microsoft Docs'
description: Hızlı bir şekilde CLI kullanarak bir VPN Gateway oluşturma hakkında bilgi edinin
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/28/2018
ms.author: cherylmc
ms.openlocfilehash: b25efb600fc89b5a6ead6ec27e212d09c9a14435
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="create-a-route-based-vpn-gateway-using-cli"></a>CLI kullanarak bir rota tabanlı VPN ağ geçidi oluşturma

Bu makalede, hızlı bir şekilde Azure CLI kullanarak bir rota tabanlı Azure VPN ağ geçidi oluşturmanıza yardımcı olur. Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Sanal ağlara bağlanmak için bir VPN ağ geçidi'ni de kullanabilirsiniz.

Bu makaledeki adımları bir sanal ağ, bir alt ağ, bir ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Bir sanal ağ geçidi 45 dakika veya oluşturmak için daha fazla sürebilir. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantı oluşturabilirsiniz. Bu adımları bir Azure aboneliği gerektirir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kullanarak bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#az_group_create) komutu. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 


```azurecli-interactive 
az group create --name TestRG1 --location eastus
```

## <a name="vnet"></a>Bir sanal ağ oluşturma

Kullanarak bir sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create) komutu. Aşağıdaki örnek adlı bir sanal ağ oluşturur **VNet1** içinde **EastUS** konumu:

```azurecli-interactive 
az network vnet create \
  -n VNet1 \
  -g TestRG1 \
  -l eastus \
  --address-prefix 10.1.0.0/16 \
  --subnet-name Frontend \
  --subnet-prefix 10.1.0.0/24
```

## <a name="gwsubnet"></a>Bir ağ geçidi alt ağı Ekle

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerini kullanmak ayrılmış IP adresleri içerir. Aşağıdaki örnekler, bir ağ geçidi alt ağı eklemek için kullanın:

```azurepowershell-interactive
az network vnet subnet create \
  --vnet-name VNet1 \
  -n GatewaySubnet \
  -g TestRG1 \
  --address-prefix 10.1.255.0/27 
```

## <a name="PublicIP"></a>İstek bir ortak IP adresi

Bir VPN ağ geçidi dinamik olarak ayrılan bir ortak IP adresi olmalıdır. Sanal ağınız için oluşturduğunuz VPN ağ geçidi için genel IP adresi ayrılır. Bir ortak IP adresi istemek için aşağıdaki örneği kullanın:

```azurecli-interactive
az network public-ip create \
  -n VNet1GWPIP \
  -g TestRG1 \
  --allocation-method Dynamic 
```

## <a name="CreateGateway"></a>VPN ağ geçidi oluşturma

[az network vnet-gateway create](/cli/azure/group#az_network_vnet_gateway_create) komutunu kullanarak VPN ağ geçidini oluşturun.

Bu komutu kullanarak çalıştırırsanız `--no-wait` parametresi, herhangi bir geri bildirim veya çıkış görmüyorum. `--no-wait` Parametresi arka planda oluşturulması için ağ geçidine izin verir. VPN ağ geçidini hemen oluşturduğunuz anlamına gelmez.

```azurecli-interactive
az network vnet-gateway create \
  -n VNet1GW \
  -l eastus \
  --public-ip-address VNet1GWPIP \
  -g TestRG1 \
  --vnet VNet1 \
  --gateway-type Vpn \
  --sku VpnGw1 \
  --vpn-type RouteBased \
  --no-wait
```

Bir VPN ağ geçidi 45 dakika veya oluşturmak için daha fazla sürebilir.

## <a name="viewgw"></a>VPN ağ geçidi görüntüleyin

```azurecli-interactive
az network vnet-gateway show \
  -n VNet1GW \
  -g TestRG1
```

Yanıtı şuna benzer:

```
{
  "activeActive": false,
  "bgpSettings": null,
  "enableBgp": false,
  "etag": "W/\"6c61f8cb-d90f-4796-8697\"",
  "gatewayDefaultSite": null,
  "gatewayType": "Vpn",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW",
  "ipConfigurations": [
    {
      "etag": "W/\"6c61f8cb-d90f-4796-8697\"",
      "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
      "name": "vnetGatewayConfig0",
      "privateIpAllocationMethod": "Dynamic",
      "provisioningState": "Updating",
      "publicIpAddress": {
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/publicIPAddresses/VNet1GWPIP",
        "resourceGroup": "TestRG1"
      },
      "resourceGroup": "TestRG1",
      "subnet": {
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet",
        "resourceGroup": "TestRG1"
      }
    }
  ],
  "location": "eastus",
  "name": "VNet1GW",
  "provisioningState": "Updating",
  "resourceGroup": "TestRG1",
  "resourceGuid": "69c269e3-622c-4123-9231",
  "sku": {
    "capacity": 2,
    "name": "VpnGw1",
    "tier": "VpnGw1"
  },
  "tags": null,
  "type": "Microsoft.Network/virtualNetworkGateways",
  "vpnClientConfiguration": null,
  "vpnType": "RouteBased"
}
```

### <a name="view-the-public-ip-address"></a>Genel IP adresi görüntüleyin

Ağ geçidiniz için atanan ortak IP adresi görüntülemek için aşağıdaki örneği kullanın:

```azurecli-interactive
az network public-ip show \
  --name VNet1GWPIP \
  --resource-group TestRG11
```

İle ilişkili değer **IPADDRESS** VPN ağ geçidinizin genel IP adresini bir alandır.

Örnek yanıt:

```
{
  "dnsSettings": null,
  "etag": "W/\"a12d4d03-b27a-46cc-b222-8d9364b8166a\"",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWPIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "13.90.195.184",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
```
## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynakları artık gerektiğinde kullanın [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubunu silmek için. Bu kaynak grubu ve içerdiği tüm kaynakları siler.

```azurecli-interactive 
az group delete --name TestRG1 --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturmayı tamamlandıktan sonra sanal ağınız ve başka bir VNet arasında bir bağlantı oluşturabilirsiniz. Veya, sanal ağ ve bir şirket içi konum arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Bir noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Başka bir VNet bağlantı oluşturun.](vpn-gateway-vnet-vnet-rm-ps.md)