---
title: URL yolu tabanlı yönlendirme kurallarıyla - Azure CLI bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: URL yolu tabanlı yönlendirme kuralları için Azure CLI kullanarak bir uygulama ağ geçidi ve sanal makine ölçek kümesi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.openlocfilehash: 061156a455664a5a3f0b4c4497d24f4e8ff6eea7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66135711"
---
# <a name="create-an-application-gateway-with-url-path-based-routing-rules-using-the-azure-cli"></a>Azure CLI kullanarak URL yolu tabanlı yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturma

Azure CLI'yi kullanarak, bir [uygulama ağ geçidi](application-gateway-introduction.md) oluştururken [URL yolu tabanlı yönlendirme kuralları](application-gateway-url-route-overview.md) yapılandırabilirsiniz. Bu öğreticide oluşturduğunuz arka uç havuzları kullanan bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Daha sonra uygun sunucularla havuzlarındaki web trafiğini ulaştığında emin yönlendirme kuralları oluşturursunuz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * URL eşlemesi ile bir uygulama ağ geçidi oluşturma
> * Arka uç havuzları ile sanal makine ölçek kümeleri oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-cli/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

[az network vnet create](/cli/azure/network/vnet) komutunu kullanarak *myVNet* adlı sanal ağı ve *myAGSubnet* adlı alt ağı oluşturun. Daha sonra [az network vnet subnet create](/cli/azure/network/vnet/subnet) kullanan arka uç sunucularının gerek duyduğu *myBackendSubnet* adlı alt ağı ekleyebilirsiniz. [az network public-ip create](/cli/azure/network/public-ip) komutunu kullanarak *myAGPublicIPAddress* adlı genel IP adresini oluşturun.

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

*myAppGateway* adlı uygulama ağ geçidini oluşturmak için [az network application-gateway create](/cli/azure/network/application-gateway) komutunu kullanabilirsiniz. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Uygulama ağ geçidi, *myAGSubnet*’e ve daha önce oluşturduğunuz *myAGPublicIPAddress*’e atanır. 

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

Adlı arka uç havuzları ekleyebilirsiniz *imagesBackendPool* ve *videoBackendPool* kullanarak uygulama ağ geçidi için [az ağ uygulama ağ geçidi adres havuzu oluşturma](/cli/azure/network/application-gateway/address-pool#az-network-application-gateway-address-pool-create). [az network application-gateway frontend-port create](/cli/azure/network/application-gateway/frontend-port#az-network-application-gateway-frontend-port-create) kullanarak havuzlara ön uç bağlantı noktasını ekleyebilirsiniz. 

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

[az network application-gateway http-listener create](/cli/azure/network/application-gateway) kullanarak trafiği yönlendirmek için gereken *backendListener* adlı arka uç dinleyicisini ekleyin.


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>URL yol eşlemesini ekleme

URL yol eşlemeleri belirli URL'lerin belirli arka uç havuzlarına yönlendirilmesini sağlar. [az network application-gateway url-path-map create](/cli/azure/network/application-gateway) ve [az network application-gateway url-path-map rule create](/cli/azure/network/application-gateway) kullanarak *imagePathRule* ve *videoPathRule* adlı URL yol eşlemelerini oluşturabilirsiniz.

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

Yönlendirme kuralı URL eşlemelerini oluşturduğunuz dinleyici ile ilişkilendirir. Adlı bir kural ekleyebileceğiniz *bağlanma2* kullanarak [az ağ uygulama ağ geçidi kuralı oluşturma](/cli/azure/network/application-gateway/rule#az-network-application-gateway-rule-create).

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

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerini oluşturma

Bu örnekte, oluşturduğunuz üç arka uç havuzunu destekleyen üç sanal makine ölçek kümesi oluşturacaksınız. Oluşturduğunuz ölçek kümeleri *myvmss1*, *myvmss2* ve *myvmss3* olarak adlandırılır. Her bir ölçek kümesi NGINX yükleyeceğiniz iki sanal makine örneği içerir.

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

Uygulama ağ geçidinin genel IP adresini almak için [az network public-ip show](/cli/azure/network/public-ip) komutunu kullanın. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Gibi `http://40.121.222.19`, `http://40.121.222.19:8080/images/test.htm`, veya `http://40.121.222.19:8080/video/test.htm`.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Temel URL’yi uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-cli/application-gateway-nginx.png)

URL'yi `http://<ip-address>:8080/video/test.html` sonuna ve temel URL aşağıdaki örneğe benzer bir şey görmeniz gerekir:

![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-cli/application-gateway-nginx-images.png)

URL'yi `http://<ip-address>:8080/video/test.html` ve aşağıdaki örnekte olduğu gibi bir şey görmeniz gerekir.

![Video URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-cli/application-gateway-nginx-video.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * URL eşlemesi ile bir uygulama ağ geçidi oluşturma
> * Arka uç havuzları ile sanal makine ölçek kümeleri oluşturma

Uygulama ağ geçitleri ve bunların ilişkili kaynakları hakkında daha fazla bilgi edinmek için nasıl yapılır makaleleriyle devam edin.
