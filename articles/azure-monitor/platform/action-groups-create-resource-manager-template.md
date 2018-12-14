---
title: Resource Manager şablonları ile Eylem grupları oluşturma
description: Bir Azure Resource Manager şablonu kullanarak bir eylem grubu oluşturmayı öğrenin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 5c0bfa3512215124a4e6169622c84c6cf3fb3cf7
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346485"
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Resource Manager şablonu ile bir eylem grubu oluştur
Bu makalede nasıl kullanılacağını gösterir bir [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) Eylem grupları yapılandırmak için. Şablonları kullanarak, otomatik olarak belirli uyarı türleri yeniden kullanılabilir Eylem grupları ayarlayabilirsiniz. Bu eylem grupları doğru bütün tarafların bir uyarı tetiklendiğinde bildirim aldığından emin olun.

Temel adımlar şunlardır:

1. Eylem grubu oluşturmayı açıklayan bir JSON dosyası olarak bir şablon oluşturun.

2. Kullanarak şablonu dağıtma [herhangi bir dağıtım yöntemi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

İlk olarak, burada eylem tanımları şablonu sabit kodlanmış bir eylem grubu için bir Resource Manager şablonunun nasıl oluşturulacağını açıklar. İkinci olarak, şablon dağıtıldığında, Web kancası yapılandırma bilgilerini giriş parametresi olarak alan bir şablonun nasıl oluşturulacağını açıklar.

## <a name="resource-manager-templates-for-an-action-group"></a>Bir eylem grubu için Resource Manager şablonları

Resource Manager şablonu kullanarak bir eylem grubu oluşturmak için kaynak türü oluştur `Microsoft.Insights/actionGroups`. Daha sonra tüm ilgili özellikleri doldurun. Bir eylem grubu oluşturma iki örnek şablonu aşağıda verilmiştir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service Name."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md).
* Daha fazla bilgi edinin [uyarılar](alerts-overview.md).
* Eklemeyi öğrenin [Resource Manager şablonu kullanarak uyarıları](../../azure-monitor/platform/alerts-activity-log.md).
