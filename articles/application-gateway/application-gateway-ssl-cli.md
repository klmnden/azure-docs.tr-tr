---
title: "SSL boşaltma - Azure uygulama ağ geçidi - Azure CLI 2.0 yapılandırma | Microsoft Docs"
description: "Bu makalede, Azure CLI 2.0 kullanarak SSL ile bir uygulama ağ geçidi oluşturma yönergelerini boşaltma sağlanmaktadır."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: a2c4062db821e39e1af4fa1d54da0121d3993db4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi yapılandırma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Klasik PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. Ayrıca, SSL yük boşaltımı ön uç sunucusundaki sertifika yönetimi basitleştirir.

## <a name="prerequisite-install-the-azure-cli-20"></a>Önkoşul: Azure CLI 2.0 yükleyin

Bu makaledeki adımları gerçekleştirmek için gerek [Mac, Linux ve Windows (Azure CLI) için Azure komut satırı arabirimini yükleyin](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="required-components"></a>Gerekli bileşenler

* **Arka uç sunucu havuzuna**: arka uç sunucularının IP adresleri listesi. Listede bulunan IP adresleri sanal ağ alt ağına ait olması gerekir veya bir ortak IP adresi veya sanal IP adresi (VIP) olmalıdır.
* **Arka uç sunucu havuzu ayarları**: her bir havuz bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası**: Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici**: dinleyicisinde bir ön uç bağlantı noktası, bir iletişim kuralı kullanılır (Http veya Https; bu ayarları büyük/küçük harfe duyarlıdır) ve (SSL yük boşaltımı yapılandırılıyorsa) SSL sertifika adı.
* **Kural**: kural dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve ne zaman belli bir Dinleyicide trafik trafiği yönlendirmek için hangi arka uç sunucu havuzuna tanımlar. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol *Https* (küçük/büyük harf duyarlı) ile değiştirilmelidir. Ekleme **SslCertificate** öğesine **HttpListener** SSL sertifikası için yapılandırılmış değişken değeri. Ön uç bağlantı noktası için güncelleştirilmesi **443**.

**Tanımlama bilgisi temelli benzeşimi etkinleştirmek için**: bir istemci oturumundan gelen isteğin web grubunda aynı VM için her zaman yönlendirilir emin olmak için bir uygulama ağ geçidini yapılandırabilirsiniz. Bunu başarmak için ağ geçidinin trafiği uygun şekilde doğrudan bir oturum tanımlama bilgisi ekleyin. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi *BackendHttpSetting* öğesindeki **Enabled**’a ayarlayın.

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a>SSL boşaltma üzerindeki var olan bir uygulama ağ geçidini yapılandırma

Mevcut bir uygulama ağ geçidi için SSL yük boşaltmayı yapılandırmak için aşağıdaki komutları girin:

```azurecli-interactive
#!/bin/bash

# Create a new front end port to be used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload the .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing the port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool to be used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using the new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking the listener to the back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a>SSL boşaltma ile bir uygulama ağ geçidi oluşturma

Aşağıdaki örnek bir uygulama ağ geçidi ile SSL boşaltma oluşturur. Sertifika ve sertifika parolası geçerli bir özel anahtara güncelleştirilmesi gerekir.

```azurecli-interactive
#!/bin/bash

# Creates an application gateway with SSL offload
az network application-gateway create \
  --name "AdatumAppGateway3" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG2" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet" \
  --subnet-address-prefix "10.0.0.0/28" \
  --frontend-port 443 \
  --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 \
  --sku "Standard_Small" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip" \
  --public-ip-address-allocation "dynamic"
```

## <a name="get-an-application-gateway-dns-name"></a>Bir uygulama ağ geçidi DNS adını Al

Ağ geçidi oluşturulduktan sonra sonraki adıma iletişimi için ön uç yapılandırmaktır.  Uygulama ağ geçidi, bir ortak IP kolay olmayan kullanırken dinamik olarak atanmış bir DNS adı gerektirir. Son kullanıcıların uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure'da bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Bir diğer ad yapılandırmak için uygulama ağ geçidi ve ilişkili kendi IP/DNS adını kullanarak ayrıntılarını almak **Publicıpaddress** öğesi uygulama ağ geçidine bağlı. İki web uygulamaları bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidi DNS adını kullanın. A kayıtlarını kullanımını VIP üzerinde değiştirebildiğinizden uygulama ağ geçidi yeniden öneririz yok.


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

## <a name="next-steps"></a>Sonraki adımlar

Bir iç yük dengeleyici ile kullanmak için bkz: bir uygulama ağ geçidi yapılandırmak istiyorsanız, [bir iç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük Dengeleme seçeneklerini genel hakkında daha fazla bilgi için bkz:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
