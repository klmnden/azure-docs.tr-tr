---
title: "Hata ve özel durum Azure Logic Apps için işleme | Microsoft Docs"
description: "Desenler hata ve özel durum işleme Logic Apps içinde."
services: logic-apps
documentationcenter: .net,nodejs,java
author: derek1ee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a74c7d18306359c9152f139299de1208b5932fe5
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Hataları ve Logic Apps içinde özel durumları işleme

Logic Apps Azure zengin araçları ve modeller, tümleştirmeler sağlam ve hatalarına karşı dayanıklı olduğundan emin olmanıza yardımcı olması için sağlar. Tüm tümleştirme mimarisi uygun şekilde kapalı kalma süresi işleme zorluk oluşturur veya bağımlı sistemlerden verir. Logic Apps işleme hataları birinci sınıf bir deneyim sağlar. Bu, iş akışlarınızı hataları ve özel durumları hareket için ihtiyacınız olan araçları sağlar.

## <a name="retry-policies"></a>İlkeleri yeniden deneyin

Bir yeniden deneme ilkesi özel durumu ve hata işleme en temel türüdür. İlk istek zaman aşımına uğrar veya başarısız olursa (bir 429 sonuçları herhangi bir istek veya 5xx yanıtı), bir yeniden deneme ilkesi tanımlar ve nasıl eylem yeniden. 

Yeniden deneme ilkelerini dört tür vardır: varsayılan, aralığı ve üstel aralığı sabit yok. Bir yeniden deneme ilkesi iş akışı tanımı'nda sağlanmazsa, hizmet tarafından tanımlanan varsayılan ilke kullanılır. 

Yeniden deneme ilkelerini yapılandırabilirsiniz *girişleri* belirli bir eylem veya yeniden denenebilir ise tetikleyici için. Benzer şekilde, yeniden deneme ilkelerini (varsa) mantığı Uygulama Tasarımcısı'nda yapılandırabilirsiniz. Mantıksal Uygulama Tasarımcısı'nda bir yeniden deneme İlkesi ayarlamak için Git **ayarları** belirli bir eylem için.

Yeniden deneme ilkelerini sınırlamaları hakkında daha fazla bilgi için bkz: [Logic Apps sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md). Desteklenen sözdizimi hakkında daha fazla bilgi için bkz: [iş akışı eylemleri ve Tetikleyicileri İlkesi bölümüne yeniden deneme][retryPolicyMSDN].

### <a name="default"></a>Varsayılan

Bir yeniden deneme ilkesi tanımlarsanız yok (**retryPolicy** tanımlı değil), varsayılan ilke kullanılır. Varsayılan ilkeyi katlanarak aralıkları 7.5 saniye ölçeği artırma sırasında en fazla dört deneme gönderen bir üstel aralığı ilkesidir. Aralık, 5-45 saniye arasında tutulabilir. Bu varsayılan ilke, bu örnekteki HTTP iş akışı tanımı ilke eşdeğerdir:

```json
"HTTP":
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7.5S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT45S"
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

### <a name="none"></a>Hiçbiri

Varsa **retryPolicy** ayarlanır **hiçbiri**, başarısız bir istek değil denenir.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | **yok** |

### <a name="fixed-interval"></a>Sabit aralık

Varsa **retryPolicy** ayarlanır **sabit**, ilke, belirtilen zaman aralığı bir sonraki istek gönderilmeden önce süre bekleyerek başarısız bir istek yeniden dener.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | **Sabit** |
| sayı | Evet | Tamsayı | Yeniden deneme sayısı. 1 ile 90 arasında olmalıdır. |
| interval | Evet | Dize | Yeniden deneme aralığı içinde [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). PT5S ve PT1D arasında olmalıdır. |

### <a name="exponential-interval"></a>Üstel aralığı

Varsa **retryPolicy** ayarlanır **üstel**, bir rastgele bir zaman aralığı katlanarak büyüyen bir aralıktan sonra başarısız bir istek ilkesi yeniden deneme sayısı. Yeniden deneme girişimlerinden değerinden daha büyük bir rastgele aralıkta gönderilmek üzere garanti **minimumInterval** ve değerinden **maximumInterval**. Aşağıdaki tabloda gösterilen aralığında bir uniform rasgele değişken denemeler için dahil oluşturulan **sayısı**:

**Rasgele değişken aralığı**

| Yeniden deneme sayısı | Minimum aralık | En fazla aralığı |
| ------------ |  ------------ |  ------------ |
| 1 | Max (0, **minimumInterval**) | Min (aralığı **maximumInterval**) |
| 2 | Max (aralığı **minimumInterval**) | Min (2 * aralığı **maximumInterval**) |
| 3 | Max (2 * aralığı **minimumInterval**) | Min (4 * aralığı **maximumInterval**) |
| 4 | Max (4 * aralığı **minimumInterval**) | Min (8 * aralığı **maximumInterval**) |
| ... |

Üstel türü ilkeleri için **sayısı** ve **aralığı** gereklidir. İçin değerler **minimumInterval** ve **maximumInterval** isteğe bağlıdır. Bunları sırasıyla PT5S ve PT1D, varsayılan değerleri geçersiz kılmak için ekleyebilirsiniz.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | **Üstel** |
| sayı | Evet | Tamsayı | Yeniden deneme sayısı. 1 ile 90 arasında olmalıdır.  |
| interval | Evet | Dize | Yeniden deneme aralığı içinde [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). PT5S ve PT1D arasında olmalıdır. |
| minimumInterval | Hayır | Dize | Minimum aralık içinde yeniden dene [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). PT5S arasında olmalıdır ve **aralığı**. |
| maximumInterval | Hayır | Dize | Minimum aralık içinde yeniden dene [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Arasında olmalıdır **aralığı** ve PT1D. |

## <a name="catch-failures-with-the-runafter-property"></a>RunAfter özelliğiyle hatalarını yakalama

Her mantıksal uygulama eylem hangi eylemleri eylem başlamadan önce bitmesi gereken bildirir. İş akışınızda adımları sıralama için benzer. Eylem tanımı'nda bu sıralama olarak bilinir **runAfter** özelliği. 

**RunAfter** hangi eylemleri ve eylem durumları yürütme eylemi açıklayan bir nesne bir özelliktir. Önceki adım sonuç ise varsayılan olarak, mantıksal uygulama Tasarımcısını kullanarak eklediğiniz tüm eylemler önceki adımdan sonra çalışacak biçimde ayarlanmış **başarılı**. 

Ancak, özelleştirebileceğiniz **runAfter** önceki eylem sonucunu olduğunda Eylemler tetiklenecek değeri **başarısız**, **Atlanan**, veya bu değerleri olası kümesi. Öğeyi belirlenen Azure Service Bus konu başlığına sonra belirli bir eylem eklemek istiyorsanız, **Insert_Row** başarısız olursa aşağıdakileri kullanabilirsiniz **runAfter** yapılandırma:

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

Unutmayın **runAfter** olursa tetiklenecek şekilde ayarlanmış **Insert_Row** eylem sonucu **başarısız**. Eylem durumu ise eylemi çalıştırmak için **başarılı**, **başarısız**, veya **Atlanan**, şu sözdizimini kullanın:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Çalıştıran ve bir önceki işlemin başarısız olursa başarıyla son eylemler olarak işaretlenmiş **başarılı**. Bunun anlamı, başarılı bir şekilde bir iş akışını çalıştırma catch tüm hatalar olarak işaretlenmişse **başarılı**.

## <a name="scopes-and-results-to-evaluate-actions"></a>Kapsamlar ve sonuçları Eylemler değerlendirmek için

Eylemler içinde gruplayabilirsiniz bir [kapsam](../logic-apps/logic-apps-loops-and-scopes.md), benzer şekilde sonra eylemi ayrı ayrı çalıştırın. Bir kapsam eylemlerin mantıksal bir gruplandırma yapar. 

Kapsam, mantıksal uygulama eylemlerinizi düzenlemek için hem bir kapsamın durumu toplama değerlendirmesini gerçekleştirmek için faydalıdır. Kapsamdaki tüm eylemler bitirdikten sonra kapsam bir durumunu alır. Kapsam durumu Çalıştır aynı ölçütüne ile belirlenir. Son eylem bir yürütme dal ise **başarısız** veya **iptal edildi**, durum **başarısız**.

Kapsam içinde oluşan hatalar için belirli eylemleri yangın için kullanabileceğiniz **runAfter** olarak işaretlenmiş bir kapsamla **başarısız**. Varsa *herhangi* Eylemler kapsamında başarısız kullanırsanız **runAfter** bir kapsamın hataları yakalamak için tek bir eylem oluşturabilirsiniz.

### <a name="get-the-context-of-failures-with-results"></a>Sonuçları hatalarıyla bağlamında Al

Bir kapsam hatalarını yakalama yararlı olsa da, bağlam tam olarak hangi eylemleri başarısız anlamanıza yardımcı olması ve herhangi bir hata veya döndürüldü durum kodları anlamak için de isteyebilirsiniz.  **@result()** İş akışı işlevinin kapsamdaki tüm eylemlerin sonucu ile ilgili bağlam sağlar.

 **@result()** İşlev tek bir parametre (kapsam adı) alır ve tüm eylem sonuçlarını, kapsam içinde bir dizi döndürür. Bu eylem nesneleri aynı özniteliklere dahil  **@actions()** nesne, eylem başlangıç saati, eylem bitiş zamanı, eylem durumu, eylemi girişleri, eylem bağıntı kimlikleri ve eylem dahil olmak üzere çıkarır. 

Bir kapsamda başarısız herhangi bir eylem bağlamı göndermek için kolayca eşleştirilebileceği bir  **@result()** ile işlev bir **runAfter** özelliği.

Bir eylem yürütmek için *her* eylemi olan bir kapsamda bir **başarısız** sonuç ve sonuçları başarısız eylemlerine dizisi filtrelemek için eşleştirilebileceği  **@result()** ile bir [Filter_array](../connectors/connectors-native-query.md) eylem ve [foreach](../logic-apps/logic-apps-loops-and-scopes.md) döngü. Filtrelenmiş sonuç dizisiyle bir eylem için her hatası kullanarak gerçekleştirebileceğiniz **foreach** döngü. 

Bir HTTP POST gönderir örneği isteği My_Scope kapsamında başarısız herhangi bir eylem yanıt gövdesi ile burada 's:

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Önceki örnekte ne olacağını açıklamak için ayrıntılı bir kılavuz şöyledir:

1. My_Scope, içindeki tüm eylemlerin sonucu elde etmek için **Filter_array** eylem filtrelerini  **@result('My_Scope')**.

2. Koşul için **Filter_array** herhangi  **@result()** eşit bir duruma sahip öğe **başarısız**. Bu durum My_Scope, tüm eylem sonuçlarını yalnızca başarısız eylem sonuçlarını içeren bir dizi diziden filtreler.

3. Gerçekleştirmek bir **foreach** eylemini *filtrelenmiş dizi* çıkarır. Bu adım bir eylem gerçekleştirir *her* önceden filtre eylem sonucu başarısız oldu.

    Kapsam içinde tek bir eylem başarısız olduysa, Eylemler **foreach** yalnızca bir kez çalıştırın. Birden çok başarısız Eylemler hatası başına tek bir eylem neden olur.

4. Bir HTTP POST gönderme **foreach** öğesi yanıt gövdesi veya  **@item() ['outputs'] ['gövdesi']**.  **@result()** Öğesi şekli aynı olup  **@actions()** şekli. Aynı şekilde ayrıştırılabilir.

5. Başarısız eylem adı ile iki özel üstbilgileri dahil  **@item() ['name']** çalıştırıp başarısız istemci izleme kimliği  **@item() ['clientTrackingId']**.

Başvuru için tek bir örneği burada verilmiştir  **@result()** öğesi. Bunu gösterir **adı**, **gövde**, ve **clientTrackingId** önceki örnekte Ayrıştırılan özellikleri. Dışında bir **foreach** eylemi  **@result()** bu nesneleri içeren bir dizi döndürür.

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

Desenler farklı özel durum işleme için makalenin önceki bölümlerinde açıklanan ifadeleri kullanabilirsiniz. Tek özel durum hataları tüm filtrelenmiş dizisi kabul kapsamı dışında eylem işleme yürütülecek seçin ve Kaldır **foreach**. Diğer yararlı olan özellikleri de içerebilir  **@result()** daha önce açıklandığı gibi yanıt.

## <a name="azure-diagnostics-and-telemetry"></a>Azure tanılama ve telemetri

Bu makalede açıklanan desenleri hataları ve bir çalışma içinde özel durumları işlemek için harika yollar sağlar, ancak aynı zamanda tanımlamak ve hataları yürütülmesi bağımsız yanıt. [Azure tanılama](../logic-apps/logic-apps-monitor-your-logic-apps.md) Azure storage hesabı veya bir event hub'ında Azure Event Hubs (tüm çalışma ve eylem durumları dahil) tüm iş akışı olayları göndermek için basit bir yol sağlar. 

Çalışma durumlarını değerlendirmek için günlüklerini ve ölçümleri izleyin veya tercih ettiğiniz izleme aracı yayımlayın. Event Hubs'a aracılığıyla tüm olayları akışını sağlamak için bir olası seçeneği olan [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/). Stream Analytics içinde herhangi bir anormallikleri, ortalama veya tanılama günlüklerine hatalarından temel alan dinamik sorgular yazabilirsiniz. Kuyruklar, konular, SQL, Azure Cosmos DB veya Power BI gibi diğer veri kaynakları için bilgi göndermek için Stream Analytics kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir müşteri nasıl bkz [hata Azure Logic Apps ile işleme derlemeler](../logic-apps/logic-apps-scenario-error-and-exception-handling.md).
* Daha fazla bilgi edinin [Logic Apps örnekleri ve senaryoları](../logic-apps/logic-apps-examples-and-scenarios.md).
* Oluşturmayı öğrenin [dağıtımları mantıksal uygulamalar için otomatik](../logic-apps/logic-apps-create-deploy-template.md).
* Bilgi edinmek için nasıl [derleme ve logic apps Visual Studio ile dağıtma](logic-apps-deploy-from-vs.md).

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
