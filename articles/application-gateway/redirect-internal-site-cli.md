---
title: İç yönlendirme - Azure CLI ile bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Azure CLI kullanarak uygun havuza iç web trafiği yönlendiren bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.openlocfilehash: e44beed592e013a089011c2a774b7300f2b3f854
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66133769"
---
# <a name="create-an-application-gateway-with-internal-redirection-using-the-azure-cli"></a>Azure CLI kullanarak iç yeniden yönlendirmeyi ile bir uygulama ağ geçidi oluşturma

Azure CLI'yı yapılandırmak için kullanabileceğiniz [web trafiğini yeniden yönlendirme](multiple-site-overview.md) oluşturduğunuzda bir [uygulama ağ geçidi](overview.md). Bu öğreticide, bir sanal makine ölçek kümesi kullanarak arka uç havuzu tanımlayın. Ardından dinleyicileri ve web trafiği uygun havuzu ulaşan emin olmak için kendi etki alanları temel alarak kurallar yapılandırın. Bu öğreticide, birden çok etki alanları ve kullandığı örnekleri olduğunuz varsayılır *www\.contoso.com* ve *www\.contoso.org*.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Uygulama ağ geçidi oluşturma
> * Dinleyicileri ve yeniden yönlendirme kuralı Ekle
> * Sanal makine ölçek kümesi ile arka uç havuzu oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

[az network vnet create](/cli/azure/network/vnet) komutunu kullanarak *myVNet* adlı sanal ağı ve *myAGSubnet* adlı alt ağı oluşturun. Daha sonra adlı alt ağ ekleyebilirsiniz *myBackendSubnet* sunucuları kullanarak arka uç havuzu tarafından gerekli [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet). [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create) komutunu kullanarak *myAGPublicIPAddress* adlı genel IP adresini oluşturun.

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

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

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
- *kural 1*: *appGatewayHttpListener* ile ilişkili varsayılan yönlendirme kuralıdır.


## <a name="add-listeners-and-rules"></a>Dinleyiciler ve kurallar ekleme 

Uygulama ağ geçidinin trafiği arka uç havuzuna uygun şekilde yönlendirmesini sağlamak için bir dinleyici gereklidir. Bu öğreticide iki etki alanınız için iki dinleyici oluşturacaksınız. Bu örnekte, dinleyici etki alanları için oluşturulan *www\.contoso.com* ve *www\.contoso.org*.

[az network application-gateway http-listener create](/cli/azure/network/application-gateway/http-listener#az-network-application-gateway-http-listener-create) kullanarak gereken arka uç dinleyicilerini ekleyin.

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

Gelen trafiği gönderir yeniden yönlendirme yapılandırması Ekle *www\.consoto.org* dinleyicisi için *www\.contoso.com* kullanarakapplicationGateway'de[az ağ uygulama ağ geçidini yeniden yönlendirme yapılandırması oluşturma](/cli/azure/network/application-gateway/redirect-config#az-network-application-gateway-redirect-config-create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name orgToCom \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener contosoListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-routing-rules"></a>Yönlendirme kuralları ekleme

Kuralları içinde oluşturuldukları sırada işlenir ve URL ile eşleşen ilk kural kullanarak uygulama ağ geçidine gönderilen trafik yönlendirilir. Örneğin, aynı bağlantı noktasında temel bir dinleyici kullanan bir kuralınız ve çok siteli dinleyici kullanan bir kuralınız varsa çok siteli kuralın beklendiği gibi çalışması için çok siteli dinleyicinin kuralı temel dinleyici kuralından önce listelenmelidir. 

Bu örnekte, iki yeni kurallar oluşturabilir ve oluşturulan varsayılan kuralı.  Kuralı [az network application-gateway rule create](/cli/azure/network/application-gateway/rule#az-network-application-gateway-rule-create) komutunu kullanarak ekleyebilirsiniz.

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

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerini oluşturma

Bu örnekte, oluşturduğunuz arka uç havuzu destekleyen bir sanal makine ölçek kümesi oluşturun. Oluşturduğunuz ölçek kümesini adlı *myvmss* ve NGINX yüklediğiniz iki sanal makine örnekleri içerir.

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

Shell penceresine şu komutu çalıştırın:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-cname-record-in-your-domain"></a>Etki alanınızda CNAME kaydı oluşturma

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresini alabilir ve etki alanınızda bir CNAME kaydı oluşturmak için kullanabilirsiniz. Uygulama ağ geçidinin DNS adresini almak için [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) komutunu kullanabilirsiniz. DNSSetting için *fqdn* değerini kopyalayın ve bu değeri oluşturduğunuz CNAME kaydının değeri olarak kullanın. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtlarının kullanımı önerilmez.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Örneğin http://www.contoso.com.

![Uygulama ağ geçidinde contoso test etme](./media/redirect-internal-site-cli/application-gateway-nginxtest.png)

Diğer etki alanınıza, adresi değiştirin; örneğin http://www.contoso.org ve trafiği www için dinleyici için yeniden yönlendirilen görmelisiniz\.contoso.com.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> * Ağı ayarlama
> * Uygulama ağ geçidi oluşturma
> * Dinleyicileri ve yeniden yönlendirme kuralı Ekle
> * Sanal makine ölçek kümesi ile arka uç havuzu oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma
