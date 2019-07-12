---
title: Azure Uyarıları'te günlük uyarıları için Web kancası eylemleri
description: Bu makalede Log Analytics çalışma alanı veya Application Insights'ı kullanarak günlük uyarı kuralı oluşturmak nasıl bir HTTP Web kancası ve olası farklı özelleştirmeler ayrıntılarını uyarı verilerini nasıl gönderen.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 6aa007c621e76cb0c188a7dab6279fd9e387b2b3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705182"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Web kancası eylemleri
Olduğunda bir [günlük uyarı Azure'da oluşturulan](alerts-log.md), seçeneğiniz vardır [Eylem grupları kullanarak yapılandırmayı](action-groups.md) bir veya daha fazla eylem gerçekleştirmek için. Bu makale, mevcut olan farklı bir Web kancası eylemleri açıklar ve özel JSON tabanlı Web kancası yapılandırma işlemi gösterilmektedir.

> [!NOTE]
> Ayrıca [ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs) , Web kancası tümleştirmeleri için. Ortak uyarı şema, tek bir genişletilebilir ve birleşik uyarı yükü Azure İzleyici uyarı tüm hizmetlerde avantajı sağlar. [Ortak uyarı şema tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası işlemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağırabilirsiniz. Çağrılan hizmet, Web kancalarını destekleyen ve nasıl aldığı herhangi bir yükü kullanacağınızı belirleyin.

Web kancası eylemleri aşağıdaki tabloda özelliklerini gerektirir.

| Özellik | Açıklama |
|:--- |:--- |
| **Web kancası URL'si** |Web kancası URL'si. |
| **Özel JSON yükü** |Uyarı oluşturulurken bu seçenek seçildiğinde, Web kancası ile göndermek için özel yükü. Daha fazla bilgi için [Yönet günlük uyarıları](alerts-log.md).|

> [!NOTE]
> **Görünümü Web kancası** yanı sıra düğmesini **Web kancası Ekle özel JSON yükü** sağlanan özelleştirme için örnek Web kancası yük günlüğü uyarısı görüntüler için seçenek. Gerçek veri içermiyor, ancak günlük uyarıları için kullanılan JSON şema temsilcisidir. 

Web kancaları, bir URL ve veriler dış hizmete gönderilen JSON biçimli bir yükü içerir. Varsayılan olarak, yük aşağıdaki tabloda değerlerini içerir. Bu yükü ile kendi özel bir değiştirilecek seçebilirsiniz. Bu durumda, değişkenleri tabloda her parametreleri için değerleri özel yükünüzü eklemek için kullanın.


| Parametre | Değişken | Açıklama |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |Uyarı kuralı adı. |
| *Önem derecesi* |#severity |Önem derecesi için tetiklenme günlük uyarı ayarlama. |
| *AlertThresholdOperator* |#thresholdoperator |Büyük veya küçük kullanan bir uyarı kuralı eşiğini işleci daha. |
| *AlertThresholdValue* |#thresholdvalue |Uyarı kuralı için eşik değer. |
| *LinkToSearchResults* |#linktosearchresults |Uyarıyı oluşturan sorgudan kayıtlar döndüren Analytics portalını bağlayın. |
| *ResultCount* |#searchresultcount |Arama sonuçlarında kayıt sayısı. |
| *Arama aralığı bitiş zamanı* |#searchintervalendtimeutc |Bitiş saati UTC, sorgu ile biçimini aa/gg/yyyy ss: dd: ss AM/PM. |
| *Arama aralığı* |#searchinterval |Ss biçiminde bir uyarı kuralı zaman penceresi. |
| *Arama aralığı StartTime* |#searchintervalstarttimeutc |Aa/gg/yyyy biçimindeki ile ss: dd UTC olarak başlangıç zamanı sorgu: ss AM/PM. 
| *SearchQuery* |#searchquery |Uyarı kuralı tarafından kullanılan günlük arama sorgusu. |
| *SearchResults* |"IncludeSearchResults": true|Varsa ilk 1.000 kayıtlarını sınırlı JSON tablo olarak sorgu tarafından döndürülen kayıtları "IncludeSearchResults": true, özel bir JSON Web kancası tanımında en üst düzey bir özellik olarak eklenir. |
| *Uyarı türü*| #alerttype | Günlük uyarı kuralı olarak yapılandırılmış türünü [ölçüm ölçüsü](alerts-unified-log.md#metric-measurement-alert-rules) veya [sonuç sayısı](alerts-unified-log.md#number-of-results-alert-rules).|
| *Çalışma alanı kimliği* |#workspaceid |Log Analytics çalışma alanınızın kimliği. |
| *Uygulama Kimliği* |#applicationid |Application ınsights'ı kimliği uygulama. |
| *Abonelik kimliği* |#subscriptionid |Kullanılan Azure aboneliğinizin kimliği. 

> [!NOTE]
> *LinkToSearchResults* gibi parametrelerini geçirir *SearchQuery*, *arama aralığı StartTime*, ve *arama aralığının bitiş zamanı* azure'a URL Analytics bölümünde izleme için portalı. Azure portalında yaklaşık 2.000 karakterle URI boyut sınırı vardır. Portal olacak *değil* parametre değerlerini sınırı aşarsa uyarıları sağlanan bağlantılar. Analiz portalında sonuçlarını görüntülemek için ayrıntıları el ile girebilirsiniz. Ya da kullanabileceğinizi [Application Insights Analytics REST API](https://dev.applicationinsights.io/documentation/Using-the-API) veya [Log Analytics REST API](/rest/api/loganalytics/) sonuçları programlı olarak alınacak. 

Örneğin, adında tek bir parametre içeren aşağıdaki özel yükü belirtebilirsiniz *metin*. Bu Web kancasını çağırır hizmet, bu parametre bekliyor.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Web kancası'na gönderildiğinde aşağıdaki gibi bir şey bu örnek yükü çözer:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Özel bir Web kancası tüm değişkenleri "#searchinterval," gibi bir JSON kutu içinde belirtilmesi gerektiğinden sonuç Web kancası ayrıca kasalarındaki içinde değişken veri gibi sahip "00: 05:00."

Özel bir yükte arama sonuçları dahil etmek için emin olun **IncludeSearchResults** JSON yükü en üst düzey özelliği olarak ayarlanır. 

## <a name="sample-payloads"></a>Örnek yükler
Bu bölümde, örnek yüklerini günlük uyarıları için Web kancaları için gösterilir. Standart ve özel olduğunda yükü olduğunda, örnek yüklerini örnek olarak verilebilir.

### <a name="standard-webhook-for-log-alerts"></a>Günlük uyarıları için standart bir Web kancası 
Bu örneklerin her ikisi yalnızca iki sütun ve iki satır işlevsiz bir yükü var.

#### <a name="log-alert-for-log-analytics"></a>Log Analytics için günlük Uyarısı
Aşağıdaki örnek için bir standart Web kancası eylemi yüktür *özel bir JSON seçeneği olmadan* Log Analytics temelinde bağlı uyarılar için kullanılır:

```json
{
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": "log alert rule",
    "Severity": "Warning",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
 }
 ```

> [!NOTE]
> Seçtiğiniz "Önem" alan değeri değiştirebilirsiniz [API tercihinizi anahtarlı](alerts-log-api-switch.md) Log Analytics günlük uyarıları için.


#### <a name="log-alert-for-application-insights"></a>Application Insights için günlük Uyarısı
Aşağıdaki örnek için standart bir Web kancası yüktür *özel bir JSON seçeneği olmadan* Application Insights tabanlı günlük uyarıları için kullanıldığında:
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"requests | where resultCode == \"500\"",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
    "AlertType": "Number of results"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Özel JSON yükü ile günlük Uyarısı
Örneğin, yalnızca uyarı adı ve arama sonuçlarını içeren özel bir yükü oluşturmak için aşağıdakileri kullanabilirsiniz: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Aşağıdaki örnek yük herhangi bir günlük uyarı için bir özel Web kancası eylemi içindir:
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [uyarılar Azure Uyarıları ' oturum](alerts-unified-log.md).
- Anlamak için nasıl [Azure günlük uyarılarını Yönet](alerts-log.md).
- Oluşturma ve yönetme [Azure Eylem grupları](action-groups.md).
- Daha fazla bilgi edinin [Application Insights](../../azure-monitor/app/analytics.md).
- Daha fazla bilgi edinin [oturum sorguları](../log-query/log-query-overview.md). 

