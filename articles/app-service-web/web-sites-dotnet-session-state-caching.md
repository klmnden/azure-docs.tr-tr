---
title: "Azure App Service’teki Azure Redis Cache ile oturum durumu"
description: "ASP.NET oturum durumu önbelleğe alma işlemini desteklemek için Azure Önbellek Hizmeti’ni nasıl kullanacağınızı öğrenin."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
ms.translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: a682e51bfaed9056b170c3e9473180ca210557b9
ms.contentlocale: tr-tr
ms.lasthandoff: 01/20/2017


---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Azure App Service’teki Azure Redis Cache ile oturum durumu
Bu konuda, oturum durumu için Azure Redis Cache Hizmetini nasıl kullanacağınız açıklanmaktadır.

ASP.NET web uygulamanız oturum durumu kullanıyorsa, bir dış oturum durumu sağlayıcısı (Redis Cache Hizmeti veya SQL Server oturum durumu sağlayıcısı) yapılandırmanız gerekir. Oturum durumu kullanıyor, ancak dış sağlayıcı kullanmıyorsanız; web uygulamanızın tek bir örneği ile sınırlı kalırsınız. Redis Cache Hizmeti etkinleştirmesi en hızlı ve en kolay hizmettir.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="createcache"></a>Önbelleği oluşturma
Önbelleği oluşturmak için [şu yönergeleri](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) uygulayın.

## <a id="configureproject"></a>Web uygulamanıza RedisSessionStateProvider NuGet paketi ekleme
NuGet `RedisSessionStateProvider` paketini yükleyin.  Paket yöneticisi konsolundan (**Araçlar** > **NuGet Paketi Yöneticisi** > **Paket Yöneticisi Konsolu**) yüklemek için aşağıdaki komutu kullanın:

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

**Araçlar** > **NuGet Paketi Yöneticisi** > **Çözüm için NugGet Paketlerini Yönet** konumuna gidin ve `RedisSessionStateProvider` araması yapın.

Daha fazla bilgi için bkz. [NuGet RedisSessionStateProvider sayfası](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) ve [Önbellek istemcisini yapılandırma](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

## <a id="configurewebconfig"></a>Web.Config Dosyasını Değiştirme
NuGet paketi derleme başvuruları için yapmanın yanı sıra, *web.config* dosyasında saplama girdileri ekler. 

1. *web.config* dosyasını açın ve **sessionState** öğesini bulun.
2. `host`, `accessKey`, `port` için değerleri girin (SSL bağlantı noktası 6380 olmalıdır) `SSL` değerini `true` olarak ayarlayın. Bu değerleri, önbellek örneğinizin [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) dikey penceresinden alabilirsiniz. Daha fazla bilgi için bkz. [Önbelleğe bağlanma](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). SSL olmayan bağlantı noktasının yeni önbellekler için varsayılan olarak devre dışı bırakıldığını unutmayın. SSL olmayan bağlantı noktasını etkinleştirme hakkında daha fazla bilgi için [Azure Redis Cache’te bir önbellek yapılandırma](https://msdn.microsoft.com/library/azure/dn793612.aspx) konusundaki [Erişim Bağlantı Noktaları](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) bölümüne bakın. Aşağıdaki biçimlendirmede *web.config* dosyası, özellikle *bağlantı noktası*, *ana bilgisayar*, accessKey *ve* ssl* ile ilgili yapılan değişiklikler gösterilir.
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <a id="usesessionobject"></a>Kodlarda Oturum Nesnesi Kullanma
Son adım, ASP.NET kodunuzda Oturum nesnesi kullanmaya başlamaktır. **Session.Add** yöntemini kullanarak nesneleri oturum durumuna ekleyin. Bu yöntem, öğeleri oturum durumu önbelleğine depolamak için anahtar-değer çiftlerini kullanır.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Aşağıdaki kod, oturum durumundan bu değeri alır.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

Web uygulamanızdaki nesneleri önbelleğe almak için Redis Cache’i de kullanabilirsiniz. Daha fazla bilgi için bkz. [15 dakikada Azure Redis Cache ile MVC film uygulaması](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
ASP.NET oturum durumu kullanma hakkında daha fazla ayrıntı için bkz. [ASP.NET Oturum Durumuna Genel Bakış][ASP.NET Session State Overview].

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)
  
  *Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)*

[installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png


