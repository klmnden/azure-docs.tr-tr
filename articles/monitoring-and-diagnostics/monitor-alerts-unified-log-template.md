---
title: Resource Manager şablonu ile günlük uyarısı oluşturma
description: Bir Azure Resource Manager şablonu ve API'yı kullanarak günlük uyarı oluşturma hakkında bilgi edinin.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 588a0686eda1966582b82a4673a8b6805453c94c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441451"
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Resource Manager şablonu ile günlük uyarısı oluşturma
Bu makalede nasıl yönetebileceğinizi gösterir [günlük uyarıları](monitor-alerts-unified-log.md) ölçekte Azure kullanarak program aracılığıyla [Azure Resource Manager şablonu](..//azure-resource-manager/resource-group-authoring-templates.md) aracılığıyla [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) ve [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md). Şu anda Azure uyarıları destekleyen sorgularından uyarılarda oturum [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ve [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Log Analytics günlük uyarı yönetme
Günlük Uyarı [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) içine tümleştirilmiştir [deneyimi yeni Azure uyarıları](monitoring-overview-unified-alerts.md); yine de Log Analytics API'leri çalıştırır ve yönetmekiçindahaöncekullanılanşemaileuyumlulukkalır[uyarılar OMS portalında](..//log-analytics/log-analytics-alerts-creating.md).

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren bir çalışma alanındaki tüm uyarıları Azure'a genişletmek otomatik olarak başlar. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [genişletmek uyarıları oms'den azure'a](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma
Günlük uyarıları için Log Analytics kayıtlı bir aramayı düzenli aralıklarla çalıştıran uyarı kuralları tarafından oluşturulur. Sorgu eşleşmenin sonuçlarını ölçütleri belirtilirse, bir uyarı kaydı oluşturulur ve bir veya daha fazla Eylemler çalıştırılır. 

Log analytics kayıtlı aramanız ve Log analytics alertsare kullanılabilir belgeler bölümünde Log Analytics kaynak şablonu. Bkz: daha fazla bilgi edinmek için [ekleme Log Analytics kayıtlı aramaları ve Uyarıları](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md); yalnızca tanım örnekleri ve bunun yanı sıra şema ayrıntılarını içerir.

### <a name="using-resource-template-via-apipowershell"></a>Kaynak şablonu API/Powershell aracılığıyla kullanarak
Log Analytics uyarı REST API, RESTful olduğu ve Azure Resource Manager REST API aracılığıyla erişilebilir. API, böylece bir PowerShell komut satırından erişilebilir ve sonuçları programlı olarak birçok farklı şekilde kullanmanıza olanak sağlayan, arama sonuçları JSON biçiminde çıkarır.

Daha fazla bilgi edinin [oluşturmak ve REST API ile Log analytics'teki uyarı kuralları yönetmek](../log-analytics/log-analytics-api-alerts.md)Powershell'den API'sine erişim örnekler dahil.

## <a name="managing-log-alert-on-application-insights"></a>Application ınsights günlük uyarı yönetme
Azure Application Insights için günlük uyarıları, Azure İzleyici altında yeni Azure uyarıları bir parçası olarak tanıtılmıştır. Bu nedenle Azure İzleyici API altında çalıştığı [zamanlanmış sorgu kuralları](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) REST işlem grubu.

### <a name="using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma
Application Insights kaynakları için günlük uyarı sahip bir tür `Microsoft.Insights/scheduledQueryRules/`. Bu kaynak türü hakkında daha fazla bilgi için bkz. [Azure İzleyici - zamanlanmış sorgu kuralları API Başvurusu](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/).

Aşağıdakiler için yapısıdır [zamanlanmış sorgu kuralı oluşturma](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/createorupdate) değişkenleri olarak örnek veri kümesiyle kaynak şablonu temel.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
        "Query":"requests",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "aznsAction":{
               "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]"
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> Hedef kaynak için gizli-bağlantı etiketi alanıyla kullanımını zorunlu [zamanlanmış sorgu kuralları ](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) API çağrısı veya kaynak şablonu. 

Yukarıdaki örnek json sampleScheduledQueryRule.json amacıyla bu izlenecek yolda (örneğin) olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

### <a name="using-resource-template-via-clipowershell"></a>Kaynak şablon CLI/Powershell aracılığıyla kullanarak
Azure İzleyici - zamanlanmış sorgu kuralları REST API ve Azure Resource Manager REST API'si ile tamamen uyumlu API'dir. Bu nedenle Azure CLI yanı sıra, Resource Manager cmdlet'ini kullanarak Powershell kullanılabilir.

Kaynak şablonu (sampleScheduledQueryRule.json) daha önce gösterilen örnek için Azure Resource Manager PowerShell cmdlet'i aracılığıyla kullanım aşağıda gösterilmektedir:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Aşağıda Azure Resource Manager kaynak şablonu (sampleScheduledQueryRule.json) daha önce gösterilen örnek için Azure CLI komutu aracılığıyla kullanımı gösterilmektedir:

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Başarılı bir işlem 201 durum yeni uyarı kuralı oluşturma döndürülecek veya mevcut bir uyarı kuralı değiştirilmişse, 200 döndürülür.


## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [günlük uyarıları için Web kancası eylemleri](monitor-alerts-unified-log-webhook.md)
* Yeni hakkında bilgi edinin [Azure uyarıları](monitoring-overview-unified-alerts.md)
* Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md)
* Daha fazla bilgi edinin [Log Analytics](../log-analytics/log-analytics-overview.md).   
