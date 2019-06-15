---
title: Azure Application Gateway URL tabanlı içerik yönlendirmeye genel bakış
description: Bu makalede, Application Gateway URL'si tabanlı içerik yönlendirme, UrlPathMap yapılandırması ve PathBasedRouting kuralı için genel bir bakış sunulmuştur.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/23/2018
ms.author: victorh
ms.openlocfilehash: ee0267146140d095487b293331a7de493ba151c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61361962"
---
# <a name="azure-application-gateway-url-path-based-routing-overview"></a>Azure Application Gateway URL yolu tabanlı yönlendirmeye genel bakış

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. 

Senaryolardan biri, farklı içerik türleri için istekleri farklı arka uç sunucu havuzlarına yönlendirmektir.

Aşağıdaki örnekte, Application Gateway için örneğin contoso.com üç arka uç sunucu havuzlarından trafik hizmet veriyor: Videoserverpool'a, Imageserverpool ve DefaultServerPool gibi.

![imageURLroute](./media/url-route-overview/figure1.png)

İçin istekleri <http://contoso.com/video/*> , videoserverpool'a ve <http://contoso.com/images/*> istekler Imageserverpool'a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

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
> PathPattern: Bu ayar, eşleştirilecek yol desenlerinin listesidir. Her biri / ile başlamalıdır. "*" işareti, yalnızca "/" işaretinin ardından en sona koyulabilir. Desen eşleştiricisine verilen dize, ilk ? veya # işaretinden sonra herhangi bir metin içermez ve burada, bu karakterlere izin verilmez.

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

URL tabanlı içerik yönlendirme hakkında bilgi edindikten sonra, URL yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturmak için [URL tabanlı yönlendirme kullanan uygulama ağ geçidi oluşturma](tutorial-url-route-powershell.md) bölümüne gidin.
