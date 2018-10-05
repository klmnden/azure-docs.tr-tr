---
title: Azure SignalR hizmeti işlevleri bağlamaları
description: Azure işlevleri ile SignalR hizmet bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: anthonychu
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/23/2018
ms.author: antchu
ms.openlocfilehash: 2892481dca9ce62d96e954656341925b4c8110f9
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802024"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Azure İşlevleri için SignalR Service bağlamaları

Bu makalede, kimliğini doğrulamak ve istemcilere bağlı gerçek zamanlı iletileri göndermek açıklanmaktadır [Azure SignalR hizmeti](https://azure.microsoft.com/services/signalr-service/) Azure işlevleri'nde SignalR hizmet bağlamaları kullanarak. Giriş ve çıkış bağlamaları SignalR hizmeti için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

SignalR hizmet bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.SignalRService](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) NuGet paketi sürüm 1.0.0-preview1-*. Paket için kaynak kodu konusu [azure işlevleri signalrservice uzantı](https://github.com/Azure/azure-functions-signalrservice-extension) GitHub deposu.

> [!NOTE]
> Azure SignalR hizmeti genel kullanıma sunulmuştur. Ancak, Azure işlevleri için SignalR hizmet bağlamaları, şu anda Önizleme aşamasındadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]

## <a name="signalr-connection-info-input-binding"></a>SignalR bağlantı bilgisi giriş bağlama

Azure SignalR hizmeti için bir istemci bağlanabilmeleri için hizmet uç noktası URL'sini ve geçerli bir erişim belirteci alması gerekir. *SignalRConnectionInfo* giriş bağlama SignalR Hizmeti uç nokta URL'nizi ve hizmete bağlanmak için kullanılan geçerli bir belirteç oluşturur. Belirteç süre sınırlı ve bağlantı belirli bir kullanıcının kimliğini doğrulamak için kullanılan belirteç önbelleğe veya gerekir istemciler arasında paylaşın. Bu bağlama kullanarak HTTP tetikleyicisi, bağlantı bilgilerini almak için istemciler tarafından kullanılabilir.

Dile özgü örneğe bakın:

* [2.x C#](#2x-c-input-example)
* [2.x JavaScript](#2x-javascript-input-example)

### <a name="2x-c-input-example"></a>Giriş 2.x C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) giriş bağlamasına kullanarak SignalR bağlantı bilgilerini alır ve HTTP üzerinden döndürür.

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçleri

Kimliği doğrulanmış bir istemci tarafından tetiklenen işlev ise oluşturulan belirteç için bir kullanıcı kimliği talebi ekleyebilirsiniz. [App Service kimlik doğrulaması] kullanarak bir işlev uygulaması için kolayca kimlik doğrulaması ekleyebilirsiniz (.. /App-Service/App-Service-Authentication-Overview.MD).

App Service kimlik doğrulaması adlı HTTP üstbilgileri ayarlar `x-ms-client-principal-id` ve `x-ms-client-principal-name` içeren kimliği doğrulanmış kullanıcının asıl istemci kimliği ve adı, sırasıyla. Ayarlayabileceğiniz `UserId` özelliğini kullanarak ya da üst bilgi değeri bağlamanın bir [ifade bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns): `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-example"></a>2.x JavaScript giriş örneği

Aşağıdaki örnek, bir SignalR bağlantı bilgisi giriş bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlantı bilgilerini döndürmek için bağlama kullanan.

Veri bağlama işte *function.json* dosyası:

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req, connectionInfo) {
    context.res = { body: connectionInfo };
    context.done();
};
```

#### <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçleri

Kimliği doğrulanmış bir istemci tarafından tetiklenen işlev ise oluşturulan belirteç için bir kullanıcı kimliği talebi ekleyebilirsiniz. [App Service kimlik doğrulaması] kullanarak bir işlev uygulaması için kolayca kimlik doğrulaması ekleyebilirsiniz (.. /App-Service/App-Service-Authentication-Overview.MD).

App Service kimlik doğrulaması adlı HTTP üstbilgileri ayarlar `x-ms-client-principal-id` ve `x-ms-client-principal-name` içeren kimliği doğrulanmış kullanıcının asıl istemci kimliği ve adı, sırasıyla. Ayarlayabileceğiniz `userId` özelliğini kullanarak ya da üst bilgi değeri bağlamanın bir [ifade bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns): `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`. 

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier 
    // claim set to the authenticated user
    context.res = { body: connectionInfo };
    context.done();
};
```

## <a name="signalr-output-binding"></a>SignalR çıktı bağlaması

Kullanım *SignalR* çıktı bağlaması Azure SignalR hizmeti kullanarak bir veya daha fazla ileti göndermek için. Bir ileti bağlanan tüm istemciler için yayın veya yalnızca kimliği doğrulanmış belirli bir kullanıcıya bağlı istemciler için yayın.

Dile özgü örneğe bakın:

* [2.x C#](#2x-c-output-example)
* [2.x JavaScript](#2x-javascript-output-example)

### <a name="2x-c-output-example"></a>C# 2.x çıkış örneği

#### <a name="broadcast-to-all-clients"></a>Tüm istemcilere yayın

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bağlanan tüm istemciler için çıktı bağlama kullanarak bir ileti gönderir. `Target` Her istemcide çağrılacak yöntemin adı. `Arguments` Özelliği istemci yöntemine geçirilecek sıfır veya daha fazla nesne bir dizisidir.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

#### <a name="send-to-a-user"></a>Bir kullanıcıya Gönder

Bir kullanıcıya ayarlayarak doğrulanan bağlantılar için bir ileti gönderebilir `UserId` özelliği SignalR iletisi.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

### <a name="2x-javascript-output-example"></a>2.x JavaScript çıktı örneği

#### <a name="broadcast-to-all-clients"></a>Tüm istemcilere yayın

Aşağıdaki örnek, bağlama bir SignalR çıkış gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) Azure SignalR hizmeti içeren bir ileti göndermek için bağlama kullanan. Çıkış bağlaması bir veya daha fazla SignalR ileti dizisi olarak ayarlayın. SignalR ileti oluşan bir `target` her istemcide çağrılacak yöntemin adını belirleyen özellik ve `arguments` bağımsız değişken olarak istemci yöntemine geçirilecek nesneleri içeren bir dizi özelliği.

Veri bağlama işte *function.json* dosyası:

Örnek function.json:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

#### <a name="send-to-a-user"></a>Bir kullanıcıya Gönder

Bir kullanıcıya ayarlayarak doğrulanan bağlantılar için bir ileti gönderebilir `userId` özelliği SignalR iletisi.

*Function.JSON* aynı kalır. JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to these user IDs
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

## <a name="configuration"></a>Yapılandırma

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `SignalRConnectionInfo` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**|| Ayarlanmalıdır `signalRConnectionInfo`.|
|**direction**|| Ayarlanmalıdır `in`.|
|**Adı**|| İşlev kodu bağlantı bilgisi nesnesi için kullanılan bir değişken adı. |
|**HubName**|**HubName**| Bu değer, bağlantı bilgilerini oluşturulduğu SignalR hub'ı adını ayarlamanız gerekir.|
|**userId**|**Kullanıcı Kimliği**| İsteğe bağlı: Kullanıcı tanımlayıcısı değeri talep erişim anahtar belirtecinde ayarlanacak. |
|**connectionStringSetting**|**connectionStringSetting**| SignalR hizmeti bağlantı dizesini (varsayılan olarak "AzureSignalRConnectionString") içeren uygulama ayarının adı |

### <a name="signalr"></a>SignalR

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `SignalR` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**|| Ayarlanmalıdır `signalR`.|
|**direction**|| Ayarlanmalıdır `out`.|
|**Adı**|| İşlev kodu bağlantı bilgisi nesnesi için kullanılan bir değişken adı. |
|**HubName**|**HubName**| Bu değer, bağlantı bilgilerini oluşturulduğu SignalR hub'ı adını ayarlamanız gerekir.|
|**connectionStringSetting**|**connectionStringSetting**| SignalR hizmeti bağlantı dizesini (varsayılan olarak "AzureSignalRConnectionString") içeren uygulama ayarının adı |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

