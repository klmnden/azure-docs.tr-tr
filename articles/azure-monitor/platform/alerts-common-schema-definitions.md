---
title: Web kancaları/Logic Apps/Azure işlevleri için genel uyarı şema tanımları
description: Web kancaları/Logic Apps/Azure işlevleri için genel uyarı şema tanımlarının anlama
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 0ca9d63f62de6a0b2385b3fb9651c34379b846d7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59010405"
---
# <a name="common-alert-schema-definitions"></a>Ortak uyarı şeması tanımları

Bu makalede [ortak uyarı şema tanımları](https://aka.ms/commonAlertSchemaDocs) Web kancaları/Logic Apps/Azure işlevleri için. 

## <a name="overview"></a>Genel Bakış

Herhangi bir uyarı örneği açıklar **etkilenen kaynak** ve **uyarının nedenini**, ve bu örnekler, ortak şema aşağıdaki bölümlerde açıklanmıştır:
* **Essentials**: Bir dizi **standart alanlar**açıklayan tüm uyarı türleri arasında ortak **hangi kaynak** uyarı ek ortak uyarı meta verileri birlikte (örneğin, önem derecesi veya açıklama) belirtir. 
* **Uyarı bağlamı**: Bir dizi tanımlayan alanları **uyarının neden**, farklı alanlara sahip **uyarı türüne göre**. Örneğin, bir etkinlik günlüğü uyarısı uyarının olayla ilgili bilgileri yoktur ancak bir ölçüm uyarısı uyarı bağlamındaki ölçüm değeri ve ölçüm adı gibi alanlarını gerekir. 

##### <a name="sample-alert-payload"></a>Örnek uyarı yükü
```json
{
  "schemaId": "azureMonitorCommonAlertSchema",
  "data": {
    "essentials": {
      "alertId": "/subscriptions/<subscription ID>/providers/Microsoft.AlertsManagement/alerts/b9569717-bc32-442f-add5-83a997729330",
      "alertRule": "WCUS-R2-Gen2",
      "severity": "Sev3",
      "signalType": "Metric",
      "monitorCondition": "Resolved",
      "monitoringService": "Platform",
      "alertTargetIDs": [
        "/subscriptions/<subscription ID>/resourcegroups/pipelinealertrg/providers/microsoft.compute/virtualmachines/wcus-r2-gen2"
      ],
      "originAlertId": "3f2d4487-b0fc-4125-8bd5-7ad17384221e_PipeLineAlertRG_microsoft.insights_metricAlerts_WCUS-R2-Gen2_-117781227",
      "firedDateTime": "2019-03-22T13:58:24.3713213Z",
      "resolvedDateTime": "2019-03-22T14:03:16.2246313Z",
      "description": "",
      "essentialsVersion": "1.0",
      "alertContextVersion": "1.0"
    },
    "alertContext": {
      "properties": null,
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Percentage CPU",
            "metricNamespace": "Microsoft.Compute/virtualMachines",
            "operator": "GreaterThan",
            "threshold": "25",
            "timeAggregation": "Average",
            "dimensions": [
              {
                "name": "ResourceId",
                "value": "3efad9dc-3d50-4eac-9c87-8b3fd6f97e4e"
              }
            ],
            "metricValue": 7.727
          }
        ]
      }
    }
  }
}
```

## <a name="essentials-fields"></a>'Temel' alanları

| Alan | Açıklama|
|:---|:---|
| Alertıd | Uyarı örneği benzersiz olarak tanımlayan GUID. |
| alertRule | Uyarı örneği oluşturulan bir uyarı kuralı adı. |
| Severity | Uyarının önem derecesi. Olası değerler: Sev0, Sev1, Sev2, Sev3, Sev4 |
| signalType | Uyarı kuralı tanımlı sinyali tanımlar. Olası değerler: Ölçüm, günlük, etkinlik günlüğü |
| monitorCondition | Bir uyarı tetiklendiğinde uyarı izleme koşulu 'Fired' için ayarlanmış. Uyarıya ateşlenmesine neden altta yatan durumun temizlediğinde, izleme koşulu 'Çözümlendi' olarak ayarlanır.   |
| monitoringService | İzleme hizmeti veya uyarıyı oluşturan bir çözüm. Uyarı bağlamı alanlarını izleme hizmeti tarafından belirlenir. |
| alertTargetIds | Bir uyarının ARM etkilenen kimlikleri tüm hedeflerin listesi. Bir Log Analytics çalışma alanı veya Application Insights örneği üzerinde tanımlanan günlük uyarısı için bunu ilgili çalışma/uygulama olacak. |
| originAlertId | Uyarı örneği olarak, oluşturma izleme hizmeti tarafından oluşturulan kimliği. |
| firedDateTime | Uyarı örneği UTC zaman harekete geçirildi, tarih saat |
| resolvedDateTime | Uyarı örneği için izleme koşulu 'Çözümlenmiş' UTC ayarlandığında, tarih saat. Şu anda yalnızca ölçüm uyarıları için geçerlidir.|
| açıklama | Uyarı kuralda tanımlanan açıklaması |
|essentialsVersion| Temel Parçalar bölümünde ilişkin sürüm numarasıdır.|
|alertContextVersion | AlertContext bölümü için sürüm numarası |

##### <a name="sample-values"></a>Örnek değerler
```json
{
  "essentials": {
    "alertId": "/subscriptions/<subscription ID>/providers/Microsoft.AlertsManagement/alerts/b9569717-bc32-442f-add5-83a997729330",
    "alertRule": "Contoso IT Metric Alert",
    "severity": "Sev3",
    "signalType": "Metric",
    "monitorCondition": "Fired",
    "monitoringService": "Platform",
    "alertTargetIDs": [
      "/subscriptions/<subscription ID>/resourceGroups/aimon-rg/providers/Microsoft.Insights/components/ai-orion-int-fe"
    ],
    "originAlertId": "74ff8faa0c79db6084969cf7c72b0710e51aec70b4f332c719ab5307227a984f",
    "firedDateTime": "2019-03-26T05:25:50.4994863Z",
    "description": "Test Metric alert",
    "essentialsVersion": "1.0",
    "alertContextVersion": "1.0"
  }
}
```

## <a name="alert-context-fields"></a>'Uyarı bağlamı' alanları

### <a name="metric-alerts"></a>Ölçüm Uyarıları

#### <a name="monitoringservice--platform"></a>monitoringService 'Platform' =

##### <a name="sample-values"></a>Örnek değerler
```json
{
  "alertContext": {
      "properties": null,
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Percentage CPU",
            "metricNamespace": "Microsoft.Compute/virtualMachines",
            "operator": "GreaterThan",
            "threshold": "25",
            "timeAggregation": "Average",
            "dimensions": [
              {
                "name": "ResourceId",
                "value": "3efad9dc-3d50-4eac-9c87-8b3fd6f97e4e"
              }
            ],
            "metricValue": 31.1105
          }
        ],
        "windowStartTime": "2019-03-22T13:40:03.064Z",
        "windowEndTime": "2019-03-22T13:45:03.064Z"
      }
    }
}
```

### <a name="log-alerts"></a>Günlük uyarıları

> [!NOTE]
> Var olan günlük uyarıları için özel JSON seçeneğini kullanırsanız, özelleştirme ortak şemada korunmaz.

#### <a name="monitoringservice--log-analytics"></a>monitoringService 'Log Analytics' =

##### <a name="sample-values"></a>Örnek değerler
```json
{
  "alertContext": {
      "SearchQuery": "search * \n| where Type == \"Heartbeat\" \n| where Category == \"Direct Agent\" \n| where TimeGenerated > ago(30m) ",
      "SearchIntervalStartTimeUtc": "3/22/2019 1:36:31 PM",
      "SearchIntervalEndtimeUtc": "3/22/2019 1:51:31 PM",
      "ResultCount": 15,
      "LinkToSearchResults": "https://portal.azure.com#@72f988bf-86f1-41af-91ab-2d7cd011db47/blade/Microsoft_OperationsManagementSuite_Workspace/AnalyticsBlade/initiator/AnalyticsShareLinkToQuery/isQueryEditorVisible/true/scope/%7B%22resources%22%3A%5B%7B%22resourceId%22%3A%22%2Fsubscriptions%<subscription ID>%2FresourceGroups%2Fpipelinealertrg%2Fproviders%2FMicrosoft.OperationalInsights%2Fworkspaces%2FINC-OmsAlertRunner%22%7D%5D%7D/query/search%20%2A%20%0A%7C%20where%20Type%20%3D%3D%20%22Heartbeat%22%20%0A%7C%20where%20Category%20%3D%3D%20%22Direct%20Agent%22%20%0A%7C%20where%20TimeGenerated%20%3E%20%28datetime%282019-03-22T13%3A51%3A31.0000000%29%20-%2030m%29%20%20/isQuerybase64Compressed/false/timespanInIsoFormat/2019-03-22T13%3a36%3a31.0000000Z%2f2019-03-22T13%3a51%3a31.0000000Z",
      "SeverityDescription": "Warning",
      "WorkspaceId": "2a1f50a7-ef97-420c-9d14-938e77c2a929",
      "SearchIntervalDurationMin": "15",
      "AffectedConfigurationItems": [
        "INC-Gen2Alert"
      ],
      "SearchIntervalInMinutes": "15",
      "Threshold": 10000,
      "Operator": "Less Than"
    }
}
```

#### <a name="monitoringservice--application-insights"></a>monitoringService 'Application Insights' =

##### <a name="sample-values"></a>Örnek değerler
```json
{
  "alertContext": {
      "SearchQuery": "search *",
      "SearchIntervalStartTimeUtc": "3/22/2019 1:36:33 PM",
      "SearchIntervalEndtimeUtc": "3/22/2019 1:51:33 PM",
      "ResultCount": 0,
      "LinkToSearchResults": "https://portal.azure.com#@72f988bf-86f1-41af-91ab-2d7cd011db47/blade/Microsoft_OperationsManagementSuite_Workspace/AnalyticsBlade/initiator/AnalyticsShareLinkToQuery/isQueryEditorVisible/true/scope/%7B%22resources%22%3A%5B%7B%22resourceId%22%3A%22%2Fsubscriptions%<subscription ID>%2FresourceGroups%2FPipeLineAlertRG%2Fproviders%2Fmicrosoft.insights%2Fcomponents%2FWEU-AIRunner%22%7D%5D%7D/query/search%20%2A/isQuerybase64Compressed/false/timespanInIsoFormat/2019-03-22T13%3a36%3a33.0000000Z%2f2019-03-22T13%3a51%3a33.0000000Z",
      "SearchIntervalDurationMin": "15",
      "SearchIntervalInMinutes": "15",
      "Threshold": 10000,
      "Operator": "Less Than",
      "ApplicationId": "8e20151d-75b2-4d66-b965-153fb69d65a6"
    }
}
```

### <a name="activity-log-alerts"></a>Etkinlik Günlüğü Uyarıları

#### <a name="monitoringservice--activity-log---administrative"></a>monitoringService 'Etkinlik günlüğü - Yönetici' =

##### <a name="sample-values"></a>Örnek değerler
```json
{
  "alertContext": {
      "authorization": {
        "action": "Microsoft.Compute/virtualMachines/restart/action",
        "scope": "/subscriptions/<subscription ID>/resourceGroups/PipeLineAlertRG/providers/Microsoft.Compute/virtualMachines/WCUS-R2-ActLog"
      },
      "channels": "Operation",
      "claims": "{\"aud\":\"https://management.core.windows.net/\",\"iss\":\"https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/\",\"iat\":\"1553260826\",\"nbf\":\"1553260826\",\"exp\":\"1553264726\",\"aio\":\"42JgYNjdt+rr+3j/dx68v018XhuFAwA=\",\"appid\":\"e9a02282-074f-45cf-93b0-50568e0e7e50\",\"appidacr\":\"2\",\"http://schemas.microsoft.com/identity/claims/identityprovider\":\"https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/\",\"http://schemas.microsoft.com/identity/claims/objectidentifier\":\"9778283b-b94c-4ac6-8a41-d5b493d03aa3\",\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\":\"9778283b-b94c-4ac6-8a41-d5b493d03aa3\",\"http://schemas.microsoft.com/identity/claims/tenantid\":\"72f988bf-86f1-41af-91ab-2d7cd011db47\",\"uti\":\"v5wYC9t9ekuA2rkZSVZbAA\",\"ver\":\"1.0\"}",
      "caller": "9778283b-b94c-4ac6-8a41-d5b493d03aa3",
      "correlationId": "8ee9c32a-92a1-4a8f-989c-b0ba09292a91",
      "eventSource": "Administrative",
      "eventTimestamp": "2019-03-22T13:56:31.2917159+00:00",
      "eventDataId": "161fda7e-1cb4-4bc5-9c90-857c55a8f57b",
      "level": "Informational",
      "operationName": "Microsoft.Compute/virtualMachines/restart/action",
      "operationId": "310db69b-690f-436b-b740-6103ab6b0cba",
      "status": "Succeeded",
      "subStatus": "",
      "submissionTimestamp": "2019-03-22T13:56:54.067593+00:00"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Ortak uyarı şeması hakkında daha fazla bilgi edinin](https://aka.ms/commonAlertSchemaDocs)


