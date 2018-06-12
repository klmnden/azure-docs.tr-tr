---
title: Hata ve özel durum Azure Logic Apps için işleme | Microsoft Docs
description: Desenler hata ve özel durum işleme Logic Apps içinde.
services: logic-apps
documentationcenter: ''
author: dereklee
manager: jeconnoc
editor: ''
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 01/31/2018
ms.author: deli; LADocs
ms.openlocfilehash: ee2c4f1408dcb6527220cd3870ab00d83987f471
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300071"
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Hataları ve Logic Apps içinde özel durumları işleme

Uygun şekilde işleme kesinti süresi veya bağımlı sistemleri sorunlarını herhangi tümleştirme mimarisi için bir sınama oluşturabilir. Sorunlar ve hatalar karşı dirençli sağlam tümleştirmeler oluşturmak için Logic Apps hataları ve özel durumları işlemek için birinci sınıf bir deneyim sunar. 

## <a name="retry-policies"></a>İlkeleri yeniden deneyin

En temel özel durumu ve hata işleme için yeniden deneme ilkesi kullanabilirsiniz. İlk istek zaman aşımına uğradı ya da başarısız olursa bir 429 veya 5xx yanıtı, bu ilkeyi sonuçlarında tanımlayan ve bunun nasıl eylem isteği yeniden deneme herhangi bir istek olduğu. 

Yeniden deneme ilkelerini dört tür vardır: varsayılan, aralığı ve üstel aralığı sabit yok. Varsayılan ilke hizmeti tarafından tanımlandığı şekilde, iş akışı tanımı bir yeniden deneme ilkesi yoksa, bunun yerine kullanılır.

Varsa, yeniden deneme ilkelerini ayarlamak için mantıksal uygulamanız için mantığı Uygulama Tasarımcısı'nı açın ve gidin **ayarları** mantıksal uygulamanızı belirli bir eylemi için. Ya da yeniden deneme ilkelerini tanımlayabilirsiniz **girişleri** özel bir eylem veya tetikleyici, yeniden denenebilir olması halinde, iş akışı tanımı için bölüm. Genel sözdizimi şöyledir:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Sözdizimi hakkında daha fazla bilgi için ve **girişleri** bölümünde, bkz: [iş akışı eylemleri ve Tetikleyicileri yeniden deneme ilkesi bölümüne][retryPolicyMSDN]. Yeniden deneme ilkesi kısıtlamaları hakkında daha fazla bilgi için bkz: [Logic Apps sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md). 

### <a name="default"></a>Varsayılan

Yok tanımladığınızda bir yeniden deneme ilkesi **retryPolicy** bölümünde, mantıksal uygulamanızı olan varsayılan ilke kullanan bir [üstel aralığı İlkesi](#exponential-interval) katlanarak en fazla dört yeniden deneme sırasında gönderir 7.5 saniyeye göre ölçeklenir aralıkları artırma. Aralığı 5 ile 45 saniye arasında tutulabilir. Bu ilke, bu örnekteki HTTP iş akışı tanımı ilke eşdeğerdir:

```json
"HTTP": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT1H"
        }
    },
    "runAfter": {}
}
```

### <a name="none"></a>None

Ayarlarsanız **retryPolicy** için **hiçbiri**, bu ilkeyi başarısız isteklerin yeniden denemez.

| Öğe adı | Gerekli | Tür | Açıklama | 
| ------------ | -------- | ---- | ----------- | 
| type | Evet | Dize | **Yok** | 
||||| 

### <a name="fixed-interval"></a>Sabit aralık

Ayarlarsanız **retryPolicy** için **sabit**, bu ilke, belirtilen zaman aralığı bir sonraki istek gönderilmeden önce süre bekleyerek başarısız bir istek yeniden dener.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | **Sabit** |
| sayı | Evet | Tamsayı | 1 ile 90 arasında olmalıdır yeniden deneme sayısı | 
| interval | Evet | Dize | Yeniden deneme aralığı [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), PT5S ve PT1D arasında olması gerekir | 
||||| 

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Üstel aralığı

Ayarlarsanız **retryPolicy** için **üstel**, bu ilke bir rastgele bir zaman aralığı katlanarak büyüyen bir aralıktan sonra başarısız bir istek yeniden dener. Yeniden deneme girişimlerinden değerinden daha büyük bir rastgele aralıkta göndermek ilke ayrıca garanti eder **minimumInterval** ve değerinden **maximumInterval**. Üstel ilkeleri gerektiren **sayısı** ve **aralığı**, while değerlerini **minimumInterval** ve **maximumInterval** isteğe bağlıdır. PT5S ve PT1D varsayılan değerleri sırasıyla geçersiz kılmak istiyorsanız, bu değerleri ekleyebilirsiniz.

| Öğe adı | Gerekli | Tür | Açıklama |
| ------------ | -------- | ---- | ----------- |
| type | Evet | Dize | **Üstel** |
| sayı | Evet | Tamsayı | 1 ile 90 arasında olmalıdır yeniden deneme sayısı  |
| interval | Evet | Dize | Yeniden deneme aralığı [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), PT5S ve PT1D arasında olması gerekir. |
| minimumInterval | Hayır | Dize | Yeniden deneme minimum aralığa [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), PT5S arasında olmalıdır ve **aralığı** |
| maximumInterval | Hayır | Dize | Yeniden deneme minimum aralığa [ISO 8601 biçim](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), arasında olması gerekir **aralığı** ve PT1D | 
||||| 

Bu tablo nasıl bir uniform rasgele değişken Belirtilen aralıktaki denemeler için dahil oluşturulacağını gösterir **sayısı**:

**Rasgele değişken aralığı**

| Yeniden deneme sayısı | Minimum aralık | En fazla aralığı |
| ------------ | ---------------- | ---------------- |
| 1 | Max (0, **minimumInterval**) | Min (aralığı **maximumInterval**) |
| 2 | Max (aralığı **minimumInterval**) | Min (2 * aralığı **maximumInterval**) |
| 3 | Max (2 * aralığı **minimumInterval**) | Min (4 * aralığı **maximumInterval**) |
| 4 | Max (4 * aralığı **minimumInterval**) | Min (8 * aralığı **maximumInterval**) |
| .... | | | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Catch ve RunAfter özelliğiyle hataları işleme

Her mantıksal uygulama eylem o eylemi başlatır, nasıl adımlarının sırasını iş akışınızda belirttiğiniz için benzer önce bitmesi gereken eylemleri bildirir. Bir eylem tanımında **runAfter** özelliği bu sıralama tanımlar ve hangi eylemleri ve eylem durumları yürütme eylemi açıklayan bir nesnedir.

Varsayılan olarak, önceki adım sonuç olduğunda önceki adımdan sonra çalışacak şekilde mantığı Uygulama Tasarımcısı'nda eklediğiniz tüm eylemler ayarlanır **başarılı**. Ancak, özelleştirebileceğiniz **runAfter** olarak önceki Eylemler neden olduğunda Eylemler yangın böylece değer **başarısız**, **Atlanan**, veya bu değerleri şunların bir birleşimi. Örneğin, belirli bir Service Bus konu başlığına sonra belirli bir öğe eklemek için **Insert_Row** eylemi başarısız oldu, bu örnek kullanabileceğinizi **runAfter** tanımı:

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

**RunAfter** özelliği ayarlanmış ne zaman çalıştırılacağını **Insert_Row** eylem durumu **başarısız**. Eylem durumu ise eylemi çalıştırmak için **başarılı**, **başarısız**, veya **Atlanan**, şu sözdizimini kullanın:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Çalıştıran ve bir önceki eylemi başarısız oldu sonra başarıyla son eylemler olarak işaretlenmiş **başarılı**. Bir iş akışını çalıştırma başarıyla catch tüm hatalar olarak işaretlenmişse, yani bu davranış **başarılı**.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Kapsamlar ve sonuçları eylemlerle değerlendir

Tek tek eylemlerle sonra adımları çalışan benzer **runAfter** özelliği gruplandırabilirsiniz Eylemler birlikte içinde bir [kapsam](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). Mantıksal olarak Eylemler gruplamak, kapsamın toplama durumunu değerlendirmek ve bu durum üzerinde temel eylemleri gerçekleştirme istediğinizde kapsamlarını kullanabilirsiniz. Çalıştırma kapsamı içindeki tüm eylemler tamamladıktan sonra kapsam kendi durumlarını alır. 

Bir kapsamın durumu denetlemek için bir mantık uygulamanın çalışma durumu gibi denetlemek için kullandığınız aynı ölçütleri kullanabilirsiniz **başarılı**, **başarısız**ve benzeri. 

Kapsamın tüm eylemler başarılı olduğunda, bir kapsamın durumu varsayılan olarak işaretlenmiş **başarılı**. Bir kapsamda son eylem olarak sonuçlanırsa **başarısız** veya **iptal edildi**, kapsamın durumu işaretlenmiş **başarısız**. 

Özel durumları yakalamak için bir **başarısız** kapsamı ve bu hataları işlemek çalışma Eylemler, kullanabileceğiniz **runAfter** söz konusu özellik **başarısız** kapsam. Bu şekilde, *herhangi* Eylemler kapsamında başarısız ve kullandığınız **runAfter** özelliği, kapsamı için hataları yakalamak için tek bir eylem oluşturabilirsiniz.

Kapsamlar üzerinde sınırları için bkz: [sınırları ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Hataları için içerik ve sonuçları alın

Bir kapsam hatalarını yakalama yararlı olsa da, herhangi bir hata veya döndürüldü durum kodları tam olarak hangi eylemleri başarısız anlamanıza yardımcı olması için bağlam da isteyebilirsiniz. **@result()** İş akışı işlevinin kapsamdaki tüm eylemlerin sonucu ile ilgili bağlam sağlar.

**@result()** İşlevi tek bir parametre (kapsamın adı) kabul eder ve tüm eylem sonuçlarını, kapsam içinde bir dizi döndürür. Bu eylem nesneleri aynı özniteliklere dahil  **@actions()** eylemin başlangıç zamanı, bitiş zamanı, durum, girişleri, bağıntı kimlikleri ve çıkışları gibi nesne. Bir kapsamda başarısız herhangi bir eylem için bağlamı göndermek için kolayca eşleştirilebileceği bir  **@result()** ile işlev bir **runAfter** özelliği.

Bir eylemi çalıştırmak için *her* eylem sahip kapsamdaki bir **başarısız** sonuç ve başarısız olan eylemler aşağıya doğru sonuçlar dizisi filtrelemek için eşleştirilebileceği  **@result()** ile bir **[filtre dizisi](../connectors/connectors-native-query.md)** eylem ve **[ForEach](../logic-apps/logic-apps-control-flow-loops.md)** döngü. Filtrelenmiş Sonuç dizisi alabilir ve her hata kullanmak için bir eylem gerçekleştirmek **ForEach** döngü. 

"My_Scope" kapsamında başarısız herhangi bir eylem yanıt gövdesi ile HTTP POST isteği gönderir ayrıntılı bir açıklama, ve ardından bir örnek aşağıda verilmiştir:

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

Bu örnekte ne olacağını açıklar ayrıntılı bir kılavuz şöyledir:

1. "My_Scope" içindeki tüm eylemlerin sonucu elde etmek için **filtre dizisi** eylem filtrelerini  **@result('My_Scope')**.

2. Koşul için **filtre dizisi** herhangi  **@result()** eşit bir duruma sahip öğe **başarısız**. Bu durum, tüm eylem sonuçlarla bir dizi aşağıya doğru "My_Scope" dan yalnızca başarısız eylem dizisi filtreler.

3. Gerçekleştirmek bir **her** döngü eylem *filtrelenmiş dizi* çıkarır. Bu adım bir eylem gerçekleştirir *her* önceden filtre eylem sonucu başarısız oldu.

   Kapsam içinde tek bir eylem başarısız olduysa, Eylemler **foreach** yalnızca bir kez çalıştırın. 
   Birden çok başarısız Eylemler hatası başına tek bir eylem neden olur.

4. Bir HTTP POST gönderme **foreach** öğesi olan yanıt gövdesi  **@item() ['outputs'] ['gövdesi']**. **@result()** Öğesi şekli aynı olup  **@actions()** şekil ve aynı şekilde ayrıştırılır.

5. Başarısız eylem adı ile iki özel üstbilgileri dahil  **@item() ['name']** çalıştırıp başarısız istemci izleme kimliği  **@item() ['clientTrackingId']**.

Başvuru için tek bir örneği burada verilmiştir  **@result()** öğesi, gösteren **adı**, **gövde**, ve **clientTrackingId** Önceki örnekte Ayrıştırılan özellikleri. Dışında bir **foreach** eylemi  **@result()** bu nesneleri içeren bir dizi döndürür.

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

Farklı özel durum desenleri işleme gerçekleştirmek için bu makalede daha önce açıklanan ifadeleri kullanabilirsiniz. Tek özel durum hataları tüm filtrelenmiş dizisi kabul kapsamı dışında eylem işleme yürütülecek seçin ve Kaldır **foreach** eylem. Diğer yararlı olan özellikleri de içerebilir  **@result()** daha önce açıklandığı gibi yanıt.

## <a name="azure-diagnostics-and-telemetry"></a>Azure tanılama ve telemetri

Önceki desenleri şekilde hataları ve çalışması içinde özel durumları işlemek harika, ancak aynı zamanda tanımlamak ve hataları yürütülmesi bağımsız yanıt. 
[Azure tanılama](../logic-apps/logic-apps-monitor-your-logic-apps.md) bir Azure Storage hesabı ya da Azure Event Hubs ile oluşturulan bir event hub tüm çalışma ve eylem durumlar da dahil olmak üzere tüm iş akışı olayları göndermek için basit bir yol sağlar. 

Çalışma durumlarını değerlendirmek için günlüklerini ve ölçümleri izleyin veya tercih ettiğiniz herhangi izleme aracına yayımlayın. Bir olası seçenektir Event Hubs'a aracılığıyla tüm olayları akışı [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/). Stream Analytics içinde herhangi bir anormallikleri, ortalama veya tanılama günlüklerine hatalarından temel alan dinamik sorgular yazabilirsiniz. Kuyruklar, konular, SQL, Azure Cosmos DB veya Power BI gibi diğer veri kaynakları bilgilerini göndermek için Stream Analytics kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir müşteri hata işleme Azure Logic Apps ile nasıl derler bakın](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Daha fazla Logic Apps örnekleri ve senaryoları Bul](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9