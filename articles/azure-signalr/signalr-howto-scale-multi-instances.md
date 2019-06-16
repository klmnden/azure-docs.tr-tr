---
title: Azure SignalR hizmeti için birden çok örnek ile ölçeklendirme
description: Birçok ölçeklendirme senaryolarda, müşteri genellikle birden fazla sağlama ve bunları birlikte, büyük ölçekli bir dağıtımı oluşturmak için kullanmak için yapılandırmanız gerekir. Örneğin, birden fazla destekleyen parçalama gerektirir.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: zhshang
ms.openlocfilehash: e284a0492774e02cab79db6d9006c1718a7fcfc9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60809116"
---
# <a name="how-to-scale-signalr-service-with-multiple-instances"></a>Birden çok örnek ile SignalR hizmetini ölçeklendirmek nasıl?
Son SignalR hizmet SDK'ın birden fazla uç nokta için SignalR hizmet örneklerini destekler. Eş zamanlı bağlantı ölçeklendirmek için bu özelliği kullanın veya bölgeler arası Mesajlaşma için kullanın.

## <a name="for-aspnet-core"></a>ASP.NET Core için

### <a name="how-to-add-multiple-endpoints-from-config"></a>Yapılandırma dosyasından birden fazla uç noktası eklemek nasıl?

Anahtara sahip yapılandırma `Azure:SignalR:ConnectionString` veya `Azure:SignalR:ConnectionString:` SignalR hizmet bağlantı dizesi.

Anahtar ile başlarsa `Azure:SignalR:ConnectionString:`, biçiminde olması gerektiğini `Azure:SignalR:ConnectionString:{Name}:{EndpointType}`burada `Name` ve `EndpointType` özellikleridir `ServiceEndpoint` nesne ve koddan erişilebilir.

Aşağıdakileri kullanarak birden çok örnek bağlantı dizeleri ekleyebilirsiniz `dotnet` komutları:

```batch
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-a <ConnectionString1>
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-b:primary <ConnectionString2>
dotnet user-secrets set Azure:SignalR:ConnectionString:backup:secondary <ConnectionString3>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Birden fazla uç nokta koddan ekleme?

A `ServicEndpoint` sınıfı, bir Azure SignalR Hizmeti uç noktası özelliklerini tanımlamak için sunulmuştur.
Azure SignalR hizmeti SDK'sı aracılığıyla kullanırken, birden fazla örneğinin uç noktası yapılandırabilirsiniz:
```cs
services.AddSignalR()
        .AddAzureSignalR(options => 
        {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString0>"),
                new ServiceEndpoint("<ConnectionString1>", type: EndpointType.Primary, name: "east-region-a"),
                new ServiceEndpoint("<ConnectionString2>", type: EndpointType.Primary, name: "east-region-b"),
                new ServiceEndpoint("<ConnectionString3>", type: EndpointType.Secondary, name: "backup"),
            };
        });
```

### <a name="how-to-customize-endpoint-router"></a>Uç nokta yönlendirici özelleştirmek nasıl?

Varsayılan olarak, SDK'sı kullanır [DefaultEndpointRouter](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/EndpointRouters/DefaultEndpointRouter.cs) uç noktaları ayarlama seçmek için.

#### <a name="default-behavior"></a>Varsayılan davranış 
1. İstemci istek yönlendirme

    İstemci `/negotiate` uygulama sunucusu ile. Varsayılan olarak, SDK'sı **rastgele seçer** kullanılabilir hizmet uç noktaları kümesinden bir uç nokta.

2. Sunucu ileti yönlendirme

    Zaman * belirli bir ileti gönderilirken ** bağlantısı *** ve hedef bağlantı geçerli sunucuya yönlendirilmesini, doğrudan bağlı uç noktanın için ileti gider. Aksi takdirde, her Azure SignalR uç noktasına iletileri yayımladınız.

#### <a name="customize-routing-algorithm"></a>Yönlendirme algoritması özelleştirme
İletileri gitmesi gereken hangi uç noktaları tanımlamak için özel bilgilere sahip olduğunuzda, kendi yönlendirici oluşturabilirsiniz.

Özel bir yönlendirici aşağıda bir örnek olarak tanımlanmıştır, ile başlayan gruplar `east-` adlı uç nokta her zaman Git `east`:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override IEnumerable<ServiceEndpoint> GetEndpointsForGroup(string groupName, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the group broadcast behavior, if the group name starts with "east-", only send messages to endpoints inside east
        if (groupName.StartsWith("east-"))
        {
            return endpoints.Where(e => e.Name.StartsWith("east-"));
        }

        return base.GetEndpointsForGroup(groupName, endpoints);
    }
}
```

Aşağıdaki varsayılan geçersiz kılan başka bir örnek anlaşma davranışı, uç noktaları seçmek için burada uygulama sunucusu konumuna bağlıdır.

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(HttpContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (endpointName.Count == 0)
        {
            context.Response.StatusCode = 400;
            var response = Encoding.UTF8.GetBytes("Invalid request");
            context.Response.Body.Write(response, 0, response.Length);
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

DI kullanarak kapsayıcının yönlendirici kaydedilecek unutmayın:

```cs
services.AddSingleton(typeof(IEndpointRouter), typeof(CustomRouter));
services.AddSignalR()
        .AddAzureSignalR(
            options => 
            {
                options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
            });
```

## <a name="for-aspnet"></a>ASP.NET için

### <a name="how-to-add-multiple-endpoints-from-config"></a>Yapılandırma dosyasından birden fazla uç noktası eklemek nasıl?

Anahtara sahip yapılandırma `Azure:SignalR:ConnectionString` veya `Azure:SignalR:ConnectionString:` SignalR hizmet bağlantı dizesi.

Anahtar ile başlarsa `Azure:SignalR:ConnectionString:`, biçiminde olması gerektiğini `Azure:SignalR:ConnectionString:{Name}:{EndpointType}`burada `Name` ve `EndpointType` özellikleridir `ServiceEndpoint` nesne ve koddan erişilebilir.

Birden çok örnek bağlantı dizeleri ekleyebilirsiniz `web.config`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <connectionStrings>
    <add name="Azure:SignalR:ConnectionString" connectionString="<ConnectionString1>"/>
    <add name="Azure:SignalR:ConnectionString:en-us" connectionString="<ConnectionString2>"/>
    <add name="Azure:SignalR:ConnectionString:zh-cn:secondary" connectionString="<ConnectionString3>"/>
    <add name="Azure:SignalR:ConnectionString:Backup:secondary" connectionString="<ConnectionString4>"/>
  </connectionStrings>
  ...
</configuration>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Birden fazla uç nokta koddan ekleme?

A `ServicEndpoint` sınıfı, bir Azure SignalR Hizmeti uç noktası özelliklerini tanımlamak için sunulmuştur.
Azure SignalR hizmeti SDK'sı aracılığıyla kullanırken, birden fazla örneğinin uç noktası yapılandırabilirsiniz:

```cs
app.MapAzureSignalR(
    this.GetType().FullName, 
    options => {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString1>"),
                new ServiceEndpoint("<ConnectionString2>"),
                new ServiceEndpoint("<ConnectionString3>"),
            }
        });
```

### <a name="how-to-customize-router"></a>Yönlendirici özelleştirmek nasıl?

ASP.NET SignalR ve ASP.NET Core SignalR arasındaki tek fark http bağlamı için türüdür `GetNegotiateEndpoint`. Biri olan ASP.NET SignalR için [IOwinContext](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR.AspNet/EndpointRouters/DefaultEndpointRouter.cs#L19) türü.

ASP.NET SignalR için özel anlaşma örnek aşağıdadır:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(IOwinContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (string.IsNullOrEmpty(endpointName))
        {
            context.Response.StatusCode = 400;
            context.Response.Write("Invalid request.");
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

DI kullanarak kapsayıcının yönlendirici kaydedilecek unutmayın:

```cs
var hub = new HubConfiguration();
var router = new CustomRouter();
hub.Resolver.Register(typeof(IEndpointRouter), () => router);
app.MapAzureSignalR(GetType().FullName, hub, options => {
    options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
});
```

## <a name="configuration-in-cross-region-scenarios"></a>Bölgeler arası senaryolarda yapılandırma

`ServiceEndpoint` Nesnesinin bir `EndpointType` özellik değeriyle `primary` veya `secondary`.

`primary` uç noktaları, istemci trafiğini almak için tercih edilen uç noktaları olan ve daha güvenilir ağ bağlantıları sahip olduğu kabul edilir; `secondary` uç noktaları daha az güvenilir ağ bağlantıları sahip olduğu kabul edilir ve sunucu trafiğini alma istemciye için değil, yalnızca yayın iletileri alma sunucusu istemci trafiği için kullanılır.

Bölgeler arası durumlarda ağ kararsız duruma gelmesine neden olabilir. Bulunan bir uygulama sunucusu için *Doğu ABD*, SignalR hizmet uç noktası bulunan aynı *Doğu ABD* bölge olarak yapılandırılabilir `primary` ve diğer bölgelerde uç noktalar olarak işaretlenmiş `secondary`. Bu yapılandırmada diğer bölgelerdeki hizmet uç noktaları için **alma** bu iletileri *Doğu ABD* uygulama sunucusu, ancak bulunmaz olması hiçbir **bölgeler arası** istemcilere yönlendirilmesi için Bu uygulama sunucusu. Mimari Aşağıdaki diyagramda gösterilmiştir:

![Cross-Geo Infra](./media/signalr-howto-scale-multi-instances/cross_geo_infra.png)

Bir istemci olduğunda çalışır `/negotiate` varsayılan yönlendirici, SDK'sı ile uygulama sunucusu ile **rastgele seçer** kullanılabilir kümesinden bir uç nokta `primary` uç noktaları. Uç nokta kullanılabilir olduğunda SDK'sı ardından **rastgele seçer** tüm kullanılabilir `secondary` uç noktaları. Uç nokta olarak işaretlenmiş **kullanılabilir** sunucu ve hizmet uç noktası arasındaki bağlantıyı olduğunda tutma.

Bölgeler arası senaryoda bir istemci çalıştığında `/negotiate` barındırılan uygulama sunucusu ile *Doğu ABD*tarafından döndürür her zaman varsayılan `primary` uç nokta aynı bölgede yer alan. Zaman tüm *Doğu ABD* uç noktaları kullanılabilir değil, istemci de başka bölgelerde Uç noktalara yönlendirilir. Yük devretme bölümüne ayrıntılı bir senaryoyu açıklar.

![Normal anlaşması](./media/signalr-howto-scale-multi-instances/normal_negotiate.png)

## <a name="fail-over"></a>Yük devretme

Zaman tüm `primary` uç noktaları kullanılabilir değil, istemcinin `/negotiate` seçer kullanılabilir `secondary` uç noktaları. Bu yük devretme mekanizması her uç nokta olarak kullanılmalıdır gerektirir `primary` uç noktası için en az bir uygulama sunucusu.

![Yük devretme](./media/signalr-howto-scale-multi-instances/failover_negotiate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, ölçeklendirme, parçalama ve bölgeler arası senaryoları için aynı uygulamada birden fazla yapılandırma hakkında bilgi edindiniz.

Birden çok uç noktaları destekliyor, yüksek kullanılabilirlik ve olağanüstü durum kurtarma senaryolarında da kullanılabilir.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma ve yüksek kullanılabilirlik için SignalR hizmetini ayarlama](./signalr-concept-disaster-recovery.md)
