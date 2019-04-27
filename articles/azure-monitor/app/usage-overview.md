---
title: Azure Application Insights ile kullanım analizi | Microsoft docs
description: Kullanıcılarınız ve uygulamanızla neler yaptığını anlayın.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/10/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: f2539d5250ff436a720fe10f748f40db29b0ee25
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60783442"
---
# <a name="usage-analysis-with-application-insights"></a>Application Insights ile kullanım analizi

Web veya mobil uygulamanızı hangi özelliklerinin en popüler misiniz? Kullanıcılarınız uygulamanızla hedeflerine ulaşması? Belirli noktalarda bıraktıklarını ve daha sonra iade etmeden?  [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) , insanların uygulamanızı nasıl kullandığını içine güçlü Öngörüler elde etmeye yardımcı olur. Uygulamanızı her güncelleştirdiğinizde, kullanıcılar için ne kadar iyi çalıştığı değerlendirebilirsiniz. Bu bilgiyle, veri odaklı kararlar sonraki geliştirme döngülerinizi yapabilirsiniz.

## <a name="send-telemetry-from-your-app"></a>Uygulamanızdan telemetri gönderin

Application Insights hem uygulama sunucu kodunuzda ve web sayfalarınızda yükleyerek en iyi deneyimi elde edilir. Uygulamanızın istemci ve sunucu bileşenleri telemetri analizi için Azure portalı için geri gönderin.

1. **Sunucu kodu:** İçin uygun modülünü yüklemek, [ASP.NET](../../azure-monitor/app/asp-net.md), [Azure](../../azure-monitor/app/app-insights-overview.md), [Java](../../azure-monitor/app/java-get-started.md), [Node.js](../../azure-monitor/app/nodejs.md), veya [diğer](../../azure-monitor/app/platforms.md) uygulama.

    * *Sunucu kodu yüklemek istemiyor musunuz? Yalnızca [Azure Application Insights kaynağı oluşturma](../../azure-monitor/app/create-new-resource.md ).*

2. **Web sayfası koduna:** Kapatmadan önce web sayfası için aşağıdaki betiği ekleyin ``</head>``. Uygun değere Application Insights kaynağınızın izleme anahtarını değiştirin:

   ```javascript
      <script type="text/javascript">
        var appInsights=window.appInsights||function(a){
            function b(a){c[a]=function(){var b=arguments;c.queue.push(function(){c[a].apply(c,b)})}}var c={config:a},d=document,e=window;setTimeout(function(){var b=d.createElement("script");b.src=a.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js",d.getElementsByTagName("script")[0].parentNode.appendChild(b)});try{c.cookie=d.cookie}catch(a){}c.queue=[];for(var f=["Event","Exception","Metric","PageView","Trace","Dependency"];f.length;)b("track"+f.pop());if(b("setAuthenticatedUserContext"),b("clearAuthenticatedUserContext"),b("startTrackEvent"),b("stopTrackEvent"),b("startTrackPage"),b("stopTrackPage"),b("flush"),!a.disableExceptionTracking){f="onerror",b("_"+f);var g=e[f];e[f]=function(a,b,d,e,h){var i=g&&g(a,b,d,e,h);return!0!==i&&c["_"+f](a,b,d,e,h),i}}return c
        }({
            instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx"
        });
        
        window.appInsights=appInsights,appInsights.queue&&0===appInsights.queue.length&&appInsights.trackPageView();
    </script>
    ```
    Web sitelerini izlemek için kullanabileceğiniz daha gelişmiş yapılandırmalar hakkında bilgi edinmek için [JavaScript SDK API başvurusunu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md) inceleyin.

3. **Mobil uygulama kodu:** Uygulamanızdan olayları toplayabilir ve bu olayların kopyalarını göre Analiz için Application Insights'a gönderme için App Center SDK'sını kullanın [bu kılavuzu izleyerek](../../azure-monitor/learn/mobile-center-quickstart.md).

4. **Telemetri alın:** Projeniz için bir kaç dakika hata ayıklama modunda çalıştırabilir ve sonuçları Application ınsights'ta genel bakış dikey penceresinde bulun.

    Uygulamanızın performansını izleyin ve uygulamanızla kullanıcıların ne yaptıklarını bulmak için uygulamanızı yayımlayın.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Kullanıcı ve oturum kimliği telemetrinizi içerir
Zaman içinde kullanıcıları izlemek için Application Insights bunları belirlemenin bir yolu gerektirir. Bir kullanıcı kimliği veya bir oturum kimliği gerektirmeyen yalnızca kullanımı aracı olayları araçtır

Kullanıcı ve oturum kimliklerini kullanarak göndermeye başlayın [bu işlem](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Kullanım demografik bilgileri ve istatistikleri keşfedin
İnsanların uygulamanızı kullandığınızda, bunlar en, kullanıcılarınızın bulunduğu yere ilginizi çeken hangi sayfaların, hangi tarayıcılar ve işletim sistemleri, kullandığını keşfedin. 

Kullanıcı ve oturum raporları sayfaları veya özel olaylar verilerinizi filtrelemek ve bunları konumu ve ortam sayfa gibi özelliklere göre segmentlere ayırın. Kendi filtrelerinizi de ekleyebilirsiniz.

![Kullanıcılar](./media/usage-overview/users.png)  

Çıkış veri kümesinde ilginç kalıpları ınsights sağdaki gelin.  

* **Kullanıcılar** rapor sayfalarınızın erişimi, seçilen süreler içinde benzersiz kullanıcı sayısı sayar. Web apps için tanımlama bilgilerini kullanan kullanıcılar sayılır. Birisi farklı tarayıcılar veya istemci makineleri sitenizle erişen veya kendi tanımlama bilgilerini temizler, ardından bunların birden çok kez sayılır.
* **Oturumları** rapor sitenize erişen kullanıcı oturumlarının sayısını sayar. Etkinlik birden fazla yarım saat, belirli bir süre tarafından sonlandırıldı, bir kullanıcı tarafından bir süre oturumdur.

[Kullanıcılar, oturumlar ve olaylar araçları hakkında daha fazla bilgi](usage-segmentation.md)  

## <a name="retention---how-many-users-come-back"></a>Bekletme - kaç kullanıcının döndürülmesini?

Bekletme ne sıklıkta belirli bir zaman aralığı sırasında bazı iş eylemi gerçekleştiren kullanıcı kohortlar göre uygulamasını kullanmak için kullanıcılarınızın dönüş anlamanıza yardımcı olur. 

- Hangi belirli özellikleri diğerlerinden geri daha fazla gelen kullanıcıların neden anlama 
- Gerçek kullanıcı verilerine dayalı form varsayımlar 
- Bekletme ürününüzün bir sorun olup olmadığını belirleme 

![Bekletme](./media/usage-overview/retention.png) 

Üstte Tutma denetimler belirli olayları ve bekletme hesaplamak için zaman aralığını tanımlamanızı sağlar. Orta grafikte belirtilen zaman aralığına göre toplam bekletme yüzdesinin görsel bir temsili sağlar. Alt grafik saklama belirli bir süre içinde temsil eder. Bu düzeyde ayrıntı, kullanıcılarınızın neler yaptığını ve hangi daha ayrıntılı ayrıntı döndüren kullanıcıları etkileyebilecek anlamanıza olanak tanır.  

[Bekletme aracıyla ilgili daha fazla bilgi](usage-retention.md)

## <a name="custom-business-events"></a>Özel iş olayları

Kullanıcıların uygulamanızla neler Temizle anlamak için özel olaylarını günlüğe kaydedecek şekilde kod satırlarını eklemek yararlıdır. Bu olaylar herhangi bir şey satın almadan ya da bir oyun kazanma gibi daha önemli iş olaylarına belirli düğmelere tıklamak gibi ayrıntılı kullanıcı eylemlerine izleyebilirsiniz. 

Bazı durumlarda, sayfa görüntülemeleri yararlı olaylar gösterebilir ancak genel olarak geçerli değildir. Bir kullanıcı, ürün satın alma olmadan bir ürün sayfasını açabilirsiniz. 

Belirli iş olayları ile kullanıcılarınızın siteniz aracılığıyla kullanıcılarınızın ilerleme grafik. Çıkış için farklı seçenekleri tercihlerini bulabilir ve ayrıldıklarını out veya güçlük. Bu bilgiyle, geliştirme kapsamınızda öncelikleri hakkında bilinçli kararlar yapabilirsiniz.

Uygulama istemci tarafında olayları kaydedilebilir:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Veya sunucu tarafı:

```csharp
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Böylece filtrelemek veya olayları portalda incelediğinizde bölme bu olayları, özellik değerlerini ekleyebilirsiniz. Ayrıca, standart bir özellikler kümesini bir dizi bireysel bir kullanıcı etkinliklerini izlemenizi sağlayan anonim kullanıcı kimliği gibi her bir olay eklenir.

Daha fazla bilgi edinin [özel olaylar](../../azure-monitor/app/api-custom-events-metrics.md#trackevent) ve [özellikleri](../../azure-monitor/app/api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Dilimlediği olayları

Kullanıcılar, oturumlar ve olaylar araçları dilim ve özel olaylar kullanıcı, olay adı ve özellikleri ayrıntılı olarak inceleyin.
![Kullanıcılar](./media/usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Tasarım uygulaması ile telemetri

Uygulamanızın her bir özelliği tasarlarken, Kullanıcılarınızla başarısını ölçmek için nasıl yükleyeceksiniz göz önünde bulundurun. Hangi iş olaylarını kaydetmek için gereken ve izleme olayları için uygulamanıza başından çağıran kod karar verin.

## <a name="a--b-testing"></a>A | B testi
Bir özelliğin hangi çeşidin daha başarılı olacaktır bilmiyorsanız, her ikisi de her farklı erişilebilir kullanıcıların serbest bırakın. Her başarısını ölçmek ve birleşik bir sürüme taşıyın.

Bu yöntem için ayrı özellik değerlerini uygulamanızın her sürüm tarafından gönderilen tüm telemetri ekleyin. İçinde active TelemetryContext özellikleri tanımlayarak bunu yapabilirsiniz. Bu varsayılan özellikleri uygulama gönderen - her telemetri iletiye özel iletilerinizi değil, ancak standart telemetri de eklenir.

Application Insights portalında, filtreleme ve farklı sürümleri karşılaştırmak için özellik değerleri, verilerinizde bölebilirsiniz.

Bunu yapmak için [bir telemetri Başlatıcısı kümesi](../../azure-monitor/app/api-filtering-sampling.md##add-properties-itelemetryinitializer):

```csharp


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

Web uygulama başlatıcısında Global.asax.cs gibi:

```csharp

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Tüm yeni TelemetryClients belirttiğiniz özellik değeri otomatik olarak ekleyin. Telemetri olaylarını tek tek varsayılan değerleri geçersiz kılabilir.

## <a name="next-steps"></a>Sonraki adımlar
   - [Kullanıcılar, Oturumlar, Etkinlikler](usage-segmentation.md)
   - [Huniler](usage-funnels.md)
   - [Bekletme](usage-retention.md)
   - [Kullanıcı Akışları](usage-flows.md)
   - [Çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)
   - [Kullanıcı bağlamı Ekle](usage-send-user-context.md)
