---
title: Öğretici - URL'ye göre web trafiğini yönlendirme -Azure CLI
description: Azure CLI kullanarak sunucuların belirli ölçeklenebilir havuzlarının URL'sine göre web trafiğini yönlendirmeyi öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 45057a23d54f42a4f5b165fc3eb50c001ce92e8d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069177"
---
# <a name="tutorial-route-web-traffic-based-on-the-url-using-the-azure-cli"></a>Öğretici: Azure CLI kullanarak URL'ye göre web trafiğini yönlendirme

Uygulamanıza erişmek için kullanılan URL’ye bağlı olarak belirli ölçeklenebilir sunucu havuzlarına yönlendirilecek web trafiğini yapılandırmak için Azure CLI’yı kullanabilirsiniz. Bu öğreticide [Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kullanan üç arka uç havuzuna sahip bir [Azure Application Gateway](application-gateway-introduction.md) oluşturursunuz. Arka uç havuzlarının her biri ortak veriler, görüntüler ve video gibi belirli bir amaca hizmet eder.  Trafiği farklı havuzlara ayırmak, müşterilerinizin ihtiyaç duydukları bilgileri ihtiyaç duydukları zaman edinmesini sağlar.

Trafik yönlendirmeyi etkinleştirmek için web trafiğinin havuzlardaki uygun sunuculara ulaşmasını sağlayacak belirli bağlantı noktalarını dinleyen dinleyicilere atanmış [yönlendirme kuralları](application-gateway-url-route-overview.md) oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Dinleyicileri, URL yol haritasını ve kuralları oluşturma
> * Ölçeklenebilir arka uç havuzları oluşturma


![URL yönlendirme örneği](./media/tutorial-url-route-cli/scenario.png)

Tercih ederseniz, bu öğreticiyi [Azure PowerShell](tutorial-url-route-powershell.md) kullanarak tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

[az network vnet create](/cli/azure/network/vnet#az_net) komutunu kullanarak *myVNet* adlı sanal ağı ve *myAGSubnet* adlı alt ağı oluşturun. Daha sonra [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) kullanan arka uç sunucularının gerek duyduğu *myBackendSubnet* adlı alt ağı ekleyin. [az network public-ip create](/cli/azure/public-ip#az_network_public_ip_create) komutunu kullanarak *myAGPublicIPAddress* adlı genel IP adresini oluşturun.

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

*myAppGateway* adlı bir uygulama ağ geçidi oluşturmak için [az network application-gateway create](/cli/azure/application-gateway#create) komutunu kullanın. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Uygulama ağ geçidi, *myAGSubnet*’e ve daha önce oluşturduğunuz *myAGPublicIPAddress*’e atanır. 

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

 Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. Uygulama ağ geçidi oluşturulduktan sonra şu yeni özellikleri görürsünüz:

- *appGatewayBackendPool* -bir uygulama ağ geçidi en az bir arka uç adres havuzuna sahip olmalıdır.
- *appGatewayBackendHttpSettings*: İletişim için 80 numaralı bağlantı noktasının ve HTTP protokolünün kullanıldığını belirtir.
- *appGatewayHttpListener*: *appGatewayBackendPool* ile ilişkili varsayılan dinleyicidir.
- *appGatewayFrontendIP*: *appGatewayHttpListener*’a *myAGPublicIPAddress*’i atar.
- *kural 1* - *appGatewayHttpListener* ile ilişkili varsayılan yönlendirme kuralıdır.


### <a name="add-image-and-video-backend-pools-and-port"></a>Görüntü ve video arka uç havuzlarını ve bağlantı noktasını ekleme

[az network application-gateway address-pool create](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create) kullanarak *imagesBackendPool* ve *videoBackendPool* adlı arka uç havuzlarını uygulama ağ geçidinize ekleyin. [az network application-gateway frontend-port create](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create) kullanarak havuzlara ön uç bağlantı noktasını ekleyebilirsiniz. 

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

### <a name="add-backend-listener"></a>Arka uç dinleyicisi ekleme

[az network application-gateway http-listener create](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create) kullanarak trafiği yönlendirmek için gereken *backendListener* adlı arka uç dinleyicisini ekleyin.


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>URL yol eşlemesini ekleme

URL yol eşlemeleri belirli URL'lerin belirli arka uç havuzlarına yönlendirilmesini sağlar. [az network application-gateway url-path-map create](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) ve [az network application-gateway url-path-map rule create](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create) kullanarak *imagePathRule* ve *videoPathRule* adlı URL yol eşlemelerini oluşturun

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

### <a name="add-routing-rule"></a>Yönlendirme kuralı ekleme

Yönlendirme kuralı URL eşlemelerini oluşturduğunuz dinleyici ile ilişkilendirir. [az network application-gateway rule create](/cli/azure/application-gateway#az_network_application_gateway_rule_create) kullanarak *rule2* adlı bir kural ekleyin.

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

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte, oluşturduğunuz üç arka uç havuzunu destekleyen üç sanal makine ölçek kümesi oluşturursunuz. Oluşturduğunuz ölçek kümeleri *myvmss1*, *myvmss2* ve *myvmss3* olarak adlandırılır. Her bir ölçek kümesi NGINX yükleyeceğiniz iki sanal makine örneği içerir.

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

```azurecli-interactive
for i in `seq 1 3`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) komutunu kullanın. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm* veya *http://40.121.222.19:8080/video/test.htm*.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Temel URL’yi uygulama ağ geçidinde test etme](./media/tutorial-url-route-cli/application-gateway-nginx.png)

URL’yi http://&lt;ip-address&gt;:8080/images/test.html olarak değiştirin. Burada &lt;ip-address&gt; değeri olarak kendi IP adresinizi kullanın. Aşağıdaki örneğe benzer bir sonuç olacaktır:

![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/tutorial-url-route-cli/application-gateway-nginx-images.png)

URL’yi http://&lt;ip-adresi&gt;:8080/video/test.html olarak değiştirin. Burada &lt;ip-adresi&gt; değeri olarak kendi IP adresinizi kullanın. Aşağıdaki örneğe benzer bir sonuç olacaktır.

![Video URL’sini uygulama ağ geçidinde test etme](./media/tutorial-url-route-cli/application-gateway-nginx-video.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, uygulama ağ geçidini ve tüm ilgili kaynakları silin.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Dinleyicileri, URL yol haritasını ve kuralları oluşturma
> * Ölçeklenebilir arka uç havuzları oluşturma

> [!div class="nextstepaction"]
> [URL yolu tabanlı yönlendirme ile bir uygulama ağ geçidi oluşturma](./tutorial-url-redirect-cli.md)
