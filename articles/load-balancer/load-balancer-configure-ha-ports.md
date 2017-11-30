---
title: "Yüksek kullanılabilirlik bağlantı noktalarını Azure yük dengeleyici için yapılandırma | Microsoft Docs"
description: "Yük Dengeleme tüm bağlantı noktalarındaki iç trafiğini için yüksek kullanılabilirlik bağlantı noktalarını kullanmayı öğrenin"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/02/2017
ms.author: kumud
ms.openlocfilehash: 646ade828e96810bdc3b07d4dc5c0276a1621969
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>İç yük dengeleyici için yüksek kullanılabilirlik bağlantı noktalarını yapılandırma

Bu makalede örnek dağıtımı için yüksek kullanılabilirlik (HA) bir iç yük dengeleyici bağlantı noktaları sağlar. Ağ sanal Gereçleri (NVAs) belirli yapılandırmaları için karşılık gelen sağlayıcı Web sitelerine bakın.

>[!NOTE]
> Yüksek kullanılabilirlik bağlantı noktalarını özelliği şu anda önizlemede değil. Önizleme sırasında bu özellik genel kullanılabilirlik sunumundaki özelliklerle aynı seviyede kullanılabilirliğe ve güvenilirliğe sahip olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Şekil 1, bu makalede açıklanan dağıtım örneği aşağıdaki yapılandırma gösterilmektedir:
- Bir iç yük dengeleyici arkasında HA bağlantı noktalarını yapılandırma arka uç havuzundaki NVAs dağıtılır. 
- UDR üzerinde DMZ alt ağ yollarını tüm trafik için NVAs sonraki atlama iç yük dengeleyici sanal IP olarak yaparak uygulanır. 
- İç yük dengeleyici LB algoritması göre etkin NVAs birine trafiği dağıtır.
- NVA trafiği işler ve arka uç alt özgün hedef iletir.
- Karşılık gelen UDR arka uç alt ağda yapılandırılmışsa dönüş yolu da aynı yol alabilir. 

![Ha örnek dağıtım noktaları](./media/load-balancer-configure-ha-ports/haports.png)

Şekil 1 - ağ sanal yüksek oranda kullanılabilir bağlantı noktaları olan bir iç yük dengeleyici arkasında dağıtılan uygulamaları 

## <a name="preview-sign-up"></a>Önizleme kaydolma

Yük Dengeleyici standart HA bağlantı noktalarını özelliği Önizlemesi'na katılmak için Azure CLI 2.0 veya PowerShell kullanarak erişmek için aboneliğinizi kaydedin. Aboneliğiniz için kayıt [yük dengeleyici Standard Önizleme](https://aka.ms/lbpreview#preview-sign-up).

>[!NOTE]
>Yük Dengeleyici standart önizlemeleri kaydı bir saate kadar sürebilir.

## <a name="configuring-ha-ports"></a>HA bağlantı noktalarını yapılandırma

Bir iç yük dengeleyici arka uç havuzundaki NVAs ile NVA sistem durumu ve HA bağlantı noktasına sahip yük dengeleyici kuralı algılamak için bir karşılık gelen yük dengeleyici durum araştırması yapılandırması ayarlama HA bağlantı noktalarının yapılandırması içerir. Genel yük dengeleyici ilişkili yapılandırma içinde ele [Get Started](load-balancer-get-started-ilb-arm-portal.md). Bu makalede HA bağlantı noktalarını yapılandırma vurgular.

Ön uç bağlantı noktası ve arka uç bağlantı noktası değeri ayarlamak yapılandırma temelde gerektirir **0**ve protokolü değerine **tüm**. Bu makalede Azure portal, PowerShell ve Azure CLI 2.0 kullanarak yüksek kullanılabilirlik bağlantı noktalarını yapılandırma.

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>Azure portalıyla HA bağlantı noktalarını yük dengeleyici kuralı yapılandırma

Azure portalı içerir **HA bağlantı noktaları** bu yapılandırma için bir onay kutusu aracılığıyla seçeneği. Seçili olduğunda, ilgili bağlantı noktası ve protokol yapılandırma otomatik olarak doldurulur. 

![Ha yapılandırması Azure portal aracılığıyla bağlantı noktaları](./media/load-balancer-configure-ha-ports/haports-portal.png)

Şekil 2 - HA Portalı aracılığıyla bağlantı noktalarını yapılandırma

### <a name="configure-ha-ports-lb-rule-via-resource-manager-template"></a>Resource Manager şablonu aracılığıyla HA bağlantı noktalarını LB kuralı yapılandırma

2017-08-01 API sürümü Microsoft.Network/loadBalancers için yük dengeleyici kaynak kullanarak HA bağlantı noktalarını yapılandırabilirsiniz. Aşağıdaki JSON parçacığı HA bağlantı noktaları için REST API aracılığıyla yük dengeleyici yapılandırmasında değişiklik gösterir.

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>PowerShell ile HA bağlantı noktalarını yük dengeleyici kuralı yapılandırma

PowerShell ile iç yük dengeleyici oluşturulurken HA bağlantı noktalarını yük dengeleyici kuralı oluşturmak için aşağıdaki komutu kullanın:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>Azure CLI 2.0 ile HA bağlantı noktalarını yük dengeleyici kuralı yapılandırma

Adım # 4 konumunda [iç bir yük dengeleyici kümesi oluşturma](load-balancer-get-started-ilb-arm-cli.md), yük dengeleyici kuralı HA bağlantı oluşturmak için aşağıdaki komutu kullanın.

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yüksek oranda kullanılabilir bağlantı noktaları](load-balancer-ha-ports-overview.md)
