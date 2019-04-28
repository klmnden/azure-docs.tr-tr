---
title: Dayanıklı işlevler - Azure izleyicileri
description: Bir Durum izleyicisini, Azure işlevleri için dayanıklı işlevler uzantısını kullanarak uygulamayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 9be062ec42f054832225c17a65b06e47dbcbe990
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123493"
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Dayanıklı işlevler - hava durumu İzleyicisi örnek senaryoda izleyin

İzleyici deseni esnek bir başvuruyor *yinelenen* bir iş akışında - Örneğin, belirli koşulların karşılanması kadar yoklama işlemi. Bu makalede kullanan bir örnek açıklanmaktadır [dayanıklı işlevler](durable-functions-overview.md) izleme uygulamak için.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnek, bir konumun geçerli hava koşulları izler ve bir kullanıcı tarafından SMS skies açık olduğunda uyarır. Normal bir Zamanlayıcı ile tetiklenen işlev hava durumunu denetleyin ve uyarıları göndermek için kullanabilirsiniz. Ancak, bu yaklaşım ile ilgili bir sorun olduğunu **ömür Yönetimi**. Yalnızca bir uyarı gönderilmesi gereken kendisini Temizle sonra devre dışı bırakmak izleyicinin hava durumu algılandı. İzleme desen, diğer avantajlarının yanı sıra kendi yürütme sona erdirilebilir:

* İzleyiciler, belirli aralıklarla çalıştırmak olmayan zamanlar: Zamanlayıcı tetikleyicisi *çalıştıran* saatte; bir izleyici *bekler* eylemler arasında bir saat. Bir izleyicinin eylemleri belirtilmediği sürece, uzun süre çalışan görevler için önemli olabilir çakışmaması.
* İzleyicilerle dinamik aralıkları: bekleme süresine bağlı olarak bazı koşullar değiştirebilirsiniz.
* Bazı koşullar karşılanması veya başka bir işlem tarafından sonlandırılacak izleyiciler sonlandırabilirsiniz.
* İzleyiciler, parametre alabilir. Tüm istenen konumu ve telefon numarası aynı hava durumu izleme işlemini nasıl uygulanabileceğini örnek gösterir.
* İzleyiciler ölçeklenebilir. Her bir izleyici bir düzenleme örneği olduğundan, birden çok monitör yeni işlevler oluşturun veya daha fazla kod tanımlamak zorunda kalmadan oluşturulabilir.
* İzleyiciler, daha büyük iş akışlarınızla kolayca tümleştirin. Bir izleyici daha karmaşık bir düzenleme işlevinin bir bölümü olabilir veya [alt düzenleme](durable-functions-sub-orchestrations.md).

## <a name="configuring-twilio-integration"></a>Twilio tümleştirmesi yapılandırma

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="configuring-weather-underground-integration"></a>Hava durumu Yeraltı tümleştirmesini yapılandırma

Bu örnek, bir konum için geçerli hava durumu koşullarını denetlemek için hava durumu Yeraltı API kullanmayı içerir.

İhtiyacınız olan ilk şey bir hava durumu Yeraltı hesabıdır. Ücretsiz, oluşturabilirsiniz [ https://www.wunderground.com/signup ](https://www.wunderground.com/signup). Hesabınızı edindikten sonra bir API anahtarı alma gerekecektir. Ziyaret ederek bunu [ https://www.wunderground.com/weather/api ](https://www.wunderground.com/weather/api), anahtar ayarlarını seçildikten sonra. Stratus Geliştirici planı ücretsizdir ve bu örneği çalıştırmak yeterli kullanılabilir.

Bir API anahtarı aldıktan sonra aşağıdaki ekleyin **uygulama ayarı** işlev uygulamanız için.

| Uygulama ayarı adı | Değer Açıklama |
| - | - |
| **WeatherUndergroundApiKey**  | Hava durumu Yeraltı API anahtarınızı. |

## <a name="the-functions"></a>İşlevleri

Bu makalede örnek uygulama aşağıdaki işlevler açıklanmaktadır:

* `E3_Monitor`: Çağıran bir düzenleyici işlevi `E3_GetIsClear` düzenli aralıklarla. Çağrı `E3_SendGoodWeatherAlert` varsa `E3_GetIsClear` true değerini döndürür.
* `E3_GetIsClear`: Bir konum için geçerli hava koşulları denetler etkinlik işlevi.
* `E3_SendGoodWeatherAlert`: Twilio aracılığıyla SMS iletisi gönderen bir etkinlik işlev.

Aşağıdaki bölümlerde, betik C# ve JavaScript için kullanılan kod ve yapılandırma açıklanmaktadır. Visual Studio geliştirme için kod makalenin sonunda gösterilir.

## <a name="the-weather-monitoring-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Düzenleme (Visual Studio Code ve Azure portalı örnek kodu) izleme hava durumu

**E3_Monitor** işlevini kullanan standart *function.json* orchestrator işlevleri için.

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Monitor/function.json)]

İşlev uygulayan kod şu şekildedir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Monitor/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/index.js)]

Bu orchestrator işlevi aşağıdaki eylemleri gerçekleştirir:

1. Alır **MonitorRequest** oluşan *konumu* izlemek için ve *telefon numarası* için hangi SMS bildirimi gönderir.
2. İzleyici zaman aşımı süresini belirler. Örnek, bir sabit kodlu değer konuyu uzatmamak amacıyla kullanır.
3. Çağrıları **E3_GetIsClear** istenen konuma Temizle skies olup olmadığını belirlemek için.
4. Hava durumu, boş olduğunda, çağıran **E3_SendGoodWeatherAlert** istenen telefon numarasına SMS bildirimi göndermek için.
5. Sonraki yoklama aralığı sırasında orchestration sürdürmek için kalıcı bir zamanlayıcı oluşturur. Örnek, bir sabit kodlu değer konuyu uzatmamak amacıyla kullanır.
6. Kadar çalışmaya devam eder [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) (C#) veya `currentUtcDateTime` geçirir izleyicinin sona erme saati (JavaScript) veya SMS uyarı gönderilir.

Birden çok orchestrator örnekleri aynı anda birden çok göndererek çalışabilir **MonitorRequests**. İzlemek istediğiniz konumu ve telefon numarası için SMS uyarısı göndermek için belirtilebilir.

## <a name="strongly-typed-data-transfer-net-only"></a>Kesin türü belirtilmiş veri aktarımı (yalnızca .NET)

Orchestrator bu nedenle, verilerin birden çok parça gerektirir [POCO nesneleri paylaşılan](../functions-reference-csharp.md#reusing-csx-code) C# ve C# betiği kesin türü belirtilmiş veri aktarımı için kullanılır: [!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/MonitorRequest.csx)]

[!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/Location.csx)]

JavaScript örnek normal JSON nesneleri parametreler olarak kullanır.

## <a name="helper-activity-functions"></a>Yardımcı etkinlik işlevleri

Diğer örneklerle yardımcı etkinlik işlevleri kullanan normal işlevlerde olduğu gibi `activityTrigger` bağlama tetikleyin. **E3_GetIsClear** işlevi hava durumu Yeraltı API'sini kullanarak geçerli hava koşulları alır ve sky açık olup olmadığını belirler. *Function.json* şu şekilde tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/function.json)]

Ve burada uygulamasıdır. Veri aktarımı için kullanılan POCOs gibi API işlemek için mantığı çağırın ve JSON paylaşılan bir sınıf C# içinde soyutlanır yanıtı ayrıştıramadı. Bir parçası olarak bulabilirsiniz [Visual Studio örnek kodu](#run-the-sample).

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/index.js)]

**E3_SendGoodWeatherAlert** işlevi, son kullanıcının bir ilerlemesi için uygun bir zaman olduğunu bildiren bir SMS mesajı göndermek için Twilio bağlama kullanır. Kendi *function.json* basittir:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/function.json)]

Ve SMS mesajı gönderir kod aşağıdaki gibidir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/index.js)]

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnekte bulunan bir HTTP tetiklemeli işlevleri'ni kullanarak, aşağıdaki HTTP POST isteği göndererek düzenleme başlayabilirsiniz:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "location": { "city": "Redmond", "state": "WA" }, "phone": "+1425XXXXXXX" }
```

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

**E3_Monitor** örneği başlatır ve istenilen konum için geçerli hava koşulları sorgular. Hava durumu boş olduğunda bir uyarı göndermek için bir etkinlik işlevi çağırır; Aksi takdirde, bir zamanlayıcı olarak ayarlar. Süreölçerdeki Süre dolduğunda düzenleme devam edecek.

Azure işlevleri portalda konumundaki işlev bakarak düzenleme ait etkinlik günlükleri görebilirsiniz.

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

Orchestration olacak [sonlandırmak](durable-functions-instance-management.md) , zaman aşımı ulaşıldığında veya açık olduğunda skies algılanır. Ayrıca `TerminateAsync` (.NET) veya `terminate` (JavaScript) içinde başka bir işlev veya çağırma **terminatePostUri** yukarıda değiştirerek 202 yanıtının başvurulan bir HTTP POST Web kancası `{text}` nedeni ile sonlandırma:

```
POST https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyası olarak Visual Studio projesi içinde düzenleme şu şekildedir:

> [!NOTE]
> Yüklemek ihtiyacınız olacak `Microsoft.Azure.WebJobs.Extensions.Twilio` aşağıdaki örnek kodu çalıştırmak için Nuget paketi.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, bir dış kaynağının durumunu izlemek için dayanıklı işlevler kullanmayı gösterilmiştir kullanarak [dayanıklı zamanlayıcılar](durable-functions-timers.md) ve koşul mantığı. Sonraki örnek, dış olayları kullanma işlemi gösterilmektedir ve [dayanıklı zamanlayıcılar](durable-functions-timers.md) insan etkileşimi işlemek için.

> [!div class="nextstepaction"]
> [İnsan etkileşimi örneği çalıştırma](durable-functions-phone-verification.md)
