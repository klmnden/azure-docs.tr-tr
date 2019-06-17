---
title: ASP.NET Core uygulamaları - Azure App Service'ı yapılandırma | Microsoft Docs
description: ASP.NET Core uygulamaları, Azure App Service'te çalışacak şekilde yapılandırma hakkında bilgi edinin
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: f2781e3cc2433f73ba7ff33e5c452e29de746adf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956210"
---
# <a name="configure-a-linux-aspnet-core-app-for-azure-app-service"></a>Bir Linux yapılandırma ASP.NET Core uygulaması Azure App Service için

ASP.NET Core uygulamaları, derlenmiş ikili dosyaları dağıtılmalıdır. Visual Studio yayımlama aracını çözüm derlenir ve App Service dağıtım altyapısı, kod deposundaki ilk dağıtır ve ardından ikili dosyaları derler derlenmiş ikili dosyaları doğrudan dağıtır.

Bu kılavuz, App Service'te yerleşik bir Linux kapsayıcı kullanan geliştiriciler temel kavramları ve ASP.NET Core için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izleyin [ASP.NET Core hızlı](quickstart-dotnetcore.md) ve [ASP.NET Core ile SQL veritabanı Öğreticisi](tutorial-dotnetcore-sqldb-app.md) ilk.

## <a name="show-net-core-version"></a>.NET Core sürümünün Göster

Geçerli .NET Core sürümünün göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Tüm desteklenen .NET Core sürümleri göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep DOTNETCORE
```

## <a name="set-net-core-version"></a>.NET Core sürümü Ayarla

Aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com) 2.1 için .NET Core sürümünüzü ayarlamak için:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "DOTNETCORE|2.1"
```

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişme

Uygulama hizmetinde [uygulama ayarlarını belirlemek](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) , uygulama kodunuz dışında. Ardından, standart ASP.NET deseni kullanarak erişebilirsiniz:

```csharp
include Microsoft.Extensions.Configuration;
// retrieve App Service app setting
System.Configuration.ConfigurationManager.AppSettings["MySetting"]
// retrieve App Service connection string
Configuration.GetConnectionString("MyDbConnection")
```

Aynı ada sahip App Service ve bir uygulama ayarı yapılandırırsanız *Web.config*, App Service değer Web.config değerine göre önceliklidir. Uygulamayı yerel olarak hata ayıklama Web.config değeri sağlar, ancak App Service değeri ürün üretim ayarlarla uygulamayı çalıştırma olanak tanır. Bağlantı dizeleri gibi çalışır. Bu şekilde kod deponuzu dışında uygulama gizli dizilerini korumak ve kodunuzu değiştirmeden uygun değerleri erişim.

## <a name="get-detailed-exceptions-page"></a>Ayrıntılı özel durumlar sayfası

ASP.NET uygulamanızı Visual Studio hata ayıklayıcıda bir özel durum oluşturur, tarayıcı ayrıntılı özel durum sayfasında görüntüler, ancak App Service'te bu sayfa tarafından genel değiştirilir **HTTP 500** hata veya **bir hata oluştu İsteğiniz işleniyor.** İleti. App Service'te ayrıntılı özel durum sayfasında görüntüleyin, ekleyin `ASPNETCORE_ENVIRONMENT` uygulama ayarı için aşağıdaki komutu çalıştırarak uygulamanızı <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>HTTPS oturumu algılayın

App Service'te [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) tüm HTTPS isteklerini, şifrelenmemiş HTTP istekleri olarak uygulamanızı ulaşmak için Ağ Yük Dengeleyiciler, olur. Uygulama mantığınız kullanıcı veya şifrelenir, istekleri bilmeniz gerekiyorsa iletilen üstbilgileri Ara yazılımında yapılandırma *Startup.cs*:

- Ara yazılımla yapılandırma [ForwardedHeadersOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerinde `Startup.ConfigureServices`.
- Özel IP adresi aralıkları ara yazılım, App Service yük dengeleyici güvenebilir bilinen ağa ekleyin.
- Çağırma [UseForwardedHeaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` diğer middlewares çağırmadan önce.

Üç tüm öğeleri bir araya getirildiğinde, kodunuz aşağıdaki örnekteki gibi görünür:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders =
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:10.0.0.0"), 104));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:192.168.0.0"), 112));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:172.16.0.0"), 108));
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    ...

    app.UseMvc();
}
```

Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer).

## <a name="deploy-multi-project-solutions"></a>Birden çok proje çözümü dağıtma

Dağıtım altyapısı ile bir ASP.NET depo dağıtırken bir *.csproj* proje kök dizininde, motorun dosya dağıtır. Bir ASP.NET deposuyla dağıtırken bir *.sln* ilk Web sitesini veya Web uygulama projesi App Service uygulaması bulduğu kök dizininde altyapısı dosyasını seçer. İstediğiniz projeyi seçin değil altyapısı için mümkündür.

Bir çoklu proje çözümü dağıtmak için App Service'te iki farklı şekilde kullanmak için projeyi belirtebilirsiniz:

### <a name="using-deployment-file"></a>.Deployment dosyası kullanma

Ekleme bir *.deployment* depo köküne dosya ve aşağıdaki kodu ekleyin:

```
[config]
project = <project-name>/<project-name>.csproj
```

### <a name="using-app-settings"></a>Uygulama ayarlarını kullanarak

İçinde <a target="_blank" href="https://shell.azure.com">Azure Cloud Shell</a>, aşağıdaki CLI komutunu çalıştırarak App Service uygulamanız için bir uygulama ayarı ekleyin. Değiştirin  *\<-adı >* ,  *\<kaynak grubu adı >* , ve  *\<proje adı >* uygun değerlerle .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturum aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL veritabanı ile ASP.NET Core uygulaması](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)