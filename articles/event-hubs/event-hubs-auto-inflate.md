---
title: Azure Event Hubs işleme birimleri otomatik olarak ölçeklendirme | Microsoft Docs
description: Otomatik şişme, işleme birimleri otomatik olarak ölçeklendirmek için bir ad alanında etkinleştirin.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: 32f99b43a37277e70d209f1f315dcb398c2b5931
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40004801"
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Azure Event Hubs işleme birimleri otomatik olarak ölçeklendirme

Azure Event Hubs akış platformu yüksek düzeyde ölçeklenebilir bir veridir. Bu nedenle, Event Hubs kullanımı genellikle başlattıktan sonra hizmeti kullanmak üzere artırır. Önceden tanımlanmış artan tür kullanımı gerektirir [üretilen iş birimleri](event-hubs-features.md#throughput-units) Event Hubs ölçeklendirip daha büyük aktarım hızlarını işleyebilme. **Otomatik şişme** özellik Event Hubs'ın otomatik olarak ölçeklendirilebilir üretilen iş birimi sayısını artırarak, kullanım karşılaması gerekir. Üretilen iş birimleri artırma engeller azaltma senaryoları, burada:

* Veri giriş hızlarını kümesi işleme birimleri en fazla.
* Veri çıkışı istek hızları kümesi işleme birimleri en fazla.

## <a name="how-auto-inflate-works"></a>Otomatik şişme nasıl çalışır

Olay hub'ları trafiği tarafından denetlenir [üretilen iş birimleri](event-hubs-features.md#throughput-units). Tek bir işleme birimi, giriş ve çıkış miktarı iki kez saniyede 1 MB sağlar. Standart event hubs, 1-20 üretilen iş birimleri ile yapılandırılabilir. Otomatik şişme seçtiğiniz minimum gerekli işleme birimleri ile küçükten başlayabilir olanak tanır. Özellik sonra otomatik olarak üretilen iş birimleri, trafik bağlı olarak gereken üst sınırı için ölçeklendirir. Otomatik şişme, aşağıdaki faydaları sağlar:

- Küçükten başlayabilir ve büyüdükçe ölçeklendirin ' için etkili bir ölçeklendirme mekanizması.
- Belirtilen üst sınıra azaltma sorunları olmadan otomatik olarak ölçeklendirin.
- Daha fazla ölçeklendirme üzerinden zaman denetlemek için ve ne kadar ölçeklendirmek için denetler.

## <a name="enable-auto-inflate-on-a-namespace"></a>Otomatik şişme ad alanınızdaki etkinleştir

Etkinleştirebilir veya otomatik şişme bir Event Hubs ad alanı üzerinde aşağıdaki yöntemlerden birini kullanarak devre dışı bırakın:

- [Azure portalında](https://portal.azure.com).
- Bir [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate).

### <a name="enable-auto-inflate-through-the-portal"></a>Otomatik şişme portal aracılığıyla etkinleştirme

Otomatik şişme özelliği, bir Event Hubs ad alanını oluştururken etkinleştirebilirsiniz:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Bu seçenek etkinleştirildiğinde, işleme birimleri ile küçükten başlayabilir ve kullanım artışı gereksinimleriniz değiştikçe ölçeği artırma. Üst sınırını Enflasyon hemen fiyatlandırması, saat başına kullanılan aktarım hızı birimi sayısı bağımlı olduğu etkilemez.

Otomatik şişme kullanarak etkinleştirebilirsiniz **ölçek** portalında ayarlar bölmesini seçeneği:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Otomatik Şişme bir Azure Resource Manager şablonu kullanarak etkinleştirin

Bir Azure Resource Manager şablon dağıtımı sırasında otomatik şişme etkinleştirebilirsiniz. Örneğin, `isAutoInflateEnabled` özelliğini **true** ayarlayıp `maximumThroughputUnits` 10. Örneğin:

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

Tam şablon için bkz: [oluşturma Event Hubs ad alanı ve Şişir etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) github'da şablonu.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)

