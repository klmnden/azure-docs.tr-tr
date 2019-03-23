---
title: Otomatik olarak bir Resource Manager şablonu kullanarak tanılama ayarlarını etkinleştirme
description: Resource Manager şablonu, tanılama günlüklerinin Event Hubs'a akış veya bunları bir depolama hesabında depolamanıza olanak tanıyacaktır tanılama ayarları oluşturmak için kullanmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/26/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 7edce5175a1dda66abf3316cb8f0eb33e9f64ef7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371478"
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Otomatik olarak bir Resource Manager şablonu kullanarak kaynak oluşturma sırasında tanılama ayarlarını etkinleştirme
Bu makalede size nasıl kullanabileceğinizi gösterir. bir [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md) oluşturulduğunda kaynak tanılama ayarlarını yapılandırmak için. Bu, otomatik olarak tanılama günlükleri ve ölçümleri Event hubs'a akış, bunları bir depolama hesabına arşivleme veya bir kaynak oluşturulduğunda bir Log Analytics çalışma alanına göndererek başlatmanıza olanak sağlar.

> [!WARNING]
> Depolama hesabındaki günlük verilerinin biçimi, 1 Kasım 2018 tarihinde JSON Satırları olarak değişecektir. [Etkinin açıklaması ve yeni biçimi işlemek üzere araçlarınızı güncelleştirme için bu makaleye bakın.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

Resource Manager şablonu kullanarak tanılama günlüklerini etkinleştirme yöntemini kaynak türüne bağlıdır.

* **İşlem dışı** kaynakları (örneğin, ağ güvenlik grupları, Logic Apps, Otomasyon) [bu makalede açıklanan tanılama ayarları](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
* **İşlem** (WAD/LAD tabanlı) kaynakları [WAD/LAD yapılandırma dosyası bu makalede açıklanan](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

Bu makalede şu yöntemlerden birini kullanarak tanılama yapılandırma açıklanmaktadır.

Temel adımlar aşağıdaki gibidir:

1. Bir şablon kaynağı oluşturma ve tanılamayı etkinleştirme açıklayan bir JSON dosyası olarak oluşturun.
2. [Herhangi bir dağıtım yöntemi kullanarak şablonu dağıtmak](../../azure-resource-manager/resource-group-template-deploy.md).

Aşağıda bir örnek işlem olmayan ve işlem kaynakları için oluşturmak istediğiniz şablon JSON dosyasının sunuyoruz.

## <a name="non-compute-resource-template"></a>Kaynak dışı işlem şablonu
İşlem olmayan kaynaklar için iki işlem gerçekleştirmesi gerekir:

1. Parametre blob depolama hesabı adı, olay hub'ı yetkilendirme kuralı kimliği ve/veya Log Analytics çalışma alanı kimliği (tanılama günlüklerini arşivleme günlüklerinin Event Hubs'a akışını ve/veya günlükleri göndermek için Azure İzleyici bir depolama hesabında etkinleştirme) için parametreleri ekleyin.
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
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
2. Tanılama günlüklerini etkinleştirmek istediğiniz kaynak kaynakları dizi türünde bir kaynak ekleyin `[resource namespace]/providers/diagnosticSettings`.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
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

Aşağıdaki özellikler blob için tanılama ayarını [bu makalede açıklanan biçimde](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate). Ekleme `metrics` özelliği de kaynak ölçümleri koşuluyla bu aynı çıktıları göndermek edebileceksiniz [kaynak Azure İzleyici ölçümleri destekliyor](../../azure-monitor/platform/metrics-supported.md).

Mantıksal uygulama oluşturan ve olay hub'larına ve bir depolama hesabında depolama akışı kapatır tam bir örnek aşağıda verilmiştir.

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
      "defaultValue": "https://azure.microsoft.com/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
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
          "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
          "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
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
Bir işlem kaynağı tanılamayı etkinleştirmek için örneğin bir sanal makine veya Service Fabric kümesi, gerekir:

1. Azure tanılama uzantısı, VM kaynak tanımına ekleyin.
2. Bir parametre olarak bir depolama hesabı ve/veya olay hub'ı belirtin.
3. WADCfg XML dosyasının içeriği doğru tüm XML karakterleri kaçış XMLCfg özelliğindeki ekleyin.

> [!WARNING]
> Bu son adım, doğru almak zor olabilir. [Bu makaleye bakın](../../virtual-machines/extensions/diagnostics-template.md#diagnostics-configuration-variables) kaçış ve düzgün biçimlendirilmiş değişkenlere tanılama yapılandırma şeması ayıran bir örnek.
> 
> 

Örnekler de dahil olmak üzere sürecin tamamı açıklanan [bu belgedeki](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Tanılama Günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Stream Event hubs'a, Azure tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)


