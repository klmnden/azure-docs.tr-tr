---
title: Azure dayanıklı işlevler birim testi
description: Bilgi nasıl dayanıklı işlevler için birim test.
services: functions
author: kadimitr
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: kadimitr
ms.openlocfilehash: 69cf91f1448e36353f83de7a271abb3b53858bb0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60648474"
---
# <a name="durable-functions-unit-testing"></a>Dayanıklı işlevler birim testi

Birim testi, modern yazılım geliştirme yöntemleri önemli bir parçasıdır. Birim testleri, iş mantığı davranışı doğrulayın ve gelecekte gözden kaçan bozucu değişiklikleri giriş koruyun. Karşınızda birim testleri bozucu değişiklikler önlemeye yardımcı şekilde dayanıklı işlevler kolayca karmaşık hale gelmesi. Aşağıdaki bölümlerde açıklanmaktadır nasıl üç işlev türleri - düzenleme istemcisi, Orchestrator ve etkinlik için birim test işlevleri.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örneklerde aşağıdaki kavramlar ve çerçeveleri gerektirir:

* Birim testi

* Dayanıklı İşlevler

* [xUnit](https://xunit.github.io/) -test çerçevesi

* [moq](https://github.com/moq/moq4) -framework sahte işlem

## <a name="base-classes-for-mocking"></a>Sahte işlem için temel sınıflar

Sahte işlem üç soyut sınıf dayanıklı işlevler aracılığıyla desteklenir:

* [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html)

* [DurableOrchestrationContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContextBase.html)

* [DurableActivityContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContextBase.html)

Bu sınıflar için temel sınıflardır [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html), [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html), ve [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) düzenleme istemcisi tanımlayın , Orchestrator ve etkinlik yöntemleri. Birim testi iş mantığı doğrulayabilmeniz için taban sınıf yöntemlerini yönelik beklenen davranışın mocks ayarlar. İş mantığı düzenleme istemcisi ve Orchestrator birim testi için iki aşamalı iş akışı şöyledir:

1. Düzenleme istemcisi ve Orchestrator'ın imzaları tanımlarken, temel sınıflar somut bir uygulama yerine kullanın.
2. Birim testleri temel sınıflar davranışını Sahne ve iş mantığı doğrulayın.

Test etmek için aşağıdaki paragrafta diğer ayrıntıları öğrenmek bağlama düzenleme istemcisi ve orchestrator'ı kullanan işlevler bağlama tetikleyin.

## <a name="unit-testing-trigger-functions"></a>Birim test tetikleyici işlevleri

Bu bölümde, yeni düzenlemeleri başlatmak için aşağıdaki HTTP tetikleyici işlevi, mantıksal birim testi doğrular.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

Birim test değerini doğrulamak için bu durumda görev `Retry-After` yanıt yükünde sağlanan üstbilgisi. Birim testi bazı sahte şekilde [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) tahmin edilebilir davranış sağlamak için yöntemleri.

İlk olarak, temel sınıfın sahte gereklidir [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). Sahte uygulayan yeni bir sınıf olması [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). Ancak, gibi sahte bir çerçeve kullanarak [moq](https://github.com/moq/moq4) bu süreci kolaylaştırır:

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Ardından `StartNewAsync` yöntemi örnek bir bilinen örneği kimliği döndürmek için

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Sonraki `CreateCheckStatusResponse` sahte her zaman döndürülecek olan boş bir HTTP 200 yanıtı.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
            Headers =
            {
                RetryAfter = new RetryConditionHeaderValue(TimeSpan.FromSeconds(10))
            }
        });
```

`TraceWriter` Ayrıca örnek:

```csharp
    // Mock TraceWriter
    var traceWriterMock = new Mock<TraceWriter>(TraceLevel.Info);

```  

Artık `Run` yöntemi, birim testi çağrılır:

```csharp
    // Call Orchestration trigger function
    var result = await HttpStart.Run(
        new HttpRequestMessage()
        {
            Content = new StringContent("{}", Encoding.UTF8, "application/json"),
            RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
        },
        durableOrchestrationClientBaseMock.Object,
        functionName,
        traceWriterMock.Object);
 ```

 Son adım, çıktı beklenen değeri ile Karşılaştırılacak içerir:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Tüm adımları birleştirdikten sonra birim testini şu kodu görürsünüz:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Birim testi orchestrator işlevleri

Orchestrator işlevleri birim genellikle çok daha fazla iş mantığı olduğundan test etmek daha da ilginç.

Bu bölümde birim testleri çıktısını doğrulayacaktır `E1_HelloSequence` Orchestrator işlevi:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Birim testi kodu bir Sahne oluşturma ile başlar:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Ardından etkinlik yöntem çağrıları örnek:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Birim testi sonraki çağıracak `HelloSequence.Run` yöntemi:

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

Ve son olarak çıkış doğrulanacak:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Tüm adımları birleştirdikten sonra birim testini şu kodu görürsünüz:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Birim test etkinliği işlevleri

Etkinlik işlevlerini dayanıklı olmayan işlevler aynı şekilde test birimi olabilir.

Bu bölümde, birim testi davranışını doğrulayacaktır `E1_SayHello` etkinlik işlevi:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Ve birim testlerini çıkış biçimini doğrular. Birim testlerini parametre türleri, doğrudan veya sahte kullanabilirsiniz [DurableActivityContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContextBase.html) sınıfı:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [XUnit hakkında daha fazla bilgi edinin](https://xunit.github.io/docs/getting-started-dotnet-core)
> 
> [Moq hakkında daha fazla bilgi edinin](https://github.com/Moq/moq4/wiki/Quickstart)
