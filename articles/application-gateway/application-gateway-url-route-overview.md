---
title: "URL tabanlı içerik yönlendirmeye genel bakış | Microsoft Belgeleri"
description: "Bu sayfada, Application Gateway URL&quot;si tabanlı içerik yönlendirme, UrlPathMap yapılandırması ve PathBasedRouting kuralı için genel bir bakış sunulmuştur."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: ee8cfffdbf054b4251ed269745f6b9ee5a5e6c64
ms.openlocfilehash: 1f273f3b55d719e37b9cdb6cefda30c3566e7226


---
# <a name="url-path-based-routing-overview"></a>URL Yolu Tabanlı Yönlendirmeye genel bakış
URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı arka uç sunucu havuzlarına yönlendirmektir.
Aşağıdaki örnekte, Application Gateway contoso.com için VideoServerPool, ImageServerPool ve DefaultServerPool gibi üç arka uç sunucu havuzlarından trafik sunmaktadır.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

http://contoso.com/video* için istekler VideoServerPool’a ve http://contoso.com/images* için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

## <a name="urlpathmap-configuration-element"></a>UrlPathMap yapılandırma öğesi
UrlPathMap öğesi, arka uç sunucu havuzu eşlemeleri için Yol desenleri belirtmek üzere kullanılır. Aşağıdaki kod örneği, şablon dosyasındaki urlPathMap öğesinin kod parçacığıdır.

```json
"urlPathMaps": [
{
"name": "<urlPathMapName>",
"id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>",
"properties": {
    "defaultBackendAddressPool": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName>"
    },
    "defaultBackendHttpSettings": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpSettingsList/<settingsName>"
    },
    "pathRules": [
        {
            "paths": [
                <pathPattern>
            ],
            "backendAddressPool": {
                "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName2>"
            },
            "backendHttpsettings": {
                "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpsettingsList/<settingsName2>"
            },

        },

    ],

}
}
]
```

> [!NOTE]
> PathPattern: Bu ayar, eşleştirilecek yol desenlerinin listesidir. Her biri / ile başlamalıdır. "*" işareti, yalnızca "/" işaretinin ardından en sona koyulabilir. Desen eşleştiricisine verilen dize, ilk ? veya # işaretinden sonra herhangi bir metin içermez ve burada, bu karakterlere izin verilmez. 
> 
> 

Daha fazla bilgi için [URL tabanlı yönlendirme kullanan bir Resource Manager şablonunu](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) inceleyebilirsiniz.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting kuralı
PathBasedRouting türündeki RequestRoutingRule, bir dinleyiciyi urlPathMap’e bağlamak için kullanılır. Bu dinleyici için alınan tüm istekler, urlPathMap’te belirtilen ilkeye göre yönlendirilir.
PathBasedRouting kuralının kod parçacığı:

```json
"requestRoutingRules": [
    {

"name": "<ruleName>",
"id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/requestRoutingRules/<ruleName>",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>"
    },

}
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
URL tabanlı içerik yönlendirme hakkında bilgi edindikten sonra, URL yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturmak için [URL tabanlı yönlendirme kullanan uygulama ağ geçidi oluşturma](application-gateway-create-url-route-portal.md) bölümüne gidin.




<!--HONumber=Nov16_HO3-->


