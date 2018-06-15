---
title: Azure günlük analizi uyarılarını yanıtlarını | Microsoft Docs
description: Günlük analizi uyarılarını Azure alanınızdaki önemli bilgileri tanımlamak ve önceden sorunları size bildiren veya düzeltmenize girişiminde Eylemler çağırma.  Bu makalede, bir uyarı kuralı ve ayrıntıları yapabilecekleri farklı eylemler oluşturmayı açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2018
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 717adf1b19b9de8542ec507df3a01b187d0df8a5
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31521073"
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Günlük analizi uyarı kurallarında eylemleri ekleyin

> [!NOTE]
> Günlük analizi uyarıların [Azure'da genişletilen](../monitoring-and-diagnostics/monitoring-alerts-extend.md).  Azure kullanım uyarıları [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) bu makaledeki bilgileri yerine kendi eylemleri tanımlamak için.


Zaman bir [uyarı günlük analizi oluşturulan](log-analytics-alerts.md), seçeneğiniz vardır [uyarı kuralı yapılandırma](log-analytics-alerts.md) bir veya daha fazla eylemleri gerçekleştirmek için.  Bu makalede, her tür yapılandırma hakkında ayrıntılar ve kullanılabilir farklı eylemler açıklanmaktadır.

| Eylem | Açıklama |
|:--|:--|
| [E-posta](#email-actions) | Bir veya daha fazla alıcıya uyarı ayrıntılarını içeren bir e-posta gönderin. |
| [Web kancası](#webhook-actions) | Bir dış işlem tek bir HTTP POST isteği üzerinden çağırır. |
| [runbook](#runbook-actions) | Bir runbook, Azure Automation'da başlatın. |


## <a name="email-actions"></a>E-posta Eylemler
E-posta Eylemler bir veya daha fazla alıcıya uyarı ayrıntılarını içeren bir e-posta gönderin.  Postanın konusunu belirtebilirsiniz ancak içeriği, Log Analytics tarafından oluşturulan standart bir biçimdedir.  Uyarı günlüğü araması tarafından döndürülen en fazla on kayıt ayrıntılarını yanı sıra adı gibi özet bilgileri içerir.  Ayrıca bu sorgudan tüm kayıt kümesini döndürür günlük analizi, günlük arama bağlantısını içerir.   Posta gönderen *Microsoft Operations Management Suite ekibi &lt; noreply@oms.microsoft.com &gt;* . 

E-posta eylemler özellikler aşağıdaki tabloda gerektirir.

| Özellik | Açıklama |
|:--- |:--- |
| Konu |E-postayla konu.  Posta gövdesini değiştiremezsiniz. |
| Alıcılar |Tüm e-posta alıcıları adresleri.  Birden fazla adres belirtirseniz, adresleri noktalı virgül (;) ile ayırın. |


## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağırma olanak tanır.  Çağrılan Hizmet Web kancalarını destekleyen ve nasıl herhangi yükü kullandığını belirlemek aldığı.  Ayrıca, istek API özelliğini algılayan bir biçimde olduğu sürece, özellikle Web kancalarını desteklemeyen bir REST API'si çağırabilirsiniz.  Bir Web kancası yanıt olarak bir uyarı kullanma örnekleri bir ileti gönderiyorsunuz [Slack'e](http://slack.com) veya bir olay oluşturma [PagerDuty](http://pagerduty.com/).  Kayma çağırmak için bir Web kancası ile bir uyarı kuralı oluşturma izlenecek tam yol şu adresten edinilebilir [Kancalarını günlük analizi uyarılar](log-analytics-alerts-webhooks.md).

Web kancası eylemleri özellikler aşağıdaki tabloda gerektirir.

| Özellik | Açıklama |
|:--- |:--- |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükü |Web kancası ile göndermek için özel yükü.  Ayrıntılar için aşağıya bakın. |


Web kancası bir URL ve dış hizmete gönderilen veriler JSON biçimli bir yükü içerir.  Varsayılan olarak, yükü değerleri aşağıdaki tabloda içerir.  Bu yük özel bir kendi tarihle seçebilirsiniz.  Bu durumda, değişkenleri tabloda her parametre için değer özel yükünüzü dahil etmek için kullanabilirsiniz.


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
| SearchResults |Aşağıya bakın |JSON biçiminde sorgu tarafından döndürülen kaydeder.  5. 000'ilk kayıtları sınırlıdır. |
| Workspaceıd |#workspaceid |Günlük analizi çalışma alanı kimliği. |

Örneğin, adlı tek bir parametre içeren aşağıdaki özel yükü belirtebilir *metin*.  Bu Web kancası çağırır hizmet, bu parametre bekleniyor.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Bu örnek yükü için Web kancası gönderildiğinde aşağıdaki gibi bir şey çözümlemek.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Özel bir yükte arama sonuçlarında en üst düzey özelliği json yükü olarak aşağıdaki satırı ekleyin.  

    "IncludeSearchResults":true

Örneğin, yalnızca uyarı adı ve arama sonuçlarını içeren özel bir yükü oluşturmak için aşağıdakileri kullanabilirsiniz. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Dış bir hizmeti başlatmak için bir Web kancası ile bir uyarı kuralı oluşturma tam bir örnek size yol [Slack'e ileti göndermek için günlük analizi uyarı Web kancası eylem oluşturma](log-analytics-alerts-webhooks.md).


## <a name="runbook-actions"></a>Runbook eylemleri
Runbook eylemleri, Azure Automation'da bir runbook başlatın.  Bu eylemin türünü kullanmak için bilmeniz gereken [Otomasyon çözümünü](log-analytics-add-solutions.md) yüklenir ve günlük analizi çalışma alanınızda yapılandırılır.  Otomasyon çözümünü yapılandırılmış Otomasyon hesabında runbook'ları arasından seçim yapabilirsiniz.

Runbook eylemleri özellikler aşağıdaki tabloda gerektirir.

| Özellik | Açıklama |
|:--- |:---|
| Runbook | Bir uyarı oluşturulduğunda başlatmak istediğiniz Runbook. |
| Üzerinde çalışır | Belirtin **Azure** runbook bulutta çalıştırmak için.  Belirtin **karma çalışanı** runbook'u ile bir aracı çalıştırmayı [karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md ) yüklü.  |

Runbook eylemleri başlatmak kullanarak runbook bir [Web kancası](../automation/automation-webhooks.md).  Uyarı kuralı oluşturduğunuzda, otomatik olarak runbook için yeni bir Web kancası adı ile oluşturur **OMS uyarı düzeltme** bir GUID ile birlikte.  

Runbook'un parametreleri doğrudan doldurulamıyor ancak [$WebhookData parametresi](../automation/automation-webhooks.md) oluşturulduğu günlük arama sonuçları dahil olmak üzere Uyarı ayrıntılarını içerir.  Runbook tanımlamak gereken **$WebhookData** uyarı özelliklerine erişmek için bir parametre olarak.  Uyarı verileri json biçiminde adlı tek bir özellik bulunur **SearchResult** (için runbook eylemleri ve standart yükü Web kancası eylemleri) veya **SearchResults** (Web kancası eylemleri özel Yükü de dahil olmak üzere **IncludeSearchResults ": true**) içinde **RequestBody** özelliği **$WebhookData**.  Bu, aşağıdaki tabloda özelliklerinde sahip.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da runbook yükü değişti.  Biçimle ilgili ayrıntılar için bkz. [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse).  Bir örnekte görebilirsiniz [örnekleri](#sample-payload) aşağıda.  

| Node | Açıklama |
|:--- |:--- |
| id |Yol ve arama GUID. |
| __metadata |Arama sonuçlarını durumunu ve kayıt sayısı dahil olmak üzere uyarı hakkında bilgi. |
| değer |Arama sonuçlarında her kayıt için ayrı girişi.  Giriş ayrıntılarını özellikleri ve kaydın değerleri aynı. |

Örneğin, aşağıdaki runbook'lar, günlük araması tarafından döndürülen kayıtları Ayıkla ve her kayıt türüne göre farklı özellikler atayın.  Runbook dönüştürerek başlatır Not **RequestBody** , BT ile PowerShell nesne olarak çalışılabilecek biçimde json öğesinden.

>[!NOTE]
> Bu runbook kullanan **SearchResult** runbook Eylemler ve standart yükü Web kancası eylemleri sonuçlarını içeren özellik olduğu.  Runbook özel bir yükü kullanarak bir Web kancası yanıttan adı veriliyordu, bu özelliğe değiştirmeniz gerekir **SearchResults**.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody

    # Get all metadata properties    
    $AlertRuleName = $RequestBody.AlertRuleName
    $AlertThresholdOperator = $RequestBody.AlertThresholdOperator
    $AlertThresholdValue = $RequestBody.AlertThresholdValue
    $AlertDescription = $RequestBody.Description
    $LinktoSearchResults =$RequestBody.LinkToSearchResults
    $ResultCount =$RequestBody.ResultCount
    $Severity = $RequestBody.Severity
    $SearchQuery = $RequestBody.SearchQuery
    $WorkspaceID = $RequestBody.WorkspaceId
    $SearchWindowStartTime = $RequestBody.SearchIntervalStartTimeUtc
    $SearchWindowEndTime = $RequestBody.SearchIntervalEndtimeUtc
    $SearchWindowInterval = $RequestBody.SearchIntervalInSeconds

    # Get detailed search results
    if($RequestBody.SearchResult -ne $null)
    {
        $SearchResultRows    = $RequestBody.SearchResult.tables[0].rows 
        $SearchResultColumns = $RequestBody.SearchResult.tables[0].columns;

        foreach ($SearchResultRow in $SearchResultRows)
        {   
            $Column = 0
            $Record = New-Object –TypeName PSObject 
        
            foreach ($SearchResultColumn in $SearchResultColumns)
            {
                $Name = $SearchResultColumn.name
                $ColumnValue = $SearchResultRow[$Column]
                $Record | Add-Member –MemberType NoteProperty –Name $name –Value $ColumnValue -Force
                        
                $Column++
            }

            # Include code to work with the record. 
            # For example $Record.Computer to get the computer property from the record.
            
        }
    }



## <a name="sample-payload"></a>Örnek yükü
Bu bölümde Web kancası ve runbook eylemler için örnek yükü gösterir.

### <a name="webhook-actions"></a>Web kancası eylemleri
Bu örnekte **SearchResult** Web kancası eylemler için birlikte standart yükü sonuçlarını içeren özellik olduğu.  Web kancası arama sonuçlarını içeren özel bir yükü kullandıysanız, bu özellik olacak **SearchResults**.

Bir Web kancası eylemi için örnek yükü aşağıdadır.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "WebhookAlert",
    "SearchQuery": "Usage",
    "SearchResult": {
        "tables": [
        {
            "name": "PrimaryResult",
            "columns": [
            {
                "name": "TenantId",
                "type": "string"
            },
            {
                "name": "Computer",
                "type": "string"
            },
            {
                "name": "TimeGenerated",
                "type": "datetime"
            },
            {
                "name": "SourceSystem",
                "type": "string"
            },
            {
                "name": "StartTime",
                "type": "datetime"
            },
            {
                "name": "EndTime",
                "type": "datetime"
            },
            {
                "name": "ResourceUri",
                "type": "string"
            },
            {
                "name": "LinkedResourceUri",
                "type": "string"
            },
            {
                "name": "DataType",
                "type": "string"
            },
            {
                "name": "Solution",
                "type": "string"
            },
            {
                "name": "BatchesWithinSla",
                "type": "long"
            },
            {
                "name": "BatchesOutsideSla",
                "type": "long"
            },
            {
                "name": "BatchesCapped",
                "type": "long"
            },
            {
                "name": "TotalBatches",
                "type": "long"
            },
            {
                "name": "AvgLatencyInSeconds",
                "type": "real"
            },
            {
                "name": "Quantity",
                "type": "real"
            },
            {
                "name": "QuantityUnit",
                "type": "string"
            },
            {
                "name": "IsBillable",
                "type": "bool"
            },
            {
                "name": "MeterId",
                "type": "string"
            },
            {
                "name": "LinkedMeterId",
                "type": "string"
            },
            {
                "name": "Type",
                "type": "string"
            }
            ],
            "rows": [
            [
                "workspaceID",
                "-",
                "2017-09-26T13:59:59Z",
                "OMS",
                "2017-09-26T13:00:00Z",
                "2017-09-26T13:59:59Z",
                "/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.operationalinsights/workspaces/workspace-workspaceID",
                null,
                "Operation",
                "LogManagement",
                8,
                0,
                0,
                8,
                0,
                0.002502,
                "MBytes",
                false,
                "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "00000000-0000-0000-0000-000000000000",
                "Usage"
            ]
            ]
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }


### <a name="runbooks"></a>Runbook'lar

Bir runbook eylemi için örnek yükü aşağıdadır.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "AutomationAlert",
    "SearchQuery": "Usage",
    "SearchResult": {
        "tables": [
        {
            "name": "PrimaryResult",
            "columns": [
            {
                "name": "TenantId",
                "type": "string"
            },
            {
                "name": "Computer",
                "type": "string"
            },
            {
                "name": "TimeGenerated",
                "type": "datetime"
            },
            {
                "name": "SourceSystem",
                "type": "string"
            },
            {
                "name": "StartTime",
                "type": "datetime"
            },
            {
                "name": "EndTime",
                "type": "datetime"
            },
            {
                "name": "ResourceUri",
                "type": "string"
            },
            {
                "name": "LinkedResourceUri",
                "type": "string"
            },
            {
                "name": "DataType",
                "type": "string"
            },
            {
                "name": "Solution",
                "type": "string"
            },
            {
                "name": "BatchesWithinSla",
                "type": "long"
            },
            {
                "name": "BatchesOutsideSla",
                "type": "long"
            },
            {
                "name": "BatchesCapped",
                "type": "long"
            },
            {
                "name": "TotalBatches",
                "type": "long"
            },
            {
                "name": "AvgLatencyInSeconds",
                "type": "real"
            },
            {
                "name": "Quantity",
                "type": "real"
            },
            {
                "name": "QuantityUnit",
                "type": "string"
            },
            {
                "name": "IsBillable",
                "type": "bool"
            },
            {
                "name": "MeterId",
                "type": "string"
            },
            {
                "name": "LinkedMeterId",
                "type": "string"
            },
            {
                "name": "Type",
                "type": "string"
            }
            ],
            "rows": [
            [
                "861bd466-5400-44be-9552-5ba40823c3aa",
                "-",
                "2017-09-26T13:59:59Z",
                "OMS",
                "2017-09-26T13:00:00Z",
                "2017-09-26T13:59:59Z",
            "/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.operationalinsights/workspaces/workspace-861bd466-5400-44be-9552-5ba40823c3aa",
                null,
                "Operation",
                "LogManagement",
                8,
                0,
                0,
                8,
                0,
                0.002502,
                "MBytes",
                false,
                "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "00000000-0000-0000-0000-000000000000",
                "Usage"
            ]
            ]
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Critical"
    }



## <a name="next-steps"></a>Sonraki adımlar
- İzlenecek yollar için tamamlamak [bir webook yapılandırma](log-analytics-alerts-webhooks.md) bir uyarı kuralı ile.  
- Nasıl yazılacağını öğrenmek [Azure automation'daki runbook'lar](https://azure.microsoft.com/documentation/services/automation) uyarılar tarafından tanımlanan sorunları düzeltmek için.
