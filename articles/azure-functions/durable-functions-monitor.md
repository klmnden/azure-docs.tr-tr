---
title: Dayanıklı işlevleri - Azure izleyicileri
description: Azure işlevleri için dayanıklı işlevleri uzantısı kullanılarak bir Durum İzleyicisi uygulamak öğrenin.
services: functions
author: kashimiz
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 9cb7a076ea922b9868bd439d160aec96f044e3b6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Dayanıklı işlevleri - hava durumu İzleyicisi örnek senaryoda izleme

İzleyici düzeni esnek bir başvuruyor *yinelenen* bir iş akışı - Örneğin, belirli koşulların karşılandığından kadar yoklama işleminde. Bu makalede kullanan bir örneğin açıklanmaktadır [dayanıklı işlevleri](durable-functions-overview.md) izleme uygulamak için.

## <a name="prerequisites"></a>Önkoşullar

* [Dayanıklı işlevleri yüklemek](durable-functions-install.md).
* Tamamlamak [Hello dizisi](durable-functions-sequence.md) gözden geçirme.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnek bir konumun geçerli hava koşulları izler ve bir kullanıcı SMS tarafından skies açık olduğunda uyarır. Normal bir zamanlayıcı tetiklemeli işlevin hava denetleyin ve uyarıları göndermek için kullanabilirsiniz. Ancak, bu yaklaşım ile ilgili bir sorun olduğunu **yaşam süresi management**. Yalnızca bir uyarı gönderilmesi gerektiğini İzleyici kendisini Temizle sonra devre dışı bırakmak gerek duyduğu hava durumu algılandı. İzleme düzeni başka avantajları arasında kendi yürütme sona erdirilebilir:

* Aralıklarına göre çalıştır izleyiciler olmayan zamanlar: Zamanlayıcı tetikleyicisi *çalıştıran* her saat; bir izleme *bekler* eylemler arasında bir saat. Bir izleyicinin Eylemler belirtilmediği sürece, uzun süre çalışan görevler için önemli olabilir çakışmaması.
* İzleyiciler dinamik aralıkları sahip olabilir: bekleme süresi dayalı bir koşulu olarak değiştirebilirsiniz.
* Bazı koşul karşılanır veya başka bir işlem tarafından sona erdirilecek izleyiciler sonlandırabilir.
* İzleyiciler parametre alabilir. Örnek, tüm istenen konumu ve telefon numarası aynı işlem hava durumu izleme nasıl uygulanabilir gösterir.
* İzleyiciler ölçeklenebilir. Her bir izleyici orchestration örneği olduğundan, birden çok monitör yeni işlevler oluşturmak veya daha fazla kod tanımlamak zorunda kalmadan oluşturulabilir.
* İzleyiciler kolayca büyük iş akışlarıyla birleştirilerek tümleştirin. Bir izleyici daha karmaşık bir orchestration işlevinin bir bölümü olabilir veya bir [alt orchestration](https://docs.microsoft.com/azure/azure-functions/durable-functions-sub-orchestrations).

## <a name="configuring-twilio-integration"></a>Twilio tümleştirmesini yapılandırma

[!INCLUDE [functions-twilio-integration](../../includes/functions-twilio-integration.md)]

## <a name="configuring-weather-underground-integration"></a>Hava durumu Yeraltı tümleştirmesini yapılandırma

Bu örnek, bir konum geçerli hava durumu koşulu denetlemek için hava durumu Yeraltı API'yi kullanarak içerir.

Size gereken ilk şey hava durumu Yeraltı bir hesaptır. Bir tane ücretsiz adresindeki oluşturabilirsiniz [ https://www.wunderground.com/signup ](https://www.wunderground.com/signup). Hesabınızı edindikten sonra bir API anahtarı edinmeye gerekir. Ziyaret ederek yapabilirsiniz [ https://www.wunderground.com/weather/api ](https://www.wunderground.com/weather/api), anahtar ayarlarını seçtikten sonra. Stratus Geliştirici planı ücretsiz ve bu örneği çalıştırmak yeterli.

Bir API anahtarı olduktan sonra aşağıdaki ekleyin **uygulama ayarı** işlevi uygulamanıza.

| Uygulama ayarı adı | Değer açıklaması |
| - | - |
| **WeatherUndergroundApiKey**  | Hava durumu Yeraltı API anahtarınıza. |

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamasında aşağıdaki işlevleri açıklanmaktadır:

* `E3_Monitor`: Çağırır bir orchestrator işlevi `E3_GetIsClear` düzenli aralıklarla. Çağırır `E3_SendGoodWeatherAlert` varsa `E3_GetIsClear` true değerini döndürür.
* `E3_GetIsClear`: Geçerli bir konum hava koşulları denetler bir etkinlik işlevi.
* `E3_SendGoodWeatherAlert`: Twilio aracılığıyla SMS iletisi gönderir bir etkinlik işlevi.

Aşağıdaki bölümlerde kullanılan kod ve yapılandırma açıklanmaktadır C# kodlama için. Visual Studio geliştirme için kod makalenin sonunda gösterilir.
 
## <a name="the-weather-monitoring-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Orchestration (Visual Studio Code ve Azure portal örnek kodu) izleme hava durumu

**E3_Monitor** işlevini kullanan standart *function.json* orchestrator işlevler için.

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Monitor/function.json)]

İşlev uygulayan kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Monitor/run.csx)]

Bu orchestrator işlevi aşağıdaki eylemleri gerçekleştirir:

1. Alır **MonitorRequest** oluşan *konumu* izlemek için ve *telefon numarası* için hangi SMS bildirim gönderir.
2. İzleyici sona erme süresini belirler. Örnek bir sabit kodlu değer okumanızdır kullanır.
3. Çağrıları **E3_GetIsClear** istenen konumda Temizle skies olup olmadığını belirlemek için.
4. Hava boş olduğunda, çağıran **E3_SendGoodWeatherAlert** istenen telefon numarasına SMS bildirim göndermek için.
5. Sonraki yoklama aralığında orchestration sürdürmek için sağlam bir zamanlayıcı oluşturur. Örnek bir sabit kodlu değer okumanızdır kullanır.
6. Kadar çalışmaya devam [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) geçişleri monitörün süre sonu zamanı ya da SMS uyarı gönderilir.

Birden çok orchestrator örnekleri, aynı anda birden çok göndererek çalışabilir **MonitorRequests**. İzlemek için konum ve bir SMS uyarı göndermek için telefon numarası belirtilebilir.

## <a name="strongly-typed-data-transfer"></a>Kesin türü belirtilmiş veri aktarımı

Orchestrator birden çok veri parçaları, bu nedenle gerektirir [POCO nesneleri paylaşılan](functions-reference-csharp.md#reusing-csx-code) kesin türü belirtilmiş veri aktarımı için kullanılır: [!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/MonitorRequest.csx)]

[!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/Location.csx)]

## <a name="helper-activity-functions"></a>Yardımcı etkinlik işlevleri

Diğer örnekleri ile yardımcı etkinlik işlevleri kullanan normal işlevlerdir gibi `activityTrigger` tetiklemek bağlama. **E3_GetIsClear** işlevi hava durumu Yeraltı API'yi kullanarak geçerli hava koşulları alır ve sky açık olup olmadığını belirler. *Function.json* şu şekilde tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/function.json)]

Ve burada uygulamasıdır. Veri aktarımı için kullanılan POCOs gibi API işlemek için mantığı çağırın ve JSON paylaşılan sınıfına soyutlanır yanıtı ayrıştırılamadı. Bir parçası olarak Bul [Visual Studio örnek kod](#run-the-sample).

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/run.csx)]

**E3_SendGoodWeatherAlert** işlevi, son kullanıcı bir ilerlemesi için iyi bir zamandır olduğunu bildiren bir SMS iletisi göndermek için Twilio bağlama kullanır. Kendi *function.json* basittir:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/function.json)]

Ve SMS mesajı gönderir kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/run.csx)]

## <a name="run-the-sample"></a>Örneği çalıştırma

Aşağıdaki örnekte dahil HTTP tetiklemeli işlevleri kullanarak, aşağıdaki HTTP POST isteği göndererek orchestration başlatabilirsiniz:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "Location": { "City": "Redmond", "State": "WA" }, "Phone": "+1425XXXXXXX" }
```
```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

**E3_Monitor** örneği başlatır ve istenilen konuma geçerli hava koşulları sorgular. Hava boş olduğunda, bir uyarı göndermek için bir etkinlik işlevini çağırır; Aksi halde, bir süreölçer ayarlar. Süreölçer süresi dolduğunda, orchestration devam eder.

Azure işlevleri Portalı'nda konumundaki işlev bakarak orchestration ait etkinlik günlükleri görebilirsiniz.

```
2018-03-01T01:14:41.649 Function started (Id=2d5fcadf-275b-4226-a174-f9f943c90cd1)
2018-03-01T01:14:42.741 Started orchestration with ID = '1608200bb2ce4b7face5fc3b8e674f2e'.
2018-03-01T01:14:42.780 Function completed (Success, Id=2d5fcadf-275b-4226-a174-f9f943c90cd1, Duration=1111ms)
2018-03-01T01:14:52.765 Function started (Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb)
2018-03-01T01:14:52.890 Received monitor request. Location: Redmond, WA. Phone: +1425XXXXXXX.
2018-03-01T01:14:52.895 Instantiating monitor for Redmond, WA. Expires: 3/1/2018 7:14:52 AM.
2018-03-01T01:14:52.909 Checking current weather conditions for Redmond, WA at 3/1/2018 1:14:52 AM.
2018-03-01T01:14:52.954 Function completed (Success, Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb, Duration=189ms)
2018-03-01T01:14:53.226 Function started (Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859)
2018-03-01T01:14:53.808 Function completed (Success, Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859, Duration=582ms)
2018-03-01T01:14:53.967 Function started (Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c)
2018-03-01T01:14:53.996 Next check for Redmond, WA at 3/1/2018 1:44:53 AM.
2018-03-01T01:14:54.030 Function completed (Success, Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c, Duration=62ms)
```

Orchestration olacak [sonlandırmak](durable-functions-instance-management.md#terminating-instances) , zaman aşımı ulaştı veya açık olduğunda skies algılanır. De kullanabilirsiniz `TerminateAsync` iç işlev veya çağırma **terminatePostUri** yukarıda değiştirme 202 yanıt başvurulan HTTP POST Web kancası `{text}` sonlandırma nedeni ile:

```
POST https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyasında bir Visual Studio projesi olarak orchestration şöyledir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek dayanıklı işlevlerinin bir dış kaynağının durumunu izlemek için nasıl kullanılacağı gösterilmiştir kullanarak [dayanıklı zamanlayıcılar](durable-functions-timers.md) ve koşullu mantık. Sonraki örnek dış olayları kullanmayı gösterir ve [dayanıklı zamanlayıcılar](durable-functions-timers.md) insan etkileşimi işlemek için.

> [!div class="nextstepaction"]
> [İnsan etkileşimi örneğini çalıştırın](durable-functions-phone-verification.md)
