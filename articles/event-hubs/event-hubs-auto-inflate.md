---
title: "Azure Event Hubs üretilen iş birimleri otomatik olarak ölçeklendirin | Microsoft Docs"
description: "Otomatik olarak üretilen iş birimleri ölçeklendirmek için bir ad alanında otomatik Şişir etkinleştir"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 20ee0e6cff2a07cbd62a79799eada5708c7a0f07
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Azure Event Hubs üretilen iş birimleri otomatik olarak ölçeklendirin

Azure Event Hubs platform akış yüksek düzeyde ölçeklenebilir bir veridir. Bu nedenle, Event Hubs kullanımı genellikle hizmeti kullanmak başlattıktan sonra artırır. Bu tür kullanımı, olay hub'ları ölçeklendirme ve daha büyük aktarım hızlarıyla işlemek için önceden belirlenmiş üretilen iş birimleri yükseltmeyi gerektirir. *Otomatik Şişir* olay hub'larını özelliği kullanım gereksinimlerini karşılamak için işleme birimlerinin sayısı otomatik olarak ölçeklendirir. Üretilen iş birimleri artırma engeller senaryoları, azaltma:

* Veri giriş hızları kümesi üretilen iş birimleri aşıyor.
* Veri çıkışı isteği oranlarının kümesi üretilen iş birimleri aşıyor.

## <a name="how-auto-inflate-works"></a>Otomatik Şişir nasıl çalışır

Olay hub'ları trafiği işleme birimleri tarafından denetlenir. Tek bir işleme birimi giriş ve çıkış miktarı iki kez saniyede 1 MB sağlar. Standart bir olay hub'ları 1-20 işleme birimleri ile yapılandırılabilir. Otomatik Şişir en düşük gerekli işleme birimleri ile küçük Başlat olanak sağlar. Özellik sonra otomatik olarak üretilen iş birimleri trafiğinizi artış bağlı olarak, gereken maksimum sınırı ölçeklendirir. Otomatik Şişir aşağıdaki avantajları sağlar:

- Küçük başlayın ve büyüdükçe ölçeği için verimli bir ölçeklendirme mekanizması.
- Otomatik olarak belirtilen üst sınıra sorunları azaltma olmadan ölçeklendirin.
- Daha fazla ölçeklendirme üzerinden ne zaman denetimi olarak ve ölçek için ne kadar denetler.

## <a name="enable-auto-inflate-on-a-namespace"></a>Bir ad otomatik Şişir etkinleştir

Etkinleştirmek veya otomatik Şişir bir olay hub'ları ad aşağıdaki yöntemlerden birini kullanarak devre dışı bırakın:

1. [Azure portal](https://portal.azure.com).
2. Bir Azure Resource Manager şablonu.

### <a name="enable-auto-inflate-through-the-portal"></a>Portal etkinleştirme otomatik Şişir

Bir olay hub'ları ad alanı oluştururken otomatik Şişir özelliğini etkinleştirebilirsiniz:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Bu seçeneği etkinleştirdiğinizde, üretilen iş birimleri küçük başlayın ve kullanımınızı artış gerektiği ölçeği. Enflasyon için üst sınır hemen fiyatlandırma, işleme birimleri saat başına kullanılan sayısına bağlı olan etkilemez.

Otomatik Şişir-kullanarak da etkinleştirebilirsiniz **ölçek** portal ayarları bölmesinde seçeneği:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak otomatik Şişir etkinleştir

Bir Azure Resource Manager şablon dağıtımı sırasında otomatik Şişir etkinleştirebilirsiniz. Örneğin, `isAutoInflateEnabled` özelliğine **true** ve `maximumThroughputUnits` 10.

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Tam şablon için bkz: [olay hub'ı oluşturma ad alanı ve Şişir etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) github'da şablonu.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)

