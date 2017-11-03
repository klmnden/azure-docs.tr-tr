---
title: "Azure işlevleri Twilio bağlama | Microsoft Docs"
description: "Azure işlevleriyle Twilio bağlamaları kullanmayı öğrenme."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8c5e8f2dfedae26486e1c8afbe0cec3f3228e86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a>SMS gönder İleti Twilio kullanarak Azure işlevlerden çıkışı bağlama
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede, yapılandırma ve Azure işlevleriyle Twilio bağlamaları kullanma açıklanmaktadır. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure işlevleri destekler Twilio çıktı birkaç satır kod ile SMS iletileri göndermek işlevlerinizi etkinleştirmek için bağlamalar ve [Twilio](https://www.twilio.com/) hesabı. 

## <a name="functionjson-for-the-twilio-output-binding"></a>bağlama Function.JSON Twilio için çıktı
Function.json dosyası aşağıdaki özellikleri sağlar:

|Özellik  |Açıklama  |
|---------|---------|
|**adı**| İşlev kodu Twilio SMS metin iletisi için kullanılan değişken adı. |
|**türü**| ayarlanmalıdır `twilioSms`.|
|**accountSid**| Bu değer, Twilio hesabının SID tutan bir uygulama ayarı adı için ayarlamanız gerekir.|
|**authToken**| Bu değer, Twilio kimlik doğrulaması belirtecine sahip bir uygulama ayarı adı için ayarlamanız gerekir.|
|**Hedef**| SMS metni gönderilir telefon numarası için bu değeri ayarlayın.|
|**Kaynak**| SMS metin gönderilen telefon numarası için bu değeri ayarlayın.|
|**yönü**| ayarlanmalıdır `out`.|
|**Gövde**| Bu değer, dinamik olarak işlevinizi kodunda belirlemek ihtiyaç duymuyorsanız SMS mesajı sabit kod için kullanılabilir. |

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Örnek C# sıra tetikleyicisi Twilio ile bağlama çıktı
#### <a name="synchronous"></a>Zaman uyumlu
Bir Azure depolama kuyruğu tetikleyicisi için bu zaman uyumlu örnek kod bir out parametresi bir siparişin bir müşteri için bir kısa mesaj göndermek için kullanır.

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

#### <a name="asynchronous"></a>Zaman uyumsuz
Bir Azure depolama kuyruğu tetikleyicisi için bu zaman uyumsuz örnek kod bir siparişin bir müşteri için bir kısa mesaj gönderir.

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

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Bağlama örnek Node.js sıra tetikleyicisi Twilio ile çıktı
Bu Node.js örnek bir siparişin bir müşteri için bir kısa mesaj gönderir.

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

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

