<properties 
    pageTitle="Azure App Service’teki Azure Redis önbelleği ile oturum durumu" 
    description="ASP.NET oturum durumu önbelleğe alma işlemini desteklemek için Azure Önbellek Hizmeti’ni nasıl kullanacağınızı öğrenin." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="02/27/2016" 
    ms.author="riande"/>


# Azure App Service’teki Azure Redis önbelleği ile oturum durumu


Bu konuda, oturum durumu için Azure Redis Önbelleği Hizmeti’ni nasıl kullanacağınız açıklanmaktadır.

ASP.NET web uygulamanız oturum durumu kullanıyorsa, bir dış oturum durumu sağlayıcısı (Redis Önbelleği Hizmeti veya SQL Server oturum durumu sağlayıcısı) yapılandırmanız gerekir. Oturum durumu kullanıyor, ancak dış sağlayıcı kullanmıyorsanız; web uygulamanızın tek bir örneği ile sınırlı kalırsınız. Redis Önbelleği Hizmeti etkinleştirmesi en hızlı ve en kolay hizmettir.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Önbelleği Oluşturma
Önbelleği oluşturmak için [şu yönergeleri](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) uygulayın.

##<a id="configureproject"></a>Web uygulamanıza RedisSessionStateProvider NuGet paketi ekleme
NuGet `RedisSessionStateProvider` paketini yükleyin.  Paket yöneticisi konsolundan (**Araçlar** > **NuGet Paketi Yöneticisi** > **Paket Yöneticisi Konsolu**) yüklemek için aşağıdaki komutu kullanın:

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
**Araçlar** > **NuGet Paketi Yöneticisi** > **Çözüm için NugGet Paketlerini Yönet** konumuna gidin ve `RedisSessionStateProvider` araması yapın.

Daha fazla bilgi için bkz. [NuGet RedisSessionStateProvider sayfası](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) ve [Önbellek istemcisini yapılandırma](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Web.Config Dosyasını Değiştirme
NuGet paketi derleme başvuruları için yapmanın yanı sıra, *web.config* dosyasında saplama girdileri ekler. 

1. *web.config* dosyasını açın ve **sessionState** öğesini bulun.

1. `host`, `accessKey`, `port` için değerleri girin (SSL bağlantı noktası 6380 olmalıdır) `SSL` değerini `true` olarak ayarlayın. Bu değerleri, önbellek örneğinizin [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) dikey penceresinden alabilirsiniz. Daha fazla bilgi için bkz. [Önbelleğe bağlanma](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). SSL olmayan bağlantı noktasının yeni önbellekler için varsayılan olarak devre dışı bırakıldığını unutmayın. SSL olmayan bağlantı noktasını etkinleştirme hakkında daha fazla bilgi için [Azure Redis Önbelleği’nde bir önbellek yapılandırma](https://msdn.microsoft.com/library/azure/dn793612.aspx) konusundaki [Erişim Bağlantı Noktaları](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) bölümüne bakın. Aşağıdaki biçimlendirmede *web.config* dosyası, özellikle *bağlantı noktası*, *ana bilgisayar*, accessKey* ve *ssl* ile ilgili yapılan değişiklikler gösterilir.

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


##<a id="usesessionobject"></a> Kodlarda Oturum Nesnesi Kullanma
Son adım, ASP.NET kodunuzda Oturum nesnesi kullanmaya başlamaktır. **Session.Add** yöntemini kullanarak nesneleri oturum durumuna ekleyin. Bu yöntem, öğeleri oturum durumu önbelleğine depolamak için anahtar-değer çiftlerini kullanır.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Aşağıdaki kod, oturum durumundan bu değeri alır.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Web uygulamanızdaki nesneleri önbelleğe almak için Redis Önbelleği’ni de kullanabilirsiniz. Daha fazla bilgi için bkz. [15 dakikada Azure Redis Önbelleği ile MVC film uygulaması](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
ASP.NET oturum durumu kullanma hakkında daha fazla ayrıntı için bkz. [ASP.NET Oturum Durumuna Genel Bakış][].

>[AZURE.NOTE] Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.

## Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [yüklü en son sürüm]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET Oturum Durumuna Genel Bakış]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 



<!--HONumber=Jun16_HO2-->


