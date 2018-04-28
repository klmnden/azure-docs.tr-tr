---
title: Yüksek kullanılabilirlik bağlantı noktalarını Azure yük dengeleyici için yapılandırma | Microsoft Docs
description: Yük Dengeleme tüm bağlantı noktalarındaki iç trafiğini için yüksek kullanılabilirlik bağlantı noktalarını kullanmayı öğrenin
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/20178
ms.author: kumud
ms.openlocfilehash: eb305986982432d7a432204e3fae8a1dff6a5d74
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>Bir iç yük dengeleyici için yüksek kullanılabilirlik bağlantı noktalarını yapılandırma

Bu makale, bir iç yük dengeleyici örnek dağıtımı için yüksek kullanılabilirlik bağlantı noktaları sağlar. Sanal gereçler (NVAs) ağ özgü yapılandırmalar hakkında daha fazla bilgi için karşılık gelen sağlayıcı Web sitelerine bakın.

>[!NOTE]
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede, standart yük dengeleyici anlatılmaktadır. Temel yük dengeleyici hakkında daha fazla bilgi için bkz: [yük dengeleyici genel bakış](load-balancer-overview.md).

Çizim, bu makalede açıklanan dağıtım örneği aşağıdaki yapılandırması gösterilmektedir:

- Yüksek kullanılabilirlik bağlantı noktalarını yapılandırma arkasındaki bir iç yük dengeleyici arka uç havuzundaki NVAs dağıtılır. 
- Kullanıcı tanımlı yönlendirme (UDR) üzerinde DMZ alt ağ yollarını tüm trafik için NVAs yük dengeleyici sanal IP sonraki atlama dahili olarak yaparak uygulanır. 
- İç yük dengeleyicisi trafiği yük dengeleyici algoritması göre etkin NVAs birine dağıtır.
- NVA trafiği işler ve arka uç alt özgün hedef iletir.
- Karşılık gelen UDR arka uç alt ağda yapılandırılmışsa dönüş yolu aynı yol alabilir. 

![Yüksek kullanılabilirlik bağlantı noktaları örnek dağıtım](./media/load-balancer-configure-ha-ports/haports.png)



## <a name="configure-high-availability-ports"></a>Yüksek kullanılabilirlik bağlantı noktalarını yapılandırma

Yüksek kullanılabilirlik bağlantı noktalarını yapılandırmak için NVAs olan bir iç yük dengeleyici arka uç havuzundaki ayarlayın. Bir karşılık gelen yük dengeleyici durum araştırması yapılandırmasını NVA sistem durumu ve yüksek kullanılabilirlik bağlantı noktasına sahip yük dengeleyici kuralı algılamak için ayarlayın. Genel yük dengeleyici ilişkili yapılandırma içinde ele [başlama](load-balancer-get-started-ilb-arm-portal.md). Bu makalede, yüksek kullanılabilirlik bağlantı noktalarını yapılandırma vurgular.

Ön uç bağlantı noktası ve arka uç bağlantı noktası değerine ayarlayarak yapılandırma temelde içerir **0**. Protokol değeri ayarlamak **tüm**. Bu makalede Azure portal, PowerShell ve Azure CLI 2.0 kullanarak yüksek kullanılabilirlik bağlantı noktalarını yapılandırma.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Azure portalı ile yüksek kullanılabilirlik bağlantı noktalarını yük dengeleyici kuralı yapılandırma

Azure Portalı'nı kullanarak yüksek kullanılabilirlik bağlantı noktalarını yapılandırmak için seçin **HA bağlantı noktaları** onay kutusu. Seçili olduğunda, ilgili bağlantı noktası ve protokol yapılandırma otomatik olarak doldurulur. 

![Azure Portalı aracılığıyla yüksek kullanılabilirlik bağlantı noktalarını yapılandırma](./media/load-balancer-configure-ha-ports/haports-portal.png)


### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Resource Manager şablonu aracılığıyla yüksek kullanılabilirlik bağlantı noktalarını Yük Dengeleme kuralı yapılandırma

2017-08-01 API sürümü Microsoft.Network/loadBalancers için yük dengeleyici kaynak kullanarak yüksek kullanılabilirlik bağlantı noktalarını yapılandırabilirsiniz. Aşağıdaki JSON parçacığı REST API aracılığıyla yüksek kullanılabilirlik bağlantı noktaları için yük dengeleyici yapılandırması değişiklikleri gösterilmektedir:

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

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>PowerShell ile yüksek kullanılabilirlik bağlantı noktalarını yük dengeleyici kuralı yapılandırma

PowerShell ile iç yük dengeleyicisi oluştururken yüksek kullanılabilirlik bağlantı noktalarını yük dengeleyici kuralı oluşturmak için aşağıdaki komutu kullanın:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli-20"></a>Azure CLI 2.0 ile yüksek kullanılabilirlik bağlantı noktalarını yük dengeleyici kuralı yapılandırma

4. adımda [bir iç yük dengeleyici kümesi oluştur](load-balancer-get-started-ilb-arm-cli.md), yüksek kullanılabilirlik bağlantı noktalarını yük dengeleyici kuralı oluşturmak için aşağıdaki komutu kullanın:

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [yüksek kullanılabilirlik bağlantı noktalarını](load-balancer-ha-ports-overview.md).
