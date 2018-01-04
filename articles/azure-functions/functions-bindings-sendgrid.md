---
title: "Azure işlevleri SendGrid bağlamaları"
description: "Azure işlevleri SendGrid bağlamaları başvurusu."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/29/2017
ms.author: tdykstra
ms.openlocfilehash: aa64bbc764fe96d7c15d62660114222458e5acf4
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure işlevleri SendGrid bağlamaları

Bu makalede kullanarak e-posta göndermek nasıl açıklanmaktadır [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) Azure işlevlerinde bağlar. Azure işlevleri için SendGrid bir çıktı bağlamayı destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Örnek

Dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betik (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) kullanan Service Bus kuyruğuna tetikler ve bir SendGrid çıkış bağlama.

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

Özniteliğin ayarı atlayabilirsiniz `ApiKey` "AzureWebJobsSendGridApiKey" adlı bir uygulama ayarı API anahtarınıza varsa özelliği.

### <a name="c-script-example"></a>C# kod örneği

Aşağıdaki örnek, bağlama SendGrid çıkış gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır.

Veri bağlama işte *function.json* dosyası:

```json 
{
    "bindings": [
        {
            "name": "message",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bağlama SendGrid çıkış gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır.

Veri bağlama işte *function.json* dosyası:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid).

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz: [yapılandırma](#configuration). Burada bir `SendGrid` yöntemi imza özniteliği örnekte:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Tam bir örnek için bkz: [C# örnek](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `SendGrid` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü**|| Gerekli - kümesine olmalıdır `sendGrid`.|
|**yönü**|| Gerekli - kümesine olmalıdır `out`.|
|**adı**|| Gerekli - istek veya istek gövdesi için işlevi kod içinde kullanılan değişken adı. Bu değer ```$return``` yalnızca bir dönüş değeri olduğunda. |
|**apikey ile yapılan**|**Apikey ile yapılan**| API anahtarınızı içeren bir uygulama ayarı adı. Ayarlanmazsa, varsayılan uygulama ayarı adı "AzureWebJobsSendGridApiKey" dir.|
|**Hedef**|**Alıcı**| Alıcının e-posta adresi. |
|**Kaynak**|**Kaynak**| Gönderenin e-posta adresi. |
|**Konu**|**Konu**| e-postanın konusu. |
|**metin**|**Metin**| e-posta içeriği. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)