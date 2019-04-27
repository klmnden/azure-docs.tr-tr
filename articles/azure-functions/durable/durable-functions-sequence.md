---
title: Dayanıklı işlevler - Azure zincirleme işlevi
description: İşlevler bir dizi yürüten bir dayanıklı işlevler örneği çalıştırma hakkında bilgi edinin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 4657bd136592c66b5dab9a712f5f1d6df898876c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730557"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Dayanıklı işlevler - Hello dizisi örnek zincirleme işlevi

İşlev zincirleme işlevler bir dizi belirli bir sırayla yürütülmesi deseni ifade eder. Genellikle, bir işlevin çıktısı başka bir işlev girişi uygulanması gerekir. Bu makalede dayanıklı işlevler hızlı başlangıcı tamamladığınızda oluşturduğunuz zincirleme dizisi ([ C# ](durable-functions-create-first-csharp.md) veya [JavaScript](quickstart-js-vscode.md)). Dayanıklı işlevler hakkında daha fazla bilgi için bkz: [dayanıklı işlevler desenleri ve teknik kavramlar](durable-functions-concepts.md).

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="the-functions"></a>İşlevleri

Bu makalede örnek uygulama aşağıdaki işlevler açıklanmaktadır:

* `E1_HelloSequence`: Çağıran bir düzenleyici işlevi `E1_SayHello` içinde birden çok kez bir dizisi. Çıkışları depolar `E1_SayHello` çağırır ve sonuçlarını kaydeder.
* `E1_SayHello`: "Hello" ile bir dize ekler etkinlik işlevi.

Aşağıdaki bölümlerde, betik C# ve JavaScript için kullanılan kod ve yapılandırma açıklanmaktadır. Visual Studio geliştirme için kod makalenin sonunda gösterilir.

> [!NOTE]
> Dayanıklı işlevler JavaScript işlevleri 2.x çalışma zamanı için yalnızca kullanılabilir.

## <a name="e1hellosequence"></a>E1_HelloSequence

### <a name="functionjson-file"></a>Function.JSON dosyası

Geliştirme için Visual Studio Code veya Azure portalını kullanıyorsanız, içeriğini işte *function.json* dosyası için bir düzenleyici işlevi. Çoğu orchestrator *function.json* dosyaları neredeyse şöyle bakın.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

En önemli şey `orchestrationTrigger` bağlama türü. Tüm orchestrator İşlevler, bu tetikleyici türü kullanmanız gerekir.

> [!WARNING]
> Orchestrator işlevleri "hiçbir g/ç" kuralı tarafından uymayı için olmayan herhangi bir giriş kullanın veya çıktı bağlaması kullanırken `orchestrationTrigger` bağlama tetikleyin.  Diğer giriş veya çıkış bağlamaları gereklidir, bunlar bunun yerine bağlamında kullanılmalıdır `activityTrigger` orchestrator tarafından çağrılan işlevleri.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>C# betiği (Visual Studio Code ve Azure portalı örnek kodu)

Kaynak kodu verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Tüm C# düzenleme işlevleri türünde bir parametresi olmalıdır `DurableOrchestrationContext`, içinde bulunduğu `Microsoft.Azure.WebJobs.Extensions.DurableTask` derleme. C# betiği kullanıyorsanız, derlemeyi kullanarak başvurulabilir `#r` gösterimi. Diğer çağrı bu bağlam nesnesinin sağlar *etkinlik* işlevleri ve giriş parametreleri kullanarak kendi [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) yöntemi.

Kod çağrıları `E1_SayHello` üç kez içinde ile farklı parametre değerleri dizisi. Her çağrının dönüş değeri eklenen `outputs` işlevi sonunda verilen listesi.

### <a name="javascript"></a>Javascript

Kaynak kodu verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Tüm JavaScript düzenleme işlevleri içermelidir [ `durable-functions` Modülü](https://www.npmjs.com/package/durable-functions). Bu JavaScript'te, dayanıklı işlevler yazmanıza olanak tanıyan bir kitaplıktır. Bir düzenleme işlevi ve diğer JavaScript işlevleri arasında üç önemli farklar vardır:

1. İşlev, bir [oluşturucu işlevi.](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript)
2. İşlev çağrısında sarmalandıktan `durable-functions` modülün `orchestrator` yöntemi (burada `df`).
3. İşlevi zaman uyumlu olmalıdır. 'Orchestrator' yöntemi çağıran 'context.done' gerçekleştirdiğinden, işlevi yalnızca 'döndürmelidir'.

`context` Nesnesini içeren bir `df` nesne sayesinde diğer çağrı *etkinlik* işlevleri ve giriş parametreleri kullanarak kendi `callActivity` yöntemi. Kod çağrıları `E1_SayHello` dizisi kullanarak, farklı parametre değerleri ile üç kez içinde `yield` yürütme döndürülecek zaman uyumsuz etkinlik işlev çağrılarında beklemesi gereken belirtmek için. Her çağrının dönüş değeri eklenen `outputs` işlevi sonunda verilen listesi.

## <a name="e1sayhello"></a>E1_SayHello

### <a name="functionjson-file"></a>Function.JSON dosyası

*Function.json* etkinlik işlevi için dosya `E1_SayHello` ilkesindekine benzer `E1_HelloSequence` da kullanması hariç, bir `activityTrigger` bağlama türü yerine bir `orchestrationTrigger` bağlama türü.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Bir düzenleme işlev tarafından çağrılan bir işlev kullanmalıdır `activityTrigger` bağlama.

Uygulamasını `E1_SayHello` biçimlendirme işlemi göreceli olarak basit bir dize.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Bu işlev türünde bir parametreye sahip [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html), orchestrator işlev çağrısıyla geçilen girdi almak için kullandığı [ `CallActivityAsync<T>` ](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

### <a name="javascript"></a>JavaScript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

JavaScript düzenleme işlevinden farklı olarak, bir etkinlik işlevin özel kurulum gerekir. Düzenleyici işlevi tarafından geçirilen giriş bulunan `context.bindings` nesne adının altında `activityTrigger` bağlama - bu durumda, `context.bindings.name`. Örnek kodun ne yaptığını olduğu bağlama adı dışarı aktarılan işlevin parametre olarak ayarlanmış olabilir ve doğrudan erişilebilir.

## <a name="run-the-sample"></a>Örneği çalıştırma

Yürütülecek `E1_HelloSequence` düzenleme, aşağıdaki HTTP POST isteği gönderin.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Önceki HTTP kod parçacığı bir giriş olduğunu varsayar `host.json` varsayılan kaldıran dosya `api/` tüm HTTP tetikleyicisi işlevlerini URL'lerden öneki. Bu yapılandırma için işaretleme bulabilirsiniz `host.json` örnekleri dosyasında.

Örneğin, "myfunctionapp" adlı bir işlev uygulamasında örneğe çalıştırıyorsanız, "{konak}", "myfunctionapp.azurewebsites.net" ile değiştirin.

Bu (konuyu uzatmamak amacıyla kırpılmış) gibi bir HTTP 202 yanıt oluşur:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Bu noktada, düzenleme, kuyruğa alınır ve hemen çalışmaya başlar. URL'de `Location` üst bilgi, yürütme durumunu denetlemek için kullanılabilir.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Düzenleme durumu sonucudur. Çalıştırır ve içinde gördüğünüz şekilde hızlı bir şekilde tamamlar *tamamlandı* bu (konuyu uzatmamak amacıyla kırpılmış) benzer bir yanıt durum:

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Gördüğünüz gibi `runtimeStatus` örneğinin *tamamlandı* ve `output` orchestrator işlevi yürütme JSON ile seri hale getirilmiş sonucunu içerir.

> [!NOTE]
> Orchestrator işlev çalışmaya HTTP POST uç nokta örnek uygulamaya bir HTTP olarak uygulanan "HttpStart" adlı bir işlev tetikler. Gibi diğer tetikleyici türleri için aynı başlangıç mantığı uygulayabilir `queueTrigger`, `eventHubTrigger`, veya `timerTrigger`.

İşlev yürütme günlüklerine bakın. `E1_HelloSequence` İşlevi çalışmaya ve açıklanan yeniden yürütme davranışı nedeniyle birden çok kez tamamlandı [genel bakış](durable-functions-concepts.md). Öte yandan, yalnızca üç yürütme vardı `E1_SayHello` olduğundan, bu işlev yürütmelerini değil yeniden.

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyası olarak Visual Studio projesi içinde düzenleme şu şekildedir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, basit bir işlevi zincirleme düzenleme gösterilmiştir. Sonraki örnek fan-dışarı/fan-arada düzeni nasıl uygulayacağınıza karar gösterir.

> [!div class="nextstepaction"]
> [Fan-dışarı/fan-arada örneği çalıştırma](durable-functions-cloud-backup.md)
