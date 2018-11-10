---
title: URL tabanlı içerik yönlendirmeye genel bakış | Microsoft Belgeleri
description: Bu sayfada, Application Gateway URL'si tabanlı içerik yönlendirme, UrlPathMap yapılandırması ve PathBasedRouting kuralı için genel bir bakış sunulmuştur.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.date: 11/7/2018
ms.author: victorh
ms.openlocfilehash: bc123307a3cc3a5040e93e517c60604dc75fc7e7
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218432"
---
# <a name="url-path-based-routing-overview"></a>URL Yolu Tabanlı Yönlendirmeye genel bakış

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. 

Senaryolardan biri, farklı içerik türleri için istekleri farklı arka uç sunucu havuzlarına yönlendirmektir.

Aşağıdaki örnekte, Application Gateway contoso.com için VideoServerPool, ImageServerPool ve DefaultServerPool gibi üç arka uç sunucu havuzlarından trafik sunmaktadır.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

http://contoso.com/video/* için istekler VideoServerPool’a ve http://contoso.com/images/* için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

> [!IMPORTANT]
> Kurallar, portalda listelendikleri sırayla işlenir. Temel dinleyiciyi yapılandırmadan önce çok siteli dinleyicileri yapılandırmanız önerilir.  Bu işlem, trafiğin doğru arka uca yönlendirilmesini güvence altına alır. Temel dinleyici listede ilk sıradaysa ve gelen bir istekle eşleşiyorsa, o dinleyici tarafından işlenir.

## <a name="urlpathmap-configuration-element"></a>UrlPathMap yapılandırma öğesi

UrlPathMap öğesi, arka uç sunucu havuzu eşlemeleri için Yol desenleri belirtmek üzere kullanılır. Aşağıdaki kod örneği, şablon dosyasındaki urlPathMap öğesinin kod parçacığıdır.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> PathPattern: Bu ayar, eşleştirilecek yol desenlerinin listesidir. Her biri / ile başlamalıdır. "*" işareti, yalnızca "/" işaretinin ardından en sona koyulabilir. Eşleştiricisine dize birinciden sonra herhangi bir metin içermez? veya # yanı sıra, bu karakterlere burada kullanılamaz. Aksi takdirde, bir URL'de izin verilen herhangi bir karakter Pathpattern'da izin verilir.

Daha fazla bilgi için [URL tabanlı yönlendirme kullanan bir Resource Manager şablonunu](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) inceleyebilirsiniz.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting kuralı

PathBasedRouting türündeki RequestRoutingRule, bir dinleyiciyi urlPathMap’e bağlamak için kullanılır. Bu dinleyici için alınan tüm istekler, urlPathMap’te belirtilen ilkeye göre yönlendirilir.
PathBasedRouting kuralının kod parçacığı:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

URL tabanlı içerik yönlendirme hakkında bilgi edindikten sonra, URL yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturmak için [URL tabanlı yönlendirme kullanan uygulama ağ geçidi oluşturma](application-gateway-create-url-route-portal.md) bölümüne gidin.
