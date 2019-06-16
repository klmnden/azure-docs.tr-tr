---
title: Hatalar ve özel durumlar Azure Application Insights ile web uygulamalarında tanılama | Microsoft Docs
description: ASP.NET uygulamaları istek telemetrisi yanı sıra özel durumları yakalama.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: mbullwin
ms.openlocfilehash: cb32069de295b883cdc6d3a9fa495b1bea719c39
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60691857"
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Application Insights ile web uygulamalarınızda özel durumları tanılama
Canlı web uygulamanızdaki özel durumları tarafından bildirilen [Application Insights](../../azure-monitor/app/app-insights-overview.md). Böylece hızla nedenlerini tanılayabilirsiniz başarısız istekler, özel durumlar ve diğer olaylarla hem istemci hem de sunucu ilişkilendirebilirsiniz.

## <a name="set-up-exception-reporting"></a>Set up reporting özel durumu
* Rapor sunucusu uygulamanızdan özel durumlar için:
  * Yükleme [Application Insights SDK'sı](../../azure-monitor/app/asp-net.md) uygulama kodunuzda veya
  * IIS web sunucuları: Çalıştırma [Application Insights Aracısı](../../azure-monitor/app/monitor-performance-live-website-now.md); veya
  * Azure web uygulamaları: Ekleme [Application Insights uzantısı](../../azure-monitor/app/azure-web-apps.md)
  * Java web uygulamaları: Yükleme [Java aracı](../../azure-monitor/app/java-agent.md)
* Yükleme [JavaScript kod parçacığını](../../azure-monitor/app/javascript.md) tarayıcı özel durumları yakalamak için web sayfalarınızda.
* Bazı uygulama çerçeveleri veya bazı ayarları ile daha fazla özel durumları yakalamak için bazı fazladan adımlar uygulamanız gerekir:
  * [Web formları](#web-forms)
  * [MVC](#mvc)
  * [Web API 1.*](#web-api-1x)
  * [Web API 2.*](#web-api-2x)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Visual Studio kullanarak özel durumları tanılama
Hata ayıklamaya yardımcı olmak üzere Visual Studio'da Uygulama çözümü açın.

Uygulama sunucunuzda veya F5'i kullanarak, geliştirme makinenizde çalıştırın.

Visual Studio'da Application Insights arama penceresi açın ve uygulamanıza ilişkin olaylar görüntülenecek ayarlayın. Hata ayıklarken, Application Insights düğmesine tıklayarak bunu yapabilirsiniz.

![Projeye sağ tıklayın ve Application Insights seçme açın.](./media/asp-net-exceptions/34.png)

Yalnızca özel durumları göstermek için raporu filtreleyebilirsiniz dikkat edin.

*Özel durumları gösteriliyor? Bkz: [özel durumları yakalama](#exceptions).*

Bir özel durum raporu, yığın izlemesi göstermek için tıklayın.
İlgili kod dosyasını açmak için yığın izlemesi satır başvuru tıklayın.

Kod içinde özel olarak CodeLens özel durumları hakkında daha fazla veri gösterdiğine dikkat edin:

![CodeLens özel durum bildirimi.](./media/asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a>Azure portalını kullanarak hataları tanılama
Application Insights, izlenen uygulamalardaki hataları tanılamanıza yardımcı olmak için seçkin bir APM deneyimi ile gelir. Başlamak için Araştır bölümünde bulunan ve Application Insights kaynak menüsünde hataları seçeneğini tıklayın.
Hata oranı eğilimleri isteklerinizi, kaç tanesinin başarısız olduğunu ve kaç kullanıcının etkilendiğini gösteren bir tam ekran görünümüyle görmeniz gerekir. Sağ tarafta, bazı faydalı dağıtımlar seçili belirli görürsünüz ilk 3 yanıt kodları, ilk 3 özel durum türleri ve en üst 3 başarısız bağımlılık türleri de dahil olmak üzere, işlem başarısız.

![Hataları önceliklendirme (işlemler sekmesi) görünümü](./media/asp-net-exceptions/FailuresTriageView.png)

Tek bir tıklamayla ardından temsili örnekleri her bu alt işlemlerin için gözden geçirebilirsiniz. Özellikle, özel durumlar tanılamak için şunun gibi bir özel durum ayrıntıları dikey penceresinde gösterilmesini belirli bir özel durum sayısı tıklayabilirsiniz:

![Özel durum ayrıntıları dikey penceresi](./media/asp-net-exceptions/ExceptionDetailsBlade.png)

**Alternatif olarak,** belirli başarısız olan bir işlemi özel durum sırasında bakarak yerine, özel durumlar genel görünümden özel durumlar sekmesine geçiş yaparak başlatabilirsiniz:

![Hataları önceliklendirme (özel durumlar sekmesine) görünümü](./media/asp-net-exceptions/FailuresTriageView_Exceptions.png)

Burada, izlenen uygulama için toplanan tüm özel durumları görebilirsiniz.

*Özel durumları gösteriliyor? Bkz: [özel durumları yakalama](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Özel İzleme ve günlük verileri
Uygulamanız için belirli tanılama veri almak için kendi telemetri verileri göndermek için kod ekleyebilirsiniz. Bu istek, sayfa görünümü ve diğer otomatik olarak toplanan verilerin yanı sıra tanılama araması görüntülenir.

Birkaç seçeneğiniz vardır:

* [TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent) genellikle kullanım desenlerini izlemek için kullanılan ancak gönderir ayrıca veri tanılama araması'nda özel olayları altında görünür. Olaylar olarak adlandırılır ve dize özellikleri ve sayısal ölçümler üzerinde yapabilecekleriniz taşıyabilir [tanılama aramalarınızı filtre](../../azure-monitor/app/diagnostic-search.md).
* [TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) POST bilgileri gibi uzun veri göndermenizi sağlar.
* [TrackException()](#exceptions) yığın izlemeler gönderir. [Özel durumları hakkında daha fazla](#exceptions).
* Log4Net veya NLog gibi bir günlüğe kaydetme çerçevesi kullanıyorsanız, yapabilecekleriniz [Bu günlükleri tutmak](asp-net-trace-logs.md) tanılama aramasındaki istek ve özel durum verileri birlikte görebilirsiniz.

Bu olayları görmek için [arama](../../azure-monitor/app/diagnostic-search.md), filtre açın ve özel olay, izleme ve özel durum seçin.

![Detaylandırma](./media/asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Uygulamanız çok sayıda telemetri oluşturuyorsa, uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Aynı işlemin bir parçası olan olayları seçtikten veya bir grup olarak seçili, böylece ilgili olaylar arasında gezinebilirsiniz. [Örnekleme hakkında bilgi edinin.](../../azure-monitor/app/sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a>İstek gönderme verisi görme
İstek Ayrıntıları POST çağrısına uygulamanızda gönderilen veri içermez. Bu veriler rapor:

* [SDK yükleme](../../azure-monitor/app/asp-net.md) uygulama projesinde.
* Çağırmak için uygulamanıza kod eklemek [Microsoft.ApplicationInsights.TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace). Gönderme verisi ileti parametreyi gönderin. Yalnızca gerekli veri göndermeye şekilde bir izin verilen boyut sınırı yoktur.
* Başarısız istek araştırırken, ilişkili izlemeleri bulun.

![Detaylandırma](./media/asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a> Özel durumlar ve ilgili tanılama verilerini yakalama
İlk başta, uygulamanızda hatalarına neden tüm özel durumları portalda görmeyeceksiniz. Tüm tarayıcı özel durumları görürsünüz (kullanıyorsanız [JavaScript SDK'sı](../../azure-monitor/app/javascript.md) web sayfalarınızda). Ancak çoğu sunucu özel durumları, IIS tarafından yakalanır ve bir bit bunları görmek için kod yazmak zorundasınız.

Şunları yapabilirsiniz:

* **Açıkça özel durumları günlüğe kaydetmek** özel durumları bildirmek için özel durum işleyicileri kod ekleyerek.
* **Özel durumları otomatik olarak yakalamanıza** ASP.NET framework'ünüzün yapılandırarak. Gerekli Eklentiler, framework'ün farklı türleri için farklıdır.

## <a name="reporting-exceptions-explicitly"></a>Özel durumlar açıkça raporlama
En basit yolu, bir özel durum işleyicisinde bir çağrı işlevine yapılan çağrılardaki veriler eklemektir.

```javascript
    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }
```

```csharp
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
```

```VB
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
```

Özellikler ve ölçümler parametreler isteğe bağlıdır, ancak yararlıdır [filtreleme ve ekleme](../../azure-monitor/app/diagnostic-search.md) ek bilgiler. Örneğin, çeşitli oyunlar çalışabilen bir uygulamanız varsa, belirli bir oyun ilgili özel durum raporları nebyla nalezena. Her sözlük için istediğiniz sayıda öğe ekleyebilirsiniz.

## <a name="browser-exceptions"></a>Tarayıcı özel durumları
Çoğu tarayıcı özel durumları raporlanır.

Web sayfanızın içerik teslim ağları veya diğer etki alanlarından gelen komut dosyalarını içeriyorsa, komut dosyası etiketi öznitelik sahip olduğundan emin olun ```crossorigin="anonymous"```, ve sunucunun gönderdiği [CORS üstbilgilerini](https://enable-cors.org/). Bu, bu kaynaklardan gelen işlenmemiş JavaScript özel durumlarının bir yığın izlemesi ve ayrıntı almak olanak tanır.

## <a name="web-forms"></a>Web formları
Web formları için HTTP modülü CustomErrors ile yapılandırılmış hiçbir yeniden yönlendirmeleri olduğunda özel durumlar toplamanız mümkün olacaktır.

Ancak, etkin yeniden yönlendirmeleri varsa Global.asax.cs uygulama_hatası işlevine aşağıdaki satırları ekleyin. (Zaten yoksa, Global.asax dosyası ekleyin.)

```csharp
    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }
```

## <a name="mvc"></a>MVC
Application Insights Web SDK sürüm 2.6 ile başlayan (beta3 ve sonraki sürümler), Application Insights toplanan işlenmemiş özel durumlar MVC 5 + denetleyicileri yöntemleri otomatik olarak oluşturulur. (Aşağıdaki örneklerde açıklandığı gibi) bu tür özel durumların izlemek için özel bir işleyici daha önce eklediyseniz, özel durumların çift izleme engellemek için kaldırabilir.

Bir özel durum filtreleri işleyemiyor sayısını vardır. Örneğin:

* Denetleyici oluşturucular oluşturulan bir özel durumlar.
* İleti işleyicilerini oluşturulan bir özel durumlar.
* Yönlendirme sırasında oluşturulan özel durumlar.
* Yanıt içeriği seri hale getirme sırasında oluşturulan bir özel durumlar.
* Uygulama başlatma sırasında özel bir durum oluştu.
* Arka plan görevlerinin özel bir durum oluştu.

Tüm özel durumları *işlenen* uygulama tarafından yine de el ile izlenmesi gerekir.
İşlenmeyen özel durumlar genellikle denetleyicilerinden kaynaklanan 500 "İç sunucu hatası" yanıt olarak neden. Bu tür yanıt sonucu olarak işlenen özel durum (veya hiç özel durum) el ile oluşturulursa karşılık gelen istek telemetrisi ile izlenen `ResultCode` 500, ancak Application Insights SDK'sı karşılık gelen bir özel durum izleyemiyor.

### <a name="prior-versions-support"></a>Önceki sürümleri desteği
Application Insights Web SDK 2.5 (ve öncesi), MVC 4 (ve öncesi) kullanırsanız, özel durumları izlemek için aşağıdaki örneklere bakın.

Varsa [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) yapılandırma `Off`, özel durumlar için kullanılabilecek sonra [HTTP modülü](https://msdn.microsoft.com/library/ms178468.aspx) toplanacak. Ancak, bu ise `RemoteOnly` (varsayılan), veya `On`, özel durum temizlenir ve Application Insights'ı otomatik olarak toplamak için kullanılabilir olmayacaktır. Bunu geçersiz kılarak düzeltelim [System.Web.Mvc.HandleErrorAttribute sınıfı](https://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)ve farklı MVC sürümleri için aşağıda gösterildiği gibi geçersiz kılınan sınıf uygulama ([GitHub kaynak](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

```csharp
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
```

#### <a name="mvc-2"></a>MVC 2
Yeni özniteliğinizi denetleyicilerinizi HandleError özniteliği değiştirin.

```csharp
    namespace MVC2App.Controllers
    {
        [AiHandleError]
        public class HomeController : Controller
        {
    ...
```

[Örnek](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Kayıt `AiHandleErrorAttribute` Global.asax.cs genel filtre olarak:

```csharp
    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...
```

[Örnek](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
AiHandleErrorAttribute FilterConfig.cs genel filtre olarak kaydedin:

```csharp
    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }
```

[Örnek](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api"></a>Web API
Application Insights Web SDK sürüm 2.6 ile başlayan (beta3 ve sonraki sürümler), denetleyici yöntemlerinde Webapı için 2 + otomatik olarak oluşturulan Application Insights toplanan işlenmemiş özel durumlar. (Aşağıdaki örneklerde açıklandığı gibi) bu tür özel durumların izlemek için özel bir işleyici daha önce eklediyseniz, özel durumların çift izleme engellemek için kaldırabilir.

Bir özel durum filtreleri işleyemiyor sayısını vardır. Örneğin:

* Denetleyici oluşturucular oluşturulan bir özel durumlar.
* İleti işleyicilerini oluşturulan bir özel durumlar.
* Yönlendirme sırasında oluşturulan özel durumlar.
* Yanıt içeriği seri hale getirme sırasında oluşturulan bir özel durumlar.
* Uygulama başlatma sırasında özel bir durum oluştu.
* Arka plan görevlerinin özel bir durum oluştu.

Tüm özel durumları *işlenen* uygulama tarafından yine de el ile izlenmesi gerekir.
İşlenmeyen özel durumlar genellikle denetleyicilerinden kaynaklanan 500 "İç sunucu hatası" yanıt olarak neden. Bu tür yanıt sonucu olarak işlenen özel durum (veya hiç özel durum) el ile oluşturulursa karşılık gelen bir istek telemetrisi ile izlenen `ResultCode` 500, ancak Application Insights SDK'sı karşılık gelen bir özel durum izleyemiyor.

### <a name="prior-versions-support"></a>Önceki sürümleri desteği
Application Insights Web SDK 2.5 (ve önceki) Webapı 1'i (ve öncesi) kullanırsanız, özel durumları izlemek için aşağıdaki örneklere bakın.

#### <a name="web-api-1x"></a>Web API 1.x
System.Web.Http.Filters.ExceptionFilterAttribute geçersiz kıl:

```csharp
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
```

Geçersiz kılınan bu öznitelik için belirli denetleyicileri ekleme veya WebApiConfig sınıfı genel filtre yapılandırmasında ekleyin:

```csharp
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
```

[Örnek](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

#### <a name="web-api-2x"></a>Web API 2.x
Bir ıexceptionlogger ekleyin:

```csharp
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
```

Bu, WebApiConfig Hizmetleri'nde ekleyin:

```csharp
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
```

[Örnek](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Alternatif, şunları yapabilirsiniz:

1. Yalnızca ExceptionHandler özel bir IExceptionHandler uygulaması ile değiştirin. Framework seçin (ne zaman bağlantı örneği için İptal değil) göndermek için hangi yanıt iletisi hala mümkün olduğunda bu yalnızca çağrılır
2. (Açıklandığı gibi Web API 1.x denetleyicilerinde yukarıdaki bölümde) - tüm durumlarda çağrılır değil özel durum filtreleri.

## <a name="wcf"></a>WCF
Attribute'u genişleten ve Ierrorhandler ve IServiceBehavior'ı uygulayan bir sınıf ekleyin.

```csharp
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

Add the attribute to the service implementations:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...
```

[Örnek](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Özel durum performans sayaçları
Varsa [Application Insights aracısı yüklü](../../azure-monitor/app/monitor-performance-live-website-now.md) sunucunuzda bir grafik .NET tarafından ölçülür özel durumlar oranının alabilirsiniz. Bu, işlenen ve yakalanamayan .NET özel durumları içerir.

Ölçüm Gezgini dikey penceresini açın, yeni bir grafik ekleyin ve seçin **özel durum oranı**listelenen performans sayaçlarını altında.

.NET framework ile özel durumların sayısı için bir zaman aralığı sayım ve aralık uzunluğu tarafından bölünmesi oranı hesaplar.

Bu Application Insights portalında TrackException raporları sayım tarafından hesaplanan 'Özel durumlar' sayısı farklıdır. Örnekleme aralığı farklıdır ve SDK'sı tüm için TrackException raporları işlenen ve yakalanamayan özel durumları göndermez.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
* [REST, SQL ve diğer bağımlılıklara yapılan çağrıları izleme](../../azure-monitor/app/asp-net-dependencies.md)
* [Sayfa yükleme süreleri, tarayıcı özel durumları ve AJAX çağrıları izleme](../../azure-monitor/app/javascript.md)
* [İzleyici performans sayaçları](../../azure-monitor/app/performance-counters.md)
