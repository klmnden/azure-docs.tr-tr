---
title: "Bir web uygulaması güvenlik duvarı yapılandırma: Azure uygulama ağ geçidi | Microsoft Docs"
description: "Bu makalede bir mevcut veya yeni uygulama ağ geçidi üzerinde bir web uygulaması Güvenlik Duvarı'nı kullanmaya başlamak hakkında yönergeler açıklanmaktadır."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: davidmu
<<<<<<< HEAD
ms.openlocfilehash: c9c740a3a1a28a1a9a4f2abf579fe2adb54e4f47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: e60bfc89378569b154f4f973d1dceb683fa58482
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Yeni veya var olan uygulama ağ geçidi Azure CLI ile bir web uygulaması Güvenlik Duvarı'nı yapılandırın

> [!div class="op_single_selector"]
> * [Azure portalı](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Bir web uygulaması Güvenlik Duvarı (WAF) oluşturmayı öğrenin-uygulama ağ geçidi etkin. Ayrıca bir WAF var olan bir uygulama ağ geçidi eklemeyi öğrenin.

Azure uygulama ağ geçidi olarak WAF web uygulamaları, SQL ekleme gibi ortak web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirilmesini korur.

 Uygulama ağ geçidi bir katman 7 yük dengeleyicidir. Bulut veya şirket içi olsanız yük devretme, HTTP isteklerini, farklı sunucular arasında performans amaçlı yönlendirme sağlar. Uygulama ağ geçidi birçok uygulama teslim denetleyicisi (ADC) özellikleri sağlar:

 * HTTP Yük Dengeleme 
 * Tanımlama bilgisi tabanlı oturum benzeşimi 
 * Güvenli Yuva Katmanı (SSL) yük boşaltma 
 * Özel sistem durumu araştırmalarının 
 * Çoklu tesis işlevi için destek
 
 Desteklenen özelliklerin tam listesi için bkz [genel bakış, uygulama ağ geçidi](application-gateway-introduction.md).

Bu makalede gösterilmektedir nasıl [var olan bir uygulama ağ geçidi için bir web uygulaması güvenlik duvarı ekleme](#add-web-application-firewall-to-an-existing-application-gateway). Ayrıca gösterir nasıl [bir web uygulaması Güvenlik Duvarı'nı kullanan bir uygulama ağ geçidi oluşturma](#create-an-application-gateway-with-web-application-firewall).

![Senaryo görüntüsü][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Önkoşul: Azure CLI 2.0 yükleyin

Bu makaledeki adımları gerçekleştirmek için gerek [Mac, Linux ve Windows Azure komut satırı arabirimi (Azure CLI) yükleyin](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>WAF yapılandırma farklılıkları

Okuduğunuz varsa [Azure CLI ile bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-cli.md), bir uygulama ağ geçidi oluşturduğunuzda yapılandırmak için SKU ayarları anlama. WAF üzerinde bir uygulama ağ geçidi SKU yapılandırdığınızda tanımlamak için ek ayarlar sağlar. Uygulama ağ geçidinde kendisini oluşturan ek değişiklik yoktur.

| **Ayar** | **Ayrıntılar**
|---|---|
|**SKU** |Normal uygulama ağ geçidi bir WAF olmadan destekleyen **standart\_küçük**, **standart\_orta**, ve **standart\_büyük**boyutları. Bir WAF başlanmasıyla, iki ek SKU vardır **WAF\_orta** ve **WAF\_büyük**. Bir WAF küçük uygulama ağ geçitleri üzerinde desteklenmiyor.|
|**Modu** | Bu ayar WAF modudur. izin verilen değerler **algılama** ve **önleme**. WAF ayarlandığında **algılama** modu, tüm tehditler bir günlük dosyasında depolanır. İçinde **önleme** modu, olayları yine kaydedilir, ancak saldırgan yetkisiz 403 alır uygulama ağ geçidi yanıt.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidi için bir web uygulaması güvenlik duvarı ekleme

Aşağıdaki komut WAF etkin uygulama ağ geçidi için mevcut bir standart uygulama ağ geçidi değiştirir:

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Bu komut, uygulama ağ geçidi WAF ile güncelleştirir. Uygulama ağ geçidiniz için günlükleri görüntülemek nasıl anlamak için bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md). Bir WAF güvenlik yapısı nedeniyle, güvenlik yaklaşımı, web uygulamalarınızı, düzenli olarak anlamak için günlükleri gözden geçirin.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

Aşağıdaki komut, bir uygulama ağ geçidi ile WAF oluşturur:

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> Temel WAF yapılandırma ile oluşturulmuş uygulama ağ geçitleri için koruma CRS 3.0 ile yapılandırılır.

## <a name="get-an-application-gateway-dns-name"></a>Bir uygulama ağ geçidi DNS adını Al

Ağ geçidi oluşturulduktan sonra sonraki adıma iletişimi için ön uç yapılandırmaktır. Genel IP kullandığınızda, uygulama ağ geçidi kolay olmayan dinamik olarak atanmış bir DNS adı gerektirir. Kullanıcıların uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanın. Daha fazla bilgi için bkz: [bir Azure bulut hizmeti için bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Bir CNAME kaydı yapılandırmak için uygulama ağ geçidine bağlı Publicıpaddress öğesi kullanarak uygulama ağ geçidi ve ilişkili IP/DNS adı ayrıntılarını alabilirsiniz. İki web uygulamaları bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidi DNS adını kullanın. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtları kullanarak önermiyoruz.

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

WAF kurallarını özelleştirmek öğrenmek için bkz: [Azure CLI 2.0 aracılığıyla web uygulaması güvenlik duvarı kurallarını özelleştirmek](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
