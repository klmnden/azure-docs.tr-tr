---
title: "Bir web uygulaması güvenlik duvarı ile - Azure CLI bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturmayı öğrenin."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: davidmu
ms.openlocfilehash: 961642796525223eba4b19d77568d4149ee9d3c6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-cli"></a>Azure CLI kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

Azure CLI oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](application-gateway-introduction.md) ile bir [web uygulaması güvenlik duvarı](application-gateway-web-application-firewall-overview.md) (WAF) kullanan bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). WAF kullandığı [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) uygulamanızı korumak için kurallar. Bu kurallar SQL ekleme gibi saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirilmesini karşı koruma içerir. 

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Ağ kurma
> * Etkin WAF ile bir uygulama ağ geçidi oluşturma
> * Bir sanal makine ölçek kümesi oluşturma
> * Bir depolama hesabı oluşturmak ve tanılama Yapılandır

![Web uygulaması güvenlik duvarı örneği](./media/application-gateway-web-application-firewall-cli/scenario-waf.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Adlı bir Azure kaynak grubu oluşturma *myResourceGroupAG* ile [az grubu oluşturma](/cli/azure/group#az_group_create).

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturun

Sanal ağ ve alt ağları, uygulama ağ geçidi ve onun ilişkili kaynakları için ağ bağlantısı sağlamak için kullanılır. Adlı sanal ağ oluşturma *myVNet* ve adlı alt ağ *myAGSubnet* ile [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create) ve [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Adlı bir ortak IP adresi oluşturma *myAGPublicIPAddress* ile [az ağ genel IP oluşturun](/cli/azure/network/public-ip#az_network_public_ip_create).

```azurecli-interactive
az network vnet create 
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myBackendSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create 
  --name myAGSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24 
az network public-ip create 
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway-with-a-waf"></a>WAF ile bir uygulama ağ geçidi oluşturma

Kullanabileceğiniz [az ağ uygulama-ağ geçidi oluşturma](/cli/azure/application-gateway#az_application_gateway_create) adlı uygulama ağ geçidi oluşturmak için *myAppGateway*. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtin. Uygulama ağ geçidi atandığı *myAGSubnet* ve *myPublicIPSddress* daha önce oluşturduğunuz.

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
az network application-gateway waf-config set --enabled true \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --firewall-mode Detection
```

Oluşturulacak uygulama ağ geçidi için birkaç dakika sürebilir. Uygulama ağ geçidi oluşturulduktan sonra bu yeni özellikleri bunu görebilirsiniz:

- *appGatewayBackendPool* -bir uygulama ağ geçidi en az bir arka uç adres havuzu olmalıdır.
- *appGatewayBackendHttpSettings* -80 numaralı bağlantı noktasını ve HTTP protokolü kullanılır, iletişim için belirtir.
- *appGatewayHttpListener* -ilişkili varsayılan dinleyici *appGatewayBackendPool*.
- *appGatewayFrontendIP* -atar *myAGPublicIPAddress* için *appGatewayHttpListener*.
- *Kuralı 1* - ilişkilendirilen kural yönlendirme varsayılan *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidi arka uç havuzu için iki sunucu sağlayan bir sanal makine ölçek kümesi oluşturun. Ölçek kümesindeki sanal makineler ilişkili *myBackendSubnet* alt ağ. Ölçek oluşturmak için kullanabileceğiniz [az vmss oluşturma](/cli/azure/vmss#az_vmss_create).

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

Bulut Kabuğu'nda dosya oluşturmak istediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Girin `sensible-editor cloudConfig.json` dosyası oluşturmak için kullanılabilir düzenleyicileri listesini görmek için. Geçerli kabuğunuzu customConfig.json adlı bir dosya oluşturun ve aşağıdaki yapılandırma yapıştırın:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh"
}
```

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings @cloudConfig.json
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Bir depolama hesabı oluşturmak ve tanılama Yapılandır

Bu öğreticide, uygulama ağ geçidi saptamak ve önlemek amacıyla verileri depolamak için bir depolama hesabı kullanır. Veri kaydı için günlük analizi veya olay hub'ı da kullanabilirsiniz. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Adlı depolama hesabı oluşturma *myagstore1* ile [az depolama hesabı oluşturma](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create).

```azurecli-interactive
az storage account create \
  --name myagstore1 \
  --resource-group myResourceGroupAG \
  --location eastus \
  --sku Standard_LRS \
  --encryption blob
```

### <a name="configure-diagnostics"></a>Tanılama Yapılandır

Tanılama verileri kaydetmek üzere ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog ve ApplicationGatewayFirewallLog günlüklerine yapılandırın. Yedek `<subscriptionId>` abonelik tanımlayıcısı ile ve Tanılama ile yapılandırma [az İzleyici tanılama ayarlarını oluştur](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_create).

```azurecli-interactive
az monitor diagnostic-settings create --resource-id '/subscriptions/<subscriptionId>/resourceGroups/myResourceGroupAG/providers/Microsoft.Network/applicationGateways/myAppGateway' \
  --logs '[ { "category": "ApplicationGatewayAccessLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayPerformanceLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayFirewallLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --storage-account '/subscriptions/<subscriptionId>/resourceGroups/myResourceGroupAG/providers/Microsoft.Storage/storageAccounts/myagstore1'
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Uygulama ağ geçidi genel IP adresi almak için [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show). Genel IP adresini kopyalayın ve ardından, tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Temel uygulama ağ geçidi URL'de test](./media/application-gateway-web-application-firewall-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Etkin WAF ile bir uygulama ağ geçidi oluşturma
> * Bir sanal makine ölçek kümesi oluşturma
> * Bir depolama hesabı oluşturmak ve tanılama Yapılandır

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.