---
title: İş akışı tetikleyiciler ve Eylemler - Azure Logic Apps | Microsoft Docs
description: Tetikleyiciler ve logic apps ile otomatik iş akışları ve işlemleri oluşturmak için Eylemler hakkında bilgi edinin
services: logic-apps
author: divyaswarnkar
manager: anneta
editor: ''
documentationcenter: ''
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/13/2017
ms.author: klam; LADocs
ms.openlocfilehash: 28d28888ce66c354da39dc636579655aadbb9e51
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="triggers-and-actions-for-logic-app-workflows"></a>Tetikleyiciler ve Eylemler mantığı uygulama iş akışları için

Tüm mantıksal uygulamalar eylemleri tarafından izlenen bir tetikleyici ile başlayın. Bu makalede, tetikleyiciler ve sistem tümleştirmeler oluşturma ve logic apps oluşturarak iş iş akışları veya işlemleri otomatikleştirmek için kullanabileceğiniz eylemleri türleri açıklanmaktadır. 
  
## <a name="triggers-overview"></a>Tetikleyiciler genel bakış 

Tüm mantıksal uygulamaları çalıştırmak bir mantıksal uygulama başlatabilirsiniz çağrıları belirten bir tetikleyici ile başlayın. Kullanabileceğiniz tetikleyici türleri şunlardır:

* A *yoklama* düzenli aralıklarla bir hizmetin HTTP uç noktası denetler tetikleyici
* A *itme* tetiklemek, çağıran [iş akışı hizmeti REST API'si](https://docs.microsoft.com/rest/api/logic/workflows)
  
Tüm Tetikleyicileri bu üst düzey öğeleri içerir:  
  
```json
"<myTriggerName>": {
    "type": "<triggerType>",
    "inputs": { <callSettings> },
    "recurrence": {  
        "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
        "interval": "<recurrence-interval-based-on-frequency>"
    },
    "conditions": [ <array-with-required-conditions> ],
    "splitOn": "<property-used-for-creating-runs>",
    "operationOptions": "<options-for-operations-on-the-trigger>"
}
```

## <a name="trigger-types-and-inputs"></a>Tetikleyici türleri ve girişleri  

Her tetikleyici türünün farklı bir arabirim vardır ve farklı *girişleri* davranışını tanımlar. 

| Tetikleyici türü | Açıklama | 
| ------------ | ----------- | 
| **Yineleme** | Tanımlanan bir zamanlamaya göre ateşlenir. Gelecekteki bir tarih ve Bu tetikleyici tetikleme süresi ayarlayabilirsiniz. Sıklığı temel alarak, kez da belirtebilirsiniz ve iş akışını çalıştırmak için gün. | 
| **İstek**  | Mantıksal uygulamanızı çağırabilirsiniz, bir uç nokta "manual" bir tetikleyici olarak da bilinir hale getirir. | 
| **HTTP** | Denetimleri, veya *yoklamalar*, bir HTTP web uç noktası. HTTP uç noktası "202" zaman uyumsuz desen kullanarak veya bir dizi dönerek, belirli bir tetikleme sözleşmesine uygun olmalıdır. | 
| **ApiConnection** | Yoklar gibi bir HTTP tetikleyicisi, ancak kullanan [Microsoft tarafından yönetilen API](../connectors/apis-list.md). | 
| **HTTPWebhook** | Mantıksal uygulamanızı gibi aranabilir bir uç nokta yapar **isteği** tetiklemek, ancak belirtilen URL kaydetme ve kaydını kaldırmak için çağırır. |
| **ApiConnectionWebhook** | Gibi çalışır **HTTPWebhook** tetikleyicisi, ancak Microsoft tarafından yönetilen API kullanır. | 
||| 

Daha fazla bilgi için bkz: [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md). 

<a name="recurrence-trigger"></a>

## <a name="recurrence-trigger"></a>Yineleme tetikleyici  

Bu tetikleyici yinelenme ve zamanlama belirttiğiniz göre çalışır ve düzenli olarak iş akışını çalıştırmak için kolay bir yol sağlar. 

Her gün çalışır bir temel yinelenme tetikleyici örneği aşağıdadır:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

Ayrıca, bir başlangıç tarihi ve saati tetikleyici tetikleme için zamanlayabilirsiniz. Örneğin, haftalık bir rapor her Pazartesi başlatmak için bu örnek gibi belirli Pazartesi başlatmak için mantıksal uygulama zamanlayabilirsiniz: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime": "2017-09-18T00:00:00Z"
    }
}
```

Bu tetikleyicinin tanımı aşağıda verilmiştir:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "second|minute|hour|day|week|month",
        "interval": <recurrence-interval-based-on-frequency>,
        "schedule": {
            // Applies only when frequency is Day or Week. Separate values with commas.
            "hours": [ <one-or-more-hour-marks> ], 
            // Applies only when frequency is Day or Week. Separate values with commas.
            "minutes": [ <one-or-more-minute-marks> ], 
            // Applies only when frequency is Week. Separate values with commas.
            "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
        },
        "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
        "timeZone": "<specify-time-zone>"
    }
}
```

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| frequency | Evet | Dize | Ne sıklıkta tetiği harekete için zaman birimi. Bu değerlerden yalnızca birini kullanın: "ikinci", "dakika", "saat", "gün", "hafta" veya "ay" | 
| interval | Evet | Tamsayı | İş akışı sıklığı temel alarak çalışan ne sıklıkta açıklar pozitif bir tamsayı. <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 1-12.000 saatleri </br>-Dakika: 1-72,000 dakika </br>-İkinci: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve sıklığı "ay" ise, yineleme 6 ayda olur. | 
| timeZone | Hayır | Dize | Bu tetikleyici kabul etmez olduğundan yalnızca bir başlangıç saati belirttiğinizde uygulanır [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini belirtin. | 
| startTime | Hayır | Dize | Başlangıç tarihi ve saati bu biçimde belirtin: <p>YYYY-MM-ddTHH saat dilimini belirtirseniz, <p>-veya- <p>YYYY-MM-saat dilimi belirtmezseniz: ssZ <p>2: 00'dan 18 Eylül 2017 istiyorsanız, bu nedenle örneğin sonra belirtin "2017-09-18T14:00:00" ve "Pasifik Standart Saati" gibi bir saat dilimini belirtin. Ya da belirtin "2017-09-18T14:00:00Z" bir saat dilimi olmadan. <p>**Not:** bu başlangıç saati izlemelisiniz [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi belirtmezseniz, boşluk olmadan sonunda harf "Z" eklemeniz gerekir. Bu "Z" eşdeğer başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlama için başlangıç saatini ilk oluşum olduğu sırada karmaşık zamanlamalar, tetikleyici herhangi erken başlangıç saatinden harekete değil. Başlangıç tarih ve saat hakkında daha fazla bilgi için bkz: [oluşturma ve zamanlama düzenli olarak çalışan görevlerin](../connectors/connectors-native-recurrence.md). | 
| weekDays | Hayır | Dize veya dize dizisi | İçin "Hafta" belirtirseniz `frequency`, iş akışını çalıştırmak istediğinizde, virgülle ayrılmış bir veya daha fazla gün belirtebilirsiniz: "Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi" ve "Pazar" | 
| hours | Hayır | Tamsayı veya tamsayı dizisi | İçin "Gün" veya "Hafta" belirtirseniz, `frequency`, bir veya daha fazla tamsayılara 0'dan 23, iş akışını çalıştırmak istediğinizde gün saat virgülle ayırarak belirtebilirsiniz. <p>Örneğin, "10", "12" ve "14" belirtin, 10'da, 12 PM ve 2 PM saat işaretleri olarak alırsınız. | 
| minutes | Hayır | Tamsayı veya tamsayı dizisi | İçin "Gün" veya "Hafta" belirtirseniz, `frequency`, bir veya daha fazla tamsayılara 0'dan 59, iş akışını çalıştırmak istediğinizde saat dakika virgülle ayırarak belirtebilirsiniz. <p>Örneğin, "30" olarak dakika işaretle belirtebilirsiniz ve 10:30:00, almak için günün saati önceki örneği kullanarak 12:30 Şöyle ve 2:30 PM. | 
||||| 

Örneğin, bu yineleme tetikleyici mantıksal uygulamanızı 10: 30'de, haftalık her Pazartesi çalıştıracağını belirtir 12:30 Şöyle ve 2:30 PM 9 Eylül 2017 2: 00'dan, daha önce başlangıç Pasifik Standart Saati:

``` json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": 1,
        "schedule": {
            "hours": [
                10,
                12,
                14
            ],
            "minutes": [
                30
            ],
            "weekDays": [
                "Monday"
            ]
        },
       "startTime": "2017-09-07T14:00:00",
       "timeZone": "Pacific Standard Time"
    }
}
```

Yineleme ve başlangıç saati örnekler Bu tetikleyici için daha fazla bilgi için bkz: [oluşturma ve zamanlama düzenli olarak çalışan görevlerin](../connectors/connectors-native-recurrence.md).

## <a name="request-trigger"></a>Tetikleyici isteği

Bu tetikleyici mantıksal uygulamanızı bir HTTP isteği aracılığıyla çağırmak için kullanabileceğiniz bir uç nokta olarak görev yapar. Bir istek tetikleyici aşağıdaki gibi görünür:  
  
```json
"myRequestTrigger": {
    "type": "Request",
    "kind": "Http",
    "inputs": {
        "schema": {
            "type": "Object",
            "properties": {
                "myInputProperty1": { "type" : "string" },
                "myInputProperty2": { "type" : "number" }
            },
            "required": [ "myInputProperty1" ]
        }
    }
} 
```

Bu tetikleyici adlı isteğe bağlı bir özellik olan `schema`:
  
| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| Şema | Hayır | Nesne | JSON şeması gelen isteği doğrular. Sonraki iş akışı adımları başvurmak için hangi özelliklerin bilmeniz yardımcı olmak için kullanışlıdır. | 
||||| 

Bu tetikleyici bir uç nokta olarak çağrılacak çağırması gerekir `listCallbackUrl` API. Bkz: [iş akışı hizmeti REST API'si](https://docs.microsoft.com/rest/api/logic/workflows).

## <a name="http-trigger"></a>HTTP tetikleyicisi  

Bu tetikleyici belirtilen uç nokta yoklar ve iş akışı veya çalıştırılması gerekip gerekmediğini belirlemek için yanıtı denetler. Burada, `inputs` nesnesini bir HTTP çağrısıyla oluşturmak için gereken bu parametreleri alır: 

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| method | Evet | Dize | Bu HTTP yöntemlerinin birini kullanır: "GET", "POST", "PUT", "DELETE", "Düzeltme Eki" veya "HEAD" | 
| uri | Evet| Dize | Tetikleyici denetleyen HTTP veya HTTPs uç noktası. Maksimum dize boyutu: 2 KB | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | Nesne | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| kimlik doğrulaması | Hayır | Nesne | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). <p>Zamanlayıcı daha desteklenen bir özellik yok: `authority`. Varsayılan olarak, bu değer `https://login.windows.net` belirtilmediğinde, ancak farklı bir değer gibi kullanabilir`https://login.windows\-ppe.net`. | 
||||| 

A *yeniden deneme ilkesi* 408, 429 ve tüm bağlantı özel durumları yanı sıra 5xx aralıklı hatalar, HTTP durum kodları işlemleri için geçerlidir. Bu ilkeyle tanımlayabilirsiniz `retryPolicy` nesnesini aşağıda gösterildiği gibi:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

İyi mantığı uygulamanızın üzerinde çalışmak için belirli bir desen ile uygun olması HTTP API HTTP tetikleyicisi gerektirir. Tetikleyici bu özellikleri tanır:  
  
| Yanıt | Gerekli | Açıklama | 
| -------- | -------- | ----------- |  
| Durum kodu | Evet | Durum kodu 200 ("Tamam"), bir Farklı Çalıştır neden olur. Diğer bir durum kodu bir Farklı Çalıştır neden olmaz. | 
| Yeniden deneme sonrasında üstbilgisi | Hayır | Mantıksal uygulama uç nokta yeniden yoklar kadar saniye sayısı. | 
| Konum üstbilgisi | Hayır | Sonraki yoklama zaman aralığını çağrısı için URL. Belirtilmezse, özgün URL'si kullanılır. | 
|||| 

Farklı türde istekleri için bazı örnek davranışları şunlardır:
  
| Yanıt kodu | Süre sonunda yeniden dene | Davranış | 
| ------------- | ----------- | -------- | 
| 200 | {none} | İş akışı çalıştırın, sonra için daha fazla veri tanımlanmış yinelenme sonra yeniden denetleyin. | 
| 200 | 10 saniye | İş akışı çalıştırın, sonra daha fazla veri için 10 saniye sonra yeniden denetleyin. |  
| 202 | 60 saniye | İş akışını tetikleyen yok. Sonraki girişiminde tanımlı yinelenme tabi bir dakika içinde gerçekleşir. Tanımlanan yineleme bir dakikadan az ise, yeniden deneme sonrasında üstbilgi önceliklidir. Aksi halde, tanımlanmış yinelenme kullanılır. | 
| 400 | {none} | Hatalı istek, iş akışını çalıştırma. Öyle değilse `retryPolicy` tanımlanır, varsayılan ilke kullanılır. Yeniden deneme sayısı üst sınırına ulaşıldı sonra tetikleyici verileri için tanımlanan yineleme sonra yeniden denetler. | 
| 500 | {none}| Sunucu hatası, iş akışını çalıştırma. Öyle değilse `retryPolicy` tanımlanır, varsayılan ilke kullanılır. Yeniden deneme sayısı üst sınırına ulaşıldı sonra tetikleyici verileri için tanımlanan yineleme sonra yeniden denetler. | 
|||| 

HTTP tetikleyicisi çıkışları şunlardır: 
  
| Öğe adı | Tür | Açıklama |
| ------------ | ---- | ----------- |
| headers | Nesne | HTTP yanıtı üstbilgileri | 
| body | Nesne | HTTP yanıt gövdesi | 
|||| 

<a name="apiconnection-trigger"></a>

## <a name="apiconnection-trigger"></a>APIConnection tetikleyici  

Temel işlevleri, bu tetikleyici HTTP tetikleyicisini gibi çalışır. Bununla birlikte, eylem tanımlamak için farklı parametreleridir. Örnek aşağıda verilmiştir:   
  
```json
"myDailyReportTrigger": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            }
        },
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "myCategory"
    }
}
```

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| konak | Evet | Nesne | Barındırılan ağ geçidi ve API uygulaması için kimliği | 
| method | Evet | Dize | Bu HTTP yöntemlerinin birini kullanır: "GET", "POST", "PUT", "DELETE", "Düzeltme Eki" veya "HEAD" | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | Nesne | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| kimlik doğrulaması | Hayır | Nesne | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). | 
||||| 

İçin `host` nesne özellikleri şunlardır:  
  
| Öğe adı | Gerekli | Açıklama | 
| ------------ | -------- | ----------- | 
| API runtimeUrl | Evet | Yönetilen API uç noktası | 
| Bağlantı adı |  | İş akışının kullandığı yönetilen API bağlantı adı. Adlı bir parametre başvurmalıdır `$connection`. |
|||| 

A *yeniden deneme ilkesi* 408, 429 ve tüm bağlantı özel durumları yanı sıra 5xx aralıklı hatalar, HTTP durum kodları işlemleri için geçerlidir. Bu ilkeyle tanımlayabilirsiniz `retryPolicy` nesnesini aşağıda gösterildiği gibi:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Bir API bağlantısı tetikleyicisi çıktıların şunlardır:
  
| Öğe adı | Tür | Açıklama |
| ------------ | ---- | ----------- |
| headers | Nesne | HTTP yanıtı üstbilgileri | 
| body | Nesne | HTTP yanıt gövdesi | 
|||| 

Daha fazla bilgi edinmek [nasıl çalışır API bağlantısı için fiyatlandırma tetikler](../logic-apps/logic-apps-pricing.md#triggers).

## <a name="httpwebhook-trigger"></a>HTTPWebhook tetikleyici  

Bir uç nokta, benzer şekilde Bu tetikleyici sağlar `Request` tetikleyicisi ancak HTTPWebhook tetikleyici de çağırır belirtilen URL kaydetme ve kaydını kaldırmak için. Bir HTTPWebhook tetikleyicisi aşağıdaki gibi görünmelidir örneği şöyledir:

```json
"myAppsSpotTrigger": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": {},
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": {},
            "retryPolicy": {}
        },
        "unsubscribe": {
            "method": "POST",
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": {}
        }
    },
    "conditions": []
}
```

Bu bölümler çoğunu isteğe bağlıdır ve HTTPWebhook tetikleyici davranışı sağlayan veya atlayın bölümleri bağlıdır. HTTPWebhook tetikleyici özellikleri şunlardır:
  
| Öğe adı | Gerekli | Açıklama | 
| ------------ | -------- | ----------- |  
| abone olma | Hayır | Tetikleyici oluşturulduğunda ve ilk kaydı gerçekleştiren çağırmak için giden isteğini belirtir. | 
| Aboneliği Kaldır | Hayır | Tetikleyici silindiğinde çağırmak için giden isteğini belirtir. | 
|||| 

Aynı şekilde bir Web kancası tetikleyici sınırları belirtebilirsiniz [HTTP zaman uyumsuz sınırları](#asynchronous-limits). Hakkında daha fazla bilgi aşağıdadır `subscribe` ve `unsubscribe` eylemler:

* `subscribe` Tetikleyici olaylarını dinleme başlayabilmeniz için çağrılır. Standart HTTP Eylemler aynı parametrelerle bu giden çağrı başlatır. Bu çağrı, iş akışı herhangi bir şekilde, örneğin, kimlik bilgileri alınır veya tetikleyici giriş parametreleri değiştirdiğinizde değiştiğinde gerçekleşir. 
  
  Bu çağrı desteklemek için `@listCallbackUrl()` işlevi iş akışı içinde bu belirli tetikleyici için benzersiz bir URL döndürür. Bu URL hizmetin REST API kullanan uç noktaları için benzersiz tanımlayıcıyı temsil eder.
  
* `unsubscribe` bir işlem Bu tetikleyici geçersiz, bu işlemler de dahil olmak üzere işler olduğunda otomatik olarak çağrılır:

  * Silme veya tetikleyici devre dışı bırakma. 
  * Silme veya iş akışını devre dışı bırakma. 
  * Silme veya abonelik devre dışı bırakma. 
  
  Bu işlev parametrelerini HTTP tetikleyicisini ile aynıdır.

HTTPWebhook çıkışlarından tetikleyebilir ve gelen istek içeriği şunlardır:
  
| Öğe adı | Tür | Açıklama |
| ------------ | ---- | ----------- |
| headers | Nesne | HTTP yanıtı üstbilgileri | 
| body | Nesne | HTTP yanıt gövdesi | 
|||| 

## <a name="triggers-conditions"></a>Tetikleyiciler: koşulları

Herhangi bir tetikleyici için iş akışı veya çalıştırılması gerekip gerekmediğini belirlemek için bir veya daha fazla koşulları kullanabilirsiniz. Bu örnekte, rapor yalnızca Tetikleyicileri iken iş akışı `sendReports` parametrenin ayarlanmış true. 

```json
"myDailyReportTrigger": {
    "type": "Recurrence",
    "conditions": [ 
        {
            "expression": "@parameters('sendReports')"
        } 
    ],
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

Son olarak, koşullar tetikleyici durum kodunu başvuruda bulunabilir. Örneğin, yalnızca Web sitenizin bir durum kodu 500 döndürdüğünde bir iş akışı başlatabilirsiniz:
  
``` json
"conditions": [ 
    {  
      "expression": "@equals(triggers().code, 'InternalServerError')"  
    }  
]  
```  

> [!NOTE]
> Varsayılan olarak, bir tetikleyici harekete yalnızca alan tarafta bir "200 Tamam" yanıt. Bir ifadenin bir tetikleyicinin durum kodu herhangi bir şekilde başvurduğunda tetikleyici varsayılan davranışı değiştirilir. Bu nedenle, birden fazla durum kodları, örneğin temel yangın, durum kodu 200 ve 201 durum kodunu tetikleyicinin isterseniz, bu deyimi, koşul olarak şunları içermelidir: 
>
> `@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="triggers-process-an-array-with-multiple-runs"></a>Tetikleyicileri: birden çok çalıştırır sahip bir dizi işlem

Bazen, tetikleyici mantıksal uygulamanızı işlemek için bir dizi döndürürse, bir "için her" döngü her dizi öğesi işlemesi çok uzun sürebilir. Bunun yerine, kullanabileceğiniz **SplitOn** , tetikleyici özelliğinde *debatch* dizi. 

Debatching dizi öğelerini böler ve her bir dizi öğesi için çalıştıran yeni bir mantıksal uygulama örneğini başlatır. Bu yaklaşım, örneğin, birden çok yeni öğeler arasındaki yoklama aralıkları döndürebilir bir uç nokta yoklamak istediğinizde yararlıdır.
Dizi en fazla öğeleri için **SplitOn** işlem tek mantığı uygulama çalıştırılmasıyla, bkz: [sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md). 

> [!NOTE]
> Ekleyebileceğiniz **SplitOn** için el ile tanımlama veya kod görünümünde mantığı uygulamanızın JSON tanımı için geçersiz kılma tarafından yalnızca tetikler. Kullanamazsınız **SplitOn** zaman uyumlu yanıt desen uygulamak istediğinizde. Kullanan herhangi bir iş akışının **SplitOn** ve bir yanıt içeriyor eylem zaman uyumsuz olarak çalışır ve hemen gönderir bir `202 ACCEPTED` yanıt.

Tetikleyicinin Swagger dosyası bir dizi bir yükü tanımlarsa **SplitOn** özelliği, tetikleyici için otomatik olarak eklenir. Aksi takdirde, bu özellik, debatch istediğiniz dizi sahip yanıt yükünün içine ekleyin. 

Örneğin, bu yanıt döndüren bir API olduğunu varsayalım: 
  
```json
{
    "Status": "Succeeded",
    "Rows": [ 
        { 
            "id": 938109380,
            "name": "customer-name-one"
        },
        {
            "id": 938109381,
            "name": "customer-name-two"
        }
    ]
}
```
  
Mantıksal uygulamanızı içerikten yeterlidir `Rows`, böylece bu örnek gibi bir tetikleyici oluşturabilirsiniz.

``` json
"myDebatchTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "Second",
        "interval": "1"
    },
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> Kullanırsanız `SplitOn` komutu, dizinin dışına özellikleri alınamıyor. Bu örnek için alamaz `status` özelliği yanıtta döndürülen API'SİNDEN.
> 
> Bir hata durumunda önlemek için `Rows` özellik yok, bu örnek kullanır `?` işleci.

İş akışı tanımınızı artık kullanabilirsiniz `@triggerBody().name` almak için `customer-name-one` ilk çalıştırma ve `customer-name-two` ikinci çalıştırma. Bu nedenle, görünüm, tetikleyici çıkarır ister Bu örnekler:

```json
{
    "body": {
        "id": 938109380,
        "name": "customer-name-one"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "customer-name-two"
    }
}
```
  
## <a name="triggers-fire-only-after-all-active-runs-finish"></a>Tetikleyicileri: yalnızca tüm etkin çalıştırır son yangın

Yalnızca tüm etkin metinler tamamladığınızda yangın yinelenme Tetikleyicileri yapılandırabilirsiniz. Bu ayarı yapılandırmak için ayarlanmış `operationOptions` özelliğine `singleInstance`:

```json
"myTrigger": {
    "type": "Http",
    "inputs": { },
    "recurrence": { },
    "operationOptions": "singleInstance"
}
```

Bir iş akışı örneği çalışırken zamanlanmış bir yinelenme olursa, tetikleyici atlar ve yeniden denetlemek için bir sonraki zamanlanmış yinelenme aralığı kadar bekler.

## <a name="actions-overview"></a>Eylemler genel bakış

Eylemler, her benzersiz davranışına sahip birçok tür vardır. Her eylem türü bir eylemin davranışını tanımlayın farklı girişleri sahiptir. Koleksiyon eylemleri kendilerini içinde birçok diğer eylemler içerebilir. 

### <a name="standard-actions"></a>Standart Eylemler  

| Eylem türü | Açıklama | 
| ----------- | ----------- | 
| **HTTP** | Bir HTTP web uç noktası çağırır. | 
| **ApiConnection**  | HTTP eylemi gibi çalışır, ancak kullanan [Microsoft tarafından yönetilen API](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | HTTPWebhook gibi çalışır, ancak Microsoft tarafından yönetilen API'lerini kullanır. | 
| **Yanıt** | Gelen bir arama yanıtı tanımlar. | 
| **Oluştur** | Eylemin girişleri rasgele bir nesneden oluşturur. | 
| **İşlevi** | Bir Azure işlevi temsil eder. | 
| **bekleme** | Sabit bir tutar saat veya belirli bir süre kadar bekler. | 
| **İş akışı** | İç içe geçmiş iş akışını temsil eder. | 
| **Oluştur** | Eylemin girişleri rasgele bir nesneden oluşturur. | 
| **Sorgu** | Bir koşula göre bir dizi filtreler. | 
| **Seç** | Her öğe bir dizi yeni bir değer projeleri. Örneğin, bir dizi sayının nesnelerinin bir dizisi dönüştürebilirsiniz. | 
| **Tablo** | Öğeleri bir dizi bir CSV ya da HTML tabloya dönüştürür. | 
| **Sonlandırma** | Bir iş akışı çalışmayı durdurur. | 
| **bekleme** | Sabit bir tutar saat veya belirli bir süre kadar bekler. | 
| **İş akışı** | İç içe geçmiş iş akışını temsil eder. | 
||| 

### <a name="collection-actions"></a>Koleksiyon Eylemler

| Eylem türü | Açıklama | 
| ----------- | ----------- | 
| **If** | Bir ifade değerlendirme ve sonuca göre ilgili şube çalıştırılır. | 
| **geçiş** | Belirli bir nesne değerlerine göre farklı eylemleri gerçekleştirin. | 
| **ForEach** | Bu döngü eylem bir dizisini yineler ve her bir dizi öğede iç eylemleri gerçekleştirir. | 
| **kadar** | Bir koşul true olarak sonuçları kadar bu döngü eylem iç eylemleri gerçekleştirir. | 
| **Kapsam** | Diğer Eylemler mantıksal gruplandırma için kullanın. | 
|||  

## <a name="http-action"></a>HTTP eylemi  

Bir HTTP eylem belirtilen uç nokta çağırır ve iş akışı veya çalıştırılması gerekip gerekmediğini belirlemek için yanıtı denetler. Örneğin:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest"
    }
}
```

Burada, `inputs` nesnesini bir HTTP çağrısıyla oluşturmak için gereken bu parametreleri alır: 

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| method | Evet | Dize | Bu HTTP yöntemlerinin birini kullanır: "GET", "POST", "PUT", "DELETE", "Düzeltme Eki" veya "HEAD" | 
| uri | Evet| Dize | Tetikleyici denetleyen HTTP veya HTTPs uç noktası. Maksimum dize boyutu: 2 KB | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | Nesne | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Hayır | Dize | Geçersiz kılmak için özel davranışları kümesini tanımlar. | 
| kimlik doğrulaması | Hayır | Nesne | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). <p>Zamanlayıcı daha desteklenen bir özellik yok: `authority`. Varsayılan olarak, bu değer `https://login.windows.net` belirtilmediğinde, ancak farklı bir değer gibi kullanabilir`https://login.windows\-ppe.net`. | 
||||| 

HTTP eylemleri ve Destek APIConnection Eylemler *ilkeleri yeniden*. 408, 429 ve tüm bağlantı özel durumları yanı sıra 5xx aralıklı hatalar, HTTP durum kodları işlemleri için bir yeniden deneme ilkesi uygulanır. Bu ilkeyle tanımlayabilirsiniz `retryPolicy` nesnesini aşağıda gösterildiği gibi:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Bu örnek HTTP eylemi üç yürütmeleri ve 30 saniyelik gecikme her denemesi arasındaki toplam aralıklı hatalar varsa, en son haberleri iki kez getirme yeniden deneme sayısı:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy": {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

Yeniden deneme aralığını belirtilen [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601). En büyük değer bir saat olsa da bu aralığı 20 saniye varsayılan ve en az değerine sahip. Varsayılan ve en fazla yeniden deneme sayısı dört saattir. Bir yeniden deneme ilkesi tanımı belirtmezseniz bir `fixed` stratejisi varsayılan yeniden deneme sayısı ve aralığı değerlerle kullanılır. Yeniden deneme ilkesi devre dışı bırakmak için türünü ayarlamak `None`.

### <a name="asynchronous-patterns"></a>Zaman uyumsuz desenleri

Varsayılan olarak, tüm HTTP tabanlı eylemleri standart zaman uyumsuz işlem düzenini destekler. Bu nedenle Uzak sunucu isteği "202 kabul edilen" yanıt işleme için kabul edilir gösteriyorsa, Logic Apps altyapısı 202 olmayan yanıt terminal durumuna ulaşmasını kadar yanıtın konumu üstbilgisinde belirtilen URL yoklama tutar.
  
Daha önce açıklanan zaman uyumsuz davranışı devre dışı bırakmak için ayarlanmış `operationOptions` için `DisableAsyncPattern` eylem girişleri içinde. Bu durumda, eylemin çıkış sunucusundan ilk 202 yanıt temel alır. Örneğin:
  
```json
"invokeLongRunningOperationAction": {
    "type": "Http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

<a name="asynchronous-limits"></a>

#### <a name="asynchronous-limits"></a>Zaman uyumsuz sınırları

Belirli bir zaman aralığı için bir zaman uyumsuz desen süresini sınırlayabilirsiniz. Terminal durumuna erişmeden zaman aralığını aşılırsa, eylemin durumu işaretlenmiş `Cancelled` ile bir `ActionTimedOut` kodu. Sınır zaman aşımı ISO 8601 biçiminde belirtilir. Bu örnek, sınırları nasıl belirteceğinizi gösterir:


``` json
"<action-name>": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>APIConnection eylemi

Bu eylem geçerli bir bağlantı ve API ve parametreleri hakkında bilgi için bir başvuru gerektiren bir Microsoft tarafından yönetilen bağlayıcı başvurur. APIConnection eylemi örneği aşağıdadır:

```json
"Send_Email": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "POST",
        "body": {
            "Subject": "New tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
}
```

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| konak | Evet | Nesne | Bağlayıcı bilgisi gibi temsil eden `runtimeUrl` ve bağlantı nesnesine başvuru alınamıyor. | 
| method | Evet | Dize | Bu HTTP yöntemlerinin birini kullanır: "GET", "POST", "PUT", "DELETE", "Düzeltme Eki" veya "HEAD" | 
| yol | Evet | Dize | API işlem için yolu | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | Nesne | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Hayır | Dize | Geçersiz kılmak için özel davranışları kümesini tanımlar. | 
| kimlik doğrulaması | Hayır | Nesne | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
||||| 

408, 429 ve tüm bağlantı özel durumları yanı sıra 5xx aralıklı hatalar, HTTP durum kodları işlemleri için bir yeniden deneme ilkesi uygulanır. Bu ilkeyle tanımlayabilirsiniz `retryPolicy` nesnesini aşağıda gösterildiği gibi:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

## <a name="apiconnection-webhook-action"></a>APIConnection Web kancası eylemi

Microsoft tarafından yönetilen bir bağlayıcı APIConnectionWebhook eylem başvurur. Bu eylem geçerli bir bağlantı ve API ve parametreleri hakkında bilgi için bir başvuru gerektirir. Aynı şekilde bir Web kancası eylemini sınırları belirtebilirsiniz [HTTP zaman uyumsuz sınırları](#asynchronous-limits).

```json
"Send_approval_email": {
    "type": "ApiConnectionWebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| konak | Evet | Nesne | Bağlayıcı bilgisi gibi temsil eden `runtimeUrl` ve bağlantı nesnesine başvuru alınamıyor. | 
| yol | Evet | Dize | API işlem için yolu | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | Nesne | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Hayır | Dize | Geçersiz kılmak için özel davranışları kümesini tanımlar. | 
| kimlik doğrulaması | Hayır | Nesne | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Yanıt eylemi  

Bu eylem bir HTTP isteği tüm yanıt yükü içerir ve içeren bir `statusCode`, `body`, ve `headers`:
  
```json
"myResponseAction": {
    "type": "Response",
    "inputs": {
        "statusCode": 200,
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        }
    },
    "runAfter": {}
}
```

Yanıt eylemi diğer eylemler için özellikle uygulama özel kısıtlamaları vardır:  
  
* Gelen istek belirleyici yanıt gerektirdiğinden bir mantıksal uygulama tanımını içinde paralel dalları yanıt eylemleri sahip olamaz.
  
* Gelen istek zaten bir yanıt aldı sonra iş akışı bir yanıt eylemi ulaşırsa, yanıt eylem başarısız veya çakışan olarak kabul edilir. Çalıştırma mantıksal uygulama sonucu olarak işaretlenmiş `Failed`.
  
* Yanıt eylemleri içeren bir iş akışı kullanamazsınız `splitOn` çağrı birden çok çalıştırır oluşturduğundan tetikleyici tanımında komutu. İş akışı işlemi geçirdiğinizde bunun sonucunda, bu durumda denetleyin ve "hatalı istek" yanıt döndürür.

## <a name="compose-action"></a>Eylem oluşturma

Çıktı eylemin girişleri değerlendirme öğesinden oluşur ve bu eylem, rastgele bir nesne oluşturmak olanak sağlar. 

> [!NOTE]
> Kullanabileceğiniz `Compose` nesneleri, dizileri ve yerel olarak XML ve ikili gibi mantıksal uygulamalar tarafından desteklenen herhangi bir türü de dahil olmak üzere herhangi bir çıktı oluşturmak için eylem.

Örneğin, kullanabileceğiniz `Compose` birden çok eylem çıkışlarından birleştirmek için eylem:

```json
"composeUserRecordAction": {
    "type": "Compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
    }
}
```

## <a name="function-action"></a>İşlev eylemi

Bu eylemi temsil eder ve çağrı sağlayan bir [Azure işlevi](../azure-functions/functions-overview.md), örneğin:

```json
"<my-Azure-Function-name>": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/sites/<your-Azure-function-app-name>/functions/<your-Azure-function-name>"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()"
        },
        "method": "POST",
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- |  
| İşlev kimliği | Evet | Dize | Aramak istediğiniz Azure işlevi için kaynak kimliği. | 
| method | Hayır | Dize | Bir işlevi çağırmak için kullanılan HTTP yöntemi. Belirtilmezse, "POST" varsayılan yöntemdir. | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
|||||

Mantıksal uygulamanızı kaydettiğinizde, Logic Apps altyapısı başvurulan işlev üzerinde bazı denetimler gerçekleştirir:

* İşlev erişiminiz olması gerekir.
* Yalnızca standart HTTP tetikleyicisi veya genel JSON Web kancası tetikleyici kullanabilirsiniz.
* İşlevin tanımlı yol sahip olmamalıdır.
* Yalnızca "işlev" ve "anonim" yetkilendirme düzeylerini izin verilir.

> [!NOTE]
> Logic Apps altyapısı alır ve çalışma zamanında kullanılan tetikleme URL'si önbelleğe alır. Herhangi bir işlem önbelleğe alınmış URL'sini geçersiz kılar, dolayısıyla eylemi çalışma zamanında başarısız olur. Bu sorunu çözmek için almak ve tetikleme URL'si tekrar önbelleğe mantıksal uygulama neden olan mantıksal uygulama yeniden kaydedin.

## <a name="select-action"></a>Bir eylem seçin

Bu eylem, yeni bir değer dizideki her öğe proje olanak tanır. Bu örnek bir dizi sayının nesneleri bir diziye dönüştürür:

```json
"selectNumbersAction": {
    "type": "Select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| başlangıç | Evet | Dizi | Kaynak dizi |
| seç | Evet | Herhangi biri | Kaynak dizideki her öğe için uygulanan projeksiyonu |
||||| 

Çıktısını `select` giriş dizisi olarak aynı kardinalite sahip bir dizi eylemdir. Her öğe tarafından tanımlandığı şekilde dönüştürülmüş `select` özelliği. Girdi boş bir dizi çıkışı da boş bir dizi ise.

## <a name="terminate-action"></a>Sonlandırma eylemi

Bu eylem, devam eden herhangi bir eylem iptal etme ve kalan herhangi bir eylem atlanıyor bir iş akışı çalıştırma durdurur. Sonlandırma işlemi zaten tamamlanmış Eylemler etkilemez.

Örneğin, sahip bir farklı çalıştır durdurmak için `Failed` durumu:

```json
"HandleUnexpectedResponse": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response",
        }
    }
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Evet | Dize | Hedef çalıştırma olan durumu kullanıcının `Failed` veya `Cancelled` |
| runError | Hayır | Nesne | Hata ayrıntıları. Desteklenen tek zaman `runStatus` ayarlanır `Failed`. |
| runError kodu | Hayır | Dize | Çalışmanın hata kodu |
| runError iletisi | Hayır | Dize | Çalışmanın hata iletisi | 
||||| 

## <a name="query-action"></a>Sorgu eylemi

Bu eylem, bir dizi koşula göre filtre olanak tanır. 

> [!NOTE]
> Nesneleri, dizileri ve yerel olarak XML ve ikili gibi mantıksal uygulamalar tarafından desteklenen herhangi bir türü de dahil olmak üzere herhangi bir çıktı oluşturmak için Oluştur eylemi kullanamazsınız.

Örneğin, ikiden büyük sayılar seçmek için şunu yazın:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| başlangıç | Evet | Dizi | Kaynak dizi |
| Burada | Evet | Dize | Kaynak diziden her öğeye uygulanan koşulu. Hiçbir değer belirtecini karşılıyorsa `where` sonucu durumudur boş bir dizi. |
||||| 

Çıktısını `query` koşulu karşılıyor giriş dizisi öğelerinden sahip bir dizi eylemdir.

## <a name="table-action"></a>Tablo eylemi

Bu eylem, bir dizi bir CSV ya da HTML tabloya Dönüştür sağlar. 

```json
"ConvertToTable": {
    "type": "Table",
    "inputs": {
        "from": "<source-array>",
        "format": "CSV | HTML"
    }
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| başlangıç | Evet | Dizi | Kaynak dizi. Varsa `from` özellik değeri boş bir dizi, çıktı boş bir tablo. | 
| Biçimi | Evet | Dize | İstediğiniz tablo biçiminde "CSV" veya "HTML" | 
| sütunları | Hayır | Dizi | İstediğiniz tablo sütunları. Varsayılan Tablo Şekil geçersiz kılmak için kullanın. | 
| sütun başlığı | Hayır | Dize | Sütun başlığı | 
| Sütun değeri | Evet | Dize | Sütun değeri | 
||||| 

Bu örnek gibi bir tablo eylemi tanımladığınız varsayın:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "HTML"
    }
}
```

Ve bu dizi için `@triggerBody()`:

```json
[ {"ID": 0, "Name": "apples"},{"ID": 1, "Name": "oranges"} ]
```

Bu örnek çıktısı şöyledir:

<table><thead><tr><th>Kimlik</th><th>Ad</th></tr></thead><tbody><tr><td>0</td><td>elmalar</td></tr><tr><td>1</td><td>portakallar</td></tr></tbody></table>

Bu tablo özelleştirmek için sütunları açıkça, örneğin belirtebilirsiniz:

```json
"ConvertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [ 
            {
                "header": "Produce ID",
                "value": "@item().id"
            },
            {
              "header": "Description",
              "value": "@concat('fresh ', item().name)"
            }
        ]
    }
}
```

Bu örnek çıktısı şöyledir:

<table><thead><tr><th>Kimliği oluşturmak</th><th>Açıklama</th></tr></thead><tbody><tr><td>0</td><td>Yeni elmalar</td></tr><tr><td>1</td><td>Yeni portakallar</td></tr></tbody></table>

## <a name="wait-action"></a>Eylem bekleyin  

Bu eylem, belirtilen zaman aralığı için iş akışı yürütme askıya alır. Bu örnek, 15 dakika bekleyin iş akışının neden olur:
  
```json
"waitForFifteenMinutesAction": {
    "type": "Wait",
    "inputs": {
        "interval": {
            "unit": "minute",
            "count": 15
        }
    }
}
```
  
Alternatif olarak, zaman içinde belirli bir süre kadar beklemek için bu örnek kullanabilirsiniz:
  
```json
"waitUntilOctoberAction": {
    "type": "Wait",
    "inputs": {
        "until": {
            "timestamp": "2017-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> İle bekleme süresi belirtebilirsiniz `interval` nesne veya `until` nesnesi, ancak ikisini birden değil.

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| geçerliliği: | Hayır | Nesne | Zaman içinde bir noktadaki dayalı bekleme süresi | 
| zaman damgası kadar | Evet | Dize | Zamanda nokta [UTC tarih saat biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) bekleme süresi dolduğunda | 
| interval | Hayır | Nesne | Aralığı birimi ve sayısı bağlı bekleme süresi | 
| aralığı birimi | Evet | Dize | Zaman birimi. Bu değerlerden yalnızca birini kullanın: "ikinci", "dakika", "saat", "gün", "hafta" veya "ay" | 
| Aralık sayısı | Evet | Tamsayı | Bekleme süresi için kullanılan aralık birim sayısını temsil eden pozitif bir tamsayı | 
||||| 

## <a name="workflow-action"></a>İş akışı eylemi

Bu eylem, bir iş akışı iç içe sağlar. Logic Apps altyapısı alt iş akışı, bir erişim denetimi gerçekleştirir alt iş akışını erişiminizin olması gerekir böylece daha belirgin olarak tetiklenecek. Örneğin:

```json
"<my-nested-workflow-action-name>": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/<my-subscription-ID>/resourceGroups/<my-resource-group-name>/providers/Microsoft.Logic/<my-nested-workflow-action-name>",
            "triggerName": "mytrigger001"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- |  
| ana bilgisayar kimliği | Evet | Dize| Aramak istediğiniz iş akışı için kaynak kimliği | 
| ana bilgisayar tetikleyiciadı | Evet | Dize | Çağrılacak istediğiniz Tetikleyici adı | 
| Sorguları | Hayır | Nesne | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | Nesne | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | Nesne | Uç noktasına gönderilen yükünü temsil eder. | 
||||| 

Bu eylemin çıkış ne de tanımladığınız üzerinde dayalı `Response` alt iş akışı için eylem. Alt iş akışını tanımlamıyorsa bir `Response` eylem çıkışları boş.

## <a name="collection-actions-overview"></a>Koleksiyon Eylemler genel bakış

Yardımcı olmak için iş akışı yürütme denetlemek, koleksiyon eylemler diğer eylemler içerebilir. Bir koleksiyonda koleksiyon dışında Eylemler başvurduğu doğrudan başvurabilir. Örneğin, tanımladığınız bir `Http` bir kapsam, ardından eylemi `@body('http')` herhangi bir iş akışında hala geçerli olduğu. Ayrıca, bir koleksiyon eylemleri yalnızca "Diğer Eylemler aynı koleksiyonunda çalıştırabilirsiniz".

## <a name="if-action"></a>Eylem

Koşullu bir açıklamadır, bu eylem, bir durumu değerlendirin ve doğru olarak değerlendirir üzerinde ifade tabanlı bir dal yürütme sağlar. Koşul başarılı bir şekilde doğru olarak değerlendirilirse, koşul "Başarılı" işaretlenir. Bulunan Eylemler `actions` veya `else` nesneleri değerlendirmek için bu değerleri:

* "Çalıştırın ve başarılı olduğunda başarılı oldu"
* "Çalıştırdığınızda ve başarısız başarısız oldu"
* "Skipped" ilgili şube çalıştırdığınızda değil

Daha fazla bilgi edinmek [logic apps koşullu ifadeler](../logic-apps/logic-apps-control-flow-conditional-statement.md).

``` json
"<my-condition-name>": {
  "type": "If",
  "expression": "<condition>",
  "actions": {
    "if-true-run-this-action": {
      "type": <action-type>,
      "inputs": {},
      "runAfter": {}
    }
  },
  "else": {
    "actions": {
        "if-false-run-this-action": {
            "type": <action-type>,
            "inputs": {},
            "runAfter": {}
        }
    }
  },
  "runAfter": {}
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| Eylemler | Evet | Nesne | Ne zaman çalışması için iç Eylemler `expression` değerlendiren `true` | 
| ifade | Evet | Dize | Değerlendirilecek ifade |
| else | Hayır | Nesne | Ne zaman çalışması için iç Eylemler `expression` değerlendiren `false` |
||||| 

Örneğin:

```json
"myCondition": {
    "type": "If",
    "actions": {
        "if-true-check-this-website": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://this-url"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if-false-check-this-other-website": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://this-other-url"
                },
                "runAfter": {}
            }
        }
    }
}
```  

### <a name="how-conditions-can-use-expressions-in-actions"></a>Koşullar ifadeleri eylemleri nasıl kullanabileceğinizi

İfadeler koşullarında nasıl kullanabileceğinizi gösteren bazı örnekler şunlardır:
  
| JSON ifade | Sonuç | 
| --------------- | ------ | 
| `"expression": "@parameters('hasSpecialAction')"` | Geçirmek bu koşul true neden olarak veren herhangi bir değer. Yalnızca Boole ifadeleri destekler. Diğer türleri Boolean değerine dönüştürmek için bu işlevlerini kullanın: `empty` veya `equals` | 
| `"expression": "@greater(actions('action1').output.value, parameters('threshold'))"` | Karşılaştırma işlevlerini destekler. Yalnızca action1 çıktısını eşik değerden daha büyük olduğunda bu örnekte, eylem çalıştırır. | 
| `"expression": "@or(greater(actions('action1').output.value, parameters('threshold')), less(actions('action1').output.value, 100))"` | İç içe geçmiş Boole ifadeleri oluşturmak için mantığı işlevlerini destekler. Action1 çıktısını eşikten ya da 100 altında daha fazla olduğunda bu örnekte, eylem çalışır. | 
| `"expression": "@equals(length(actions('action1').outputs.errors), 0))"` | Bir dizinin tüm öğeleri olup olmadığını denetlemek için dizi işlevleri kullanabilirsiniz. Hataları dizi boş olduğunda bu örnekte, eylem çalıştırır. | 
| `"expression": "parameters('hasSpecialAction')"` | Bu ifade bir hataya neden olur ve geçerli bir koşul değil. Koşullar kullanmalıdır "@" simgesi. | 
||| 

## <a name="switch-action"></a>Anahtar eylemi

Switch deyimi Bu eylem, bir nesne, ifade ya da belirteci belirli değerlerine göre farklı eylemleri gerçekleştirir. Bu eylem nesne, ifade ya da belirtecinde değerlendirir, sonuç eşleşen ve Eylemler için yalnızca bu durumda çalışan durum seçer. Hiçbir durumda sonucu eşleştiğinde, varsayılan eylem çalıştırır. Switch deyimi çalıştığında, yalnızca bir örnek sonuç eşleşmelidir. Daha fazla bilgi edinmek [switch ifadeleri logic apps içinde](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"<my-switch-statement-name>": {
   "type": "Switch",
   "expression": "<evaluate-this-object-expression-token>",
   "cases": {
      "myCase1" : {
         "actions" : {
           "myAction1": {}
         },
         "case": "<result1>"
      },
      "myCase2": {
         "actions" : {
           "myAction2": {}
         },
         "case": "<result2>"
      }
   },
   "default": {
      "actions": {
          "myDefaultAction": {}
      }
   },
   "runAfter": {}
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| ifade | Evet | Dize | Nesne, ifade ya da değerlendirmek için belirteci | 
| Durumları | Evet | Nesne | İfade sonucuna göre çalıştır iç Eylemler kümesi içerir. | 
| Durumu | Evet | Dize | Sonucu ile eşleşecek değer | 
| Eylemler | Evet | Nesne | Deyimin sonucu eşleşen çalışması için çalıştırma iç eylemleri | 
| varsayılan | Hayır | Nesne | Hiçbir örnek sonucu eşleştiğinde çalışacak iç eylemleri | 
||||| 

Örneğin:

``` json
"myApprovalEmailAction": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions" : {
           "Send_an_email": {...}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions" : {
           "Send_an_email_2": {...}
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

## <a name="foreach-action"></a>Foreach eylemi

Bu döngü eylem bir dizisini yineler ve her bir dizi öğede iç eylemleri gerçekleştirir. Varsayılan olarak, paralel Foreach döngüsü çalışır. Paralel sayısı, "her için" döngüleri için döngüler çalıştırın, bkz: [sınırları ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). Her döngü sıralı olarak çalışacak şekilde ayarlanmış `operationOptions` parametresi `Sequential`. Daha fazla bilgi edinmek [Foreach döngüsü logic apps içinde](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop).

```json
"<my-forEach-loop-name>": {
    "type": "Foreach",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    },
    "foreach": "<array>",
    "runAfter": {}
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| Eylemler | Evet | Nesne | Döngünün içinde çalıştırmak için iç eylemleri | 
| foreach | Evet | Dize | Dizi yinelemek için | 
| operationOptions | Hayır | Dize | Özelleştirme davranışı için herhangi bir işlemi seçeneği belirtir. Şu anda yalnızca destekler `Sequential` varsayılan davranışı paralel olduğu yineleme sırayla çalıştırmak için. |
||||| 

Örneğin:

```json
"forEach_EmailAction": {
    "type": "Foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "Send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "foreach": "@body('email_filter')",
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Eylem kadar

Bir koşul doğru olarak değerlendirir kadar bu döngü eylem iç eylemleri çalıştırır. Daha fazla bilgi edinmek ["kadar" mantıksal uygulamalar Döngülerde](../logic-apps/logic-apps-control-flow-loops.md#until-loop).

```json
 "<my-Until-loop-name>": {
    "type": "Until",
    "actions": {
        "myActionName": {
            "type": "<action-type>",
            "inputs": {},
            "runAfter": {}
        }
    },
    "expression": "<myCondition>",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- | 
| Eylemler | Evet | Nesne | Döngünün içinde çalıştırmak için iç eylemleri | 
| ifade | Evet | Dize | Her yinelemeden sonra değerlendirilecek ifade | 
| Sınırı | Evet | Nesne | Döngünün sınırlar. En az bir sınır tanımlamanız gerekir. | 
| sayı | Hayır | Tamsayı | Gerçekleştirmek için yineleme sayısı sınırı | 
| timeout | Hayır | Dize | Zaman aşımı sınırını [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601) ne kadar döngü çalışması gerektiğini belirtir |
||||| 

Örneğin:

```json
 "runUntilSucceededAction": {
    "type": "Until",
    "actions": {
        "Http": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 100,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

## <a name="scope-action"></a>Kapsam eylemi

Bu eylem mantıksal Grup eylemleri bir iş akışında sağlar. Kapsam Ayrıca kendi durumlarını tüm eylemleri bu kapsamı bitiş çalıştıran alır. Daha fazla bilgi edinmek [kapsamları](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md).

```json
"<my-scope-action-name>": {
    "type": "Scope",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    }
}
```

| Ad | Gerekli | Tür | Açıklama | 
| ---- | -------- | ---- | ----------- |  
| Eylemler | Evet | Nesne | Kapsam içinde çalıştırmak için iç eylemleri |
||||| 

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md)
* Daha fazla bilgi edinmek [iş akışı REST API'si](https://docs.microsoft.com/rest/api/logic/workflows)