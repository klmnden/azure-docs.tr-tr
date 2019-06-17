---
title: Azure İzleyici günlükler için ölçüm uyarıları oluşturma
description: Popüler log analytics verileri neredeyse gerçek zamanlı ölçüm uyarıları oluşturma Öğreticisi.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 1c744e0063d5c56b2ca17f2b6c6fa694ad13a26c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64872570"
---
# <a name="create-metric-alerts-for-logs-in-azure-monitor"></a>Azure İzleyici günlükler için ölçüm uyarıları oluşturma

## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyicisi'ni destekleyen [ölçüm uyarı türü](../../azure-monitor/platform/alerts-metric-near-real-time.md) üzerinden avantajları olduğu [Klasik uyarılar](../../azure-monitor/platform/alerts-classic-portal.md). Ölçümler kullanılabilir [Azure hizmetlerinin büyük listesi](../../azure-monitor/platform/metrics-supported.md). Bu makalede (yani) bir alt kümesi için kaynak - kullanımını açıklar `Microsoft.OperationalInsights/workspaces`.

Ölçümleriniz Azure veya şirket içi kaynaklar dahil olmak üzere günlüklerinden ölçümleri bir parçası olarak ayıklanan popüler Log Analytics günlükleri ile ilgili ölçüm Uyarıları'nı kullanabilirsiniz. Desteklenen Log Analytics çözümleri aşağıda listelenmiştir:

- [Performans sayaçları](../../azure-monitor/platform/data-sources-performance-counters.md) Windows ve Linux makineler için
- [Aracı sistem durumu sinyal kayıtları](../../azure-monitor/insights/solution-agenthealth.md)
- [Güncelleştirme yönetimi](../../automation/automation-update-management.md) kayıtları
- [Olay verilerini](../../azure-monitor/platform/data-sources-windows-events.md) günlükleri

Kullanmak için birçok faydası vardır **günlükleri için ölçüm uyarıları** sorgu tabanlı üzerinden [günlük uyarıları](../../azure-monitor/platform/alerts-log.md) Azure'da; bunlardan bazıları aşağıda listelenmiştir:

- Ölçüm uyarıları neredeyse gerçek zamanlı günlük kaynak aynı emin olmak için günlükleri çatalları verileri için özellik ve ölçüm uyarıları izleme sunar.
- Ölçüm uyarıları bilgisi - yalnızca sonra ne zaman uyarı tetiklenir ve ne zaman uyarı çözümlenen bildiren yok; Günlük uyarıları aksine, durum bilgisiz olduğundan ve uyarı koşul karşılanıyorsa her aralıkta tetikleme tutun.
- Birden çok boyutta günlüğü için ölçüm uyarıları sağlar ve böylece bilgisayarların, işletim sistemi türü, basit; vb. gibi belirli değerleri filtreleme analytics'te sorgu penning ihtiyacı.

> [!NOTE]
> Özel ölçüm ve/veya boyut yalnızca veriler için seçilen süre içinde olup olmadığını gösterilir. Bu ölçümler, Azure Log Analytics çalışma alanına sahip müşteriler için kullanılabilir.

## <a name="metrics-and-dimensions-supported-for-logs"></a>Ölçüm ve günlükleri için desteklenen boyutlar

 Ölçüm uyarıları boyutlar kullanmak için ölçümleri uyarı destekler. Ölçümünüzün doğru düzeyine filtrelemek için boyutları kullanabilirsiniz. Günlüklerinden desteklenen ölçümler tam listesini [Log Analytics çalışma alanları](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces) listelenir; çözümleri arasında desteklenir.

> [!NOTE]
> Log Analytics çalışma ayıklanırken için desteklenen ölçümleri görüntülemek için [Azure İzleyici - ölçümler](../../azure-monitor/platform/metrics-charts.md); ölçüm bir günlük için söz konusu ölçüm oluşturulması için uyar. Ölçüm uyarısı için günlükleri - seçilen boyutları görünmesi için Azure İzleyici - ölçümler aracılığıyla araştırma.

## <a name="creating-metric-alert-for-log-analytics"></a>Log Analytics için ölçüm uyarısı oluşturma

Log Analytics'te Azure İzleyici - ölçümler ile işlenmeden önce popüler günlüklerinden ölçüm verileri cmdlet'iyle yönetilir. Bu, kullanıcıların ölçüm uyarısı - 1 dakika düşük sıklığa sahip uyarılar dahil olmak üzere yanı sıra ölçüm platform özelliklerinden yararlanmak sağlar.
Aşağıda listelenen günlükleri için ölçüm uyarısı kaynaklı araçlarıdır.

## <a name="prerequisites-for-metric-alert-for-logs"></a>Günlükler için ölçüm uyarısı için Önkoşullar

Log Analytics veri çalışır Ölçüm günlükleri için toplanan önce aşağıdaki yukarı ve kullanılabilir ayarlanmalıdır:

1. **Etkin bir Log Analytics çalışma alanı**: Geçerli ve etkin Log Analytics çalışma alanı mevcut olması gerekir. Daha fazla bilgi için [Azure portalında Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md).
2. **Aracı için Log Analytics çalışma alanı yapılandırılmış**: Aracının önceki adımda kullanılan Log Analytics çalışma alanına Azure Vm'leri (veya) şirket içi Vm'leri için veri göndermek yapılandırılmış olması gerekir. Daha fazla bilgi için [Log Analytics - Aracısı genel bakış](../../azure-monitor/platform/agents-overview.md).
3. **Desteklenen Log Analytics çözümleri yüklü**: Log Analytics çözümü, yapılandırılmış ve gönderen verileri Log Analytics çalışma alanına - desteklenen olmalıdır çözümler [Windows ve Linux için performans sayaçları](../../azure-monitor/platform/data-sources-performance-counters.md), [aracı sistem durumu sinyal kayıtlarını](../../azure-monitor/insights/solution-agenthealth.md) , [Güncelleştirme yönetimi](../../automation/automation-update-management.md), ve [olay verilerini](../../azure-monitor/platform/data-sources-windows-events.md).
4. **Günlükleri göndermek için yapılandırılmış analiz çözümleri oturum**: Log Analytics çözümü, gerekli günlükleri/veri karşılık gelen olmalıdır [Log Analytics çalışma alanları için desteklenen ölçümler](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces) etkin. Örneğin, *% kullanılabilir bellek* sayacı bunu yapılandırılmalıdır [performans sayaçları](../../azure-monitor/platform/data-sources-performance-counters.md) çözüm ilk.

## <a name="configuring-metric-alert-for-logs"></a>Yapılandırma günlükleri için ölçüm Uyarısı

 Ölçüm uyarıları oluşturulabilir ve Resource Manager şablonları, REST API, PowerShell ve Azure CLI Azure portalını kullanarak yönetilen. Günlükleri için ölçüm uyarıları olduğundan bir değişken ölçüm uyarıları - önkoşulları tamamladıktan sonra günlükler için ölçüm uyarısı için belirtilen Log Analytics çalışma alanı oluşturulabilir. Tüm özelliklerini ve işlevlerini [ölçüm uyarıları](../../azure-monitor/platform/alerts-metric-near-real-time.md) yükü şema, geçerli bir kota sınırları ve faturalandırılan fiyat; günlükleri de için ölçüm uyarıları için geçerli olacaktır.

Adım adım ayrıntıları ve örnekleri - bakın [oluşturma ve ölçüm Uyarıları yönetme](https://aka.ms/createmetricalert). Özellikle, günlükleri için-ölçüm uyarıları için ölçüm Uyarıları yönetmek için yönergeleri izleyin ve aşağıdakilerden emin olun:

- Geçerli bir ölçüm uyarısı için hedef *Log Analytics çalışma alanı*
- Seçilen ölçüm uyarısı için seçilen sinyal *Log Analytics çalışma alanı* türünde **ölçüm**
- Belirli koşullar veya boyut filtreleri kullanarak kaynak filtresi; çok boyutlu ölçümler için günlükleri
- Yapılandırma sırasında *sinyal mantığını*, yayılmasını boyut (bilgisayar gibi) birden çok değeri tek bir uyarı oluşturulabilir.
- Varsa **değil** seçilen için ölçüm uyarısı oluşturmak için Azure portalını kullanarak *Log Analytics çalışma alanı*; ardından el ile kullanıcı gerekir ilk kullanarakbirölçümgünlükverileridönüştürmekiçinaçıkbirkuraloluşturun[Azure İzleyici - zamanlanmış sorgu kuralları](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules).

> [!NOTE]
> Azure portalı - Log Analytics çalışma alanı için ölçüm uyarısı oluşturma sırasında ölçüm günlük verileri dönüştürme kuralı karşılık gelen [Azure İzleyici - zamanlanmış sorgu kuralları](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) arka planda otomatik olarak oluşturulan  *herhangi bir eylem veya kullanıcı müdahalesi gerektirmeden*. Azure portal dışındaki yollardan günlükleri oluşturma için ölçüm uyarısı için bkz: [günlükleri için ölçüm uyarıları için kaynak şablonu](#resource-template-for-metric-alerts-for-logs) bölümünde ölçüm uyarısı önce ölçüm dönüştürme kuralı için bir temel ScheduledQueryRule günlük oluşturma örnek anlamına gelir oluşturulan günlüklerde ölçüm uyarısı için hiçbir veri olacaktır oluşturma - else.

## <a name="resource-template-for-metric-alerts-for-logs"></a>Kaynak şablonu için günlükleri için ölçüm uyarıları

Daha önce belirtildiği gibi ölçüm uyarıları günlüklerinden oluşturulması için iki yönlü bir işlemdir:

1. Ölçümleri scheduledQueryRule API'sini kullanarak desteklenen günlüklerinden ayıklanacağı bir kural oluşturun
2. Günlüğünde (adım 1) ayıklanan ölçüm için ölçüm uyarısı oluşturma ve bir hedef kaynak olarak Log Analytics çalışma alanı

### <a name="metric-alerts-for-logs-with-static-threshold"></a>Statik eşik ile günlükler için ölçüm uyarıları

Aynı şeyi elde etmek için bir Azure Resource Manager şablonu aşağıdaki - örnek statik eşiği ölçüm uyarısı oluşturma ölçümleri günlüklerinden scheduledQueryRule aracılığıyla ayıklamak için kural oluşturma başarılı yere bağlıdır kullanabilirsiniz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Söyleyin. Yukarıdaki JSON metricfromLogsAlertStatic.json - kaydedildikten sonra kaynak tabanlı şablon oluşturma için parametre bir JSON dosyası ile birlikte. Bir örnek parametre JSON dosyası aşağıda verilmiştir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule" 
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterThan"
        },
        "threshold":{
            "value": "1"
        },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

Yukarıdaki parametre dosyası varsayılarak metricfromLogsAlertStatic.parameters.json kaydedilir; kullanarak günlükleri için ölçüm uyarısı oluşturmayı seçebilirsiniz sonra [kaynak şablonu Azure portalında oluşturma](../../azure-resource-manager/resource-group-template-deploy-portal.md).

Alternatif olarak, bir Azure Powershell komutunu aşağıdaki de kullanabilirsiniz:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertStatic.json TemplateParameterFile metricfromLogsAlertStatic.parameters.json
```

Veya kullanım kaynak Azure CLI kullanarak şablonu dağıtın:

```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlertStatic.json --parameters @metricfromLogsAlertStatic.parameters.json
```

### <a name="metric-alerts-for-logs-with-dynamic-thresholds"></a>Dinamik eşikler ile günlükler için ölçüm uyarıları

Aynı şeyi elde etmek için bir Azure Resource Manager şablonu aşağıdaki - örnek burada ölçümleri günlüklerinden scheduledQueryRule aracılığıyla ayıklamak için kural oluşturma başarılı dinamik eşikler ölçüm uyarısı oluşturulmasını bağlıdır kullanabilirsiniz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterOrLessThan",
            "allowedValues": [
                "GreaterThan",
                "LessThan",
                "GreaterOrLessThan"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "alertSensitivity": {
            "type": "string",
            "defaultValue": "Medium",
            "allowedValues": [
                "High",
                "Medium",
                "Low"
            ],
            "metadata": {
                "description": "Tunes how 'noisy' the Dynamic Thresholds alerts will be: 'High' will result in more alerts while 'Low' will result in fewer alerts."
            }
        },
        "numberOfEvaluationPeriods": {
            "type": "string",
            "defaultValue": "4",
            "metadata": {
                "description": "The number of periods to check in the alert evaluation."
            }
        },
        "minFailingPeriodsToAlert": {
            "type": "string",
            "defaultValue": "3",
            "metadata": {
                "description": "The number of unhealthy periods to alert on (must be lower or equal to numberOfEvaluationPeriods)."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.MultipleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "criterionType": "DynamicThresholdCriterion",
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "alertSensitivity": "[parameters('alertSensitivity')]",
                            "failingPeriods": {
                                "numberOfEvaluationPeriods": "[parameters('numberOfEvaluationPeriods')]",
                                "minFailingPeriodsToAlert": "[parameters('minFailingPeriodsToAlert')]"
                            },
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Söyleyin. Yukarıdaki JSON metricfromLogsAlertDynamic.json - kaydedildikten sonra kaynak tabanlı şablon oluşturma için parametre bir JSON dosyası ile birlikte. Bir örnek parametre JSON dosyası aşağıda verilmiştir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule"
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterOrLessThan"
          },
          "alertSensitivity": {
              "value": "Medium"
          },
          "numberOfEvaluationPeriods": {
              "value": "4"
          },
          "minFailingPeriodsToAlert": {
              "value": "3"
          },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

Yukarıdaki parametre dosyası varsayılarak metricfromLogsAlertDynamic.parameters.json kaydedilir; kullanarak günlükleri için ölçüm uyarısı oluşturmayı seçebilirsiniz sonra [kaynak şablonu Azure portalında oluşturma](../../azure-resource-manager/resource-group-template-deploy-portal.md).

Alternatif olarak, bir Azure Powershell komutunu aşağıdaki de kullanabilirsiniz:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertDynamic.json TemplateParameterFile metricfromLogsAlertDynamic.parameters.json
```

Veya kullanım kaynak Azure CLI kullanarak şablonu dağıtın:

```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlertDynamic.json --parameters @metricfromLogsAlertDynamic.parameters.json
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [ölçüm uyarıları](alerts-metric.md).
- Hakkında bilgi edinin [günlük uyarıları Azure'da](../../azure-monitor/platform/alerts-unified-log.md).
- Hakkında bilgi edinin [azure'daki uyarıları](alerts-overview.md).
