---
title: Özellik bayraklarını kullanarak bir .NET Core uygulaması için öğretici | Microsoft Docs
description: Bu öğreticide, .NET Core uygulamalarında özellik bayraklarını uygulanacağını öğrenin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: f712cc34a3d41ea9472bf9428606cb378eef8c18
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244255"
---
# <a name="tutorial-use-feature-flags-in-a-net-core-app"></a>Öğretici: .NET Core uygulamasında özellik bayraklarını kullanma

.NET Core özellik yönetim kitaplıkları, .NET veya ASP.NET Core uygulamada özellik bayraklarını uygulamak için kullanılan deyimsel desteği sağlar. Böylece tüm yazmak zorunda değil özellik kodunuza daha bildirimli olarak işaretler. eklemenize izin `if` deyimleri için bunları el ile. Özellik bayrağı yaşam döngüleri (örneğin, yenileme ve durumları bayrak, istek araması sırasında sabit olması için bir bayrak durumunu garanti önbelleği) yönettikleri sahnenin arkasına. Ayrıca, ASP.NET Core kitaplığı MVC denetleyici eylemleri, görünümler, yollar ve ara yazılım gibi kullanıma hazır Tümleştirmelerin sunar.

[ASP.NET Core uygulaması için özellik bayraklarını ekleme](./quickstart-feature-flag-aspnet-core.md) hızlı başlangıç, birkaç özellik bayrakları bir ASP.NET Core uygulaması eklemek için yol gösterir. Bu öğreticide, bunları daha ayrıntılı açıklanmaktadır. Bkz: [ASP.NET Core özellik yönetim belgelerine](https://go.microsoft.com/fwlink/?linkid=2091410) için eksiksiz bir başvuru.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Özellik bayrakları, anahtar Özellik kullanılabilirliği kontrol etmek için uygulamanızın parçalarını içinde ekleyin.
> * Özellik bayraklarını yönetmek için kullanırken uygulama yapılandırması ile tümleştirin.

## <a name="setup"></a>Kurulum

.NET Core özellik Yöneticisi `IFeatureManager` framework'ün yerel yapılandırma sistemden özellik bayraklarını alır. Sonuç olarak, .NET Core destekler, yerel dahil olmak üzere herhangi bir yapılandırma kaynağını kullanarak, uygulamanızın özellik bayraklarını tanımlayabilirsiniz *appsettings.json* dosya veya ortam değişkenleri. Özellik Yöneticisi üzerinde .NET Core bağımlılık ekleme kullanmaktadır. Standart kurallarını kullanarak özellik Yönetim Hizmetleri kaydedebilirsiniz.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

Özellik Yöneticisi özellik bayrakları .NET Core yapılandırma verilerini "FeatureManagement" bölümünden varsayılan olarak alır. Aşağıdaki örnek, bunun yerine "MyFeatureFlags" adlı farklı bir bölümünden okumak için söyler.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

Filtre, özellik bayrakları kullanıyorsanız, ek bir kitaplık içerir ve kaydetmek gerekir. Aşağıdaki örnekte adlı bir yerleşik özellik filtresini kullanma işlemi gösterilmektedir **PercentageFilter "** .

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

Etkili bir şekilde çalışması için özellik bayraklarını uygulama dışında tutun ve ayrı olarak yönetin. Bunun yapılması, dilediğiniz zaman bayrağı durumlarını değiştirme ve uygulamaya hemen etkili bu değişiklikleri sağlar. Uygulama yapılandırması, düzenleme ve tüm özelliğinizi denetlemek için merkezi bir yerde adanmış bir portal kullanıcı Arabirimi bayraklar ve kitaplıkları, .NET Core istemcisi üzerinden doğrudan uygulamanıza bayrakları sunar sağlar. Yapılandırma sağlayıcısı uygulama yapılandırması için ASP.NET Core uygulamanızı bağlamak için en kolay yolu olan `Microsoft.Extensions.Configuration.AzureAppConfiguration`. Aşağıdakileri ekleyerek bu NuGet paketi kodunuza kullanabilirsiniz *Program.cs* dosyası:

```csharp
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
           .ConfigureAppConfiguration((hostingContext, config) => {
               var settings = config.Build();
               config.AddAzureAppConfiguration(options => {
                   options.Connect(settings["ConnectionStrings:AppConfig"])
                          .UseFeatureFlags();
                });
           })
           .UseStartup<Startup>();
```

Özellik bayrağı değerleri, Zamanla değişen beklenir. Varsayılan olarak, her 30 saniyede özellik bayrağı değerleri özellik Yöneticisi yenileyin. Farklı bir yoklama aralığı içinde kullanabileceğiniz `options.UseFeatureFlags()` yukarıdaki çağrısı.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.PollInterval = TimeSpan.FromSeconds(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Özellik bayrağı bildirimi

Her bir özellik bayrağını iki bölümden oluşur: bir ad ve bir özelliğin durumunu olup olmadığını değerlendirmek için kullanılan bir veya daha fazla filtrelerinin listesi *üzerinde* (diğer bir deyişle, değeri olduğunda `True`). Kendisi için bir özellik açık olması bir kullanım örneği bir filtre tanımlar. Bir özellik bayrağını birden çok filtre varsa, filtrenin filtrelerden birini özelliği etkin olması gerektiğini belirleyen kadar sırayla geçiş yapılır. Bu noktada, özellik bayrağını olarak kabul edilir *üzerinde* ve kalan herhangi bir filtre sonuç atlanır. Filtre özelliği etkin olduğunu belirtirse, özellik bayrağı olduğu *kapalı*.

Özellik Yöneticisi destekler *appsettings.json* özellik bayrakları için bir yapılandırma kaynağı olarak. Aşağıdaki örnek, bir json dosyasında özellik bayraklarını ayarlayın gösterilmektedir.

```JSON
"FeatureManagement": {
    "FeatureX": true, // Feature flag set to on
    "FeatureY": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Kural olarak, `FeatureManagement` bu json belgesinin bölümü için özellik bayrağı ayarları kullanılır. Yukarıdaki örnek, üç özellik bayraklarını tanımlanan kendi filtrelerle göstermektedir *EnabledFor* özelliği:

* **Feature** olduğu *üzerinde*.
* **FeatureB** olduğu *kapalı*.
* **FeatureC** adlı bir filtre belirtir *yüzdesi* ile bir *parametreleri* özelliği. *Yüzde* yapılandırılabilir bir filtre örnek olarak verilmiştir; için % 50 olasılığını belirtir **FeatureC** bayrağı olmasını *üzerinde*.

## <a name="referencing"></a>Başvuran

Zorunlu olmasa da, özellik bayrakları olarak tanımlanması gerekir `enum` değişkenleri böylece kodda kolayca başvurulabilir.

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-check"></a>Özellik bayrağı denetimi

Özellik Yönetim temel düzeni özellik bayrağı ayarlandıysa ilk Denetlenecek olan *üzerinde* ve bu durumda kapalı eylemleri gerçekleştirin.

```csharp
IFeatureManager featureManager;
...
if (featureManager.IsEnabled(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Bağımlılık ekleme

ASP.NET Core MVC, özellik Yöneticisi ' nde `IFeatureManager` bağımlılık ekleme erişilebilir.

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-action"></a>Denetleyici eylemi

MVC denetleyicileri içinde bir `Feature` özniteliği bir tam denetleyici sınıfı veya belirli bir eylem etkin olup olmadığını denetlemek için kullanılabilir. Aşağıdaki `HomeController` denetleyici gerektirir *Feature* olmasını *üzerinde* içerdiği herhangi bir eylem yürütülmeden önce.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

Aşağıdaki `Index` yukarıdaki eylem gerektiren *Feature* olmasını *üzerinde* çalıştırılmadan önce.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Ne zaman bir MVC denetleyici veya eylemin denetleme özelliği bayrağı olduğu için engellendi *kapalı*, kayıtlı bir `IDisabledFeatureHandler` çağrılır. Varsayılan `IDisabledFeatureHandler` hiç yanıt gövdesi istemciye bir 404 durum kodu döndürür.

## <a name="view"></a>Görünüm

MVC görünümlerde bir `<feature>` etiketi, bir özellik bayrağını etkin olup olmadığını üzerinde temel içeriğini işlemek için kullanılabilir.

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

## <a name="mvc-filter"></a>MVC filtre

Bir özellik bayrağını durumuna bağlı etkinleştirilir, MVC filtreleri ayarlanabilir. Aşağıdaki adlı bir MVC filtre ekler `SomeMvcFilter`. Bu filtre, MVC işlem hattı yalnızca Eğer içinde tetiklenir *Feature* etkinleştirilir.

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="route"></a>Rota

Özellik bayraklarını dinamik olarak bağlı yollar sunulabilir. Aşağıdaki ayarlar bir rota ekler `Beta` varsayılan denetleyicisi olarak yalnızca *Feature* etkinleştirilir.

```csharp
app.UseMvc(routes => {
    routes.MapRouteForFeature(nameof(MyFeatureFlags.FeatureA), "betaDefault", "{controller=Beta}/{action=Index}/{id?}");
});
```

## <a name="middleware"></a>Ara yazılım

Özellik bayrakları, uygulama dalları ve ara yazılım koşullu olarak eklemek için kullanılabilir. Bir ara yazılım bileşeni istek işlem hattı yalnızca aşağıdaki ekler *Feature* etkinleştirilir.

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Bu, daha genel bir özellik bayrağını dayalı tüm uygulama dallanma yeteneği devre dışı oluşturur.

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özellik bayraklarını kullanarak ASP.NET Core uygulamanızı uygulamak öğrendiniz `Microsoft.FeatureManagement` kitaplıkları. ASP.NET Core ve uygulama yapılandırması özellik yönetim desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [ASP.NET Core özellik bayrağını örnek kod](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Microsoft.FeatureManagement belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
