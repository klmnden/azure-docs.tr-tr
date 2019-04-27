---
title: Azure Application Gateway'de WebSocket desteği | Microsoft Docs
description: Bu sayfa, uygulama ağ geçidi WebSocket desteği'ne genel bakış sağlar.
author: vhorne
ms.author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 03/18/2019
ms.openlocfilehash: 54c34690e678f07d6309a1877b0ca5d0a0b274f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831250"
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Application Gateway'de WebSocket desteği'ne genel bakış

Uygulama ağ geçidi, tüm ağ geçidi boyutları arasında Websocket'e yönelik yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. 

WebSocket protokolü, standartlaştırılmış [RFC6455](https://tools.ietf.org/html/rfc6455) uzun süre çalışan bir TCP bağlantı üzerinden bir sunucu ve istemci arasındaki tam çift yönlü iletişimi sağlar. Web sunucusu ve çift yönlü olarak gerekli HTTP tabanlı uygulamaları yoklama gerek kalmadan olabilir istemci arasında daha etkileşimli bir iletişim için bu özelliği sağlar. WebSocket düşük HTTP farklı olarak ek yükü vardır ve birden çok istek/yanıt kullanımını daha verimli olan kaynakları kaynaklanan aynı TCP bağlantısını yeniden kullanabilirsiniz. WebSocket protokolleri, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışacak şekilde tasarlanmıştır.

WebSocket trafiğini almak için 80 veya 443 bağlantı noktasını standart bir HTTP dinleyicisi kullanmaya devam edebilirsiniz. WebSocket trafiğini sonra uygulama ağ geçidi kuralları'nda belirtildiği gibi uygun arka uç havuzuna kullanarak WebSocket etkin arka uç sunucusuna yönlendirilir. Arka uç sunucusuna açıklanan uygulama ağ geçidi araştırmaları yanıt [sistem durumu araştırması genel bakış](application-gateway-probe-overview.md) bölümü. Uygulama ağ geçidi sistem durumu araştırmaları, HTTP/HTTPS yalnızca etkilenir. Her arka uç sunucusuna WebSocket trafiğini sunucusuna yönlendirmek uygulama ağ geçidi için HTTP araştırmaları için yanıt vermelidir.

Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.

## <a name="how-does-websocket-work"></a>WebSocket nasıl çalışır

WebSocket bağlantısı kurmak için belirli bir HTTP tabanlı anlaşması, istemci ve sunucu arasında değiştirilir. Başarılı olursa, uygulama katmanı Protokolü "HTTP kullanarak daha önce oluşturulmuş TCP bağlantısı WebSockets için yükseltilir". Böyle bir kez HTTP tamamen resmi dışında olduğunda; verileri WebSocket bağlantısı kapatılana kadar her iki bitiş noktası tarafından WebSocket protokolü kullanılarak alınan veya gönderilemez. 

![addcert](./media/application-gateway-websocket/websocket.png)

### <a name="listener-configuration-element"></a>Dinleyici yapılandırma öğesi

Var olan bir HTTP dinleyicisi WebSocket trafiğini desteklemek için kullanılabilir. Örnek şablon dosyasındaki httpListeners öğesinin bir parçacığı aşağıda verilmiştir. WebSocket desteği ve WebSocket trafiğini güvenli hale hem HTTP hem de HTTPS dinleyicileri gerekir. Benzer şekilde portal veya Azure PowerShell dinleyicileri ile bir uygulama ağ geçidi oluşturmak için kullanabileceğiniz açık WebSocket trafiğini desteklemek için 80/443 numaralı bağlantı noktası.

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, Backendhttpsetting'de ve yönlendirme kuralı yapılandırması

Bir BackendAddressPool etkin WebSocket sunucuları ile arka uç havuzu olarak tanımlamak için kullanılır. Bir arka uç bağlantı noktası ile 80 ve 443 Backendhttpsetting'de tanımlanır. Tanımlama bilgisi tabanlı benzeşim ve requestTimeouts özelliklerini WebSocket trafiğini ilgili değildir. Yönlendirme kuralında yapılması gereken bir değişiklik yoktur, 'Temel' uygun dinleyicisini ilgili arka uç adres havuzuna bağlamak için kullanılır. 

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

Arka ucunuzu yapılandırılmış çalıştıran bir HTTP/HTTPS web sunucusuna sahip olmanız gerekir (genellikle 80/443) çalışmaya WebSocket için bağlantı noktası. WebSocket Protokolü HTTP üstbilgi alanı olarak WebSocket Protokolü yükseltmeye sahip olacak şekilde ilk el sıkışma gerektirdiğinden bu gereksinimidir. Bir üst bilgisi bir örnek verilmiştir:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: https://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Bu uygulama ağ geçidi arka uç sistem durumu araştırma yalnızca HTTP ve HTTPS protokollerini destekler, başka bir neden olmasıdır. Arka uç sunucusu için HTTP veya HTTPS araştırmaları yanıt vermezse, arka uç havuz dışına alınır.

## <a name="next-steps"></a>Sonraki adımlar

WebSocket desteği hakkında daha fazla edindikten sonra Git [bir uygulama ağ geçidi oluşturma](quick-create-powershell.md) bir WebSocket ile kullanmaya başlamak için web uygulaması etkin.