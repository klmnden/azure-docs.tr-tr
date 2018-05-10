---
title: Azure dayanıklı işlevleri birim testi
description: Bilgi nasıl dayanıklı işlevleri için birim testi.
services: functions
author: kadimitr
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/28/2018
ms.author: kadimitr
ms.openlocfilehash: 7de9a6f0d4dfcb45932b89504c0d38c3c70283e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="durable-functions-unit-testing"></a>Dayanıklı işlevleri birim testi

Birim testi modern yazılım geliştirme uygulamalarını önemli bir parçasıdır. Birim testleri iş mantığı davranışını doğrulayabilir ve gözden kaçan önemli değişiklikler gelecekte Tanıtımı koruyabilirsiniz. Giriş birim testleri önemli değişiklikler önlemek için yardımcı olacak şekilde dayanıklı işlevleri kolayca karmaşıklığı büyüyebilir. Aşağıdaki bölümlerde açıklanmıştır birimine üç işlev türleri - Orchestration istemci, Orchestrator ve etkinliği test nasıl işlevleri. 

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örneklerde, aşağıdaki kavramlar ve çerçeveleri bilgi gerektirir: 

* Birim testi

* Dayanıklı İşlevler 

* [xUnit](https://xunit.github.io/) -test çerçevesi

* [moq](https://github.com/moq/moq4) -framework Mocking


## <a name="base-classes-for-mocking"></a>Mocking için temel sınıflar 

Mocking dayanıklı işlevlerinde iki soyut sınıflar aracılığıyla desteklenir:

* [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) 

* [DurableOrchestrationContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContextBase.html)

Temel sınıflar için bu sınıflardır [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) ve [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) Orchestration istemci ve Orchestrator yöntemleri tanımlar. Birim testi iş mantığı doğrulayabilmeniz için mocks taban sınıf yöntemlerini için beklenen bir davranış ayarlayın. Birim Orchestration istemci ve Orchestrator iş mantığı testi için iki aşamalı iş akışı vardır:

1. Temel sınıflar, Orchestration istemci ve Orchestrator'ın imzaları tanımlarken yerine somut uygulaması kullanın.
2. Birim testleri temel sınıflar davranışını mock ve iş mantığı doğrulayın. 

Test etmek için aşağıdaki paragrafta diğer ayrıntıları bulmak bağlama orchestration istemci ve orchestrator kullanan işlevler bağlama tetikler.

## <a name="unit-testing-trigger-functions"></a>Birim testi tetikleyici işlevleri

Bu bölümde, yeni düzenlemelerin başlatmak için aşağıdaki HTTP tetikleyicisi işlevi mantıksal birim testi doğrular.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

Birim testi değerini doğrulamak için bu durumda görev `Retry-After` yanıt yükünde sağlanan üstbilgi. Birim testi bazı mock şekilde [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) tahmin edilebilir bir davranış sağlamak için yöntemleri. 

İlk olarak, temel sınıfın mock gereklidir [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). Mock uygulayan yeni bir sınıf olabilir [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). Ancak, bir mocking framework gibi kullanılarak [moq](https://github.com/moq/moq4) işlemini basitleştirir:    

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Ardından `StartNewAsync` yöntemi mocked iyi bilinen örneği bir kimlik döndürmek için

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Sonraki `CreateCheckStatusResponse` boş bir HTTP 200 yanıtı her zaman mocked iade değil.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
        });
```

`TraceWriter` Ayrıca mocked:

```csharp
    // Mock TraceWriter
    var traceWriterMock = new Mock<TraceWriter>(TraceLevel.Info);

```  

Şimdi `Run` yöntemi, birim testi çağrılır:

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

 Çıktı beklenen değerle karşılaştırmak için son adımdır bakın:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Birim testi tüm adımları birleştirme sonra aşağıdaki kodu olacaktır: 

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Birim testi orchestrator işlevleri

Orchestrator işlevleri için birim genellikle çok daha fazla iş mantığı olduğundan testi daha ilginç.

Bu bölümde birim testleri çıktısını doğrulayacak `E1_HelloSequence` Orchestrator işlevi:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Birim testi kodunu bir mock oluşturma ile başlar:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Ardından etkinlik yöntem çağrılarını mocked:

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

Birim testi tüm adımları birleştirme sonra aşağıdaki kodu olacaktır:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Birim testi etkinlik işlevleri

Etkinlik işlevleri dayanıklı olmayan işlevleri aynı şekilde test birim olabilir. Etkinlik işlevleri mocking için bir taban sınıfı yok. Birim testleri parametre türleri doğrudan kullanın.

Bu bölümde birim testi davranışını doğrulayacak `E1_SayHello` etkinlik işlevi:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Ve birim testi çıktı biçimi doğrular:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [XUnit hakkında daha fazla bilgi edinin](http://xunit.github.io/docs/getting-started-dotnet-core)

> [Moq hakkında daha fazla bilgi edinin](https://github.com/Moq/moq4/wiki/Quickstart)