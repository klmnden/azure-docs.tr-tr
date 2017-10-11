---
title: "OMS günlük analizi uyarı REST API kullanarak"
description: "Günlük analizi uyarı REST API, bu yer Operations Management Suite (OMS) günlük analizi uyarıları oluşturma ve yönetme olanak sağlar.  Bu makalede, farklı işlemler gerçekleştirmek için API ve çeşitli örnekler ayrıntıları sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5ce72ffef4394bf3bbe39fa420c4fcaa965ae35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Oluşturma ve uyarı kurallarında günlük analizi REST API ile yönetme
Günlük analizi uyarı REST API, uyarıları Operations Management Suite (OMS) oluşturma ve yönetme olanak sağlar.  Bu makalede, farklı işlemler gerçekleştirmek için API ve çeşitli örnekler ayrıntıları sağlar.

Günlük analizi arama REST API RESTful ve Azure Resource Manager REST API'si erişilebilir. Bu belgede API kullanarak bir PowerShell komut satırı burada erişilen örnekler bulacaksınız [ARMClient](https://github.com/projectkudu/ARMClient), Azure Kaynak Yöneticisi API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. ARMClient ve PowerShell kullanımını günlük analizi arama API erişmek için birçok seçenek biridir. Bu araçların OMS çalışma alanları çağrı yapmak ve bunların içindeki arama komutları gerçekleştirmek için RESTful Azure Kaynak Yöneticisi API'si kullanabilir. Arama sonuçlarını birçok farklı yolla programlı olarak kullanmanıza olanak sağlayan API arama sonuçlarını, JSON biçiminde çıktı.

## <a name="prerequisites"></a>Ön koşullar
Şu anda, uyarıları günlük analizi olarak kaydedilmiş bir aramayı ile yalnızca oluşturulabilir.  Başvurabilirsiniz [günlük Search REST API'sini](log-analytics-log-search-api.md) daha fazla bilgi için.

## <a name="schedules"></a>Zamanlamalar
Kaydedilmiş bir aramayı bir veya daha fazla zamanlama olabilir. Ne sıklıkta arama çalıştırma ve ölçütleri belirlenen zaman aralığı olan zamanlamayı tanımlar.
Zamanlamaları aşağıdaki tabloda özelliklere sahip.

| Özellik | Açıklama |
|:--- |:--- |
| aralığı |Arama ne sıklıkta çalıştırılır. Dakika cinsinden ölçülür. |
| QueryTimeSpan |Ölçüt değerlendirildiği zaman aralığı. Aralıktan büyük veya eşit olmalıdır. Dakika cinsinden ölçülür. |
| Sürüm |Kullanılan API sürümü.  Şu anda, bu her zaman 1 olarak ayarlanması gerekir. |

Örneğin, bir olay sorgusu bir aralığı 15 dakika ve 30 dakikalık bir zaman aralığı ile göz önünde bulundurun. Bu durumda, sorgu 15 dakikada bir çalışır ve ölçütlerini gerçek üzerinden çözümlemek etseydi uyarı tetikleyen 30 dakikalık aralık.

### <a name="retrieving-schedules"></a>Zamanlamalar alınıyor
Kayıtlı bir aramaya tüm zamanlamalar almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Get yöntemi bir zamanlama Kimliğiyle kaydedilmiş bir aramayı için belirli bir zamanlama almak için kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Bir zamanlama için bir örnek yanıt aşağıdadır.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Bir zamanlama oluşturma
Put yöntemini benzersiz zamanlama Kimliğine sahip yeni bir zamanlama oluşturmak için kullanın.  Farklı ile ilişkili olsalar bile, iki zamanlamaları aynı Kimliğe sahip olamaz, kayıtlı aramalar unutmayın.  OMS konsolunda bir zamanlama oluşturmak, bir GUID zamanlama kimliği için oluşturulur

> [!NOTE]
> Tüm kayıtlı aramaları, çizelgeler ve günlük analizi API ile oluşturulan eylemler için ad, küçük olması gerekir.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Bir zamanlama düzenleme
Bu zamanlamayı değiştirmek için aynı kayıtlı arama için bir zamanlama kimlikli Put yöntemini kullanın.  İstek gövdesini zamanlama etag eklemeniz gerekir.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Zamanlamalar silme
Delete yöntemi bir zamanlama Kimliğine sahip bir zamanlama silmek için kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Eylemler
Bir zamanlama birden çok eylemler olabilir. Bir posta gönderme veya bir runbook'u başlatma gibi gerçekleştirmek için bir veya daha fazla işlem bir eylem tanımlayabilir veya ne zaman bir arama sonuçlarını bazı ölçütlere uyan belirleyen bir eşik tanımlayabilir.  Böylece eşiğine ulaşıldığında işlemleri gerçekleştirilen bazı eylemler her ikisi de tanımlayacaksınız.

Tüm eylemler aşağıdaki tabloda özelliklere sahip.  Farklı türde bir uyarı aşağıda açıklanan farklı ek özellikler vardır.

| Özellik | Açıklama |
|:--- |:--- |
| Tür |Eylem türü.  Şu anda olası uyarı ve Web kancası değerlerdir. |
| Ad |Uyarı görünen adı. |
| Sürüm |Kullanılan API sürümü.  Şu anda, bu her zaman 1 olarak ayarlanması gerekir. |

### <a name="retrieving-actions"></a>Eylemler Alınıyor
Tüm eylemler için bir zamanlama almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Get yöntemini eylem Kimliğine sahip bir zamanlama için belirli bir eylem almak için kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Oluşturma veya Eylemler düzenleme
Yeni bir eylem oluşturmak için zamanlama için benzersiz olan bir eylem kimliği ile Put yöntemini kullanın.  OMS konsolunda bir eylem oluşturduğunuzda, bir GUID için eylem kimliğidir.

> [!NOTE]
> Tüm kayıtlı aramaları, çizelgeler ve günlük analizi API ile oluşturulan eylemler için ad, küçük olması gerekir.

Bu zamanlamayı değiştirmek için aynı kayıtlı arama için bir eylem kimliğiyle Put yöntemini kullanın.  İstek gövdesini zamanlama etag eklemeniz gerekir.

Aşağıdaki bölümlerde bu örnekler verilmiştir için yeni bir eylem oluşturmak için istek biçimi eylem türüne göre değişir.

### <a name="deleting-actions"></a>Eylemler siliniyor
Delete yöntemi eylem Kimlikli bir eylem silmek için kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Uyarı eylemleri
Bir zamanlama tek bir uyarı eylemi olması gerekir.  Uyarı eylemleri bir veya daha fazla aşağıdaki tabloda bölümler var.  Her daha aşağıda ayrıntılı olarak açıklanmıştır.

| Bölüm | Açıklama |
|:--- |:--- |
| Eşik |Eylem çalıştırıldığında ölçütlerini. |
| EmailNotification |Birden çok alıcıya posta gönderin. |
| Düzeltme |Bir runbook tanımlanan sorunu düzeltmeyi denemek için Azure Otomasyonu'nda başlatın. |

#### <a name="thresholds"></a>Eşikleri
Bir uyarı eylem tek bir eşik olması gerekir.  Kayıtlı arama sonuçlarını arama ile ilişkili bir eylem Eşikte eşleştiğinde, başka bir işlem bu uygulamada çalıştırılır.  Böylece eşikleri içermeyen diğer türleri Eylemler ile kullanılan bir eylem yalnızca bir eşik de içerebilir.

Eşikleri aşağıdaki tabloda özelliklere sahip.

| Özellik | Açıklama |
|:--- |:--- |
| işleci |Eşik karşılaştırma işleci. <br> gt şundan = <br> lt = küçüktür |
| Değer |Eşik değeri. |

Örneğin, bir olay sorgusu zaman aralığı 15 dakika, 30 dakikalık bir Timespan ve 10'dan büyük bir eşik ile göz önünde bulundurun. Bu durumda, sorgu 15 dakikada bir çalışır ve 30 dakikalık aralık oluşturulan 10 olayları döndürülen bir uyarı tetikleyen.

Aşağıdaki örnek yanıt için bir eylem yalnızca bir eşik ile ' dir.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Put yöntemini benzersiz eylem kimliği ile yeni bir eşik eylemi için bir zamanlama oluşturmak için kullanın.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Put yöntemini var olan bir eylem kimliği ile bir zamanlama için bir eşik eylemi değiştirmek için kullanın.  İstek gövdesini eylemin etag eklemeniz gerekir.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-posta bildirimi
E-posta bildirimleri bir veya daha fazla alıcıya posta gönderin.  Aşağıdaki tabloda özellikleri içerirler.

| Özellik | Açıklama |
|:--- |:--- |
| Alıcıları |Posta adresleri listesi. |
| Konu |Posta konusu. |
| Eki |Bu her zaman "Hiçbiri" değerine sahip şekilde ekleri şu anda, desteklenmez. |

Aşağıdaki örnek yanıt bir eşik ile bir e-posta bildirim eylemi için ' dir.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Put yöntemini benzersiz eylem kimliği ile yeni bir e-posta eylemi için bir zamanlama oluşturmak için kullanın.  Aşağıdaki örnek, bir e-posta bildirimi bir eşik ile oluşturur, bu nedenle kayıtlı arama sonuçlarını eşiği aştığında posta gönderilir.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Put yöntemini var olan bir eylem kimliği ile bir zamanlama için bir e-posta eylemi değiştirmek için kullanın.  İstek gövdesini eylemin etag eklemeniz gerekir.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Düzeltme eylemleri
Düzeltmeler, Azure automation'da uyarı tarafından tanımlanan sorunu düzeltmeye çalışır bir runbook başlatın.  Bir düzeltme eylemi kullanılan runbook için bir Web kancası oluşturun ve ardından URI WebhookUri özelliğinde belirtmeniz gerekir.  OMS konsolunu kullanarak bu eylemi oluşturduğunuzda, yeni bir Web kancası runbook için otomatik olarak oluşturulur.

Düzeltmeler aşağıdaki tabloda özellikleri içerir.

| Özellik | Açıklama |
|:--- |:--- |
| RunbookName |Runbook'un adı. Bu OMS çalışma alanınızdaki Otomasyon çözümünü yapılandırılan Otomasyon hesabı yayımlanan bir runbook'ta eşleşmelidir. |
| WebhookUri |Web kancası URI'si. |
| Süre sonu |Sona erme tarihi ve saati Web kancası.  Web kancası bir sona erme yoksa, bu geçerli bir gelecek tarih olabilir. |

Aşağıdaki örnek yanıt bir eşik ile bir düzeltme eylemi için ' dir.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Put yöntemini benzersiz eylem kimliği ile yeni bir düzeltme eylemi için bir zamanlama oluşturmak için kullanın.  Aşağıdaki örnek, bir düzeltme bir eşik ile oluşturur, bu nedenle kayıtlı arama sonuçlarını eşiği aştığında runbook başlatıldıktan.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Put yöntemini var olan bir eylem kimliği ile bir zamanlama için bir düzeltme eylemi değiştirmek için kullanın.  İstek gövdesini eylemin etag eklemeniz gerekir.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Örnek
Yeni bir e-posta Uyarı oluşturmak için tam bir örnek verilmiştir.  Bu eşik ve e-posta içeren bir eylem birlikte yeni bir zamanlama oluşturur.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Web kancası eylemleri
Web kancası eylemleri, bir URL çağırma ve isteğe bağlı olarak gönderilecek bir yükü sağlayarak bir işlem başlatın.  Azure Otomasyon çalışma kitabı dışındaki işlemler çağırabilir Web kancası için amacı dışında düzeltme eylemleri benzerdir.  Uzak işlem teslim edilecek bir yükü sağlama ek seçeneği de sağlar.

Web kancası eylemleri bir eşik gerekmez ancak bunun yerine bir uyarı eylem bir eşik ile sahip bir zamanlama eklenmelidir.  Eşiğine ulaşıldığında, tüm çalışan birden çok Web kancası eylemleri ekleyebilirsiniz.

Web kancası eylemleri aşağıdaki tabloda özellikleri içerir.

| Özellik | Açıklama |
|:--- |:--- |
| WebhookUri |Posta konusu. |
| CustomPayload |Web kancası için gönderilecek özel yükü.  Web kancası bekleniyor üzerinde biçimi bağlıdır. |

Web kancası eylem ve bir eşik ile ilişkili bir uyarı eylem için örnek yanıt aşağıdadır.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Oluşturma veya bir Web kancası eylemi düzenleme
Put yöntemini benzersiz eylem kimliği ile yeni bir Web kancası eylemi için bir zamanlama oluşturmak için kullanın.  Kayıtlı arama sonuçlarını eşiği aştığında Web kancası tetiklenen amacıyla aşağıdaki örnek bir Web kancası eylemi ve bir uyarı eylem bir eşik ile oluşturur.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Put yöntemini var olan bir eylem kimliği ile bir zamanlama için bir Web kancası eylemi değiştirmek için kullanın.  İstek gövdesini eylemin etag eklemeniz gerekir.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük aramalar gerçekleştirmek için REST API](log-analytics-log-search-api.md) günlük analizi içinde.

