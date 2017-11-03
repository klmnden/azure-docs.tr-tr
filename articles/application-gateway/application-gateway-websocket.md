---
title: "Azure uygulama ağ geçidi WebSocket desteği | Microsoft Docs"
description: "Bu sayfa uygulama ağ geçidi WebSocket desteği'ne genel bakış sağlar."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: amsriva
ms.openlocfilehash: 75b06ddd02da231b7813c609c848c75e42116da5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Uygulama ağ geçidi WebSocket desteği'ne genel bakış

Uygulama ağ geçidi tüm ağ geçidi boyutları arasında WebSocket için yerel destek sağlar. Seçmeli olarak etkinleştirmek veya WebSocket desteği devre dışı bırakmak için kullanıcı tarafından yapılandırılabilir bir ayar yoktur. 

WebSocket Protokolü içinde standartlaştırılmış [RFC6455](https://tools.ietf.org/html/rfc6455) uzun süre çalışan bir TCP bağlantısı üzerinden bir sunucu ve istemci arasındaki tam çift yönlü iletişimi sağlar. Web sunucusu ve çift yönlü yoklama için gereken HTTP tabanlı uygulamaları olarak gerek kalmadan olabilir istemci arasında daha etkileşimli iletişim için bu özelliği sağlar. WebSocket düşük HTTP yükü var ve kaynakların daha verimli bir kullanımı kaynaklanan birden çok istek/yanıt aynı TCP bağlantısını yeniden kullanabilirsiniz. WebSocket protokolleri, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışmak üzere tasarlanmıştır.

Standart bir HTTP dinleyicisi WebSocket trafiği almak için 80 veya 443 numaralı bağlantı noktasını kullanmaya devam edebilirsiniz. WebSocket trafiği sonra uygulama ağ geçidi kurallarını belirtildiği gibi uygun arka uç havuzu kullanarak WebSocket etkin arka uç sunucusuna yönlendirilir. Arka uç sunucusuna açıklanan uygulama ağ geçidi araştırmalar yanıt [sistem durumu araştırma genel bakış](application-gateway-probe-overview.md) bölümü. Uygulama ağ geçidi sistem durumu araştırmalarının, yalnızca HTTP/HTTPS etkilenir. Her arka uç sunucusu HTTP araştırmalar sunucusuna WebSocket trafiği yönlendirmek uygulama ağ geçidi için yanıt gerekir.

## <a name="listener-configuration-element"></a>Dinleyici yapılandırma öğesi

Varolan bir HTTP dinleyicisi WebSocket trafiğini desteklemek için kullanılabilir. Bir örnek şablon dosyası Httplistener öğeden parçacığıyla verilmiştir. WebSocket desteği ve WebSocket trafiğinin güvenliğini sağlamak için hem HTTP hem de HTTPS dinleyicileri gerekir. Benzer şekilde kullanabilirsiniz [portal](application-gateway-create-gateway-portal.md) veya [PowerShell](application-gateway-create-gateway-arm.md) dinleyicileri ile bir uygulama ağ geçidi oluşturmak için bağlantı noktası WebSocket trafiğini desteklemek için 80/443 üzerindeki.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting ve yönlendirme kuralı yapılandırma

Bir BackendAddressPool bir arka uç havuzu etkin WebSocket sunucularıyla tanımlamak için kullanılır. BackendHttpSetting bir arka uç bağlantı noktası 80 ve 443 tanımlanır. Tanımlama bilgisi temelli benzeşimi ve requestTimeouts özelliklerini WebSocket trafiği ilgili değildir. Yönlendirme kuralında gerekli bir değişiklik yapılmaz, 'Temel' karşılık gelen arka uç adres havuzu için uygun dinleyicisi bağlamanın kullanılır. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>Etkin WebSocket arka uç

Arka yapılandırılmış üzerinde çalışan bir HTTP/HTTPS web sunucunuzun olması gerekir (genellikle 80/443) çalışmaya WebSocket için bağlantı noktası. WebSocket Protokolü WebSocket protokolü bir üstbilgi alanı olarak yükseltme ile HTTP olması için ilk el sıkışma gerektirdiğinden bu gerekli değildir. Bir üst bilgisi bir örnek verilmiştir:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Başka bir nedeni, bu uygulama ağ geçidi arka uç sistem durumu araştırma yalnızca HTTP ve HTTPS protokollerini destekler ' dir. Arka uç sunucusuna HTTP veya HTTPS araştırmaları yanıt vermezse, arka uç havuzu dışında alınır.

## <a name="next-steps"></a>Sonraki adımlar

WebSocket desteği hakkında daha fazla öğrenme sonra Git [bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway.md) WebSocket ile çalışmaya başlamak için web uygulaması etkinleştirildi.

