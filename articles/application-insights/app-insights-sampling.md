---
title: Azure Application ınsights telemetri örnekleme | Microsoft Docs
description: Nasıl ses telemetri denetim altında tutun.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.reviewer: vitalyg
ms.author: mbullwin
ms.openlocfilehash: 7fca6ffa9efa3eed9f7c74ee89ad8bb9651494bb
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044714"
---
# <a name="sampling-in-application-insights"></a>Application Insights’ta örnekleme


Örnekleme bir özelliktir [Azure Application Insights](app-insights-overview.md). Bir uygulama verilerinin istatistiksel olarak doğru olan analizini korurken telemetri trafiğini ve depolama alanını azaltmak için önerilen yoldur. Tanılama araştırmalar yaparken öğeleri arasında gidebilmeniz filtre ilgili olan öğeleri seçer.
Ölçüm sayıları için portalda sunulduğunda istatistikleri herhangi bir etkisi en aza indirmek için örnekleme, hesaba renormalized.

Örnekleme, trafik ve veri maliyetleri azalttı ve azaltma önlemenize yardımcı olur.

## <a name="in-brief"></a>Kısaca:
* Örnekleme, 1'de korur *n* kaydeder ve rest atar. Örneğin, 1, 5 olayları, % 20 örnekleme oranını korumak. 
* Uygulamanız çok miktarda telemetri, ASP.NET web sunucusu uygulamalarda gönderirse örnekleme otomatik olarak gerçekleşir.
* Örnekleme el ile de ayarlayabilirsiniz Bu, ya da portal kullanımı ve Tahmini maliyetler sayfasında; veya .config dosyasında; ASP.NET SDK'sı ya da ağ trafiğini azaltmak için Java SDK Applicationınsights.xml dosyasındaki.
* Özel olay günlükleri kaydetmek ve bir etkinlik kümesi tutulur veya birlikte iptal olduğunu emin olmak istiyorsanız aynı Operationıd değere sahip olduğunuzdan emin olun.
* Örnekleme bölen *n* özelliğinde her bir kayıttaki bildirilen `itemCount`, arama göründüğü kolay ad "istek sayısı" veya "olayı sayısı" altında. Örnekleme, işlem olmadığında `itemCount==1`.
* Analytics sorguları yazma durumunda [örnekleme, hesaba](../log-analytics/query-language/aggregations.md). Özellikle, kayıtları yalnızca Sayım yerine kullanmalısınız `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Örnekleme türleri
Üç alternatif örnekleme yöntemi vardır:

* **Uyarlamalı örnekleme** telemetri SDK'sını ASP.NET uygulamanıza gönderilen hacmi otomatik olarak ayarlar. SDK'sı v 2.0.0-beta3 ile başlayarak varsayılan örnekleme yöntem budur. Uyarlamalı örnekleme şu anda yalnızca ASP.NET sunucu tarafı telemetri için kullanılabilir. Framework'ün tamamını hedefleyen Asp.NET Core uygulamaları için Uyarlamalı örnekleme Microsoft.ApplicationInsights.AspNetCore SDK 1.0.0 sürümüne kullanılabilir. NetCore hedefleyen Asp.NET Core uygulamaları için Uyarlamalı örnekleme Microsoft.ApplicationInsights.AspNetCore SDK'sının 2.2.0-beta1 kullanılabilir.

* **Sabit fiyat örnekleme** sunucunuzdan hem ASP.NET veya Java ve kullanıcılarınızın tarayıcılarından gönderilen telemetri hacmini azaltır. Hızı ayarladığınızda. İstemci ve sunucu kendi örnekleme eşitleyecek şekilde söz konusu, arama istekleri ve ilgili sayfa görünümleri arasında gezinebilirsiniz.
* **Alım örneklemesi** Azure portalında çalışır. Bazı, ayarladığınız bir örnekleme hızı anda uygulamanızdan gelen telemetriyi atar. Uygulamanızdan gönderilen telemetri trafiğini azaltmak değil, ancak aylık kota içinde tutmanıza yardımcı olur. Alım örneklemesi ana avantajı, uygulamanızı yeniden dağıtmaya gerek kalmadan örnekleme hızını ayarlayabilirsiniz ve tüm sunucular ve istemciler için aynı şekilde çalışır ' dir. 

Bağdaşık veya sabit oranı örnekleme işlemi varsa, alım örneklemesi devre dışı bırakıldı.

## <a name="ingestion-sampling"></a>Alım örneklemesi
Bu tür bir örnekleme burada Application Insights hizmet uç noktası, web sunucusu, tarayıcılar ve cihazlardan telemetri ulaştığında bir noktada çalışır. Uygulamanızdan gönderilen telemetri trafiğini azaltmaz ancak miktarını azaltmak işlenen ve korunur (ve IP adresleri için ücret) Application Insights tarafından.

Bu tür bir örnekleme, uygulamanızı genellikle, aylık kota geçer ve örnekleme SDK tabanlı tür kullanma seçeneğiniz yoksa kullanın. 

Örnekleme hızını, kullanım ve Tahmini maliyetler sayfasını ayarlayın:

![Uygulama genel bakış dikey penceresinden tıklama ayarları, kota, örnekler, ardından bir örnekleme hızı seçin ve Güncelleştir'e tıklayın.](./media/app-insights-sampling/04.png)

Örnekleme diğer türleri gibi ilgili telemetri öğelerinin algoritma korur. Örneğin, arama telemetriyi incelerken, belirli bir özel durumla ilişkili istek bulmak mümkün olacaktır. İstek oranı gibi ölçüm sayar ve özel durum oranı doğru bir şekilde korunur.

Örnekleme tarafından atılan veri noktaları kullanılabilir değil herhangi bir Application Insights özellik gibi [sürekli dışarı aktarma](app-insights-export-telemetry.md).

SDK tabanlı uyarlamalı veya sabit oranı örnekleme çalışırken alım örneklemesi çalışmaz. Visual Studio'da ASP.NET SDK etkin olduğunda varsayılan veya Durum İzleyicisi'ni kullanarak Uyarlamalı örnekleme etkin olduğunu unutmayın ve alım örneklemesi devre dışı bırakılır. SDK'sı örnekleme Oranı % 100'den az ise, ayarladığınız alımı örnekleme oranını göz ardı edilir.

> [!WARNING]
> Kutucukta gösterilen değeri, alım örneklemesi için ayarladığınız değeri gösterir. SDK'sı örnekleme işlemi ise gerçek örnekleme oranını temsil etmez.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Web sunucunuzdaki Uyarlamalı örnekleme
Uyarlamalı örnekleme, Application Insights SDK ASP.NET v 2.0.0-beta3 ve üzeri için kullanılabilir ve varsayılan olarak etkindir. 

Uyarlamalı örnekleme, Application Insights hizmet uç noktası için web sunucusu uygulamanızdan gönderilen telemetri hacmini etkiler. Birim, en yüksek trafik hızı belirtilen içinde tutmak için otomatik olarak ayarlanır.

Bu telemetrinin düşük hacimlerinde uygulama hata ayıklama kadar çalışmaz veya düşük kullanım ile Web sitesi etkilenmez.

Hedef birim elde etmek için bazı oluşturulan telemetri atılır. Ancak, örnekleme diğer türleri gibi ilgili telemetri öğelerinin algoritma korur. Örneğin, arama telemetriyi incelerken, belirli bir özel durumla ilişkili istek bulmak mümkün olacaktır. 

İstek oranı gibi ölçüm sayar ve özel durum oranı ayarlanmış için örnekleme hızını, dengelemek için yaklaşık olarak doğru değerleri ölçüm Gezgini'nde göster.

### <a name="update-nuget-packages"></a>NuGet paketlerini güncelleştirme ###

Projenizin NuGet paketlerini en son sürüme güncelleştirin *yayın öncesi* Application Insights'ın sürümü. Visual Studio'da Çözüm Gezgini'nde projeye sağ tıklayın, NuGet paketlerini Yönet'i seçin denetleyin **ön sürümü dahil et** Microsoft.applicationınsights.Web arayın. 

### <a name="configuring-adaptive-sampling"></a>Uyarlamalı örnekleme yapılandırma ###

İçinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), çeşitli parametreleri ayarlayabileceğiniz `AdaptiveSamplingTelemetryProcessor` düğümü. Gösterilen rakamları, varsayılan değerler şunlardır:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    İçin uyumlu algoritma amaçlar hedef hızı **her server konağında**. Web uygulamanız çok ana bilgisayarda çalışıyorsa, hedef oranınız trafik sırasında Application Insights portalında içinde kalması için bu değeri azaltın.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Telemetri geçerli hızı tekrar değerlendirilir olduğu aralığı. Değerlendirme hareketli ortalama gerçekleştirilir. Telemetrinizi ani artışları için Mükellefi ise bu aralığı kısaltmak isteyebilirsiniz.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Örnekleme yüzdesi değeri değişiklikler olduğunda, ne kadar süre sonra sonra biz yeniden daha az veri yakalamak için örnekleme yüzdesini azaltmak için izin verilir.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Örnekleme yüzdesi değeri değiştiğinde kullandığınızda, nasıl kısa bir süre sonra size tekrar daha fazla veri yakalamak için örnekleme yüzdesi artırmak için izin verilir.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Örnekleme yüzdesi değiştikçe, biz ayarlamaya izin en düşük değer nedir?
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Örnekleme yüzdesi değiştikçe, biz ayarlamaya izin en büyük değer nedir?
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    Hareketli ortalamanın hesaplanmasında ağırlığı en son değer atanmış. Eşit veya 1'den küçük bir değer kullanın. Küçük değerler algoritması daha az reaktif bir şekilde ani değişiklikleri yapın.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Uygulamayı yalnızca başladığında atanan değer. Hata ayıklarken azaltmak yok. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Örneklenen istemediğiniz türleri noktalı virgülle ayrılmış liste. Türler tanınan: bağımlılık, olay, özel durum, sayfa görüntülemesi, istek, izleme. Belirtilen tür tüm örneklerine iletilir; örneklenen belirtilmeyen türleri.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Örneklenen istediğiniz türlerini noktalı virgülle ayrılmış liste. Türler tanınan: bağımlılık, olay, özel durum, sayfa görüntülemesi, istek, izleme. Belirtilen tür örneklenen; Tüm diğer örnek türleri her zaman aktarılmaz.


**Geçmek** Uyarlamalı örnekleme, AdaptiveSamplingTelemetryProcessor düğümünde applicationınsights yapılandırma dosyasından kaldırın.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatif: Uyarlamalı örnekleme kodda yapılandırın.
Örnekleme parametresi .config dosyasında ayarlamak yerine, bu değerleri programlı olarak ayarlayabilirsiniz. Bu, örnekleme hızını tekrar değerlendirilir olduğunda çağrılan bir geri çağırma işlevini belirtmenize olanak sağlar. Bu, örneğin, hangi örnekleme hızı kullanıldığını bulmak için kullanabilirsiniz.

Kaldırma `AdaptiveSamplingTelemetryProcessor` .config dosyasını düğümünden.

*C#*

```csharp

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust the settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report the sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Telemetri işlemcileri hakkında bilgi edinin](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>JavaScript ile web sayfaları için örnekleme
Web sayfaları için herhangi bir sunucudan sabit oranı örnekleme yapılandırabilirsiniz. 

Olduğunda, [web sayfaları için Application Insights yapılandırma](app-insights-javascript.md), Application Insights portalından aldığınız JavaScript kod parçacığını değiştirmek. (ASP.NET uygulamalarında kod parçacığını genellikle _Layout.cshtml içinde sağlanabilir.)  Gibi bir satır Ekle `samplingPercentage: 10,` izleme anahtarını önce:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Örnekleme yüzdesi, yakın 100/N N bir tamsayı olduğu bir yüzdesini seçin.  Şu anda örnekleme diğer değerleri desteklemiyor.

Sabit fiyat örnekleme sunucuda da etkinleştirirseniz, söz konusu, arama istekleri ve ilgili sayfa görünümleri arasında gezinebilirsiniz şekilde istemciler ve sunucu eşitler.

## <a name="fixed-rate-sampling-for-aspnet-and-java-web-sites"></a>ASP.NET ve Java web siteleri için sabit oranı örnekleme
Sabit fiyat örnekleme, web sunucusu ve web tarayıcıları gönderilen trafik azaltır. Uyarlamalı örnekleme aksine, sizin tarafınızdan karar sabit bir hızda telemetri azaltır. Bu ayrıca istemci ve sunucu örnekleme eşitler ve böylece ilgili öğeleri korunur - Örneğin, bir sayfa görünümü'nde arama baktığınızda, ilgili istek bulabilirsiniz.

Örnekleme algoritması, ilgili öğeleri korunur. Her HTTP isteği için olay, istek ve ilgili olayları ya da iptal veya birlikte iletilen. 

Yaklaşık olarak doğru olduklarından emin ölçüm Gezgini'nde için örnekleme hızını, dengelemek için bir faktör oranları gibi istek ve özel durum sayısı çarpılır.

### <a name="configuring-fixed-rate-sampling-in-aspnet"></a>ASP.NET'te sabit oranı örnekleme yapılandırma ###

1. **Projenizin NuGet paketlerini güncelleştirme** en son *yayın öncesi* Application Insights'ın sürümü. Visual Studio'da Çözüm Gezgini'nde projeye sağ tıklayın, NuGet paketlerini Yönet'i seçin denetleyin **ön sürümü dahil et** Microsoft.applicationınsights.Web arayın. 
2. **Uyarlamalı örnekleme devre dışı**: içinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md)kaldırın veya açıklama `AdaptiveSamplingTelemetryProcessor` düğümü.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

3. **Sabit fiyat örnekleme modülü etkinleştirin.** Bu kod parçacığını [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

### <a name="configuring-fixed-rate-sampling-in-java"></a>JAVA'da sabit oranı örnekleme yapılandırma ###

1. İndirme ve web uygulamanızı en son yapılandırma [application ınsights java SDK'sı](app-insights-java-get-started.md)

2. **Sabit fiyat örnekleme modülündeki** Applicationınsights.xml dosyasını aşağıdaki kod parçacığını ekleyerek.

```XML
    <TelemetryProcessors>
        <BuiltInProcessors>
            <Processor type = "FixedRateSamplingTelemetryProcessor">
                <!-- Set a percentage close to 100/N where N is an integer. -->
                <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
                <Add name = "SamplingPercentage" value = "50" />
            </Processor>
        </BuiltInProcessors>
    <TelemetryProcessors/>
```

3. Ekleyebileceğiniz veya işlemci etiketi "FixedRateSamplingTelemetryProcessor" içine aşağıdaki etiketleri kullanarak örnekleme gelen telemetri belirli türlerini hariç tut
```XML
    <ExcludedTypes>
        <ExcludedType>Request</ExcludedType>
    </ExcludedTypes>

    <IncludedTypes>
        <IncludedType>Exception</IncludedType>
    </IncludedTypes>
```
Dahil veya gelen örnekleme dışlanan telemetri türleri şunlardır: bağımlılık, olay, özel durum, sayfa görüntülemesi, istek ve izleme.

> [!NOTE]
> Örnekleme yüzdesi, yakın 100/N N bir tamsayı olduğu bir yüzdesini seçin.  Şu anda örnekleme diğer değerleri desteklemiyor.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternatif: sunucu kodunuzda sabit oranı örnekleme etkinleştir
Örnekleme parametresi .config dosyasında ayarlamak yerine, bu değerleri programlı olarak ayarlayabilirsiniz. 

*C#*

```csharp

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Telemetri işlemcileri hakkında bilgi edinin](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-to-use-sampling"></a>Ne zaman örnekleme kullanılır?
ASP.NET SDK sürüm 2.0.0-beta3 kullanıyorsanız, Uyarlamalı örnekleme etkin otomatik olarak veya üzeri. Hangi kullandığınız SDK sürüm ne olursa olsun alım örneklemesi Application Insights'ı toplanan verileri örneklemek izin verecek şekilde etkinleştirebilirsiniz.

Varsayılan olarak hiçbir örneklemeye Java SDK'yı etkindir. Şu anda yalnızca sabit oranı örnekleme destekler. Java SDK'sı Uyarlamalı örnekleme desteklenmiyor.

Genel olarak, birçok küçük ve orta ölçekli uygulamalar için örnekleme gerekmez. En doğru istatistikleri ve en kullanışlı tanılama bilgileri, tüm kullanıcı etkinliklerini veri toplayarak elde edilir. 

Örnekleme başlıca avantajları şunlardır:

* Uygulamanızın telemetri oranı çok yüksek kısa gönderdiğinde uygulama öngörüleri hizmet düşme ("kısıtlamalar") veri noktaları aralığı süresi. 
* İçinde tutmak [kota](app-insights-pricing.md) veri noktaları için fiyatlandırma katmanınızı. 
* Telemetri koleksiyonundan ağ trafiğini azaltmak için. 

### <a name="which-type-of-sampling-should-i-use"></a>Örnekleme türünü kullanmalıyım?
**Alma, örnekleme kullanın:**

* Genellikle, telemetri aylık kota de gidin.
* Örnekleme - Örneğin ASP.NET sürüm 2'den önceki desteklemiyor SDK'sı bir sürümünü kullanıyorsunuz.
* Çok sayıda telemetri kullanıcılarınızın web tarayıcılarından karşılaşacağınız.

**Sabit fiyat, örnekleme kullanın:**

* ASP.NET web Hizmetleri sürüm 2.0.0 için Application Insights SDK'sını kullanarak veya üzeri ya da Java SDK'sı v2.0.1 veya sonraki bir sürümü ve
* Olayları zaman araştırdığınızı böylece istemci ve sunucu arasında eşitlenen örnekleme istediğiniz [arama](app-insights-diagnostic-search.md), istemci ve sunucu, sayfa görüntülemeleri ve http istekleri gibi ilgili olaylar arasında gezinebilirsiniz.
* Uygulamanız için uygun örnekleme yüzdesini emin olursunuz. Doğru ölçümleri almak için yüksek olmalıdır, ancak fiyatlandırma kota ve azaltma sınırları aşıyor oranı. 

**Uyarlamalı örnekleme kullanın:**

Bir örnekleme biçimlerinin koşulları uygulamazsanız, Uyarlamalı örnekleme öneririz. Bu varsayılan olarak ASP.NET sunucusu SDK'sı sürüm 2.0.0-beta3 etkin veya üzeri. Belirli bir minimum fiyatı ulaşılana kadar trafik azaltmaz, bu nedenle, düşük kullanımlı siteleri etkilenmez.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Örnekleme işlemi olup olmadığını nasıl anlarım?
Uygulanmış nerede olursa olsun gerçek örnekleme hızını bulmak için kullanmak bir [Analytics sorgusu](app-insights-analytics.md) bunun gibi:

```
union * 
| where timestamp > ago(1d)
| summarize 100/avg(itemCount) by bin(timestamp, 1h), itemType
| render timechart 
```

Her kaydı tutulur `itemCount` temsil ettiği, orijinal kayıt sayısını gösteren 1 + önceki atılan kayıt sayısı eşittir. 

## <a name="how-does-sampling-work"></a>Örnekleme nasıl çalışır?
ASP.NET sürümlerinden 2.0.0 SDK'yı ve Java SDK'sı sürüm 2.0.1 sabit oranı örnekleme özelliği ve ve sonraki sürümler. Uyarlamalı örnekleme bir SDK 2.0.0 ve sonraki sürümlerde ASP.NET sürümlerinden özelliğidir. Alım örneklemesi Application Insights hizmetine özelliğidir ve SDK'sı örnekleme şu anda gerçekleştirmiyorsa işleminde olabilir. 

Örnekleme algoritması bırakmak hangi telemetri öğelerinin ve hangilerini (Bu SDK'sı veya Application Insights hizmeti olup olmadığı) tutun karar verir. Örnekleme karar alma hedeflenir tüm birbiriyle veri noktaları olduğu gibi Application ınsights'da, eyleme dönüştürülebilir ve sınırlı bir veri kümesi ile bile güvenilir bir tanılama deneyimi sürdürürken bir yandan korumak için çeşitli kurallar temel alır. Ek telemetri öğelerini (örneğin, özel durum ve bu istekten günlüğe izlemeleri) uygulamanız için başarısız bir istek gönderir, örneğin, örnekleme bu istek ve diğer telemetriyi bölecek değil. Tutar ya da tümünü bir araya bırakır. Sonuç olarak, Application ınsights'ta istek ayrıntılarını baktığınızda, her zaman istekle birlikte kendi ilişkili telemetri öğelerini görebilirsiniz. 

Örnekleme kararı, belirli bir işlemine ait tüm telemetri öğeleri korunur veya bırakılan, yani istek üzerinde işlem kimliği temel alır. İşlem olmayan telemetri öğeleri için kimliği kümesi (için örnek telemetri öğeleri zaman uyumsuz iş parçacığından hiç http bağlamı ile bildirilen) örnekleme yalnızca her türden telemetri öğelerinin yüzdesi yakalar. .NET SDK'sının 2.5.0-Beta2 ve ASP.NET Core SDK'sının 2.2.0-beta3 önce örnekleme karar tanımlayan "kullanıcı" uygulamalar için kullanıcı kimliği karması temel (diğer bir deyişle, en sık karşılaşılan web uygulamaları). Kullanıcılar (örneğin, web Hizmetleri) tanımlamadığınız uygulama türleri için örnekleme karar istek üzerinde işlem kimliği temel alınmıştır.

Application Insights hizmetine ölçümleri telemetri size geri sunmak, koleksiyonun zaman dengelemek için veri noktaları için kullanılan aynı örnekleme yüzdesi olarak ayarlar. Bu nedenle, Application ınsights telemetri bakarak, kullanıcıların çok yakın gerçek sayılar olan istatistiksel olarak doğru anların görüyorsunuz.

Yaklaşık doğruluğu büyük ölçüde yapılandırılmış örnekleme yüzdesi bağlıdır. Ayrıca, büyük hacimli kullanıcıları çok sayıda genellikle benzer istekler işlemek uygulamalar için doğruluğu artırır. Öte yandan, bu uygulamalar genellikle tüm telemetri kotanın, azaltma gelen veri kaybına neden olmadan kalsanız gönderebilir gibi önemli bir yük ile çalışmayan uygulamalar için örnekleme gerekmiyor. 

> [!WARNING]
> Application Insights telemetri türlerini ölçümleri ve oturumları örnek değil. Duyarlık azalma bu telemetri türleri için yüksek oranda istenmeyen olabilir.
> 

### <a name="adaptive-sampling"></a>Uyarlamalı örnekleme
Uyarlamalı örnekleme geçerli SDK'dan aktarım hızı izler ve hedef maksimum oran içinde kalmaya için örnekleme yüzdesini ayarlayan bir bileşen ekler. Ayarlama, düzenli aralıklarla yeniden hesaplanır ve giden aktarım hızını üzerinde bir hareketli ortalama temel alır.

## <a name="sampling-and-the-javascript-sdk"></a>Örnekleme ve JavaScript SDK'sı
Sabit fiyat örnekleme sunucu tarafı SDK'sı ile birlikte, istemci-tarafı (JavaScript) SDK'sı katılır. İzleme eklenmiş sayfaları için sunucu tarafı, "SAMPLE" içinde kararı kullanıcılardan yalnızca istemci tarafı telemetri gönderir Bu mantık, istemci - ve sunucu-tarafı arasında kullanıcı oturumunu bütünlüğünü korumak için tasarlanmıştır. Sonuç olarak, tüm Application ınsights özel telemetri öğesinden bu kullanıcı veya oturum için diğer tüm telemetri öğeleri bulabilirsiniz. 

*Yukarıda açıklanan şekilde my istemci ve sunucu tarafı telemetri Eşgüdümlü örnekleri gösterme.*

* Sabit fiyat örnekleme hem istemci ve sunucu üzerinde etkin olduğunu doğrulayın.
* SDK sürümü 2.0 olduğundan emin olun veya üzeri.
* İstemci ve sunucu aynı örnekleme yüzdesini ayarlayın denetleyin.

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular
*Bir basit "toplama her telemetri türü yüzdesi X" örnekleme olmayan neden?*

* Çok yüksek bir ölçüm anların kesinlik ile bu örnekleme yaklaşımı sağlayacak, ancak kullanıcı, oturum ve tanılama için kritik olan istek başına tanılama verilerin bağıntısını olanağı sonu. Bu nedenle, çalıştığını daha iyi "tüm telemetri öğeleri için yüzde X uygulama kullanıcılarının Topla" veya "uygulama isteklerin yüzdesi X için tüm telemetri toplamak" örnekleme mantığı. Sıfırlamaya olmaktır istekler (örneğin, arka plan zaman uyumsuz işleme) ile ilişkili olmayan telemetri öğeleri için "tüm öğeler için her telemetri türü yüzdesi X toplayın." 

*Örnekleme yüzdesi, zaman içinde değiştirebilir miyim?*

* Evet, Uyarlamalı örnekleme telemetri, şu anda gözlemlenen hacmine dayalı örnekleme yüzdesini aşamalı olarak değiştirir.

*Sabit fiyat örnekleme kullanırsam nasıl bilebilirim hangi örnekleme yüzdesi Uygulamam için en iyi şekilde çalışır?*

* Bir yolu ile başlaması Uyarlamalı örnekleme, Bul ne kapatır (yukarıdaki soruda bakın üzerinde) derecelendirmek ve ardından sabit hızında geçiş hızı kullanarak örnekleme. 
  
    Aksi takdirde, düşünmeniz gerekmez. Geçerli telemetri kullanımınızı Application ınsights'da analiz oluştuğunu her azaltma inceleyin ve toplanan telemetri hacmini tahmin. Seçtiğiniz fiyatlandırma katmanının, birlikte üç bu girişlerin ne kadar toplanan telemetri hacmini azaltmak isteyebilirsiniz önerin. Bununla birlikte, kullanıcılarınızın sayısı arasında bir artış ya da diğer bazı shift telemetrinin toplu tahmininize geçersiz.

*Örnekleme yüzdesi çok düşük yapılandırabilirim ne olur?*

* Aşırı Düşük örnekleme yüzdesi (sayısı içeren aşırı agresif örnekleme), Application Insights veri birimi azaltma için veri görselleştirme dengelemek istediğinde anların doğruluğunu azaltır. Bazı sık başarısız olan veya yavaş istekler kullanıma tümcelerindeki olarak da, tanılama deneyimi olumsuz, etkilenebilir.

*Örnekleme yüzdesi çok yüksek yapılandırabilirim ne olur?*

* Çok yüksek bir örnekleme yüzdesi (değil agresif yeterince) yapılandırma toplanan telemetri hacmini yetersiz bir düşüş olur. Application Insights kullanarak maliyeti, fazla kullanım ücretlendirmesi nedeniyle planlı daha yüksek olabilir ve azaltma için ilgili telemetri veri kaybı hala karşılaşabilirsiniz.

*Hangi platformlarda örnekleme kullanabilir miyim?*

* SDK'sı örnekleme şu anda gerçekleştirmiyorsa alım örneklemesi için belirli bir hacim yukarıda herhangi bir telemetri otomatik olarak ortaya çıkabilir. ASP.NET SDK veya Java SDK(1.0.10 or before) önceki sürümü eski bir sürümünü kullanıyorsanız bu, örneğin, işe yarar.
* ASP.NET SDK'sı sürüm 2.0.0 kullanıyorsanız ve üzeri (Azure veya kendi sunucuda barındırılan), Uyarlamalı örnekleme varsayılan olarak sahip olursunuz, ancak sabit hızında yukarıda açıklanan şekilde geçiş yapabilirsiniz. Sabit fiyat örnekleme ile ilgili olaylar örneklemek için SDK'sı tarayıcı otomatik olarak eşitler. 
* Java SDK'sı sürüm 2.0.1 kullanıyorsanız veya Applicationınsights.XML sabit oranı örnekleme etkinleştirmek için yukarıdaki yapılandırabileceğiniz değilse. Örnekleme, varsayılan olarak kapalıdır. Sabit fiyat örnekleme ile ilgili olaylar örneklemek için SDK'sı tarayıcı otomatik olarak eşitler.

*Her zaman görmek istiyorum nadir belirli olaylar vardır. Nasıl bunları örnekleme modülü alabilir miyim?*

* TelemetryClient ile yeni bir TelemetryConfiguration (varsayılan etkin) ayrı bir örneğini başlatır. Bu, nadir olayları göndermek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* [Filtreleme](app-insights-api-filtering-sampling.md) ne SDK'nızı gönderir, katı daha fazla denetim sağlar.

