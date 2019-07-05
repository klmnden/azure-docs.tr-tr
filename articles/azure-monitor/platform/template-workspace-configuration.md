---
title: Azure Resource Manager şablonları oluşturma ve Log Analytics çalışma alanı yapılandırma | Microsoft Docs
description: Azure Resource Manager şablonları oluşturmak ve Log Analytics çalışma alanları yapılandırmak için kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d21ca1b0-847d-4716-bb30-2a8c02a606aa
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: magoedte
ms.openlocfilehash: 39dbb504603544a468907d87d236338cb95e39a3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441624"
---
# <a name="manage-log-analytics-workspace-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Log Analytics çalışma alanını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Kullanabileceğiniz [Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-authoring-templates.md) oluşturmak ve Azure İzleyici'de Log Analytics çalışma alanları yapılandırmak için. Şablonlar ile gerçekleştirebileceğiniz görevler örnekleri şunlardır:

* Fiyatlandırma katmanını ayarlama dahil olmak üzere bir çalışma alanı oluşturma 
* Çözüm ekleme
* Kaydedilmiş arama oluştur
* Bir bilgisayar grubu oluşturun
* Windows aracısının yüklü olduğu IIS günlükler bilgisayarlardan koleksiyonunu etkinleştir
* Linux ve Windows bilgisayarlardan performans sayaçlarını Topla
* Linux Bilgisayarları'nda syslog olaylarını Topla 
* Windows olay günlüklerinden olaylarını Topla
* Bir Azure sanal makinesi için log analytics aracısını ekleme
* Azure Tanılama'yı kullanarak toplanan dizin verileri log analytics'e yapılandırın

Bu makalede, şablonları ile gerçekleştirebileceğiniz yapılandırma bazılarını göstermeyi şablonu örnekleri sağlar.

## <a name="api-versions"></a>API sürümleri
Aşağıdaki tabloda, bu örnekte kullanılan kaynaklar için API sürümü listeler.

| Resource | Kaynak türü | API sürümü |
|:---|:---|:---|
| Çalışma alanı   | Çalışma alanları    | 2017-03-15-Önizleme |
| Ara      | savedSearches | 2015-03-20 |
| Veri kaynağı | veri kaynakları   | 2015-11-01-Önizleme |
| Çözüm    | çözümler     | 2015-11-01-Önizleme |

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma
Aşağıdaki örnek, yerel makinenizden bir şablonu kullanarak bir çalışma alanı oluşturur. JSON şablonunu, çalışma alanının adı için yalnızca isteyecek şekilde yapılandırılmış ve büyük olasılıkla ortamınızdaki standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir.  

Aşağıdaki parametreleri varsayılan değeri ayarlayın:

* Konum - Doğu ABD için varsayılanları
* Nisan 2018'de yayınlanan yeni GB başına fiyatlandırma katmanına varsayılan fiyatlandırma modelinde SKU-

> [!NOTE]
>Oluşturma veya yeni Nisan 2018 fiyatlandırma modelini tercih bir Abonelikteki Log Analytics çalışma alanını yapılandırma, yalnızca geçerli Log Analytics fiyatlandırma katmanı ise **PerGB2018**.  
>Bazı abonelikler varsa, [pre-Nisan 2018 fiyatlandırma modeline](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs#new-pricing-model), belirtebileceğiniz **tek başına** fiyatlandırma katmanı ve bu başarılı olur pre-Nisan 2018 fiyatlandırma modelinde iki abonelik için ve yeni abonelikler için fiyatlandırma. Fiyatlandırma katmanını yeni proicing modeli benimseyen Aboneliklerde çalışma alanları için ayarlanacak **PerGB2018**. 

### <a name="create-and-deploy-template"></a>Şablon oluşturma ve dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```
2. Gereksinimlerinizi karşılayacak şekilde şablonunu düzenleyin.  Gözden geçirme [Microsoft.OperationalInsights/workspaces şablon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) başvuru hangi özellikler ve değerler desteklendiğini öğrenin. 
3. Bu dosyayı farklı Kaydet **deploylaworkspacetemplate.json** yerel bir klasöre.
4. Bu şablonu dağıtmaya hazırsınız. Çalışma alanı oluşturmak için PowerShell veya komut satırı'nı kullanın.

   * PowerShell için şablonu içeren klasörden aşağıdaki komutları kullanın:
   
        ```powershell
        New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
        ```

   * Komut satırı için şablonu içeren klasörden aşağıdaki komutları kullanın:

        ```cmd
        azure config mode arm
        azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile deploylaworkspacetemplate.json
        ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:<br><br> ![Dağıtım tamamlandığında örnek sonucu](./media/template-workspace-configuration/template-output-01.png)

## <a name="configure-a-log-analytics-workspace"></a>Log Analytics çalışma alanını yapılandırma
Aşağıdaki örnek şablonu göstermektedir nasıl yapılır:

1. Çözüm çalışma alanına ekleme
2. Kaydedilmiş arama oluştur
3. Bir bilgisayar grubu oluşturun
4. Windows aracısının yüklü olduğu IIS günlükler bilgisayarlardan koleksiyonunu etkinleştir
5. Mantıksal Disk performans sayaçları Linux bilgisayarından toplar (% kullanılan Inode'ları; Boş megabayt; % Kullanılan alan; Disk aktarımı/sn; Disk Okuma/sn; Disk Yazma/sn)
6. Linux bilgisayarlardan Syslog olaylarını Topla
7. Windows bilgisayarlardan uygulama olay günlüğü'ndeki hata ve uyarı olaylarını Topla
8. Windows bilgisayarlardan bellek kullanılabilir MBayt performans sayacı Topla
9. IIS günlükleri ve Azure tanılama tarafından bir depolama hesabına yazılan Windows olay günlüklerini Topla

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "Workspace name"
      }
    },
    "serviceTier": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Standalone",
        "PerNode",
        "PerGB2018"
      ],
      "defaultValue": "PerGB2018",
      "metadata": {
        "description": "Pricing tier: PerGB2018 or legacy tiers (Free, Standalone or PerNode) which are not available to all customers"
    }
      },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
      }
    },
    {
    "immediatePurgeDataOn30Days": {
      "type": "bool",
      "metadata": {
        "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. This only applies when retention is being set to 30 days."
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "East US",
        "West Europe",
        "Southeast Asia",
        "Australia Southeast"
      ]
    },
    "applicationDiagnosticsStorageAccountName": {
        "type": "string",
        "metadata": {
          "description": "Name of the storage account with Azure diagnostics output"
        }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
        "type": "string",
        "metadata": {
          "description": "The resource group name containing the storage account with Azure diagnostics output"
        }
    }
  },
  "variables": {
    "Updates": {
      "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "Updates"
    },
    "AntiMalware": {
      "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "AntiMalware"
    },
    "SQLAssessment": {
      "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "SQLAssessment"
    },
    "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-11-01-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "Name": "[parameters('serviceTier')]"
        },
    "retentionInDays": "[parameters('dataRetention')]"
      },
      "resources": [
        {
          "apiVersion": "2015-03-20",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "Category": "VMSS",
            "ETag": "*",
            "DisplayName": "VMSS Instance Count",
            "Query": "Event | where Source == \"ServiceFabricNodeBootstrapAgent\" | summarize AggregatedValue = count() by Computer",
            "Version": 1
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-03-20",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [ 
              "wad-iis-logfiles" 
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceName": {
      "type": "string",
      "value": "[parameters('workspaceName')]"
    },
    "provisioningState": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').provisioningState]"
    },
    "source": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').source]"
    },
    "customerId": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').customerId]"
    },
    "pricingTier": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').sku.name]"
    },
    "retentionInDays": {
      "type": "int",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').retentionInDays]"
    },
    "portalUrl": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').portalUrl]"
    }
  }
}

```
### <a name="deploying-the-sample-template"></a>Örnek şablonu dağıtma
Örnek şablonu dağıtmak için:

1. Bağlı örnek bir dosyaya, örneğin kaydedin `azuredeploy.json` 
2. İstediğiniz yapılandırmaya sahip bir şablon Düzenle
3. Şablonu dağıtmak için PowerShell veya komut satırı'nı kullanın

#### <a name="powershell"></a>PowerShell
```powershell
New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json
```

#### <a name="command-line"></a>Komut satırı
```cmd
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```

## <a name="example-resource-manager-templates"></a>Örnek Resource Manager şablonları
Azure hızlı başlangıç Şablon Galerisi de dahil olmak üzere Log Analytics için birkaç şablon içerir:

* [Log Analytics VM uzantısı ile Windows çalıştıran bir sanal makine dağıtma](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Log Analytics VM uzantısı ile Linux çalıştıran bir sanal makine dağıtma](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Mevcut bir Log Analytics çalışma alanını kullanarak Azure Site Recovery izleme](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Mevcut bir Log Analytics çalışma alanını kullanarak Azure Web uygulamalarını izleme](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Mevcut bir depolama hesabı için Log Analytics Ekle](https://azure.microsoft.com/resources/templates/oms-existing-storage-account/)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Resource Manager şablonu kullanarak sanal makinelere Windows aracısını dağıtmak](../../virtual-machines/extensions/oms-windows.md).
* [Linux aracısını dağıtmak için Azure Resource Manager şablonu kullanarak Vm'leri](../../virtual-machines/extensions/oms-linux.md).
