---
title: Bir uygulama ağ geçidi sertifikası - Azure CLI oluşturma | Microsoft Docs
description: Bir uygulama ağ geçidi oluşturmak ve Azure CLI kullanarak SSL sonlandırma için bir sertifika eklemek öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: victorh
ms.openlocfilehash: 645ccad03997256a050b48aab7104e9fe9ae7ef3
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-the-azure-cli"></a>Bir uygulama ağ geçidi HTTP ile Azure CLI kullanarak HTTPS yeniden yönlendirmesi için oluşturma

Azure CLI oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](application-gateway-introduction.md) SSL sonlandırma için bir sertifika ile. Yönlendirme kuralı HTTPS bağlantı noktası uygulama ağ geçidiniz için HTTP trafiği yönlendirmek için kullanılır. Bu örnekte, ayrıca oluşturduğunuz bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) iki sanal makine örneklerini içeren uygulama ağ geçidi arka uç havuzu için.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ kurma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı Ekle
> * Bir sanal makineyi ölçeği varsayılan arka uç havuzuyla Ayarla oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Üretim kullanımı için güvenilen bir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir. Bu öğreticide, bir kendinden imzalı bir sertifika ve pfx dosyası openssl komutunu kullanarak oluşturun.

```azurecli-interactive
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out appgwcert.crt
```

Sertifikanızı için anlamlı değerleri girin. Varsayılan değerleri kabul edebilir.

```azurecli-interactive
openssl pkcs12 -export -out appgwcert.pfx -inkey privateKey.key -in appgwcert.crt
```

Sertifika için parola girin. Bu örnekte, *Azure123456!* kullanılıyor.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kullanarak bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#create).

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAG* içinde *eastus* konumu.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

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

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Kullanabileceğiniz [az ağ uygulama-ağ geçidi oluşturma](/cli/azure/network/application-gateway#az_network_application_gateway_create) adlı uygulama ağ geçidi oluşturmak için *myAppGateway*. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtin. 

Uygulama ağ geçidi atandığı *myAGSubnet* ve *myAGPublicIPAddress* daha önce oluşturduğunuz. Uygulama ağ geçidi oluşturduğunuzda, bu örnekte, oluşturduğunuz sertifikayı ve parolasını ilişkilendirin. 

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
  --frontend-port 443 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress \
  --cert-file appgwcert.pfx \
  --cert-password "Azure123456!"

```

 Oluşturulacak uygulama ağ geçidi için birkaç dakika sürebilir. Uygulama ağ geçidi oluşturulduktan sonra bu yeni özellikleri bunu görebilirsiniz:

- *appGatewayBackendPool* -bir uygulama ağ geçidi en az bir arka uç adres havuzu olmalıdır.
- *appGatewayBackendHttpSettings* -80 numaralı bağlantı noktasını ve HTTP protokolü kullanılır, iletişim için belirtir.
- *appGatewayHttpListener* -ilişkili varsayılan dinleyici *appGatewayBackendPool*.
- *appGatewayFrontendIP* -atar *myAGPublicIPAddress* için *appGatewayHttpListener*.
- *Kuralı 1* - ilişkilendirilen kural yönlendirme varsayılan *appGatewayHttpListener*.

## <a name="add-a-listener-and-redirection-rule"></a>Dinleyici ve yeniden yönlendirme kuralı Ekle

### <a name="add-the-http-port"></a>HTTP bağlantı noktası ekleme

Kullanabileceğiniz [az ağ uygulama ağ geçidi ön uç-port oluşturmak](/cli/azure/network/application-gateway/frontend-port#az_network_application_gateway_frontend_port_create) HTTP bağlantı noktası uygulama ağ geçidi eklemek için.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name httpPort
```

### <a name="add-the-http-listener"></a>HTTP dinleyicisi ekleme

Kullanabileceğiniz [az ağ uygulama ağ geçidi http dinleyicisi oluşturmak](/cli/azure/network/application-gateway/http-listener#az_network_application_gateway_http_listener_create) adlı dinleyicisi eklemek için *myListener* uygulama ağ geçidi için.

```azurecli-interactive
az network application-gateway http-listener create \
  --name myListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port httpPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-redirection-configuration"></a>Yeniden yönlendirme yapılandırması Ekle

Uygulama ağ geçidi kullanarak HTTPS yeniden yönlendirme yapılandırması HTTP eklemek [az ağ uygulama ağ geçidi yeniden yönlendirme-config oluşturma](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name httpToHttps \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener appGatewayHttpListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-the-routing-rule"></a>Yönlendirme kuralı Ekle

Adlı yönlendirme kuralı Ekle *Kural 2* kullanarak uygulama ağ geçidi için yeniden yönlendirme yapılandırması ile [az ağ uygulama ağ geçidi kuralını](/cli/azure/network/application-gateway/rule#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener myListener \
  --rule-type Basic \
  --redirect-config httpToHttps
```

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte, oluşturduğunuz adlandırılmış kümesi bir sanal makine ölçek *myvmss* uygulama ağ geçidi arka uç havuzu için sunucuları sağlar. Ölçek kümesindeki sanal makineler ilişkili *myBackendSubnet* ve *appGatewayBackendPool*. Ölçek oluşturmak için kullanabileceğiniz [az vmss oluşturma](/cli/azure/vmss#az_vmss_create).

```azurecli-interactive
az vmss create \
  --name myvmss \
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
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>NGINX yükleme

Bu komut kabuğu penceresinde çalıştırın:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/vhorne/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Uygulama ağ geçidi genel IP adresi almak için kullanabileceğiniz [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show). Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Güvenli uyarı](./media/tutorial-http-redirect-cli/application-gateway-secure.png)

Kendinden imzalı bir sertifika kullanıyorsa uyarı güvenlik kabul etmeyi seçin **ayrıntıları** ve ardından **Web sayfasına gidin**. Güvenli NGINX siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Temel uygulama ağ geçidi URL'de test](./media/tutorial-http-redirect-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ kurma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı Ekle
> * Bir sanal makineyi ölçeği varsayılan arka uç havuzuyla Ayarla oluşturma

> [!div class="nextstepaction"]
> [Uygulama ağ geçidi ile neler yapabileceğiniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)
