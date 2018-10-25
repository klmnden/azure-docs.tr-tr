---
title: Dayanıklı işlevler - Azure özel düzenleme durumu
description: Yapılandırma ve dayanıklı işlevler için özel düzenleme durumu kullanma hakkında bilgi edinin.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: b8017288adb75c990113b0f2ff5ba29a1f1e0a18
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986687"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) özel düzenleme durumu

Özel düzenleme durumu orchestrator işleviniz için bir özel durum değeri ayarlamanıza olanak tanır. Bu durum HTTP GetStatus API aracılığıyla sağlanan veya `DurableOrchestrationClient.GetStatusAsync` API.

> [!NOTE]
> JavaScript için özel düzenleme durumları gelecek bir sürümde kullanılabilir.

## <a name="sample-use-cases"></a>Örnek kullanım durumları 

### <a name="visualize-progress"></a>İlerleme durumunu görselleştirin

İstemciler, durum uç noktası yoklama ve ilerleme durumunu görselleştirir geçerli yürütme aşaması UI görüntüleme. Aşağıdaki örnek paylaşımı ilerleme durumunu gösterir:

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  var outputs = new List<string>();

  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
  context.SetCustomStatus("Tokyo");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
  context.SetCustomStatus("Seattle");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));
  context.SetCustomStatus("London");

  // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
  return outputs;
}

[FunctionName("E1_SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
  return $"Hello {name}!";
}
```

Ve ardından istemci düzenleme çıktısını alır yalnızca `CustomStatus` alan "London" için ayarlanır:

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
  [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
  [OrchestrationClient] DurableOrchestrationClientBase starter,
  string functionName,
  ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    DurableOrchestrationStatus durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    while (durableOrchestrationStatus.CustomStatus.ToString() != "London")
    {
      await Task.Delay(200);
      durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    }

    HttpResponseMessage httpResponseMessage = new HttpResponseMessage(HttpStatusCode.OK)
    {
      Content = new StringContent(JsonConvert.SerializeObject(durableOrchestrationStatus))
    };

    return httpResponseMessage;
  }
}
```

### <a name="output-customization"></a>Çıkış özelleştirme 

Başka bir ilgi çekici senaryo kullanıcıları benzersiz özellikleri veya etkileşimler göre özelleştirilmiş çıkış döndürerek kesimlere. Özel düzenleme durumu yardımıyla, istemci tarafı kod genel olarak kalır. Aşağıdaki örnekte gösterildiği gibi tüm ana değişiklikleri sunucu tarafında gerçekleşir:

```csharp
[FunctionName("CityRecommender")]
public static void Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  int userChoice = context.GetInput<int>();

  switch (userChoice)
  {
    case 1:
    context.SetCustomStatus(new
    {
      recommendedCities = new[] {"Tokyo", "Seattle"},
      recommendedSeasons = new[] {"Spring", "Summer"}
     });
      break;
    case 2:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Seattle, London"},
        recommendedSeasons = new[] {"Summer"}
      });
        break;
      case 3:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Tokyo, London"},
        recommendedSeasons = new[] {"Spring", "Summer"}
      });
        break;
  }

  // Wait for user selection and refine the recommendation
} 
```

### <a name="instruction-specification"></a>Yönergesi belirtimi 

Orchestrator, özel bir durum aracılığıyla istemcilere benzersiz yönergeler sağlayabilir. Özel durum yönergeleri düzenleme kodunda adımlarla eşlenir:

```csharp
[FunctionName("ReserveTicket")]
public static async Task<bool> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  string userId = context.GetInput<string>();

  int discount = await context.CallActivityAsync<int>("CalculateDiscount", userId);

  context.SetCustomStatus(new
  {
    discount = discount,
    discountTimeout = 60,
    bookingUrl = "https://www.myawesomebookingweb.com",
  });

  bool isBookingConfirmed = await context.WaitForExternalEvent<bool>("BookingConfirmed");

  context.SetCustomStatus(isBookingConfirmed
    ? new {message = "Thank you for confirming your booking."}
    : new {message = "The booking was not confirmed on time. Please try again."});

  return isBookingConfirmed;
}
```

## <a name="sample"></a>Örnek

Aşağıdaki örnekte, özel durumu ilk olarak ayarlanır;

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { nextActions = new [] {"A", "B", "C"}, foo = 2, };
    ctx.SetCustomStatus(customStatus);

    // ...do more work...
}
```

Düzenleme devam ederken, dış istemcilere bu özel durum getirebilirsiniz:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

İstemcileri şu yanıtı alırsınız: 

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
>  Bir Azure tablo depolama sütuna sığamayacak kadar olması gerektiğinden, özel durum yükü 16 KB olarak UTF-16 JSON metnini sınırlıdır. Bunlar daha büyük yükü gerekirse geliştiriciler dış depolama kullanabilir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevler HTTP API'leri hakkında bilgi edinin](durable-functions-http-api.md)


