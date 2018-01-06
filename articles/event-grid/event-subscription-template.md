---
title: "Azure olay kılavuz abonelik şablonuyla"
description: "Bir olay kılavuz abonelik Resource Manager şablonu ile oluşturun."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2b9f55f8e944d688b622e30a773e1a34698f22ec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Olay kılavuz abonelik için kullanım Resource Manager şablonu

Bu makalede Azure Resource Manager şablonu olay kılavuz abonelikleri oluşturmak için nasıl kullanılacağı gösterilmektedir. Kullandığınız biçimi olup kaynak grubu olayları ya da belirli bir kaynak türü için olayları abone göre farklılık gösterir. Bu makalede iki biçim gösterilmektedir.

## <a name="subscribe-to-resource-group-events"></a>Kaynak grubu olaylarına abone olma

Kaynak grubu olaylara abone olma kullanırsanız `Microsoft.EventGrid/eventSubscriptions` kaynak türü için. Olayı için noktası türünü, kullanın ya da `WebHook` veya `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

Bu şablonu bir kaynak grubuna dağıttığınızda, bu kaynak grubu için olaylara abone olun.

## <a name="subscribe-to-resource-events"></a>Kaynak olaylarına abone olma

Kaynak olaylara abone olma, doğru kaynak için abonelik adını ve kaynak türü abonelik tanımında dahil ederek ilişkilendirin. Kaynak türü için `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. Ad için kullanmak `<resource-name>/Microsoft.EventGrid/<subscription-name>`. Olayı için noktası türünü, kullanın ya da `WebHook` veya `EventHub`.

Aşağıdaki örnek, Blob Depolama olaylarına abone olma gösterilmektedir.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Bir bir giriş için Resource Manager'ı için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).
* Olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).