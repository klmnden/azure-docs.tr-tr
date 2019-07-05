---
title: Filtreleme ve Azure Application Insights SDK'da önişleme | Microsoft Docs
description: Telemetri işlemciler ve SDK'sı Telemetri başlatıcılar filtrelemek veya Application Insights portalına telemetri gönderilmeden önce özellikleri verileri eklemek için yazın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/23/2016
ms.author: mbullwin
ms.openlocfilehash: 062b565369c3b6e877d36f883a152ca6c013e0cf
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67479650"
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtreleme ve telemetri, Application Insights SDK'sı ön işleme


Yazma ve nasıl telemetri yakalanır ve Application Insights hizmetine gönderilmeden önce işlenen özelleştirmek Application Insights SDK için eklentileri yapılandırın.

* [Örnekleme](../../azure-monitor/app/sampling.md) istatistiklerinizi etkilemeden telemetri hacmini azaltır. Birlikte tutar ilgili veri noktaları, aralarında bir sorun tanılarken gidebilirsiniz. Portalda, örnekleme için dengelemek için toplam sayısı çarpılır.
* Telemetri işlemcilerle filtreleme [ASP.NET](#filtering) veya [Java](../../azure-monitor/app/java-filter-telemetry.md) seçin ya da sunucuya gönderilmeden önce SDK telemetri değiştirme sağlar. Örneğin, istekleri robotlar hariç tutarak telemetri hacmini azaltabilir. Ancak filtreleme topluyor trafiğini azaltmak için daha basit bir yaklaşım. Ne iletilen üzerinde daha fazla denetim sağlar, ancak tüm başarılı isteklerini filtrelerseniz bu istatistiklerinizi - Örneğin, etkiler dikkat etmelisiniz.
* [Telemetri başlatıcılar Ekle özellikleri](#add-properties) uygulamanızdan telemetri standart modüllerden dahil olmak üzere gönderilen tüm telemetri için. Örneğin, hesaplanan değerler ekleyebilirsiniz; veya sürüm numaraları, Portalı'nda verilere filtre uygulamak.
* [SDK API'si](../../azure-monitor/app/api-custom-events-metrics.md) özel olaylar ve ölçümler göndermek için kullanılır.

Başlamadan önce:

* Application Insights yükleme [ASP.NET SDK](../../azure-monitor/app/asp-net.md) veya [için Java SDK'sı](../../azure-monitor/app/java-get-started.md) uygulamanızda.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filtreleme: ITelemetryProcessor
Bu teknik, hangi dahil veya hariç ait telemetri akışına üzerinden daha doğrudan denetim olanağı verir. Örnekleme, birlikte kullanın veya ayrı ayrı.

Telemetri filtreleme için telemetri işlemci yazma ve SDK'sı ile kaydedin. Tüm telemetri işlemci girilir ve akıştan bırakın veya özellik eklemek seçebilirsiniz. Bu, HTTP isteği Toplayıcı ve bağımlılık Toplayıcı gibi standart modüllerden telemetri yanı sıra, kendi yazdığınız telemetri içerir. Örneğin, robotlar veya başarılı bağımlılık çağrıları istekleriyle ilgili telemetri filtre olabilir.

> [!WARNING]
> SDK'sından gönderilen telemetri filtreleme işlemci kullanma portalında gördüğünüz istatistikleri eğriltmek ve onu ilgili öğeleri takip etmek zor hale getirebilirsiniz.
>
> Bunun yerine, kullanmayı [örnekleme](../../azure-monitor/app/sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Bir telemetri işlemci (C#) oluşturma
1. Projenizdeki Application Insights SDK'sı sürüm 2.0.0 olduğunu doğrulayın veya üzeri. Visual Studio Çözüm Gezgini'nde projenize sağ tıklayın ve NuGet paketlerini Yönet'i seçin. NuGet Paket Yöneticisi'nde Microsoft.applicationınsights.Web denetleyin.
2. Bir filtre oluşturmak için ITelemetryProcessor uygulayın. Telemetri modülü, telemetri başlatıcısını ve telemetri kanal gibi başka bir genişletilebilirlik noktası budur.

    Telemetri işlemcileri işleme zinciri oluşturmak dikkat edin. Bir telemetri işlemcisi örneği, zincirdeki sonraki işlemci için bir bağlantı geçirin. Telemetri veri noktası işlemi yöntemine geçirildiğinde, yapar ve sonra zincirdeki sonraki Telemetri işlemci çağırır.

```csharp
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
3. Bunu Applicationınsights.config dosyasında ekleyin:

```xml
<TelemetryProcessors>
  <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
     <!-- Set public property -->
     <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
  </Add>
</TelemetryProcessors>
```

(Bir örnekleme filtre başlatmak burada aynı bölüme budur.)

Sınıfınızda genel adlandırılmış özellikleri sağlayarak, dize değerlerini .config dosyasından geçirebilirsiniz.

> [!WARNING]
> Tür adı ve kodu sınıf ve özellik adlarına .config dosyasındaki tüm özellik adlarını eşleştirmek için dikkatli olun. .Config dosyası mevcut olmayan türü veya özellik başvuruyorsa, SDK'yı sessizce hiç telemetri göndermek başarısız olabilir.
>
>

**Alternatif olarak,** kod filtrede başlatabilirsiniz. Uygun başlatma sınıfında - örneğin AppStart Global.asax.cs içinde - işlemci zincirine Ekle:

```csharp
var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
builder.Use((next) => new SuccessfulDependencyFilter(next));

// If you have more processors:
builder.Use((next) => new AnotherProcessor(next));

builder.Build();
```

Bu noktadan sonra oluşturulan TelemetryClients, işlemci kullanır.

### <a name="example-filters"></a>Örnek filtreleri
#### <a name="synthetic-requests"></a>Yapay istekler
Robotlar ve web testi filtreleyin. Ölçüm Gezgini yapay kaynaklarını filtreleme seçeneği verir ancak bu seçenek SDK filtreleyerek trafiğini azaltır.

```csharp
public void Process(ITelemetry item)
{
  if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

  // Send everything else:
  this.Next.Process(item);
}
```

#### <a name="failed-authentication"></a>Başarısız kimlik doğrulaması
"401" yanıt istekleri filtreleyin.

```csharp
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

#### <a name="filter-out-fast-remote-dependency-calls"></a>Hızlı uzak bağımlılık çağrıları Filtrele
Yavaş çağrı tanılama istiyorsanız, hızlı olanları filtreleyin.

> [!NOTE]
> Bu, portalda gördüğünüz istatistikleri eğme. Bağımlılık grafiği, bağımlılık çağrıları tüm hataları varsa gibi görünecektir.
>
>

```csharp
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
[Bu blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) bağımlılıklarını otomatik olarak normal ping göndererek bağımlılık sorunlarını tanılamak için bir proje açıklar.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Özellikleri ekleyin: ITelemetryInitializer
Tüm telemetri ile gönderilen genel özelliklerini tanımlamak için telemetri başlatıcıları kullanın; ve standart telemetri modülleri seçili davranışını geçersiz kılar.

Örneğin, Web Paketi için Application Insights HTTP istekleriyle ilgili telemetri toplar. Varsayılan olarak, bu yanıt kodu herhangi bir istekle başarısız olarak işaretler > 400 =. Ancak 400 başarılı değerlendirilecek istiyorsanız, başarı özelliğini ayarlayan bir telemetri Başlatıcısı sağlayabilir.

Bir telemetri Başlatıcısı sağlarsanız, Track*() yöntemlerden herhangi birini adlı olduğunda çağrılır. Bu, standart telemetri modülleri tarafından çağrılır yöntemleri içerir. Kural olarak, bu modülleri bir başlatıcısı tarafından zaten ayarlanmış herhangi bir özelliği ayarlı değil.

**Başlatıcı tanımlayın**

*C#*

```csharp
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

**Yük, başlatıcı**

Applicationınsights.config dosyasında:

```xml
<ApplicationInsights>
  <TelemetryInitializers>
    <!-- Fully qualified type name, assembly name: -->
    <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
    ...
  </TelemetryInitializers>
</ApplicationInsights>
```

*Alternatif olarak,* Global.aspx.cs örnek kodda Başlatıcı örneği oluşturabilir:

```csharp
protected void Application_Start()
{
    // ...
    TelemetryConfiguration.Active.TelemetryInitializers
    .Add(new MyTelemetryInitializer());
}
```


[Bu örneğe ilişkin daha fazla bilgi bkz.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="java-telemetry-initializers"></a>Java telemetri başlatıcıları

[Java SDK Belgeleri](https://docs.microsoft.com/java/api/com.microsoft.applicationinsights.extensibility.telemetryinitializer?view=azure-java-stable)

```Java
public interface TelemetryInitializer
{ /** Initializes properties of the specified object. * @param telemetry The {@link com.microsoft.applicationinsights.telemetry.Telemetry} to initialize. */

void initialize(Telemetry telemetry); }
```

Ardından özel Başlatıcı applicationınsights.XML dosyanızda kaydedin.

```xml
<Add type="mypackage.MyConfigurableContextInitializer">
    <Param name="some_config_property" value="some_value" />
</Add>
```

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetri başlatıcıları
*JavaScript*

Portaldan aldığınız başlatma koddan hemen sonra bir telemetri başlatıcısını ekleyin:

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

            // To check the telemetry items type - for example PageView:
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

TelemetryItem üzerinde mevcut olan özel dışı özelliklerin özeti için bkz: [Application Insights dışarı veri modeli](../../azure-monitor/app/export-data-model.md).

Dilediğiniz sayıda başlatıcılar ekleyebilirsiniz.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor ve ITelemetryInitializer
Telemetri işlemciler ve telemetri başlatıcılar arasındaki fark nedir?

* Bunları ile yapabileceklerinizi içinde bazı çakışıyor vardır: her iki telemetri özellikleri eklemek için kullanılabilir.
* TelemetryInitializers TelemetryProcessors önce her zaman çalışır.
* TelemetryProcessors tamamen değiştirmek veya atmak telemetri öğesinin olanak sağlar.
* Performans sayacı telemetri TelemetryProcessors işlem yok.

## <a name="troubleshooting-applicationinsightsconfig"></a>Applicationınsights.config sorunlarını giderme
* Derleme adı ve tam olarak nitelenmiş tür adını doğru olduğunu onaylayın.
* Applicationınsights.config dosyasını çıkış dizindir ve son değişiklikleri içeren onaylayın.

## <a name="reference-docs"></a>Başvuru belgeleri
* [API’ye Genel Bakış](../../azure-monitor/app/api-custom-events-metrics.md)
* [ASP.NET başvurusu](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK kodu
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK'SI](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Sonraki adımlar
* [Arama olayları ve günlükleri](../../azure-monitor/app/diagnostic-search.md)
* [Örnekleme](../../azure-monitor/app/sampling.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
