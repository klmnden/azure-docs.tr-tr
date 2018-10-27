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
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/10/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: eeaf46a9ce523ecd11689d0aa430fcc522732f70
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139865"
---
# <a name="usage-analysis-with-application-insights"></a>Application Insights ile kullanım analizi

Web veya mobil uygulamanızı hangi özelliklerinin en popüler misiniz? Kullanıcılarınız uygulamanızla hedeflerine ulaşması? Belirli noktalarda bıraktıklarını ve daha sonra iade etmeden?  [Azure Application Insights](app-insights-overview.md) , insanların uygulamanızı nasıl kullandığını içine güçlü Öngörüler elde etmeye yardımcı olur. Uygulamanızı her güncelleştirdiğinizde, kullanıcılar için ne kadar iyi çalıştığı değerlendirebilirsiniz. Bu bilgiyle, veri odaklı kararlar sonraki geliştirme döngülerinizi yapabilirsiniz.

## <a name="send-telemetry-from-your-app"></a>Uygulamanızdan telemetri gönderin

Application Insights hem uygulama sunucu kodunuzda ve web sayfalarınızda yükleyerek en iyi deneyimi elde edilir. Uygulamanızın istemci ve sunucu bileşenleri telemetri analizi için Azure portalı için geri gönderin.

1. **Sunucu kodu:** için uygun modülünü yüklemek, [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), veya [diğer](app-insights-platforms.md) uygulama.

    * *Sunucu kodu yüklemek istemiyor musunuz? Yalnızca [Azure Application Insights kaynağı oluşturma](app-insights-create-new-resource.md).*

2. **Web sayfası koduna:** açın [Azure portalında](https://portal.azure.com), uygulamanız için Application Insights kaynağını açın ve ardından açın **Başlarken > İzleme ve tanılama istemci tarafı**. 

    ![Betik ana web sayfanızın baş kopyalayın.](./media/app-insights-usage-overview/02-monitor-web-page.png)

3. **Mobil uygulama kodu:** uygulamanızdan olayları toplayabilir ve bu olayların kopyalarını göre Analiz için Application Insights'a gönderme için App Center SDK'sını kullanın [bu kılavuzu izleyerek](app-insights-mobile-center-quickstart.md).

4. **Telemetri alın:** projeniz için bir kaç dakika hata ayıklama modunda çalıştırabilir ve sonuçları Application ınsights'ta genel bakış dikey penceresinde bulun.

    Uygulamanızın performansını izleyin ve uygulamanızla kullanıcıların ne yaptıklarını bulmak için uygulamanızı yayımlayın.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Kullanıcı ve oturum kimliği telemetrinizi içerir
Zaman içinde kullanıcıları izlemek için Application Insights bunları belirlemenin bir yolu gerektirir. Bir kullanıcı kimliği veya bir oturum kimliği gerektirmeyen yalnızca kullanımı aracı olayları araçtır

Kullanıcı ve oturum kimliklerini kullanarak göndermeye başlayın [bu işlem](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Kullanım demografik bilgileri ve istatistikleri keşfedin
İnsanların uygulamanızı kullandığınızda, bunlar en, kullanıcılarınızın bulunduğu yere ilginizi çeken hangi sayfaların, hangi tarayıcılar ve işletim sistemleri, kullandığını keşfedin. 

Kullanıcı ve oturum raporları sayfaları veya özel olaylar verilerinizi filtrelemek ve bunları konumu ve ortam sayfa gibi özelliklere göre segmentlere ayırın. Kendi filtrelerinizi de ekleyebilirsiniz.

![Kullanıcılar](./media/app-insights-usage-overview/users.png)  

Çıkış veri kümesinde ilginç kalıpları ınsights sağdaki gelin.  

* **Kullanıcılar** rapor sayfalarınızın erişimi, seçilen süreler içinde benzersiz kullanıcı sayısı sayar. Web apps için tanımlama bilgilerini kullanan kullanıcılar sayılır. Birisi farklı tarayıcılar veya istemci makineleri sitenizle erişen veya kendi tanımlama bilgilerini temizler, ardından bunların birden çok kez sayılır.
* **Oturumları** rapor sitenize erişen kullanıcı oturumlarının sayısını sayar. Etkinlik birden fazla yarım saat, belirli bir süre tarafından sonlandırıldı, bir kullanıcı tarafından bir süre oturumdur.

[Kullanıcılar, oturumlar ve olaylar araçları hakkında daha fazla bilgi](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Sayfa görünümleri

Kullanım dikey penceresinden en popüler sayfalarınızı dökümünü almak için sayfa görünümü kutucuğu aracılığıyla tıklayın:

![Genel Bakış dikey penceresinden bir sayfa görünümleri grafiğe tıklayın](./media/app-insights-usage-overview/05-games.png)

Yukarıdaki örnekte, bir oyun web sitesinden bulunur. Grafikten anında görebiliriz:

* Kullanım geçen hafta içinde geliştirilmiş edilmemiş. Belki de arama motoru iyileştirme hakkında düşünüyoruz?
* Tenis en popüler oyun sayfasıdır. Ek geliştirmeler bu sayfaya odaklanalım.
* Ortalama olarak, kullanıcıların tenis sayfasını yaklaşık üç kez her hafta ziyaret edin. (Kullanıcılara yaklaşık üç kat daha fazla oturum var.)
* Çoğu kullanıcı, çalışma saatleri ve ABD çalışma haftası boyunca sitesini ziyaret edin. Belki de biz bir "hızlı gizle" düğmesi web sayfasında sağlamanız gerekir.
* [Ek açıklamaları](app-insights-annotations.md) yeni sürümleri Web sitesinin ne zaman dağıtılan grafikte göster. Son dağıtımları hiçbiri bir fark edilebilir etkisi kullanımı gerekiyordu.

Trafik sitenize siteniz, sayfa görünümü telemetrisini gönderir, özel bir özellik tarafından bölme gibi daha ayrıntılı olarak araştırmak istersek?

1. Açık **olayları** araç ve Application Insights kaynak menüsünde. Bu araç kaç sayfa görüntülemeleri ve özel olaylar üzerinde filtreleme, cohorting ve segmentasyon seçeneklerini çeşitli tabanlı uygulamanızdan gönderilen analiz etmenize olanak tanır.
2. "Kullanan" açılır menüde "Any sayfa görünümü" seçin.
3. "Bölme ölçütü" açılır menüde, sayfa görünümü telemetrisini ayırmak bir özellik seçin.

## <a name="retention---how-many-users-come-back"></a>Bekletme - kaç kullanıcının döndürülmesini?

Bekletme ne sıklıkta belirli bir zaman aralığı sırasında bazı iş eylemi gerçekleştiren kullanıcı kohortlar göre uygulamasını kullanmak için kullanıcılarınızın dönüş anlamanıza yardımcı olur. 

- Hangi belirli özellikleri diğerlerinden geri daha fazla gelen kullanıcıların neden anlama 
- Gerçek kullanıcı verilerine dayalı form varsayımlar 
- Bekletme ürününüzün bir sorun olup olmadığını belirleme 

![Bekletme](./media/app-insights-usage-overview/retention.png) 

Üstte Tutma denetimler belirli olayları ve bekletme hesaplamak için zaman aralığını tanımlamanızı sağlar. Orta grafikte belirtilen zaman aralığına göre toplam bekletme yüzdesinin görsel bir temsili sağlar. Alt grafik saklama belirli bir süre içinde temsil eder. Bu düzeyde ayrıntı, kullanıcılarınızın neler yaptığını ve hangi daha ayrıntılı ayrıntı döndüren kullanıcıları etkileyebilecek anlamanıza olanak tanır.  

[Bekletme aracıyla ilgili daha fazla bilgi](app-insights-usage-retention.md)

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

Daha fazla bilgi edinin [özel olaylar](app-insights-api-custom-events-metrics.md#trackevent) ve [özellikleri](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Dilimlediği olayları

Kullanıcılar, oturumlar ve olaylar araçları dilim ve özel olaylar kullanıcı, olay adı ve özellikleri ayrıntılı olarak inceleyin.
![Kullanıcılar](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Tasarım uygulaması ile telemetri

Uygulamanızın her bir özelliği tasarlarken, Kullanıcılarınızla başarısını ölçmek için nasıl yükleyeceksiniz göz önünde bulundurun. Hangi iş olaylarını kaydetmek için gereken ve izleme olayları için uygulamanıza başından çağıran kod karar verin.

## <a name="a--b-testing"></a>A | B testi
Bir özelliğin hangi çeşidin daha başarılı olacaktır bilmiyorsanız, her ikisi de her farklı erişilebilir kullanıcıların serbest bırakın. Her başarısını ölçmek ve birleşik bir sürüme taşıyın.

Bu yöntem için ayrı özellik değerlerini uygulamanızın her sürüm tarafından gönderilen tüm telemetri ekleyin. İçinde active TelemetryContext özellikleri tanımlayarak bunu yapabilirsiniz. Bu varsayılan özellikleri uygulama gönderen - her telemetri iletiye özel iletilerinizi değil, ancak standart telemetri de eklenir.

Application Insights portalında, filtreleme ve farklı sürümleri karşılaştırmak için özellik değerleri, verilerinizde bölebilirsiniz.

Bunu yapmak için [bir telemetri Başlatıcısı kümesi](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

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
   - [Kullanıcılar, Oturumlar, Etkinlikler](app-insights-usage-segmentation.md)
   - [Huniler](usage-funnels.md)
   - [Bekletme](app-insights-usage-retention.md)
   - [Kullanıcı Akışları](app-insights-usage-flows.md)
   - [Çalışma kitapları](app-insights-usage-workbooks.md)
   - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
