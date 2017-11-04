---
title: "Azure Application Insights SDK ön işleme ve filtreleme | Microsoft Docs"
description: "Telemetri işlemciler ve SDK'sı Telemetri başlatıcıları filtre veya telemetri Application Insights portalına gönderilmeden önce veri özellikleri eklemek için yazın."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: borooji;mbullwin
ms.openlocfilehash: 9261f44a0c0400a0a8d908b0ff72318c637771de
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtreleme ve Application Insights SDK'sı telemetri ön işleme


Yazma ve eklentiler nasıl telemetri yakalanan ve Application Insights hizmetine gönderilmeden önce işlenen özelleştirmek Application Insights SDK'sı için yapılandırın.

* [Örnekleme](app-insights-sampling.md) , istatistikleri etkilemeden telemetri hacmini azaltır. Birlikte tutar ilgili veri noktaları aralarında bir sorunu tanılamada gidebilirsiniz. Portalda, toplam sayıları için örnekleme dengelemek için çarpılır.
* Telemetri işlemcilerle filtreleme [ASP.NET](#filtering) veya [Java](app-insights-java-filter-telemetry.md) seçin veya sunucuya gönderilmeden önce SDK telemetri değiştirme olanak sağlar. Örneğin, robots istekleri hariç tutarak telemetri hacmi azaltabilir. Ancak filtreleme örnekleme daha trafiğini azaltmak için daha basit bir yaklaşım. Ne iletilen üzerinde daha fazla denetim sağlar, ancak tüm başarılı istek filtresi, örneğin, istatistikleri - etkiler farkında olması gerekir.
* [Telemetri başlatıcıları Özellikler ekleme](#add-properties) telemetri standart modüllerden dahil uygulamanızın gönderilen telemetri için. Örneğin, hesaplanan değerler ekleyebilirsiniz; veya veri Portalı'nda filtrelemek için sürüm numaralarıyla.
* [SDK API'si](app-insights-api-custom-events-metrics.md) özel olayları ve ölçümleri göndermek için kullanılır.

Başlamadan önce:

* Application Insights yükleme [ASP.NET SDK](app-insights-asp-net.md) veya [Java için SDK](app-insights-java-get-started.md) uygulamanızda.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filtreleme: ITelemetryProcessor
Bu teknik ne dahil ya da telemetri akışına dışlanan üzerinden daha doğrudan denetim olanağı verir. Örnekleme ile birlikte kullanın ya da ayrı olarak.

Telemetri filtrelemek için bir telemetri işlemci yazma ve SDK'sı ile kaydedin. Tüm telemetri işlemcinizin gider ve akıştan bırakma ya da özellikleri eklemek seçebilirsiniz. Bu, HTTP isteği Toplayıcı ve bağımlılık toplayıcısı gibi standart modüllerden telemetri yanı sıra, kendiniz yazmıştır telemetri içerir. Örneğin, robots ya da başarılı bağımlılık çağrıları isteklerini hakkında telemetriyi filtre olabilir.

> [!WARNING]
> SDK'dan gelen gönderilen telemetriyi filtreleme işlemciler kullanarak portalında gördüğünüz istatistikleri eğme ve ilgili öğeler izleyin zorlaştırır.
>
> Bunun yerine, kullanmayı [örnekleme](app-insights-sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Bir telemetri işlemci (C#) oluşturma
1. Projenize Application Insights SDK'sı sürüm 2.0.0 olduğunu doğrulayın veya daha sonra. Visual Studio Çözüm Gezgini'nde projenize sağ tıklayın ve NuGet paketlerini Yönet'i seçin. NuGet Paket Yöneticisi'nde Microsoft.applicationınsights.Web denetleyin.
2. Bir filtre oluşturmak için ITelemetryProcessor uygulayın. Telemetri modülü, telemetri başlatıcı ve telemetri kanal gibi başka bir genişletilebilirlik noktası budur.

    Telemetri işlemciler işleme zinciri oluşturmak dikkat edin. Telemetri işlemcisi örneği olduğunda, sonraki işlemciye zincirindeki bir bağlantı geçir. Telemetri veri noktası için işlem yöntemi geçirildiğinde, çalışır ve sonraki Telemetri işlemci zincirinde çağırır.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify the item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. Bu Applicationınsights.Config'de ekleyin:

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Burada örnekleme filtre başlatma aynı bölüm budur.)

Sınıfınızda ortak adlandırılmış özellikleri sağlayarak .config dosyasından dize değerlerini geçirebilirsiniz.

> [!WARNING]
> Tür adı ve kodda sınıf ve özellik adları .config dosyasına hiçbir özellik adlarını eşleştirmek için dikkatli olun. .Config dosyası mevcut olmayan türe veya özelliğe başvuruyorsa, SDK'yı sessizce tüm telemetri göndermeyi başarısız olabilir.
>
>

**Alternatif olarak,** kod filtrede başlatabilirsiniz. Uygun başlatma sınıfında - örneğin AppStart Global.asax.cs içinde - zincirine işlemci ekleyin:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

Bu noktadan sonra oluşturulan TelemetryClients, işlemci kullanır.

### <a name="example-filters"></a>Örnek filtreleri
#### <a name="synthetic-requests"></a>Yapay istekleri
Bot ve web testi filtreleyin. Ölçüm Gezgini yapay kaynaklarını filtre seçeneğini sağlamakla birlikte, bu seçenek SDK filtreleyerek trafiğini azaltır.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Başarısız kimlik doğrulaması
"401" yanıt istekleri filtreler.

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Hızlı uzak bağımlılık çağrıları filtre
Yalnızca yavaş çağrılar tanılamak istiyorsanız, hızlı olanları filtreleyin.

> [!NOTE]
> Bu portalda gördüğünüz istatistikleri eğme. Bağımlılık çağrıları tüm hataları varsa gibi bağımlılık grafik arar.
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Bağımlılık sorunlarını tanılama
[Bu blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) bağımlılıkları için otomatik olarak normal ping göndererek bağımlılık sorunları tanılamak için bir proje açıklar.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Özellikleri ekleyin: ITelemetryInitializer
Tüm telemetri ile gönderilen genel özelliklerini tanımlamak için telemetri başlatıcıları kullanın; ve standart telemetri modülleri seçili davranışını geçersiz kılmak için.

Örneğin, Web Paketi için Application Insights telemetri HTTP istekleriyle ilgili toplar. Varsayılan olarak, bu yanıt kodu ile herhangi bir istek başarısız olarak işaretler > = 400. Ancak başarılı 400 Muamele etmek istiyorsanız, başarı özelliğini ayarlayan bir telemetri Başlatıcı sağlayabilir.

Bir telemetri Başlatıcı sağladığınız adlı Track*() yöntemlerden herhangi birini zaman çağrılır. Bu, standart telemetri modülleri tarafından adlı yöntemler içerir. Kurala göre bu modülleri başlatıcısı tarafından zaten ayarlanmış herhangi bir özelliği ayarlı değil.

**Başlatıcı tanımlayın**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Başlatıcı yükleme**

Applicationınsights.Config'de:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Alternatif olarak,* kodda, örneğin Global.aspx.cs Başlatıcı örneği:

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Bu örnek daha fazla bilgi bakın.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetri başlatıcıları
*JavaScript*

Portaldan aldığınız başlatma koddan hemen sonra bir telemetri başlatıcı ekleyin:

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

TelemetryItem üzerinde kullanılabilir özel olmayan özelliklerin özeti için bkz: [uygulama Öngörüler dışarı veri modeli](app-insights-export-data-model.md).

İstediğiniz sayıda başlatıcıları ekleyebilirsiniz.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor ve ITelemetryInitializer
Telemetri işlemciler ve telemetri başlatıcıları arasındaki fark nedir?

* Bazı üst üste geliyor bunlarla yapabileceklerinizi içinde vardır: her ikisi de telemetri özellikleri eklemek için kullanılabilir.
* TelemetryInitializers TelemetryProcessors önce her zaman çalışır.
* TelemetryProcessors tamamen değiştirin veya bir telemetri öğesi atmak olanak sağlar.
* TelemetryProcessors performans sayacı telemetri işlem yok.


## <a name="reference-docs"></a>Başvuru belgeleri
* [API’ye Genel Bakış](app-insights-api-custom-events-metrics.md)
* [ASP.NET başvurusu](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK kod
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK'SI](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Sonraki adımlar
* [Arama olayları ve günlükleri](app-insights-diagnostic-search.md)
* [Örnekleme](app-insights-sampling.md)
* [Sorun giderme](app-insights-troubleshoot-faq.md)
