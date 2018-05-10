---
title: Kullanıcı Gezinti desenlerle kullanıcı akar Azure Application ınsights'ta çözümleme | Microsoft docs
description: Kullanıcılar web uygulamanızı özelliklerini ve sayfaları arasında nasıl gidin analiz edin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/24/2018
ms.author: mbullwin; daviste
ms.openlocfilehash: 5913039d3ab288c2fc8366073b340538989512ee
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Kullanıcı Gezinti desenlerle kullanıcı akar Application ınsights'ta Çözümle

![Uygulama kullanıcı Öngörüler akar aracı](./media/app-insights-usage-flows/00001-flows.png)

Kullanıcı akar aracı, kullanıcılarınızın sayfaları ve sitenizin özellikleri arasında nasıl gezindiğini visualizes. Gibi sorulara yanıt verilmesi için mükemmeldir:

* Nasıl kullanıcıların, sitenizde bir sayfa başka bir sayfaya geçemezsiniz?
* Sitenizin sayfasında hangi kullanıcıların tıklayın?
* Kullanıcılar en sitenizden karmaşıklığı yerleri nerede?
* Burada kullanıcılar aynı eylemi tekrar tekrar yineleyin yerler var mı?

Bir başlangıç sayfası görünümü, özel bir olay veya belirttiğiniz özel durum kullanıcı akar Aracı'nı başlatır. Bu ilk olay verildiğinde, kullanıcı aktığını önce ve daha sonra kullanıcı oturumları sırasında gerçekleşen olayları gösterir. Değişen kalınlığı satırlarını her yol kullanıcılar tarafından izlenen kaç kez gösterir. Özel **oturumu başlattı** düğümleri Göster burada sonraki düğümlerin bir oturumu başladı. **Oturum sona erdi** düğümleri Göster kaç kullanıcının hiçbir sayfa görünümleri veya özel olaylar sonra önceki düğümü, burada Kullanıcılar sitenizi büyük olasılıkla sol vurgulama gönderilen.

> [!NOTE]
> Application Insights kaynağınıza, sayfa görünümleri veya kullanıcı akar aracı kullanmak için özel olaylar içermesi gerekir. [Sayfa görünümleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](app-insights-javascript.md).
>
>

## <a name="start-by-choosing-an-initial-event"></a>İlk olay seçerek başlatın

![Kullanıcı akışlar için ilk bir olay seçin](./media/app-insights-usage-flows/00002-flows-initial-event.png)

Kullanıcı akar aracıyla sorulara yanıt verilmesi başlamak için ilk sayfa görünümü, özel olay ya da görselleştirme için başlangıç noktası olarak hizmet verecek özel durumu seçin:

1. Bağlantıya tıklayın **kullanıcılar sonra ne yapacaksınız...?**  başlık veya tıklatın **Düzenle** düğmesi.
2. Bir sayfa görünümü, özel bir olay ya da özel durumu seçin **ilk olay** açılır.
3. Tıklatın **oluşturma grafik**.

Görselleştirme "1. adım" sütunun ne kullanıcıların en sık gelen en az sık yukarıdan sıralı yalnızca ilk olay sonra yaptığını gösterir. Kullanıcılar bundan sonra ne yaptığını "2. adım" ve sonraki sütunları göster tüm yolları kullanıcılar kullanıcılarınızın siteniz aracılığıyla gittiğinizde bir resim oluşturma.

Varsayılan olarak, kullanıcı akar aracı yalnızca son 24 saat sayfa görünümleri ve sitenizin özel olay rastgele örnekler. Zaman aralığını artırın ve performans ve doğruluk bakiyesi Düzenle menüsünde rastgele örnekleme için değiştirin.

Bazı sayfa görünümleri, özel etkinlikler ve özel durumlar için ilgili değilse tıklatın **X** gizlemek istediğiniz düğümlerde. İstediğiniz gizlemek için düğümleri seçtikten sonra **oluşturma grafik** görselleştirme aşağıdaki düğmesine. Tüm gizli düğümleri görmek için tıklatın **Düzenle** düğmesini tıklatıp bakmak **olayları dışlanan** bölümü.

Sayfa görünümleri veya özel olaylar eksik ise üzerinde görselleştirme görmeyi beklediğiniz:

* Denetleme **olayları dışlanan** bölümüne **Düzenle** menüsü.
* Artı düğmeleri kullanın **başkalarının** düğümleri görselleştirme daha az sıklıkta olaylarını içerir.
* Sayfa görünümü ya da özel olay beklediğiniz kullanıcılar tarafından seyrek gönderilirse görselleştirme zaman aralığını artırmayı deneyin **Düzenle** menüsü.
* Sayfa emin olun görüntülemek için özel bir olay veya özel durum beklediğiniz sitenizin kaynak kodundaki Application Insights SDK'sı tarafından toplanacak ayarlanır. [Özel olaylar toplama hakkında daha fazla bilgi edinin.](app-insights-api-custom-events-metrics.md)

Daha fazla adım görseldeki görmek istiyorsanız, kullanmak **önceki adımları** ve **sonraki adımlar** bırakmalar görselleştirme üstünde.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Bir sayfa veya özellik ziyaret ettikten sonra kullanıcıların gittikleri ve ne bunlar tıklatın?

![Burada kullanıcılar'ı anlamak için kullanıcı akar kullanın](./media/app-insights-usage-flows/00003-flows-one-step.png)

İlk olay sayfa görünümü ise, görselleştirme ilk sütun ("Adım 1") sayfasını ziyaret hemen sonra kullanıcıların ne yaptığını anlamak için hızlı bir yoludur. Kullanıcı akar görselleştirme yanındaki bir pencerede sitenizi açmayı deneyin. Kullanıcıların "1. adım" sütununda olayların listesini sayfasına nasıl etkileşim, beklentilerinizi karşılaştırın. Genellikle, ekibinizin anlamsız görünüyor sayfasında bir kullanıcı Arabirimi öğesi, en sık kullanılan sayfasında arasında olabilir. Siteniz için tasarım geliştirmeleri için harika bir başlangıç noktası olabilir.

İlk olay özel bir olay ise, ilk sütun bu eylemi gerçekleştirdikten sonra kullanıcıların ne yaptığını gösterir. Sayfa görünümlerle gibi kullanıcılarınızın gözlemlenen davranışını ekibinizin hedefleri ve beklentileri eşleşip eşleşmediğini göz önünde bulundurun. Seçili, ilk olay "Öğesi için ü alışveriş sepetine eklenen" ise, örneğin, "Checkout Git" ve "Satın alma tamamlandı" görselleştirme kısa süre içinde bundan sonra görünüyorsa görmek için arayın. Kullanıcı davranışlarını beklentilerinizi farklı ise, kullanıcıların nasıl alma anlamak için görsel öğe kullanın "sitenizin geçerli tasarım gereği yakalanan".

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Kullanıcılar en sitenizden karmaşıklığı yerleri nerede?

İzlemesi **oturumu sona erdi** yüksek yukarı görünen düğümü özellikle başlarında bir akış görselleştirme sütununda. Bu, büyük olasılıkla önceki yol sayfaları ve kullanıcı Arabirimi etkileşimleri izledikten sonra sitenizden churned çok sayıda kullanıcı anlamına gelir. Bazen karmaşası - Örneğin, bir e-ticaret sitesinde bir satın alma tamamladığınızda - bekleniyordu, ancak genellikle karmaşası bir tasarım sorunları, düşük performans veya diğer sorunlar sitenizle geliştirilebilir işaretidir.

Göz önünde bulundurmanız, **oturumu sona erdi** düğümleri yalnızca bu Application Insights kaynağı tarafından toplanan telemetri dayanır. Application Insights telemetri belirli kullanıcı etkileşimleri almazsa, oturum sona kullanıcı akar aracı diyor sonra kullanıcılar hala bu yolla sitenizle kurduğunda.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Burada kullanıcılar aynı eylemi tekrar tekrar yineleyin yerler var mı?

Sayfa görünümü veya birden çok kullanıcı tarafından görselleştirme sonraki adımlarda arasında yinelenen özel olay arayın. Bu, genellikle kullanıcıların, sitenizde yinelenen Eylemler performans gösterdiğini anlamına gelir. Yineleme bulursanız, sitenizin tasarımını değiştirme veya yineleme azaltmak için yeni işlevsellik ekleme hakkında düşünün. Örneğin, her bir tablo öğesi satırındaki yinelenen eylemler gerçekleştirme kullanıcılar bulursanız Toplu Düzenle işlevselliği ekleme.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>İsteğe bağlı olarak her zaman bir oturumda görünür veya ilk olay temsil olay bir oturum görünür ilk kez mu?

Görselleştirme üzerindeki ilk olay yalnızca bir kullanıcı bu sayfa görünümü veya özel olay bir oturum sırasında gönderilen ilk kez temsil eder. Kullanıcılar, birden çok kez bir oturumda ilk olay gönderebilir sonra "1. adım" sütunu, yalnızca sonra kullanıcıların nasıl davranacağını gösterir *ilk* ilk olay örneği, tüm örnekleri.

### <a name="some-of-the-nodes-in-my-visualization-are-too-high-level-for-example-a-node-that-just-says-button-clicked-how-can-i-break-it-down-into-more-detailed-nodes"></a>Çok yüksek düzeyde my görselleştirme düğümlerin bazılarıdır. Örneğin, yalnızca "Düğmesine tıklama" belirten bir düğüm Nasıl ı, daha ayrıntılı düğümlerin içine ayırabilirsiniz?

Kullanım **bölme** içinde seçenekleri **Düzenle** menüsü:

1. İçinde bölmek istediğiniz olayı seçin **olay** menüsü.
2. Bir boyut seçin **boyut** menüsü. "Düğmesine Tıklanana" adlı bir olay varsa, örneğin, "Düğmesine adı." adlı bir özel özellik deneyin

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıma genel bakış](app-insights-usage-overview.md)
* [Kullanıcıları, oturumlar ve olaylar](app-insights-usage-segmentation.md)
* [Bekletme](app-insights-usage-retention.md)
* [Uygulamanıza özel olaylar ekleme](app-insights-api-custom-events-metrics.md)