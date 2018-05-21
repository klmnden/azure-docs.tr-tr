---
title: Birden çok siteyi barındıran ile - bir uygulama ağ geçidi oluşturma Azure CLI | Microsoft Docs
description: Azure CLI kullanarak birden çok site barındıran bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 7728ade5f84cd97f3ff0cebd0d5545d68d0ebd79
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="create-an-application-gateway-with-multiple-site-hosting-using-the-azure-cli"></a>Birden çok site Azure CLI kullanarak barındırma ile bir uygulama ağ geçidi oluşturma

Azure CLI yapılandırmak için kullanabileceğiniz [birden çok web sitesi barındırma](application-gateway-multi-site-overview.md) oluştururken bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide, sanal makine ölçekleme kümeleri kullanarak arka uç havuzları oluşturun. Daha sonra dinleyicileri ve uygun sunucuları havuzlarındaki web trafiği ulaştığında emin olmak için kendi etki alanlarını temel alan kurallar yapılandırın. Bu öğretici birden çok etki alanları ve kullanım örnekleri sahibi olduğunu varsayar *www.contoso.com* ve *www.fabrikam.com*.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Uygulama ağ geçidi oluşturma
> * Dinleyicileri ve yönlendirme kuralları oluşturma
> * Sanal makine ölçek kümeleri ile arka uç havuzları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturun

![Çok siteli yönlendirme örneği](./media/tutorial-multisite-cli/scenario.png)

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

### <a name="add-the-backend-pools"></a>Arka uç havuzları ekleme

Adlı arka uç havuzları ekleme *contosoPool* ve *fabrikamPool* kullanarak arka uç sunucuları içerecek şekilde gerekli [az ağ uygulama ağ geçidi adres havuzu oluşturma](/cli/azure/application-gateway#az_network_application_gateway_address_pool_create).

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name contosoPool
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name fabrikamPool
```

### <a name="add-listeners"></a>Dinleyicileri ekleme

Dinleyici için arka uç havuzu uygun şekilde trafiği yönlendirmek uygulama ağ geçidi etkinleştirmek için gereklidir. Bu öğreticide, iki etki alanları için iki dinleyici oluşturun. Bu örnekte, etki alanları için dinleyicileri oluşturulan *www.contoso.com* ve *www.fabrikam.com*. 

Adlı dinleyicileri eklemek *contosoListener* ve *fabrikamListener* kullanarak trafiği yönlendirmek için gereken [az ağ uygulama ağ geçidi http dinleyicisi oluşturmak](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com
az network application-gateway http-listener create \
  --name fabrikamListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.fabrikam.com   
  ```

### <a name="add-routing-rules"></a>Yönlendirme kuralları ekleme

Kurallar, oluşturuldukları sırada işlenir ve URL ile eşleşen ilk kural kullanarak uygulama ağ geçidi için gönderilen trafik yönlendirilir. Temel bir dinleyici ve çok siteli dinleyicisi hem aynı bağlantı noktasında kullanan bir kural kullanarak bir kuralı varsa, örneğin, çok siteli dinleyicisi kuralla beklendiği şekilde çalışması için temel dinleyicisi çok siteli kuralı için sırada olan kural önce listelenmiş olması gerekir. 

Bu örnekte, iki yeni kural oluşturma ve uygulama ağ geçidi oluşturduğunuzda, oluşturulan varsayılan kuralı silin. Kural kullanarak ekleyebilirsiniz [az ağ uygulama ağ geçidi kuralını](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoListener \
  --rule-type Basic \
  --address-pool contosoPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name fabrikamRule \
  --resource-group myResourceGroupAG \
  --http-listener fabrikamListener \
  --rule-type Basic \
  --address-pool fabrikamPool
az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri oluşturma

Bu örnekte, üç arka uç havuzu uygulama ağ geçidi destek üç sanal makine ölçek kümeleri oluşturun. Oluşturduğunuz ölçek kümeleri adlı *myvmss1*, *myvmss2*, ve *myvmss3*. Her bir ölçek kümesi NGINX yüklemek iki sanal makine örneklerini içerir.

```azurecli-interactive
for i in `seq 1 2`; do
  if [ $i -eq 1 ]
  then
    poolName="contosoPool"
  fi
  if [ $i -eq 2 ]
  then
    poolName="fabrikamPool"
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
for i in `seq 1 2`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{
  "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="create-a-cname-record-in-your-domain"></a>Etki alanınızda bir CNAME kaydı oluşturun

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresi alın ve etki alanınızdaki bir CNAME kaydı oluşturmak için kullanın. Kullanabileceğiniz [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show) uygulama ağ geçidi DNS adresi alınamıyor. Kopya *fqdn* DNSSettings değerini ve oluşturduğunuz CNAME kaydı değeri olarak kullanın. 

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

Uygulama ağ geçidi yeniden başlatıldığında VIP değişebilir çünkü A kayıtlarını kullanılması önerilmez.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Gibi http://www.contoso.com.

![Uygulama ağ geçidi test contoso sitede](./media/tutorial-multisite-cli/application-gateway-nginxtest1.png)

Diğer etki alanınıza adresini değiştirin ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

![Uygulama ağ geçidi test fabrikam sitede](./media/tutorial-multisite-cli/application-gateway-nginxtest2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Uygulama ağ geçidi oluşturma
> * Dinleyicileri ve yönlendirme kuralları oluşturma
> * Sanal makine ölçek kümeleri ile arka uç havuzları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturun

> [!div class="nextstepaction"]
> [Uygulama ağ geçidi ile neler yapabileceğiniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)