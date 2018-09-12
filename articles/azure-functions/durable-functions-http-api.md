---
title: Dayanıklı işlevler - Azure HTTP API'leri
description: HTTP API'leri, Azure işlevleri için dayanıklı işlevler uzantısını uygulamayı öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: azfuncdf
ms.openlocfilehash: 29fd4e62c13852e23e15f89ab6b4e2976fc42b25
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377149"
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) HTTP API'leri

Dayanıklı görev uzantısı kullanıma sunan bir dizi HTTP API'si, aşağıdaki görevleri gerçekleştirmek için kullanılabilir:

* Orchestration örneği durumu getirilemedi.
* Bir olay bir bekleyen düzenleme örneğine gönderin.
* Çalışan bir düzenleme örneği sonlandırın.


Her biri bu HTTP API'lerini doğrudan dayanıklı görev uzantısı tarafından işlenen bir Web kancası bir işlemdir. Bunlar herhangi bir işleve işlev uygulamasına özel değildir.

> [!NOTE]
> Bu işlemler üzerinde örnek Yönetimi API'lerini kullanarak doğrudan da çağrılabilir [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. Daha fazla bilgi için [örnek Yönetimi](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>HTTP API URL'si bulma

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı kullanıma sunan bir [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) desteklenen tüm işlemler için bağlantılar içeren bir HTTP yanıtı yükü oluşturmak için kullanılan API. Bu API kullanımı gösterilmiştir HTTP tetikleyici işlevi bir örnek aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

Bu örnek işlevi aşağıdaki JSON yanıt verilerini oluşturur. Tüm alanlar veri türü `string`.

| Alan             |Açıklama                           |
|-------------------|--------------------------------------|
| id                |Orchestration örneği kimliği. |
| statusQueryGetUri |Orchestration örneği durumu URL'si. |
| sendEventPostUri  |Orchestration örneği "raise olay" URL'si. |
| terminatePostUri  |Orchestration örneği "sonlandırma" URL'si. |
| rewindPostUri     |Orchestration örneği "geri" URL'si. |

Bir yanıt örneği aşağıda verilmiştir:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "rewindPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/rewind?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```
> [!NOTE]
> Web kancası URL'leri biçimi, Azure işlevleri ana bilgisayarın hangi sürümünün kullanmakta olduğunuz bağlı olarak farklı olabilir. Yukarıdaki örnek için Azure işlevleri 2.0 ana bilgisayardır.

## <a name="async-operation-tracking"></a>Zaman uyumsuz işlem izleme

Daha önce bahsedilen HTTP yanıtına, uzun süre çalışan HTTP zaman uyumsuz API'leri ile dayanıklı işlevler uygulama yardımcı olmak için tasarlanmıştır. Bu bazen olarak adlandırılır *yoklama tüketici modeli*. İstemci/sunucu akışı aşağıdaki gibi çalışır:

1. İstemci bir düzenleyici işlevi gibi uzun süre çalışan bir işlem başlatmak için bir HTTP isteği verir.
2. Hedef HTTP tetikleyicisi ile bir HTTP 202 yanıtı döndürür bir `Location` üstbilgiyle `statusQueryGetUri` değeri.
3. URL'de istemci yoklama `Location` başlığı. HTTP 202 yanıtları görmek devam bir `Location` başlığı.
4. Örnek tamamlandıktan (veya başarısız olduğunda), uç noktayı `Location` üst bilgisi, HTTP 200 döndürür.

Bu protokol, dış istemcilere ya da bir HTTP uç noktasını yoklayarak ve aşağıdaki Destek Hizmetleri ile uzun süre çalışan işlemleri koordine sağlar `Location` başlığı. Temel parçaları dayanıklı işlevler HTTP API'lere zaten oluşturulmuştur.

> [!NOTE]
> Varsayılan olarak, tüm HTTP tabanlı eylemler tarafından sağlanan [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) standart zaman uyumsuz işlem desenini destekler. Bu özellik, Logic Apps iş akışının bir parçası uzun süre çalışan dayanıklı işlevi eklemek mümkün kılar. Zaman uyumsuz HTTP desenleri bulunabilir Logic Apps hakkında daha fazla ayrıntı desteği [Azure Logic Apps iş akışı eylemleri ve Tetikleyicileri belgeleri](../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>HTTP API Başvurusu

Tüm HTTP API'lerini aşağıdaki parametreleri uzantısı sınav zamanı tarafından uygulanır. Tüm parametre veri türü `string`.

| Parametre  | Parametre türü  | Açıklama |
|------------|-----------------|-------------|
| InstanceId | URL'si             | Orchestration örneği kimliği. |
| taskHub    | Sorgu dizesi    | Adını [görev hub](durable-functions-task-hubs.md). Belirtilmezse, geçerli işlevi uygulamanın görev hub adı varsayılır. |
| bağlantı | Sorgu dizesi    | **Adı** depolama hesabı için bağlantı dizesi. Belirtilmemişse, işlev uygulaması için varsayılan bağlantı dizesini kabul edilir. |
| systemKey  | Sorgu dizesi    | API'yi çağırmak için gereken yetkilendirme anahtar. |
| showHistory| Sorgu dizesi    | İsteğe bağlı parametre. Varsa kümesine `true`, orchestration yürütme geçmişini yanıt yükünde dahil edilir.| 
| showHistoryOutput| Sorgu dizesi    | İsteğe bağlı parametre. Varsa kümesine `true`, etkinlik çıkışı dahil edilecek düzenleme yürütme geçmişi.| 

`systemKey` Azure işlevleri ana bilgisayar tarafından otomatik olarak oluşturulmuş bir yetkilendirme anahtardır. Özellikle dayanıklı görev uzantısı API'ler için erişim verir ve aynı şekilde yönetilebilir [diğer yetkilendirme anahtarları](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). Bulunacak en basit yolu `systemKey` değerdir kullanarak `CreateCheckStatusResponse` API daha önce bahsedilen.

Sonraki birkaç bölümlerde HTTP API'lerini uzantı tarafından desteklenen özel kapak ve bunların nasıl kullanılabileceğini örnekleri sağlar.

### <a name="get-instance-status"></a>Örneğinin durumunu Al

Belirtilen orchestration örneğinin durumunu alır.

#### <a name="request"></a>İstek

İşlevleri 1.0 için istek biçimi aşağıdaki gibidir:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçimi, aynı parametrelere sahip ancak biraz farklı bir URL ön eki vardır:

```http
GET /runtime/webhooks/durabletask/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}&showHistory={showHistory}&showHistoryOutput={showHistoryOutput}
```

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değeri döndürülür.

* **200 (Tamam) HTTP**: Belirtilen örnek tamamlanmış bir durumda.
* **202 (kabul edildi) HTTP**: belirtilen örneği devam ediyor.
* **HTTP 400 (Hatalı istek)**: Belirtilen örnek başarısız oldu veya sonlandırıldı.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek yok ya da çalışan başlatılmadı.
* **HTTP 500 (iç sunucu hatası)**: belirtilen örneği işlenmeyen bir özel durumla başarısız oldu.

Yanıt yükü **HTTP 200** ve **HTTP 202** durumda şu alanlara sahip bir JSON nesnesi:

| Alan           | Veri türü | Açıklama |
|-----------------|-----------|-------------|
| runtimeStatus   | dize    | Örnek çalışma zamanı durumu. Değerler *çalıştıran*, *bekleyen*, *başarısız*, *iptal*, *kesildi*, *Tamamlandı*. |
| input           | JSON      | Örneği başlatmak için kullanılan JSON verileri. |
| customStatus    | JSON      | Özel düzenleme durumu için kullanılan JSON verileri. Bu alan `null` değilse ayarlayın. |
| çıkış          | JSON      | Örnek JSON çıkışı. Bu alan `null` örneği tamamlanmış durumda değilse. |
| oluşturulma zamanı     | dize    | Örneği oluşturulduğu zaman. ISO 8601 genişletilmiş gösterimi kullanır. |
| lastUpdatedTime | dize    | Hangi örneğinin son kalıcı saat. ISO 8601 genişletilmiş gösterimi kullanır. |
| historyEvents   | JSON      | Orchestration yürütme geçmişini içeren bir JSON dizisi. Bu alan `null` sürece `showHistory` sorgu dizesi parametresi ayarlandığında `true`.  | 

Orchestration yürütme geçmişini ve etkinlik çıktıları (okunabilmesi için biçimlendirilmiştir) dahil olmak üzere bir örnek yanıt yükü şu şekildedir:

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

**HTTP 202** yanıt da içeren bir **konumu** aynı URL'ye başvuran yanıt üst bilgisi `statusQueryGetUri` alan daha önce bahsedilen.

### <a name="get-all-instances-status"></a>Tüm örnekleri durumunu Al

Ayrıca, tüm örnekleri durumu sorgulayabilirsiniz. Kaldırma `instanceId` 'örneği durumunu Al' istek. Parametreleri 'Get örneği durumu.' ile aynıdır 

#### <a name="request"></a>İstek

İşlevleri 1.0 için istek biçimi aşağıdaki gibidir:

```http
GET /admin/extensions/DurableTaskExtension/instances/?taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 aynı parametreleri ancak biraz farklı bir URL ön eki biçimdedir: 

```http
GET /runtime/webhooks/durabletask/instances/?taskHub={taskHub}&connection={connection}&code={systemKey}
```

#### <a name="response"></a>Yanıt

Yanıt yükü düzenleme durumu (okunabilmesi için biçimlendirilmiştir) dahil olmak üzere, bir örnek aşağıda verilmiştir:

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
> Çok sayıda örnek tablosundaki satırları varsa bu işlem Azure depolama g/ç açısından çok pahalı olabilir. Örnek tablo hakkında daha fazla ayrıntı bulunabilir [performansı ve ölçeği dayanıklı işlevler (Azure işlevleri) içinde](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-perf-and-scale#instances-table) belgeleri.
> 

### <a name="raise-event"></a>Olayı

Çalışan bir düzenleme örneği için bir olay bildirim iletisi gönderir.

#### <a name="request"></a>İstek

İşlevleri 1.0 için istek biçimi aşağıdaki gibidir:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçimi, aynı parametrelere sahip ancak biraz farklı bir URL ön eki vardır:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Aşağıdaki benzersiz parametreleri yanı sıra daha önce bahsedilen varsayılan belirlenen parametreler bu API için istek:

| Alan       | Parametre türü  | Veri tType | Açıklama |
|-------------|-----------------|-----------|-------------|
| EventName   | URL'si             | dize    | Hedef düzenleme örneği bekleniyor olayın adı. |
| {İçerik}   | İstek içeriği | JSON      | JSON biçimli bir olay yükü. |

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değeri döndürülür.

* **202 (kabul edildi) HTTP**: oluşturulan olay işleme için kabul edildi.
* **HTTP 400 (Hatalı istek)**: İstek içeriği türünde değildi `application/json` veya geçerli bir JSON değildi.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP (Gone) 410**: Belirtilen örnek tamamlandı veya başarısız ve yükseltilmiş meydana gelen olayları işlenemiyor.

JSON dizesi gönderen bir örnek istek işte `"incr"` adlı bir olay bekleniyor örneğine **işlemi**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Bu API için yanıtlar herhangi bir içerik içermez.

### <a name="terminate-instance"></a>Örnek Sonlandır

Çalışan bir düzenleme örneği sonlandırır.

#### <a name="request"></a>İstek

İşlevleri 1.0 için istek biçimi aşağıdaki gibidir:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçimi, aynı parametrelere sahip ancak biraz farklı bir URL ön eki vardır:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

İstek parametreleri bu API için şu benzersiz parametre yanı sıra daha önce bahsedilen varsayılan kümesi içerir.

| Alan       | Parametre türü  | Veri Türü | Açıklama |
|-------------|-----------------|-----------|-------------|
| reason      | Sorgu dizesi    | dize    | İsteğe bağlı. Orchestration örneği sonlandırılıyor nedeni. |

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değeri döndürülür.

* **202 (kabul edildi) HTTP**: sonlandırma isteği işleme için kabul edildi.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP (Gone) 410**: Belirtilen örnek tamamlandı veya başarısız oldu.

Çalışan bir örneği sona erer ve bir nedenini belirten bir örnek istek işte **buggy**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API için yanıtlar herhangi bir içerik içermez.

## <a name="rewind-instance-preview"></a>Geri Sar örneği (Önizleme)

En son başarısız işlemleri yeniden yürüterek çalışır duruma başarısız düzenleme örneğine yükler.

#### <a name="request"></a>İstek

İşlevleri 1.0 için istek biçimi aşağıdaki gibidir:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/rewind?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

İşlevler 2.0 biçimi, aynı parametrelere sahip ancak biraz farklı bir URL ön eki vardır:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/rewind?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

İstek parametreleri bu API için şu benzersiz parametre yanı sıra daha önce bahsedilen varsayılan kümesi içerir.

| Alan       | Parametre türü  | Veri Türü | Açıklama |
|-------------|-----------------|-----------|-------------|
| reason      | Sorgu dizesi    | dize    | İsteğe bağlı. Orchestration örneği geri sarma nedeni. |

#### <a name="response"></a>Yanıt

Birkaç olası durum kodu değeri döndürülür.

* **202 (kabul edildi) HTTP**: Geri Sar istek işleme için kabul edildi.
* **HTTP 404 (bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP (Gone) 410**: Belirtilen örnek tamamlandı veya sonlandırıldı.

Başarısız bir olayı geri sarar ve bir nedenini belirten bir örnek istek işte **sabit**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/rewind?reason=fixed&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API için yanıtlar herhangi bir içerik içermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hataları işleme hakkında bilgi edinin](durable-functions-error-handling.md)
