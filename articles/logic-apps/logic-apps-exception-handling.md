---
title: "Hata & özel durum işleme - Azure Logic Apps | Microsoft Docs"
description: "Hata ve özel durum işleme Azure Logic Apps içinde desenleri"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 20f93d5dbcc91a633f040feb454e96ed3f7d561f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Hataları ve Azure Logic Apps içinde özel durumları işleme

Desenler, tümleştirmeler emin olun yardımcı olmak için güçlü ve hatalarına karşı dayanıklı ve Azure mantıksal uygulamaları zengin araçlar sağlar. Herhangi bir tümleştirme mimarideki kesinti süresi veya bağımlı sistemleri sorunlarını uygun şekilde işlemek üzere emin yapma zorluk oluşturur. Logic Apps, iş akışlarınızı hataları ve özel durumları hareket için ihtiyacınız olan araçları vermiş birinci sınıf bir deneyim hataları işleme yapar.

## <a name="retry-policies"></a>İlkeleri yeniden deneyin

Bir yeniden deneme ilkesi özel durumu ve hata işleme en temel türüdür. İlk istek zaman aşımına uğradı ya da başarısız olursa (bir 429 sonuçları herhangi bir istek veya 5xx yanıtı), bu ilkeyi tanımlar ve nasıl eylemi yeniden deneyin. Yeniden deneme ilkelerini üç tür vardır `exponential`, `fixed`, ve `none`. Bir yeniden deneme ilkesi iş akışı tanımı'nda sağlanmazsa, varsayılan ilke kullanılır. Yeniden deneme ilkelerini yapılandırabilirsiniz **girişleri** belirli bir eylem veya yeniden denenebilir ise tetikleyici için. Benzer şekilde, mantıksal Uygulama Tasarımcısı yeniden ilkeleri (varsa) altında yapılandırılabilir **ayarları** verilen bloğu.

Yeniden deneme ilkelerini sınırlamaları hakkında daha fazla bilgi için bkz: [Logic Apps sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md) ve desteklenen sözdizimi hakkında daha fazla bilgi için bkz: [iş akışı eylemleri ve Tetikleyicileriyenidendenemeilkesibölümüne][retryPolicyMSDN].

### <a name="exponential-interval"></a>Üstel aralığı
`exponential` İlke türü bir rastgele bir zaman aralığı katlanarak büyüyen bir aralıktan sonra başarısız bir istek deneyecek. Yeniden deneme girişimlerinden değerinden daha büyük bir rastgele aralıkta gönderilmek üzere garanti **minimumInterval** ve değerinden **maximumInterval**. Bir Tekdüzen rastgele değişkende dahil için her yeniden deneme aralığını altına oluşturulmayacak **sayısı**:
<table>
<tr><th> Rastgele değişkeni aralığı </th></tr>
<tr><td>

| Yeniden deneme sayısı | Minimum aralık | En fazla aralığı |
| ------------ |  ------------ |  ------------ |
| 1 | Max (0, **minimumInterval**) | Min (aralığı **maximumInterval**) |
| 2 | Max (aralığı **minimumInterval**) | Min (2 * aralığı **maximumInterval**) |
| 3 | Max (2 * aralığı **minimumInterval**) | Min (4 * aralığı **maximumInterval**) |
| 4 | Max (4 * aralığı **minimumInterval**) | Min (8 * aralığı **maximumInterval**) |
| ... |

</td></tr></table>

İçin `exponential` yazın ilkeleri **sayısı** ve **aralığı** sırasında gerekli olan **minimumInterval** ve **maximumInterval** olabilir varsayılan değerleri PT5S ve PT1D sırasıyla geçersiz kılmak için isteğe bağlı olarak sağlanan.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | `exponential` |
| sayı | Evet | Tamsayı | sayısı yeniden deneme sayısı, 1 ile 90 arasında olmalıdır  |
| interval | Evet | Dize | yeniden deneme aralığı içinde [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), PT5S ile PT1D arasında olmalıdır |
| minimumInterval | Hayır| Dize | Minimum aralık içinde yeniden dene [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), PT5S arasında olmalıdır ve **aralığı** |
| maximumInterval | Hayır| Dize | Minimum aralık içinde yeniden dene [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), arasında olmalıdır **aralığı** ve PT1D |

### <a name="fixed-interval"></a>Sabit aralık

`fixed` İlke türü, başarısız bir istek sağlanan sonraki istek göndermeden önce zaman aralığını bekleyerek deneyecek.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | `fixed`|
| sayı | Evet | Tamsayı | sayısı yeniden deneme sayısı, 1 ile 90 arasında olmalıdır |
| interval | Evet | Dize | yeniden deneme aralığı içinde [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), PT5S ile PT1D arasında olmalıdır |

### <a name="none"></a>None
`none` İlke türü, başarısız bir istek değil deneyecek.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | `none`|

### <a name="default"></a>Varsayılan
Yeniden deneme ilkesi belirtilmezse, varsayılan ilke kullanılır. Varsayılan ilke katlanarak 7.5 saniye ölçeği ve için 5-45 saniye arasında tutulabilir aralıkları artırma sırasında en fazla 4 deneme gönderen bir üstel aralığı ilkesidir. Bu varsayılan ilke (kullanılır **retryPolicy** tanımsızdır) HTTP iş akışı tanımı bu örnekteki ilke eşdeğerdir:

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

## <a name="catch-failures-with-the-runafter-property"></a>RunAfter özelliğiyle hatalarını yakalama

Her mantıksal uygulama eylem hangi eylemleri iş akışınızda adımları sıralama gibi eylem başlamadan önce bitmesi gereken bildirir. Eylem tanımı'nda bu sıralama olarak bilinir `runAfter` özelliği. Bu özellik, hangi eylemleri ve eylem durumları yürütme eylemi açıklayan bir nesnedir. Varsayılan olarak, mantıksal Uygulama Tasarımcısı eklenen tüm eylemleri ayarlamak `runAfter` önceki adımda, önceki adımda `Succeeded`. Bununla birlikte, bu değer, önceki eylem olduğunda Eylemler tetiklenecek özelleştirebilirsiniz `Failed`, `Skipped`, veya bu değerleri olası kümesi. Öğeyi sonra belirli bir eylemi atanmış bir Service Bus konu başlığına eklemek istiyorsanız `Insert_Row` başarısız olursa aşağıdakileri kullanabilirsiniz `runAfter` yapılandırma:

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

Bildirim `runAfter` özelliği ayarlanmış olursa tetiklenecek `Insert_Row` eylem `Failed`. Eylem durumu ise eylemi çalıştırmak için `Succeeded`, `Failed`, veya `Skipped`, şu sözdizimini kullanın:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Çalıştırın ve önceki bir eylemi başarısız oldu sonra başarıyla tamamlanmış eylemler olarak işaretlenmiş `Succeeded`. Bir iş akışını çalıştırma başarıyla catch tüm hatalar olarak işaretlenmişse, yani bu davranış `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Kapsamlar ve sonuçları Eylemler değerlendirmek için

Nasıl çalıştırabileceğiniz benzer ayrı Eylemler sonra aynı zamanda Eylemler içinde gruplayabilirsiniz bir [kapsam](../logic-apps/logic-apps-loops-and-scopes.md), Eylemler mantıksal bir gruplandırması olarak davranır. Kapsamları mantıksal uygulama eylemleri düzenlemek için hem bir kapsamın durumu toplama değerlendirmesini gerçekleştirmek için faydalıdır. Kapsamdaki tüm eylemler bitirdikten sonra kapsam bir durumunu alır. Kapsam durumu Çalıştır aynı ölçütüne ile belirlenir. Son eylem bir yürütme dal ise `Failed` veya `Aborted`, durum `Failed`.

Kapsam içinde gerçekleşen hatalar için belirli eylemleri yangın için kullanabileceğiniz `runAfter` olarak işaretlenmiş bir kapsamla `Failed`. Varsa *herhangi* Eylemler kapsamında başarısız, kapsam sağlar başarısız olduktan sonra çalışan hataları yakalamak için tek bir eylem oluşturun.

### <a name="getting-the-context-of-failures-with-results"></a>Sonuçları hatalarıyla bağlamı alma

Bir kapsam hatalarını yakalama yararlı olsa da, tam olarak başarısız oldu, hangi eylemleri ve hatalar veya döndürüldü durum kodları anlamanıza yardımcı olması için bağlamı da isteyebilirsiniz. `@result()` İş akışı işlevinin kapsamdaki tüm eylemlerin sonucu ile ilgili bağlam sağlar.

`@result()`tek bir parametre, kapsam adı alır ve tüm eylem sonuçlarını, kapsam içinde bir dizi döndürür. Bu eylem nesneleri aynı özniteliklere dahil `@actions()` nesne, eylem başlangıç saati, eylem bitiş zamanı, eylem durumu, eylemi girişleri, eylem bağıntı kimlikleri ve eylem dahil olmak üzere çıkarır. Bir kapsamda başarısız herhangi bir eylem bağlamı göndermek için kolayca eşleştirilebileceği bir `@result()` ile işlev bir `runAfter`.

Bir eylem yürütmek için *her* kapsamdaki eylem, `Failed`, başarısız olan eylemler için sonuçları dizisi filtre, eşleştirin `@result()` ile bir  **[filtre dizisi](../connectors/connectors-native-query.md)**  eylem ve  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  döngü. Filtrelenmiş Sonuç dizisi alabilir ve her hata kullanmak için bir eylem gerçekleştirmek **ForEach** döngü. Kapsam içinde başarısız oldu herhangi bir eylem yanıt gövdesi ile HTTP POST isteği gönderir ayrıntılı bir açıklama, ve ardından örneği `My_Scope`.

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

Ne olacağını açıklamak için ayrıntılı bir kılavuz şöyledir:

1. Tüm eylemlerin sonucu elde etmek için `My_Scope`, **filtre dizisi** eylem filtrelerini `@result('My_Scope')`.

2. Koşul için **filtre dizisi** herhangi `@result()` Durum eşit olan öğe `Failed`. Bu durum tüm eylem sonuçlarını diziyle filtreler `My_Scope` sahip bir dizi yalnızca eylem sonuçlarını başarısız oldu.

3. Gerçekleştirmek bir **her** eylemini **filtre dizi** çıkarır. Bu adım bir eylem gerçekleştirir *her* önceden filtre eylem sonucu başarısız oldu.

    Kapsam içinde tek bir eylem başarısız olduysa, Eylemler `foreach` yalnızca bir kez çalıştırın. 
    Çok sayıda başarısız Eylemler hatası başına tek bir eylem neden olur.

4. Bir HTTP POST gönderme `foreach` öğesi yanıt gövdesi veya `@item()['outputs']['body']`. `@result()` Öğesi şekli aynı olup `@actions()` şekil ve aynı şekilde ayrıştırılır.

5. Başarısız eylem adı ile iki özel üstbilgileri dahil `@item()['name']` çalıştırıp başarısız istemci izleme kimliği `@item()['clientTrackingId']`.

Başvuru için tek bir örneği burada verilmiştir `@result()` öğesi, gösteren `name`, `body`, ve `clientTrackingId` önceki örnekte Ayrıştırılan özellikleri. Dışında bir `foreach`, `@result()` bu nesneleri içeren bir dizi döndürür.

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

Farklı özel durum desenleri işleme gerçekleştirmek için daha önce gösterilen ifadeleri kullanabilirsiniz. Tek özel durum hataları tüm filtrelenmiş dizisi kabul kapsamı dışında eylem işleme yürütülecek seçin ve Kaldır `foreach`. Diğer yararlı olan özellikleri de içerebilir `@result()` daha önce gösterilen yanıt.

## <a name="azure-diagnostics-and-telemetry"></a>Azure tanılama ve telemetri

Önceki desenleri şekilde hataları ve çalışması içinde özel durumları işlemek harika, ancak aynı zamanda tanımlamak ve hataları yürütülmesi bağımsız yanıt. 
[Azure tanılama](../logic-apps/logic-apps-monitor-your-logic-apps.md) bir Azure Storage hesabı veya bir Azure olay hub'ı (tüm çalışma ve eylem durumları dahil) tüm iş akışı olayları göndermek için basit bir yol sağlar. Çalışma durumlarını değerlendirmek için günlükleri ve ölçümleri izleyin veya tercih ettiğiniz izleme aracı yayımlama. Olası bir seçenek olan Azure Event Hub'ına aracılığıyla tüm olayları akışını sağlamak için [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). Stream Analytics içinde herhangi bir anormallikleri, ortalama veya hataları kapalı dinamik sorgular tanılama günlükleri yazabilirsiniz. Akış analizi, kuyruklar, konular, SQL, Azure Cosmos DB ve Power BI gibi diğer veri kaynakları için kolayca çıkarabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

* [Bir müşteri hata işleme Azure Logic Apps ile nasıl derler bakın](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Daha fazla Logic Apps örnekleri ve senaryoları Bul](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Mantıksal uygulamalar için otomatik dağıtımları oluşturmayı öğrenin](../logic-apps/logic-apps-create-deploy-template.md)
* [Visual Studio ile mantıksal uygulamalar oluşturma ve dağıtma](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
