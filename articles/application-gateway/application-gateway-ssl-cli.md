---
title: SSL sonlandırma - Azure CLI ile bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Bir uygulama ağ geçidi oluşturmak ve Azure CLI kullanarak SSL sonlandırma için bir sertifika eklemek öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/18/2018
ms.author: victorh
ms.openlocfilehash: 03b5d0447ef732586fe2104690c609a3adec2b6c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34356146"
---
# <a name="create-an-application-gateway-with-ssl-termination-using-the-azure-cli"></a>İle SSL sonlandırma Azure CLI kullanarak bir uygulama ağ geçidi oluşturma

Azure CLI oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](application-gateway-introduction.md) için bir sertifika ile [SSL sonlandırma](application-gateway-backend-ssl.md) kullanan bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) arka uç sunucuları için. Bu örnekte uygulama ağ geçidi varsayılan arka uç havuzuna eklenen iki sanal makine örnekleri ölçek kümesini içerir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ kurma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
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

Kullanabileceğiniz [az ağ uygulama-ağ geçidi oluşturma](/cli/azure/application-gateway#create) uygulama ağ geçidi oluşturmak için. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtin. 

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

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidi varsayılan arka uç havuzunda sunucuları sağlayan bir sanal makine ölçek kümesi oluşturun. Ölçek kümesindeki sanal makineler ilişkili *myBackendSubnet* ve *appGatewayBackendPool*. Ölçek oluşturmak için kullanabileceğiniz [az vmss oluşturma](/cli/azure/vmss#az_vmss_create).

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

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
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

![Güvenli uyarı](./media/application-gateway-ssl-cli/application-gateway-secure.png)

Kendinden imzalı bir sertifika kullanıyorsa uyarı güvenlik kabul etmeyi seçin **ayrıntıları** ve ardından **Web sayfasına gidin**. Güvenli NGINX siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Temel uygulama ağ geçidi URL'de test](./media/application-gateway-ssl-cli/application-gateway-nginx.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ kurma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Bir sanal makineyi ölçeği varsayılan arka uç havuzuyla Ayarla oluşturma

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.
