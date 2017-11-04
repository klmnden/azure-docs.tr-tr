---
title: "Azure Application Insights ile kullanım analizi | Microsoft docs"
description: "Kullanıcılarınızın ve uygulamanızı ile neler yaptığını anlayın."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 10/10/2017
ms.author: mbullwin
ms.openlocfilehash: 57d9ebc5a9689a6f1d48464aa20ffdc7fa61b00f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="usage-analysis-with-application-insights"></a>Application Insights ile kullanım analizi

Web veya mobil uygulama özelliklerine en popüler misiniz? Kullanıcılarınızın uygulamanızla hedeflerine ulaşması? Bunlar belirli zamanlarda bırakma ve döndürmeleri daha sonra?  [Azure Application Insights](app-insights-overview.md) , kişiler, uygulamanızın kullanımını içine güçlü Öngörüler kazanmanıza yardımcı olur. Uygulamanızı güncelleştirme her zaman, kullanıcılar için ne kadar iyi çalıştığı değerlendirebilirsiniz. Bu bilgiyle, veri sonraki geliştirme döngüsü hakkında kararlar tabanlı yapabilirsiniz.

## <a name="send-telemetry-from-your-app"></a>Telemetriyi uygulamanızdan Gönder

Uygulama sunucusu kodunuzu hem de web sayfalarınıza Application Insights yükleyerek en iyi deneyimi elde edilir. İstemci ve sunucu bileşenleri, uygulamanızın telemetri geri analiz için Azure portalı gönderin.

1. **Sunucu kodu:** için uygun modülünü yükleyin, [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), veya [diğer](app-insights-platforms.md) uygulama.

    * *Sunucu kodu yüklemek istemiyor musunuz? Yalnızca [Azure Application Insights kaynağı oluşturma](app-insights-create-new-resource.md).*

2. **Web sayfası koduna:** açmak [Azure portal](https://portal.azure.com), uygulamanız için Application Insights kaynağı açın ve ardından açın **Başlarken > İzleme ve tanılama istemci tarafı**. 

    ![Komut dosyası ana web sayfanızın head kopyalayın.](./media/app-insights-usage-overview/02-monitor-web-page.png)

3. **Mobil uygulama kodu:** uygulamanızdan olaylarını toplamak ve ardından bu olayların kopyalarını göre Analiz için Application Insights'a gönderme mobil Center SDK'yı kullanma [bu kılavuz aşağıdaki](app-insights-mobile-center-quickstart.md).

4. **Telemetri alın:** projeniz için bir kaç dakika hata ayıklama modunda çalıştırılması ve genel bakış dikey penceresinde Application Insights sonuçlarında arayın.

    Uygulamanızın performansını izlemek ve uygulamanızla kullanıcıların ne yaptıklarını bulmak için uygulamanızı yayımlayın.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Kullanıcı ve oturum kimliği, telemetri dahil
Zaman içinde kullanıcıları izlemek için Application Insights bunları belirlemenin bir yolu gerektirir. Bir kullanıcı kimliği veya bir oturum kimliği gerektirmez yalnızca kullanım aracı olayları araçtır

Kullanıcı ve oturum kimliklerini kullanarak göndermeye Başla [bu işlem](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Kullanım demografisine ve istatistikleri keşfedin
Kişiler uygulamanızı kullandığınızda, bunlar en, kullanıcılarınızın bulunduğu ilgileniyor hangi sayfaların, hangi tarayıcılar ve işletim sistemleri bunlar kullandığını öğrenin. 

Kullanıcı ve oturum raporları, veri sayfaları veya özel olaylar tarafından filtre ve bunları konumu, ortam ve sayfa gibi özelliklere göre segmentlere ayırmak. Kendi filtreler de ekleyebilirsiniz.

![Kullanıcılar](./media/app-insights-usage-overview/users.png)  

Veri kümesi ilginç kalıpları çıkışı sağdaki Öngörüler gelin.  

* **Kullanıcılar** rapor içinde seçilen zaman dönemleriniz sayfalarınızı erişim benzersiz kullanıcı sayısı sayar. Web uygulamaları için tanımlama bilgilerini kullanarak kullanıcıların sayılır. Birisi farklı tarayıcılar veya istemci makinelerle sitenize erişen veya kendi tanımlama bilgilerini temizler, ardından bunlar birden çok kez sayılacaktır.
* **Oturumları** raporu, sitenize erişen kullanıcı oturumlarını sayar. Bir süre etkinlik bir süre işlem yapılmadığında birden fazla yarım saat biri tarafından sonlandırıldı, bir kullanıcı tarafından oturumdur.

[Kullanıcıları, oturumlar ve olayları araçları hakkında daha fazla bilgi](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Sayfa görünümleri

Kullanım dikey penceresinden en popüler sayfalarınızı dökümünü almak için sayfa görünümleri döşeme tıklayın:

![Genel Bakış dikey penceresinden sayfa görünümleri grafiği tıklatın](./media/app-insights-usage-overview/05-games.png)

Yukarıdaki örnek bir oyun web sitesinden ' dir. Grafikte, biz hemen görebilirsiniz:

* Kullanım, geçen hafta içinde geliştirilmiş kurmadı. Belki de arama motoru iyileştirme hakkında düşünüyoruz?
* Tenis en popüler oyun sayfasıdır. Daha fazla geliştirmeleri bu sayfaya şimdi odaklanır.
* Ortalama, kullanıcıların tenis sayfasını yaklaşık üç kez haftalık ziyaret edin. (Yaklaşık üç kat daha fazla oturumları kullanıcıları daha vardır.)
* Kullanıcıların çoğunun ABD çalışma hafta sırasında ve çalışma saatleri içinde sitesini ziyaret edin. Web sayfasında belki de bir "hızlı gizle" düğmesini sunuyoruz.
* [Ek açıklamaları](app-insights-annotations.md) Web sitesi yeni sürümlerini ne zaman dağıtılan grafikte göster. Son dağıtımlarda hiçbirinin kullanım belirgin bir etkisi vardı.

Ne sitenizi siteniz, sayfa görünümü telemetrisi gönderir özel bir özellik tarafından bölme gibi daha ayrıntılı trafiği incelemek istediğiniz?

1. Açık **olayları** Application Insights kaynağı menüde aracı. Bu araç kaç sayfa görünümleri ve özel olaylar çeşitli süzme, cohorting ve kesimleme seçenekleri dayalı uygulamanızdan gönderilen analiz etmenize olanak sağlar.
2. "Kimin kullanılan" açılır listede "Any sayfa görünümü" seçin.
3. "Tarafından bölme" açılır listede, sayfa görünümü telemetrisi bölmek bir özellik seçin.

## <a name="retention---how-many-users-come-back"></a>Bekletme - kaç kullanıcı döndürülmesini?

Bekletme ne sıklıkta bir, belirli bir zaman aralığındaki yüzdeler sırasında bazı iş eylemi gerçekleştiren kullanıcı cohorts göre kendi uygulama kullanmak için kullanıcılarınızın dönüş anlamanıza yardımcı olur. 

- Hangi belirli özellikleri diğerlerinden geri daha fazla gelen kullanıcıların neden anlama 
- Form varsayımlar gerçek kullanıcı verilerine dayalı 
- Bekletme ürününüz için bir sorun olup olmadığını 

![Bekletme](./media/app-insights-usage-overview/retention.png) 

Üstteki bekletme denetimleri, belirli olayları ve saklama hesaplamak için zaman aralığını tanımlamanıza olanak sağlar. Orta grafiğinde görsel bir genel saklama yüzdesi belirtilen zaman aralığına göre sağlar. Grafiğin altındaki belirli bir dönemde saklama temsil eder. Bu düzeyde ayrıntı, kullanıcıların ne yaptıklarını ve ne daha ayrıntılı ayrıntı düzeyi döndürmeyi kullanıcıları etkileyebilecek anlamanıza olanak sağlar.  

[Bekletme aracı hakkında daha fazla bilgi](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Özel iş olayları

Uygulamanızla kullanıcıların ne NET bir anlayış almak için özel günlük olaylarıyla kod satırlarını eklemek yararlıdır. Bu olaylar herhangi bir şey satın alma veya oyun kazanma gibi daha önemli iş olaylarına belirli düğmelerini gibi ayrıntılı kullanıcı eylemlerine izleyebilirsiniz. 

Bazı durumlarda, sayfa görünümleri yararlı olaylar gösterebilir rağmen genel doğru değil. Bir kullanıcı, ürün satın alma olmadan bir ürün sayfasını açabilir. 

Belirli iş olaylarla kullanıcılarınızın siteniz aracılığıyla kullanıcılarınızın ilerleme grafik. Farklı seçenekler için tercihlerini çıkışı bulabilir ve bunlar bırakma out veya sorunlar vardır. Bu bilgiyle, geliştirme kapsamınızı önceliklerini hakkında bilinçli kararlar yapabilirsiniz.

Uygulama istemci tarafında olaylar kaydedilebilir:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Veya sunucu tarafı:

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Filtre ya da Portalı'nda incelediğinizde olayları bölme özellik değerlerini bu olaylara iliştirebilirsiniz. Ayrıca, tek bir kullanıcıya etkinlik dizisini izlemenizi sağlayan bir anonim kullanıcı kimliği gibi her olay için bir standart özellikler kümesi eklenir.

Daha fazla bilgi edinmek [özel olaylar](app-insights-api-custom-events-metrics.md#trackevent) ve [özellikleri](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Dilimlediği olayları

Kullanıcıları, oturumlar ve olayları araçlarında dilim ve kullanıcı, olay adı ve özellikleri tarafından özel olaylar inin.
![Kullanıcılar](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Tasarım telemetri uygulama

Her bir özellik, uygulamanızın tasarlarken, kullanıcılarınız ile başarısını ölçmek için nasıl adımıdır göz önünde bulundurun. Hangi iş olaylarını kaydetmek için gereken ve izleme olayları için uygulamanıza başından çağıran kodu karar verin.

## <a name="a--b-testing"></a>A | B testi
Bir özelliğin hangi değişken daha başarılı olacaktır bilmiyorsanız, bunların her farklı erişilebilir kullanıcıların her ikisi de serbest bırakın. Her başarısını ölçmenize ve birleştirilmiş bir sürüme taşıyın.

Bu bir teknik uygulamanızı her sürümü tarafından gönderilen tüm telemetri için ayrı özellik değerlerini ekleyin. Bunu etkin TelemetryContext özelliklerini tanımlayarak yapabilirsiniz. Bu varsayılan özellikleri, uygulamanın gönderdiği - her telemetri iletiye özel iletilerinizi değil, ancak standart telemetriyi de eklenir.

Application Insights portalında filtre ve farklı sürümlerini karşılaştırmak için özellik değerleri, verilerinizde bölebilirsiniz.

Bunu yapmak için [telemetri Başlatıcı ayarlama](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

Web uygulama Başlatıcı Global.asax.cs gibi:

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Tüm yeni TelemetryClients belirttiğiniz özellik değeri otomatik olarak ekler. Telemetri olaylarını tek tek varsayılan değerleri geçersiz kılabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
   - [Kullanıcılar, Oturumlar, Etkinlikler](app-insights-usage-segmentation.md)
   - [Huniler](usage-funnels.md)
   - [Bekletme](app-insights-usage-retention.md)
   - [Kullanıcı Akışları](app-insights-usage-flows.md)
   - [Çalışma kitapları](app-insights-usage-workbooks.md)
   - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
