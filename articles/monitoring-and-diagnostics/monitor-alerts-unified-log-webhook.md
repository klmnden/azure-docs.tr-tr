---
title: Azure Uyarıları'te günlük uyarıları için Web kancası eylemleri | Microsoft Docs
description: Bu makalede, nasıl bir günlük uyarı kuralı için günlük analizi veya uygulama Öngörüler push kullanılarak veri HTTP Web kancası ve farklı özelleştirmeler ayrıntılarını olası açıklanmaktadır.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 28c8e6ab6a23a46bdea31c71b08b9c6a28d1be33
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Web kancası eylemleri
Zaman bir [uyarı Azure içinde oluşturulan ](monitor-alerts-unified-usage.md), seçeneğiniz vardır [Eylem grupları kullanarak yapılandırma](monitoring-action-groups.md) bir veya daha fazla eylemleri gerçekleştirmek için.  Bu makalede, özel JSON tabanlı Web kancası yapılandırma hakkında ayrıntılar ve kullanılabilir farklı Web kancası eylemleri açıklanmaktadır.


## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağırma olanak tanır.  Çağrılan Hizmet Web kancalarını destekleyen ve nasıl herhangi yükü kullandığını belirlemek aldığı.   Bir Web kancası yanıt olarak bir uyarı kullanma örnekleri bir ileti gönderiyorsunuz [Slack'e](http://slack.com) veya bir olay oluşturma [PagerDuty](http://pagerduty.com/).  

Web kancası eylemleri özellikler aşağıdaki tabloda gerektirir:

| Özellik | Açıklama |
|:--- |:--- |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükü |Uyarı oluşturulurken bu seçenek seçildiğinde Web kancası ile göndermek için özel yükü. Ayrıntılar adresinde [Azure uyarıları kullanarak Uyarıları yönetme ](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Web kancası düğmesi yanında test *INCLUDE özel JSON yükü Web kancası için* seçeneği günlük uyarı için Web kancası URL'si test etmek için sahte çağrısı tetikler. Gerçek veri ve günlük uyarılar için kullanılan JSON şeması temsilcisi içermiyor. 

Web kancası bir URL ve dış hizmete gönderilen veriler JSON biçimli bir yükü içerir.  Varsayılan olarak, yükü değerleri aşağıdaki tabloda içerir: özel bir kendi bu yükü tarihle seçebilirsiniz.  Bu durumda, değişkenleri tabloda her parametre için değer özel yükünüzü dahil etmek için kullanabilirsiniz.


| Parametre | Değişken | Açıklama |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Uyarı kuralı adı. |
| Önem Derecesi |#severity |Önem derecesi için Mazotlu günlük uyarı ayarlanmıştır. |
| AlertThresholdOperator |#thresholdoperator |Uyarı kuralı için eşik işleci.  *Büyük* veya *değerinden*. |
| AlertThresholdValue |#thresholdvalue |Uyarı kuralı için eşik değer. |
| LinkToSearchResults |#linktosearchresults |Günlük analizi günlük kayıtları uyarı oluşturulan sorgudan döndüren bir arama bağlayın. |
| ResultCount |#searchresultcount |Arama sonuçlarında kayıt sayısı. |
| Arama aralığı bitiş saati |#searchintervalendtimeutc |Bitiş saati UTC biçiminde bir sorgu için. |
| Arama aralığı |#searchinterval |Zaman penceresi için uyarı kuralı. |
| Arama aralığı başlangıç saati |#searchintervalstarttimeutc |Sorgu saati UTC biçiminde başlatın. 
| SearchQuery |#searchquery |Uyarı kuralı tarafından kullanılan günlük arama sorgusu. |
| SearchResults |"IncludeSearchResults": true|Bir JSON tablosu olarak ilk 1.000 kayıtları sınırlı sorgu tarafından döndürülen kayıt; varsa "IncludeSearchResults": true, özel JSON Web kancası tanımında en üst düzey bir özellik olarak eklenir. |
| Workspaceıd |#workspaceid |Günlük analizi çalışma alanı kimliği. |
| Uygulama Kimliği |#applicationid |Uygulama Insight Kimliğini uygulama. |
| Abonelik Kimliği |#subscriptionid |Application Insights ile kullanılan Azure aboneliğinizi kimliği. 


Örneğin, adlı tek bir parametre içeren aşağıdaki özel yükü belirtebilir *metin*.  Bu Web kancası çağırır hizmet, bu parametre bekleniyor.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Bu örnek yükü için Web kancası gönderildiğinde aşağıdaki gibi bir şey çözümlemek.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```

Özel bir yükte arama sonuçlarında için emin **IncudeSearchResults** json yükü en üst düzey özelliği olarak ayarlayın. 

## <a name="sample-payloads"></a>Örnek yükü
Bu bölümde, günlük için uyarıları, yükü standart olduğunda ve ne zaman gibi Web kancası için örnek yükü gösterir kendi özel.

> [!NOTE]
> Geriye dönük uyumluluk sağlamak üzere Azure günlük analizi kullanarak uyarıları için standart Web kancası yükü aynı [günlük analizi uyarı Yönetim](../log-analytics/log-analytics-alerts-creating.md). Ancak kullanarak günlük uyarılar için [Application Insights](../application-insights/app-insights-analytics.md), standart Web kancası yükü eylem Grup şemasını temel alan.

### <a name="standard-webhook-for-log-alerts"></a>Standart Web kancası günlük uyarılar için 
Bu örneklerin her ikisi de yalnızca iki sütun ve iki satır bir kukla yükü açıklamıştır.

#### <a name="log-alert-for-azure-log-analytics"></a>Azure günlük analizi için günlük uyarı
Standart Web kancası eylemi için örnek yükü aşağıdadır *özel Json seçeneği olmadan* günlük analizi tabanlı uyarılar için kullanılan.

```json
{
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
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
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
 }
 ```   

#### <a name="log-alert-for-azure-application-insights"></a>Azure Application Insights için günlük uyarı
Standart bir Web kancası için örnek yükü aşağıdadır *özel Json seçeneği olmadan* uygulama Öngörüler tabanlı günlük-uyarıları için kullanıldığında.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
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
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Error",
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Özel JSON yükü olan günlük uyarı
Örneğin, yalnızca uyarı adı ve arama sonuçlarını içeren özel bir yükü oluşturmak için aşağıdakileri kullanabilirsiniz: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Herhangi bir günlük uyarı için bir özel Web kancası eylemi için örnek yükü aşağıdadır.
    
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
- Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum açın ](monitor-alerts-unified-log.md)
- Oluşturma ve yönetme [azure'da Eylem grupları](monitoring-action-groups.md)
- Daha fazla bilgi edinmek [Application Insights](../application-insights/app-insights-analytics.md)
- Daha fazla bilgi edinmek [günlük analizi](../log-analytics/log-analytics-overview.md). 