---
title: "Web kancası eylemleri günlük uyarılarını Azure Uyarıları'ni (Önizleme) | Microsoft Docs"
description: "Bu makalede, HTTP Web kancası ve farklı özelleştirmeler ayrıntılarını veri olası gönderir günlük analizi veya uygulama kullanarak bir günlük uyarı kuralı nasıl açıklanır."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 49905638-f9f2-427b-8489-a0bcc7d8b9fe
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: vinagara
ms.openlocfilehash: 4af1bb61888810011ce64fde7931cabfefe76ab6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Web kancası eylemleri
Zaman bir [uyarı Azure (Önizleme) oluşturulan](monitor-alerts-unified-usage.md), seçeneğiniz vardır [Eylem grupları kullanarak yapılandırma](monitoring-action-groups.md) bir veya daha fazla eylemleri gerçekleştirmek için.  Bu makalede, özel JSON tabanlı Web kancası yapılandırma hakkında ayrıntılar ve kullanılabilir farklı Web kancası eylemleri açıklanmaktadır.


## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağırma olanak tanır.  Çağrılan Hizmet Web kancalarını destekleyen ve tüm yükü nasıl kullanacağınızı belirleyin aldığı.  Ayrıca, istek API özelliğini algılayan bir biçimde olduğu sürece, özellikle Web kancalarını desteklemeyen bir REST API'si çağırabilirsiniz.  Bir Web kancası yanıt olarak bir uyarı kullanma örnekleri bir ileti gönderiyorsunuz [Slack'e](http://slack.com) veya bir olay oluşturma [PagerDuty](http://pagerduty.com/).  Kayma çağırmak için bir Web kancası ile bir uyarı kuralı oluşturma izlenecek tam yol şu adresten edinilebilir [Kancalarını günlük analizi uyarılar](../log-analytics/log-analytics-alerts-webhooks.md).

Web kancası eylemleri özellikler aşağıdaki tabloda gerektirir.

| Özellik | Açıklama |
|:--- |:--- |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükü |Web kancası ile göndermek için özel yükü.  Uyarı oluşturulurken seçeneği belirlenmişse. Ayrıntılar adresinde [Azure Uyarıları'ni (Önizleme) kullanarak Uyarıları yönetme](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Web kancası düğmesi yanında test *INCLUDE özel JSON yükü Web kancası için* seçeneği günlük uyarı için Web kancası URL'si test etmek için sahte çağrısı tetikler. Gerçek veri veya günlük uyarılar için kullanılan JSON şeması temsilcisi içermiyor. Herhangi bir Web kancası temsilcisi özel JSON ile test ederken eylem grubunda yapılandırılmış tüm Web kancalarını özel JSON yükü ile gönderilir.

Web kancası bir URL ve dış hizmete gönderilen veriler JSON biçimli bir yükü içerir.  Varsayılan olarak, aşağıdaki tabloda değerleri yükü içerir.  Bu yük özel bir kendi tarihle seçebilirsiniz.  Bu durumda, değişkenleri tabloda her parametre için değer özel yükünüzü dahil etmek için kullanabilirsiniz.


| Parametre | Değişken | Açıklama |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Uyarı kuralı adı. |
| AlertThresholdOperator |#thresholdoperator |Uyarı kuralı için eşik işleci.  *Büyük* veya *değerinden*. |
| AlertThresholdValue |#thresholdvalue |Uyarı kuralı için eşik değer. |
| LinkToSearchResults |#linktosearchresults |Günlük analizi günlük kayıtları uyarı oluşturulan sorgudan döndüren bir arama bağlayın. |
| ResultCount |#searchresultcount |Arama sonuçlarında kayıt sayısı. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Bitiş saati UTC biçiminde bir sorgu için. |
| SearchIntervalInSeconds |#searchinterval |Zaman penceresi için uyarı kuralı. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Sorgu saati UTC biçiminde başlatın. |
| SearchQuery |#searchquery |Uyarı kuralı tarafından kullanılan günlük arama sorgusu. |
| SearchResults |"IncludeSearchResults":true|Bir JSON tablosu olarak ilk 1.000 kayıtları/satırları sınırlı sorgu tarafından döndürülen kayıt; varsa "IncludeSearchResults": true, üst düzey bir özellik olarak özel JSON Web kancası tanımında eklenir |
| WorkspaceID |#workspaceid |Günlük analizi çalışma alanı kimliği. |
| Önem Derecesi |#severity |Önem derecesi için Mazotlu günlük uyarı ayarlanmıştır. |

Örneğin, adlı tek bir parametre içeren aşağıdaki özel yükü belirtebilir *metin*.  Bu Web kancası çağırır hizmet, bu parametre bekleniyor.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Bu örnek yükü için Web kancası gönderildiğinde aşağıdaki gibi bir şey çözümlemek.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Özel bir yükte arama sonuçlarında için emin **IncudeSearchResults** json yükü en üst düzey özelliği olarak ayarlayın.


Dış bir hizmeti başlatmak için bir Web kancası ile bir uyarı kuralı oluşturma tam bir örnek size yol [Slack'e ileti göndermek için günlük analizi uyarı Web kancası eylem oluşturma](../log-analytics/log-analytics-alerts-webhooks.md).

## <a name="sample-payload"></a>Örnek yükü
Bu bölümde, günlük için uyarıları, yükü standart olduğunda ve ne zaman gibi Web kancası için örnek yükü gösterir kendi özel.

> [!NOTE]
> Geriye dönük uyumluluk sağlamak üzere Azure günlük analizi kullanarak uyarıları için standart Web kancası yükü aynı [OMS uyarı Yönetim](../log-analytics/log-analytics-solution-alert-management.md). Ancak kullanarak günlük uyarılar için [Application Insights](../application-insights/app-insights-analytics.md), standart Web kancası yükü eylem Grup şemasını temel alan

### <a name="standard-webhook-for-log-alerts"></a>Standart Web kancası günlük uyarılar için
Her ikisi de bu örnekler biz yalnızca iki sütun ve iki satır bir kukla yükü açıklamıştır.

#### <a name="log-alert-for-azure-log-analytics"></a>Azure günlük analizi için günlük uyarı
Günlük analizi tabanlı günlük-uyarılar için kullanıldığında özel Json olmadan bir standart Web kancası eylemi için örnek yükü aşağıdadır.

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
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }



#### <a name="log-alert-for-azure-application-insights"></a>Azure Application Insights için günlük uyarı
Standart Web kancası eylem uygulama Öngörüler tabanlı günlük-uyarıları için kullanıldığında özel Json olmadan için örnek yükü aşağıdadır.


    {
    "schemaId":"LogAlert","data":
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
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }
    }


#### <a name="log-alert-with-custom-json-payload"></a>Özel JSON yükü olan günlük uyarı
Örneğin, yalnızca uyarı adı ve arama sonuçlarını içeren özel bir yükü oluşturmak için aşağıdakileri kullanabilirsiniz.

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }

Herhangi bir günlük uyarı için bir özel Web kancası eylemi için örnek yükü aşağıdadır.


    {
    "AlertRuleName":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
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




## <a name="next-steps"></a>Sonraki adımlar
- Oluşturma ve yönetme [azure'da Eylem grupları](monitoring-action-groups.md)
- Hakkında bilgi edinin [günlük uyarıları Azure uyarılar (Önizleme)](monitor-alerts-unified-log.md)
