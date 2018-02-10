---
title: "İç yönlendirmesi - Azure CLI ile bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak uygun havuzu iç web trafiğini yönlendiren bir uygulama ağ geçidi oluşturmayı öğrenin."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 4228a3f534a5dc58ab2efa3c5cf0edd4caee43c9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-application-gateway-with-internal-redirection-using-the-azure-cli"></a>Bir uygulama ağ geçidi ile Azure CLI kullanarak iç yönlendirmesi oluşturma

Azure CLI yapılandırmak için kullanabileceğiniz [web trafiği yeniden yönlendirme](application-gateway-multi-site-overview.md) oluştururken bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide, bir sanal makine ölçek kümesi kullanan bir arka uç havuzu oluşturun. Daha sonra dinleyicileri ve web trafiği uygun havuz konumunda ulaşan emin olmak için kendi etki alanlarını temel alan kurallar yapılandırın. Bu öğretici birden çok etki alanları ve kullanım örnekleri sahibi olduğunu varsayar *www.contoso.com* ve *www.contoso.org*.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Ağ kurma
> * Uygulama ağ geçidi oluşturma
> * Dinleyicileri ve yeniden yönlendirme kuralı Ekle
> * Arka uç havuzu ile ayarlanmış bir sanal makine ölçek oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturun

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

Adlı sanal ağ oluşturma *myVNet* ve adlı alt ağın *myAGSubnet* kullanarak [az ağ vnet oluşturma](/cli/azure/network/vnet#az_net). Daha sonra adlı alt ağ ekleyebilirsiniz *myBackendSubnet* kullanarak sunucularının arka uç havuzu tarafından gerekli [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Adlı ortak IP adresi oluşturma *myAGPublicIPAddress* kullanarak [az ağ genel IP oluşturun](/cli/azure/public-ip#az_network_public_ip_create).

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


## <a name="add-listeners-and-rules"></a>Dinleyicileri ve kuralları ekleme 

Dinleyici için arka uç havuzu uygun şekilde trafiği yönlendirmek uygulama ağ geçidi etkinleştirmek için gereklidir. Bu öğreticide, iki etki alanları için iki dinleyici oluşturun. Bu örnekte, etki alanları için dinleyicileri oluşturulan *www.contoso.com* ve *www.contoso.org*.

Kullanarak trafiği yönlendirmek için gerekli arka uç dinleyicileri eklemek [az ağ uygulama ağ geçidi http dinleyicisi oluşturmak](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoComListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com
az network application-gateway http-listener create \
  --name contosoOrgListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.org   
  ```

### <a name="add-the-redirection-configuration"></a>Yeniden yönlendirme yapılandırması Ekle

Gelen trafiği gönderir yeniden yönlendirme yapılandırması eklemek *www.consoto.org* için dinleyici için *www.contoso.com* uygulama ağ geçidi kullanarak [az ağ uygulama ağ geçidi yeniden yönlendirme yapılandırması oluşturma](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name orgToCom \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener contosoComListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-routing-rules"></a>Yönlendirme kuralları ekleme

Kurallar, oluşturuldukları sırada işlenir ve URL ile eşleşen ilk kural kullanarak uygulama ağ geçidi için gönderilen trafik yönlendirilir. Oluşturulan varsayılan temel kural Bu öğreticide gerekli değildir. Bu örnekte adlı iki yeni kural oluşturma *contosoComRule* ve *contosoOrgRule* ve oluşturulan varsayılan kuralını silin.  Kullanan kurallar ekleyebilirsiniz [az ağ uygulama ağ geçidi kuralını](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoComRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoComListener \
  --rule-type Basic \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoOrgRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoOrgListener \
  --rule-type Basic \
  --redirect-config orgToCom
az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri oluşturma

Bu örnekte, oluşturulmuş varsayılan arka uç havuzu destekleyen bir sanal makine ölçek kümesi oluşturun. Oluşturduğunuz ölçek kümesini adlı *myvmss* ve NGINX yüklediğiniz iki sanal makine örneklerini içerir.

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

## <a name="create-cname-record-in-your-domain"></a>Etki alanınızda CNAME kaydı oluşturun

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresi alın ve etki alanınızdaki bir CNAME kaydı oluşturmak için kullanın. Kullanabileceğiniz [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show) uygulama ağ geçidi DNS adresi alınamıyor. Kopya *fqdn* DNSSettings değerini ve oluşturduğunuz CNAME kaydı değeri olarak kullanın. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebilir çünkü A kayıtlarını kullanılması önerilmez.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Örneğin, http://www.contoso.com.

![Uygulama ağ geçidi test contoso sitede](./media/tutorial-internal-site-redirect-cli/application-gateway-nginxtest.png)

Diğer etki alanınıza adresini değiştirmek, örneğin http://www.contoso.org ve trafiği www.contoso.com için dinleyici için yeniden yönlendirilen görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Uygulama ağ geçidi oluşturma
> * Dinleyicileri ve yeniden yönlendirme kuralı Ekle
> * Arka uç havuzu ile ayarlanmış bir sanal makine ölçek oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturun

> [!div class="nextstepaction"]
> [Uygulama ağ geçidi ile neler yapabileceğiniz hakkında daha fazla bilgi edinin](./application-gateway-introduction.md)