---
title: Tetikleyici ve eylem iş akışı tanımlama dili - Azure Logic Apps türler için başvuru
description: Azure Logic Apps iş akışı tanımlama dili tetikleyicisi ve eylem türler için başvuru kılavuzu
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: reference
ms.date: 05/13/2019
ms.openlocfilehash: aa5d3a0555875571276fdf4046ad0e4dd1e69bbd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596953"
---
# <a name="reference-for-trigger-and-action-types-in-workflow-definition-language-for-azure-logic-apps"></a>Azure Logic Apps iş akışı tanımlama dili tetikleyicisi ve eylem türleri için başvuru

Bu başvuru tetikleyiciler ve Eylemler içinde açıklanan ve doğrulayan mantıksal uygulamanızın temel iş akışı tanımını tanımlamak için kullanılan genel türleri açıklayan [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md).
Özel bağlayıcı tetikleyiciler ve mantıksal uygulamalarınızda kullanabileceğiniz eylemler bulmak için listenin altında bkz [bağlayıcılara genel bakış](https://docs.microsoft.com/connectors/).

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Tetikleyiciler genel bakış

Her iş akışı örneği oluşturun ve iş akışının başlatılacağı çağrıları tanımlayan bir tetikleyici içerir. Genel tetikleyici kategorileri şunlardır:

* A *yoklama* bir hizmetin uç noktası düzenli aralıklarla denetleyen tetikleyicisi

* A *anında iletme* bir uç nokta için bir abonelik oluşturur ve sunan bir tetikleyici bir *geri çağırma URL'si* için uç nokta tetikleyici belirtilen olay gerçekleştiğinde veya veri kullanılabilir olduğunda bildirimde bulunabilir. Tetikleyici ardından Açmadığınızda önce uç noktanın yanıt bekler. 

Bazı isteğe bağlıdır, ancak bu üst düzey öğeleri Tetikleyiciler vardır:  
  
```json
"<trigger-name>": {
   "type": "<trigger-type>",
   "inputs": { "<trigger-inputs>" },
   "recurrence": { 
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "conditions": [ "<array-with-conditions>" ],
   "runtimeConfiguration": { "<runtime-config-options>" },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
},
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Tetikleyici adı*> | String | Tetikleyici adı | 
| <*Tetikleyici türü*> | String | Örneğin "Http" veya "ApiConnection" tetikleyici türü | 
| <*Tetikleyici giriş*> | JSON nesnesi | Tetikleyicinin davranışını tanımlayan girişleri | 
| <*zaman birimi*> | String | Tetikleyici ne sıklıkta açıklayan zaman birimi: "Saniye", "Minute", "Hour", "Day", "Week", "Month" | 
| <*sayı, zaman birimi*> | Integer | Tetikleyici ne sıklıkta belirten bir değeri sıklığı temel yeniden tetikleyici kadar beklenecek zaman birimlerinin sayısı tabanlı <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6 sıklığıdır "Month" ise, her 6 ayda bir yineleme olur. | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*koşullar ile dizi*> | Dizi | Bir veya daha fazla bilgi içeren bir dizi [koşullar](#trigger-conditions) iş akışı çalıştırılıp çalıştırılmayacağını belirleyen. Tetikleyiciler için kullanılabilir. | 
| <*çalışma zamanı yapılandırma seçenekleri*> | JSON nesnesi | Ayarlayarak tetikleyici çalışma zamanı davranışı değiştirebilirsiniz `runtimeConfiguration` özellikleri. Daha fazla bilgi için [çalışma zamanı yapılandırma ayarlarını](#runtime-config-options). | 
| <*splitOn-expression*> | String | Bir dizi döndürür tetikleyiciler, bir ifade belirtebilirsiniz, [ayırır veya *debatches* ](#split-on-debatch) işleme için birden çok iş akışı örneği içinde öğeleri dizisi. | 
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

## <a name="trigger-types-list"></a>Tetikleyici türleri listesi

Farklı bir arabirim ve tetikleyicinin davranışını belirleyen girişlerin her tetikleyici türü vardır. 

### <a name="built-in-triggers"></a>Yerleşik Tetikleyicileri

| Tetikleyici türü | Açıklama | 
|--------------|-------------| 
| [**HTTP**](#http-trigger) | Denetler veya *yoklamalar* herhangi bir uç nokta. Bu uç nokta "202" zaman uyumsuz desen kullanma ya da bir dizi dönerek belirli tetikleyici sözleşmeye uymalıdır. | 
| [**HTTPWebhook**](#http-webhook-trigger) | Mantıksal uygulamanız için çağrılabilen bir uç nokta oluşturur ancak kaydetmek veya kaydını silmek için belirtilen URL çağırır. |
| [**Yinelenme**](#recurrence-trigger) | Tanımlanan bir zamanlamaya göre ateşlenir. Gelecekteki bir tarih ve saat bu tetikleme adımını için ayarlayabilirsiniz. Sıklığı temel alarak, süreleri de belirtebilirsiniz ve iş akışınızı çalıştırmak için gün. | 
| [**İstek**](#request-trigger)  | Mantıksal uygulamanız için çağrılabilen bir uç noktası oluşturur ve "elle" tetikleyici olarak da bilinen olduğu. Örneğin, [çağrı, tetikleyici veya iç içe iş akışları HTTP uç noktaları ile](../logic-apps/logic-apps-http-endpoint.md). | 
||| 

### <a name="managed-api-triggers"></a>Yönetilen API Tetikleyicileri

| Tetikleyici türü | Açıklama | 
|--------------|-------------| 
| [**ApiConnection**](#apiconnection-trigger) | Denetler veya *yoklamalar* kullanarak bir uç nokta [Microsoft tarafından yönetilen API'leri](../connectors/apis-list.md). | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | Mantıksal uygulamanız için çağrılabilir bir uç noktasını çağırarak oluşturur [Microsoft tarafından yönetilen API'leri](../connectors/apis-list.md) abone olma ve aboneliği için. | 
||| 

## <a name="triggers---detailed-reference"></a>Tetikleyiciler - ayrıntılı başvuru

<a name="apiconnection-trigger"></a>

### <a name="apiconnection-trigger"></a>APIConnection tetikleyicisi  

Bu tetikleyiciyi denetler veya *yoklamalar* kullanarak bir uç nokta [Microsoft tarafından yönetilen API'leri](../connectors/apis-list.md) bu tetikleyiciyi değişebilir için parametreleri uç noktasına göre şekilde. Bu tetikleyici tanımında birçok olarak bölümlerde isteğe bağlıdır. Tetikleyicinin davranış olup olmadığını bölümler dahil olduğunuza bağlıdır.

```json
"<APIConnection_trigger_name>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },
      "method": "<method-type>",
      "path": "/<api-operation>",
      "retryPolicy": { "<retry-behavior>" },
      "queries": { "<query-parameters>" }
   },
   "recurrence": { 
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*APIConnection_trigger_name*> | String | Tetikleyici adı | 
| <*Bağlantı adı*> | String | İş akışı kullanan yönetilen API bağlantısı adı | 
| <*yöntem türü*> | String | Yönetilen API ile iletişim kurmak için HTTP yöntemi: "GET", "PUT", "POST", "DÜZELTME EKİ", "SİL" | 
| <*API işlemi*> | String | API işlemi çağırmak için | 
| <*zaman birimi*> | String | Tetikleyici ne sıklıkta açıklayan zaman birimi: "Saniye", "Minute", "Hour", "Day", "Week", "Month" | 
| <*sayı, zaman birimi*> | Integer | Tetikleyici ne sıklıkta belirten bir değeri sıklığı temel yeniden tetikleyici kadar beklenecek zaman birimlerinin sayısı tabanlı <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6 sıklığıdır "Month" ise, her 6 ayda bir yineleme olur. | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*Sorgu parametreleri*> | JSON nesnesi | Tüm sorgu parametreleri API'si ile içerecek şekilde çağırın. Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` çağrı. | 
| <*max-runs*> | Integer | Varsayılan olarak, iş akışı örnekleri aynı anda veya paralel kadar çalıştırmak [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Integer | İş akışınızı en fazla örnek sayısını çalışırken, değiştirebileceğiniz dayalı `runtimeConfiguration.concurrency.runs` özelliği, tüm yeni çalıştırmaları isimlerine bu kuyruğa [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | 
| <*splitOn-expression*> | String | Dizi döndüren tetikleyicileri, bu ifade oluşturmak ve her dizi öğesi için bir iş akışı örneği çalıştırmak yerine, bir "for each" döngüsü kullanın kullanılması için bir dizi başvuruyor. <p>Örneğin, bu ifade, tetikleyici gövde içeriği içinde döndürülen dizideki bir öğeyi temsil eder: `@triggerbody()?['value']` |
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). |
||||

*Çıkışlar*
 
| Öğe | Tür | Açıklama |
|---------|------|-------------|
| Üst bilgileri | JSON nesnesi | Yanıt üst bilgiler |
| Gövde | JSON nesnesi | Yanıt gövdesinden |
| Durum kodu | Integer | Yanıt durum kodu |
|||| 

*Örnek*

Bu tetikleyici tanımı içinde gelen her gün için bir Office 365 Outlook hesabı e-posta için denetler: 

```json
"When_a_new_email_arrives": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "get",
      "path": "/Mail/OnNewEmail",
      "queries": {
          "fetchOnlyWithAttachment": false,
          "folderPath": "Inbox",
          "importance": "Any",
          "includeAttachments": false
      }
   },
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

<a name="apiconnectionwebhook-trigger"></a>

### <a name="apiconnectionwebhook-trigger"></a>ApiConnectionWebhook tetikleyicisi

Bu tetikleyiciyi kullanarak bir abonelik isteği bir uç noktaya gönderen bir [Microsoft tarafından yönetilen API](../connectors/apis-list.md), sağlar bir *geri çağırma URL'si* için uç nokta bir yanıtın ve yanıt vermek uç nokta bekler yere gönderebilirsiniz. Daha fazla bilgi için [uç nokta abonelikleri](#subscribe-unsubscribe).

```json
"<ApiConnectionWebhook_trigger_name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "body": {
          "NotificationUrl": "@{listCallbackUrl()}"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },
      "retryPolicy": { "<retry-behavior>" },
      "queries": "<query-parameters>"
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-run-queue>
      }
   },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Bağlantı adı*> | String | İş akışı kullanan yönetilen API bağlantısı adı | 
| <*Gövde içeriği*> | JSON nesnesi | Yönetilen API için yükü olarak göndermek için herhangi bir ileti içeriği | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*Sorgu parametreleri*> | JSON nesnesi | API çağrısı ile içerecek şekilde tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` çağrı. | 
| <*max-runs*> | Integer | Varsayılan olarak, iş akışı örnekleri aynı anda veya paralel kadar çalıştırmak [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Integer | İş akışınızı en fazla örnek sayısını çalışırken, değiştirebileceğiniz dayalı `runtimeConfiguration.concurrency.runs` özelliği, tüm yeni çalıştırmaları isimlerine bu kuyruğa [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | 
| <*splitOn-expression*> | String | Dizi döndüren tetikleyicileri, bu ifade oluşturmak ve her dizi öğesi için bir iş akışı örneği çalıştırmak yerine, bir "for each" döngüsü kullanın kullanılması için bir dizi başvuruyor. <p>Örneğin, bu ifade, tetikleyici gövde içeriği içinde döndürülen dizideki bir öğeyi temsil eder: `@triggerbody()?['value']` |
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

*Örnek*

Bu tetikleyici tanımında, Office 365 Outlook API'ye abone, API uç noktası için bir geri çağırma URL'si sağlar ve yeni bir e-posta geldiğinde yanıtlamak uç nokta için bekler.

```json
"When_a_new_email_arrives_(webhook)": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "body": {
         "NotificationUrl": "@{listCallbackUrl()}" 
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "path": "/MailSubscription/$subscriptions",
      "queries": {
          "folderPath": "Inbox",
          "hasAttachment": "Any",
          "importance": "Any"
      }
   },
   "splitOn": "@triggerBody()?['value']"
}
```

<a name="http-trigger"></a>

### <a name="http-trigger"></a>HTTP tetikleyicisi

Bu tetikleyiciyi veya belirtilen yinelenme zamanlamasına göre belirtilen uç noktasını yoklayan bakar. Uç noktanın yanıt iş akışının çalıştığı olup olmadığını belirler.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<endpoint-URL>",
      "headers": { "<header-content>" },
      "body": "<body-content>",
      "authentication": { "<authentication-method>" },
      "retryPolicy": { "<retry-behavior>" },
      "queries": "<query-parameters>"
   },
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem türü*> | String | Belirtilen uç noktası'ı yoklamak için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "DÜZELTME EKİ", "SİL" | 
| <*uç nokta URL'si*> | String | HTTP veya HTTPS uç noktası URL'sini yoklamak için <p>Maksimum dize boyutu: 2 KB | 
| <*zaman birimi*> | String | Tetikleyici ne sıklıkta açıklayan zaman birimi: "Saniye", "Minute", "Hour", "Day", "Week", "Month" | 
| <*sayı, zaman birimi*> | Integer | Tetikleyici ne sıklıkta belirten bir değeri sıklığı temel yeniden tetikleyici kadar beklenecek zaman birimlerinin sayısı tabanlı <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6 sıklığıdır "Month" ise, her 6 ayda bir yineleme olur. | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*header-content*> | JSON nesnesi | Bir istekle göndermesini üstbilgileri <p>Örneğin, dil ve bir istek türünü ayarlamak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*Gövde içeriği*> | String | İstek yükü olarak gönderilecek ileti içeriği | 
| <*kimlik doğrulama yöntemi*> | JSON nesnesi | İstek yöntemi, kimlik doğrulaması için kullanır. Daha fazla bilgi için [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). Zamanlayıcı, ötesinde `authority` özelliği desteklenir. Belirtilmediğinde varsayılan değer: `https://login.windows.net`, ancak farklı bir değer gibi kullanabileceğiniz`https://login.windows\-ppe.net`. |
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). |  
 <*Sorgu parametreleri*> | JSON nesnesi | İstekle birlikte içerecek şekilde tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` isteği. | 
| <*max-runs*> | Integer | Varsayılan olarak, iş akışı örnekleri aynı anda veya paralel kadar çalıştırmak [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Integer | İş akışınızı en fazla örnek sayısını çalışırken, değiştirebileceğiniz dayalı `runtimeConfiguration.concurrency.runs` özelliği, tüm yeni çalıştırmaları isimlerine bu kuyruğa [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | 
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

*Çıkışlar*

| Öğe | Tür | Açıklama |
|---------|------|-------------| 
| Üst bilgileri | JSON nesnesi | Yanıt üst bilgiler | 
| Gövde | JSON nesnesi | Yanıt gövdesinden | 
| Durum kodu | Integer | Yanıt durum kodu | 
|||| 

*Gelen istekler için gereksinimler*

Uç nokta da mantıksal uygulamanız ile çalışmak için belirli bir tetikleyici düzeni ya da sözleşme uygun ve bu özellikleri tanıması gerekir:  
  
| Yanıt | Gerekli | Açıklama | 
|----------|----------|-------------| 
| Durum kodu | Evet | "200 Tamam" durum kodu çalıştırma başlatır. Diğer bir durum kodu çalıştırma başlamaz. | 
| Retry-after üst bilgisi | Hayır | Mantıksal uygulamanızı yeniden uç noktasını yoklayan kadar saniye sayısı | 
| Konum üst bilgisi | Hayır | Sonraki yoklama zaman aralığını aramak için URL. Belirtilmezse, özgün URL'yi kullanılır. | 
|||| 

*Farklı istekler için örnek davranışları*

| Durum kodu | Süre sonunda yeniden dene | Davranış | 
|-------------|-------------|----------|
| 200 | {none} | İş akışını çalıştırmak ve daha fazla veri için tanımlanan yineleme sonra tekrar kontrol edin. | 
| 200 | 10 saniye | İş akışını çalıştırmak ve daha fazla veri için 10 saniye sonra yeniden kontrol edin. |  
| 202 | 60 saniye | İş akışı tetikleyicisi yok. Sonraki girişimi tanımlanan yineleme tabi bir dakika içinde gerçekleşir. Bir dakikadan az tanımlanan yineleme ise retry-after üst bilgisi önceliklidir. Aksi takdirde, tanımlanan yineleme kullanılır. | 
| 400 | {none} | Hatalı istek, iş akışı çalıştırma. Hayır ise `retryPolicy` tanımlanır, varsayılan İlkesi kullanılır. Yeniden deneme sayısı limite ulaşıldıktan sonra tetikleyici yeniden tanımlanan yineleme sonra verilerini denetler. | 
| 500 | {none}| Sunucu hatası, iş akışı çalıştırma. Hayır ise `retryPolicy` tanımlanır, varsayılan İlkesi kullanılır. Yeniden deneme sayısı limite ulaşıldıktan sonra tetikleyici yeniden tanımlanan yineleme sonra verilerini denetler. | 
|||| 

<a name="http-webhook-trigger"></a>

### <a name="httpwebhook-trigger"></a>HTTPWebhook tetikleyicisi  

Bu tetikleyici, bir abonelik belirtilen uç nokta URL'sini çağırarak kaydedebilirsiniz bir uç noktası oluşturarak mantıksal uygulamanızı çağrılabilir yapar. Bu tetikleyici, iş akışınızı oluşturduğunuzda, bir giden talep aboneliği kaydetmek için çağrı yapar. Bu şekilde, tetikleyici olaylarını dinleme başlayabilirsiniz. Bu tetikleyiciyi bir işlemi geçersiz kılar, bir giden istek otomatik olarak aboneliğini iptal etmek için arama yapar. Daha fazla bilgi için [uç nokta abonelikleri](#subscribe-unsubscribe).

Ayrıca belirtebileceğiniz [zaman uyumsuz sınırları](#asynchronous-limits) üzerinde bir **HTTPWebhook** tetikleyici.
Tetikleyicinin davranışı kullanın ya da atlamak bölümleri bağlıdır. 

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<endpoint-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "retryPolicy": { "<retry-behavior>" }
         },
      },
      "unsubscribe": {
         "method": "<method-type>",
         "url": "<endpoint-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" }
      }
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

Gibi bazı değerler <*yöntem türü*>, her ikisi için de kullanılabilir `"subscribe"` ve `"unsubscribe"` nesneleri.

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem türü*> | String | Abonelik isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "Düzeltme Eki" veya "Sil" | 
| <*uç nokta abone URL'si*> | String | Uç nokta URL'si abonelik isteğinin gönderileceği adresi | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem türü*> | String | İptal isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "Düzeltme Eki" veya "Sil" | 
| <*endpoint-unsubscribe-URL*> | String | Uç nokta URL'si nerede iptal isteği gönder | 
| <*Gövde içeriği*> | String | Abonelik veya iptal isteğinde gönderilecek içerik herhangi bir ileti | 
| <*kimlik doğrulama yöntemi*> | JSON nesnesi | İstek yöntemi, kimlik doğrulaması için kullanır. Daha fazla bilgi için [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*max-runs*> | Integer | Varsayılan olarak, tüm iş akışı örnekleri aynı anda veya paralel kadar çalıştırmak [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Integer | İş akışınızı en fazla örnek sayısını çalışırken, değiştirebileceğiniz dayalı `runtimeConfiguration.concurrency.runs` özelliği, tüm yeni çalıştırmaları isimlerine bu kuyruğa [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | 
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

*Çıkışlar* 

| Öğe | Tür | Açıklama |
|---------|------|-------------| 
| Üst bilgileri | JSON nesnesi | Yanıt üst bilgiler | 
| Gövde | JSON nesnesi | Yanıt gövdesinden | 
| Durum kodu | Integer | Yanıt durum kodu | 
|||| 

*Örnek*

Bu tetikleyiciyi belirtilen uç noktası için bir abonelik oluşturur, benzersiz bir geri çağırma URL'si sağlar ve yeni yayımlanan teknoloji makaleler için bekler.

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "POST",
         "uri": "https://pubsubhubbub.appspot.com/subscribe",
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
         }
      }
   }
}
```

<a name="recurrence-trigger"></a>

### <a name="recurrence-trigger"></a>Yineleme tetikleyicisi  

Bu tetikleyiciyi belirtilen yinelenme zamanlamaya göre çalışan ve düzenli aralıklarla çalışan bir iş akışı oluşturmak için kolay bir yol sağlar. 

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
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
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*zaman birimi*> | String | Tetikleyici ne sıklıkta açıklayan zaman birimi: "Saniye", "Minute", "Hour", "Day", "Week", "Month" | 
| <*sayı, zaman birimi*> | Integer | Tetikleyici ne sıklıkta belirten bir değeri sıklığı temel yeniden tetikleyici kadar beklenecek zaman birimlerinin sayısı tabanlı <p>Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6 sıklığıdır "Month" ise, her 6 ayda bir yineleme olur. | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*start-date-time-with-format-YYYY-MM-DDThh:mm:ss*> | String | Başlangıç tarih ve saat şu biçimde: <p>YYYY-MM-ddTHH bir saat dilimi belirtirseniz <p>veya <p>YYYY-AA-saat dilimi belirtmezseniz ssZ <p>Örneğin, 18 Eylül 2017 2: 00'da isterseniz, ardından belirtin "2017-09-18T14:00:00" ve "Pasifik Standart Saati" gibi bir saat dilimi belirtin veya belirtin "2017-09-18T14:00:00Z" olmadan bir saat dilimi. <p>**Not:** Bu başlangıç zamanı izlemelidir [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi belirtmezseniz, sonunda boşluk olmadan "Z" harfi eklemeniz gerekir. Bu "Z" eş değeri başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlamalar için ilk yinelenme, başlangıç zamanıdır sırada karmaşık zamanlamalar için tetikleyici başlangıç saatinden herhangi bir erken etkinleşmez. Başlangıç tarihler ve saatler hakkında daha fazla bilgi için bkz: [oluşturma ve zamanlama düzenli olarak çalışan görevlerin](../connectors/connectors-native-recurrence.md). | 
| <*saat dilimi*> | String | Bu tetikleyiciyi kabul etmez çünkü yalnızca bir başlangıç zamanı belirttiğinizde geçerlidir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini belirtin. | 
| <*bir-veya-daha fazla bilgi-saat-işaretleri*> | Tamsayı veya tamsayı dizisi | İçin "Day" veya "Week" belirtirseniz `frequency`, bir veya daha fazla tam sayılar 0'dan 23, iş akışını çalıştırmak istediğinizde günün saat virgülle ayırarak belirtebilirsiniz. <p>Örneğin, "10", "12" ve "14" belirtin, 10 AM, PM 12 ve 2 Pasifik saat işaretlerinde olarak alırsınız. | 
| <*bir-veya-daha fazla bilgi-dakika-işaretleri*> | Tamsayı veya tamsayı dizisi | İçin "Day" veya "Week" belirtirseniz `frequency`, bir veya daha fazla tam sayılar 0'dan 59, iş akışını çalıştırmak istediğinizde saat, dakika, virgülle ayırarak belirtebilirsiniz. <p>Örneğin, "30" dakika işareti belirtebilirsiniz ve önceki örnekte için günün saatlerini kullanarak 10:30 AM, alın 12:30 PM ve 2:30 PM. | 
| weekDays | Dize veya dize dizisi | İçin "Week" belirtirseniz `frequency`, iş akışını çalıştırmak istediğinizde, virgülle ayırarak bir veya daha fazla gün belirtebilirsiniz: "Pazartesi", "Salı", "Çarşamba", "Thursday", "Friday", "Cumartesi" ve "Sunday" | 
| <*max-runs*> | Integer | Varsayılan olarak, tüm iş akışı örnekleri aynı anda veya paralel kadar çalıştırmak [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Integer | İş akışınızı en fazla örnek sayısını çalışırken, değiştirebileceğiniz dayalı `runtimeConfiguration.concurrency.runs` özelliği, tüm yeni çalıştırmaları isimlerine bu kuyruğa [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | 
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

*Örnek 1*

Bu temel yinelenme tetikleyicisini her gün çalışır:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

*Örnek 2*

Başlangıç tarihi ve saati tetikleme adımını belirtebilirsiniz. Bu yineleme tetikleyicisi, belirtilen tarihte başlar ve günlük başlatılır:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1,
      "startTime": "2017-09-18T00:00:00Z"
   }
}
```

*Örnek 3*

Bu yineleme tetikleyicisi 9 Eylül 2017'de 2: 00'da başlar ve her Pazartesi saat 10: 30'da, haftalık harekete 12:30 PM ve 2:30 PM Pasifik Standart Saati:

``` json
"Recurrence": {
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

Daha fazla bilgi ve bu tetikleyicinin örnekler için bkz. [oluşturma ve zamanlama düzenli olarak çalışan görevlerin](../connectors/connectors-native-recurrence.md).

<a name="request-trigger"></a>

### <a name="request-trigger"></a>İstek tetikleyicisi

Bu tetikleyici, gelen istekleri kabul edebilecek bir uç noktası oluşturarak mantıksal uygulamanızı çağrılabilir yapar. Bu tetikleyici için bir JSON şeması açıklayan ve yükü veya tetikleyici gelen istekte alan girişleri doğrulama sağlar. Şema ayrıca tetikleyici özellikleri başvuru iş akışındaki sonraki eylemlerine kolaylaştırır. 

Bu tetikleyiciyi çağırmak için kullanmalısınız `listCallbackUrl` açıklanan API [iş akışı hizmeti REST API'si](https://docs.microsoft.com/rest/api/logic/workflows). Bu tetikleyiciyi bir HTTP uç noktası olarak kullanmayı öğrenmek için bkz. [çağrı, tetikleyici veya iç içe iş akışları HTTP uç noktaları ile](../logic-apps/logic-apps-http-endpoint.md).

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "<method-type>",
      "relativePath": "<relative-path-for-accepted-parameter>",
      "schema": {
         "type": "object",
         "properties": { 
            "<property-name>": {
               "type": "<property-type>"
            }
         },
         "required": [ "<required-properties>" ]
      }
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-run-queue>
      },
   },
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*özellik adı*> | String | Bir özelliğin yükü tanımlayan JSON şema adı | 
| <*özellik türü*> | String | Özelliğin türü | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem türü*> | String | Mantıksal uygulamanızı çağırmak için gelen istekleri kullanmalıdır yöntemi: "GET", "PUT", "POST", "DÜZELTME EKİ", "SİL" |
| <*göreli yol-için-kabul edildi-parametresi*> | String | Uç noktasının URL'sini kabul edebilen parametresi için göreli yolu | 
| <*gerekli özellikleri*> | Dizi | Değer gerektiren bir veya daha fazla özellikleri | 
| <*max-runs*> | Integer | Varsayılan olarak, tüm iş akışı örnekleri aynı anda veya paralel kadar çalıştırmak [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Integer | İş akışınızı en fazla örnek sayısını çalışırken, değiştirebileceğiniz dayalı `runtimeConfiguration.concurrency.runs` özelliği, tüm yeni çalıştırmaları isimlerine bu kuyruğa [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | 
| <*işlem seçeneği*> | String | Ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

*Örnek*

Bu tetikleyiciyi belirtir: gelen bir istek tetikleyicisi çağırmak için HTTP POST yöntemini kullanmanız gerekir ve gelen istek girişini doğrulayan bir şema içerir: 

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "POST",
      "schema": {
         "type": "object",
         "properties": {
            "customerName": {
               "type": "String"
            },
            "customerAddress": { 
               "type": "Object",
               "properties": {
                  "streetAddress": {
                     "type": "string"
                  },
                  "city": {
                     "type": "string"
                  }
               }
            }
         }
      }
   }
}
```

<a name="trigger-conditions"></a>

## <a name="trigger-conditions"></a>Tetikleyici koşulları

Bir tetikleyici ve yalnızca tetikleyici, iş akışının çalıştırılması gerekip gerekmediğini belirleyen koşulları için bir veya daha fazla ifadeler içeren bir dizi içerebilir. Eklenecek `conditions` , iş akışı tetikleyici özelliğini mantıksal uygulamanızı kod görünümü Düzenleyicisi'nde açın.

Örneğin, bir tetikleyici yalnızca bir Web sitesi bir iç sunucu hatası durum kodunu tetikleyicinin başvurarak döndürdüğünde harekete belirtebilirsiniz `conditions` özelliği:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Hour",
      "interval": 1
   },
   "conditions": [ {
      "expression": "@equals(triggers().code, 'InternalServerError')"
   } ]
}
```

Varsayılan olarak, bir tetikleyici yalnızca aldıktan sonra bir "200 Tamam" yanıt. Bir ifade bir tetikleyicinin durum kodu başvuruda bulunduğunda, tetikleyicinin varsayılan davranışı değiştirilir. Bu nedenle, "200" gibi birden fazla durum kodu ve "201" durum kodu ateşlenmesine tetikleyici istiyorsanız, bu ifade, koşul olarak eklemeniz gerekir: 

`@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="trigger-multiple-runs"></a>Birden çok çalıştırma tetikleyin

Bazen tetikleyicinize işlemek mantıksal uygulamanız için bir dizi döndürürse, bir "for each" döngüsü dizideki tüm öğeler işlemek için çok uzun sürebilir. Bunun yerine kullanabileceğiniz **SplitOn** tetikleyicinize özelliğinde *debatch* dizi. Ayırma, dizi öğeleri böler ve her dizi öğesi için'ı çalıştıran yeni bir iş akışı örneğini başlatır. Bu yaklaşım, örneğin, yoklama aralıklarında birden çok yeni öğe döndürebilir bir uç noktaya yoklama istediğinizde yararlıdır.
Dizi sayısı bu öğeler için **SplitOn** işlem tek bir mantıksal uygulama çalıştırmasında, bkz: [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). 

> [!NOTE]
> Kullanamazsınız **SplitOn** düzendeki zaman uyumlu yanıt. Kullanan herhangi bir iş akışı **SplitOn** ve yanıt içeren eylemin zaman uyumsuz olarak çalışır ve hemen gönderir bir `202 ACCEPTED` yanıt.

Bir dizi olan bir yük, tetikleyicinin Swagger dosyası tanımlıyorsa **SplitOn** özelliği Tetikleyiciniz için otomatik olarak eklenir. Aksi takdirde, bu özellik debatch istediğiniz dizinin sahip yanıt yükünde içine ekleyin. 

*Örnek*

Bu yanıt veren bir API olduğunu varsayalım: 
  
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
 
Mantıksal uygulamanız dizideki içerikten yeterlidir `Rows`, bu nedenle şu örnekteki gibi bir tetikleyici oluşturabilirsiniz:

``` json
"HTTP_Debatch": {
   "type": "Http",
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
   "recurrence": {
      "frequency": "Second",
      "interval": 1
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> Kullanırsanız `SplitOn` komut dizisi dışında özellikleri alınamıyor. Bu örnekte, alınamıyor. Bu nedenle `status` yanıttaki özellik API'den döndürülen.
> 
> Bir hatadan kaçınmak için `Rows` özelliği yoksa, bu örnekte `?` işleci.

İş akışı tanımınızı artık kullanabilirsiniz `@triggerBody().name` almak için `name` olan değerlerini `"customer-name-one"` ilk çalışma ve `"customer-name-two"` ikinci Çalıştır. Bu nedenle, görünüm tetikleyicinize çıkarır aşağıdaki örneklerde ister:

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

<a name="actions-overview"></a>

## <a name="actions-overview"></a>Eylemlere genel bakış

Azure Logic Apps, çeşitli işlem türleri - her bir eylemin benzersiz davranışını tanımlayan farklı giriş sağlar. 

Bazı isteğe bağlı olsa bu üst düzey öğe eylemleri vardır:

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { 
      "<input-name>": { "<input-value>" },
      "retryPolicy": "<retry-behavior>" 
   },
   "runAfter": { "<previous-trigger-or-action-status>" },
   "runtimeConfiguration": { "<runtime-config-options>" },
   "operationOptions": "<operation-option>"
},
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------|
| <*Eylem adı*> | String | Eylem adı | 
| <*Eylem türü*> | String | Örneğin, "Http" veya "ApiConnection", eylem türü| 
| <*Giriş adı*> | String | Eylemin davranışını tanımlayan bir giriş adı | 
| <*giriş değeri*> | Çeşitli | Dize, tamsayı, JSON nesnesi ve benzeri giriş değeri | 
| <*Önceki-tetikleyici-veya-işlem-status*> | JSON nesnesi | Adı ve sonuçta elde edilen durum tetikleyici veya eylemi, bu geçerli bir eylem hemen çalıştırmadan önce çalıştırmanız gerekir | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------|
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Yeniden deneme ilkeleri daha fazla bilgi için bkz. | 
| <*çalışma zamanı yapılandırma seçenekleri*> | JSON nesnesi | Bazı eylemler için eylem davranışı çalışma zamanında ayarlayarak değiştirebileceğiniz `runtimeConfiguration` özellikleri. Daha fazla bilgi için [çalışma zamanı yapılandırma ayarlarını](#runtime-config-options). | 
| <*işlem seçeneği*> | String | Bazı eylemler için ayarlayarak varsayılan davranışı değiştirebilirsiniz `operationOptions` özelliği. Daha fazla bilgi için [işlem seçenekleri](#operation-options). | 
|||| 

## <a name="action-types-list"></a>Eylem türleri listesi

Bazı yaygın olarak kullanılan eylem türleri şunlardır: 

* [Yerleşik eylem türleri](#built-in-actions) bu örnekleri ve daha fazlası gibi: 

  * [**HTTP** ](#http-action) için HTTP veya HTTPS üzerinden uç noktalarına çağrı yapma

  * [**Yanıt** ](#response-action) isteklerini yanıtlama

  * [**JavaScript kod yürütmesine** ](#run-javascript-code) JavaScript çalıştırma için kod parçacıkları

  * [**İşlev** ](#function-action) Azure işlevleri çağırma

  * Veri işlem eylemleri gibi [ **katılın**](#join-action), [ **Compose**](#compose-action), [ **tablo** ](#table-action), [ **Seçin**](#select-action)ve diğerleri oluşturan veya çeşitli giriş verileri dönüştürme

  * [**İş akışı** ](#workflow-action) başka bir mantıksal uygulama iş akışı çağırma

* [Yönetilen API eylem türleri](#managed-api-actions) gibi [ **ApiConnection** ](#apiconnection-action) ve [ **ApiConnectionWebHook** ](#apiconnectionwebhook-action) çeşitli arayın bağlayıcılar ve örneğin, Microsoft tarafından yönetilen API'leri, Azure Service Bus, Office 365 Outlook, Power BI, Azure Blob Depolama, OneDrive, GitHub ve daha fazla bilgi

* [İş akışı eylemi türleri kontrol](#control-workflow-actions) gibi [ **varsa**](#if-action), [ **Foreach**](#foreach-action), [ **anahtarı**  ](#switch-action), [ **Kapsam**](#scope-action), ve [ **kadar**](#until-action), diğer eylemleri içerir ve Yardım İş akışı yürütme düzenleme

<a name="built-in-actions"></a>

### <a name="built-in-actions"></a>Yerleşik eylemler

| Eylem türü | Açıklama | 
|-------------|-------------| 
| [**Oluşturan**](#compose-action) | Çeşitli türlerde olabilen girişler, tek bir çıktı oluşturur. | 
| [**JavaScript kodu yürütme**](#run-javascript-code) | İçinde belirli bir ölçüte uyan JavaScript kod parçacıkları çalıştırın. Kod gereksinimler ve daha fazla bilgi için bkz. [ekleme ve satır içi kod ile çalışma kod parçacıkları](../logic-apps/logic-apps-add-run-inline-code.md). |
| [**İşlevi**](#function-action) | Bir Azure işlevi çağırır. | 
| [**HTTP**](#http-action) | Bir HTTP uç noktası çağırır. | 
| [**Katılın**](#join-action) | Bir dizideki tüm öğeler bir dize oluşturur ve bu öğeleri ile belirtilen bir sınırlayıcı karakter ayırır. | 
| [**JSON Ayrıştır**](#parse-json-action) | Kullanıcı dostu belirteçleri JSON özelliklerinde içerik oluşturur. Sonra mantıksal uygulamanızın belirteçleri dahil olmak üzere bu özelliklere başvuruda bulunabilir. | 
| [**Sorgu**](#query-action) | Bir koşul veya filtre temel başka bir dizideki öğelerden bir dizi oluşturur. | 
| [**Yanıt**](#response-action) | Gelen çağrıyı veya isteği bir yanıt oluşturur. | 
| [**Seçin**](#select-action) | Bir dizi JSON nesnesi ile belirtilen haritasına dayalı olarak başka bir diziden öğeleri dönüştürerek oluşturur. | 
| [**Tablo**](#table-action) | Bir diziyi bir CSV veya HTML tablosu oluşturur. | 
| [**sonlandırma**](#terminate-action) | Etkin olarak çalışan bir iş akışı durdurur. | 
| [**bekleme**](#wait-action) | İş akışınızı, belirtilen bir süre boyunca veya belirli bir tarih ve saate kadar duraklatılır. | 
| [**İş akışı**](#workflow-action) | Başka bir iş akışı içinde bir iş akışı gömer. | 
||| 

<a name="managed-api-actions"></a>

### <a name="managed-api-actions"></a>Yönetilen API eylemleri

| Eylem türü | Açıklama | 
|-------------|-------------|  
| [**ApiConnection**](#apiconnection-action) | Bir HTTP uç noktasını kullanarak çağıran bir [Microsoft tarafından yönetilen API](../connectors/apis-list.md). | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-action) | HTTP Web kancası gibi çalışır, ancak kullanan bir [Microsoft tarafından yönetilen API](../connectors/apis-list.md). | 
||| 

<a name="control-workflow-actions"></a>

### <a name="control-workflow-actions"></a>Denetim iş akışı eylemleri

Bu Eylemler, iş akışının yürütülmesini denetlemenize ve diğer Eylemler dahil yardımcı olur. Bir denetimi iş akışı eylemi dışında doğrudan Eylemler, Denetim iş akışı eylemi içinde başvurabilirsiniz. Örneğin, bir `Http` eylemi bir kapsam içinde başvurabilir `@body('Http')` ifade yerden iş akışı. Ancak, bir denetim iş akışı eylemi içinde mevcut eylem yalnızca "aynı denetimi iş akışı yapı diğer eylemler çalıştırabilir".

| Eylem türü | Açıklama | 
|-------------|-------------| 
| [**ForEach**](#foreach-action) | Bir dizideki her öğe için bir döngü içindeki aynı eylemleri çalıştırın. | 
| [**Eğer**](#if-action) | Bağlı çalışma Eylemler belirtilen koşulun true veya false. | 
| [**Kapsam**](#scope-action) | Bir dizi eylemi Grup durumu göre eylemleri çalıştırın. | 
| [**Anahtar**](#switch-action) | İfadeler, nesneler veya belirteçleri değerlerinden her örneği tarafından belirtilen değerleri eşleştiğinde durumlarına düzenlenmiş eylemleri çalıştırın. | 
| [**Kadar**](#until-action) | Belirtilen koşul true olana kadar Eylemler bir döngüde çalışır. | 
|||  

## <a name="actions---detailed-reference"></a>Eylemler - ayrıntılı başvuru

<a name="apiconnection-action"></a>

### <a name="apiconnection-action"></a>APIConnection eylemi

Bu eylem bir HTTP isteği gönderir. bir [Microsoft tarafından yönetilen API](../connectors/apis-list.md) ve API'nin parametreleri ve yanı sıra geçerli bir bağlantı başvurusu hakkında bilgi gerektirir. 

``` json
"<action-name>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<api-name>']['connectionId']"
         },
         "<other-action-specific-input-properties>"        
      },
      "method": "<method-type>",
      "path": "/<api-operation>",
      "retryPolicy": "<retry-behavior>",
      "queries": { "<query-parameters>" },
      "<other-action-specific-properties>"
    },
    "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Eylem adı*> | String | Bağlayıcı tarafından sağlanan eylemin adı | 
| <*API adı*> | String | Bağlantı için kullanılan Microsoft tarafından yönetilen API adı | 
| <*yöntem türü*> | String | API'yi çağırmak için HTTP yöntemi: "GET", "PUT", "POST", "Düzeltme Eki" veya "Sil" | 
| <*API işlemi*> | String | API işlemi çağırmak için | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*diğer-işlem-özel-giriş-properties*> | JSON nesnesi | Bu özel eylem için geçerli herhangi bir giriş özellikleri | 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*Sorgu parametreleri*> | JSON nesnesi | Tüm sorgu parametreleri API'si ile içerecek şekilde çağırın. <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` çağrı. | 
| <*diğer-işlem-özel-properties*> | JSON nesnesi | Bu özel eylem için geçerli diğer tüm özellikler | 
|||| 

*Örnek*

Bu tanımı açıklar **bir e-posta** Office 365 Outlook Bağlayıcısı, Microsoft tarafından yönetilen bir API için eylem: 

```json
"Send_an_email": {
   "type": "ApiConnection",
   "inputs": {
      "body": {
         "Body": "Thank you for your membership!",
         "Subject": "Hello and welcome!",
         "To": "Sophie.Owen@contoso.com"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "POST",
      "path": "/Mail"
    },
    "runAfter": {}
}
```

<a name="apiconnection-webhook-action"></a>

### <a name="apiconnectionwebhook-action"></a>APIConnectionWebhook eylemi

Bu eylem bir abonelik isteği HTTP üzerinden bir uç noktaya kullanarak gönderen bir [Microsoft tarafından yönetilen API](../connectors/apis-list.md), sağlar bir *geri çağırma URL'si* için uç nokta bir yanıt ve uç noktaya bekler burada gönderebilir yanıt. Daha fazla bilgi için [uç nokta abonelikleri](#subscribe-unsubscribe).

```json
"<action-name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<api-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "retryPolicy": "<retry-behavior>",
         "queries": { "<query-parameters>" },
         "<other-action-specific-input-properties>"
      },
      "unsubscribe": {
         "method": "<method-type>",
         "uri": "<api-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "<other-action-specific-properties>"
      },
   },
   "runAfter": {}
}
```

Gibi bazı değerler <*yöntem türü*>, her ikisi için de kullanılabilir `"subscribe"` ve `"unsubscribe"` nesneleri.

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Eylem adı*> | String | Bağlayıcı tarafından sağlanan eylemin adı | 
| <*yöntem türü*> | String | Abone veya bir uç noktasından aboneliği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "Düzeltme Eki" veya "Sil" | 
| <*API abone URL'si*> | String | API'ye abone için kullanılacak URI | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*API aboneliği URL'si*> | String | API'den aboneliği için kullanılacak URI | 
| <*header-content*> | JSON nesnesi | Tüm üstbilgileri istek göndermek için <p>Örneğin, dilini ayarlamak ve üzerinde bir talep türü için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*Gövde içeriği*> | JSON nesnesi | İstekte göndermek için herhangi bir ileti içeriği | 
| <*kimlik doğrulama yöntemi*> | JSON nesnesi | İstek yöntemi, kimlik doğrulaması için kullanır. Daha fazla bilgi için [Scheduler giden bağlantı kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*Sorgu parametreleri*> | JSON nesnesi | API çağrısı ile içerecek şekilde tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` çağrı. | 
| <*diğer-işlem-özel-giriş-properties*> | JSON nesnesi | Bu özel eylem için geçerli herhangi bir giriş özellikleri | 
| <*diğer-işlem-özel-properties*> | JSON nesnesi | Bu özel eylem için geçerli diğer tüm özellikler | 
|||| 

Şirket sınırları belirtebilirsiniz bir **ApiConnectionWebhook** aynı şekilde eylem [zaman uyumsuz HTTP sınırları](#asynchronous-limits).

<a name="compose-action"></a>

### <a name="compose-action"></a>Oluştur eylemi

Bu eylem, birden çok girişler ifadeleri de dahil olmak üzere, tek bir çıktı oluşturur. Azure Logic Apps yerel olarak destekleyen, diziler, JSON nesneleri, XML ve ikili gibi herhangi bir türü çıkış ve giriş olabilir.
Eylemin çıkış diğer eylemleri daha sonra kullanabilirsiniz. 

```json
"Compose": {
   "type": "Compose",
   "inputs": "<inputs-to-compose>",
   "runAfter": {}
},
```

*Gerekli* 

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*compose girişleri*> | Tüm | Tek bir çıktı oluşturmak için girişleri | 
|||| 

*Örnek 1*

<!-- markdownlint-disable MD038 -->
Bu eylem tanımı birleştirir `abcdefg ` bir boşluk ve değerle `1234`:
<!-- markdownlint-enable MD038 -->

```json
"Compose": {
   "type": "Compose",
   "inputs": "abcdefg 1234",
   "runAfter": {}
},
```

Bu eylem oluşturan çıktısı aşağıdaki gibidir:

`abcdefg 1234`

*Örnek 2*

Bu eylem tanımını içeren bir dize değişkeniyle birleştirir `abcdefg` ve içeren bir tamsayı değişkeni `1234`:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@{variables('myString')}@{variables('myInteger')}",
   "runAfter": {}
},
```

Bu eylem oluşturan çıktısı aşağıdaki gibidir:

`"abcdefg1234"`

<a name="run-javascript-code"></a>

### <a name="execute-javascript-code-action"></a>JavaScript kodu eylemi Yürüt

Bu eylem bir JavaScript kod parçacığını çalıştırır ve sonuçları aracılığıyla döndürür bir `Result` sonraki eylemlerde başvurabilirsiniz belirteci.

```json
"Execute_JavaScript_Code": {
   "type": "JavaScriptCode",
   "inputs": {
      "code": "<JavaScript-code-snippet>",
      "explicitDependencies": {
         "actions": [ <previous-actions> ],
         "includeTrigger": true
      }
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*JavaScript kod parçacığı*> | Değişir | Çalıştırmak istediğiniz JavaScript kodu. Kod gereksinimler ve daha fazla bilgi için bkz. [ekleme ve satır içi kod ile çalışma kod parçacıkları](../logic-apps/logic-apps-add-run-inline-code.md). <p>İçinde `code` özniteliği, kod parçacığınızı salt okunur kullanabilirsiniz `workflowContext` giriş olarak nesnesi. Bu nesne, tetikleyici ve önceki eylemlerin iş sonuçları, kod erişim vermek alt özellikleri vardır. Hakkında daha fazla bilgi için `workflowContext` nesne, bkz: [başvuru tetikleyici ve eylem sonuçlarını, kodunuzda](../logic-apps/logic-apps-add-run-inline-code.md#workflowcontext). |
||||

*Bazı durumlarda gerekli*

`explicitDependencies` Özniteliği, açıkça tetikleyici, önceki eylemlerin ya da her ikisi de kod parçacığınız için bağımlılıklar olarak sonuçları dahil etmek istediğinizi belirtir. Bu bağımlılıklar ekleme hakkında daha fazla bilgi için bkz. [parametreleri için satır içi kod eklemek](../logic-apps/logic-apps-add-run-inline-code.md#add-parameters). 

İçin `includeTrigger` özniteliği belirtebilirsiniz `true` veya `false` değerleri.

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*Önceki eylemlerin*> | Dize dizisi | Belirtilen eylem adları olan bir dizi. Burada eylem adları alt çizgi (_), boşluk kullanın, iş akışı tanımı içinde görünen eylem adları kullanın (""). |
||||

*Örnek 1*

Bu eylem, mantıksal uygulamanızın adını alır ve metin "< mantıksal uygulama adı > Merhaba Dünya" sonuç olarak döndüren bir kod çalıştırır. Bu örnekte, erişerek iş akışının adı koda başvuran `workflowContext.workflow.name` özelliği salt okunur üzerinden `workflowContext` nesne. Kullanma hakkında daha fazla bilgi için `workflowContext` nesne, bkz: [başvuru tetikleyici ve eylem sonuçlarını, kodunuzda](../logic-apps/logic-apps-add-run-inline-code.md#workflowcontext).

```json
"Execute_JavaScript_Code": {
   "type": "JavaScriptCode",
   "inputs": {
      "code": "var text = \"Hello world from \" + workflowContext.workflow.name;\r\n\r\nreturn text;"
   },
   "runAfter": {}
}
```

*Örnek 2*

Bu eylem, bir Office 365 Outlook hesabı yeni bir e-posta tetiklenir ulaşan bir mantıksal uygulama içinde kod çalıştırır. Mantıksal uygulama onay isteği birlikte alınan e-postasındaki içeriği ileten bir gönderme onay e-posta eylemi de kullanır. 

Kod tetikleyicinin e-posta adreslerini ayıklar `Body` özelliği ile birlikte bu e-posta adreslerini döndürür `SelectedOption` onay eylemi özelliği değeri. Eylem açıkça bağımlılık olarak gönder onay e-posta eylemi içeren `explicitDependencies`  >  `actions` özniteliği.

```json
"Execute_JavaScript_Code": {
   "type": "JavaScriptCode",
   "inputs": {
      "code": "var re = /(([^<>()\\[\\]\\\\.,;:\\s@\"]+(\\.[^<>()\\[\\]\\\\.,;:\\s@\"]+)*)|(\".+\"))@((\\[[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}])|(([a-zA-Z\\-0-9]+\\.)+[a-zA-Z]{2,}))/g;\r\n\r\nvar email = workflowContext.trigger.outputs.body.Body;\r\n\r\nvar reply = workflowContext.actions.Send_approval_email_.outputs.body.SelectedOption;\r\n\r\nreturn email.match(re) + \" - \" + reply;\r\n;",
      "explicitDependencies": {
         "actions": [
            "Send_approval_email_"
         ]
      }
   },
   "runAfter": {}
}
```



<a name="function-action"></a>

### <a name="function-action"></a>Eylem işlevi

Bu eylemi daha önce oluşturulmuş bir çağıran [Azure işlevi](../azure-functions/functions-create-first-azure-function.md).

```json
"<Azure-function-name>": {
   "type": "Function",
   "inputs": {
     "function": {
        "id": "<Azure-function-ID>"
      },
      "method": "<method-type>",
      "headers": { "<header-content>" },
      "body": { "<body-content>" },
      "queries": { "<query-parameters>" } 
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*Azure işlev kimliği*> | String | Azure işlevini çağırmak istediğinizde kaynak kimliği. Bu değer için biçim şu şekildedir:<p>"/subscriptions/ <*azure abonelik kimliği*> /resourceGroups/ <*Azure kaynak grubu*> /providers/Microsoft.Web/sites/ <*işlevi Azure için uygulama adı*> /Functions/ <*azure işlev adı*> " | 
| <*yöntem türü*> | String | Bir işlevi çağırmak için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "Düzeltme Eki" veya "Sil" <p>Belirtilmezse, varsayılan değer "POST" yöntemidir. | 
||||

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*header-content*> | JSON nesnesi | Çağrı göndermek için üst bilgileri <p>Örneğin, dilini ayarlamak ve üzerinde bir talep türü için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*Gövde içeriği*> | JSON nesnesi | İstekte göndermek için herhangi bir ileti içeriği | 
| <*Sorgu parametreleri*> | JSON nesnesi | API çağrısı ile içerecek şekilde tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` çağrı. | 
| <*diğer-işlem-özel-giriş-properties*> | JSON nesnesi | Bu özel eylem için geçerli herhangi bir giriş özellikleri | 
| <*diğer-işlem-özel-properties*> | JSON nesnesi | Bu özel eylem için geçerli diğer tüm özellikler | 
||||

Logic Apps altyapısı, mantıksal uygulamanızı kaydettiğinizde, başvurulan işlev bu denetimleri gerçekleştirir:

* İş akışınızı, işlev erişiminiz olması gerekir.

* İş akışınızı, yalnızca standart HTTP tetikleyicisi veya genel JSON Web kancası tetikleyici kullanabilirsiniz. 

  Logic Apps altyapısı, alır ve çalışma zamanında kullanılan tetikleyicinin URL önbelleğe alır. Ancak, herhangi bir işlem önbelleğe alınan URL geçersiz kılar **işlevi** eylem çalışma zamanında başarısız olur. Bu sorunu gidermek için mantıksal uygulama alır ve tetikleme URL'si tekrar önbelleğe alır, mantıksal uygulama yeniden kaydedin.

* İşlevi, tanımlı bir yolun sahip olamaz.

* Yalnızca "işlev" ve "anonim" yetkilendirme düzeyi izin verilir. 

*Örnek*

Bu eylem tanımı, önceden oluşturulmuş "GetProductID" işlevini çağırır:

```json
"GetProductID": {
   "type": "Function",
   "inputs": {
     "function": {
        "id": "/subscriptions/<XXXXXXXXXXXXXXXXXXXX>/resourceGroups/myLogicAppResourceGroup/providers/Microsoft.Web/sites/InventoryChecker/functions/GetProductID"
      },
      "method": "POST",
      "headers": { 
          "x-ms-date": "@utcnow()"
       },
      "body": { 
          "Product_ID": "@variables('ProductID')"
      }
   },
   "runAfter": {}
}
```

<a name="http-action"></a>

### <a name="http-action"></a>HTTP eylemi

Bu eylem, belirtilen uç noktaya bir istek gönderir ve yanıtı iş akışının çalıştırılması gerekip gerekmediğini belirlemek için denetler. 

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<HTTP-or-HTTPS-endpoint-URL>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem türü*> | String | İstek göndermek için kullanılacak yöntemi: "GET", "PUT", "POST", "Düzeltme Eki" veya "Sil" | 
| <*HTTP-or-HTTPS-endpoint-URL*> | String | Çağırmak için HTTP veya HTTPS uç noktası. Maksimum dize boyutu: 2 KB | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*header-content*> | JSON nesnesi | Bir istekle göndermesini üst bilgileri <p>Örneğin, türü ve dili ayarlamak için şunu yazın: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*Gövde içeriği*> | JSON nesnesi | İstekte göndermek için herhangi bir ileti içeriği | 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum kodu ve tüm bağlantı özel durumları aralıklı hatalar için yeniden deneme davranışı özelleştirir. Daha fazla bilgi için [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*Sorgu parametreleri*> | JSON nesnesi | İstekle birlikte içerecek şekilde tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi ekler `?api-version=2018-01-01` çağrı. | 
| <*diğer-işlem-özel-giriş-properties*> | JSON nesnesi | Bu özel eylem için geçerli herhangi bir giriş özellikleri | 
| <*diğer-işlem-özel-properties*> | JSON nesnesi | Bu özel eylem için geçerli diğer tüm özellikler | 
|||| 

*Örnek*

Bu eylem tanımı, en son haberleri belirtilen uç noktaya bir istek göndererek alır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest"
   }
}
```

<a name="join-action"></a>

### <a name="join-action"></a>Eylem katılın

Bu eylem, bir dizideki tüm öğeler bir dize oluşturur ve bu öğeleri belirtilen sınırlayıcıyı karakteriyle ayırır. 

```json
"Join": {
   "type": "Join",
   "inputs": {
      "from": <array>,
      "joinWith": "<delimiter>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Dizi*> | Dizi | Dizi veya kaynak öğeleri sağlayan bir ifade. Bir ifade belirtirseniz, bu ifade çift tırnak içine alın. | 
| <*Sınırlayıcı*> | Tek bir karakter dizesi | Her dize öğesinde ayıran karakter | 
|||| 

*Örnek*

Bu tamsayı dizisi içeren önceden oluşturulmuş "myIntegerArray" değişken olduğunu varsayalım: 

`[1,2,3,4]` 

Bu eylem tanımı değerlerini kullanarak değişkenin alır `variables()` bir ifadede işlev ve bu dizeyi virgülle ayırarak bu değerleri oluşturur: `"1,2,3,4"`

```json
"Join": {
   "type": "Join",
   "inputs": {
      "from": "@variables('myIntegerArray')",
      "joinWith": ","
   },
   "runAfter": {}
}
```

<a name="parse-json-action"></a>

### <a name="parse-json-action"></a>Eylem JSON Ayrıştır

Bu eylem, kullanıcı dostu alanları oluşturur veya *belirteçleri* özelliklerinden JSON içerik. Bu özellikleri daha sonra mantıksal uygulamanızın belirteçleri kullanarak de erişebilirsiniz. Örneğin, JSON çıktısını Azure Service Bus ve Azure Cosmos DB gibi hizmetlerden kullanmak istediğinizde, bu eylem mantıksal uygulamanızda içerebilir, böylece daha kolay, çıktı verileri de başvurabilirsiniz. 

```json
"Parse_JSON": {
   "type": "ParseJson",
   "inputs": {
      "content": "<JSON-source>",
         "schema": { "<JSON-schema>" }
      },
      "runAfter": {}
},
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*JSON-kaynak*> | JSON nesnesi | Parse JSON içeriği | 
| <*JSON şeması*> | JSON nesnesi | Temel alınan kaynak JSON içeriği ayrıştırmak için eylemini kullanan JSON içeriği tanımlayan JSON şeması. <p>**İpucu**: Logic Apps Tasarımcısı'nda için şema sağlamak veya bir örnek yük eylemi şema oluşturabilmesi sağlayın. | 
|||| 

*Örnek*

Bu eylem tanımı akışınızı ancak yalnızca eylemleri çalıştırma aşağıdaki kullanabilirsiniz, bu belirteçleri oluşturur **JSON Ayrıştır** eylem: 

`FirstName`, `LastName`, ve `Email`

```json
"Parse_JSON": {
   "type": "ParseJson",
   "inputs": {
      "content": {
         "Member": {
            "Email": "Sophie.Owen@contoso.com",
            "FirstName": "Sophie",
            "LastName": "Owen"
         }
      },
      "schema": {
         "type": "object",
         "properties": {
            "Member": {
               "type": "object",
               "properties": {
                  "Email": {
                     "type": "string"
                  },
                  "FirstName": {
                     "type": "string"
                  },
                  "LastName": {
                     "type": "string"
                  }
               }
            }
         }
      }
   },
   "runAfter": { }
},
```

Bu örnekte, "içerik" özelliği ayrıştırılamadı eylemin JSON içeriği belirtir. Bu JSON içeriği örnek yük şema oluşturmak için de sağlayabilirsiniz.

```json
"content": {
   "Member": { 
      "FirstName": "Sophie",
      "LastName": "Owen",
      "Email": "Sophie.Owen@contoso.com"
   }
},
```

"Şema" özelliği, JSON içeriği tanımlamak için kullanılan JSON şeması belirtir:

```json
"schema": {
   "type": "object",
   "properties": {
      "Member": {
         "type": "object",
         "properties": {
            "FirstName": {
               "type": "string"
            },
            "LastName": {
               "type": "string"
            },
            "Email": {
               "type": "string"
            }
         }
      }
   }
}
```

<a name="query-action"></a>

### <a name="query-action"></a>Sorgu eylemi

Bu eylem, bir koşul veya filtre temel başka bir dizideki öğelerden bir dizi oluşturur.

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": <array>,
      "where": "<condition-or-filter>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Dizi*> | Dizi | Dizi veya kaynak öğeleri sağlayan bir ifade. Bir ifade belirtirseniz, bu ifade çift tırnak içine alın. |
| <*koşul veya filtre*> | String | Kaynak dizideki öğeleri filtreleme için kullanılan koşul <p>**Not**: Hiçbir değer koşulu karşılayan, eylem boş bir dizi oluşturur. |
|||| 

*Örnek*

Bu eylem tanımı ikidir belirtilen değerden büyük değerler içeren bir dizi oluşturur:

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": [ 1, 3, 0, 5, 4, 2 ],
      "where": "@greater(item(), 2)"
   }
}
```

<a name="response-action"></a>

### <a name="response-action"></a>Yanıt eylemi  

Bu eylem bir HTTP isteğinin yanıtı yükü oluşturur. 

```json
"Response" {
    "type": "Response",
    "kind": "http",
    "inputs": {
        "statusCode": 200,
        "headers": { <response-headers> },
        "body": { <response-body> }
    },
    "runAfter": {}
},
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Yanıt durum kodu*> | Integer | Gelen istek için gönderilen HTTP durum kodu. Varsayılan kodu "200 Tamam", ancak kod 2xx, 4xx veya 5xx ancak değil 3xxx ile başlayan herhangi bir geçerli durum kodu olabilir. | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*response-headers*> | JSON nesnesi | Yanıtla eklenecek bir veya daha fazla üst bilgileri | 
| <*yanıt gövdesi*> | Çeşitli | Dize, JSON nesnesi veya bir önceki eylem bile ikili içerik yanıt gövdesi | 
|||| 

*Örnek*

Bu eylem tanımı bir HTTP isteğine yanıt olarak belirtilen durum kodu, ileti gövdesi ve ileti üstbilgileri oluşturur:

```json
"Response": {
   "type": "Response",
   "inputs": {
      "statusCode": 200,
      "body": {
         "ProductID": 0,
         "Description": "Organic Apples"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "content-type": "application/json"
      }
   },
   "runAfter": {}
}
```

*Kısıtlamaları*

Diğer Eylemler aksine **yanıt** eylem özel kısıtlamaları vardır: 

* Akışınızın kullanabileceği **yanıt** yalnızca iş akışınızı anlamına gelen HTTP isteği tetikleyicisi ile iş akışı başladığında eylem bir HTTP isteği tarafından tetiklenen gerekir.

* Akışınızın kullanabileceği **yanıt** herhangi bir eylem *dışında* içinde **Foreach** döngüleri **kadar** sıralı döngü dahil olmak üzere, döngüler ve paralel dallar. 

* Tüm eylemleri yalnızca gerekli olduğunda orijinal HTTP isteği, iş akışının yanıt alır. **yanıt** eylem içinde tamamlanmış olan [HTTP istek zaman aşımı sınırı](../logic-apps/logic-apps-limits-and-config.md#request-limits).

  Ancak, iç içe geçmiş iş akışı olarak başka bir mantıksal uygulama iş akışınızı çağırırsa, üst iş akışı iç içe geçmiş iş akışı tamamlanmadan önce ne kadar süre geçtikten ne olursa olsun iç içe geçmiş iş akışı tamamlanana kadar bekler.

* İş akışınızı kullandığında **yanıt** eylem ve zaman uyumlu yanıt düzeni, iş akışı da kullanamaz **splitOn** birden fazla çalıştırma, komut oluşturduğundan tetikleyici tanımında komutu. PUT yöntemini kullanıldığında ve true ise bu durumda denetleyin, "hatalı istek" yanıt verin.

  Aksi takdirde, iş akışınızı kullanıyorsa **splitOn** komut ve **yanıt** eylem iş akışı zaman uyumsuz olarak çalışır ve hemen bir "202 kabul edildi" yanıtı döndürür.

* İş akışının yürütme ulaştığında **yanıt** eylem, ancak gelen isteği zaten aldığı bir yanıt **yanıt** eylem çakışması nedeniyle "Başarısız" işaretlenmiş. Ve sonuç olarak, mantıksal uygulama çalıştırması "Başarısız" durumu ile de işaretlenir.

<a name="select-action"></a>

### <a name="select-action"></a>Eylem seçin

Bu eylem, belirtilen haritasına dayalı olarak başka bir diziden öğeleri dönüştürerek, JSON nesneleriyle bir dizi oluşturur. Çıkış dizisi ve kaynak dizi her zaman aynı sayıda öğe vardır. Çıkış dizideki nesne sayısını değiştiremezsiniz ancak ekleyebilir veya bu nesneler arasında özellikleri ve değerlerini Kaldır. `select` Özelliği, kaynak dizideki öğeleri dönüştürme eşlemesi tanımlayan en az bir anahtar-değer çifti belirtir. Bir anahtar-değer çifti, çıkış dizi içindeki tüm nesneler üzerinde bir özelliği ve değerini gösterir. 

```json
"Select": {
   "type": "Select",
   "inputs": {
      "from": <array>,
      "select": { 
          "<key-name>": "<expression>",
          "<key-name>": "<expression>"        
      }
   },
   "runAfter": {}
},
```

*Gerekli* 

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Dizi*> | Dizi | Dizi veya kaynak öğeleri sağlayan bir ifade. Bir ifade çift tırnak içine emin olun. <p>**Not**: Kaynak dizi boşsa, boş bir dizi eylem oluşturur. | 
| <*anahtar adı*> | String | Sonuç atanan özellik adı <*ifadesi*> <p>Çıkış dizi içindeki tüm nesneler üzerinde yeni bir özellik eklemek için belirtin bir <*anahtar adı*> Bu özellik için bir <*ifade*> özellik değeri için. <p>Dizi içindeki tüm nesneler bir özelliği kaldırmak için Atla <*anahtar adı*> Bu özellik için. | 
| <*İfade*> | String | Kaynak dizideki öğeyi dönüştüren ve sonuca atar ifade <*anahtar adı*> | 
|||| 

**Seçin** eylem Bu çıktıyı kullanmak istediği herhangi bir işlem ya da bir dizi kabul etmesi gereken şekilde bu dizi çıkış olarak oluşturur veya dizi tüketici eylemi kabul eden türe dönüştürmeniz gerekir. Örneğin, çıkış dizisi, bir dizeye dönüştürmek için bu diziye geçirebilirsiniz **Oluştur** eylemi ve ardından çıktısını başvuru **oluşturma** diğer eylemlerinizi eylem.

*Örnek*

Bu eylem tanımı, tamsayı dizisinden bir JSON nesne dizisi oluşturur. Eylem kaynağı dizi aracılığıyla yinelenir kullanarak her bir tamsayı değeri alır `@item()` ifade ve atar her değer için "`number`" her JSON nesnesi özelliği: 

```json
"Select": {
   "type": "Select",
   "inputs": {
      "from": [ 1, 2, 3 ],
      "select": { 
         "number": "@item()" 
      }
   },
   "runAfter": {}
},
```

Bu eylem oluşturan bir dizi şöyledir:

`[ { "number": 1 }, { "number": 2 }, { "number": 3 } ]`

Diğer Eylemler çıkış bu dizi kullanmak için bu çıkış geçirin. bir **Compose** eylem:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@body('Select')",
   "runAfter": {
      "Select": [ "Succeeded" ]
   }
},
```

Çıktıdan sonra kullanabilirsiniz **Compose** diğer eylemlerinizi, örneğin, bir uygulamada **Office 365 Outlook - e-posta Gönder** eylem:

```json
"Send_an_email": {
   "type": "ApiConnection",
   "inputs": {
      "body": {
         "Body": "@{outputs('Compose')}",
         "Subject": "Output array from Select and Compose actions",
         "To": "<your-email@domain>"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "post",
      "path": "/Mail"
   },
   "runAfter": {
      "Compose": [ "Succeeded" ]
   }
},
```

<a name="table-action"></a>

### <a name="table-action"></a>Tablo eylemi

Bu eylem, bir diziyi bir CSV veya HTML tablosu oluşturur. JSON nesnesi içeren diziler için bu eylem nesnelerin özelliği adlarından sütun üst bilgilerini otomatik olarak oluşturur. Diğer veri türlerini içeren diziler için sütun üst bilgilerini ve değerlerini belirtmeniz gerekir. Örneğin, bu dizi, bu eylem için sütun başlık adları kullanabilirsiniz "ID" ve "Ürün_Adı" özellikleri içerir:

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]` 

```json
"Create_<CSV | HTML>_table": {
   "type": "Table",
   "inputs": {
      "format": "<CSV | HTML>",
      "from": <array>,
      "columns": [ 
         {
            "header": "<column-name>",
            "value": "<column-value>"
         },
         {
            "header": "<column-name>",
            "value": "<column-value>"
         } 
      ]
   },
   "runAfter": {}
}
```

*Gerekli* 

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| < CSV *veya* HTML >| String | Oluşturmak istediğiniz tablo biçimi | 
| <*Dizi*> | Dizi | Dizi veya tablo için kaynak öğeleri sağlayan ifade <p>**Not**: Eylem, kaynak diziden boşsa, boş bir tablo oluşturur. | 
|||| 

*İsteğe bağlı*

Sütun üst bilgilerini ve değerleri özelleştirmenize veya belirtmek için kullanın `columns` dizisi. Zaman `header-value` çiftleri aynı üst bilgi adı varsa, bu üst bilgi adı altındaki aynı sütun değerlerine görünür. Aksi takdirde, her bir benzersiz üstbilgisi benzersiz bir sütun tanımlar.

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*sütun adı*> | String | Bir sütun için üst bilgi adı | 
| <*Sütun değeri*> | Tüm | Bu sütunda değeri | 
|||| 

*Örnek 1*

Şu anda bu dizi içeren bir önceden oluşturulmuş "myItemArray" değişken olduğunu varsayalım: 

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]`

Bu eylem tanımı "myItemArray" değişkeninden CSV tablosu oluşturur. Tarafından kullanılan ifade `from` özelliğini alır dizisi "myItemArray" kullanarak `variables()` işlevi: 

```json
"Create_CSV_table": {
   "type": "Table",
   "inputs": {
      "format": "CSV",
      "from": "@variables('myItemArray')"
   },
   "runAfter": {}
}
```

Bu eylem oluşturur CSV tablosu şu şekildedir: 

```
ID,Product_Name 
0,Apples 
1,Oranges 
```

*Örnek 2*

Bu eylem tanımı "myItemArray" değişkeninden bir HTML tablosu oluşturur. Tarafından kullanılan ifade `from` özelliğini alır dizisi "myItemArray" kullanarak `variables()` işlevi: 

```json
"Create_HTML_table": {
   "type": "Table",
   "inputs": {
      "format": "HTML",
      "from": "@variables('myItemArray')"
   },
   "runAfter": {}
}
```

Bu eylem oluşturur HTML tablosu şu şekildedir: 

<table><thead><tr><th>Kimlik</th><th>Ürün_Adı</th></tr></thead><tbody><tr><td>0</td><td>Elma</td></tr><tr><td>1</td><td>Portakallar</td></tr></tbody></table>

*Örnek 3*

Bu eylem tanımı "myItemArray" değişkeninden bir HTML tablosu oluşturur. Ancak, bu örnekte "Stock_ID" ve "Description" varsayılan sütun başlığı adları geçersiz kılar ve "Organic" sözcük "Description" sütunundaki değerler ekler.

```json
"Create_HTML_table": {
   "type": "Table",
   "inputs": {
      "format": "HTML",
      "from": "@variables('myItemArray')",
      "columns": [ 
         {
            "header": "Stock_ID",
            "value": "@item().ID"
         },
         {
            "header": "Description",
            "value": "@concat('Organic ', item().Product_Name)"
         }
      ]
    },
   "runAfter": {}
},
```

Bu eylem oluşturur HTML tablosu şu şekildedir: 

<table><thead><tr><th>Stock_ID</th><th>Açıklama</th></tr></thead><tbody><tr><td>0</td><td>Organik elma</td></tr><tr><td>1</td><td>Organik portakallar</td></tr></tbody></table>

<a name="terminate-action"></a>

### <a name="terminate-action"></a>Sonlandırma eylemi

Bu eylem çalıştırmak için bir iş akışı örneği durdurur, devam eden tüm eylemleri iptal eder, kalan tüm eylemler atlar ve belirtilen duruma döndürür. Örneğin, kullanabileceğiniz **sonlandırma** mantıksal uygulamanızı tamamen bir hata durumundan çıkmak gerekir çalıştırıldığında eylem. Bu eylem zaten tamamlanmış Eylemler etkilemez ve içinde yer alamaz **Foreach** ve **kadar** döngüleri, sıralı döngüleri dahil. 

```json
"Terminate": {
   "type": "Terminate",
   "inputs": {
       "runStatus": "<status>",
       "runError": {
            "code": "<error-code-or-name>",
            "message": "<error-message>"
       }
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*status*> | String | Çalıştırmak için döndürülecek durum: "Başarısız", "İptal" veya "Başarılı" |
|||| 

*İsteğe bağlı*

"RunStatus" nesnesi için özellikler, yalnızca "runStatus" özelliği "Başarısız" durumuna ayarlandığında geçerlidir.

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*hata-code-veya-name*> | String | Kod veya hata adı |
| <*hata iletisi*> | String | İleti veya hata ve eylemleri açıklayan metin uygulama kullanıcısının sürebilir | 
|||| 

*Örnek*

Bu eylem tanımı bir iş akışı çalıştırmasını durdurur, "Başarısız" çalışma durumuna ayarlar ve durumu, bir hata kodu ve bir hata iletisi döndürür:

```json
"Terminate": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "Unexpected response",
            "message": "The service received an unexpected response. Please try again."
        }
   },
   "runAfter": {}
}
```

<a name="wait-action"></a>

### <a name="wait-action"></a>Eylem bekleyin  

Bu eylem, belirtilen zaman aralığı veya belirtilen süre, ancak ikisini birden kadar iş akışı yürütme duraklatır. 

*Belirtilen zaman aralığı*

```json
"Delay": {
   "type": "Wait",
   "inputs": {
      "interval": {
         "count": <number-of-units>,
         "unit": "<interval>"
      }
   },
   "runAfter": {}
},
```

*Belirtilen zaman*

```json
"Delay_until": {
   "type": "Wait",
   "inputs": {
      "until": {
         "timestamp": "<date-time-stamp>"
      }
   },
   "runAfter": {}
},
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Birim sayısı*> | Integer | İçin **gecikme** eylemi, beklemek için birim sayısı | 
| <*aralığı*> | String | İçin **gecikme** eylemi, beklenecek: "Saniye", "Minute", "Hour", "Day", "Week", "Month" | 
| <*tarih zaman damgası*> | String | İçin **gecikme kadar** eylem yürütme devam etmek için tarih ve saat. Bu değer kullanmalıdır [UTC tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time). | 
|||| 

*Örnek 1*

Bu eylem tanımı iş akışı 15 dakika boyunca duraklatır:

```json
"Delay": {
   "type": "Wait",
   "inputs": {
      "interval": {
         "count": 15,
         "unit": "Minute"
      }
   },
   "runAfter": {}
},
```

*Örnek 2*

Bu eylem tanımı, belirtilen süre kadar iş akışı duraklatır:

```json
"Delay_until": {
   "type": "Wait",
   "inputs": {
      "until": {
         "timestamp": "2017-10-01T00:00:00Z"
      }
   },
   "runAfter": {}
},
```

<a name="workflow-action"></a>

### <a name="workflow-action"></a>İş akışı eylemi

Bu eylem, içerir ve diğer mantıksal uygulama iş akışlarını yeniden anlamına gelir. başka bir önceden oluşturulmuş mantıksal uygulama çağırır. Alt çıkışları kullanabilirsiniz veya *iç içe geçmiş* mantıksal uygulama eylemleri iç içe geçmiş mantıksal uygulamayı izleyin ve koşuluyla alt mantıksal uygulama bir yanıt döndürür.

Logic Apps altyapısı çağırmak için bu nedenle, tetikleyici erişebildiğinden emin olun, istediğiniz tetikleyici erişimi denetler. Ayrıca, iç içe geçmiş mantıksal uygulamayı şu ölçütleri karşılamalıdır:

* İç içe geçmiş mantıksal uygulama bir tetikleyici çağrılabilir gibi yapar bir [istek](#request-trigger) veya [HTTP](#http-trigger) tetikleyicisi

* Üst mantıksal uygulamanızı aynı Azure aboneliğinde

* İç içe geçmiş mantıksal uygulamayı çıkışları üst mantıksal uygulamanızda kullanmak için iç içe geçmiş mantıksal uygulamayı olmalıdır bir [yanıt](#response-action) eylemi 

```json
"<nested-logic-app-name>": {
   "type": "Workflow",
   "inputs": {
      "body": { "<body-content" },
      "headers": { "<header-content>" },
      "host": {
         "triggerName": "<trigger-name>",
         "workflow": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group>/providers/Microsoft.Logic/<nested-logic-app-name>"
         }
      }
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*iç içe mantıksal--adı*> | String | Aramak istediğiniz mantıksal uygulamanın adı | 
| <*Tetikleyici adı*> | String | İç içe geçmiş mantıksal uygulamayı çağırmak istediğinizde Tetikleyici adı | 
| <*Azure abonelik kimliği*> | String | İç içe geçmiş mantıksal uygulamayı Azure abonelik kimliği |
| <*Azure kaynak grubu*> | String | İç içe geçmiş mantıksal uygulamayı Azure kaynak grubu adı |
| <*iç içe mantıksal--adı*> | String | Aramak istediğiniz mantıksal uygulamanın adı |
||||

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*header-content*> | JSON nesnesi | Çağrı göndermek için üst bilgileri | 
| <*Gövde içeriği*> | JSON nesnesi | Çağrı göndermek için herhangi bir ileti içeriği | 
||||

*Çıkışlar*

Bu eylemin çıktıları iç içe geçmiş mantıksal uygulamanın yanıt eylemi bağlı olarak değişiklik gösterir. İç içe geçmiş mantıksal uygulamayı bir yanıt eylemi içermiyorsa, çıkış boş.

*Örnek*

"Start_search" eylemi başarıyla tamamlandıktan sonra bu iş akışı eylemi tanımı belirtilen girişlerinde geçirir "Get_product_information" adlı başka bir mantıksal uygulama çağırır: 

```json
"actions": {
   "Start_search": { <action-definition> },
   "Get_product_information": {
      "type": "Workflow",
      "inputs": {
         "body": {
            "ProductID": "24601",
         },
         "host": {
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/InventoryManager-RG/providers/Microsoft.Logic/Get_product_information",
            "triggerName": "Find_product"
         },
         "headers": {
            "content-type": "application/json"
         }
      },
      "runAfter": { 
         "Start_search": [ "Succeeded" ]
      }
   }
},
```

## <a name="control-workflow-action-details"></a>Denetim iş akışı eylemi ayrıntıları

<a name="foreach-action"></a>

### <a name="foreach-action"></a>Foreach eylemi

Döngü Bu eylem, bir dizi aracılığıyla yinelenir ve dizideki tüm eylemleri gerçekleştirir. Varsayılan olarak, "for each" döngüsü paralel döngüler en fazla sayıya kadar çalışır. Bu en fazla için bkz. [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Bilgi ["for each" oluşturma döngüler](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop).

```json
"For_each": {
   "type": "Foreach",
   "actions": { 
      "<action-1>": { "<action-definition-1>" },
      "<action-2>": { "<action-definition-2>" }
   },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": <count>
      }
    },
    "operationOptions": "<operation-option>"
}
```

*Gerekli* 

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*action-1...n*> | String | Her dizi öğesi üzerinde çalışan eylemlerin adları | 
| <*Eylem-definition-1... n*> | JSON nesnesi | Çalışan eylemlerin tanımları | 
| <*için-her-ifadesi*> | String | Belirtilen dizideki her öğe başvuran ifadesi | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Sayısı*> | Integer | Varsayılan olarak, yineleme aynı anda veya paralel kadar Çalıştır "for each" döngüsü [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Yeni bir ayarlayarak bu sınırı değiştirmek için <*sayısı*> değeri için bkz: ["for each" döngüsü değiştirme eşzamanlılık](#change-for-each-concurrency). | 
| <*işlem seçeneği*> | String | Paralel yapmak yerine, sırasıyla, bir "for each" döngüsü çalıştırmak için ya da ayarlayın <*işlem seçeneği*> için `Sequential` veya <*sayısı*> için `1`, ikisini birden belirtmeyin. Daha fazla bilgi için bkz [Çalıştır "for each" döngüsü sırayla](#sequential-for-each). | 
|||| 

*Örnek*

Bu "for each" döngüsü, gelen e-posta eklerini içeren dizideki her öğe için bir e-posta gönderir. Döngü eki incelemeleri bir kişiye eki içeren bir e-posta gönderir.

```json
"For_each": {
   "type": "Foreach",
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "body": {
               "Body": "@base64ToString(items('For_each')?['Content'])",
               "Subject": "Review attachment",
               "To": "Sophie.Owen@contoso.com"
                },
            "host": {
               "connection": {
                  "id": "@parameters('$connections')['office365']['connectionId']"
               }
            },
            "method": "post",
            "path": "/Mail"
         },
         "runAfter": {}
      }
   },
   "foreach": "@triggerBody()?['Attachments']",
   "runAfter": {}
}
```

Tetikleyiciden çıktı olarak iletilen bir dizi belirtmek için bu ifade alır <*dizi adı*> tetikleyici gövdesindeki diziden. Dizinin mevcut değilse, bir hatadan kaçınmak için ifade kullanır. `?` işleci:

`@triggerBody()?['<array-name>']` 

<a name="if-action"></a>

### <a name="if-action"></a>Eylem

Olduğundan bu eylem bir *koşullu ifade*, bir koşulu temsil eder ve koşul true olmasına göre farklı bir dal çalıştıran bir ifadeyi değerlendirir ya da yanlış. Koşul true ise, koşulu "Başarılı" durumu ile işaretlenir. Bilgi [koşul deyimlerini oluşturmak nasıl](../logic-apps/logic-apps-control-flow-conditional-statement.md).

``` json
"Condition": {
   "type": "If",
   "expression": { "<condition>" },
   "actions": {
      "<action-1>": { "<action-definition>" }
   },
   "else": {
      "actions": {
        "<action-2>": { "<action-definition" }
      }
   },
   "runAfter": {}
}
```

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Koşul*> | JSON nesnesi | Koşul, değerlendirilecek bir ifade olabilir | 
| <*Eylem-1*> | JSON nesnesi | Çalıştırılan eylem <*koşul*> true olarak değerlendirilir | 
| <*Eylem-definition*> | JSON nesnesi | Eylem tanımı | 
| <*Eylem-2*> | JSON nesnesi | Çalıştırılan eylem <*koşul*> yanlış olarak değerlendirilir | 
|||| 

Eylemler `actions` veya `else` nesneler bu durumlar alın:

* "Çalıştırdığınızda ve başarılı başarılı oldu"
* "Çalıştırdığınızda ve başarısız başarısız oldu"
* "Skipped" karşılık gelen dal çalıştırdığınızda değil

*Örnek*

Bu durum, tamsayı değişkeni sıfırdan büyük bir değer varsa, iş akışı bir Web sitesini denetler belirtir. Değişken sıfır veya daha az ise, iş akışı farklı bir Web sitesini denetler.

```json
"Condition": {
   "type": "If",
   "expression": {
      "and": [ {
         "greater": [ "@variables('myIntegerVariable')", 0 ] 
      } ]
   },
   "actions": { 
      "HTTP - Check this website": {
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
         "HTTP - Check this other website": {
            "type": "Http",
            "inputs": {
               "method": "GET",
               "uri": "http://this-other-url"
            },
            "runAfter": {}
         }
      }
   },
   "runAfter": {}
}
```  

#### <a name="how-conditions-use-expressions"></a>Koşul ifadeleri kullanma

Koşullarda ifadeleri nasıl kullanabileceğinizi gösteren bazı örnekler şunlardır:
  
| JSON | Sonuç | 
|------|--------| 
| "ifadesi": "@parameters('<*hasSpecialAction*>')" | Yalnızca Boolean ifadeler için koşul true olarak değerlendirilen herhangi bir değer için başarılı. <p>Diğer türleri Boolean değerine dönüştürmek için bu işlevleri kullanın: `empty()` veya `equals()`. | 
| "ifadesi": "@greater(actions('<*action*>').output.value, parametreleri ('<*eşiği*>'))" | Karşılaştırma işlevleri için eylem yalnızca çalışan çıktısı <*eylem*> olan birden çok <*eşiği*> değeri. | 
| "ifadesi": "@or(büyük (actions('<*action*>').output.value, parametreleri ('<*eşiği*>')), daha az (Eylemler ('<*eylem aynı*>').output.Value, 100)) " | Mantıksal işlevler ve oluşturma Boolean ifadeleri iç içe için eylem olduğunda çalışan çıktısı <*eylem*> olan birden çok <*eşiği*> değer veya 100'altında. | 
| "ifadesi": "@equals(uzunluk (actions('<*action*>').outputs.errors), 0))" | Dizi öğeleri olup olmadığını denetlemek için dizi işlevlerini kullanabilirsiniz. Eylem olduğunda çalışan `errors` dizidir boş. | 
||| 

<a name="scope-action"></a>

### <a name="scope-action"></a>Kapsam eylemi

Bu eylem eylemlere mantıksal olarak gruplandıran *kapsamları*, hangi alma kendi durum eylemleri sonra kapsam son çalıştırma. Kapsamın durumu sonra diğer eylemleri çalıştırmak olup olmadığını belirlemek için de kullanabilirsiniz. Bilgi [kapsamlar oluşturmak nasıl](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md).

```json
"Scope": {
   "type": "Scope",
   "actions": {
      "<inner-action-1>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      },
      "<inner-action-2>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      }
   }
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*inner-action-1...n*> | JSON nesnesi | Kapsam içinde çalışacak bir veya daha fazla eylemleri |
| <*Eylem girişleri*> | JSON nesnesi | Her eylem için giriş |
|||| 

<a name="switch-action"></a>

### <a name="switch-action"></a>İşlemi Değiştir

Olarak da bilinen bu eylem, bir *geçiş deyimi*, diğer eylemlere düzenler *çalışmaları*ve varsa, her durumda, varsayılan durum dışında bir değer atar. Akışınız çalıştığında, **anahtar** eylem değeri bir ifade, nesne veya belirteç her örneği için belirtilen değerlere karşı karşılaştırır. Varsa **anahtar** eylem eşleşen servis talebi bulur, yalnızca bu Eylemler, iş akışı çalıştırır. Her zaman **anahtar** eylemi çalıştığında, yalnızca bir ya da eşleşen servis talebi yok veya herhangi bir eşleşme yok. Herhangi bir eşleşme varsa **anahtar** eylem varsayılan eylemleri çalıştırır. Bilgi [switch deyimleri oluşturma](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"Switch": {
   "type": "Switch",
   "expression": "<expression-object-or-token>",
   "cases": {
      "Case": {
         "actions": {
           "<action-name>": { "<action-definition>" }
         },
         "case": "<matching-value>"
      },
      "Case_2": {
         "actions": {
           "<action-name>": { "<action-definition>" }
         },
         "case": "<matching-value>"
      }
   },
   "default": {
      "actions": {
         "<default-action-name>": { "<default-action-definition>" }
      }
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*ifade-nesne-veya-token*> | Değişir | İfadesi, JSON nesnesi veya değerlendirmek için belirteci | 
| <*Eylem adı*> | String | Eşleşen servis talebi için çalıştırılacak eylemin adı | 
| <*Eylem-definition*> | JSON nesnesi | Eşleşen servis talebi için çalıştırma eylemini tanımı | 
| <*eşleşen değer*> | Değişir | Değerlendirilen sonucuyla Karşılaştırılacak değer | 
|||| 

*İsteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Varsayılan eylem adı*> | String | Hiç eşleşen servis talebi bulunduğunda çalıştırmak için varsayılan eylem adı | 
| <*Varsayılan eylem tanımı*> | JSON nesnesi | Hiç eşleşen servis talebi mevcut olduğunda çalıştırılacak eylemi tanımı | 
|||| 

*Örnek*

Bu eylem tanımı yanıt onay isteği e-posta için kişi "Onayla" veya "Reddet" seçeneğini seçili olup olmadığını değerlendirir. Bu, tercihine bağlı **anahtar** eylem Yanıtlayıcı, ancak her durumda farklı ifade başka bir e-posta göndermek için eşleşen bir durum için eylemleri çalıştırır. 

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
            "Send_an_email": { 
               "type": "ApiConnection",
               "inputs": {
                  "Body": "Thank you for your approval.",
                  "Subject": "Response received", 
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions": {
            "Send_an_email_2": { 
               "type": "ApiConnection",
               "inputs": {
                  "Body": "Thank you for your response.",
                  "Subject": "Response received", 
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}     
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": { 
         "Send_an_email_3": { 
            "type": "ApiConnection",
            "inputs": {
               "Body": "Please respond with either 'Approve' or 'Reject'.",
               "Subject": "Please respond", 
               "To": "Sophie.Owen@contoso.com"
            },
            "host": {
               "connection": {
                  "name": "@parameters('$connections')['office365']['connectionId']"
               }
            },
            "method": "post",
            "path": "/Mail"
         },
         "runAfter": {} 
      }
   },
   "runAfter": {
      "Send_approval_email": [ 
         "Succeeded"
      ]
   }
}
```

<a name="until-action"></a>

### <a name="until-action"></a>Eylem kadar

Bu döngü eylemini belirtilen koşul true olana kadar çalıştırılan eylemleri içerir. Diğer tüm eylemler çalıştırdıktan sonra Döngü koşulu son adım olarak denetler. Birden fazla eylem içerebilir `"actions"` nesne ve eylem en az bir sınır tanımlaması gerekir. Bilgi ["kadar" döngüler oluşturmak nasıl](../logic-apps/logic-apps-control-flow-loops.md#until-loop). 

```json
 "Until": {
   "type": "Until",
   "actions": {
      "<action-name>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      },
      "<action-name>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      }
   },
   "expression": "<condition>",
   "limit": {
      "count": <loop-count>,
      "timeout": "<loop-timeout>"
   },
   "runAfter": {}
}
```

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Eylem adı*> | String | Döngünün içinde çalıştırmak istediğiniz eylemin adı | 
| <*Eylem türü*> | String | Çalıştırmak istediğiniz eylem türü | 
| <*Eylem girişleri*> | Çeşitli | Çalıştırılacak eylemi için girişler | 
| <*Koşul*> | String | Koşul veya sonra değerlendirilecek ifade Döngüdeki eylemleri sonlanması | 
| <*döngü sayımı*> | Integer | En iyi eylem çalıştırabilirsiniz döngüleri sayısını sınırlama. Varsayılan `count` 60 değerdir. | 
| <*döngü zaman aşımı*> | String | Döngü çalıştırabilirsiniz en uzun süre sınırı. Varsayılan `timeout` değer `PT1H`, gerekli olduğu [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601). |
|||| 

*Örnek*

Şu koşullardan biri karşılanana kadar bu döngü eylem tanımı bir HTTP isteği belirtilen URL'ye gönderir: 

* İstek ile bir yanıt alır "200 Tamam" durum kodu.
* Döngü 60 bir kez çalışır.
* Döngü bir saat boyunca çalıştırıldı.

```json
 "Run_until_loop_succeeds_or_expires": {
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
    "expression": "@equals(outputs('Http')['statusCode', 200])",
    "limit": {
        "count": 60,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

<a name="subscribe-unsubscribe"></a>

## <a name="webhooks-and-subscriptions"></a>Web kancaları ve abonelikler

Web kancası tabanlı tetikleyiciler ve Eylemler uç noktalar düzenli olarak iade etmeyin, ancak bunun yerine belirli olayları veya bu Uç noktalara veri için bekleyin. Bu tetikleyiciler ve Eylemler *abone* sağlayarak uç noktalarına bir *geri çağırma URL'si* burada uç nokta gönderebilir yanıtlar.

`subscribe` Çağrı, iş akışı herhangi bir yolla, örneğin, kimlik bilgilerini yenilendiğinde ya da bir tetikleyici veya eylem için giriş parametrelerini değiştirme değiştiğinde gerçekleşir. Bu çağrı, standart HTTP eylem olarak aynı parametreleri kullanır. 

`unsubscribe` Çağrısı bir işlem tetikleyici veya eylemi örneğin geçersiz yaptığında otomatik olarak gerçekleşir:

* Silme veya tetikleyici devre dışı bırakılıyor. 
* Silme veya iş akışı devre dışı bırakılıyor. 
* Silme veya abonelik devre dışı bırakılıyor. 

Bu çağrılar desteklemek için `@listCallbackUrl()` benzersiz bir ifade döndürür "geri çağırma URL'si" tetikleyici veya eylemi için. Bu URL, hizmetin REST API uç noktaları için benzersiz bir tanımlayıcı temsil eder. Bu işlev için parametre olarak Web kancası tetikleyici veya eylemi aynıdır.

<a name="asynchronous-limits"></a>

## <a name="change-asynchronous-duration"></a>Zaman uyumsuz süresini değiştirme

Tetikleyiciler ve Eylemler için ekleyerek belirli bir zaman aralığı için zaman uyumsuz desen süresini sınırlayabilirsiniz `limit.timeout` özelliği. Aralığı kesildiyse, eylemin durumu olarak işaretlendiğinde eylemi tamamlanmadı, bu şekilde, `Cancelled` ile `ActionTimedOut` kod. `timeout` Özelliği kullanan [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). 

``` json
"<trigger-or-action-name>": {
   "type": "Workflow | Webhook | Http | ApiConnectionWebhook | ApiConnection",
   "inputs": {},
   "limit": {
      "timeout": "PT10S"
   },
   "runAfter": {}
}
```

<a name="runtime-config-options"></a>

## <a name="runtime-configuration-settings"></a>Çalışma zamanı yapılandırma ayarları

Tetikleyiciler ve Eylemler ile bu varsayılan çalışma zamanı davranışını değiştirebilirsiniz `runtimeConfiguration` tetikleyici veya eylemi tanımındaki özellikler.

| Özellik | Tür | Açıklama | Tetikleyici veya eylemi | 
|----------|------|-------------|-------------------| 
| `runtimeConfiguration.concurrency.runs` | Integer | Değişiklik [ *varsayılan sınırı* ](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) aynı anda ya da paralel iş akışı örnekleri sayısı. Bu değer, arka uç sistemlerine alma isteklerinin sayısı sınırlandırmanıza yardımcı olabilir. <p>Ayarı `runs` özelliğini `1` ayarını aynı şekilde çalışır `operationOptions` özelliğini `SingleInstance`. Ya da özellik, her ikisini de ayarlayabilirsiniz. <p>Varsayılan sınırı değiştirmek için bkz [değişiklik tetikleyici eşzamanlılık](#change-trigger-concurrency) veya [tetikleme örnekleri sırayla](#sequential-trigger). | Tüm tetikleyiciler | 
| `runtimeConfiguration.concurrency.maximumWaitingRuns` | Integer | Değişiklik [ *varsayılan sınırı* ](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) akışınızı zaten maksimum eşzamanlı örnek çalışırken çalıştırmak için bekleyebileceği iş akışı örnekleri sayısı. Eşzamanlılık sınırı değiştirebilirsiniz `concurrency.runs` özelliği. <p>Varsayılan sınırı değiştirmek için bkz [değişiklik bekleme çalıştırmaları sınırlamak](#change-waiting-runs). | Tüm tetikleyiciler | 
| `runtimeConfiguration.concurrency.repetitions` | Integer | Değişiklik [ *varsayılan sınırı* ](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) sayısı "for each" döngüsü aynı anda ya da paralel yineleme. <p>Ayarı `repetitions` özelliğini `1` ayarını aynı şekilde çalışır `operationOptions` özelliğini `SingleInstance`. Ya da özellik, her ikisini de ayarlayabilirsiniz. <p>Varsayılan sınırı değiştirmek için bkz ["for each" eşzamanlılık değiştirme](#change-for-each-concurrency) veya ["for each" çalıştırma sırayla döngü](#sequential-for-each). | Eylem: <p>[Foreach](#foreach-action) | 
| `runtimeConfiguration.paginationPolicy.minimumItemCount` | Integer | Bu değer, destek ve sayfalandırma açık olan belirli eylemler için belirtir *minimum* almak için sonuç sayısı. <p>Sayfalandırma üzerinde etkinleştirmek için bkz: [toplu veri, öğeleri veya sonuçlarını sayfalandırma kullanarak elde](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md) | Eylem: Değiştirilen |
| `runtimeConfiguration.staticResult` | JSON nesnesi | Destek ve Eylemler için [statik sonucu](../logic-apps/test-logic-apps-mock-data-static-results.md) , ayarı `staticResult` nesne, bu özniteliklere sahip: <p>- `name`, görünen geçerli eylem statik sonucu tanımı adı başvuran `staticResults` , mantıksal uygulama iş akışınızın özniteliğinde `definition` özniteliği. Daha fazla bilgi için [statik sonuçları - iş akışı tanımı dil şeması başvurusu](../logic-apps/logic-apps-workflow-definition-language.md#static-results). <p> - `staticResultOptions`, statik sonuçları olup olmadığını belirtir `Enabled` veya geçerli eylem için değil. <p>Statik sonuçlarına kapatmak için bkz [statik sonuçlarını ayarlayarak logic apps sahte veriler ile Test](../logic-apps/test-logic-apps-mock-data-static-results.md) | Eylem: Değiştirilen |
||||| 

<a name="operation-options"></a>

## <a name="operation-options"></a>İşlem Seçenekleri

Tetikleyiciler ve Eylemler ile varsayılan davranışı değiştirebilirsiniz `operationOptions` tetikleyici veya eylemi, tanımında özelliği.

| İşlem seçeneği | Tür | Açıklama | Tetikleyici veya eylemi | 
|------------------|------|-------------|-------------------| 
| `DisableAsyncPattern` | String | HTTP tabanlı Eylemler, zaman uyumlu olarak yerine zaman uyumsuz olarak çalışır. <p><p>Bu seçeneği belirlemek için bkz: [eylemleri eşzamanlı çalışacak](#asynchronous-patterns). | Eylemler: <p>[ApiConnection](#apiconnection-action), <br>[HTTP](#http-action), <br>[Yanıt](#response-action) | 
| `OptimizedForHighThroughput` | String | Değişiklik [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) eylem yürütme için 5 dakika başına sayısına [sınırı](../logic-apps/logic-apps-limits-and-config.md#throughput-limits). <p><p>Bu seçeneği belirlemek için bkz: [yüksek üretilen iş modunda çalışacak](#run-high-throughput-mode). | Tüm eylemleri | 
| `Sequential` | String | "For each" çalıştırma paralel aynı anda tüm yerine, bir saatte bir yineleme döngüsü. <p>Bu seçenek ayarını aynı şekilde çalışır `runtimeConfiguration.concurrency.repetitions` özelliğini `1`. Ya da özellik, her ikisini de ayarlayabilirsiniz. <p><p>Bu seçeneği belirlemek için bkz [Çalıştır "for each" döngüsü sırayla](#sequential-for-each).| Eylem: <p>[Foreach](#foreach-action) | 
| `SingleInstance` | String | Tetikleyici her mantıksal uygulama örneği için sırayla çalışır ve daha önce etkin çalıştırma sonraki mantıksal uygulama örneği tetiklemeden önce tamamlanması için bekleyin. <p><p>Bu seçenek ayarını aynı şekilde çalışır `runtimeConfiguration.concurrency.runs` özelliğini `1`. Ya da özellik, her ikisini de ayarlayabilirsiniz. <p>Bu seçeneği belirlemek için bkz: [örnekleri sırayla tetiklemek](#sequential-trigger). | Tüm tetikleyiciler | 
||||

<a name="change-trigger-concurrency"></a>

### <a name="change-trigger-concurrency"></a>Değişiklik tetikleyici eşzamanlılık

Varsayılan olarak, mantıksal uygulama örneği aynı anda aynı anda veya paralel kadar çalıştırma [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Bu nedenle, önceki iş akışı örneği çalıştırma tamamlanmadan önce her bir tetikleyici örneği tetikler. Bu sınır, arka uç sistemlerine alma isteklerinin sayısı denetim yardımcı olur. 

Ekler veya güncelleştirir Tasarımcısı aracılığıyla Eş zamanlılık ayarı değiştirmek için varsayılan sınırı değiştirmek için kod görünümü Düzenleyicisi veya Logic Apps Tasarımcısı'nda kullanabileceğiniz `runtimeConfiguration.concurrency.runs` özelliği temel tetikleyici tanımı ve bunun tersi de geçerlidir. Bu özellik paralel olarak çalıştırılabilir iş akışı örneği en fazla sayısını denetler. 

> [!NOTE] 
> Sıralı olarak kullanarak ya da Tasarımcı veya kod görünümü Düzenleyicisi'ni çalıştırmak için tetikleyiciyi ayarlayın, tetikleyicinin ayarlamamanız `operationOptions` özelliğini `SingleInstance` kod görünümü düzenleyicisinde. Aksi takdirde, bir doğrulama hatası alırsınız. Daha fazla bilgi için [örnekleri sırayla tetiklemek](#sequential-trigger).

#### <a name="edit-in-code-view"></a>Kod Görünümü'nde Düzenle 

Temel tetikleyicisi tanımı, ekleme veya güncelleştirme `runtimeConfiguration.concurrency.runs` arasında bir değer özelliğini `1` ve `50` aralığında.

10 örneğe eş zamanlı çalıştırma sınırlayan bir örnek aşağıda verilmiştir:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": 10
      }
   }
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps Tasarımcısı'nda Düzenle

1. Tetikleyicinin sağ üst köşedeki üç nokta (...) düğmesini seçin ve ardından **ayarları**.

2. Altında **eşzamanlılık denetimi**ayarlayın **sınırı** için **üzerinde**. 

3. Sürükleme **paralellik derecesi** kaydırıcıyı istediğiniz değer. Mantıksal uygulamanızı sıralı olarak çalıştırmak için kaydırıcı değeri sürükleyin **1**.

<a name="change-for-each-concurrency"></a>

### <a name="change-for-each-concurrency"></a>"For each" eşzamanlılık değiştirme

Varsayılan olarak, "for each" döngüsü yinelemeleri aynı anda veya paralel olarak en fazla çalıştırma [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Ekler veya güncelleştirir Tasarımcısı aracılığıyla Eş zamanlılık ayarı değiştirmek için varsayılan sınırı değiştirmek için kod görünümü Düzenleyicisi veya Logic Apps Tasarımcısı'nda kullanabileceğiniz `runtimeConfiguration.concurrency.repetitions` "for each" eylem temel özellik tanımı geçme veya tam tersi. Bu özellik paralel olarak çalıştırılabilir yineleme sayısını denetler.

> [!NOTE] 
> Sıralı olarak kullanarak ya da Tasarımcı veya kod görünümü Düzenleyicisi'ni çalıştırmak için "for each" eylem ayarlarsanız, eylemin ayarlamamanız `operationOptions` özelliğini `Sequential` kod görünümü düzenleyicisinde. Aksi takdirde, bir doğrulama hatası alırsınız. Daha fazla bilgi için bkz [Çalıştır "for each" döngüsü sırayla](#sequential-for-each).

#### <a name="edit-in-code-view"></a>Kod Görünümü'nde Düzenle 

"For each" tanımı temel ekleme veya güncelleştirme `runtimeConfiguration.concurrency.repetitions` arasında bir değer özelliğini `1` ve `50` aralığında. 

Eş zamanlı çalıştırma 10 yinelemelere sınırlayan bir örnek aşağıda verilmiştir:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": 10
      }
   }
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps Tasarımcısı'nda Düzenle

1. İçinde **her** eylem, sağ üst köşedeki üç nokta (...) düğmesini seçin ve ardından **ayarları**.

2. Altında **eşzamanlılık denetimi**ayarlayın **eşzamanlılık denetimi** için **üzerinde**. 

3. Sürükleme **paralellik derecesi** kaydırıcıyı istediğiniz değer. Mantıksal uygulamanızı sıralı olarak çalıştırmak için kaydırıcı değeri sürükleyin **1**.

<a name="change-waiting-runs"></a>

### <a name="change-waiting-runs-limit"></a>Çalıştırmaları sınırı bekleyen değişiklik

Varsayılan olarak, tüm mantıksal uygulama iş akışı örneği aynı anda aynı anda veya paralel kadar çalıştırın [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Daha önce etkin iş akışı örneği çalıştırma tamamlanmadan önce her bir tetikleyici örneği tetikler. Ancak [bu varsayılan sınırı değiştirmek](#change-trigger-concurrency), iş akışı örneklerinin yeni eşzamanlılık sınırına ulaştığında herhangi bir yeni örnekleri çalıştırmak için beklemeniz gerekir. 

Bekleyebileceği çalıştırmalarının sayısı da sahip bir [varsayılan sınır](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits), değiştirebilirsiniz. Ancak, mantıksal uygulamanızı bekleme çalıştırmaları sınırına ulaştıktan sonra Logic Apps altyapısı yeni çalışmaları artık kabul eder. İstek ve Web kancası Tetikleyicileri 429 hataları döndürür ve yoklama denemeleri atlanıyor yinelenen Tetikleyiciler başlatın.

Bekleyen çalıştırmaları varsayılan sınırı değiştirmek için temel tetikleyicisi tanımı, ekleme `runtimeConfiguration.concurency.maximumWaitingRuns` özelliği arasında bir değer ile `0` ve `100`. 

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "maximumWaitingRuns": 50
      }
   }
}
```

<a name="sequential-trigger"></a>

### <a name="trigger-instances-sequentially"></a>Örnek ardışık olarak tetikleyin

Her mantıksal çalıştırmak için önceki örnek yalnızca tamamlandıktan sonra uygulama iş akışı örneği çalıştırmak, sıralı olarak çalıştırmak için tetikleyiciyi ayarlayın. Ayrıca ekler Tasarımcısı aracılığıyla Eş zamanlılık ayarı değiştirerek veya güncelleştirmeleri çünkü kod görünümü Düzenleyicisi veya Logic Apps Tasarımcısı'nda kullanabilirsiniz `runtimeConfiguration.concurrency.runs` özelliği temel tetikleyici tanımı ve bunun tersi de geçerlidir. 

> [!NOTE] 
> Sıralı olarak kullanarak ya da Tasarımcı veya kod görünümü Düzenleyicisi çalıştırılacak bir tetikleyici ayarladığınızda, tetikleyicinin ayarlamamanız `operationOptions` özelliğini `Sequential` kod görünümü düzenleyicisinde. Aksi takdirde, bir doğrulama hatası alırsınız. 

#### <a name="edit-in-code-view"></a>Kod Görünümü'nde Düzenle

Tetikleyici tanımında ya da bu özellikler, ancak ikisini birden ayarlayın. 

Ayarlama `runtimeConfiguration.concurrency.runs` özelliğini `1`:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": 1
      }
   }
}
```

*- veya -*

Ayarlama `operationOptions` özelliğini `SingleInstance`:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "operationOptions": "SingleInstance"
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps Tasarımcısı'nda Düzenle

1. Tetikleyicinin sağ üst köşedeki üç nokta (...) düğmesini seçin ve ardından **ayarları**.

2. Altında **eşzamanlılık denetimi**ayarlayın **sınırı** için **üzerinde**. 

3. Sürükleme **paralellik derecesi** sayı kaydırıcısını `1`. 

<a name="sequential-for-each"></a>

### <a name="run-for-each-loops-sequentially"></a>"For each" çalıştırma sıralı döngü

Bir "for each" döngüsü çalıştırmak için yalnızca önceki yinelemede bittikten sonra yineleme çalıştıran, sıralı olarak çalıştırmak için "for each" eylem ayarlayın. Ayrıca ekler eylemin eşzamanlılık Tasarımcısı aracılığıyla değiştirme veya güncelleştirmeleri çünkü kod görünümü Düzenleyicisi veya Logic Apps Tasarımcısı'nda kullanabilirsiniz `runtimeConfiguration.concurrency.repetitions` özelliği, temel alınan işlem tanımı ve bunun tersi de geçerlidir. 

> [!NOTE] 
> Sıralı olarak ya da bir tasarımcı veya kod görünümü düzenleyici kullanarak çalıştırmak için bir "for each" eylem ayarladığınızda, eylemin ayarlamamanız `operationOptions` özelliğini `Sequential` kod görünümü düzenleyicisinde. Aksi takdirde, bir doğrulama hatası alırsınız. 

#### <a name="edit-in-code-view"></a>Kod Görünümü'nde Düzenle

Eylem tanımında ya da bu özellikler, ancak ikisini birden ayarlayın. 

Ayarlama `runtimeConfiguration.concurrency.repetitions` özelliğini `1`:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": 1
      }
   }
}
```

*- veya -*

Ayarlama `operationOptions` özelliğini `Sequential`:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "operationOptions": "Sequential"
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps Tasarımcısı'nda Düzenle

1. İçinde **her** eylemin sağ üst köşesindeki üç nokta (...) düğmesini seçin ve ardından **ayarları**.

2. Altında **eşzamanlılık denetimi**ayarlayın **eşzamanlılık denetimi** için **üzerinde**. 

3. Sürükleme **paralellik derecesi** sayı kaydırıcısını `1`. 

<a name="asynchronous-patterns"></a>

### <a name="run-actions-synchronously"></a>Eylemler zaman uyumlu olarak çalışır

Varsayılan olarak, tüm HTTP tabanlı eylemleri standart zaman uyumsuz işlem yapıdadır. Bu düzen, bir HTTP tabanlı eylem belirtilen uç noktaya bir istek gönderdiğinde, uzak sunucu bir "202 kabul edildi" yanıtı geri gönderir belirtir. Bu yanıt, sunucu istek işleme için kabul anlamına gelir. Logic Apps altyapısı, herhangi bir 202 yanıt olduğu yanıtın location üst bilgisi işlemenin durması kadar tarafından belirtilen URL denetlediği.

Ancak, bir zaman aşımı taleplere sahip sınırlamak için uzun süre çalışan işlemleri için zaman uyumsuz davranış ekleme ve ayarı devre dışı bırakabilirsiniz `operationOptions` özelliğini `DisableAsyncPattern` eylem girişleri altında.
  
```json
"<some-long-running-action>": {
   "type": "Http",
   "inputs": { "<action-inputs>" },
   "operationOptions": "DisableAsyncPattern",
   "runAfter": {}
}
```

<a name="run-high-throughput-mode"></a>

### <a name="run-in-high-throughput-mode"></a>Yüksek aktarım hızı modunda çalıştır

Tek bir mantıksal uygulama çalıştırmak için her 5 dakikada yürütülen eylem sayısı olan bir [varsayılan sınırı](../logic-apps/logic-apps-limits-and-config.md#throughput-limits). Bu sınırı artırmak için [maksimum](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) olası, belirlenen `operationOptions` özelliğini `OptimizedForHighThroughput`. Bu ayar, mantıksal uygulamanızı "yüksek aktarım hızı" moduna geçirir. 

> [!NOTE]
> Yüksek aktarım modu önizlemededir. Bir iş yükü, gerektiğinde birden fazla mantıksal uygulama üzerinden de dağıtabilirsiniz.

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { "<action-inputs>" },
   "operationOptions": "OptimizedForHighThroughput",
   "runAfter": {}
}
```

<a name="connector-authentication"></a>

## <a name="authenticate-http-triggers-and-actions"></a>HTTP Tetikleyicileri ve eylemleri kimlik doğrulaması

HTTP uç noktaları, farklı kimlik doğrulaması türlerini destekler. Kimlik doğrulama bu HTTP Tetikleyicileri ve eylemleri için ayarlayabilirsiniz:

* [HTTP](../connectors/connectors-native-http.md)
* [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)
* [HTTP Web kancası](../connectors/connectors-native-webhook.md)

Ayarlayabileceğiniz kimlik doğrulama türleri şunlardır:

* [Temel kimlik doğrulaması](#basic-authentication)
* [İstemci sertifikası kimlik doğrulaması](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth kimlik doğrulaması](#azure-active-directory-oauth-authentication)

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızı işleyen herhangi bir önemli bilgi koruma emin olun. Güvenli parametreleri kullanın ve gerektiğinde verileri kodlamak. Kullanma ve parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Temel kimlik doğrulaması

İçin [temel kimlik doğrulaması](../active-directory-b2c/active-directory-b2c-custom-rest-api-netfw-secure-basic.md) Azure Active Directory kullanarak, tetikleyici veya eylemi tanımınızı dahil edebilirsiniz bir `authentication` aşağıdaki tabloda belirtilen özellikleri içeren JSON nesnesi. Parametre değerleri çalışma zamanında erişmek için kullanabileceğiniz `@parameters('parameterName')` tarafından sağlanan ifadenin [iş akışı tanımlama dili](https://aka.ms/logicappsdocs). 

| Özellik | Gereklidir | Value | Açıklama | 
|----------|----------|-------|-------------| 
| **type** | Evet | "Temel" | Burada "Temel" olan kullanmak için kimlik doğrulaması türü | 
| **Kullanıcı adı** | Evet | "@parameters('userNameParam')" | Hedef hizmet uç noktası erişimi kimlik doğrulaması için kullanıcı adı |
| **Parola** | Evet | "@parameters('passwordParam')" | Hedef hizmet uç noktası erişimi kimlik doğrulaması için parola |
||||| 

Bu örnekte HTTP eylemi tanımı `authentication` bölümü belirtiyor `Basic` kimlik doğrulaması. Kullanma ve parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızı işleyen herhangi bir önemli bilgi koruma emin olun. Güvenli parametreleri kullanın ve gerektiğinde verileri kodlamak. Parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>İstemci sertifikası kimlik doğrulaması

İçin [sertifika tabanlı kimlik doğrulaması](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) Azure Active Directory'yi kullanarak, tetikleyici veya eylemi tanımınızı içerebilir bir `authentication` aşağıdaki tabloda belirtilen özellikleri içeren JSON nesnesi. Parametre değerleri çalışma zamanında erişmek için kullanabileceğiniz `@parameters('parameterName')` tarafından sağlanan ifadenin [iş akışı tanımlama dili](https://aka.ms/logicappsdocs). Kullanabileceğiniz istemci sertifikalarının sayısına yönelik sınırlar için bkz: [limitler ve yapılandırma için Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md).

| Özellik | Gereklidir | Value | Açıklama |
|----------|----------|-------|-------------|
| **type** | Evet | "ClientCertificate" | Güvenli Yuva Katmanı (SSL) istemci sertifikaları için kullanılacak kimlik doğrulaması türü. Otomatik olarak imzalanan sertifikalar desteklendiğinden, SSL için otomatik olarak imzalanan sertifikalar desteklenmiyor. |
| **PFX** | Evet | "@parameters('pfxParam') | Bir kişisel bilgi değişimi (PFX) dosyasından base64 ile kodlanmış içeriği |
| **Parola** | Evet | "@parameters('passwordParam')" | PFX dosyasına erişim için parola |
||||| 

Bu örnekte HTTP eylemi tanımı `authentication` bölümü belirtiyor `ClientCertificate` kimlik doğrulaması. Kullanma ve parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızı işleyen herhangi bir önemli bilgi koruma emin olun. Güvenli parametreleri kullanın ve gerektiğinde verileri kodlamak. Parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-ad-oauth-authentication"></a>Azure Active Directory (AD) OAuth kimlik doğrulaması

İçin [Azure AD OAuth kimlik doğrulaması](../active-directory/develop/authentication-scenarios.md), tetikleyici veya eylemi tanımınızı içerebilir bir `authentication` aşağıdaki tabloda belirtilen özellikleri içeren JSON nesnesi. Parametre değerleri çalışma zamanında erişmek için kullanabileceğiniz `@parameters('parameterName')` tarafından sağlanan ifadenin [iş akışı tanımlama dili](https://aka.ms/logicappsdocs).

| Özellik | Gereklidir | Value | Açıklama |
|----------|----------|-------|-------------|
| **type** | Evet | `ActiveDirectoryOAuth` | Azure AD OAuth "ActiveDirectoryOAuth" olan kullanmak için kimlik doğrulaması türü |
| **Yetkilisi** | Hayır | <*URL-için-yetkilisi-token-yayımcısı*> | Kimlik Doğrulama belirtecini sağlar yetkilisi URL'si |
| **Kiracı** | Evet | <*Kiracı kimliği*> | Azure AD kiracısı için Kiracı kimliği |
| **Hedef kitle** | Evet | <*yetki kaynağı*> | Yetkilendirme, örneğin kullanmak istediğiniz kaynak `https://management.core.windows.net/` |
| **clientId** | Evet | <*istemci kimliği*> | Yetkilendirmesi uygulama istemci kimliği |
| **credentialType** | Evet | "Sertifika" veya "Gizli" | İstemci kimlik bilgisi türü yetkilendirmesi için kullanır. Bu özellik ve değer temel Tanımınızda görünmez, ancak kimlik bilgisi türü için gerekli parametreler belirler. |
| **PFX** | Evet, yalnızca "Sertifika" kimlik bilgisi türü | "@parameters('pfxParam') | Bir kişisel bilgi değişimi (PFX) dosyasından base64 ile kodlanmış içeriği |
| **Parola** | Evet, yalnızca "Sertifika" kimlik bilgisi türü | "@parameters('passwordParam')" | PFX dosyasına erişim için parola |
| **Gizli anahtarı** | Evet, yalnızca "Gizli dizisini" kimlik bilgisi türü için | "@parameters('secretParam')" | Yetkilendirmesi için istemci gizli anahtarı |
|||||

Bu örnekte HTTP eylemi tanımı `authentication` bölümü belirtiyor `ActiveDirectoryOAuth` kimlik doğrulaması ve "Gizli" kimlik bilgisi türü. Kullanma ve parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
         "audience": "https://management.core.windows.net/",
         "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızı işleyen herhangi bir önemli bilgi koruma emin olun. Güvenli parametreleri kullanın ve gerektiğinde verileri kodlamak. Parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md)