---
title: .NET Azure işlevleri'nde bağımlılık ekleme kullanın
description: Bağımlılık ekleme, kaydetme ve Hizmetleri içinde .NET işlevleri kullanmak için kullanmayı öğrenin
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, sunucusuz mimari
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2019
ms.author: jehollan
ms.openlocfilehash: 2044718d2ec7a7acc58e1e7ba9ba04ec5caf16b3
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65408459"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>.NET Azure işlevleri'nde bağımlılık ekleme kullanın

Azure işlevlerini destekleyen bir tekniktir elde etmek için bağımlılık ekleme (dı) yazılım tasarım deseni [denetimi tersine çevirme (IOC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) sınıfları ve bunların bağımlılıklarını arasında.

Azure işlevleri üzerinde ASP.NET Core bağımlılık ekleme özellikleri oluşturur.  Hizmetleri, yaşam süresi ve tasarım desenlerini anlamanız gereken [ASP.NET Core bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) işlevlerde kullanmadan önce.

## <a name="installing-dependency-injection-packages"></a>Bağımlılık ekleme paketlerini yükleme

Bağımlılık ekleme özellikleri kullanmak için bu API'leri gösterir NuGet paketini içerecek şekilde gerekir.

```powershell
Install-Package Microsoft.Azure.Functions.Extensions
```

## <a name="registering-services"></a>Hizmetleri kaydediliyor

Hizmetleri kaydetmek için bir yapılandırma yöntemi oluşturabilir ve bileşenlere ekleme bir `IFunctionsHostBuilder` örneği.  Azure işlevleri ana bilgisayar oluşturur bir `IFunctionsHostBuilder` ve doğrudan yapılandırılmış, yönteme geçirir.

Kaydetmek için kendi yöntemini yapılandırmak, türünü belirten bir bütünleştirilmiş kod özniteliği eklemeniz gerekir, yöntemi kullanarak yapılandırma `FunctionsStartup` özniteliği.

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

## <a name="service-lifetimes"></a>Hizmet yaşam süresi yok

Azure işlev uygulamaları olarak aynı hizmet yaşam süreleri sağlamak [ASP.NET bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes)geçici, kapsamlı ve tekil.

Bir işlev uygulaması ile bir kapsamlı hizmet ömrü, işlevi yürütme ömrü eşleşir. Kapsamlı hizmet, yürütme bir kez oluşturulur.  Sonraki istekleri yürütme sırasında bu hizmet için bu örneği yeniden kullanın.  Singleton hizmet ömrü konak ömrü eşleşen ve işlev yürütmelerini örneğine arasında yeniden.

Singleton ömrü Hizmetleri önerilir bağlantıları ve istemcileri, örneğin bir `SqlConnection`, `CloudBlobClient`, veya `HttpClient`.

Görüntüleme veya indirme bir [farklı hizmet yaşam süreleri, örnek](https://aka.ms/functions/di-sample).

## <a name="logging-services"></a>Günlüğe kaydetme Hizmetleri

Kendi oturum açma sağlayıcısı gerekirse kaydetmek için önerilen yöntem olduğu bir `ILoggerProvider`.  Application Insights için işlevleri ekler Application ınsights'ı otomatik olarak sizin için.  

> [!WARNING]
> Eklemeyin `AddApplicationInsightsTelemetry()` çakışacak Hizmetleri kaydolacak gibi hizmetlere koleksiyonu ile ne ortamı tarafından sağlanır. 
 
## <a name="function-app-provided-services"></a>Sağlanan uygulama hizmetleri işlevi

İşlev konak birçok hizmet kendisi kaydeder.  Bir bağımlılık gerçekleştirilecek güvenli hizmetler aşağıda verilmiştir.  Diğer ana bilgisayar Hizmetleri kaydetmek veya bağımlı için desteklenmez.  Diğer hizmetler varsa lütfen bir bağımlılık, gerçekleştirmek istediğiniz [Github'da bir sorun ve tartışma oluşturma](https://github.com/azure/azure-functions-host).

|Hizmet türü|Yaşam süresi|Açıklama|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|singleton|Çalışma zamanı yapılandırma|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|singleton|Sorumlu Kimliği ana bilgisayar örneği sağlamak için|

### <a name="overriding-host-services"></a>Ana Bilgisayar Hizmetleri geçersiz kılma

Ana bilgisayar tarafından sağlanan hizmetleri geçersiz kılma şu anda desteklenmiyor.  Geçersiz kılma için istediğiniz hizmetleri varsa, lütfen [Github'da bir sorun ve tartışma oluşturma](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [İşlev uygulamanızı izleme](functions-monitoring.md)
* [İşlevleri için en iyi uygulamalar](functions-best-practices.md)