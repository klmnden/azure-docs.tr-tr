---
title: "Bir uygulama ağ geçidi - Azure CLI 2.0 oluşturma | Microsoft Docs"
description: "Resource Manager'da Azure CLI 2.0 kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 9d3732d538f3ed9ecb87247f378a3736692025ca
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanarak bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Klasik PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager şablonu](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Azure uygulama ağ geçidi, çeşitli katman 7 Yük Dengeleme özellikleri, uygulamanız için sunarak, bir hizmet olarak uygulama teslim denetleyici (ADC) sağlar ayrılmış bir sanal gereç olmayan.

## <a name="cli-versions"></a>CLI sürümleri

Aşağıdaki komut satırı arabirimi (CLI) sürümlerinden birini kullanarak bir uygulama ağ geçidi oluşturabilirsiniz:

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md): Klasik ve Azure Resource Manager dağıtım modelleri için Azure CLI
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md): Resource Manager dağıtım modeli için yeni nesil CLI

## <a name="prerequisite-install-the-azure-cli-20"></a>Önkoşul: Azure CLI 2.0 yükleyin

Bu makaledeki adımları gerçekleştirmek için gerek [macOS, Linux ve Windows için Azure CLI yükleme](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Bir uygulama ağ geçidi oluşturmak için bir Azure hesabınız olmalıdır. Yoksa, kaydolun bir [ücretsiz deneme sürümü](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Senaryo

Bu senaryoda, Azure portalı kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin.

Bu senaryo aşağıdakileri yapar:

* Orta uygulama ağ geçidi ile iki örnek oluşturun.
* Bir ayrılmış 10.0.0.0/16 CIDR bloğu ile AdatumAppGatewayVNET adlı bir sanal ağ oluşturun.
* Appgatewaysubnet adlı, CIDR bloğu olarak 10.0.0.0/28 kullanan bir alt ağ oluşturacaksınız.

> [!NOTE]
> Uygulama ağ geçidi oluşturulduktan sonra ve ilk dağıtım sırasında değil özel sistem durumu araştırmalarının, arka uç havuzu adresleri ve ek kuralları da dahil olmak üzere uygulama ağ geçidi yapılandırmasından gerçekleşir.

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi, kendi alt gerektirir. Bir sanal ağ oluştururken, birden çok alt ağlar için yeterli adres alanı bırakın emin olun. Bir uygulama ağ geçidi bir alt ağa dağıttıktan sonra yalnızca ek uygulama ağ geçitleri bu alt ağ ekleyebilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Açık **Microsoft Azure komut istemi** ve oturum açın:

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Aynı zamanda `az login` aka.ms/devicelogin adresindeki kodunu girerek gerektirir cihaz oturum açma anahtarı olmadan.

Önceki komutu girdikten sonra bir kodu alırsınız. Oturum açma işlemine devam etmek için bir tarayıcıda https://aka.ms/devicelogin gidin.

![cmd gösteren cihaz oturum açma][1]

Tarayıcıda, aldığınız kodu girin. Bir oturum açma sayfasına yönlendirir.

![Tarayıcı kodu girin][2]

Oturum açmak için kodu girin ve ardından devam etmek için tarayıcıyı kapatın.

![başarıyla oturum açıldı][3]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Uygulama ağ geçidi oluşturmadan önce içermesi için bir kaynak grubu oluşturun. Aşağıdaki komutu kullanın:

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Arka uç IP adresleri için arka uç sunucu IP adresi kullanın. Bu değerler, sanal ağ içinde kullanılabilecek özel IP, genel IP'ler veya arka uç sunucuları için tam etki alanı adları olabilir. Aşağıdaki örnek bir uygulama ağ geçidi HTTP ayarları, bağlantı noktalarını ve kurallar için ek yapılandırmalar oluşturur:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

Önceki örnekte uygulama ağ geçidi oluşturma sırasında gerekli olmayan çeşitli özellikleri gösterir. Aşağıdaki kod örneği, bir uygulama ağ geçidi gerekli bilgilerle oluşturur:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> Oluşturma sırasında kullanılacak parametreleri listesi için aşağıdaki komutu çalıştırın: `az network application-gateway create --help`.

Bu örnek, dinleyicisi, arka uç havuzu, arka uç HTTP ayarları ve kuralları için varsayılan ayarlarla temel uygulama ağ geçidi oluşturur. Bu ayarları sağlama başarılı olduktan sonra dağıtımınızı uyacak şekilde değiştirebilirsiniz.

Web uygulaması, önceki adımlarda arka uç havuzuyla tanımlanmışsa, Yük Dengeleme artık başlar.

## <a name="get-the-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adını Al
Ağ geçidi oluşturduktan sonra iletişim için ön uç ardından yapılandırın. Genel IP kullanırken, uygulama ağ geçidi kolay olmayan dinamik olarak atanmış bir DNS adı gerektirir. Kullanıcıların uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanın. Daha fazla bilgi için bkz: [bir Azure hizmetini özel etki alanı ayarları sağlamak için Azure DNS'yi](../dns/dns-custom-domain.md).

Bir diğer ad yapılandırmak için uygulama ağ geçidine bağlı Publicıpaddress öğesi kullanarak uygulama ağ geçidi ve ilişkili IP/DNS adı ayrıntılarını alabilirsiniz. İki web uygulamaları bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidi DNS adını kullanın. VIP üzerinde değişebileceğinden Kullanım A kayıtlarını uygulama ağ geçidi için yeniden başlatma.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Sonraki adımlar

Özel sistem durumu araştırmalarının oluşturmayı öğrenmek için şu adrese gidin [portalını kullanarak uygulama ağ geçidi için özel bir araştırma oluşturmak](application-gateway-create-probe-portal.md).

SSL boşaltma yapılandırmak ve web sunucularınızın kapalı maliyetli SSL şifre çözme ele öğrenmek için bkz: [Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi yapılandırma](application-gateway-ssl-arm.md).

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
