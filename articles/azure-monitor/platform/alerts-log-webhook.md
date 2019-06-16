---
title: Azure Uyarıları'te günlük uyarıları için Web kancası eylemleri
description: Bu makalede, nasıl bir günlük uyarı kuralı kullanarak log analytics çalışma alanı ya da uygulama öngörüleri, gönderir veri HTTP Web kancası ve farklı özelleştirmeler ayrıntılarını olası açıklanır.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 809c98c1e2e51ae51d7fe03f2165a5d9eecb05cc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64681812"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Web kancası eylemleri
Olduğunda bir [günlük uyarı Azure'da oluşturulan](alerts-log.md), seçeneğiniz vardır [Eylem grupları kullanarak yapılandırma](action-groups.md) bir veya daha fazla eylem gerçekleştirmek için.  Bu makalede, özel JSON tabanlı Web kancası yapılandırma hakkında ayrıntıları ve mevcut olan farklı bir Web kancası eylemleri açıklar.

> [!NOTE]
> Ayrıca [ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs), Genişletilebilir tek bir avantajı sağlar ve birleşik uyarı yük boyunca tüm uyarı Hizmetleri Azure İzleyici'de, Web kancası tümleştirmeleri için. [Ortak uyarı şema tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağrılacak sağlar.  Çağrılan hizmet ve Web kancalarını destekleyen hiçbir yük kullanma belirlemek aldığı.    

Web kancası eylemleri özellikler aşağıdaki tabloda gerektirir:

| Özellik | Açıklama |
|:--- |:--- |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükü |Uyarı oluşturulurken bu seçenek seçildiğinde, Web kancası ile göndermek için özel bir yükü. Ayrıntılar kullanılabilir [günlük Uyarıları Yönet](alerts-log.md) |

> [!NOTE]
> Görünümü Web kancası düğmesini yanı sıra *Web kancası Ekle özel JSON yükü* günlük uyarı seçeneğinin, sağlanan özelleştirme için örnek Web kancası yükü görüntülenir. Gerçek veri ve günlük uyarıları için kullanılan JSON şeması temsilcisi içermiyor. 

Web kancaları, bir URL ve dış hizmete gönderilen veriler JSON biçimli bir yükü içerir.  Varsayılan olarak, yük aşağıdaki tabloda değerler içerir:  Bu yükü ile kendi özel bir değiştirilecek seçebilirsiniz.  Bu durumda, değişkenleri tablodaki her parametre için özel yükünüzü değerlerine dahil etmek için kullanabilirsiniz.


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
> LinkToSearchResults SearchQuery, arama aralığının StartTime & arama aralığının bitiş saati gibi parametreleri Analytics bölümünde izleme için Azure portalı URL'sini geçirir. Azure portalında sahip olur ve yaklaşık 2000 karakter sınırını boyut URI *değil* parametrelerin değerleri söz konusu sınırı aşarsa Uyarıları ' sağlanan bağlantıyı aç. Kullanıcıları el ile Analytics portalında sonuçları görüntülemek veya kullanmak için ayrıntıları giriş [Application Insights Analytics REST API](https://dev.applicationinsights.io/documentation/Using-the-API) veya [Log Analytics REST API](/rest/api/loganalytics/) programlı olarak sonuçlar almak için 

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

Özel bir yükte arama sonuçları dahil etmek için emin olun **IncludeSearchResults** json yükü en üst düzey özelliği olarak ayarlanır. 

## <a name="sample-payloads"></a>Örnek yükler
Bu bölümde, yükü standart olduğunda ve ne zaman gibi günlük uyarıları için Web kancası için örnek yük gösterilir, özel.

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

> [!NOTE]
> Önem derecesi alanını değeri varsa değişebilir [API tercihinizi anahtarlı](alerts-log-api-switch.md) Log Analytics günlük uyarıları için.


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
    "Severity": "3",
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
- Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](alerts-unified-log.md)
- Anlamak [azure'da günlük uyarıları yönetme](alerts-log.md)
- Oluşturma ve yönetme [Azure Eylem grupları](action-groups.md)
- Daha fazla bilgi edinin [Application Insights](../../azure-monitor/app/analytics.md)
- Daha fazla bilgi edinin [oturum sorguları](../log-query/log-query-overview.md). 

