---
title: "Azure işlevleri Twilio bağlama"
description: "Azure işlevleriyle Twilio bağlamaları kullanmayı öğrenme."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff31f8b265452b6864e36323e770f808f87de019
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="twilio-binding-for-azure-functions"></a>Azure işlevleri için Twilio bağlama

Bu makalede kullanarak metin iletileri göndermek nasıl açıklanmaktadır [Twilio](https://www.twilio.com/) Azure işlevlerinde bağlar. Azure işlevleri destekler çıktı için bağlamaları Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Paketler

Twilio bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet paketi. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/) GitHub depo.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Örnek

Dile özgü örneğe bakın:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C# örnek

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

Bu örnekte `TwilioSms` özniteliği yönteminin dönüş değeri ile. Öznitelik ile kullanmak için bir alternatif olan bir `out SMSMessage` parametresi ya da bir `ICollector<SMSMessage>` veya `IAsyncCollector<SMSMessage>` parametresi.

### <a name="c-script-example"></a>C# kod örneği

Aşağıdaki örnek, bağlama Twilio çıkış gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlevini kullanan bir `out` bir kısa mesaj göndermek için parametre.

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

C# kod şöyledir:

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

Out Parametreleri zaman uyumsuz kodda kullanamazsınız. Zaman uyumsuz C# betik kodu örneği şöyledir:

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

Aşağıdaki örnek, bağlama Twilio çıkış gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır.

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

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz: [yapılandırma](#configuration). Burada bir `TwilioSms` yöntemi imza özniteliği örnekte:

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

Tam bir örnek için bkz: [C# örnek](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `TwilioSms` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**|| ayarlanmalıdır `twilioSms`.|
|**Yönü**|| ayarlanmalıdır `out`.|
|**Adı**|| İşlev kodu Twilio SMS metin iletisi için kullanılan değişken adı. |
|**accountSid**|**AccountSid**| Bu değer, Twilio hesabının SID tutan bir uygulama ayarı adı için ayarlamanız gerekir.|
|**authToken**|**AuthToken**| Bu değer, Twilio kimlik doğrulaması belirtecine sahip bir uygulama ayarı adı için ayarlamanız gerekir.|
|**Hedef**|**Alıcı**| SMS metni gönderilir telefon numarası için bu değeri ayarlayın.|
|**Kaynak**|**Kaynak**| SMS metin gönderilen telefon numarası için bu değeri ayarlayın.|
|**Gövde**|**Gövde**| Bu değer, dinamik olarak işlevinizi kodunda belirlemek ihtiyaç duymuyorsanız SMS mesajı sabit kod için kullanılabilir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)


