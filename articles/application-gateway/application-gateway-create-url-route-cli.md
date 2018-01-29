---
title: "URL yolu tabanlı yönlendirme kuralları ile - Azure CLI bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "URL yolu tabanlı yönlendirme kuralları Azure CLI kullanarak bir uygulama ağ geçidi ve sanal makine ölçek oluşturmayı öğrenin."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: davidmu
ms.openlocfilehash: 73db1f05bacd3edd93394f346274727ca5735c87
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-application-gateway-with-url-path-based-routing-rules-using-the-azure-cli"></a>Azure CLI kullanarak URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma

Azure CLI yapılandırmak için kullanabileceğiniz [URL yolu tabanlı yönlendirme kuralları](application-gateway-url-route-overview.md) oluştururken bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide oluşturduğunuz kullanarak arka uç havuzları bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Uygun sunucuları havuzlarındaki web trafiği ulaştığında emin olun yönlendirme kuralları oluşturursunuz.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Ağ kurma
> * Bir uygulama ağ geçidi ile URL eşlemesi oluşturma
> * Sanal makine ölçek kümeleri ile arka uç havuzları oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-cli/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kullanarak bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#create).

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAG* içinde *eastus* konumu.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturun 

Adlı sanal ağ oluşturma *myVNet* ve adlı alt ağın *myAGSubnet* kullanarak [az ağ vnet oluşturma](/cli/azure/network/vnet#az_net). Daha sonra adlı alt ağ ekleyebilirsiniz *myBackendSubnet* kullanarak arka uç sunucuları tarafından gerekli [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Adlı ortak IP adresi oluşturma *myAGPublicIPAddress* kullanarak [az ağ genel IP oluşturun](/cli/azure/public-ip#az_network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-the-application-gateway-with-url-map"></a>URL eşlemesi ile uygulama ağ geçidi oluşturma

Kullanabileceğiniz [az ağ uygulama-ağ geçidi oluşturma](/cli/azure/application-gateway#create) adlı uygulama ağ geçidi oluşturmak için *myAppGateway*. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtin. Uygulama ağ geçidi atandığı *myAGSubnet* ve *myAGPublicIPAddress* daha önce oluşturduğunuz. 

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
  --frontend-port 80 \
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


### <a name="add-image-and-video-backend-pools-and-port"></a>Görüntü ve video arka uç havuzu ve bağlantı noktası ekleme

Adlı arka uç havuzları ekleyebilirsiniz *imagesBackendPool* ve *videoBackendPool* kullanarak uygulama ağ geçidi için [az ağ uygulama ağ geçidi adres havuzu oluşturma](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). Ön uç bağlantı noktası kullanarak havuzları ekleme [az ağ uygulama ağ geçidi ön uç-port oluşturmak](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name imagesBackendPool
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name videoBackendPool
az network application-gateway frontend-port create \
  --port 8080 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name port8080
```

### <a name="add-backend-listener"></a>Arka uç dinleyici ekleyin

Adlı arka uç dinleyicisi eklemek *backendListener* kullanarak trafiği yönlendirmek gerekli [az ağ uygulama ağ geçidi http dinleyicisi oluşturmak](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>URL yolu Eşlemesi Ekle

URL yolu eşlemeleri belirli URL'leri belirli arka uç havuzları yönlendirildiğinden emin olun. URL yolu eşlemeleri adlı oluşturabilirsiniz *imagePathRule* ve *videoPathRule* kullanarak [az ağ uygulama ağ geçidi url yolu-eşleme oluşturma](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) ve [az ağ Uygulama ağ geçidi url yolu eşleme kuralı oluşturma](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name myPathMap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --address-pool imagesBackendPool \
  --default-address-pool appGatewayBackendPool \
  --default-http-settings appGatewayBackendHttpSettings \
  --http-settings appGatewayBackendHttpSettings \
  --rule-name imagePathRule
az network application-gateway url-path-map rule create \
  --gateway-name myAppGateway \
  --name videoPathRule \
  --resource-group myResourceGroupAG \
  --path-map-name myPathMap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-routing-rule"></a>Yönlendirme kuralı Ekle

Yönlendirme kuralı URL eşlemeleri oluşturduğunuz dinleyicisi ile ilişkilendirir. Adlı kuralı ekleyebilirsiniz *Kural 2* kullanarak [az ağ uygulama ağ geçidi kuralını](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map myPathMap \
  --address-pool appGatewayBackendPool
```

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri oluşturma

Bu örnekte, oluşturduğunuz üç arka uç havuzu destekleyen üç sanal makine ölçek kümeleri oluşturun. Oluşturduğunuz ölçek kümeleri adlı *myvmss1*, *myvmss2*, ve *myvmss3*. Her bir ölçek kümesi NGINX yüklemek iki sanal makine örneklerini içerir.

```azurecli-interactive
for i in `seq 1 3`; do
  if [ $i -eq 1 ]
  then
    poolName="appGatewayBackendPool" 
  fi
  if [ $i -eq 2 ]
  then
    poolName="imagesBackendPool"
  fi
  if [ $i -eq 3 ]
  then
    poolName="videoBackendPool"
  fi
  az vmss create \
    --name myvmss$i \
    --resource-group myResourceGroupAG \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Azure123456! \
    --instance-count 2 \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --vm-sku Standard_DS2 \
    --upgrade-policy-mode Automatic \
    --app-gateway myAppGateway \
    --backend-pool-name $poolName
done
```

### <a name="install-nginx"></a>NGINX yükleme

Geçerli kabuğunuzu customConfig.json adlı bir dosya oluşturun ve aşağıdaki yapılandırma yapıştırın. Bulut Kabuğu'nda dosya oluşturmak istediğiniz herhangi bir düzenleyicisini kullanabilirsiniz.  Girin `sensible-editor cloudConfig.json` dosyası oluşturmak için kullanılabilir düzenleyicileri listesini görmek için.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh"
}
```

Bu komut kabuğu penceresinde çalıştırın:

```azurecli-interactive
for i in `seq 1 3`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings @cloudConfig.json
done
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Uygulama ağ geçidi genel IP adresi almak için kullanabileceğiniz [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show). Genel IP adresini kopyalayın ve ardından, tarayıcınızın adres çubuğuna yapıştırın. Gibi *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm*, veya *http://40.121.222.19:8080/video/test.htm*.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Temel uygulama ağ geçidi URL'de test](./media/application-gateway-create-url-route-cli/application-gateway-nginx.png)

Temel URL sonuna http://<ip-address>:8080/video/test.html için URL'yi değiştirin ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

![Uygulama ağ geçidi görüntüleri URL Sına](./media/application-gateway-create-url-route-cli/application-gateway-nginx-images.png)

URL için http://<ip-address>:8080/video/test.html değiştirin ve aşağıdaki örneğe benzer bir şey görmeniz gerekir.

![Uygulama ağ geçidi olarak test video URL'si](./media/application-gateway-create-url-route-cli/application-gateway-nginx-video.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Bir uygulama ağ geçidi ile URL eşlemesi oluşturma
> * Sanal makine ölçek kümeleri ile arka uç havuzları oluşturma

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.
