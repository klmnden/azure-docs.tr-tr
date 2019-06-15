---
title: .NET Azure işlevleri'nde bağımlılık ekleme kullanın
description: Bağımlılık ekleme, kaydetme ve Hizmetleri içinde .NET işlevleri kullanmak için kullanmayı öğrenin
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, sunucusuz mimari
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/28/2019
ms.author: jehollan, glenga, cshoe
ms.openlocfilehash: b1a6751f0d788c26af60b28eee994dc9b3877f00
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66693253"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>.NET Azure işlevleri'nde bağımlılık ekleme kullanın

Azure işlevlerini destekleyen bir tekniktir elde etmek için bağımlılık ekleme (dı) yazılım tasarım deseni [denetimi tersine çevirme (IOC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) sınıfları ve bunların bağımlılıklarını arasında.

Azure işlevleri üzerinde ASP.NET Core bağımlılık ekleme özellikleri oluşturur. Hizmetleri, yaşam süresi ve tasarım desenlerini farkına varmadan [ASP.NET Core bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI özellikler bir Azure işlevleri'nde kullanmadan önce uygulama önerilir.

## <a name="prerequisites"></a>Önkoşullar

Bağımlılık ekleme kullanabilmeniz için aşağıdaki NuGet paketlerini yüklemeniz gerekir:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft.NET.Sdk.Functions paketi](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) 1.0.28 sürümü veya üzeri

## <a name="register-services"></a>Kayıt Hizmetleri

Hizmetleri kaydolmak için yapılandırıp bileşenleri eklemek için bir yöntem oluşturabilirsiniz bir `IFunctionsHostBuilder` örneği.  Azure işlevleri ana bilgisayar örneği oluşturur `IFunctionsHostBuilder` ve doğrudan yönteme geçirir.

Register yöntemi ekleyin `FunctionsStartup` türü adını belirten derleme özniteliğini başlatma sırasında kullanılır.

```csharp
[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
            builder.Services.AddSingleton((s) => {
                return new CosmosClient(Environment.GetEnvironmentVariable("COSMOSDB_CONNECTIONSTRING"));
            });
            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

## <a name="use-injected-dependencies"></a>Eklenen bağımlılıkları kullanın

ASP.NET Core Oluşturucu ekleme bağımlılıklarınızı işlevinizi kullanılabilir hale getirmek için kullanır. Aşağıdaki örnekte nasıl `IMyService` ve `HttpClient` bağımlılıkları HTTP ile tetiklenen bir işlev eklenmiş.

```csharp
namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();;
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

Oluşturucu ekleme kullanımını bağımlılık ekleme yararlanmak isterseniz, statik işlevler kullanmamalısınız anlamına gelir.

## <a name="service-lifetimes"></a>Hizmet yaşam süresi yok

Azure işlev uygulamaları olarak aynı hizmet yaşam süreleri sağlamak [ASP.NET bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): geçici, kapsamı belirlenmiş ve tekil.

Bir işlev uygulamasında, işlevi yürütme ömrü kapsamlı hizmet ömrü eşleşir. Kapsamlı hizmet, yürütme bir kez oluşturulur. Sonraki istekleri yürütme sırasında bu hizmeti için mevcut hizmet örneği yeniden kullanın. Singleton hizmet ömrü konak ömrü eşleşen ve işlev yürütmelerini örneğine arasında yeniden.

Singleton ömrü Hizmetleri önerilir bağlantıları ve istemciler için örneğin `SqlConnection`, `CloudBlobClient`, veya `HttpClient` örnekleri.

Görüntüleme veya indirme bir [farklı hizmet yaşam süreleri, örnek](https://aka.ms/functions/di-sample) GitHub üzerinde.

## <a name="logging-services"></a>Günlüğe kaydetme Hizmetleri

Kendi oturum açma sağlayıcısı gerekirse kaydetmek için önerilen yöntem olduğu bir `ILoggerProvider` örneği. Application ınsights'ı Azure işlevleri tarafından otomatik olarak eklenir.

> [!WARNING]
> Eklemeyin `AddApplicationInsightsTelemetry()` haliyle Hizmetleri koleksiyonuna çakışan Hizmetleri kayıtları'ortam tarafından sağlanan hizmetlerle.

## <a name="function-app-provided-services"></a>Sağlanan uygulama hizmetleri işlevi

Birçok hizmet işlevi konağa kaydeder. Aşağıdaki hizmetler bir bağımlılık, uygulamanızda olarak yararlanmak güvenli şunlardır:

|Hizmet Türü|Yaşam süresi|Açıklama|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|singleton|Çalışma zamanı yapılandırma|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|singleton|Sorumlu Kimliği ana bilgisayar örneği sağlamak için|

Bir bağımlılık yap istediğiniz diğer hizmetleri varsa [bir sorun oluşturun ve bunları Github'da önerilen](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Ana Bilgisayar Hizmetleri geçersiz kılma

Ana bilgisayar tarafından sağlanan hizmetleri geçersiz kılma şu anda desteklenmiyor.  Geçersiz kılmak istediğiniz hizmetleri varsa [bir sorun oluşturun ve bunları Github'da önerilen](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İşlev uygulamanızı izleme](functions-monitoring.md)
- [İşlevleri için en iyi uygulamalar](functions-best-practices.md)
