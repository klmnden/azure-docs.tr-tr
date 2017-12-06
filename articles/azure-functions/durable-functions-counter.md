---
title: "Durum bilgisi olan teklileri dayanıklı işlevlerinde - Azure"
description: "Durum bilgisi olan tek Azure işlevleri için dayanıklı işlevleri uzantısı'nda uygulama hakkında bilgi edinin."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 05099e868e62f612be0a3354eb8b339507ac7e4a
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="stateful-singletons-in-durable-functions---counter-sample"></a>Durum bilgisi olan teklileri dayanıklı işlevlerinde - sayaç örneği

Durum bilgisi olan teklileri durumunu depolamak ve çağrılır ve diğer işlevleri tarafından sorgulanan uzun süre çalışan (büyük olasılıkla eternal) orchestrator işlevlerdir. Durum bilgisi olan teklileri benzer [aktör modeli](https://en.wikipedia.org/wiki/Actor_model) dağıtılmış bilgi işlem.

Değil bir uygun "Aktör" uygulamasına, orchestrator işlevleri birçok aynı çalışma zamanı özelliklerine sahiptir. Örneğin, bunlar durum bilgisi olan, güvenilir, tek iş parçacıklı, konum saydam ve genel olarak adreslenebilir. Özellikle yararlı ancak ayrı bir çerçeve gerek kalmadan gerçek aktör uygulamalarına olun özellikleri şunlardır.

Bu makalede nasıl çalıştırılacağı gösterilmektedir *sayaç* örnek. Örnek destekleyen bir singleton nesne gösterir *artırma* ve *azaltma* işlemleri ve iç durumuna uygun şekilde güncelleştirir.

## <a name="prerequisites"></a>Ön koşullar

* ' Ndaki yönergeleri izleyin [yükleme dayanıklı işlevleri](durable-functions-install.md) örneğini kurmak için.
* Bu makalede, zaten gitti varsayar [Hello dizisi](durable-functions-sequence.md) örnek gözden geçirme.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Sayaç senaryo normal durum bilgisiz işlevler kullanılarak uygulamak şaşırtıcı zordur. Sahip olduğunuz ana zorluklarından yönetirken **eşzamanlılık**. Operations ister *artırma* ve *azaltma* atomik olmasına gerek veya başka olabilir birbirinin üzerine işlemlerinin neden yarış durumları.

Sayaç verileri barındırmak için tek bir VM'YE'ni kullanarak bir seçenek değildir, ancak bu pahalı ve yönetme **güvenilirlik** tek bir VM'ye düzenli aralıklarla yeniden başlatılabilir beri zor olabilir. Alternatif olarak dağıtılmış bir platform ile eşitleme kullanabileceğinizi eşzamanlılık yönetmenize yardımcı olmak için blob kira araçları gibi çalışır, ancak bu durum önemli miktarda **karmaşıklık**.

Dayanıklı işlevleri yapar bu tür bir senaryo Önemsiz orchestration örnekleri için tek bir VM'ye Benzetilen ve orchestrator işlevi yürütme her zaman tek iş parçacıklı olmadığından uygulamak için. Yalnızca, ancak bunlar uzun süre çalışan, durum bilgisi olan ve dış olaylarına tepki. Aşağıdaki örnek kod, bu tür bir sayaç uzun süre çalışan orchestrator işlevi olarak uygulamak gösterilmiştir.

## <a name="the-sample-function"></a>Örnek işlevi

Bu makalede kılavuzluk **E3_Counter** işlevi için örnek uygulama.



## <a name="the-counter-orchestration"></a>Sayaç düzenleme

Aşağıdaki bölümlerde, Visual Studio Code ve Azure Portal geliştirme için kullanılan kod açıklanmaktadır.

### <a name="c-script"></a>C# betiği

Function.json dosyası:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Counter/function.json)]

Run.csx dosyası:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Counter/run.csx)]

### <a name="precompiled-c"></a>Önceden derlenmiş C# 

Aşağıdaki bölümlerde, Visual Studio geliştirme için kullanılan kod açıklanmaktadır.

Orchestrator işlevi uygulayan kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Counter.cs)]

### <a name="explanation-of-the-code"></a>Kod açıklaması

Bu orchestrator işlevi temelde şunları yapar:

1. Adlı bir dış olayını dinler *işlemi* kullanarak [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_).
2. Artırır veya azaltır `counterState` İstenen işleme bağlı olarak yerel değişken.
3. Kullanarak orchestrator yeniden [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) yöntemi, en son değerini ayarlama `counterState` yeni giriş olarak.
4. Devamlı veya kadar çalışmaya devam eder bir *son* iletisi alındığında.

Bu bir örnektir bir *eternal orchestration* &mdash; diğer bir deyişle, bir olası asla sona eren. Tarafından gönderilen iletileri yanıtlar [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) tüm orchestrator olmayan işlev tarafından çağrılan yöntem.

Bir benzersiz bu orchestrator işlevi özelliğidir etkili bir şekilde geçmiş olduğunu: `ContinueAsNew` yöntemi her işlenen olayından sonra geçmişini sıfırlar. Bu, rasgele bir yaşam süresi olan bir orchestrator uygulamak için tercih edilen yoludur. Kullanarak bir `while` döngü orchestrator işlevin geçmişi, gereksiz yere yüksek bellek kullanımında kaynaklanan, sınırsız büyümesine neden olabilir.

> [!NOTE]
> `ContinueAsNew` Yöntemi eternal düzenlemelerin yanı sıra diğer kullanım örnekleri sahiptir. Daha fazla bilgi için bkz: [Eternal düzenlemelerin](durable-functions-eternal-orchestrations.md).

## <a name="run-the-sample"></a>Örnek çalıştırın

Aşağıdaki HTTP POST isteği göndererek orchestration başlatabilirsiniz. İzin vermek için `counterState` sıfırda başlatmak için (varsayılan değeri `int`), bu istekte içerik yok.

```
POST http://{host}/orchestrators/E3_Counter
Content-Length: 0
```

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{
  "id":"bcf6fb5067b046fbb021b52ba7deae5a",
  "statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}",
  "sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}",
  "terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

**E3_Counter** örneği başlatır ve onu kullanarak gönderilmek üzere bir olay için hemen bekler `RaiseEventAsync` veya kullanarak **sendEventUrl** 202 yanıt olarak başvurulan HTTP POST Web kancası. Geçerli `eventName` değerler *incr*, *decr*, ve *son*.

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Type: application/json
Content-Length: 6

"incr"
```

Azure işlevleri portalındaki işlevi günlüklerine bakarak "incr" işleminin sonuçlarını görebilirsiniz.

```
2017-06-29T18:54:53.998 Function started (Id=34e34a61-38b3-4eac-b6e2-98b85e32eec8)
2017-06-29T18:54:53.998 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Function started (Id=b45d6c2f-39f3-42a2-b904-7761b2614232)
2017-06-29T18:58:01.458 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Received 'incr' operation.
2017-06-29T18:58:01.458 Function completed (Success, Id=b45d6c2f-39f3-42a2-b904-7761b2614232, Duration=8ms)
2017-06-29T18:58:11.518 Function started (Id=e1f38cb2-546a-404d-ab22-1ac8f81a93d9)
2017-06-29T18:58:11.518 Current counter state is 1. Waiting for next operation.
```

Benzer şekilde, orchestrator durumunu denetlemek, gördüğünüz `input` alan güncelleştirilmiş değeri (1) ayarlandı.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey} HTTP/1.1
```

```
HTTP/1.1 202 Accepted
Content-Length: 129
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":1,"output":null,"createdTime":"2017-06-29T18:58:01Z","lastUpdatedTime":"2017-06-29T18:58:11Z"}
```

Bu örnek için yeni işlemleri göndermeye devam ve durumu buna göre güncelleştirilir gözlemleyin. Örnek KILL istiyorsanız, bunu göndererek yapabileceğiniz bir *son* işlemi.

> [!WARNING]
> Yazma zaman yarış durumları çağrılırken bilinen vardır `ContinueAsNew` dış olayları veya sonlandırma istekleri gibi iletileri aynı anda işlerken. Bu yarış durumları yönelik en son bilgiler için bkz [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/67).

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek nasıl ele alınacağını gösterilmiştir [dış olayları](durable-functions-external-events.md) ve uygulamanıza [eternal düzenlemelerin](durable-functions-eternal-orchestrations.md) içinde [durum bilgisi olan teklileri](durable-functions-singletons.md). Sonraki örnek dış olayları kullanmayı gösterir ve [dayanıklı zamanlayıcılar](durable-functions-timers.md) insan etkileşimi işlemek için.

> [!div class="nextstepaction"]
> [İnsan etkileşimi örneğini çalıştırın](durable-functions-phone-verification.md)
