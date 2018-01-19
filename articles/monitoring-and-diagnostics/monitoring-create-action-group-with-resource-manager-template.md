---
title: "Resource Manager şablonları ile eylemi gruplar oluşturun | Microsoft Docs"
description: "Bir Azure Resource Manager şablonu kullanarak bir eylem grubu oluşturmayı öğrenin."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 5e715cad5cb28ad0c763ffb29c43e9ee98741699
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Resource Manager şablonu ile bir eylem grubu oluştur
Bu makalede nasıl kullanılacağı gösterilmektedir bir [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) Eylem grupları yapılandırmak için. Şablonları kullanarak otomatik olarak belirli uyarı türleri içinde yeniden kullanılabilir Eylem grupları ayarlayabilirsiniz. Bu eylem grupları tüm doğru tarafların bir uyarı tetiklendiğinde bildirim aldığından emin olun.

Temel adımlar şunlardır:

1. Eylem grubunun nasıl oluşturulacağı açıklayan bir JSON dosyası bir şablon oluşturun.

2. Kullanarak şablonu dağıtmak [herhangi bir dağıtım yöntemini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

İlk olarak, burada eylem tanımları şablonda sabit kodlanmış bir eylem grubu için bir Resource Manager şablonu oluşturma açıklanmaktadır. İkinci olarak, şablonu dağıttığınızda, Web kancası yapılandırma bilgilerini giriş parametresi olarak geçen bir şablonun nasıl oluşturulacağını açıklar.

## <a name="resource-manager-templates-for-an-action-group"></a>Bir eylem grubu için Resource Manager şablonları

Resource Manager şablonu kullanarak bir eylem grubu oluşturmak için bir kaynak türü oluşturmak `Microsoft.Insights/actionGroups`. Ardından tüm ilgili özellikleri doldurur. Bir eylem grubu oluşturma iki örnek Şablonlar aşağıda verilmiştir.

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
      "apiVersion": "2017-04-01",
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
      "apiVersion": "2017-04-01",
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
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md).
* Daha fazla bilgi edinmek [uyarıları](monitoring-overview-alerts.md).
* Eklemeyi öğrenin [Resource Manager şablonu kullanarak uyarıları](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
