---
title: Dış trafik yeniden yönlendirmesi ile - Azure CLI bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Azure CLI kullanarak uygun havuzu iç web trafiğini yönlendiren bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: a0132fa43f28997d80e391b7445eeb1809c19167
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-external-redirection-using-the-azure-cli"></a>Azure CLI kullanarak dış yeniden yönlendirme ile bir uygulama ağ geçidi oluşturma

Azure CLI yapılandırmak için kullanabileceğiniz [web trafiği yeniden yönlendirme](multiple-site-overview.md) oluştururken bir [uygulama ağ geçidi](overview.md). Bu öğreticide, bir dinleyici ve uygulama ağ geçidi harici bir siteye ulaştığında web trafiğini yönlendiren kuralı yapılandırın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Dinleyici ve yeniden yönlendirme kuralı oluşturma
> * Uygulama ağ geçidi oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kullanarak bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#create).

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAG* içinde *eastus* konumu.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

Adlı sanal ağ oluşturma *myVNet* ve adlı alt ağın *myAGSubnet* kullanarak [az ağ vnet oluşturma](/cli/azure/network/vnet#az_net). Adlı ortak IP adresi oluşturma *myAGPublicIPAddress* kullanarak [az ağ genel IP oluşturun](/cli/azure/public-ip#az_network_public_ip_create). Bu kaynaklar, uygulama ağ geçidi ve onun ilişkili kaynakları için ağ bağlantısı sağlamak için kullanılır.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Kullanabileceğiniz [az ağ uygulama-ağ geçidi oluşturma](/cli/azure/application-gateway#create) adlı uygulama ağ geçidi oluşturmak için *myAppGateway*. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtin. Uygulama ağ geçidi atandığı *myAGSubnet* ve *myPublicIPSddress* daha önce oluşturduğunuz. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 8080 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

Oluşturulacak uygulama ağ geçidi için birkaç dakika sürebilir. Uygulama ağ geçidi oluşturulduktan sonra bu yeni özellikleri bunu görebilirsiniz:

- *appGatewayBackendPool* -bir uygulama ağ geçidi en az bir arka uç adres havuzu olmalıdır.
- *appGatewayBackendHttpSettings* -80 numaralı bağlantı noktasını ve HTTP protokolü kullanılır, iletişim için belirtir.
- *appGatewayHttpListener* -ilişkili varsayılan dinleyici *appGatewayBackendPool*.
- *appGatewayFrontendIP* -atar *myAGPublicIPAddress* için *appGatewayHttpListener*.
- *Kuralı 1* - ilişkilendirilen kural yönlendirme varsayılan *appGatewayHttpListener*.

### <a name="add-the-redirection-configuration"></a>Yeniden yönlendirme yapılandırması Ekle

Gelen trafiği gönderir yeniden yönlendirme yapılandırması eklemek *www.consoto.org* için dinleyici için *www.contoso.com* uygulama ağ geçidi kullanarak [az ağ uygulama ağ geçidi yeniden yönlendirme yapılandırması oluşturma](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name myredirect \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Temporary \
  --target-url "http://bing.com"
```

### <a name="add-a-listener-and-routing-rule"></a>Dinleyici ve yönlendirme kuralı Ekle

Dinleyici uygun şekilde trafiği yönlendirmek uygulama ağ geçidi etkinleştirmek için gereklidir. Kullanarak bir dinleyici oluşturun [az ağ uygulama ağ geçidi http dinleyicisi oluşturmak](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create) ile oluşturulan ön uç bağlantı noktası ile [az ağ uygulama ağ geçidi ön uç-port oluşturmak](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). Bir kural gelen trafiği göndermek nereye bilmesi için dinleyici gereklidir. Adlı temel bir kural oluşturmak *redirectRule* kullanarak [az ağ uygulama ağ geçidi kuralını](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name redirectPort
az network application-gateway http-listener create \
  --name redirectListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port redirectPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectListener \
  --rule-type Basic \
  --redirect-config myredirect
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Uygulama ağ geçidi genel IP adresi almak için kullanabileceğiniz [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show). Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

Görmeniz gerekir *aratıp* tarayıcınızda görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> * Ağ kurma
> * Dinleyici ve yeniden yönlendirme kuralı oluşturma
> * Uygulama ağ geçidi oluşturma