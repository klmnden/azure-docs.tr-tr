---
title: Dayanıklı işlevleri - Azure zincirleme işlevi
description: Bir dizi işlevleri yürütür dayanıklı işlevleri örneği çalıştırmak öğrenin.
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
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: e53b38bf336816ca670fad3ab70a43e5cc8b3437
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Dayanıklı işlevleri - Hello sırası örneği zincirleme işlevi

İşlev zincirleme işlevleri bir dizi belirli bir sırada yürütmenin düzeni ifade eder. Genellikle bir işlev çıktısını başka bir işlevin giriş uygulanması gerekir. Bu makalede kullanan bir örneğin açıklanmaktadır [dayanıklı işlevleri](durable-functions-overview.md) işlevi zincirleme uygulamak için.

## <a name="prerequisites"></a>Önkoşullar

* [Dayanıklı işlevleri yüklemek](durable-functions-install.md).

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamasında aşağıdaki işlevleri açıklanmaktadır:

* `E1_HelloSequence`: Çağırır bir orchestrator işlevi `E1_SayHello` birden çok kez sırayla. Çıkışlarından depolar `E1_SayHello` çağıran ve sonuçları kaydeder.
* `E1_SayHello`: "Hello" dizesiyle başına bir etkinlik işlevi.

Aşağıdaki bölümlerde, scripting C# ve JavaScript için kullanılan kod ve yapılandırma açıklanmaktadır. Visual Studio geliştirme için kod makalenin sonunda gösterilir.

> [!NOTE]
> Dayanıklı işlevleri v2 işlevleri çalışma zamanında yalnızca JavaScript'te mevcut değil.

## <a name="e1hellosequence"></a>E1_HelloSequence
### <a name="functionjson-file"></a>Function.JSON dosyası

Geliştirme için Visual Studio Code veya Azure portalını kullanıyorsanız, içeriği işte *function.json* orchestrator işlevi için dosya. Çoğu orchestrator *function.json* dosyaları neredeyse tam olarak şöyle arayın.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

Önemli şey `orchestrationTrigger` bağlama türü. Tüm orchestrator işlevleri, bu tetikleyici türünü kullanmanız gerekir.

> [!WARNING]
> Orchestrator işlevleri "hiçbir g/ç" kuralı tarafından uymanız için olmayan herhangi bir giriş kullanın veya bağlamaları kullanırken çıktı `orchestrationTrigger` tetiklemek bağlama.  Diğer giriş veya çıkış bağlamaları gereklidir, bunlar bunun yerine bağlamında kullanılmalıdır `activityTrigger` orchestrator tarafından çağrılan işlev.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>C# betik (Visual Studio Code ve Azure portal örnek kodu) 

Kaynak kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Tüm C# orchestration işlevleri türünde bir parametresi olmalıdır `DurableOrchestrationContext`, içinde bulunduğu `Microsoft.Azure.WebJobs.Extensions.DurableTask` derleme. C# betiği kullanıyorsanız, derleme kullanarak başvurulabilir `#r` gösterimi. Bu bağlam nesnesinin diğer çağrı olanak sağlayan *etkinlik* işlevler ve giriş parametreleri kullanarak kendi [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) yöntemi.

Kod çağrıları `E1_SayHello` üç kez sırayla farklı parametre değerleri. Her çağrısının dönüş değeri eklenen `outputs` işlevi sonunda döndürülen listesi.

### <a name="javascript"></a>Javascript

Kaynak kod aşağıdaki gibidir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Tüm JavaScript orchestration işlevleri içermelidir `durable-functions` modülü. Bu işlem dışı diller için dayanıklı'nın yürütme Protokolü orchestration işlevin eylemlere çeviren bir JavaScript kitaplıktır. Orchestration işlevi ve diğer JavaScript işlevleri arasında üç önemli farklılıklar vardır:

1. İşlev bir [oluşturucu işlevi.](https://docs.microsoft.com/en-us/scripting/javascript/advanced/iterators-and-generators-javascript)
2. İşlev çağrısı paketlenir `durable-functions` Modülü (burada `df`).
3. İşlev çağrısı yaparak sona erer `return`değil `context.done`.

`context` Nesnesini içeren bir `df` nesne diğer çağrı olanak tanır *etkinlik* işlevler ve giriş parametreleri kullanarak kendi `callActivityAsync` yöntemi. Kod çağrıları `E1_SayHello` üç kez dizisindeki kullanarak farklı parametre değerleri ile `yield` yürütme döndürülecek zaman uyumsuz etkinlik işlev çağrılarını beklemesi gereken belirtmek için. Her çağrısının dönüş değeri eklenen `outputs` işlevi sonunda döndürülen listesi.

## <a name="e1sayhello"></a>E1_SayHello
### <a name="functionjson-file"></a>Function.JSON dosyası

*Function.json* etkinlik işlevi için dosya `E1_SayHello` olarak benzer `E1_HelloSequence` kullanır ancak bu bir `activityTrigger` bağlama türü yerine bir `orchestrationTrigger` bağlama türü.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Orchestration işlev tarafından çağrılan işlev kullanmalısınız `activityTrigger` bağlama.

Uygulaması `E1_SayHello` işlemini biçimlendirme oldukça basit bir dize.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Bu işlev türünde bir parametreye sahiptir [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html), orchestrator işlev çağrısı tarafından kendisine geçirilen giriş almak için kullandığı [ `CallActivityAsync<T>` ](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

### <a name="javascript"></a>JavaScript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

JavaScript orchestration işlevinden farklı olarak, hiçbir özel kurulum bir JavaScript etkinlik işlevi gerekir. Orchestrator işlevi tarafından geçirilen giriş bulunan `context.bindings` nesne adı altında `activitytrigger` bağlama - bu durumda, `context.bindings.name`. Örnek kod yaptığı olduğu bağlama adı verilen işlevi parametre olarak ayarlanmış olabilir ve doğrudan erişilebilir.

## <a name="run-the-sample"></a>Örneği çalıştırma

Yürütülecek `E1_HelloSequence` orchestration, aşağıdaki HTTP POST isteği gönder.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

Örneğin, "myfunctionapp" adlı bir işlev uygulamasında örnek çalıştırıyorsanız, "{konak}" "myfunctionapp.azurewebsites.net" ile değiştirin.

Bu (okumanızdır kırpılmış) gibi bir HTTP 202 yanıt oluşur:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Bu noktada, orchestration sıraya ve hemen çalışmaya başlar. URL'de `Location` üstbilgisi, yürütme durumunu denetlemek için kullanılabilir.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Orchestration durumunu sonucudur. Çalıştığı ve içinde görmeleri hızla tamamlandığında *tamamlandı* bu (okumanızdır kırpılmış) benzer bir yanıt durum:

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Gördüğünüz gibi `runtimeStatus` örneğinin *tamamlandı* ve `output` orchestrator işlevi yürütme JSON serileştirilmiş sonucunu içerir.

> [!NOTE]
> Orchestrator işlevi başlatılan HTTP POST uç nokta örnek uygulamayı bir HTTP olarak uygulanan tetiklemek "HttpStart" adlı işlev. Gibi diğer tetikleyici türleri için benzer başlangıç mantığı uygulayabilirsiniz `queueTrigger`, `eventHubTrigger`, veya `timerTrigger`.

İşlev yürütme günlüklerine bakın. `E1_HelloSequence` İşlevi başlatıldı ve açıklanan yeniden yürütme davranışı nedeniyle birden çok kez tamamlandı [genel bakış](durable-functions-overview.md). Diğer taraftan, yalnızca üç yürütmeleri vardı `E1_SayHello` beri bu işlevi yürütmeleri değil yeniden.

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyasında bir Visual Studio projesi olarak orchestration şöyledir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, bir basit işlevi zincirleme orchestration gösterilmiştir. Sonraki örnek fan-dışarı/fan-arada desen uygulamak nasıl gösterir. 

> [!div class="nextstepaction"]
> [Fan-dışarı/fan-arada örneğini çalıştırın](durable-functions-cloud-backup.md)
