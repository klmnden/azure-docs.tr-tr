---
title: HTTP API'leri dayanıklı işlevlerinde - Azure
description: HTTP API'leri için Azure işlevleri dayanıklı işlevleri uzantısı'nda uygulama hakkında bilgi edinin.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 3c000e268c4c926991c3f1928f226065a436c6d2
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264894"
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>Dayanıklı işlevleri (Azure işlevleri) HTTP API'leri

Dayanıklı görev uzantısı bir dizi aşağıdaki görevleri gerçekleştirmek için kullanılan HTTP API sunar:

* Orchestration örneği durum getirin.
* Bir olay bir bekleme orchestration örneğine gönderin.
* Çalışan bir orchestration örneği sonlanır.


Her bu HTTP API'lerini doğrudan dayanıklı görev uzantısı tarafından işlenen bir Web kancası işlemdir. Bunlar herhangi bir işlev uygulaması işlevde özgü değildir.

> [!NOTE]
> Bu işlem ayrıca örnek Yönetimi API'lerini doğrudan kullanarak çağrılabilir [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. Daha fazla bilgi için bkz: [örnek Yönetimi](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>HTTP API URL bulma

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıf düzenlemenizi sağlayan bir [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) desteklenen tüm işlemler için bağlantıları içeren bir HTTP yanıt yükü oluşturmak için kullanılan bir API. Bu API kullanımı gösterilmiştir HTTP tetikleyicisi işlevi örnek aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

Bu örnek işlevi aşağıdaki JSON yanıt veriler üretir. Tüm alanlar veri türünde `string`.

| Alan             |Açıklama                           |
|-------------------|--------------------------------------|
| id                |Orchestration örnek kimliği. |
| statusQueryGetUri |Orchestration örnek durum URL'si. |
| sendEventPostUri  |Orchestration örneği "olursa olay" URL'si. |
| terminatePostUri  |Orchestration örneği "sonlandırmak" URL'si. |

Bir örnek yanıt şöyledir:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```
> [!NOTE]
> Web kancası URL'leri biçimi çalıştırdığınız Azure işlevleri ana bilgisayarın hangi sürümünün bağlı olarak değişebilir. Yukarıdaki örnek için Azure işlevleri 2.0 ana bilgisayardır.

## <a name="async-operation-tracking"></a>Zaman uyumsuz işlem izleme

Daha önce bahsedilen HTTP yanıtı uzun süre çalışan HTTP zaman uyumsuz dayanıklı işlevleri ile API'leri uygulama yardımcı olmak için tasarlanmıştır. Bu bazen olarak adlandırılır *yoklama tüketici düzeni*. İstemci/sunucu akış gibi çalışır:

1. İstemci bir orchestrator işlevi gibi uzun çalışan bir işlemi başlatmak için bir HTTP isteği gönderir.
2. Bir HTTP 202 yanıtı ile hedef HTTP tetikleyicisi döndürür bir `Location` üstbilgiyle `statusQueryGetUri` değeri.
3. İstemci URL'de yoklar `Location` üstbilgi. HTTP 202 yanıtları görmeye devam bir `Location` üstbilgi.
4. Örnek tamamlandığında (veya başarısız olduğunda), uç `Location` üstbilgi HTTP 200 döndürür.

Bu protokol dış istemcilere ya da bir HTTP uç noktası yoklama ve aşağıdaki Destek Hizmetleri ile uzun süre çalışan işlemi koordine sağlar `Location` üstbilgi. Temel Parçalar dayanıklı işlevleri HTTP API zaten oluşturulmuştur.

> [!NOTE]
> Varsayılan olarak, tüm HTTP tabanlı eylemler tarafından sağlanan [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) standart zaman uyumsuz işlem düzenini destekler. Bu özellik, uzun süre çalışan dayanıklı işlevi Logic Apps iş akışının bir parçası katıştırmak mümkün kılar. Zaman uyumsuz HTTP desenleri bulunabilir Logic Apps hakkında daha fazla ayrıntı desteği [Azure Logic Apps iş akışı eylemleri ve Tetikleyicileri belgelerine](../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>HTTP API Başvurusu

Tüm HTTP API'leri aşağıdaki parametreleri uzantısı Al tarafından uygulanır. Veri türü tüm parametrelerin `string`.

| Parametre  | Parametre türü  | Açıklama |
|------------|-----------------|-------------|
| örnek kimliği | URL'si             | Orchestration örnek kimliği. |
| taskHub    | Sorgu dizesi    | Adını [görev hub](durable-functions-task-hubs.md). Belirtilmezse, geçerli işlevi uygulamanın görev hub adı varsayılır. |
| bağlantı | Sorgu dizesi    | **Adı** depolama hesabı bağlantı dizesi. Belirtilmezse, işlev uygulaması için varsayılan bağlantı dizesini kabul edilir. |
| systemKey  | Sorgu dizesi    | Yetkilendirme anahtar API'sini çağırmak için gerekiyor. |
| showHistory| Sorgu dizesi    | İsteğe bağlı parametre. Varsa kümesine `true`, orchestration yürütme geçmişini yanıt yükünde dahil edilir.| 
| showHistoryOutput| Sorgu dizesi    | İsteğe bağlı parametre. Varsa kümesine `true`, etkinlik çıkarır dahil edilir orchestration yürütme geçmişi.| 

`systemKey` Azure işlevleri ana bilgisayar tarafından otomatik olarak oluşturulan bir yetkilendirme anahtardır. Özellikle dayanıklı görev uzantısı API'leri erişim verir ve aynı şekilde yönetilebilir [diğer yetkilendirme anahtarlar](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). Bulunacak en basit yolu `systemKey` değerdir kullanarak `CreateCheckStatusResponse` API daha önce bahsedilen.

Sonraki birkaç bölümlerde HTTP API'leri uzantısı tarafından desteklenen özel kapak ve bunların nasıl kullanılabileceğini örnekleri sağlayın.

### <a name="get-instance-status"></a>Örnek durumunu Al

Belirtilen orchestration örneği durumunu alır.

#### <a name="request"></a>İstek

İşlevler 1.0 için istek biçimi aşağıdaki gibidir:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçiminde hepsi aynı parametreleri ancak biraz farklı bir URL öneki içeriyor:

```http
GET /runtime/webhooks/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}&showHistory={showHistory}&showHistoryOutput={showHistoryOutput}
```

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 200 (Tamam)**: Belirtilen örnek tamamlanmış bir durumda.
* **HTTP 202 (kabul edilen)**: belirtilen örneği devam ediyor.
* **HTTP 400 (Hatalı istek)**: Belirtilen örnek başarısız oldu veya sonlandırıldı.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek yok ya da çalışan başlatılmadı.

İçin yanıt yükü **HTTP 200** ve **HTTP 202** durumda aşağıdaki alanları olan bir JSON nesnesi:

| Alan           | Veri türü | Açıklama |
|-----------------|-----------|-------------|
| runtimeStatus   | dize    | Çalışma zamanı durumu örneği. Değerler *çalıştıran*, *bekleyen*, *başarısız*, *iptal edildi*, *kesildi*, *Tamamlandı*. |
| input           | JSON      | Örneği başlatmak için kullanılan JSON verileri. |
| customStatus    | JSON      | Özel orchestration durumunun kullanılan JSON verileri. Bu alan `null` değilse ayarlayın. |
| çıkış          | JSON      | JSON çıktı örneği. Bu alan `null` örneği tamamlanmış durumda değilse. |
| createdTime     | dize    | Örneğin oluşturulduğu saat. ISO 8601 gösterimi genişletilmiş kullanır. |
| lastUpdatedTime | dize    | En son örnek kalıcı süre. ISO 8601 gösterimi genişletilmiş kullanır. |
| historyEvents   | JSON      | Orchestration yürütme geçmişini içeren bir JSON dizisi. Bu alan `null` sürece `showHistory` sorgu dizesi parametresi olarak ayarlanmış `true`.  | 

Orchestration yürütme geçmişi ve etkinlik çıkışları (okunabilirlik için biçimlendirilmiş) dahil olmak üzere bir örnek yanıt yükü şöyledir:

```json
{
  "createdTime": "2018-02-28T05:18:49Z",
  "historyEvents": [
      {
          "EventType": "ExecutionStarted",
          "FunctionName": "E1_HelloSequence",
          "Timestamp": "2018-02-28T05:18:49.3452372Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Tokyo!",
          "ScheduledTime": "2018-02-28T05:18:51.3939873Z",
          "Timestamp": "2018-02-28T05:18:52.2895622Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Seattle!",
          "ScheduledTime": "2018-02-28T05:18:52.8755705Z",
          "Timestamp": "2018-02-28T05:18:53.1765771Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello London!",
          "ScheduledTime": "2018-02-28T05:18:53.5170791Z",
          "Timestamp": "2018-02-28T05:18:53.891081Z"
      },
      {
          "EventType": "ExecutionCompleted",
          "OrchestrationStatus": "Completed",
          "Result": [
              "Hello Tokyo!",
              "Hello Seattle!",
              "Hello London!"
          ],
          "Timestamp": "2018-02-28T05:18:54.3660895Z"
      }
  ],
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "lastUpdatedTime": "2018-02-28T05:18:54Z",
  "output": [
      "Hello Tokyo!",
      "Hello Seattle!",
      "Hello London!"
  ],
  "runtimeStatus": "Completed"
}
```

**HTTP 202** yanıt de içeren bir **konumu** aynı URL'ye başvuran yanıt üstbilgisi `statusQueryGetUri` alan daha önce bahsedilen.

### <a name="get-all-instances-status"></a>Tüm örneklerinin durumunu Al

Ayrıca tüm örneklerinin durumunu sorgulayabilirsiniz. Kaldırma `instanceId` 'örneği durumunu Al' istek. Parametreler 'Get örneği durumu.' ile aynıdır 

#### <a name="request"></a>İstek

İşlevler 1.0 için istek biçimi aşağıdaki gibidir:

```http
GET /admin/extensions/DurableTaskExtension/instances/?taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçiminde hepsi aynı biraz farklı bir URL öneki ancak sahiptir: 

```http
GET /runtime/webhooks/DurableTaskExtension/instances/?taskHub={taskHub}&connection={connection}&code={systemKey}
```

#### <a name="response"></a>Yanıt

Yanıt yüklerini (okunabilirlik için biçimlendirilmiş) orchestration durumu da dahil olmak üzere bir örneği burada verilmiştir:

```json
[
    {
        "instanceId": "7af46ff000564c65aafbfe99d07c32a5",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:39Z",
        "lastUpdatedTime": "2018-06-04T10:46:47Z"
    },
    {
        "instanceId": "80eb7dd5c22f4eeba9f42b062794321e",
        "runtimeStatus": "Running",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:28Z",
        "lastUpdatedTime": "2018-06-04T15:18:38Z"
    },
    {
        "instanceId": "9124518926db408ab8dfe84822aba2b1",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:54Z",
        "lastUpdatedTime": "2018-06-04T10:47:03Z"
    },
    {
        "instanceId": "d100b90b903c4009ba1a90868331b11b",
        "runtimeStatus": "Pending",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:39Z",
        "lastUpdatedTime": "2018-06-04T15:18:39Z"
    }
]
```

> [!NOTE]
> Çok sayıda örnekleri tablosundaki satırları varsa bu işlemi Azure depolama g/ç açısından çok pahalı olabilir. Örnek tablo hakkında daha fazla ayrıntı bulunabilir [performansı ve ölçeği dayanıklı işlevlerinde (Azure işlevleri)](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-perf-and-scale#instances-table) belgeleri.
> 

### <a name="raise-event"></a>Olayı

Çalışan bir orchestration örneği için bir olay bildirim iletisi gönderir.

#### <a name="request"></a>İstek

İşlevler 1.0 için istek biçimi aşağıdaki gibidir:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçiminde hepsi aynı parametreleri ancak biraz farklı bir URL öneki içeriyor:

```http
POST /runtime/webhooks/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Bu API için parametreleri aşağıdaki benzersiz parametreleri yanı sıra daha önce bahsedilen varsayılan kümesi dahil isteyin:

| Alan       | Parametre türü  | Veri tType | Açıklama |
|-------------|-----------------|-----------|-------------|
| EventName   | URL'si             | dize    | Hedef orchestration örneği bekleniyor olayın adı. |
| {İçerik}   | İstek içeriği | JSON      | JSON biçimli olay yükü. |

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 202 (kabul edilen)**: yükseltilmiş olay işleme için kabul edildi.
* **HTTP 400 (Hatalı istek)**: İstek içeriği türü değildi `application/json` veya geçerli bir JSON değildi.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP 410 (Gone)**: Belirtilen örnek tamamlandı veya başarısız oldu ve yükseltilmiş olaylara işleyemiyor.

JSON dizesi gönderir bir örnek isteği işte `"incr"` adlı bir olay bekleniyor örneğine **işlemi**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Bu API için yanıtlar herhangi bir içerik içermez.

### <a name="terminate-instance"></a>Örnek Sonlandır

Çalışan bir orchestration örneği sonlandırır.

#### <a name="request"></a>İstek

İşlevler 1.0 için istek biçimi aşağıdaki gibidir:

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçiminde hepsi aynı parametreleri ancak biraz farklı bir URL öneki içeriyor:

```http
DELETE /runtime/webhooks/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

Bu API için parametreleri aşağıdaki benzersiz parametresini yanı sıra daha önce bahsedilen varsayılan kümesi dahil isteyin.

| Alan       | Parametre türü  | Veri Türü | Açıklama |
|-------------|-----------------|-----------|-------------|
| reason      | Sorgu dizesi    | dize    | İsteğe bağlı. Orchestration örneği sonlandırılıyor nedeni. |

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 202 (kabul edilen)**: sonlandırma isteği işleme için kabul edildi.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP 410 (Gone)**: Belirtilen örnek tamamlandı veya başarısız oldu.

Çalışan bir örneği sonlandırır ve bir nedenini belirten bir örnek isteği işte **buggy**:

```
DELETE /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API için yanıtlar herhangi bir içerik içermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hataların nasıl ele alınacağını öğrenin](durable-functions-error-handling.md)
