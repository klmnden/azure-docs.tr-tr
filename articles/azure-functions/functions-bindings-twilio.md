---
title: Azure işlevleri Twilio bağlama
description: Azure işlevleri ile Twilio bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d973e4003f8501fb3b4d1b461703bd0b869c205
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669163"
---
# <a name="twilio-binding-for-azure-functions"></a>Azure işlevleri için Twilio bağlama

Bu makalede kullanarak kısa mesaj göndermek açıklanmaktadır [Twilio](https://www.twilio.com/) Azure işlevleri'nde bağlar. Azure işlevleri destekler çıkış bağlamaları için Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Twilio bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.Twilio/) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Twilio bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example---functions-1x"></a>Örnek - işlev 1.x

Dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betiği (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir kuyruk iletisi tarafından tetiklendiğinde bir kısa mesaj gönderir.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

Bu örnekte `TwilioSms` yöntemi dönüş değerine sahip öznitelik. Özniteliğiyle kullanmaya alternatiftir bir `out SMSMessage` parametresi veya `ICollector<SMSMessage>` veya `IAsyncCollector<SMSMessage>` parametresi.

### <a name="c-script-example"></a>C# betiği örneği

Aşağıdaki örnek, bağlama bir Twilio çıktı gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevini kullanan bir `out` kısa mesaj göndermek için parametre.

Veri bağlama işte *function.json* dosyası:

Örnek function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

C# betik kodunu şu şekildedir:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Out Parametreleri zaman uyumsuz kod kullanamazsınız. Zaman uyumsuz C# betik kodunu örneği aşağıda verilmiştir:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bağlama bir Twilio çıktı gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan.

Veri bağlama işte *function.json* dosyası:

Örnek function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="example---functions-2x"></a>Örnek - işlev 2.x

Dile özgü örneğe bakın:

* [2.x C#](#2x-c-example)
* [2.x C# betiği (.csx)](#2x-c-script-example)
* [2.x JavaScript](#2x-javascript-example)

### <a name="2x-c-example"></a>2.x C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir kuyruk iletisi tarafından tetiklendiğinde bir kısa mesaj gönderir.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static CreateMessageOptions Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new CreateMessageOptions(new PhoneNumber(order["mobileNumber"].ToString()))
    {
        Body = $"Hello {order["name"]}, thanks for your order!"
    };

    return message;
}
```

Bu örnekte `TwilioSms` yöntemi dönüş değerine sahip öznitelik. Özniteliğiyle kullanmaya alternatiftir bir `out CreateMessageOptions` parametresi veya `ICollector<CreateMessageOptions>` veya `IAsyncCollector<CreateMessageOptions>` parametresi.

### <a name="2x-c-script-example"></a>2.x C# betiği örneği

Aşağıdaki örnek, bağlama bir Twilio çıktı gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevini kullanan bir `out` kısa mesaj göndermek için parametre.

Veri bağlama işte *function.json* dosyası:

Örnek function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

C# betik kodunu şu şekildedir:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static void Run(string myQueueItem, out CreateMessageOptions message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the CreateMessageOptions variable.
    message = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Out Parametreleri zaman uyumsuz kod kullanamazsınız. Zaman uyumsuz C# betik kodunu örneği aşağıda verilmiştir:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static async Task Run(string myQueueItem, IAsyncCollector<CreateMessageOptions> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the CreateMessageOptions variable.
    CreateMessageOptions smsText = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="2x-javascript-example"></a>2.x JavaScript örneği

Aşağıdaki örnek, bağlama bir Twilio çıktı gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan.

Veri bağlama işte *function.json* dosyası:

Örnek function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) özniteliği.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz. [yapılandırma](#configuration). İşte bir `TwilioSms` özniteliği örnek bir yöntem imzası:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(
    AccountSidSetting = "TwilioAccountSid", 
    AuthTokenSetting = "TwilioAuthToken", 
    From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    ...
}
 ```

Tam bir örnek için bkz. [C# örneği](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `TwilioSms` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**|| Ayarlanmalıdır `twilioSms`.|
|**direction**|| Ayarlanmalıdır `out`.|
|**Adı**|| İşlev kodu için Twilio SMS mesajı kullanılan değişken adı. |
|**accountSid**|**accountSid**| Bu değer, Twilio hesap SID'si tutan bir uygulama ayarı adı için ayarlamanız gerekir.|
|**authToken**|**AuthToken**| Bu değer, Twilio kimlik doğrulama belirtecinizi içeren uygulama ayarı adı için ayarlamanız gerekir.|
|**Hedef**|**Alıcı**| Bu değer, telefon numarasına gönderilen SMS metni ayarlanır.|
|**Kaynak**|**Kaynak**| SMS metni gönderildiği telefon numarası için bu değeri ayarlayın.|
|**Gövde**|**Gövde**| Bu değer, işleviniz için kodda dinamik olarak ayarlamak gerekmiyorsa, SMS mesajı sabit kod için kullanılabilir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)