---
title: Resource Manager şablonu ile günlük uyarısı oluşturma
description: Bir Azure Resource Manager şablonu ve API kullanarak günlük uyarı oluşturmayı öğrenin.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 5afa34a5eadf5367b3ab28749735197ca6ed82bd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263210"
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Resource Manager şablonu ile günlük uyarısı oluşturma
Bu makalede nasıl yönetebileceğiniz gösterilmektedir [oturum uyarıları](monitor-alerts-unified-log.md) Azure kullanarak program aracılığıyla ölçekli olarak [Azure Resource Manager şablonu](..//azure-resource-manager/resource-group-authoring-templates.md) aracılığıyla [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) ve [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md). Azure uyarıları destekler sorgularından uyarılar şu anda oturum [Azure günlük analizi](../log-analytics/log-analytics-tutorial-viewdata.md) ve [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Günlük analizi günlük uyarıdaki yönetme
Günlük uyarı için [Azure günlük analizi](../log-analytics/log-analytics-tutorial-viewdata.md) içine tümleştirilmiştir [yeni Azure uyarılar deneyimi](monitoring-overview-unified-alerts.md); hala günlük analizi API'leri devre dışı çalıştırır ve Uyumluluk yönetmekiçindahaöncekullanılanşemasıkaldığı[OMS portalında uyarılar](..//log-analytics/log-analytics-alerts-creating.md).

> [!NOTE]
> 14 Mayıs 2018 başlayan bir çalışma alanındaki tüm uyarıları, Azure'da genişletmek otomatik olarak başlatılacak. Bir kullanıcı gönüllü 14 Mayıs 2018 önce Azure genişletme uyarıları başlatabilir. Daha fazla bilgi için bkz: [genişletmek uyarıları OMS Azure içine](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak
Günlük analizi için günlük uyarıları, düzenli aralıklarla kaydedilmiş bir aramayı çalıştırma uyarı kuralları tarafından oluşturulur. Sorgu eşleşmesinin sonuçlarını ölçütleri belirtilmişse, bir uyarı kaydı oluşturulur ve bir veya daha fazla eylem çalıştırın. 

Kaynak şablonu için [günlük analizi kaydedilen arama](../log-analytics/log-analytics-log-searches.md) ve [oturum analytics uyarıları](../log-analytics/log-analytics-alerts.md) günlük analizi belgeleri bölümünde kullanılabilir. Hakkında daha fazla bilgi edinin [ekleme günlük analizi Kaydedilmiş aramaları ve Uyarıları](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md); yalnızca tanım örnekleri ve bunun yanı sıra şema ayrıntılarını içerir.

### <a name="using-resource-template-via-apipowershell"></a>Kaynak şablonu API/Powershell aracılığıyla kullanarak
Günlük analizi uyarı REST API RESTful ve Azure Resource Manager REST API'si erişilebilir. API, böylece bir PowerShell komut satırından erişilebilir ve arama sonuçlarını, JSON biçiminde sonuçları birçok farklı yolla programlı olarak kullanmanıza olanak sağlayan çıktı.

Daha fazla bilgi edinmek [oluşturma ve uyarı kurallarında günlük analizi REST API ile yönetme](../log-analytics/log-analytics-api-alerts.md); Powershell'den API'sine erişim örnekleri dahil olmak üzere.

## <a name="managing-log-alert-on-application-insights"></a>Application Insights günlük uyarıdaki yönetme
Azure Application Insights için günlük uyarıları, yeni Azure uyarılar Azure İzleyicisi altında bir parçası olarak tanıtılmıştır. Bu nedenle Azure İzleyici API'SİYLE altında çalıştığı [zamanlanmış sorgu kuralları](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) REST işlemi grubu.

### <a name="using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak
Günlük uyarı Application Insights kaynaklar için bir tür olan `Microsoft.Insights/scheduledQueryRules/`. Bu kaynak türü hakkında daha fazla bilgi için bkz: [Azure İzleyicisi - zamanlanmış sorgu kuralları API Başvurusu](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/).

Bir yapıdır aşağıdaki [zamanlanmış sorgu kuralları oluşturma](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/createorupdate) örnek veri kümesi değişkenleri olarak kaynak şablonu tabanlı.

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
> Etiket alanı bağlantıyla gizli-hedef kaynak kullanımını zorunlu [zamanlanmış sorgu kuralları ](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) API çağrısı veya kaynak şablonu. 

Yukarıdaki örnek json (deyin) sampleScheduledQueryRule.json bu kılavuzda amacıyla olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

### <a name="using-resource-template-via-clipowershell"></a>Kaynak şablonu CLI/Powershell aracılığıyla kullanarak
Azure - zamanlanmış sorgu kuralları API REST API ve Azure Resource Manager REST API ile tamamen uyumlu izleyicisidir. Bu nedenle Resource Manager cmdlet'i ve bunun yanı sıra Azure CLI kullanarak Powershell yoluyla kullanılabilir.

Örnek kaynak daha önce gösterilen şablonu (sampleScheduledQueryRule.json) için Azure Resource Manager PowerShell cmdlet'i aracılığıyla kullanım aşağıda gösterilmiştir:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Örnek kaynak daha önce gösterilen şablonu (sampleScheduledQueryRule.json) için Azure CLI, Azure Resource Manager komutu ile kullanım aşağıda gösterilmiştir:

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Başarılı bir işlem 201 durum yeni uyarı kuralı oluşturmayı döndürülecek veya varolan bir uyarı kuralı değiştirilirse 200 döndürülür.


## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [günlük uyarılar için Web kancası eylemleri](monitor-alerts-unified-log-webhook.md)
* Yeni hakkında bilgi edinin [Azure uyarıları](monitoring-overview-unified-alerts.md)
* Daha fazla bilgi edinmek [Application Insights](../application-insights/app-insights-analytics.md)
* Daha fazla bilgi edinmek [günlük analizi](../log-analytics/log-analytics-overview.md).   
