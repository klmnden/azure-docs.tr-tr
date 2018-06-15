---
title: Özel orchestration durum dayanıklı işlevlerinde - Azure
description: Yapılandırma ve özel orchestration durumunu dayanıklı işlevleri için kullanmak hakkında bilgi edinin.
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
ms.date: 04/24/2018
ms.author: azfuncdf
ms.openlocfilehash: 840b96b9cfdb28ca1b17f54698677f4d491342c8
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
ms.locfileid: "32310354"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>Dayanıklı işlevleri (Azure işlevleri) özel düzenleme durumu

Özel orchestration durumu, orchestrator işlevi için bir özel durum değeri ayarlamanıza olanak tanır. Bu durum HTTP GetStatus API üzerinden sağlanır veya `DurableOrchestrationClient.GetStatusAsync` API.

## <a name="sample-use-cases"></a>Örnek kullanım durumları 

### <a name="visualize-progress"></a>İlerlemeyi Görselleştirme

İstemciler durum uç noktası yoklamak ve ilerleme durumunu geçerli yürütme aşaması visualizes UI görüntüleme. Aşağıdaki örnek, ilerleme paylaşımı gösterir:

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

Ve ardından istemci orchestration çıktısını alırsınız yalnızca `CustomStatus` alanını "Londra" ayarlayın:

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

### <a name="output-customization"></a>Çıktı özelleştirme 

Başka bir ilginç senaryo kullanıcılar benzersiz özelliklerine veya etkileşimleri göre özelleştirilmiş çıkış döndürerek kesimlere. Özel orchestration durum yardımıyla, istemci tarafı kodlar genel olarak kalır. Tüm ana değişiklikler, aşağıdaki örnekte gösterildiği gibi sunucu tarafında gerçekleşir:

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

### <a name="instruction-specification"></a>Yönerge belirtimi 

Orchestrator özel durumu aracılığıyla istemcilere benzersiz yönergeleri sağlar. Özel durum yönergeleri orchestration kod adımlarına eşlenecek:

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

Aşağıdaki örnekte, özel durum önce ayarlanır;

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

Orchestration çalışırken, dış istemcilere bu özel durum getirebilirsiniz:

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
>  Bir Azure Table Storage sütuna sığmayacak kadar gerektiği için özel durum yükü UTF-16 JSON metnin 16 KB ile sınırlıdır. Daha büyük yük gerekiyorsa geliştiriciler harici depolama kullanabilir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [HTTP API'leri dayanıklı işlevleri hakkında bilgi edinin](durable-functions-http-api.md)


