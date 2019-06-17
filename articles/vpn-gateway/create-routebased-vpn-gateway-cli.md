---
title: 'Rota temelli bir Azure VPN gateway oluşturun: CLI | Microsoft Docs'
description: CLI kullanarak bir VPN ağ geçidi oluşturmak nasıl hızlı bir şekilde öğrenin
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/04/2018
ms.author: cherylmc
ms.openlocfilehash: f5f62a6bfa1baa205e0496dd901f1f1eef660079
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60391251"
---
# <a name="create-a-route-based-vpn-gateway-using-cli"></a>CLI kullanarak rota temelli VPN ağ geçidi oluşturma

Bu makalede, Azure CLI kullanarak rota temelli Azure VPN ağ geçidi hızlı bir şekilde oluşturmanıza yardımcı olur. Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Vnet'leri bağlamak için bir VPN ağ geçidi'ni de kullanabilirsiniz.

Bu makaledeki adımlarda, sanal ağ, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Bir sanal ağ geçidi, 45 dakika veya oluşturmak için daha fazla sürebilir. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantıları oluşturabilirsiniz. Bu adımlar, bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kullanarak bir kaynak grubu oluşturmanız [az grubu oluşturma](/cli/azure/group) komutu. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 


```azurecli-interactive 
az group create --name TestRG1 --location eastus
```

## <a name="vnet"></a>Sanal ağ oluşturma

Kullanarak bir sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet) komutu. Aşağıdaki örnekte adlı bir sanal ağ oluşturur **VNet1** içinde **EastUS** konumu:

```azurecli-interactive 
az network vnet create \
  -n VNet1 \
  -g TestRG1 \
  -l eastus \
  --address-prefix 10.1.0.0/16 \
  --subnet-name Frontend \
  --subnet-prefix 10.1.0.0/24
```

## <a name="gwsubnet"></a>Bir ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Bir ağ geçidi alt ağı eklemek için aşağıdaki örnekleri kullanın:

```azurepowershell-interactive
az network vnet subnet create \
  --vnet-name VNet1 \
  -n GatewaySubnet \
  -g TestRG1 \
  --address-prefix 10.1.255.0/27 
```

## <a name="PublicIP"></a>Genel bir IP adresi isteme

Bir VPN ağ geçidi, dinamik olarak ayrılan bir genel IP adresi olmalıdır. Sanal ağınız için oluşturduğunuz VPN ağ geçidi genel IP adresi ayrılır. Genel bir IP adresi istemek için aşağıdaki örneği kullanın:

```azurecli-interactive
az network public-ip create \
  -n VNet1GWIP \
  -g TestRG1 \
  --allocation-method Dynamic 
```

## <a name="CreateGateway"></a>VPN ağ geçidi oluşturma

[az network vnet-gateway create](/cli/azure/group) komutunu kullanarak VPN ağ geçidini oluşturun.

Bu komutu kullanarak çalıştırırsanız `--no-wait` parametresi, tüm geri bildirim veya çıktı görmezsiniz. `--no-wait` Parametresi, ağ geçidinin arka planda oluşturulmasına olanak sağlar. VPN ağ geçidini hemen oluşturduğunuz anlamına gelmez.

```azurecli-interactive
az network vnet-gateway create \
  -n VNet1GW \
  -l eastus \
  --public-ip-address VNet1GWIP \
  -g TestRG1 \
  --vnet VNet1 \
  --gateway-type Vpn \
  --sku VpnGw1 \
  --vpn-type RouteBased \
  --no-wait
```

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir.

## <a name="viewgw"></a>VPN ağ geçidi görüntüleyin

```azurecli-interactive
az network vnet-gateway show \
  -n VNet1GW \
  -g TestRG1
```

Yanıt şuna benzer:

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
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP",
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

### <a name="view-the-public-ip-address"></a>Genel IP adresini görüntüleyin

Ağ geçidine atanan genel IP adresini görüntülemek için aşağıdaki örneği kullanın:

```azurecli-interactive
az network public-ip show \
  --name VNet1GWIP \
  --resource-group TestRG11
```

İlişkili değer **IPADDRESS** VPN ağ geçidinizin genel IP adresini bir alandır.

Örnek yanıt:

```
{
  "dnsSettings": null,
  "etag": "W/\"a12d4d03-b27a-46cc-b222-8d9364b8166a\"",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "13.90.195.184",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
```
## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda kullanın [az grubu Sil](/cli/azure/group) kaynak grubu silinemedi. Böylece, kaynak grubu ve içerdiği tüm kaynaklar silinir.

```azurecli-interactive 
az group delete --name TestRG1 --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturma tamamlandıktan sonra başka bir sanal ağ ve sanal ağınız arasında bir bağlantı oluşturabilirsiniz. Veya sanal ağınız ile şirket içi konumunuz arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Başka bir sanal ağa bağlantı oluşturma](vpn-gateway-vnet-vnet-rm-ps.md)
