---
title: "Azure Event Hubs üretilen iş birimleri otomatik olarak ölçeklendirin | Microsoft Docs"
description: "Otomatik olarak üretilen iş birimleri ölçeklendirmek için bir ad alanında otomatik Şişir etkinleştir"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 1cd31e0866ee6088483f88e8f80d01f75764c771
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Azure Event Hubs üretilen iş birimleri otomatik olarak ölçeklendirin

Azure Event Hubs platform akış yüksek düzeyde ölçeklenebilir bir veridir. Bu nedenle, Event Hubs müşteriler genellikle kullanımları hizmetine ekleme sonra artırın. Bu tür artar, olay hub'ları ölçeklendirme ve daha büyük aktarım hızlarıyla işlemek için önceden belirlenmiş üretilen iş birimleri artırma gerektirir. *Otomatik Şişir* olay hub'larını özelliği kullanım gereksinimlerini karşılamak için işleme birimlerinin sayısı otomatik olarak ölçeklendirir. Üretilen iş birimleri artırma engeller senaryoları, azaltma:

* Veri giriş hızları kümesi üretilen iş birimleri aşıyor.
* Veri çıkışı isteği oranlarının kümesi üretilen iş birimleri aşıyor.

## <a name="how-auto-inflate-works"></a>Otomatik Şişir nasıl çalışır

Olay hub'ları trafiği işleme birimleri tarafından denetlenir. Tek bir işleme birimi giriş ve çıkış miktarı iki kez saniyede 1 MB sağlar. Standart olay hub'ları 1-20 işleme birimleri ile yapılandırılabilir. Otomatik Şişir en düşük gerekli işleme birimleri ile küçük Başlat olanak sağlar. Özellik sonra otomatik olarak üretilen iş birimleri trafiğinizi artış bağlı olarak, gereken maksimum sınırı ölçeklendirir. Otomatik Şişir aşağıdaki avantajları sağlar:

- Küçük başlayın ve büyüdükçe ölçeği için verimli bir ölçeklendirme mekanizması.
- Otomatik olarak belirtilen üst sınıra sorunları azaltma olmadan ölçeklendirin.
- Daha fazla ölçeklendirme üzerinden ne zaman denetimi olarak ve ölçek için ne kadar denetler.

## <a name="enable-auto-inflate-on-a-namespace"></a>Bir ad otomatik Şişir etkinleştir

Etkinleştirmek veya otomatik Şişir aşağıdaki yöntemlerden birini kullanarak bir ad üzerinde devre dışı bırakabilirsiniz:

1. [Azure portal](https://portal.azure.com).
2. Bir Azure Resource Manager şablonu.

### <a name="enable-auto-inflate-through-the-portal"></a>Portal etkinleştirme otomatik Şişir

Bir olay hub'ları ad alanı oluştururken bir ad alanı otomatik Şişir özelliğini etkinleştirebilirsiniz:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Bu seçeneği etkinleştirdiğinizde, üretilen iş birimleri küçük başlayın ve kullanımınızı artış gerektiği ölçeği. Enflasyon için üst sınır fiyatlandırma, işleme birimleri saat başına kullanılan sayısına bağlı olan etkilemez.

Otomatik Şişir-kullanarak da etkinleştirebilirsiniz **ölçek** seçeneği ayarlar dikey penceresinde Portalı'nda:
 
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
* [Olay Hub’ı oluşturma](event-hubs-create.md)
