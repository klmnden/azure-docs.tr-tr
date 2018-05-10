---
title: Azure Application ınsights'ta telemetri örnekleme | Microsoft Docs
description: Telemetri denetimindeki toplu korumak nasıl.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: mbullwin; vitalyg
ms.openlocfilehash: 53753a3202362c73356e8e39bfca9d813f6387e0
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="sampling-in-application-insights"></a>Application Insights’ta örnekleme


Örnekleme bir özelliktir [Azure Application Insights](app-insights-overview.md). Uygulama verileri istatistiksel olarak doğru bir analizini korurken telemetri trafiği ve depolama azaltmak için önerilen yoldur. Böylece, tanılama araştırmalar yaparken öğeleri arasında gezinebilirsiniz filtre ilgili öğeleri seçer.
Ölçüm sayıları için portalda sunulduğunda istatistikleri herhangi bir etkisi en aza indirmek için örnekleme hesaba renormalized.

Örnekleme trafiği ve veri maliyetleri azaltır ve azaltma önlemenize yardımcı olur.

## <a name="in-brief"></a>Kısaca:
* Örnekleme korur 1'de *n* kaydeder ve rest atar. Örneğin, 1, 5 olayları, % 20 örnekleme oranını korumak. 
* Uygulamanız çok sayıda telemetri, ASP.NET web sunucusu uygulamalarında gönderirse örnekleme otomatik olarak gerçekleşir.
* El ile örnekleme de ayarlayabilirsiniz, ya da portal kullanımı ve tahmini maliyetleri sayfa; veya ASP.NET SDK .config dosyasına; ya da ağ trafiğini azaltmak için Java SDK Applicationınsights.xml dosyasında.
* Özel olayları günlüğe kaydedin ve olayların kümesini korunur veya birlikte atılan emin emin olmak istiyorsanız aynı Operationıd değere sahip olduğundan emin olun.
* Örnekleme bölen *n* özelliğinde her bir kayıttaki bildirilen `itemCount`, arama göründüğü kolay adı "istek sayısı" veya "olay sayısı" altında. Örnekleme işlemi içinde olmadığında `itemCount==1`.
* Analitik sorguları yazma durumunda [örnekleme hesaba](app-insights-analytics-tour.md#counting-sampled-data). Özellikle, yalnızca kayıtları sayım yerine, kullanmanız gereken `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Örnekleme türleri
Üç alternatif örnekleme yöntemi vardır:

* **Uyarlamalı örnekleme** telemetri ASP.NET uygulamanızı SDK'da gönderilen hacmi otomatik olarak ayarlar. SDK v 2.0.0-beta3 ile başlayarak varsayılan örnekleme yöntem budur. Uyarlamalı örnekleme şu anda yalnızca ASP.NET sunucu tarafı telemetri için kullanılabilir. Tam Framework hedefleme Asp.NET Core uygulamaları için Uyarlamalı örnekleme Microsoft.ApplicationInsights.AspNetCore SDK 1.0.0 sürümünde kullanılabilir. NetCore hedefleme Asp.NET Core uygulamaları için Uyarlamalı örnekleme Microsoft.ApplicationInsights.AspNetCore SDK 2.2.0-beta1 kullanılabilir.

* **Sabit oran örnekleme** sunucunuzdan hem ASP.NET veya Java ve kullanıcılarınızın tarayıcılarından gönderilen telemetri hacmini azaltır. Hızı ayarlayın. İstemci ve sunucu kendi örnekleme eşitler böylece söz konusu, arama ilgili sayfa görünümleri ve istekler arasında gezinebilirsiniz.
* **Alım örnekleme** Azure portalında çalışır. Ayarladığınız örnekleme hızında uygulamanızdan ulaşan telemetri bazıları atar. Uygulamanızdan gönderdiği telemetri akışı azaltmaz ancak içinde aylık kota tutmanıza yardımcı olur. Alım örnekleme ana avantajı, uygulamanızın yeniden dağıtmadan örnekleme hızını ayarlayabilirsiniz ve tüm sunucular ve istemciler için aynı şekilde çalışır ' dir. 

Bağdaşık veya sabit oranı örnekleme işlemi yoksa alım örnekleme devre dışı bırakılır.

## <a name="ingestion-sampling"></a>Alım örnekleme
Bu biçimi örnekleme, burada web sunucusu, tarayıcılar ve cihazlar telemetrisinden Application Insights Hizmeti uç noktası ulaştığında bir noktada çalışır. Uygulamanızdan gönderdiği telemetri akışı azaltmaz ancak miktarını azaltmak işlenen ve korunur (ve için sizden ücret) Application Insights tarafından.

Bu tür örnekleme uygulamanızı genellikle aylık kotasına gider ve örnekleme SDK tabanlı tür kullanma seçeneğiniz yoksa kullanın. 

Örnekleme hızı kullanım ve tahmini maliyetleri sayfa ayarlayın:

![Uygulama genel bakış dikey penceresinden ayarları, kota, örnekleri, tıklatın ardından örnekleme hızını seçin ve Güncelleştir'i tıklatın.](./media/app-insights-sampling/04.png)

Örnekleme diğer türleri gibi ilgili telemetri öğeleri algoritma korur. Örneğin, arama telemetri incelerken, belirli bir özel durumla ilişkili istek bulmak mümkün olur. Ölçüm isteği hızı gibi sayar ve özel durum oranı doğru korunur.

Örnekleme tarafından atılan veri noktaları kullanılamıyor herhangi bir Application Insights özellik gibi [sürekli verme](app-insights-export-telemetry.md).

SDK tabanlı uyarlamalı veya sabit oranı örnekleme çalışıyorken alım örnekleme çalışmaz. ASP.NET SDK, Visual Studio veya Durum İzleyicisi'ni kullanarak etkin olduğunda ve alım örnekleme devre dışı Uyarlamalı örnekleme varsayılan olarak etkin olduğunu unutmayın. SDK örnekleme hızı % 100'den az ise, ayarladığınız alım örnekleme oranını göz ardı edilir.

> [!WARNING]
> Kutucuğu gösterilen değer alım örnekleme için belirlediğiniz değeri gösterir. SDK örnekleme işlemiyse gerçek örnekleme oranını temsil etmez.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Web sunucunuzdaki Uyarlamalı örnekleme
Uyarlamalı örnekleme Application Insights SDK ASP.NET v 2.0.0-beta3 ve sonraki sürümleri için kullanılabilir ve varsayılan olarak etkindir. 

Uyarlamalı örnekleme telemetri web sunucusu uygulamanızdan Application Insights Hizmeti uç noktası için gönderilen hacmi etkiler. Birim, en yüksek trafik hızı belirtilen içinde tutmak için otomatik olarak düzeltilir.

Bu telemetrinin, düşük birimlerinin hata ayıklama, bir uygulama şekilde çalışmaz veya bir Web sitesi düşük kullanımı ile etkilenmeyecek.

Hedef birimin elde etmek için oluşturulan telemetri bazıları atılır. Ancak örnekleme diğer türleri gibi ilgili telemetri öğeleri algoritma korur. Örneğin, arama telemetri incelerken, belirli bir özel durumla ilişkili istek bulmak mümkün olur. 

Ölçüm isteği hızı gibi sayar ve özel durum oranı ayarlanmış için örnekleme hızını dengelemek için böylece bunlar ölçüm Gezgininde yaklaşık doğru değerler göster.

### <a name="update-nuget-packages"></a>NuGet paketlerini güncelleştirme ###

En son projenizin NuGet paketlerini güncelleştirmeyi *yayın öncesi* Application Insights sürümü. Visual Studio'da Çözüm Gezgini'nde projeye sağ tıklayın, NuGet paketlerini Yönet seçin denetleyin **dahil et** ve Microsoft.applicationınsights.Web arayın. 

### <a name="configuring-adaptive-sampling"></a>Uyarlamalı örnekleme yapılandırma ###

İçinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), çeşitli parametreleri ayarlayabileceğiniz `AdaptiveSamplingTelemetryProcessor` düğümü. Aşağıdaki şekilde gösterilen varsayılan değerler şunlardır:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    Uyarlamalı algoritması için amaçlar hedef hızı **her sunucu ana bilgisayarda**. Web uygulamanızı pek çok ana bilgisayarda çalışıyorsa, bu değeri, Application Insights portalındaki trafiğinin hedefi oranını içinde kalır amacıyla azaltın.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Telemetri geçerli hızı tekrar değerlendirilir olduğu aralığı. Değerlendirme hareketli ortalama gerçekleştirilir. Telemetrinizi ani WINS'e zararlardan ise bu aralığı kısaltmak isteyebilirsiniz.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Yüzde değeri değişiklikleri örnekleme, ne zaman sonra biz yeniden daha az veri yakalamak için örnekleme yüzdesi düşürmek için izin verilir.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Yüzde değeri değişiklikleri örnekleme, nasıl kısa süre içinde biz yeniden daha fazla veri yakalamak için örnekleme artırmak için izin verilir.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Yüzde örnekleme değiştikçe biz ayarlamak için izin verilen en düşük değer nedir.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Yüzde örnekleme değiştikçe ne biz ayarlamak için izin en yüksek değerdir.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    Hareketli ortalamanın hesaplanmasında ağırlık en son değer atanmış. Eşit veya 1'den küçük bir değer kullanın. Küçük değerler algoritma ani için daha az reaktif değişiklikleri yapın.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Uygulamanın yalnızca başladığında atanan değer. Hatalarını ayıkladığınız sırada bu azaltma yok. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Örneklenen istemediğiniz türleri noktalı virgülle ayrılmış listesi. Türler tanınan: bağımlılık, olay, özel durum, sayfa görünümü, istek, izleme. Belirtilen türlerle tüm örneklerini iletilir; belirtilmediği türleri örneklenen.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Örneklenen istediğiniz türlerini noktalı virgülle ayrılmış listesi. Türler tanınan: bağımlılık, olay, özel durum, sayfa görünümü, istek, izleme. Belirtilen türlerle örneklenen; diğer türleri tüm örneklerini her zaman aktarılmaz.


**Geçmek** Uyarlamalı örnekleme, AdaptiveSamplingTelemetryProcessor düğümü applicationınsights yapılandırma dosyasından kaldırın.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatif: Uyarlamalı örnekleme yapılandırma
Örnekleme parametre .config dosyası ayarı yerine bu değerleri programlı olarak ayarlayabilirsiniz. Örnekleme hızını tekrar değerlendirilir olduğunda çağrılan bir geri çağırma işlevini belirtmenize olanak tanır. Bu, örneğin, hangi örnekleme hızını kullanılmakta bulmak için kullanabilirsiniz.

Kaldırma `AdaptiveSamplingTelemetryProcessor` .config dosyası düğümden.

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

([Telemetri işlemciler hakkında daha fazla bilgi](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>JavaScript web sayfaları için örnekleme
Web sayfaları için herhangi bir sunucudan sabit oranı örnekleme yapılandırabilirsiniz. 

Olduğunda, [web sayfaları için Application Insights yapılandırma](app-insights-javascript.md), Application Insights portalından aldığınız JavaScript kod parçacığı değiştirin. (ASP.NET uygulamalarında kod parçacığını genellikle _Layout.cshtml içinde sağlanabilir.)  Benzer bir satır ekleyin `samplingPercentage: 10,` izleme anahtarını önce:

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

Örnekleme yüzdesi, 100/N yakın N bir tamsayı olduğu bir yüzde seçin.  Şu anda örnekleme diğer değerleri desteklemiyor.

Ayrıca sunucu sabit oranı örnekleme etkinleştirirseniz, söz konusu, arama ilgili sayfa görünümleri ve istekler arasında gezinebilirsiniz için sunucu ve istemcileri eşitler.

## <a name="fixed-rate-sampling-for-aspnet-and-java-web-sites"></a>ASP.NET ve Java web siteleri için sabit oranı örnekleme
Sabit oranı örnekleme, web sunucunuz ve web tarayıcıları gönderilen trafiğini azaltır. Uyarlamalı örnekleme aksine, telemetri sizin tarafınızdan karar sabit bir oranda azaltır. Bu ayrıca istemci ve sunucu örnekleme eşitler böylece ilgili öğeler korunur - Örneğin, arama sayfası görünümünde baktığınızda, ilgili isteğini bulabilirsiniz.

Örnekleme algoritması ilgili öğeler korur. Her HTTP isteği için olay, istek ve ilgili olaylar atılan ya da birlikte aktarılan. 

Yaklaşık doğru; böylece ölçümleri Explorer'da için örnekleme hızını dengelemek için bir faktör istek ve özel durum sayısı gibi oranları çarpılır.

### <a name="configuring-fixed-rate-sampling-in-aspnet"></a>Sabit oran örnekleme ASP.NET yapılandırma ###

1. **Projenizin NuGet paketlerini güncelleştirmeyi** en son *yayın öncesi* Application Insights sürümü. Visual Studio'da Çözüm Gezgini'nde projeye sağ tıklayın, NuGet paketlerini Yönet seçin denetleyin **dahil et** ve Microsoft.applicationınsights.Web arayın. 
2. **Uyarlamalı örnekleme devre dışı**: içinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), kaldırmak veya çıkışı açıklama `AdaptiveSamplingTelemetryProcessor` düğümü.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

3. **Sabit oran örnekleme modülü etkinleştirin.** Bu parçacığını ekleyin [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
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

1. İndirme ve son, web uygulamanızın yapılandırma [uygulama Öngörüler java SDK'sı](app-insights-java-get-started.md)

2. **Sabit oran örnekleme modülü etkinleştirmek** aşağıdaki kod parçacığında Applicationınsights.xml dosyasına ekleyerek düzenleyin.

```XML
    <TelemetryProcessors>
        <BuiltInProcessors>
            <Processor type = "FixedRateSamplingTelemetryProcessor">
                <!-- Set a percentage close to 100/N where N is an integer. -->
                <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
                <Add name = "SamplingPercentage" value = "50" />
            </Processor>
        </BuilrInProcessors>
    <TelemetryProcessors/>
```

3. Dahil edebilirsiniz veya işlemci etiketi "FixedRateSamplingTelemetryProcessor" içine aşağıdaki etiketleri kullanarak örnekleme gelen telemetri belirli türlerdeki hariç tutun
```XML
    <ExcludedTypes>
        <ExcludedType>Request</ExcludedType>
    </ExcludedTypes>

    <IncludedTypes>
        <IncludedType>Exception</IncludedType>
    </IncludedTypes>
```
Hangi dahil veya gelen örnekleme hariç telemetri türleri şunlardır: bağımlılık, olay, özel durum, sayfa görünümü, istek ve izleme.

> [!NOTE]
> Örnekleme yüzdesi, 100/N yakın N bir tamsayı olduğu bir yüzde seçin.  Şu anda örnekleme diğer değerleri desteklemiyor.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternatif: sunucu kodunuzdaki sabit oranı örnekleme etkinleştir
Örnekleme parametre .config dosyası ayarı yerine bu değerleri programlı olarak ayarlayabilirsiniz. 

*C#*

```csharp

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Telemetri işlemciler hakkında daha fazla bilgi](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-to-use-sampling"></a>Ne zaman örnekleme kullanılır?
Uyarlamalı örnekleme ASP.NET SDK sürüm 2.0.0-beta3 kullanırsanız, otomatik olarak etkinleştirilmiş veya sonraki bir sürümü. Hangi kullandığınız SDK'sı sürümünden bağımsız olarak, toplanan veri örneği Application Insights izin vermek alım örnekleme etkinleştirebilirsiniz.

Varsayılan olarak hiçbir örnekleme Java SDK'etkindir. Şu anda yalnızca sabit oranı örnekleme destekler. Uyarlamalı örnekleme Java SDK'desteklenmiyor.

Genel olarak, en küçük ve orta ölçekli uygulamalar için örnekleme gerekmez. En doğru istatistikleri ve en kullanışlı tanılama bilgileri, tüm kullanıcı etkinliklerini veri toplayarak elde edilir. 

Örnekleme ana avantajları şunlardır:

* Uygulamanızı telemetri çok yüksek oranda kısa gönderdiğinde uygulama Öngörüler hizmeti düşme ("kısıtlamaları") veri noktaları aralığı zaman. 
* İçinde tutmak için [kota](app-insights-pricing.md) fiyatlandırma katmanınızı veri noktaları. 
* Telemetri koleksiyonundan ağ trafiğini azaltmak için. 

### <a name="which-type-of-sampling-should-i-use"></a>Hangi tür örnekleme kullanmalıyım?
**Alım varsa örnekleme kullanın:**

* Genellikle, telemetri aylık kota de gidin.
* Örnekleme - Örneğin ASP.NET sürüm 2'den önceki desteklemiyor SDK sürümünü kullanıyorsunuz.
* Çok sayıda telemetri kullanıcılarınızın web tarayıcılarından alıyorsunuz.

**Sabit oranı, örnekleme kullanın:**

* ASP.NET web Hizmetleri sürüm 2.0.0 Application Insights SDK'sı kullanıyorsanız veya üstü ya da Java SDK'sı v2.0.1 veya sonraki bir sürümü ve
* Olayların ne zaman çalışıyoruz böylece istemci ve sunucu arasında eşitlenen örnekleme istediğiniz [arama](app-insights-diagnostic-search.md), istemci ve sunucu, sayfa görünümleri ve http istekleri gibi ilgili olaylar arasında gezinebilirsiniz.
* Uygulamanız için uygun örnekleme yüzdesi emin olduğunuz. Doğru ölçümler almak için yüksek olmalı, ancak fiyatlandırma kota ve azaltma sınırlarını aşıyor oranı. 

**Uyarlamalı örnekleme kullanın:**

Örnekleme başka biçimlerde koşulları uygulamazsanız, Uyarlamalı örnekleme öneririz. Bu varsayılan ASP.NET sunucusu SDK sürüm 2.0.0-beta3 olarak etkin veya sonraki bir sürümü. Belirli bir minimum oran ulaşılana kadar trafiği azaltmaz, bu nedenle, az kullanılan siteler etkilenmez.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Örnekleme işlemi olup olmadığını nasıl anlayabilirim?
Uygulanmış burada olsun gerçek örnekleme hızını keşfetmek için bir [Analytics sorgu](app-insights-analytics.md) bu gibi:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

Her kaydı tutulur `itemCount` temsil eder, özgün kayıt sayısını gösteren 1 + önceki atılan kayıt sayısı eşit. 

## <a name="how-does-sampling-work"></a>Örnekleme nasıl çalışır?
Sabit oran örnekleme özelliği 2.0.0 ASP.NET sürümlerden SDK'da ve Java SDK'sı sürüm 2.0.1 ve sonraki sürümlerde. Uyarlamalı örnekleme bir SDK'sının başlayarak 2.0.0 ASP.NET sürümlerden özelliğidir. Alım örnekleme Application Insights hizmeti, bir özelliğidir ve SDK örnekleme gerçekleştirmiyorsa işleminde olabilir. 

Örnekleme algoritması bırakmak için hangi telemetri öğeleri ve hangilerinin (Bu SDK veya Application Insights hizmeti olup olmadığı) tutmak için karar verir. Örnekleme karar tüm birbiriyle veri noktalarını olduğu gibi bir işlem yapılabilir ve hatta sınırlı bir veri kümesi ile güvenilir Application Insights tanılama deneyimi koruma korumak için hedeflenir çeşitli kurallar temel alır. Başarısız bir istek için ek telemetri öğeleri (örneğin, özel durumu ve bu istekten oturum izlemeleri) uygulamanızı gönderirse, örneğin, örnekleme bu istek ve başka telemetriyle bölecek değil. Tutar ya da hepsini bir araya bırakır. Sonuç olarak, Application Insights isteği ayrıntılarında baktığınızda, her zaman istekle ilişkili telemetri öğelerinden birlikte görebilirsiniz. 

Örnekleme kararı, belirli bir işlemi ait tüm telemetri öğeleri korunur veya bırakılan olduğunu anlamına gelir isteği işlemi kimliğini temel alır. İşlem olmayan telemetri öğeler için kimliği kümesi (için örnek telemetri öğeleri http bağlam ile zaman uyumsuz iş parçacıklarından bildirilen) örnekleme telemetri öğelerin her tür yüzdesi yalnızca yakalar. .NET SDK'ın 2.5.0-Beta2 ve ASP.NET Core SDK 2.2.0-beta3 öncesinde örnekleme karar "kullanıcı" tanımlamak uygulamaları için kullanıcı kimliği karmasını temel (diğer bir deyişle, en tipik web uygulamaları). Kullanıcılar (örneğin, web Hizmetleri) tanımlamadığınız uygulama türleri için örnekleme karar isteği işlemi kimliği temel alarak.

Telemetri size geri sunan Application Insights hizmeti ölçümler için veri noktaları dengelemek için kullanılan koleksiyon, aynı anda aynı örnekleme yüzdeyle ayarlanır. Bu nedenle, Application Insights telemetrisi bakıldığında kullanıcıların çok yakın gerçek sayılar olan istatistiksel olarak doğru yakın görüyorsunuz.

Yaklaşık doğruluğu büyük ölçüde yapılandırılmış örnekleme yüzdesi bağlıdır. Ayrıca, kullanıcıların çok sayıda genellikle benzer isteklerinden büyük miktarda işleyen uygulamalar için doğruluğu artırır. Bu uygulamalar genellikle kendi telemetri azaltma gelen veri kaybına neden olmadan kotanın kalsanız gönderebilirsiniz gibi diğer taraftan, önemli bir yük ile çalışmıyor uygulamalar için örnekleme gerekli değildir. 

> [!WARNING]
> Application Insights ölçümleri ve oturumlar telemetri türlerini örnek değil. Duyarlık azalma bu telemetri türleri için yüksek oranda istenmeyen olabilir.
> 

### <a name="adaptive-sampling"></a>Uyarlamalı örnekleme
Uyarlamalı örnekleme iletim SDK'dan gelen geçerli hızı izler ve hedef en yüksek hızı içinde kalmasını denemek için örnekleme yüzdesi ayarlayan bir bileşen ekler. Ayarlama, düzenli aralıklarla yeniden hesaplanır ve giden aktarım hızını üzerinde bir hareketli ortalama dayanır.

## <a name="sampling-and-the-javascript-sdk"></a>Örnekleme ve JavaScript SDK'sı
Sunucu tarafı SDK'sı ile birlikte sabit oranı örnekleme istemci-tarafı (JavaScript) SDK katıldığı. İzleme eklenmiş sayfaları için sunucu tarafı kendi "SAMPLE" içinde kararı kullanıcılardan yalnızca istemci-tarafı telemetri gönderir Bu mantık, istemci - ve sunucu-tarafı boyunca kullanıcı oturumunun bütünlüğünü sağlamak için tasarlanmıştır. Sonuç olarak, Application Insights herhangi belirli telemetri öğeden bu kullanıcıyı ya da oturumu için diğer tüm telemetri öğeleri bulabilirsiniz. 

*Yukarıda açıklanan şekilde my istemci ve sunucu tarafı telemetri Eşgüdümlü örnekleri gösterme.*

* Sabit oran örnekleme hem de sunucu ve istemci etkin olduğunu doğrulayın.
* SDK sürüm 2.0 olduğundan emin olun veya üstü.
* İstemci ve sunucu aynı örnekleme yüzdesine ayarlayın denetleyin.

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular
*Bir basit "toplama her telemetri türü yüzdesi X" örnekleme değil neden?*

* Bu örnekleme yaklaşım ölçüm yaklaşık değerler, çok yüksek duyarlılık ile sağlayacak olsa da, kullanıcı, oturum ve tanılama için önemli olan istek başına tanılama verilerin bağıntısını yeteneği bölün. Bu nedenle, works daha iyi "tüm telemetri öğeleri için yüzde X uygulama kullanıcılarının toplamak" veya "uygulama isteklerin yüzdesi X için tüm telemetri Topla" örnekleme mantığı. Sıfırlamaya kullanmaktır (örneğin, arka plan zaman uyumsuz işleme) isteklerle ilişkili olmayan telemetri öğeleri için "her telemetri türü için tüm öğeleri yüzdesi X topla." 

*Örnekleme yüzdesi zamanla değiştirebilir miyim?*

* Evet, Uyarlamalı örnekleme telemetri şu anda gözlemlenen hacmine dayalı örnekleme yüzdesini aşamalı olarak değiştirir.

*Sabit oran örnekleme kullanırsanız, nasıl anlarım hangi örnekleme yüzdesi Uygulamam için en iyi çalışır?*

* Bir yolu başlamak Uyarlamalı örnekleme, ne kapatır (yukarıdaki soruya bakın) üzerinde derecelendirmek ve sabit hızında geçiş Bul hızı kullanarak örnekleme. 
  
    Aksi takdirde, tahmin gerekir. Geçerli telemetri kullanımınızı Application ınsights'ta çözümleme, oluştuğunu her azaltma inceleyin ve toplanan telemetri hacmini tahmin etme. Seçilen fiyatlandırma katmanınızı birlikte bu üç girişleri ne kadar toplanan telemetri birimi küçültmek isteyebilirsiniz öneririz. Ancak, kullanıcılarınızın sayısı artan ya da diğer bazı shift telemetri birimindeki, tahmin geçersiz.

*Örnekleme yüzdesi çok düşük yapılandırırsanız ne olur?*

* Veri birimi azaltılması için veri görselleştirme dengelemek Application Insights denediğinde, aşırı düşük örnekleme yüzdesi (over-aggressive örnekleme) yaklaşık değerler doğruluğunu azaltır. En sık başarısız olan veya yavaş isteklerden bazılarının çıkışı örneklenebilir olarak da, tanılama deneyimi olumsuz, etkilenebilir.

*Örnekleme yüzdesi çok yüksek yapılandırırsanız ne olur?*

* Çok yüksek örnekleme yüzdesi (değil agresif yeterince) yapılandırma toplanan telemetri hacmi yetersiz bir azalmayla sonuçlanır. Application Insights kullanma maliyetini nedeniyle fazla kullanım ücretleri planlanan daha yüksek olabilir ve azaltma için ilgili telemetri veri kaybı hala karşılaşabilirsiniz.

*Hangi platformlarda örnekleme kullanabilir miyim?*

* SDK örnekleme gerçekleştirmiyor alım örnekleme otomatik olarak belirli bir birim üzerinde herhangi bir telemetri için ortaya çıkabilir. ASP.NET SDK veya Java SDK(1.0.10 or before) önceki sürümünü daha eski bir sürümü kullanıyorsanız, bu, örneğin, çalışır.
* ASP.NET SDK sürüm 2.0.0 kullanıyorsanız ve üstünde (Azure veya kendi sunucusunda barındırılan), Uyarlamalı örnekleme varsayılan olarak alır ancak yukarıda açıklandığı gibi sabit hızında geçiş yapabilirsiniz. Sabit oran örnekleme ile ilgili olaylar örneklemek için SDK tarayıcı otomatik olarak eşitler. 
* Java SDK'sı sürüm 2.0.1 kullanıyorsanız veya Applicationınsights.XML üzerinde sabit oranı örnekleme etkinleştirmek için yukarıdaki yapılandırabilirsiniz değilse. Örnekleme varsayılan olarak kapalıdır. Sabit oran örnekleme ile ilgili olaylar örneklemek için SDK tarayıcı otomatik olarak eşitler.

*Her zaman görmek istiyorum nadir belirli olaylar vardır. Nasıl bunları örnekleme modülü alabilirim?*

* Yeni TelemetryConfiguration (varsayılan etkin) ile TelemetryClient ayrı bir örneğini başlatır. Nadir olayları göndermek için bunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* [Filtreleme](app-insights-api-filtering-sampling.md) ne, SDK gönderir, daha katı denetim sağlayabilir.

