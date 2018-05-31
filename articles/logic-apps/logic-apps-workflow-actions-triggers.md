---
title: İş akışı tetikleyiciler ve Eylemler - Azure Logic Apps | Microsoft Docs
description: Tetikleyiciler ve Eylemler Azure mantıksal uygulamaları için iş akışı tanımları hakkında bilgi edinin
services: logic-apps
author: kevinlam1
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 5/8/2018
ms.author: klam; LADocs
ms.openlocfilehash: 88ee3d810a80bed418e8dbafa4f3e35ccf5e85b1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33886791"
---
# <a name="triggers-and-actions-for-workflow-definitions-in-azure-logic-apps"></a>Tetikleyiciler ve Eylemler Azure Logic Apps içinde iş akışı tanımları

İçinde [Azure Logic Apps](../logic-apps/logic-apps-overview.md), Tetikleyicileri eylemleri tarafından izlenen tüm mantığı uygulama iş akışları başlayın. Bu makalede tetikleyiciler ve otomatik otomatikleştirme iş iş akışları veya tümleştirme çözümleri işlemlerin mantıksal uygulamalar oluşturmak için kullanabileceğiniz eylemleri açıklar. Logic apps Logic Apps Tasarımcısı ile görsel olarak ya da temel iş akışı tanımları ile doğrudan yazarak yapı [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md). Azure portal ya da Visual Studio kullanabilirsiniz. Bilgi nasıl [works tetikleyiciler ve Eylemler için fiyatlandırma](../logic-apps/logic-apps-pricing.md).

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Tetikleyiciler genel bakış

Tüm mantıksal uygulamalar oluşturmak ve bir mantıksal uygulama iş akışı başlatmalarını çağrıları tanımlayan bir tetikleyici ile başlayın. Kullanabileceğiniz tetikleyici türleri şunlardır:

* A *yoklama* düzenli aralıklarla bir hizmetin HTTP uç noktası denetler tetikleyici
* A *itme* tetiklemek, çağıran [iş akışı hizmeti REST API'si](https://docs.microsoft.com/rest/api/logic/workflows)
 
Bazı isteğe bağlıdır, ancak bu üst düzey öğeleri tüm Tetikleyicileri vardır:  
  
```json
"<triggerName>": {
   "type": "<triggerType>",
   "inputs": { "<trigger-behavior-settings>" },
   "recurrence": { 
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": "<recurrence-interval-based-on-frequency>"
   },
   "conditions": [ <array-with-required-conditions> ],
   "splitOn": "<property-used-for-creating-runs>",
   "operationOptions": "<optional-trigger-operations>"
}
```

*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*tetikleyiciadı*> | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici yazın, örneğin: "Http" veya "ApiConnection" | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
| yineleme | JSON nesnesi | Ne sıklıkta tetiği harekete açıklar aralığı ve sıklığı |  
| frequency | Dize | Ne sıklıkta tetiği harekete açıklar zaman biriminin: "Saniye", "dakika", "Saat", "Gün", "Hafta" veya "Ay" | 
| interval | Tamsayı | Ne sıklıkta tetiği harekete açıklar pozitif bir tamsayı sıklığı temel alarak. <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 1-12.000 saatleri </br>-Dakika: 1-72,000 dakika </br>-İkinci: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve sıklığı "ay" ise, yineleme 6 ayda olur. | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [Koşullar](#trigger-conditions) | Dizi | İş akışını çalıştırmak gerekip gerekmediğini belirlemek bir veya daha fazla koşulları | 
| [splitOn](#split-on-debatch) | Dize | Yukarı böler bir deyim veya *debatches*, işleme için birden çok iş akışı örneği içine öğeleri dizisi. Bu seçenek, bir dizi döndürecek Tetikleyiciler için kullanılabilir ve yalnızca doğrudan kod görünümünde çalışırken. | 
| [operationOptions](#trigger-operation-options) | Dize | Bazı tetikleyiciler varsayılan tetikleyici davranışını değiştirmenize izin ek seçenekler sağlar | 
||||| 

## <a name="trigger-types-and-details"></a>Tetikleyici türleri ve ayrıntıları  

Her tetikleyici türü farklı bir arabirim ve tetikleyici davranışını tanımlayın girişleri sahiptir. 

| Tetikleyici türü | Açıklama | 
| ------------ | ----------- | 
| [**Yineleme**](#recurrence-trigger) | Tanımlanan bir zamanlamaya göre ateşlenir. Gelecekteki bir tarih ve Bu tetikleyici tetikleme süresi ayarlayabilirsiniz. Sıklığı temel alarak, kez da belirtebilirsiniz ve iş akışını çalıştırmak için gün. | 
| [**İstek**](#request-trigger)  | Mantıksal uygulamanızı bir aranabilir uç noktada, "manual" bir tetikleyici olarak da bilinir hale getirir. Örneğin, [çağrısı, tetikleyici veya HTTP uç noktaları iş akışlarıyla iç içe](../logic-apps/logic-apps-http-endpoint.md). | 
| [**HTTP**](#http-trigger) | Denetimleri, veya *yoklamalar*, bir HTTP web uç noktası. HTTP uç noktası "202" zaman uyumsuz desen kullanarak veya bir dizi dönerek, belirli tetikleyici sözleşmesine uygun olmalıdır. | 
| [**ApiConnection**](#apiconnection-trigger) | HTTP tetikleyicisini gibi çalışır, ancak kullanan [Microsoft tarafından yönetilen API](../connectors/apis-list.md). | 
| [**HTTPWebhook**](#httpwebhook-trigger) | İstek tetikleyici gibi çalışır, ancak belirtilen URL kaydetme ve kaydını kaldırmak için çağırır. |
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | HTTPWebhook tetikleyici gibi çalışır, ancak kullanan [Microsoft tarafından yönetilen API](../connectors/apis-list.md). | 
||| 

<a name="recurrence-trigger"></a>

## <a name="recurrence-trigger"></a>Yineleme tetikleyici  

Bu tetikleyici, belirtilen yinelenme ve zamanlamaya göre çalışır ve düzenli olarak iş akışını çalıştırmak için kolay bir yol sağlar. 

Tetikleyici tanımı aşağıda verilmiştir:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month",
      "interval": <recurrence-interval-based-on-frequency>,
      "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
      "timeZone": "<time-zone>",
      "schedule": {
         // Applies only when frequency is Day or Week. Separate values with commas.
         "hours": [ <one-or-more-hour-marks> ], 
         // Applies only when frequency is Day or Week. Separate values with commas.
         "minutes": [ <one-or-more-minute-marks> ], 
         // Applies only when frequency is Week. Separate values with commas.
         "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
      }
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```
*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| Yineleme | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici türü olan "Recurrence" | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
| yineleme | JSON nesnesi | Ne sıklıkta tetiği harekete açıklar aralığı ve sıklığı |  
| frequency | Dize | Ne sıklıkta tetiği harekete açıklar zaman biriminin: "Saniye", "dakika", "Saat", "Gün", "Hafta" veya "Ay" | 
| interval | Tamsayı | Ne sıklıkta tetiği harekete açıklar pozitif bir tamsayı sıklığı temel alarak. <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 1-12.000 saatleri </br>-Dakika: 1-72,000 dakika </br>-İkinci: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve sıklığı "ay" ise, yineleme 6 ayda olur. | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| startTime | Dize | Başlangıç tarihi ve saati şu biçimde: <p>YYYY-MM-ddTHH saat dilimini belirtirseniz, <p>-veya- <p>YYYY-MM-saat dilimi belirtmezseniz: ssZ <p>2: 00'dan 18 Eylül 2017 istiyorsanız, bu nedenle örneğin sonra belirtin "2017-09-18T14:00:00" ve "Pasifik Standart Saati" gibi bir saat dilimini belirtin ya da belirtin "2017-09-18T14:00:00Z" bir saat dilimi olmadan. <p>**Not:** bu başlangıç saati izlemelisiniz [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi belirtmezseniz, boşluk olmadan sonunda harf "Z" eklemeniz gerekir. Bu "Z" eşdeğer başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlama için başlangıç saatini ilk oluşum olduğu sırada karmaşık zamanlamalar, tetikleyici herhangi erken başlangıç saatinden harekete değil. Başlangıç tarih ve saat hakkında daha fazla bilgi için bkz: [oluşturma ve zamanlama düzenli olarak çalışan görevlerin](../connectors/connectors-native-recurrence.md). | 
| timeZone | Dize | Bu tetikleyici kabul etmez olduğundan yalnızca bir başlangıç saati belirttiğinizde uygulanır [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini belirtin. | 
| hours | Tamsayı veya tamsayı dizisi | İçin "Gün" veya "Hafta" belirtirseniz, `frequency`, bir veya daha fazla tamsayılara 0'dan 23, iş akışını çalıştırmak istediğinizde gün saat virgülle ayırarak belirtebilirsiniz. <p>Örneğin, "10", "12" ve "14" belirtin, 10'da, 12 PM ve 2 PM saat işaretleri olarak alırsınız. | 
| minutes | Tamsayı veya tamsayı dizisi | İçin "Gün" veya "Hafta" belirtirseniz, `frequency`, bir veya daha fazla tamsayılara 0'dan 59, iş akışını çalıştırmak istediğinizde saat dakika virgülle ayırarak belirtebilirsiniz. <p>Örneğin, "30" olarak dakika işaretle belirtebilirsiniz ve 10:30:00, almak için günün saati önceki örneği kullanarak 12:30 Şöyle ve 2:30 PM. | 
| weekDays | Dize veya dize dizisi | İçin "Hafta" belirtirseniz `frequency`, iş akışını çalıştırmak istediğinizde, virgülle ayrılmış bir veya daha fazla gün belirtebilirsiniz: "Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi" ve "Pazar" | 
| Eşzamanlılık | JSON nesnesi | Bu nesne için yinelenen ve yoklama tetikleyicileri, aynı anda çalıştırabilirsiniz iş akışı örneği en fazla sayısını belirtir. Arka uç sistemleri alma isteklerini sınırlamak için bu değeri kullanın. <p>Örneğin, bu değer 10 örneklerine eşzamanlılık sınırını ayarlar: `"concurrency": { "runs": 10 }` | 
| operationOptions | Dize | `singleInstance` Seçeneği, yalnızca tüm etkin metinler sona erdikten sonra tetikleyici ateşlenir belirtir. Bkz: [Tetikleyicileri: yalnızca etkin bitiş çalıştıktan sonra yangın](#single-instance). | 
|||| 

*Örnek 1*

Bu temel yinelenme tetikleyici her gün çalışır:

```json
"recurrenceTriggerName": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

*Örnek 2*

Bir başlangıç tarihi ve saati tetikleyici tetikleme için belirtebilirsiniz. Bu yinelenme tetikleyici belirtilen tarihte başlar ve günlük başlatılır:

```json
"recurrenceTriggerName": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1,
      "startTime": "2017-09-18T00:00:00Z"
   }
}
```

*Örnek 3*

Bu yinelenme tetikleyici 9 Eylül 2017 2: 00'dan başlar ve her Pazartesi haftalık 10: 30'da, harekete 12:30 Şöyle ve 2:30 PM Pasifik Standart Saati:

``` json
"myRecurrenceTrigger": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "hours": [ 10, 12, 14 ],
         "minutes": [ 30 ],
         "weekDays": [ "Monday" ]
      },
      "startTime": "2017-09-07T14:00:00",
      "timeZone": "Pacific Standard Time"
   }
}
```

Daha fazla bilgi ayrıca bu tetikleyicinin örnekler için bkz: [oluşturma ve zamanlama düzenli olarak çalışan görevlerin](../connectors/connectors-native-recurrence.md).

<a name="request-trigger"></a>

## <a name="request-trigger"></a>Tetikleyici isteği

Bu tetikleyici, gelen HTTP isteklerini kabul edebilecek bir uç nokta oluşturarak mantıksal uygulamanızı aranabilir yapar. Bu tetikleyici çağırmak için kullanmanız gerekir `listCallbackUrl` API [iş akışı hizmeti REST API'si](https://docs.microsoft.com/rest/api/logic/workflows). Bu tetikleyici bir HTTP uç noktası olarak kullanmayı öğrenmek için bkz: [çağrısı, tetikleyici veya HTTP uç noktaları iş akışlarıyla iç içe](../logic-apps/logic-apps-http-endpoint.md).

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "GET | POST | PUT | PATCH | DELETE | HEAD",
      "relativePath": "<relative-path-for-accepted-parameter>",
      "schema": {
         "type": "object",
         "properties": { 
            "<propertyName>": {
               "type": "<property-type>"
            }
         },
         "required": [ "<required-properties>" ]
      }
   }
}
```

*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| El ile | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici türü olan "İstek" | 
| türü | Dize | "Http" istek türü | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| method | Dize | Tetikleyici çağırmak için istekleri kullanmalıdır yöntemi: "GET", "PUT", "POST", "Düzeltme Eki", "DELETE" veya "HEAD" |
| RelativePath | Dize | HTTP uç noktanın URL'sini kabul parametresi için göreli yolu | 
| Şema | JSON nesnesi | Açıklayan ve yükü doğrular JSON Şeması veya tetikleyici gelen istekte alan girişleri. Bu şemayı başvurmak için özellikler bilmeniz sonraki iş akışı eylemlerinin yardımcı olur. | 
| properties | JSON nesnesi | Bir veya daha fazla özelliklerinde yükü tanımlayan JSON şeması | 
| Gerekli | Dizi | Değerleri için gereken bir veya daha fazla özellikleri | 
|||| 

*Örnek*

Bu istek tetikleyici gelen istek tetikleyici ve gelen istek girişini doğrulayan bir şema çağırmak için HTTP POST yöntemini kullandığını belirtir: 

```json
"myRequestTrigger": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "POST",
      "schema": {
         "type": "Object",
         "properties": {
            "customerName": {
               "type": "String"
            },
            "customerAddress": { 
               "type": "Object",
               "properties": {
                  "streetAddress": {
                     "type": "String"
                  },
                  "city": {
                     "type": "String"
                  }
               }
            }
         }
      }
   }
} 
```

<a name="http-trigger"></a>

## <a name="http-trigger"></a>HTTP tetikleyicisi  

Bu tetikleyici belirtilen uç nokta yoklar ve yanıt denetler. Yanıt, iş akışı veya çalıştırılması gerekip gerekmediğini belirler. `inputs` JSON nesnesi içerir ve gerektirir `method` ve `uri` HTTP çağrısıyla oluşturmak için gerekli parametreleri:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET | PUT | POST | PATCH | DELETE | HEAD",
      "uri": "<HTTP-or-HTTPS-endpoint-to-poll>",
      "queries": "<query-parameters>",
      "headers": { "<headers-for-request>" },
      "body": { "<payload-to-send>" },
      "authentication": { "<authentication-method>" },
      "retryPolicy": {
          "type": "<retry-policy-type>",
          "interval": "<retry-interval>",
          "count": <number-retry-attempts>
      }
   },
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": <recurrence-interval-based-on-frequency>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```

*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| HTTP | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici türü olan "Http" | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
| method | Evet | Dize | Belirtilen uç nokta yoklama için HTTP yöntemini: "GET", "PUT", "POST", "Düzeltme Eki", "DELETE" veya "HEAD" | 
| uri | Evet| Dize | Tetikleyici ya da yoklar HTTP veya HTTPS uç noktası URL'si <p>Maksimum dize boyutu: 2 KB | 
| yineleme | JSON nesnesi | Ne sıklıkta tetiği harekete açıklar aralığı ve sıklığı |  
| frequency | Dize | Ne sıklıkta tetiği harekete açıklar zaman biriminin: "Saniye", "dakika", "Saat", "Gün", "Hafta" veya "Ay" | 
| interval | Tamsayı | Ne sıklıkta tetiği harekete açıklar pozitif bir tamsayı sıklığı temel alarak. <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 1-12.000 saatleri </br>-Dakika: 1-72,000 dakika </br>-İkinci: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve sıklığı "ay" ise, yineleme 6 ayda olur. | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| sorgu | JSON nesnesi | URL ile dahil etmek istediğiniz tüm sorgu parametreleri <p>Örneğin, bu öğeyi ekler `?api-version=2015-02-01` sorgu URL dizesi: <p>`"queries": { "api-version": "2015-02-01" }` <p>Sonuç: `https://contoso.com?api-version=2015-02-01` | 
| headers | JSON nesnesi | İsteği göndermek için bir veya daha fazla üstbilgileri <p>Örneğin, dil ve bir istek türünü ayarlamak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | JSON nesnesi | Uç noktasına gönderilecek yükünü (veriler) | 
| kimlik doğrulaması | JSON nesnesi | Gelen istek kimlik doğrulaması için kullanması gereken yöntem. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). Zamanlayıcı, ötesinde `authority` özelliği desteklenir. Belirtilmediğinde, varsayılan değer: `https://login.windows.net`, ancak farklı bir değer gibi kullanabilir`https://login.windows\-ppe.net`. | 
| retryPolicy | JSON nesnesi | Bu nesne 4xx veya 5xx durum kodları sahip aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| Eşzamanlılık | JSON nesnesi | Bu nesne için yinelenen ve yoklama tetikleyicileri, aynı anda çalıştırabilirsiniz iş akışı örneği en fazla sayısını belirtir. Arka uç sistemleri alma isteklerini sınırlamak için bu değeri kullanın. <p>Örneğin, bu değer 10 örneklerine eşzamanlılık sınırını ayarlar: <p>`"concurrency": { "runs": 10 }` | 
| operationOptions | Dize | `singleInstance` Seçeneği, yalnızca tüm etkin metinler sona erdikten sonra tetikleyici ateşlenir belirtir. Bkz: [Tetikleyicileri: yalnızca etkin bitiş çalıştıktan sonra yangın](#single-instance). | 
|||| 

İyi mantığı uygulamanızın üzerinde çalışmak için HTTP API için belirli bir desen uygun HTTP tetikleyicisi gerektirir. HTTP tetikleyicisi bu özellikleri tanır:  
  
| Yanıt | Gerekli | Açıklama | 
| -------- | -------- | ----------- |  
| Durum kodu | Evet | "200 Tamam" durum kodu bir farklı çalıştır başlatır. Diğer bir durum kodu bir farklı çalıştır başlamıyor. | 
| Yeniden deneme sonrasında üstbilgisi | Hayır | Mantıksal uygulama uç nokta yeniden yoklar kadar saniye sayısı | 
| Konum üstbilgisi | Hayır | Sonraki yoklama zaman aralığını çağrısı için URL. Belirtilmezse, özgün URL'si kullanılır. | 
|||| 

*Farklı istekleri için örnek davranışları*

| Durum kodu | Süre sonunda yeniden dene | Davranış | 
| ----------- | ----------- | -------- | 
| 200 | {none} | İş akışı çalıştırın, sonra için daha fazla veri tanımlanmış yinelenme sonra yeniden denetleyin. | 
| 200 | 10 saniye | İş akışı çalıştırın, sonra daha fazla veri için 10 saniye sonra yeniden denetleyin. |  
| 202 | 60 saniye | İş akışını tetikleyen yok. Sonraki girişiminde tanımlı yinelenme tabi bir dakika içinde gerçekleşir. Tanımlanan yineleme bir dakikadan az ise, yeniden deneme sonrasında üstbilgi önceliklidir. Aksi halde, tanımlanmış yinelenme kullanılır. | 
| 400 | {none} | Hatalı istek, iş akışını çalıştırma. Öyle değilse `retryPolicy` tanımlanır, varsayılan ilke kullanılır. Yeniden deneme sayısı üst sınırına ulaşıldı sonra tetikleyici verileri için tanımlanan yineleme sonra yeniden denetler. | 
| 500 | {none}| Sunucu hatası, iş akışını çalıştırma. Öyle değilse `retryPolicy` tanımlanır, varsayılan ilke kullanılır. Yeniden deneme sayısı üst sınırına ulaşıldı sonra tetikleyici verileri için tanımlanan yineleme sonra yeniden denetler. | 
|||| 

### <a name="http-trigger-outputs"></a>HTTP tetikleyicisini çıkarır

| Öğe adı | Tür | Açıklama |
| ------------ | ---- | ----------- |
| headers | JSON nesnesi | HTTP yanıtı üstbilgileri | 
| body | JSON nesnesi | HTTP yanıtı gövdesinden | 
|||| 

<a name="apiconnection-trigger"></a>

## <a name="apiconnection-trigger"></a>APIConnection tetikleyici  

Bu tetikleyici gibi çalışır [HTTP tetikleyicisini](#http-trigger), ancak kullanır [Microsoft tarafından yönetilen API](../connectors/apis-list.md) Bu tetikleyici parametrelerini farklı şekilde. 

Tetikleyici davranışı bölümleri dahil olup olmadığı hakkında dolayısıyla birçok bölümleri isteğe bağlıdır ancak tetikleyicinizin tanımını şöyledir:

```json
"<APIConnectionTriggerName>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "api": {
            "runtimeUrl": "<managed-API-endpoint-URL>"
         },
         "connection": {
            "name": "@parameters('$connections')['<connection-name>'].name"
         },
      },
      "method": "GET | PUT | POST | PATCH | DELETE | HEAD",
      "queries": "<query-parameters>",
      "headers": { "<headers-for-request>" },
      "body": { "<payload-to-send>" },
      "authentication": { "<authentication-method>" },
      "retryPolicy": {
          "type": "<retry-policy-type>",
          "interval": "<retry-interval>",
          "count": <number-retry-attempts>
      }
   },
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": "<recurrence-interval-based-on-frequency>"
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```

*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| *APIConnectionTriggerName* | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici türü olan "ApiConnection" | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
| konak | JSON nesnesi | Kimliği ve ana bilgisayar ağ geçidi için yönetilen API tanımlayan JSON nesnesinin <p>`host` JSON nesnesi bu öğeleri sahiptir: `api` ve `connection` | 
| api | JSON nesnesi | Yönetilen API uç noktası URL'si: <p>`"runtimeUrl": "<managed-API-endpoint-URL>"` | 
| bağlantı | JSON nesnesi | Bağlantı için ad, adlı bir parametre için bir başvuru içermelidir iş akışının kullandığı, yönetilen API `$connection`: <p>`"name": "@parameters('$connections')['<connection-name>'].name"` | 
| method | Dize | Yönetilen API'si ile iletişim kurmak için HTTP yöntemini: "GET", "PUT", "POST", "Düzeltme Eki", "DELETE" veya "HEAD" | 
| yineleme | JSON nesnesi | Ne sıklıkta tetiği harekete açıklar aralığı ve sıklığı |  
| frequency | Dize | Ne sıklıkta tetiği harekete açıklar zaman biriminin: "Saniye", "dakika", "Saat", "Gün", "Hafta" veya "Ay" | 
| interval | Tamsayı | Ne sıklıkta tetiği harekete açıklar pozitif bir tamsayı sıklığı temel alarak. <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 1-12.000 saatleri </br>-Dakika: 1-72,000 dakika </br>-İkinci: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve sıklığı "ay" ise, yineleme 6 ayda olur. | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| sorgu | JSON nesnesi | URL ile dahil etmek istediğiniz tüm sorgu parametreleri <p>Örneğin, bu öğeyi ekler `?api-version=2015-02-01` sorgu URL dizesi: <p>`"queries": { "api-version": "2015-02-01" }` <p>Sonuç: `https://contoso.com?api-version=2015-02-01` | 
| headers | JSON nesnesi | İsteği göndermek için bir veya daha fazla üstbilgileri <p>Örneğin, dil ve bir istek türünü ayarlamak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | JSON nesnesi | Yönetilen API'sine göndermek için (veri) yükü tanımlayan JSON nesnesi | 
| kimlik doğrulaması | JSON nesnesi | Gelen istek kimlik doğrulaması için kullanması gereken yöntem. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
| retryPolicy | JSON nesnesi | Bu nesne 4xx veya 5xx durum kodları sahip aralıklı hatalar için yeniden deneme davranışı özelleştirir: <p>`"retryPolicy": { "type": "<retry-policy-type>", "interval": "<retry-interval>", "count": <number-retry-attempts> }` <p>Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| Eşzamanlılık | JSON nesnesi | Bu nesne için yinelenen ve yoklama tetikleyicileri, aynı anda çalıştırabilirsiniz iş akışı örneği en fazla sayısını belirtir. Arka uç sistemleri alma isteklerini sınırlamak için bu değeri kullanın. <p>Örneğin, bu değer 10 örneklerine eşzamanlılık sınırını ayarlar: `"concurrency": { "runs": 10 }` | 
| operationOptions | Dize | `singleInstance` Seçeneği, yalnızca tüm etkin metinler sona erdikten sonra tetikleyici ateşlenir belirtir. Bkz: [Tetikleyicileri: yalnızca etkin bitiş çalıştıktan sonra yangın](#single-instance). | 
||||

*Örnek*

```json
"Create_daily_report": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "api": {
            "runtimeUrl": "https://myReportsRepo.example.com/"
         },
         "connection": {
            "name": "@parameters('$connections')['<connection-name>'].name"
         }     
      },
      "method": "POST",
      "body": {
         "category": "statusReports"
      }  
   },
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

### <a name="apiconnection-trigger-outputs"></a>APIConnection tetikleyici çıkarır
 
| Öğe adı | Tür | Açıklama |
| ------------ | ---- | ----------- |
| headers | JSON nesnesi | HTTP yanıtı üstbilgileri | 
| body | JSON nesnesi | HTTP yanıtı gövdesinden | 
|||| 

<a name="httpwebhook-trigger"></a>

## <a name="httpwebhook-trigger"></a>HTTPWebhook tetikleyici  

Bu tetikleyici gibi çalışır [isteği tetikleyici](#request-trigger) mantığı uygulamanız için çağrılabilir bir uç noktası oluşturarak. Ancak, bu tetikleyici de belirtilen uç nokta URL'si kaydetme veya bir abonelik kaydı için çağırır. Aynı şekilde bir Web kancası tetikleyici sınırları belirtebilirsiniz [HTTP zaman uyumsuz sınırları](#asynchronous-limits). 

İşte tetikleyicinizin tanımını birçok bölümleri isteğe bağlıdır ancak ve tetikleyici davranışı kullanın veya atlayın bölümleri bağlıdır:

```json
"HTTP_Webhook": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "<subscribe-to-endpoint-URL>",
            "headers": { "<headers-for-request>" },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "<subscription-topic>"
            },
            "authentication": {},
            "retryPolicy": {}
        },
        "unsubscribe": {
            "method": "POST",
            "url": "<unsubscribe-from-endpoint-URL>",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "<subscription-topic>"
            },
            "authentication": {}
        }
    },
}
```

*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| HTTP_Webhook | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici türü olan "HttpWebhook" | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
| abone olma | JSON nesnesi| Giden istek çağırın ve tetikleyici oluşturduğunuzda ilk kaydı gerçekleştirmek için. Tetikleyici olayları uç noktada dinleme başlayabilmeniz için bu çağrı yapılır. Daha fazla bilgi için bkz: [abone olma ve aboneliği](#subscribe-unsubscribe). | 
| method | Dize | Abonelik istek için kullanılan HTTP yöntemini: "GET", "PUT", "POST", "Düzeltme Eki", "DELETE" veya "HEAD" | 
| uri | Dize | Abonelik isteğinin gönderileceği yeri için uç nokta URL'si | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| Aboneliği Kaldır | JSON nesnesi | Giden istek otomatik olarak çağırmak ve bir işlem tetikleyici geçersiz yaptığında, aboneliği iptal et. Daha fazla bilgi için bkz: [abone olma ve aboneliği](#subscribe-unsubscribe). | 
| method | Dize | İptal isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "Düzeltme Eki", "DELETE" veya "HEAD" | 
| uri | Dize | İptal isteğini gönderileceği yeri için uç nokta URL'si | 
| body | JSON nesnesi | Abonelik veya iptal isteği yükünü (veri) tanımlayan JSON nesnesinin | 
| kimlik doğrulaması | JSON nesnesi | Gelen istek kimlik doğrulaması için kullanması gereken yöntem. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
| retryPolicy | JSON nesnesi | Bu nesne 4xx veya 5xx durum kodları sahip aralıklı hatalar için yeniden deneme davranışı özelleştirir: <p>`"retryPolicy": { "type": "<retry-policy-type>", "interval": "<retry-interval>", "count": <number-retry-attempts> }` <p>Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
|||| 

*Örnek*

```json
"myAppSpotTrigger": {
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
      },
      "unsubscribe": {
         "method": "POST",
         "url": "https://pubsubhubbub.appspot.com/subscribe",
         "body": {
            "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
            "hub.mode": "unsubscribe",
            "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
         },
      }
   },
}
```

<a name="subscribe-unsubscribe"></a>

### <a name="subscribe-and-unsubscribe"></a>`subscribe` ve `unsubscribe`

`subscribe` Çağrısı, iş akışı herhangi bir şekilde, örneğin, kimlik bilgileri yenilenmesi veya tetikleyici parametreleri değişiklik giriş değiştiğinde gerçekleşir. Çağrı aynı parametreleri standart HTTP eylem olarak kullanır. 
 
`unsubscribe` Çağrısı bir işlem HTTPWebhook tetikleyici örneğin geçersiz yaptığında, otomatik olarak gerçekleşir:

* Silme veya tetikleyici devre dışı bırakma. 
* Silme veya iş akışını devre dışı bırakma. 
* Silme veya abonelik devre dışı bırakma. 

Bu çağrı desteklemek için `@listCallbackUrl()` işlevi döndürür benzersiz bu tetikleyicinin "geri çağırma URL'si". Bu URL hizmetin REST API kullanan uç noktaları için benzersiz bir tanımlayıcı temsil eder. Bu işlev parametrelerini HTTP tetikleyicisini ile aynıdır.

### <a name="httpwebhook-trigger-outputs"></a>HTTPWebhook tetikleyici çıkarır

| Öğe adı | Tür | Açıklama |
| ------------ | ---- | ----------- |
| headers | JSON nesnesi | HTTP yanıtı üstbilgileri | 
| body | JSON nesnesi | HTTP yanıtı gövdesinden | 
|||| 

<a name="apiconnectionwebhook-trigger"></a>

## <a name="apiconnectionwebhook-trigger"></a>ApiConnectionWebhook tetikleyici

Bu tetikleyici gibi çalışır [HTTPWebhook tetikleyici](#httpwebhook-trigger), ancak kullanır [Microsoft tarafından yönetilen API](../connectors/apis-list.md). 

Tetikleyici tanımı aşağıda verilmiştir:

```json
"<ApiConnectionWebhookTriggerName>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },        
      "body": {
          "NotificationUrl": "@{listCallbackUrl()}"
      },
      "queries": "<query-parameters>"
   }
}
```

*Gerekli*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ApiConnectionWebhookTriggerName*> | JSON nesnesi | Javascript nesne gösterimi (JSON) biçiminde açıklandığı bir nesnedir tetikleyici için ad  | 
| type | Dize | Tetikleyici türü olan "ApiConnectionWebhook" | 
| Girişleri | JSON nesnesi | Tetikleyici davranışını tanımlayın tetikleyici girişleri | 
| konak | JSON nesnesi | Kimliği ve ana bilgisayar ağ geçidi için yönetilen API tanımlayan JSON nesnesinin <p>`host` JSON nesnesi bu öğeleri sahiptir: `api` ve `connection` | 
| bağlantı | JSON nesnesi | Bağlantı için ad, adlı bir parametre için bir başvuru içermelidir iş akışının kullandığı, yönetilen API `$connection`: <p>`"name": "@parameters('$connections')['<connection-name>']['connectionId']"` | 
| body | JSON nesnesi | Yönetilen API'sine göndermek için (veri) yükü tanımlayan JSON nesnesi | 
| NotificationUrl | Dize | Benzersiz bir döndürür yönetilen API'sini kullanabilirsiniz Bu tetikleyicinin "geri çağırma URL'si" | 
|||| 

*İsteğe bağlı*

| Öğe adı | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| sorgu | JSON nesnesi | URL ile dahil etmek istediğiniz tüm sorgu parametreleri <p>Örneğin, bu öğeyi ekler `?folderPath=Inbox` sorgu URL dizesi: <p>`"queries": { "folderPath": "Inbox" }` <p>Sonuç: `https://<managed-API-URL>?folderPath=Inbox` | 
|||| 

<a name="trigger-conditions"></a>

## <a name="triggers-conditions"></a>Tetikleyiciler: koşulları

Herhangi bir tetikleyici için bir dizi iş akışı veya çalıştırılması gerekip gerekmediğini belirlemek bir veya daha fazla koşulları ile ekleyebilirsiniz. Bu örnekte, rapor tetiği harekete yalnızca iş akışı `sendReports` parametrenin ayarlanmış true. 

```json
"myDailyReportTrigger": {
   "type": "Recurrence",
   "conditions": [ {
      "expression": "@parameters('sendReports')"
   } ],
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

Ayrıca, koşullar tetikleyici durum kodu başvuruda bulunabilir. Örneğin, yalnızca Web sitenizin bir "500" durum kodu döndürdüğünde bir iş akışı başlatmak istediğinizi varsayalım:

``` json
"conditions": [ {
   "expression": "@equals(triggers().code, 'InternalServerError')"  
} ]  
```  

> [!NOTE]
> Varsayılan olarak, bir tetikleyici harekete yalnızca alan tarafta bir "200 Tamam" yanıt. Bir ifadenin bir tetikleyicinin durum kodu herhangi bir şekilde başvurduğunda tetikleyici varsayılan davranışı değiştirilir. Bu nedenle, birden fazla durum kodu için örneğin, durum kodu 200 ve durum kodu 201, tetiklenecek tetikleyicinin isterseniz, bu deyimi, koşul olarak şunları içermelidir: 
>
> `@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="triggers-split-an-array-into-multiple-runs"></a>Tetikleyicileri: Bölünmüş bir diziye birden çok çalıştırır

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
        "interval": 1
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

<a name="trigger-operation-options"></a>

## <a name="triggers-operation-options"></a>Tetikleyicileri: İşlem seçenekleri

Bu tetikleyicileri varsayılan davranışını değiştirmenize izin daha fazla seçenekler sağlar.

| Tetikleyici | İşlemi seçeneği | Açıklama |
|---------|------------------|-------------|
| [Yineleme](#recurrence-trigger), <br>[HTTP](#http-trigger), <br>[ApiConnection](#apiconnection-trigger) | daıtmaktadır | Yalnızca tüm etkin tetikleyici çalıştıran yangın tamamlandı. |
||||

<a name="single-instance"></a>

### <a name="triggers-fire-only-after-active-runs-finish"></a>Tetikleyicileri: yalnızca etkin bitiş çalıştıktan ateşle

Yinelenme ayarlayabileceğiniz Tetikleyiciler için tetikleyici yangın yalnızca tüm etkin çalıştırır bitirdikten belirtebilirsiniz. Bir iş akışı örneği çalışırken zamanlanmış bir yinelenme olursa, tetikleyici atlar ve zamanlanan bir sonraki yeniden denetlemeden önce yinelenme kadar bekler. Örneğin:

```json
"myRecurringTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Hour",
        "interval": 1,
    },
    "operationOptions": "singleInstance"
}
```

<a name="actions-overview"></a>

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
| sorgu | Hayır | JSON nesnesi | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | JSON nesnesi | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | JSON nesnesi | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | JSON nesnesi | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Hayır | Dize | Geçersiz kılmak için özel davranışları kümesini tanımlar. | 
| kimlik doğrulaması | Hayır | JSON nesnesi | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). <p>Zamanlayıcı daha desteklenen bir özellik yok: `authority`. Varsayılan olarak, bu değer `https://login.windows.net` belirtilmediğinde, ancak farklı bir değer gibi kullanabilir`https://login.windows\-ppe.net`. | 
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
| konak | Evet | JSON nesnesi | Bağlayıcı bilgisi gibi temsil eden `runtimeUrl` ve bağlantı nesnesine başvuru alınamıyor. | 
| method | Evet | Dize | Bu HTTP yöntemlerinin birini kullanır: "GET", "POST", "PUT", "DELETE", "Düzeltme Eki" veya "HEAD" | 
| yol | Evet | Dize | API işlem için yolu | 
| sorgu | Hayır | JSON nesnesi | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | JSON nesnesi | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | JSON nesnesi | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | JSON nesnesi | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Hayır | Dize | Geçersiz kılmak için özel davranışları kümesini tanımlar. | 
| kimlik doğrulaması | Hayır | JSON nesnesi | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
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
| konak | Evet | JSON nesnesi | Bağlayıcı bilgisi gibi temsil eden `runtimeUrl` ve bağlantı nesnesine başvuru alınamıyor. | 
| yol | Evet | Dize | API işlem için yolu | 
| sorgu | Hayır | JSON nesnesi | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | JSON nesnesi | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | JSON nesnesi | Uç noktasına gönderilen yükünü temsil eder. | 
| retryPolicy | Hayır | JSON nesnesi | Bu nesne 4xx veya 5xx hataları yeniden deneme davranışını özelleştirmek için kullanın. Daha fazla bilgi için bkz: [yeniden deneme ilkelerini](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Hayır | Dize | Geçersiz kılmak için özel davranışları kümesini tanımlar. | 
| kimlik doğrulaması | Hayır | JSON nesnesi | İstek kimlik doğrulaması için kullanması gereken yöntemi temsil eder. Daha fazla bilgi için bkz: [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
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
| sorgu | Hayır | JSON nesnesi | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | JSON nesnesi | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | JSON nesnesi | Uç noktasına gönderilen yükünü temsil eder. | 
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
| runError | Hayır | JSON nesnesi | Hata ayrıntıları. Desteklenen tek zaman `runStatus` ayarlanır `Failed`. |
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
| geçerliliği: | Hayır | JSON nesnesi | Zaman içinde bir noktadaki dayalı bekleme süresi | 
| zaman damgası kadar | Evet | Dize | Zamanda nokta [UTC tarih saat biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) bekleme süresi dolduğunda | 
| interval | Hayır | JSON nesnesi | Aralığı birimi ve sayısı bağlı bekleme süresi | 
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
| sorgu | Hayır | JSON nesnesi | URL'de dahil edilmesini istediğiniz herhangi bir sorgu parametre temsil eder. <p>Örneğin, `"queries": { "api-version": "2015-02-01" }` ekler `?api-version=2015-02-01` URL. | 
| headers | Hayır | JSON nesnesi | İstekte gönderilen her bir başlığı temsil eder. <p>Örneğin, dilini ayarlamak ve bir istek yazmak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Hayır | JSON nesnesi | Uç noktasına gönderilen yükünü temsil eder. | 
||||| 

Bu eylemin çıkış ne de tanımladığınız üzerinde dayalı `Response` alt iş akışı için eylem. Alt iş akışını tanımlamıyorsa bir `Response` eylem çıkışları boş.

## <a name="collection-actions-overview"></a>Koleksiyon Eylemler genel bakış

Yardımcı olmak için iş akışı yürütme denetlemek, koleksiyon eylemler diğer eylemler içerebilir. Bir koleksiyonda koleksiyon dışında Eylemler başvurduğu doğrudan başvurabilir. Örneğin, tanımladığınız bir `Http` bir kapsam, ardından eylemi `@body('http')` herhangi bir iş akışında hala geçerli olduğu. Ayrıca, bir koleksiyon eylemleri yalnızca "Diğer Eylemler aynı koleksiyonunda çalıştırabilirsiniz".

## <a name="if-action"></a>Eylem

Koşullu bir açıklamadır, bu eylem, bir durumu değerlendirin ve doğru olarak değerlendirir üzerinde ifade tabanlı bir dal yürütme sağlar. Koşul başarılı bir şekilde doğru olarak değerlendirilirse, koşul "Başarılı" durumu ile işaretlenir. Bulunan Eylemler `actions` veya `else` nesneleri değerlendirmek için bu değerleri:

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
| Eylemler | Evet | JSON nesnesi | Ne zaman çalışması için iç Eylemler `expression` değerlendiren `true` | 
| ifade | Evet | Dize | Değerlendirilecek ifade |
| else | Hayır | JSON nesnesi | Ne zaman çalışması için iç Eylemler `expression` değerlendiren `false` |
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
| `"expression": "parameters('hasSpecialAction')"` | Bu ifade bir hataya neden olur ve geçerli bir koşul değil. Koşullar kullanmalıdır "\@" simgesi. | 
||| 

## <a name="switch-action"></a>Anahtar eylemi

Switch deyimi Bu eylem, bir nesne, ifade ya da belirteci belirli değerlerine göre farklı eylemleri gerçekleştirir. Bu eylem nesne, ifade ya da belirtecinde değerlendirir, sonuç eşleşen ve Eylemler için yalnızca bu durumda çalışan durum seçer. Hiçbir durumda sonucu eşleştiğinde, varsayılan eylem çalıştırır. Switch deyimi çalıştığında, yalnızca bir örnek sonuç eşleşmelidir. Daha fazla bilgi edinmek [switch ifadeleri logic apps içinde](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"<my-switch-statement-name>": {
   "type": "Switch",
   "expression": "<evaluate-this-object-expression-token>",
   "cases": {
      "myCase1": {
         "actions": {
           "myAction1": {}
         },
         "case": "<result1>"
      },
      "myCase2": {
         "actions": {
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
| Durumları | Evet | JSON nesnesi | İfade sonucuna göre çalıştır iç Eylemler kümesi içerir. | 
| Durumu | Evet | Dize | Sonucu ile eşleşecek değer | 
| Eylemler | Evet | JSON nesnesi | Deyimin sonucu eşleşen çalışması için çalıştırma iç eylemleri | 
| varsayılan | Hayır | JSON nesnesi | Hiçbir örnek sonucu eşleştiğinde çalışacak iç eylemleri | 
||||| 

Örneğin:

``` json
"myApprovalEmailAction": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {...}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions": {
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
| Eylemler | Evet | JSON nesnesi | Döngünün içinde çalıştırmak için iç eylemleri | 
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
| Eylemler | Evet | JSON nesnesi | Döngünün içinde çalıştırmak için iç eylemleri | 
| ifade | Evet | Dize | Her yinelemeden sonra değerlendirilecek ifade | 
| Sınırı | Evet | JSON nesnesi | Döngünün sınırlar. En az bir sınır tanımlamanız gerekir. | 
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
| Eylemler | Evet | JSON nesnesi | Kapsam içinde çalıştırmak için iç eylemleri |
||||| 

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md)
* Daha fazla bilgi edinmek [iş akışı REST API'si](https://docs.microsoft.com/rest/api/logic/workflows)