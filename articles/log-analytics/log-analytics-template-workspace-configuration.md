---
title: Azure Resource Manager şablonları oluşturma ve bir günlük analizi çalışma alanı yapılandırılacağı | Microsoft Docs
description: Azure Resource Manager şablonları oluşturmak ve günlük analizi çalışma alanları yapılandırmak için kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: d21ca1b0-847d-4716-bb30-2a8c02a606aa
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: json
ms.topic: article
ms.date: 04/25/2018
ms.author: richrund
ms.openlocfilehash: 297f15430c64e5de3c10e6f38855664a50d11a8d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="manage-log-analytics-using-azure-resource-manager-templates"></a>Günlük analizi Azure Resource Manager şablonları kullanarak yönetme
Kullanabileceğiniz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) oluşturmak ve günlük analizi çalışma alanları yapılandırmak için. Şablonları ile gerçekleştirebileceğiniz görevler örnekleri şunlardır:

* Ayar fiyatlandırma katmanı da dahil olmak üzere çalışma alanı oluşturma 
* Çözüm ekleme
* Kaydedilen Aramalar oluşturun
* Bir bilgisayar grubu oluşturun
* Windows aracısının yüklü olduğu IIS günlüklerinin bilgisayarlardan toplamayı etkinleştir
* Linux ve Windows bilgisayarlarından performans sayaçlarını Topla
* Linux bilgisayarlarda syslog olaylarını Topla 
* Windows olay günlüklerini olaylarını Topla
* Bir Azure sanal makinesi için günlük analizi Aracısı Ekle
* Azure Tanılama'yı kullanarak toplanan dizin verileri için günlük analizi yapılandırın

Bu makale, bazı şablonları ile gerçekleştirebileceğiniz yapılandırmasının göstermek şablon örnekleri sağlar.

## <a name="create-a-log-analytics-workspace"></a>Günlük analizi çalışma alanı oluşturma
Aşağıdaki örnek, yerel makinenize şablonu kullanarak bir çalışma alanı oluşturur. JSON şablonunu çalışma alanı adı için yalnızca istemek için yapılandırılmış ve büyük olasılıkla ortamınızdaki standart bir yapılandırma olarak kullanılacak bir parametre için varsayılan bir değer belirtir.  

Aşağıdaki parametreler varsayılan bir değer ayarlayın:

* Konumu - Doğu ABD varsayılanlara
* SKU - varsayılan olarak Nisan 2018 serbest yeni GB başına fiyatlandırma katmanına fiyatlandırma modeli

>[!WARNING]
>Oluşturma veya günlük analizi çalışma alanındaki fiyatlandırma modeli yeni Nisan 2018 seçti bir abonelik yapılandırma, fiyatlandırma katmanı yalnızca geçerli günlük analizi ise **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Oluşturma ve şablon dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
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
          },
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
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
2. Gereksinimlerinizi karşılayacak şekilde şablonunu düzenleyin.  Gözden geçirme [Microsoft.OperationalInsights/workspaces şablonu](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) hangi özellikleri ve değerleri desteklenen öğrenmek için başvuru. 
3. Bu dosya olarak kaydetmek **deploylaworkspacetemplate.json** bir yerel klasöre.
4. Bu şablonu dağıtmaya hazırsınız. PowerShell veya komut satırı cretae çalışma alanı için kullanın.

   * PowerShell için şablonu içeren klasöründen aşağıdaki komutları kullanın:
   
        ```powershell
        New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
        ```

   * Komut satırı için şablonu içeren klasöründen aşağıdaki komutları kullanın:

        ```cmd
        azure config mode arm
        azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile deploylaworkspacetemplate.json
        ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:<br><br> ![Dağıtım tamamlandığında, örnek sonuç](./media/log-analytics-template-workspace-configuration/template-output-01.png)

## <a name="configure-a-log-analytics-workspace"></a>Günlük analizi çalışma alanı yapılandırın
Aşağıdaki şablonu örnek göstermektedir nasıl yapılır:

1. Çalışma Alanı'na çözümleri Ekle
2. Kaydedilen Aramalar oluşturun
3. Bir bilgisayar grubu oluşturun
4. Windows aracısının yüklü olduğu IIS günlüklerinin bilgisayarlardan toplamayı etkinleştir
5. Mantıksal Disk performans sayaçlarını Linux bilgisayarından toplar (% kullanılan Inode; Boş megabayt; % Kullanılan alan; Disk aktarımı/sn; Disk Okuma/sn; Disk Yazma/sn)
6. Linux bilgisayarlardan Syslog olaylarını Topla
7. Uygulama olay günlüğü'ndeki Windows bilgisayarlardan hata ve uyarı olaylarını Topla
8. Windows bilgisayarlardan bellek kullanılabilir MBayt performans sayacı Topla
9. IIS ve Windows olay günlükleri Azure altyapınıza bir depolama hesabına yazılan Topla

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "workspaceName"
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
      "metadata": {
        "description": "Service Tier: Free, Standalone, PerNode, or PerGB2018"
    }
      },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Free plans can only have 7 days, Standalone and OMS plans include 30 days for free"
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
      "apiVersion": "2017-03-15-preview",
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
          "apiVersion": "2017-03-15-preview",
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
          "apiVersion": "2015-11-01-preview",
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
### <a name="deploying-the-sample-template"></a>Örnek şablon dağıtma
Örnek şablonu dağıtmak için:

1. Ekli örnek bir dosyaya, örneğin kaydedin `azuredeploy.json` 
2. İstediğiniz yapılandırmasına sahip olmak şablonunu düzenleyin.
3. Şablonu dağıtmak için PowerShell veya komut satırını kullanın

#### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json
```

#### <a name="command-line"></a>Komut satırı
```cmd
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```

## <a name="example-resource-manager-templates"></a>Örnek Resource Manager şablonları
Azure hızlı başlangıç Şablon Galerisi dahil olmak üzere günlük analizi için çeşitli şablonlar içerir:

* [Günlük analizi VM uzantısı ile Windows çalıştıran bir sanal makine dağıtma](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Günlük analizi VM uzantısı ile Linux çalıştıran bir sanal makine dağıtma](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Azure Site Recovery varolan bir günlük analizi çalışma alanını kullanarak izleme](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Varolan bir günlük analizi çalışma alanını kullanarak Azure Web uygulamaları İzle](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Varolan bir depolama hesabı için OMS ekleme](https://azure.microsoft.com/resources/templates/oms-existing-storage-account/)

## <a name="next-steps"></a>Sonraki adımlar
* [Windows Aracısı Azure Resource Manager şablonu kullanarak sanal makineleri dağıtmak](../virtual-machines/windows/extensions-oms.md).
* [Azure Resource Manager şablonu kullanarak sanal makineleri Linux aracı dağıtım](../virtual-machines/linux/extensions-oms.md).

