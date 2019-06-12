---
title: Hata ve özel durum işleme - Azure Logic Apps | Microsoft Docs
description: Hata ve özel durum işleme Azure Logic Apps'te desenleri hakkında bilgi edinin
services: logic-apps
ms.service: logic-apps
author: dereklee
ms.author: deli
manager: jeconnoc
ms.date: 01/31/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 3f812c1142b5cd40169f7340163295b0f7ea6a4d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996613"
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Hataları ve Azure Logic apps'te özel durumları işleme

Bir challenge, kapalı kalma süresi veya bağımlı sistemleri tarafından neden olduğu sorunları tüm tümleştirme mimarisi uygun şekilde işleme biçimini oluşturabilir. Sorunlar ve hatalar işleyeceğinizi sağlam ve esnek tümleştirmeler oluşturmanıza yardımcı olmak için Logic Apps hataları ve özel durumları işlemek için birinci sınıf bir deneyim sağlar. 

<a name="retry-policies"></a>

## <a name="retry-policies"></a>İlkeleri yeniden deneme

En temel özel durum ve hata işleme için kullanabileceğiniz bir *yeniden deneme ilkesi* herhangi bir eylemde veya desteklenen durumlarda tetikleyin. Bir yeniden deneme ilkesi görüntülenip görüntülenmeyeceğini ve nasıl eylem veya tetikleyici özgün istek zaman aşımına uğradığında bir istek veya başarısız olan bir 408, 429 ve 5xx yanıt sonuçlanan tüm istekleri yeniden deneme belirtir. Diğer bir yeniden deneme ilkesi kullanılırsa, varsayılan İlkesi kullanılır. 

Yeniden deneme ilkesi türleri şunlardır: 

| Tür | Açıklama | 
|------|-------------| 
| **Varsayılan** | Bu ilke, en fazla dört yeniden denemeleri gönderir *katlanarak artan* ölçeklendirme 7.5 saniye ancak 5-45 saniye arasında kapsanacaksınız aralıkları. | 
| **Üstel aralık**  | Bu ilke, sonraki isteği göndermeden önce bir katlanarak artan aralığında seçilen rastgele bir aralığını bekler. | 
| **Sabit aralık**  | Bu ilke, sonraki isteği göndermeden önce belirtilen zaman aralığı bekler. | 
| **Yok.**  | İsteği yeniden gönderin yok. | 
||| 

Yeniden deneme ilkesi sınırları hakkında daha fazla bilgi için bkz. [Logic Apps limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#request-limits). 

### <a name="change-retry-policy"></a>Değişiklik yeniden deneme ilkesi

Farklı yeniden deneme ilkesi seçmek için aşağıdaki adımları izleyin: 

1. Mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın. 

2. Açık **ayarları** bir eylem veya tetikleyici için.

3. Eylem veya tetikleyici destekler, altında yeniden deneme ilkelerine, **yeniden deneme ilkesi**, istediğiniz türü seçin. 

Veya el ile yeniden deneme ilkesinde belirtebileceğiniz `inputs` bölüm için bir eylem veya tetikleyici destekleyen ilkeleri yeniden deneyin. Bir yeniden deneme ilkesi belirtmezseniz, eylem varsayılan ilkeyi kullanır.

```json
"<action-name>": {
   "type": "<action-type>", 
   "inputs": {
      "<action-specific-inputs>",
      "retryPolicy": {
         "type": "<retry-policy-type>",
         "interval": "<retry-interval>",
         "count": <retry-attempts>,
         "minimumInterval": "<minimum-interval>",
         "maximumInterval": "<maximun-interval>"
      },
      "<other-action-specific-inputs>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*retry-policy-type*> | String | Kullanmak istediğiniz yeniden deneme ilkesi türü: `default`, `none`, `fixed`, veya `exponential` | 
| <*retry-interval*> | String | Yeniden deneme aralığı değeri burada kullanmalıdır [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Varsayılan en düşük aralık `PT5S` ve en büyük aralık `PT1D`. Üstel aralık İlkesi kullandığınızda, farklı minimum ve maksimum değerleri belirtebilirsiniz. | 
| <*retry-attempts*> | Integer | 1 ile 90 arasında olmalıdır, yeniden deneme sayısı | 
||||

*İsteğe bağlı*

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*minimum-interval*> | String | Üstel aralık İlkesi, rastgele Seçilen aralıktaki en küçük aralığını [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) | 
| <*maximum-interval*> | String | Üstel aralık İlkesi, rastgele Seçilen aralıktaki en büyük aralığını [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) | 
|||| 

Farklı ilke türleri hakkında daha fazla bilgi aşağıda verilmiştir.

<a name="default-retry"></a>

### <a name="default"></a>Varsayılan

Bir yeniden deneme ilkesi belirtmezseniz varsayılan ilkesi eylemini kullanan aslında bir [üstel aralık ilke](#exponential-interval) 7.5 saniye ölçeklenir aralıkları katlanarak artan konumunda en fazla dört deneme gönderir. Aralığı 5-45 saniye arasında tavan sabitlenir. 

Eylem veya tetikleyici içinde açıkça tanımlanmış ancak varsayılan ilke örneği HTTP eylemi nasıl davranacağını şöyledir:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "http://myAPIendpoint/api/action",
      "retryPolicy" : {
         "type": "exponential",
         "interval": "PT7S",
         "count": 4,
         "minimumInterval": "PT5S",
         "maximumInterval": "PT1H"
      }
   },
   "runAfter": {}
}
```

### <a name="none"></a>None

Eylem veya tetikleyici başarısız istekleri yeniden deneme olmayan belirtmek için ayarlayın <*yeniden deneme ilkesi türü*> için `none`.

### <a name="fixed-interval"></a>Sabit aralık

Eylem veya tetikleyici belirtilen zaman aralığı sonraki isteği göndermeden önce bekleyeceğini belirtmek için ayarlayın <*yeniden deneme ilkesi türü*> için `fixed`.

*Örnek*

Bu yeniden deneme ilkesi, ilk başarısız istek bir 30 saniyelik gecikme her girişimden sonra birden fazla kez en son haberleri iki almaya çalışır:

```json
"Get_latest_news": {
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

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Üstel aralık

Eylem veya tetikleyici bir rastgele aralık sonraki isteği göndermeden önce bekleyeceğini belirtmek için ayarlayın <*yeniden deneme ilkesi türü*> için `exponential`. Rastgele aralık katlanarak artan bir aralıktan seçilir. İsteğe bağlı olarak ayrıca varsayılan minimum ve maksimum aralıkları kendi minimum ve maksimum aralıkları belirterek geçersiz kılabilirsiniz.

**Rasgele değişken aralıkları**

Bu tablo nasıl her yeniden deneme ve yeniden deneme sayısı için belirtilen aralıktaki bir Tekdüzen rastgele değişken Logic Apps oluşturur gösterir:

| Yeniden deneme sayısı | En düşük aralık | En uzun aralık |
|--------------|------------------|------------------|
| 1 | max (0 <*en düşük aralık*>) | Min (aralık <*en büyük aralık*>) |
| 2 | max (aralık <*en düşük aralık*>) | Min (2 * aralığı <*en büyük aralık*>) |
| 3 | max (2 * aralığı <*en düşük aralık*>) | Min (4 * aralığı <*en büyük aralık*>) |
| 4 | en fazla (4 * aralığı <*en düşük aralık*>) | Min (8 * aralığı <*en büyük aralık*>) |
| .... | .... | .... | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Catch ve RunAfter özelliğiyle hataları işleme

Her mantıksal uygulama eylemi, o eylemi başlatır, adımlarının sırasını, iş akışınızı nasıl belirttiğiniz benzer önce bitmesi gereken eylemleri bildirir. Bir eylem tanımındaki **runAfter** özelliği bu sıralama tanımlar ve hangi eylemleri ve eylem durumları eylemi Çalıştır açıklayan bir nesnedir.

Varsayılan olarak, mantıksal Uygulama Tasarımcısı'nda eklediğiniz tüm eylemleri önceki adımının sonucu olduğunda önceki adımdan sonra çalışacak biçimde ayarlanmış **başarılı**. Ancak, özelleştirebileceğiniz **runAfter** eylemleri olarak önceki eylemlerin neden olduğunda harekete böylece değer **başarısız**, **atlandı**, veya bu değerleri birleşimi. Örneğin, belirli bir Service Bus konu başlığına sonra belirli bir öğe eklemeye **Insert_Row** eylem başarısız, bu örnekte kullanabileceğinizi **runAfter** tanımı:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

**RunAfter** özelliği ayarlanmış olduğunda çalıştırılacak **Insert_Row** eylem durumu **başarısız**. Eylem durumu ise bir eylemi çalıştırmak için **başarılı**, **başarısız**, veya **atlandı**, bu sözdizimini kullanın:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Çalıştıran ve bir önceki eylemi başarısız oldu sonra başarıyla son eylemleri olarak işaretlenmiş **başarılı**. Başarıyla catch tüm hataları bir iş akışında, çalıştırma olarak işaretlenmişse, yani bu davranışı **başarılı**.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Kapsamlar ve sonuçları eylemlerle değerlendir

Benzer adımları tek tek eylemlerle sonra çalışan **runAfter** özelliği gruplandırabilirsiniz eylemleri birlikte içinde bir [kapsam](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). Mantıksal olarak eylemi gruplandırmanın, kapsamın toplam durumunu değerlendirmek ve bu duruma dayanarak eylemleri gerçekleştirmek istediğinizde, kapsamları kullanabilirsiniz. Kapsam, bir kapsamdaki tüm eylemleri çalıştırma işlemini tamamladıktan sonra kendi durumlarını alır. 

Bir kapsamın durumu denetlemek için aşağıdakiler gibi bir mantıksal uygulama çalıştırma durumunu denetlemek için kullandığınız aynı kriteri kullanabilirsiniz **başarılı**, **başarısız**ve benzeri. 

Kapsamın tüm eylemleri başarılı olduğunda bir kapsamın durumu varsayılan olarak işaretlenmiş **başarılı**. Bir kapsam içinde son eylem olarak sonuçlanırsa **başarısız** veya **Aborted**, kapsamın durumu işaretlenmiş **başarısız**. 

Özel durumları yakalamak için bir **başarısız** kapsamı ve bu hataları işlemek çalışma Eylemler, kullanabileceğiniz **runAfter** söz konusu özellik **başarısız** kapsam. Bu şekilde, *herhangi* kapsamındaki eylemleri başarısız ve kullandığınız **runAfter** özelliği, kapsamı için hataları yakalamak için tek bir eylem oluşturabilirsiniz.

Kapsamlar hakkında daha fazla limitleri için bkz [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Hataları için bağlam ve sonuçları alın

Bir kapsam hatalarını yakalama kullanışlı olsa da, tam olarak hangi eylemlerin herhangi bir hata veya döndürüldü durum kodları başarısız anlamanıza yardımcı olması için bağlam da isteyebilirsiniz. `@result()` İfade sonucu bir kapsam içindeki tüm eylemleri hakkında bir bağlam sağlar.

`@result()` İfade (kapsamın adı) tek bir parametre kabul eder ve tüm eylem sonuçlarına ilişkin bu kapsam içinde bir dizi döndürür. Bu eylem nesneler aynı özniteliklere içerir  **\@actions()** eylemin başlangıç zamanı, bitiş zamanı, durum, girişler, bağıntı kimlikleri ve çıktılar gibi bir nesne. Bir kapsam içinde başarısız olan herhangi bir eylem için bağlamı göndermek için kolayca eşleşebileceğini denetleyebilmesi bir  **\@result()** işleviyle bir **runAfter** özelliği.

Her eylem için eylem olan bir kapsam içinde çalıştırmak için bir **başarısız** sonucu, ve sonuçları aşağı başarısız Eylemler dizisi filtrelemek için eşleştirilebileceği  **\@result()** ile bir **[ Diziyi filtreleme](../connectors/connectors-native-query.md)** eylem ve [ **her** ](../logic-apps/logic-apps-control-flow-loops.md) döngü. Filtrelenmiş Sonuç dizisi alın ve her hata kullanmak için bir eylem gerçekleştirmek **her** döngü. 

"My_Scope" kapsamında yanıt gövdesi için başarısız olan tüm eylemler ile bir HTTP POST isteği gönderir ayrıntılı bir açıklama, ardından, bir örnek aşağıda verilmiştir:

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": "@result('My_Scope')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "My_Scope": [
         "Failed"
      ]
    }
},
"For_each": {
   "type": "foreach",
   "actions": {
      "Log_exception": {
         "type": "Http",
         "inputs": {
            "method": "POST",
            "body": "@item()['outputs']['body']",
            "headers": {
               "x-failed-action-name": "@item()['name']",
               "x-failed-tracking-id": "@item()['clientTrackingId']"
            },
            "uri": "http://requestb.in/"
         },
         "runAfter": {}
      }
   },
   "foreach": "@body('Filter_array')",
   "runAfter": {
      "Filter_array": [
         "Succeeded"
      ]
   }
}
```

Bu örnekte ne açıklayan ayrıntılı bilgileri aşağıda verilmiştir:

1. "My_Scope" içindeki tüm eylemler sonucu alma **filtre dizisi** eylemi bu filtre ifadesi kullanır: `@result('My_Scope')`

2. Koşul için **filtre dizisi** herhangi `@result()` Durum eşit olan öğe **başarısız**. Bu durum, tüm eylem sonuçlardan "My_Scope" bir dizi aşağı yalnızca başarısız olan eylem sonuçları içeren dizi filtreler.

3. Gerçekleştirmek bir **her** döngüye eylem *filtrelenmiş bir dizi* çıkarır. Bu adım, daha önce Filtre her başarısız olan eylem sonucu için bir eylem gerçekleştirir.

   Kapsamı tek bir eylemle başarısız olduysa, Eylemler **her** döngü yalnızca bir kez çalıştırın. 
   Birden çok başarısız Eylemler hatası başına tek bir eylemle neden olur.

4. Bir HTTP POST gönderme **her** öğesi olan yanıt gövdesi `@item()['outputs']['body']` ifade. 

   `@result()` Öğe şekli aynıdır `@actions()` şekillendirme ve aynı şekilde ayrıştırılır.

5. Başarısız olan eylem adlı iki özel üst bilgiler dahil (`@item()['name']`) ve istemci izleme kimliği çalıştırma başarısız (`@item()['clientTrackingId']`).

Başvuru için tek bir örneği aşağıda verilmiştir `@result()` gösteren öğesi **adı**, **gövdesi**, ve **clientTrackingId** önceki Ayrıştırılan özellikleri örnek. Dışında bir **her** eylem `@result()` bu nesneler dizisi döndürür.

```json
{
   "name": "Example_Action_That_Failed",
   "inputs": {
      "uri": "https://myfailedaction.azurewebsites.net",
      "method": "POST"
   },
   "outputs": {
      "statusCode": 404,
      "headers": {
         "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
         "Server": "Microsoft-IIS/8.0",
         "X-Powered-By": "ASP.NET",
         "Content-Length": "68",
         "Content-Type": "application/json"
      },
      "body": {
         "code": "ResourceNotFound",
         "message": "/docs/folder-name/resource-name does not exist"
      }
   },
   "startTime": "2016-08-11T03:18:19.7755341Z",
   "endTime": "2016-08-11T03:18:20.2598835Z",
   "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
   "clientTrackingId": "08587307213861835591296330354",
   "code": "NotFound",
   "status": "Failed"
}
```

Farklı özel durum desenleri işleme gerçekleştirmek için bu makalede daha önce açıklanan ifadeleri kullanabilirsiniz. Eylem tüm filtrelenmiş hata dizisini kabul eden kapsamı dışında işleme tek bir özel durum yürütmek seçin ve Kaldır **her** eylem. Diğer yararlı olan özellikleri de içerebilir  **\@result()** daha önce açıklandığı gibi yanıt.

## <a name="azure-diagnostics-and-metrics"></a>Azure tanılama ve ölçümler

Önceki desenleri şekilde hataları ve çalışması içinde özel durumları işlemek harika, ancak aynı zamanda tanımlamak ve yürütülmesi bağımsız hatalar için yanıt. 
[Azure tanılama](../logic-apps/logic-apps-monitor-your-logic-apps.md) bir Azure depolama hesabı veya bir olay hub'ı Azure Event Hubs ile oluşturulan tüm çalışma ve eylem durumlar da dahil olmak üzere tüm iş akışı olayları göndermek için basit bir yol sağlar. 

Çalıştırma durumları değerlendirmek için günlükleri ve ölçümleri izleyin veya tercih ettiğiniz izleme aracı yayımlarsınız. Olası bir seçenek olan tüm olayları aracılığıyla Event Hubs'a akışını [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). Stream Analytics'te her türlü anormallik, ortalamalar veya tanılama günlüklerine hatalardan temel alan dinamik sorgular yazabilirsiniz. Stream Analytics, kuyruklar, konular, SQL, Azure Cosmos DB veya Power BI gibi diğer veri kaynakları bilgilerini göndermek için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir müşteri hata işleme Azure Logic Apps ile nasıl derler bakın](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Daha fazla Logic Apps örnekleri ve senaryoları bulun](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
