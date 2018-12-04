---
title: Azure Uyarıları'te günlük uyarıları için Web kancası eylemleri
description: Bu makalede, nasıl bir günlük uyarı kuralı günlük analytics veya application ınsights kullanarak gönderir veri HTTP Web kancası ve farklı özelleştirmeler ayrıntılarını olası açıklanır.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: e344b4064f8211af1c4a03359e64ff25792fd4fd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837418"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Web kancası eylemleri
Olduğunda bir [günlük uyarı Azure'da oluşturulan](alert-log.md), seçeneğiniz vardır [Eylem grupları kullanarak yapılandırma](monitoring-action-groups.md) bir veya daha fazla eylem gerçekleştirmek için.  Bu makalede, özel JSON tabanlı Web kancası yapılandırma hakkında ayrıntıları ve mevcut olan farklı bir Web kancası eylemleri açıklar.


## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağrılacak sağlar.  Çağrılan hizmet ve Web kancalarını destekleyen hiçbir yük kullanma belirlemek aldığı.    

Web kancası eylemleri özellikler aşağıdaki tabloda gerektirir:

| Özellik | Açıklama |
|:--- |:--- |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükü |Uyarı oluşturulurken bu seçenek seçildiğinde, Web kancası ile göndermek için özel bir yükü. Ayrıntılar kullanılabilir [günlük Uyarıları Yönet](alert-log.md) |

> [!NOTE]
> Görünümü Web kancası düğmesini yanı sıra *Web kancası Ekle özel JSON yükü* günlük uyarı seçeneğinin, sağlanan özelleştirme için örnek Web kancası yükü görüntülenir. Gerçek veri ve günlük uyarıları için kullanılan JSON şeması temsilcisi içermiyor. 

Web kancaları, bir URL ve dış hizmete gönderilen veriler JSON biçimli bir yükü içerir.  Varsayılan olarak, yük değerleri aşağıdaki tabloda içerir: Bu yükü ile kendi özel bir değiştirilecek seçebilirsiniz.  Bu durumda, değişkenleri tablodaki her parametre için özel yükünüzü değerlerine dahil etmek için kullanabilirsiniz.


| Parametre | Değişken | Açıklama |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Uyarı kuralı adı. |
| Severity |#severity |Önem derecesi için tetiklenme günlük uyarı ayarlama. |
| AlertThresholdOperator |#thresholdoperator |Eşik işleci için uyarı kuralı.  *Büyüktür* veya *küçüktür*. |
| AlertThresholdValue |#thresholdvalue |Uyarı kuralı için eşik değer. |
| LinkToSearchResults |#linktosearchresults |Uyarıyı oluşturan sorgudan kayıtlar döndüren analiz portalı bağlayın. |
| ResultCount |#searchresultcount |Arama sonuçlarında kayıt sayısı. |
| Arama aralığı bitiş zamanı |#searchintervalendtimeutc |Bitiş zamanı, UTC sorguda format - aa/gg/yyyy ss: dd: ss AM/PM. |
| Arama aralığı |#searchinterval |Zaman penceresi için uyarı kuralı, biçimi - SS. |
| Arama aralığı StartTime |#searchintervalstarttimeutc |UTC olarak başlangıç zamanı sorgu için format - aa/gg/yyyy ss: dd: ss AM/PM.. 
| SearchQuery |#searchquery |Uyarı kuralı tarafından kullanılan günlük arama sorgusu. |
| SearchResults |"IncludeSearchResults": true|İlk 1.000 kayıtlara sınırlı bir JSON tablo olarak sorgu tarafından döndürülen kayıtları; varsa "IncludeSearchResults": true, özel JSON Web kancası tanımında en üst düzey bir özellik olarak eklenir. |
| Çalışma alanı kimliği |#workspaceid |Log Analytics çalışma alanınızın kimliği. |
| Uygulama Kimliği |#applicationid |Kimliği, Application Insight uygulama. |
| Abonelik Kimliği |#subscriptionid |Application Insights ile kullanılan Azure aboneliğinizin kimliği. 

> [!NOTE]
> LinkToSearchResults SearchQuery, arama aralığının StartTime & arama aralığının bitiş saati gibi parametreleri Analytics bölümünde izleme için Azure portalı URL'sini geçirir. Azure portalında sahip olur ve yaklaşık 2000 karakter sınırını boyut URI *değil* parametrelerin değerleri söz konusu sınırı aşarsa Uyarıları ' sağlanan bağlantıyı aç. Kullanıcıları el ile Analytics portalında sonuçları görüntülemek veya kullanmak için ayrıntıları giriş [Application Insights Analytics REST API](https://dev.applicationinsights.io/documentation/Using-the-API) veya [Log Analytics REST API](https://dev.loganalytics.io/reference) programlı olarak sonuçlar almak için 

Örneğin, adında tek bir parametre içeren aşağıdaki özel yükü belirtebilirsiniz *metin*.  Bu Web kancasını çağırır hizmet, bu parametre bekleniyor.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Bu örnek yükü Web kancası'na gönderildiğinde aşağıdaki gibi bir şey çözümlemek.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Özel bir Web kancası tüm değişkenlerinde belirtilen "#searchinterval" gibi JSON kutu içinde olduğundan, sonuçta elde edilen Web kancası gibi kutu içinde değişken veri de bulunur "00: 05:00".

Özel bir yükte arama sonuçları dahil etmek için emin olun **IncudeSearchResults** json yükü en üst düzey özelliği olarak ayarlanır. 

## <a name="sample-payloads"></a>Örnek yükler
Bu bölümde, yükü standart olduğunda ve ne zaman gibi günlük uyarıları için Web kancası için örnek yük gösterilir, özel.

> [!NOTE]
> Geriye dönük uyumluluk sağlamak için Azure Log Analytics kullanarak uyarıları için standart bir Web kancası yükü aynı [Log Analytics uyarı Yönetim](../monitoring-and-diagnostics/alert-metric.md). Ancak kullanarak günlük uyarıları için [Application Insights](../application-insights/app-insights-analytics.md), standart bir Web kancası yükü eylem grubu şemasını temel alır.

### <a name="standard-webhook-for-log-alerts"></a>Günlük uyarıları için standart bir Web kancası 
Bu örneklerin her ikisi, yalnızca iki sütun ve iki satır işlevsiz bir yükü açıklamıştır.

#### <a name="log-alert-for-azure-log-analytics"></a>Azure Log Analytics için günlük Uyarısı
Standart Web kancası eylemi için bir örnek yük aşağıdadır *özel Json seçeneği olmadan* log analytics tabanlı uyarılar için kullanılıyor.

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

#### <a name="log-alert-for-azure-application-insights"></a>Azure Application Insights için günlük Uyarısı
Bir örnek yük için standart bir Web kancası aşağıdadır *özel Json seçeneği olmadan* application ınsights tabanlı günlük-uyarılar için kullanıldığında.
    
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

#### <a name="log-alert-with-custom-json-payload"></a>Özel JSON yükü ile günlük Uyarısı
Örneğin, yalnızca uyarı adı ve arama sonuçlarını içeren özel bir yükü oluşturmak için aşağıdakileri kullanabilirsiniz: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Aşağıdaki, tüm günlük uyarısı için bir özel Web kancası eylemi için bir örnek yüktür.
    
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
- Hakkında bilgi edinin [oturum uyarılar Azure uyarıları ](monitor-alerts-unified-log.md)
- Anlamak [azure'da managaing günlük uyarıları](alert-log.md)
- Oluşturma ve yönetme [Azure Eylem grupları](monitoring-action-groups.md)
- Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md)
- Daha fazla bilgi edinin [Log Analytics](../azure-monitor/log-query/log-query-overview.md). 
