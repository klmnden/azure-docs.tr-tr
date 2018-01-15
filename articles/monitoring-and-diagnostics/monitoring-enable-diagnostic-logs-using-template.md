---
title: "Otomatik olarak bir Resource Manager şablonu kullanarak tanılama ayarlarını etkinleştirin | Microsoft Docs"
description: "Tanılama günlüklerinize Event hubs'a akışla aktarmak veya bir depolama hesabında depolamak sağlayacak tanılama ayarları oluşturmak için Resource Manager şablonu kullanmayı öğrenin."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a8a88a8c-4a48-4df6-8f7e-d90634d39c57
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: johnkem
ms.openlocfilehash: 6355433dab7bac910dd89a50b74df13d6cf1b8fc
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak kaynak oluşturma sırasında otomatik olarak tanılama ayarlarını etkinleştirin
Bu makalede biz nasıl kullanabileceğinizi gösterir. bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md) oluşturulduğunda bir kaynakta tanılama ayarlarını yapılandırmak için. Bu, tanılama günlüklerini ve Event Hubs, bir depolama hesabında arşivleme veya bir kaynak oluşturulduğunda için günlük analizi göndererek ölçümlere akış otomatik olarak başlatılmasını sağlar.

Resource Manager şablonu kullanarak tanılama günlüklerini etkinleştirme yöntemi kaynak türüne bağlıdır.

* **İşlem olmayan** kaynakları (örneğin, ağ güvenlik grupları, Logic Apps Otomasyonu) kullanmak [tanılama bu makalede açıklanan ayarlarını](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **İşlem** (WAD/LAD tabanlı) kaynakları kullanmak [WAD/LAD yapılandırma dosyasını bu makalede açıklanan](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

Bu makalede her iki yöntemi kullanarak tanılama yapılandırma açıklanmaktadır.

Temel adımlar aşağıdaki gibidir:

1. Bir şablonu kaynak oluşturmak ve tanılama etkinleştirmek nasıl açıklayan bir JSON dosyası olarak oluşturun.
2. [Herhangi bir dağıtım yöntemi kullanarak şablonu dağıtmak](../azure-resource-manager/resource-group-template-deploy.md).

Aşağıdaki işlem dışı ve işlem kaynakları için oluşturmak için gereken şablon JSON dosyası örneği sunuyoruz.

## <a name="non-compute-resource-template"></a>İşlem olmayan kaynak şablonu
İşlem dışı kaynaklar için iki işlem yapmanız gerekir:

1. Depolama hesabı adı, olay hub'ı yetkilendirme kuralı kimliği ve/veya OMS günlük analizi çalışma alanı kimliği (tanılama günlüklerini arşivleme Event hubs'a günlükler akış ve/veya günlüğüne günlükleri göndermek bir depolama hesabında etkinleştirmek için parametreleri blob parametreleri ekleme Analytics).
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Azure Resource ID of the Log Analytics workspace for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Tanılama günlüklerini etkinleştirmek istediğiniz kaynak kaynakları dizisinde bir kaynak türü ekleyin `[resource namespace]/providers/diagnosticSettings`.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2017-05-01-preview",
        "properties": {
          "name": "[parameters('settingName')]",
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
          "eventHubName": "[parameters('eventHubName')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "category": "AllMetrics",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

Tanılama ayarını özellikleri blob izleyen [bu makalede açıklanan biçimde](https://docs.microsoft.com/rest/api/monitor/ServiceDiagnosticSettings/CreateOrUpdate). Ekleme `metrics` özelliği, sağlanan bu aynı çıktıları kaynak ölçümleri de göndermenizi etkinleştirecek [kaynak Azure İzleyici ölçümleri destekleyen](monitoring-supported-metrics.md).

Aşağıda, bir mantıksal uygulama oluşturan ve olay hub'ları ve depolama hesabındaki depolama akışı kapatır tam bir örnek verilmiştir.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2017-05-01-preview",
          "properties": {
            "name": "[parameters('settingName')]",
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Kaynak şablonu işlem
İşlem kaynak üzerinde tanılamayı etkinleştirmek için örneğin bir sanal makine ya da Service Fabric kümesi, gerekir:

1. Azure tanılama uzantısını VM kaynak tanımına ekleyin.
2. Bir depolama hesabı ve/veya olay hub'ı bir parametre olarak belirtin.
3. WADCfg XML dosyasının içeriğini tüm XML karakterleri düzgün kaçış XMLCfg özelliği ekleyin.

> [!WARNING]
> Bu son adım sağ almak zor olabilir. [Bu makaleye bakın](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) tanılama yapılandırma şeması kaçışlı ve doğru biçimlendirilmiş değişkenleri böler bir örnek.
> 
> 

Örnekler dahil olmak üzere tüm işlem açıklanan [bu belgedeki](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure tanılama günlükleri hakkında daha fazla bilgi](monitoring-overview-of-diagnostic-logs.md)
* [Akış olay hub'ları için Azure tanılama günlükleri](monitoring-stream-diagnostic-logs-to-event-hubs.md)

