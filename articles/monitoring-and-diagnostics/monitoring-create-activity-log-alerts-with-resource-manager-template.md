---
title: "Resource Manager şablonu ile bir etkinlik günlüğü uyarı oluşturabilir. | Microsoft Docs"
description: "Azure kaynaklarınızı oluşturduğunuzda bilgi edinin."
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
ms.date: 07/06/2017
ms.author: ancav
ms.openlocfilehash: 92076c7fe1f867919b7e02abf79cf0fb74fb7eb4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a>Resource Manager şablonu ile bir etkinlik günlüğü uyarı oluşturabilir.
Bu makalede nasıl kullanılacağı gösterilmektedir bir [Azure Resource Manager şablonu](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) etkinlik günlüğü uyarıları yapılandırmak için. Şablonlarını kullanarak kolayca otomatik dağıtım işleminin bir parçası belirli etkinlik günlüğü olay koşullara göre etkinleştirme çok uyarı ayarlayabilirsiniz.

Temel adımlar şunlardır:

1. Etkinlik günlüğü uyarı oluşturmak nasıl açıklayan bir JSON dosyası bir şablon oluşturun.

2. Kullanarak şablonu dağıtmak [herhangi bir dağıtım yöntemini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

## <a name="resource-manager-template-for-an-activity-log-alert"></a>Bir etkinlik günlüğü uyarı için Resource Manager şablonu
Resource Manager şablonu kullanarak bir etkinlik günlüğü uyarı oluşturmak için bir kaynak türü oluşturmak `microsoft.insights/activityLogAlerts`. Ardından tüm ilgili özellikleri doldurur. Bir etkinlik günlüğü uyarı oluşturan bir şablonu aşağıda verilmiştir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ] 
        },
        "actions": {
          "actionGroups": 
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

Ziyaret bizim [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) etkinlik günlüğü uyarı şablonları ile ilgili bazı örnekler.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [uyarıları](monitoring-overview-alerts.md).
- Eklemeyi öğrenin [Resource Manager şablonu kullanarak eylem grupları](monitoring-create-action-group-with-resource-manager-template.md).
- Bilgi nasıl [aboneliğinizi tüm otomatik ölçeklendirme motoru işlemleri izlemek için bir etkinlik günlüğü uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Bilgi nasıl [aboneliğinizi tüm başarısız otomatik ölçeklendirme ölçek/genişletme işlemleri izlemek için bir etkinlik günlüğü uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
