---
title: "Hataları ve özel durumları Azure Application Insights ile web uygulamalarında tanılama | Microsoft Docs"
description: "ASP.NET uygulamaları isteği telemetri ile birlikte özel durumları yakalar."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: mbullwin
ms.openlocfilehash: d6a0b945bad36842142d16a4840c9c3d69e1564e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Web uygulamalarınızı Application Insights ile özel durumları tanılama
Özel durumlar, canlı web uygulamanızda tarafından bildirilen [Application Insights](app-insights-overview.md). Böylece hızla nedenlerini tanılamak özel durumlar ve hem istemci hem de sunucu, diğer olayları ile başarısız olan istekler ilişkilendirebilirsiniz.

## <a name="set-up-exception-reporting"></a>Set up reporting özel durumu
* Özel durumlar için sunucu uygulamanızdan bildirdi:
  * Yükleme [Application Insights SDK'sı](app-insights-asp-net.md) , uygulama kodunuzda veya
  * IIS web sunucuları: çalıştırmak [Application Insights Aracısı](app-insights-monitor-performance-live-website-now.md); veya
  * Azure web uygulamaları: eklemek [uygulama Öngörüler uzantısı](app-insights-azure-web-apps.md)
  * Java web uygulamaları: yükleme [Java aracı](app-insights-java-agent.md)
* Yükleme [JavaScript kod parçacığı](app-insights-javascript.md) tarayıcı özel durumları yakalamak için web sayfalarındaki.
* Bazı uygulama çerçeveleri veya bazı ayarlar ile daha fazla özel durumları yakalamak için bazı ek adımlar gerekir:
  * [Web formları](#web-forms)
  * [MVC](#mvc)
  * [Web API 1.*](#web-api-1x)
  * [Web API 2.*](#web-api-2x)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Visual Studio kullanarak tanılama özel durumlar
Hata ayıklamaya yardımcı olmak için Visual Studio'da Uygulama çözümü açın.

Sunucunuzda veya F5 kullanarak geliştirme makinenizde uygulamayı çalıştırın.

Visual Studio'da Uygulama Insights arama penceresi açın ve uygulamanızdan olayları görüntülemek üzere ayarlayın. Hatalarını ayıkladığınız sırada yalnızca Application Insights düğmesine tıklayarak bunu yapabilirsiniz.

![Projeye sağ tıklayın ve Application Insights seçme açın.](./media/app-insights-asp-net-exceptions/34.png)

Yalnızca özel durumları göstermek için raporu filtreleyebilirsiniz dikkat edin.

*Özel durumlar gösteren? Bkz: [özel durumları yakalama](#exceptions).*

Yığın izleme göstermek için bir özel durum raporu tıklatın.
İlgili kod dosyasını açmak için yığın izlemesi satırı başvurusunda'ı tıklatın.  

Kodda, CodeLens özel durumlar hakkında veri gösterdiğine dikkat edin:

![Özel durumlar CodeLens bildirimi.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a>Azure portalını kullanarak hataları tanılama
Application Insights ile birlikte gelen hataları izlenen uygulamalarınızda tanılamanıza yardımcı olacak bir seçkin APM deneyimi. Araştır bölümünde bulunan Application Insights kaynağı menüde hataları seçeneğinde başlatmak için tıklatın. Hata oranı eğilimleri isteklerinizi, kaç tanesinin başarısız olduğunu ve kaç tane kullanıcıların etkilendiğini gösteren bir tam ekran görünümü görmeniz gerekir. Sağ tarafta seçili belirli en yararlı dağıtımları bazıları göreceğiniz en üst 3 yanıt kodları, en üst 3 özel durum türleri ve bağımlılık türleri başarısız üst 3 dahil olmak üzere, işlem başarısız. 

![Görünüm (işlemler sekmesi) hataları Önceliklendirme](./media/app-insights-asp-net-exceptions/FailuresTriageView.png)

Tek bir tıklatmayla, ardından temsilcisi örnekleri her işlemlerinin şu alt kümeleri için gözden geçirebilirsiniz. Özellikle, özel durumlar tanılamak için bunun gibi bir özel durum ayrıntıları dikey gösterilmesini belirli bir özel durum sayısı tıklatabilirsiniz:

![Özel durum ayrıntıları dikey penceresi](./media/app-insights-asp-net-exceptions/ExceptionDetailsBlade.png)

**Alternatif olarak,** belirli başarısız olan işlemi özel durum sırasında arayan yerine özel durumları, genel görünümden özel durumlar sekmesine geçerek başlatabilirsiniz:

![Görünüm (özel durumlar sekmesini) hataları Önceliklendirme](./media/app-insights-asp-net-exceptions/FailuresTriageView_Exceptions.png)

Burada, izlenen uygulama için toplanan tüm özel durumları görebilirsiniz.

*Özel durumlar gösteren? Bkz: [özel durumları yakalama](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Özel İzleme ve günlük verileri
Tanılama veri uygulamanıza özgü almak için kendi telemetri verileri göndermek için kod ekleyebilirsiniz. Bu istek, sayfa görünümü ve diğer otomatik olarak toplanan verilerin yanı sıra tanılama arama görüntülenir.

Birkaç seçeneğiniz vardır:

* [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) genellikle kullanım desenlerini izlemek için kullanılır, ancak gönderir ayrıca veri tanılama Arama'da özel olayları altında görüntülenir. Olayları adlandırılır ve dize özellikleri ve üzerinde yapabilecekleriniz sayısal ölçümleri taşıyabilir [tanılama aramalarınız filtre](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) POST bilgileri gibi daha uzun veri göndermesini sağlar.
* [TrackException()](#exceptions) yığın izlemeler gönderir. [Özel durumlar hakkında daha fazla](#exceptions).
* Log4Net veya NLog gibi günlük framework kullanırsanız, yapabilecekleriniz [Bu günlükleri yakalama](app-insights-asp-net-trace-logs.md) ve bunları istek ve özel durum verilerin yanında tanılama arama bölümüne bakın.

Bu olayları görmek için açık [arama](app-insights-diagnostic-search.md)filtre açın ve ardından özel olay, izleme veya özel durumu seçin.

![Detaylandırma](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Uygulamanız çok sayıda telemetri oluşturuyorsa, uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Aynı işlem parçası olan olayları seçili veya böylece ilgili olaylar arasında gezinebilirsiniz grup olarak işaretli. [Örnekleme hakkında bilgi edinin.](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a>İstek gönderme verisi görme
İstek Ayrıntıları uygulamanıza POST çağrıda gönderilen verileri eklemeyin. Bu veriler olduğunu bildirdi:

* [SDK yükleme](app-insights-asp-net.md) uygulama projenizdeki.
* Kod çağırmak için uygulamanızda ekleme [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). Gönderme verisi ileti parametresinde gönderin. Yalnızca gerekli verileri göndermeye şekilde bir izin verilen boyut sınırı yoktur.
* Başarısız bir istek incelediğinizde ilişkilendirilmiş izlemeleri bulun.  

![Detaylandırma](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a>Özel durumlar ve ilgili Tanılama verileri yakalama
İlk başta, uygulamanızda hatalarına neden tüm özel durumları portalında görmez. Tarayıcı özel durumlar görürsünüz (kullanıyorsanız, [JavaScript SDK'sı](app-insights-javascript.md) web sayfalarınızda). Ancak çoğu sunucu özel durumları IIS tarafından yakalanan ve bit bunları görmek için kod yazmak zorunda.

Şunları yapabilirsiniz:

* **Özel durumlar açıkça oturum** kodu özel durumlar bildirmek için özel durum işleyicileri ekleyerek.
* **Özel durumlar otomatik olarak yakalama** , ASP.NET framework yapılandırarak. Gerekli eklemeleri framework'ün farklı türleri için farklıdır.

## <a name="reporting-exceptions-explicitly"></a>Özel durumlar açıkça raporlama
En basit yolu, bir özel durum işleyici TrackException() çağrısı eklemektir.

JavaScript

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

VB

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

Özellikleri ve ölçülerini parametreler isteğe bağlıdır, ancak için yararlı olan [filtreleme ve ekleyerek](app-insights-diagnostic-search.md) ek bilgiler. Örneğin, birkaç oyunlar çalıştırabilirsiniz bir uygulamanız varsa, belirli bir oyuna ilgili tüm özel durum raporları bulunamadı. Her sözlük için istediğiniz kadar öğe ekleyebilirsiniz.

## <a name="browser-exceptions"></a>Tarayıcı özel durumları
Çoğu tarayıcı özel durumları raporlanır.

Web sayfanızın içerik teslim ağlara veya diğer etki alanından komut dosyaları içeriyorsa, komut dosyası etiketinin öznitelik olduğundan emin olun ```crossorigin="anonymous"```, ve sunucunun gönderdiği [CORS üstbilgilerini](http://enable-cors.org/). Bu, bu kaynaklardan işlenmemiş JavaScript özel durumlarının bir yığın izleme ve ayrıntılı almaya izin verir.

## <a name="web-forms"></a>Web formları
Web formları için HTTP modülü CustomErrors ile yapılandırılmış hiçbir yeniden yönlendirmeleri olduğunda özel durumları Topla mümkün olacaktır.

Ancak etkin yeniden yönlendirmeleri varsa, Global.asax.cs uygulama_hatası işlevinde aşağıdaki satırları ekleyin. (Zaten yoksa, Global.asax dosyası ekleyin.)

*C#*

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a>MVC
Varsa [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) Yapılandırması `Off`, özel durumlar için kullanılabilecek sonra [HTTP modülü](https://msdn.microsoft.com/library/ms178468.aspx) toplanacak. Ancak, bu ise `RemoteOnly` (varsayılan) veya `On`, sonra özel temizlenmiş ve Application Insights'ı otomatik olarak toplamak için kullanılamaz. Geçersiz kılarak düzeltme [System.Web.Mvc.HandleErrorAttribute sınıfı](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)ve farklı MVC sürümleri için aşağıda gösterildiği gibi geçersiz kılınan sınıf uygulama ([github kaynağına](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report the exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a>MVC 2
Yeni özniteliğinizi denetleyicilerinizi HandleError özniteliği değiştirin.

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[Örnek](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Kayıt `AiHandleErrorAttribute` Global.asax.cs genel filtre olarak:

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[Örnek](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
FilterConfig.cs genel filtre olarak AiHandleErrorAttribute kaydedin:

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[Örnek](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a>Web API 1.x
System.Web.Http.Filters.ExceptionFilterAttribute geçersiz kıl:

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

Geçersiz kılınan bu öznitelik için belirli denetleyicileri ekleme veya WebApiConfig sınıfı genel filtre yapılandırmasında eklemek:

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[Örnek](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

Özel durum filtreleri işleyemiyor durumlar vardır. Örneğin:

* Denetleyici oluşturucular oluşturulan özel durumları.
* İleti işleyicileri oluşturulan özel durumları.
* Yönlendirme sırasında oluşturulan özel durumları.
* Yanıtın içerik serileştirilmesi sırasında oluşturulan özel durumları.

## <a name="web-api-2x"></a>Web API 2.x
IExceptionLogger uygulaması ekleyin:

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

Bu, WebApiConfig Hizmetleri'nde ekleyin:

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  }

[Örnek](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Alternatif, şunları yapabilir:

1. Yalnızca ExceptionHandler özel bir IExceptionHandler uygulama ile değiştirin. Framework'te (zaman bağlantı örneği için durduruldu değil) göndermek için hangi yanıt iletisi seçmek hala mümkün olduğunda bu yalnızca çağrılır
2. Her durumda değil olarak adlandırılan özel durum filtreleri (açıklandığı gibi Web API 1.x denetleyicilerinde yukarıdaki bölümde) -.

## <a name="wcf"></a>WCF
Öznitelik genişleten ve IErrorHandler ve IServiceBehavior uygulayan bir sınıf ekleyin.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

Öznitelik, hizmet uygulamaları ekleyin:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[Örnek](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Özel durum performans sayaçları
Varsa [Application Insights aracısı yüklü](app-insights-monitor-performance-live-website-now.md) sunucunuz üzerinde bir grafik .NET tarafından ölçülür özel durumlara oranı elde edebilirsiniz. Bu, işlenen ve işlenmeyen özel durumları .NET içerir.

Ölçüm Gezgini dikey penceresini açın, yeni bir grafik ekleyin ve seçin **özel durum oranı**, performans sayaçları altında listelenen.

.NET framework ile özel durum sayısı bir zaman aralığı sayım ve aralık uzunluğu ile bölünmesi oranı hesaplar.

Bu TrackException raporları sayım Application Insights portalı tarafından hesaplanan 'Özel durumları' sayısı farklıdır. Örnekleme aralıkları farklıdır ve SDK TrackException raporları tüm işlenen ve işlenmeyen özel durumlar göndermez.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Sonraki adımlar
* [REST, SQL ve diğer çağrıları bağımlılıkları izleme](app-insights-asp-net-dependencies.md)
* [Sayfa yükleme sürelerinin, tarayıcı özel durumlar ve AJAX çağrıları izleme](app-insights-javascript.md)
* [İzleyici performans sayaçları](app-insights-performance-counters.md)
