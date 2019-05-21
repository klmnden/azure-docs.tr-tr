---
title: Web uygulaması güvenlik duvarını etkinleştirme - Azure CLI
description: Azure CLI kullanarak bir uygulama ağ geçidinde web uygulaması güvenlik duvarı ile web trafiğini kısıtlama hakkında bilgi edinin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 5/20/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 1822fe032a7c7a6382dbae2cb9f7095d1d076008
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65955484"
---
# <a name="enable-web-application-firewall-using-the-azure-cli"></a>Azure CLI kullanarak web uygulaması Güvenlik Duvarı'nı etkinleştir

Bir [uygulama ağ geçidi](overview.md) üzerindeki trafiği [web uygulaması güvenlik duvarı](waf-overview.md) (WAF) ile kısıtlayabilirsiniz. WAF, uygulamanızı korumak için [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) kurallarını kullanır. Bu kurallar SQL ekleme, siteler arası betik saldırıları ve oturum ele geçirme gibi saldırılara karşı korumayı içerir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * WAF etkinken bir uygulama ağ geçidi oluşturma
> * Sanal makine ölçek kümesi oluşturma
> * Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

![Web uygulaması güvenlik duvarı örneği](./media/tutorial-restrict-web-traffic-cli/scenario-waf.png)

Tercih ederseniz, bu yordamı kullanarak tamamlayabilirsiniz [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. *az group create* komutuyla [myResourceGroupAG](/cli/azure/group#az-group-create) adlı bir Azure kaynak grubu oluşturun.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Sanal ağ ve alt ağlar, uygulama ağ geçidi ve ilişkili kaynakları ile ağ bağlantısı sağlamak için kullanılır. [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) ve [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) komutlarını kullanarak *myVNet* adlı sanal ağı ve *myAGSubnet* adlı alt ağı oluşturun. [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create) komutunu ile *myAGPublicIPAddress* adlı genel IP adresini oluşturun.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myBackendSubnet \
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create \
  --name myAGSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24

az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway-with-a-waf"></a>WAF ile uygulama ağ geçidi oluşturma

*myAppGateway* adlı uygulama ağ geçidini oluşturmak için [az network application-gateway create](/cli/azure/network/application-gateway) komutunu kullanabilirsiniz. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Uygulama ağ geçidi, *myAGSubnet*’e ve daha önce oluşturduğunuz *myAGPublicIPAddress*’e atanır.

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --capacity 2 \
  --sku WAF_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress

az network application-gateway waf-config set \
  --enabled true \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --firewall-mode Detection \
  --rule-set-version 3.0
```

Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. Uygulama ağ geçidi oluşturulduktan sonra şu yeni özellikleri görürsünüz:

- *appGatewayBackendPool* -bir uygulama ağ geçidi en az bir arka uç adres havuzuna sahip olmalıdır.
- *appGatewayBackendHttpSettings*: İletişim için 80 numaralı bağlantı noktasının ve HTTP protokolünün kullanıldığını belirtir.
- *appGatewayHttpListener*: *appGatewayBackendPool* ile ilişkili varsayılan dinleyicidir.
- *appGatewayFrontendIP*: *appGatewayHttpListener*’a *myAGPublicIPAddress*’i atar.
- *kural 1*: *appGatewayHttpListener* ile ilişkili varsayılan yönlendirme kuralıdır.

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidinde arka uç havuzu için iki sunucu sağlayan bir sanal makine ölçek kümesi oluşturursunuz. Ölçek kümesindeki sanal makineler *myBackendSubnet* ile ilişkilidir. Ölçek kümesini oluşturmak için [az vmss create](/cli/azure/vmss#az-vmss-create) komutunu kullanabilirsiniz.

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
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],"commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

Bu makalede, uygulama ağ geçidi, algılama ve önleme amacıyla verileri depolamak için bir depolama hesabı kullanır. Veri kaydetmek için Azure İzleyici günlüklerine veya olay hub'ı kullanabilirsiniz. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[New-AzureRmStorageAccount](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) komutuyla *myagstore1* adlı bir depolama hesabı oluşturun.

```azurecli-interactive
az storage account create \
  --name myagstore1 \
  --resource-group myResourceGroupAG \
  --location eastus \
  --sku Standard_LRS \
  --encryption blob
```

### <a name="configure-diagnostics"></a>Tanılama yapılandırma

Tanılamayı ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog ve ApplicationGatewayFirewallLog günlüklerine verileri kaydedecek şekilde yapılandırın. `<subscriptionId>` numarasını abonelik tanımlayıcınızla değiştirin ve [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) komutuyla tanılamayı yapılandırın.

```azurecli-interactive
appgwid=$(az network application-gateway show --name myAppGateway --resource-group myResourceGroupAG --query id -o tsv)

storeid=$(az storage account show --name myagstore1 --resource-group myResourceGroupAG --query id -o tsv)

az monitor diagnostic-settings create --name appgwdiag --resource $appgwid \
  --logs '[ { "category": "ApplicationGatewayAccessLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayPerformanceLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayFirewallLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --storage-account $storeid
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) komutunu kullanın. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Temel URL’yi uygulama ağ geçidinde test etme](./media/tutorial-restrict-web-traffic-cli/application-gateway-nginxtest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, uygulama ağ geçidini ve tüm ilgili kaynakları silin.

```azurecli-interactive
az group delete --name myResourceGroupAG 
```

## <a name="next-steps"></a>Sonraki adımlar

* [SSL sonlandırma ile bir uygulama ağ geçidi oluşturma](./tutorial-ssl-cli.md)
