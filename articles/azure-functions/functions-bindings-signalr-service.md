---
title: Azure SignalR hizmeti işlevleri bağlamaları
description: Azure işlevleri ile SignalR hizmet bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: f0d4a607676285ed4f0f91d8ce8c83ddf1313b89
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437807"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Azure İşlevleri için SignalR Service bağlamaları

Bu makalede, kimliğini doğrulamak ve istemcilere bağlı gerçek zamanlı iletileri göndermek açıklanmaktadır [Azure SignalR hizmeti](https://azure.microsoft.com/services/signalr-service/) Azure işlevleri'nde SignalR hizmet bağlamaları kullanarak. Giriş ve çıkış bağlamaları SignalR hizmeti için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

SignalR hizmet bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) NuGet paketi sürüm 1.*. Paket için kaynak kodu konusu [azure işlevleri signalrservice uzantı](https://github.com/Azure/azure-functions-signalrservice-extension) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]


### <a name="java-annotations"></a>Java ek açıklamaları

SignalR hizmet ek açıklamalar Java işlevleri kullanmak için bağımlılık ekleme gerekir *azure-işlevler-java-kitaplığı-signalr* pom.xml yapıya (sürüm 1.0 veya üzeri).

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

> [!NOTE]
> SignalR hizmet bağlamaları Java'da yapma emin 2.4.419 sürümü kullandığınız ya da üst sürümünü Azure işlevleri çekirdek Araçları'nı (ana sürüm 2.0.12332) kullanılacak.

## <a name="using-signalr-service-with-azure-functions"></a>SignalR hizmeti ile Azure işlevlerini kullanma

Yapılandırma ve SignalR Service ve Azure işlevleri birlikte kullanma hakkında daha fazla bilgi için başvurmak [Azure işlevleri geliştirme ve Azure SignalR hizmeti yapılandırmasıyla](../azure-signalr/signalr-concept-serverless-development-config.md).

## <a name="signalr-connection-info-input-binding"></a>SignalR bağlantı bilgisi giriş bağlama

Azure SignalR hizmeti için bir istemci bağlanabilmeleri için hizmet uç noktası URL'sini ve geçerli bir erişim belirteci alması gerekir. *SignalRConnectionInfo* giriş bağlama SignalR Hizmeti uç nokta URL'nizi ve hizmete bağlanmak için kullanılan geçerli bir belirteç oluşturur. Belirteç süre sınırlı ve bağlantı belirli bir kullanıcının kimliğini doğrulamak için kullanılan belirteç önbelleğe veya gerekir istemciler arasında paylaşın. Bu bağlama kullanarak HTTP tetikleyicisi, bağlantı bilgilerini almak için istemciler tarafından kullanılabilir.

Dile özgü örneğe bakın:

* [2.x C#](#2x-c-input-examples)
* [2.x JavaScript](#2x-javascript-input-examples)
* [2.x Java](#2x-java-input-examples)

Bu bağlama, SignalR istemci SDK'sı tarafından tüketilebilecek bir "anlaşma" işlevi oluşturmak için nasıl kullanıldığı hakkında daha fazla bilgi için bkz: [Azure işlevleri geliştirme ve yapılandırma makalesine](../azure-signalr/signalr-concept-serverless-development-config.md) içinde SignalR hizmeti kavramları belgeleri.

### <a name="2x-c-input-examples"></a>2.x C# giriş örnekleri

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) giriş bağlamasına kullanarak SignalR bağlantı bilgilerini alır ve HTTP üzerinden döndürür.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçleri

Kimliği doğrulanmış bir istemci tarafından tetiklenen işlev ise oluşturulan belirteç için bir kullanıcı kimliği talebi ekleyebilirsiniz. Kimlik doğrulaması kullanarak bir işlev uygulaması için kolayca ekleyebilirsiniz [App Service kimlik doğrulaması](../app-service/overview-authentication-authorization.md).

App Service kimlik doğrulaması adlı HTTP üstbilgileri ayarlar `x-ms-client-principal-id` ve `x-ms-client-principal-name` içeren kimliği doğrulanmış kullanıcının asıl istemci kimliği ve adı, sırasıyla. Ayarlayabileceğiniz `UserId` özelliğini kullanarak ya da üst bilgi değeri bağlamanın bir [ifade bağlama](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-examples"></a>2.x JavaScript giriş örnekleri

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
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

#### <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçleri

Kimliği doğrulanmış bir istemci tarafından tetiklenen işlev ise oluşturulan belirteç için bir kullanıcı kimliği talebi ekleyebilirsiniz. Kimlik doğrulaması kullanarak bir işlev uygulaması için kolayca ekleyebilirsiniz [App Service kimlik doğrulaması](../app-service/overview-authentication-authorization.md).

App Service kimlik doğrulaması adlı HTTP üstbilgileri ayarlar `x-ms-client-principal-id` ve `x-ms-client-principal-name` içeren kimliği doğrulanmış kullanıcının asıl istemci kimliği ve adı, sırasıyla. Ayarlayabileceğiniz `userId` özelliğini kullanarak ya da üst bilgi değeri bağlamanın bir [ifade bağlama](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`. 

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
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

### <a name="2x-java-input-examples"></a>2.x Java giriş örnekleri

Aşağıdaki örnekte gösterildiği bir [Java işlevi](functions-reference-java.md) giriş bağlamasına kullanarak SignalR bağlantı bilgilerini alır ve HTTP üzerinden döndürür.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçleri

Kimliği doğrulanmış bir istemci tarafından tetiklenen işlev ise oluşturulan belirteç için bir kullanıcı kimliği talebi ekleyebilirsiniz. Kimlik doğrulaması kullanarak bir işlev uygulaması için kolayca ekleyebilirsiniz [App Service kimlik doğrulaması](../app-service/overview-authentication-authorization.md).

App Service kimlik doğrulaması adlı HTTP üstbilgileri ayarlar `x-ms-client-principal-id` ve `x-ms-client-principal-name` içeren kimliği doğrulanmış kullanıcının asıl istemci kimliği ve adı, sırasıyla. Ayarlayabileceğiniz `UserId` özelliğini kullanarak ya da üst bilgi değeri bağlamanın bir [ifade bağlama](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

## <a name="signalr-output-binding"></a>SignalR çıktı bağlaması

Kullanım *SignalR* çıktı bağlaması Azure SignalR hizmeti kullanarak bir veya daha fazla ileti göndermek için. Bir ileti bağlanan tüm istemciler için yayın veya yalnızca kimliği doğrulanmış belirli bir kullanıcıya bağlı istemciler için yayın.

Bir kullanıcının ait olduğu grupları yönetmek için de kullanabilirsiniz.

Dile özgü örneğe bakın:

* [2.x C#](#2x-c-send-message-output-examples)
* [2.x JavaScript](#2x-javascript-send-message-output-examples)
* [2.x Java](#2x-java-send-message-output-examples)

### <a name="2x-c-send-message-output-examples"></a>2.x C# çıkışı örnekleri ileti gönder

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
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

#### <a name="send-to-a-group"></a>Bir grup gönderin

Ayarlayarak grubuna eklenmiş olan bağlantılara bir ileti gönderebilir `GroupName` özelliği SignalR iletisi.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

### <a name="2x-c-group-management-output-examples"></a>2.x C# Grup Yönetimi, örnek çıktı

SignalR hizmeti, gruba eklenecek kullanıcıların sağlar. İletileri sonra bir gruba gönderilebilir. Kullanabileceğiniz `SignalRGroupAction` sınıfıyla `SignalR` çıktı bağlaması bir kullanıcının grup üyeliğini yönetme.

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek, bir kullanıcı grubuna ekler.

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    string userId,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userId,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Gruptan kullanıcı kaldırma

Aşağıdaki örnek, bir kullanıcıyı bir gruptan kaldırır.

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    string userId,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userId,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

### <a name="2x-javascript-send-message-output-examples"></a>2.x JavaScript Gönder ileti çıkışı örnekleri

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
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-user"></a>Bir kullanıcıya Gönder

Bir kullanıcıya ayarlayarak doğrulanan bağlantılar için bir ileti gönderebilir `userId` özelliği SignalR iletisi.

*Function.JSON* aynı kalır. JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-group"></a>Bir grup gönderin

Ayarlayarak grubuna eklenmiş olan bağlantılara bir ileti gönderebilir `groupName` özelliği SignalR iletisi.

*Function.JSON* aynı kalır. JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

### <a name="2x-javascript-group-management-output-examples"></a>2.x JavaScript Grup Yönetimi, örnek çıktı

SignalR hizmeti, gruba eklenecek kullanıcıların sağlar. İletileri sonra bir gruba gönderilebilir. Kullanabileceğiniz `SignalR` çıktı bağlaması bir kullanıcının grup üyeliğini yönetme.

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek, bir kullanıcı grubuna ekler.

*Function.JSON*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

#### <a name="remove-user-from-a-group"></a>Gruptan kullanıcı kaldırma

Aşağıdaki örnek, bir kullanıcıyı bir gruptan kaldırır.

*Function.JSON*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

### <a name="2x-java-send-message-output-examples"></a>2.x Java Gönder ileti çıkışı örnekleri

#### <a name="broadcast-to-all-clients"></a>Tüm istemcilere yayın

Aşağıdaki örnekte gösterildiği bir [Java işlevi](functions-reference-java.md) bağlanan tüm istemciler için çıktı bağlama kullanarak bir ileti gönderir. `target` Her istemcide çağrılacak yöntemin adı. `arguments` Özelliği istemci yöntemine geçirilecek sıfır veya daha fazla nesne bir dizisidir.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-user"></a>Bir kullanıcıya Gönder

Bir kullanıcıya ayarlayarak doğrulanan bağlantılar için bir ileti gönderebilir `userId` özelliği SignalR iletisi.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.userId = "userId1";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-group"></a>Bir grup gönderin

Ayarlayarak grubuna eklenmiş olan bağlantılara bir ileti gönderebilir `groupName` özelliği SignalR iletisi.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.groupName = "myGroup";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

### <a name="2x-java-group-management-output-examples"></a>2.x Grup Yönetimi Java örnek çıktı

SignalR hizmeti, gruba eklenecek kullanıcıların sağlar. İletileri sonra bir gruba gönderilebilir. Kullanabileceğiniz `SignalRGroupAction` sınıfıyla `SignalROutput` çıktı bağlaması bir kullanıcının grup üyeliğini yönetme.

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek, bir kullanıcı grubuna ekler.

```java
@FunctionName("addToGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction addToGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "add";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

#### <a name="remove-user-from-a-group"></a>Gruptan kullanıcı kaldırma

Aşağıdaki örnek, bir kullanıcıyı bir gruptan kaldırır.

```java
@FunctionName("removeFromGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction removeFromGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "remove";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
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

> [!div class="nextstepaction"]
> [Azure işlevleri geliştirme ve Azure SignalR hizmeti ile yapılandırma](../azure-signalr/signalr-concept-serverless-development-config.md)
