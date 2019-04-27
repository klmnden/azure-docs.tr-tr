---
title: Geliştirme ve Azure işlevleri SignalR hizmeti uygulamaları yapılandırma
description: Geliştirme ve Azure işlevleri ve Azure SignalR hizmeti kullanarak sunucusuz gerçek zamanlı uygulamalar yapılandırma hakkında ayrıntılar
author: anthonychu
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: antchu
ms.openlocfilehash: 9b68b9d0bbac984c29759cf4b7b026a559a9d819
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809016"
---
# <a name="azure-functions-development-and-configuration-with-azure-signalr-service"></a>Azure işlevleri geliştirme ve Azure SignalR hizmeti ile yapılandırma

Azure işlevleri uygulamaları yararlanabilir [Azure SignalR hizmeti bağlamaları](../azure-functions/functions-bindings-signalr-service.md) gerçek zamanlı özellikleri eklemek için. İstemci uygulamaları için Azure SignalR hizmeti bağlayın ve gerçek zamanlı iletileri almak için İstemci SDK'ları çeşitli dillerde kullanır.

Bu makalede, geliştirme ve SignalR hizmeti ile tümleştirilmiş bir Azure işlev uygulaması yapılandırma kavramlarını açıklar.

## <a name="signalr-service-configuration"></a>SignalR hizmet yapılandırması

Azure SignalR hizmeti farklı modlarda yapılandırılabilir. Azure işlevleri ile kullanıldığında, hizmet yapılandırılmalıdır *sunucusuz* modu.

Azure portalında bulun *ayarları* SignalR hizmeti kaynağınızın sayfası. Ayarlama *hizmeti modu* için *sunucusuz*.

![SignalR hizmeti modu](media/signalr-concept-azure-functions/signalr-service-mode.png)

## <a name="azure-functions-development"></a>Azure işlevleri geliştirme

Azure SignalR hizmeti ve Azure işlevleri ile genellikle yerleşik sunucusuz gerçek zamanlı bir uygulama iki Azure işlevleri gerektirir:

* "Anlaşma" geçerli bir SignalR hizmeti elde etmek için istemcinin çağıran bir işlev belirtecine erişmesini ve hizmet uç noktası URL'si
* İleti göndermek ya da grup üyeliğini yönetme veya daha fazla işlevi

### <a name="negotiate-function"></a>Anlaşma işlevi

Azure SignalR hizmetine bağlanmak için geçerli bir belirteç bir istemci uygulaması gerektirir. Anonim veya belirli bir kullanıcı kimliği doğrulanmış bir erişim belirteci Bir HTTP uç noktası "bir belirteç ve SignalR hizmet uç noktası URL'si gibi diğer bağlantı bilgilerini almak için anlaşma" adlı sunucusuz SignalR hizmeti uygulamaları gerektirir.

HTTP kullan Azure işlevi tetiklenir ve *SignalRConnectionInfo* bağlantı bilgileri nesnesi oluşturmak için bağlama giriş. İşlevi ile biten bir HTTP yönlendirmesi olmalıdır `/negotiate`.

Anlaşma işlevinin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [ *SignalRConnectionInfo* giriş bağlama başvurusunu](../azure-functions/functions-bindings-signalr-service.md#signalr-connection-info-input-binding).

Kimliği doğrulanmış bir belirteç oluşturma hakkında bilgi edinmek için bkz [App Service kimlik doğrulaması kullanarak](#using-app-service-authentication).

### <a name="sending-messages-and-managing-group-membership"></a>İleti gönderme ve grup üyeliğini yönetme

Kullanım *SignalR* bağlı Azure SignalR hizmeti için istemcilere göndermek için çıktı bağlama. Tüm istemciler için iletileri yayınlayabilirsiniz veya bir alt kümesini bir belirli bir kullanıcı kimliği ile kimlik doğrulaması veya belirli bir gruba eklenmiş olan istemciler için gönderebilirsiniz.

Kullanıcılar, bir veya daha fazla gruba eklenebilir. Ayrıca *SignalR* çıktı bağlaması ekleme veya kaldırma kullanıcılar/gruplar.

Daha fazla bilgi için [ *SignalR* çıkış bağlaması başvurusunun](../azure-functions/functions-bindings-signalr-service.md#signalr-output-binding).

### <a name="signalr-hubs"></a>SignalR Hubs

SignalR "hub'lar" kavramı vardır. Her istemci bağlantısı ve Azure işlevleri ' gönderilen her ileti için belirli bir hub alanındadır. Hub'ları, bağlantılarınızı ve iletileri mantıksal ad alanında ayrı bir yolu olarak kullanabilirsiniz.

## <a name="client-development"></a>İstemci geliştirme

SignalR istemcisi kolayca bağlanma ve Azure SignalR hizmeti iletileri almak için birkaç farklı dilden birinde SDK SignalR istemci uygulamalardan yararlanabilirsiniz.

### <a name="configuring-a-client-connection"></a>İstemci bağlantısı yapılandırma

SignalR hizmetine bağlanmak için bir istemci, bu adımlardan oluşur başarılı bağlantı anlaşması tamamlamanız gerekir:

1. İsteğin yapılacağı *anlaşma* geçerli bağlantı bilgilerini almak için HTTP uç noktası ele yukarıda
1. SignalR hizmet uç noktası URL'sini kullanarak hizmete bağlanın ve öğesinden alınan erişim belirteci *anlaşma* uç noktası

SignalR istemci SDK'ları zaten anlaşma anlaşmasını gerçekleştirmek için gereken mantığı içerir. Görüşme uç noktasının URL'sini eksi geçirmek `negotiate` segmente, SDK'ın `HubConnectionBuilder`. JavaScript'te bir örnek aşağıdadır:

```javascript
const connection = new signalR.HubConnectionBuilder()
  .withUrl('https://my-signalr-function-app.azurewebsites.net/api')
  .build()
```

Kural gereği, SDK'yı otomatik olarak ekler `/negotiate` URL'sine ve anlaşma başlamak için kullanır.

> [!NOTE]
> Bir tarayıcıda JavaScript/TypeScript SDK'sı kullanıyorsanız, yapmanız [çıkış noktaları arası kaynak paylaşımı (CORS) etkinleştirme](#enabling-cors) işlev uygulamanız üzerinde.

SignalR istemci SDK'sı kullanma hakkında daha fazla bilgi için dilinize yönelik belgelere bakın:

* [.NET Standard](https://docs.microsoft.com/aspnet/core/signalr/dotnet-client)
* [JavaScript](https://docs.microsoft.com/aspnet/core/signalr/javascript-client)
* [Java](https://docs.microsoft.com/aspnet/core/signalr/java-client)

### <a name="sending-messages-from-a-client-to-the-service"></a>Bir istemciden hizmete gönderme

SignalR SDK'sı istemci uygulamalar bir SignalR hub'ı, arka uç mantığınızı çağırmak izin verir, ancak bu işlev, Azure işlevleri ile SignalR hizmeti kullandığınızda henüz desteklenmiyor. Azure işlevleri çağırmak için HTTP'yi istekleri'ni kullanın.

## <a name="azure-functions-configuration"></a>Azure işlevleri yapılandırması

Azure SignalR hizmeti ile tümleştirilen azure işlev uygulamaları gibi teknikler kullanılarak tüm genel Azure işlev uygulaması gibi dağıtılabilir [sürekli olarak dağıtım](../azure-functions/functions-continuous-deployment.md), [zip dağıtım](../azure-functions/deployment-zip-push.md)ve [paketinden çalıştırma](../azure-functions/run-functions-from-deployment-package.md).

Ancak, birkaç SignalR hizmet bağlamaları kullanan uygulamalar için özel hususlar vardır. İstemci bir tarayıcısında çalıştırılıyorsa, CORS etkin olmalıdır. Ve uygulama kimlik doğrulaması gerektiriyorsa, negotiate uç nokta App Service kimlik doğrulaması ile tümleştirilebilir.

### <a name="enabling-cors"></a>CORS'yi etkinleştirme

JavaScript/TypeScript istemci bağlantı anlaşması başlatmak için anlaşma işlevi HTTP isteklerini gönderir. İstemci uygulaması, farklı bir Azure işlev uygulaması etki barındırıldığında, çıkış noktaları arası kaynak paylaşımı (CORS) işlev uygulamasını etkinleştirilmesi gerekir veya tarayıcı istekleri engeller.

#### <a name="localhost"></a>localhost

İşlev uygulaması, yerel bilgisayarınızda çalışırken ekleyebileceğiniz bir `Host` bölümünü *local.settings.json* CORS'yi etkinleştirmek için. İçinde `Host` bölümünde, iki özellik ekleyin:

* `CORS` -kaynağı istemci uygulamasının temel URL'sini girin
* `CORSCredentials` -ayarlayın `true` "withCredentials" isteklere izin vermek için

Örnek:

```json
{
  "IsEncrypted": false,
  "Values": {
    // values
  },
  "Host": {
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
}
```

#### <a name="azure"></a>Azure

CORS yapılandırma ekranın gidin CORS bir Azure işlev uygulaması üzerinde etkinleştirmek için *Platform özellikleri* Azure portalında işlev uygulamanızın sekmesi.

CORS ile erişim-denetim-Allow-Credentials anlaş işlevi çağırmak SignalR istemcisi için etkinleştirilmesi gerekir. Bunu etkinleştirmek için onay kutusunu seçin.

İçinde *çıkış noktaları* bölümünde, web uygulamanızın kaynak temel URL ile bir giriş ekleyin.

![CORS yapılandırma](media/signalr-concept-serverless-development-config/cors-settings.png)

### <a name="using-app-service-authentication"></a>App Service kimlik doğrulaması kullanma

Azure işlevleri, Facebook, Twitter, Microsoft Account, Google ve Azure Active Directory gibi popüler sağlayıcılar destekleyen yerleşik kimlik doğrulama sahiptir. Bu özellik ile tümleştirilebilir *SignalRConnectionInfo* Azure SignalR hizmeti için bir kullanıcı kimliği doğrulanan bağlantılar oluşturmak için bağlama Uygulamanızı kullanarak iletiler gönderebilir *SignalR* çıkışı, kullanıcı kimliği için hedeflenen bağlama

Azure portalında işlev uygulamanızın *Platform özellikleri* sekmesini *kimlik doğrulama/yetkilendirme* Ayarları penceresi. Belgelerini izleyin [App Service kimlik doğrulaması](../app-service/overview-authentication-authorization.md) tercih ettiğiniz bir kimlik sağlayıcısı kullanarak kimlik doğrulamasını yapılandırmak için.

Kimliği doğrulanmış HTTP isteklerini yapılandırıldıktan sonra içerecektir `x-ms-client-principal-name` ve `x-ms-client-principal-id` kimliği doğrulanmış bir kimliğin kullanıcı adı ve kullanıcı kimliği, sırasıyla içeren üst bilgiler.

Bu üst bilgilerinde kullanabilirsiniz, *SignalRConnectionInfo* kimliği doğrulanmış bağlantılar oluşturmak için bağlama yapılandırması. İşte bir örnek C# kullanan işlev anlaşma `x-ms-client-principal-id` başlığı.

```csharp
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

Ayarlayarak iletileri için o kullanıcı ardından gönderebilir `UserId` özelliği SignalR iletisi.

```csharp
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

Diğer diller hakkında daha fazla bilgi için bkz: [Azure SignalR hizmeti bağlamaları](../azure-functions/functions-bindings-signalr-service.md) Azure işlevleri başvurmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, geliştirmek ve Azure işlevleri'ni kullanarak sunucusuz SignalR hizmeti uygulamaları yapılandırmak öğrendiniz. Bir uygulama oluşturmayı deneyin öğreticileri ve hızlı başlangıçlardan birini kullanarak kendiniz [SignalR hizmeti genel bakış sayfasında](index.yml).