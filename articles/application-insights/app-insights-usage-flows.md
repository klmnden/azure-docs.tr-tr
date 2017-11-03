---
title: "Kullanıcı Gezinti desenlerle kullanıcı akar Azure Application ınsights'ta çözümleme | Microsoft docs"
description: "Kullanıcılar web uygulamanızı özelliklerini ve sayfaları arasında nasıl gidin analiz edin."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d17ed3dff08f00a1d6a2108608e42b29f95fbd84
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Kullanıcı Gezinti desenlerle kullanıcı akar Application ınsights'ta Çözümle

![Uygulama kullanıcı Öngörüler akar aracı](./media/app-insights-usage-flows/flows.png)

Kullanıcı akar aracı, kullanıcılarınızın sayfaları ve sitenizin özellikleri arasında nasıl gezindiğini visualizes. Gibi sorulara yanıt verilmesi için mükemmeldir:
* Nasıl kullanıcıların, sitenizde bir sayfa başka bir sayfaya geçemezsiniz?
* Sitenizin sayfasında hangi kullanıcıların tıklayın?
* Kullanıcılar en sitenizden karmaşıklığı yerleri nerede?
* Burada kullanıcılar aynı eylemi tekrar tekrar yineleyin yerler var mı?

Kullanıcı akar aracını bir ilk sayfa görünümü ya da belirttiğiniz bir olay başlatır. Bu sayfa görünümü veya özel olay verildiğinde, kullanıcı aktığını sayfa görünümleri ve daha sonra bir oturumu sırasında iki adımları hemen ardından, vb. kullanıcılara gönderilen özel olayları gösterir. Değişen kalınlığı satırlarını her yol kullanıcılar tarafından izlenen kaç kez gösterir. Özel "Oturumu sona erdi" düğümleri kaç kullanıcının hiçbir sayfa görünümleri veya özel olaylar sonra önceki düğümü, burada Kullanıcılar sitenizi büyük olasılıkla sol vurgulama gönderilen gösterir.



> [!NOTE]
> Application Insights kaynağınıza, sayfa görünümleri veya kullanıcı akar aracı kullanmak için özel olaylar içermesi gerekir. [Sayfa görünümleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>İlk sayfa görünümü veya özel olay seçerek başlatın

![Kullanıcı akışlar için ilk bir olay seçin](./media/app-insights-usage-flows/flows-initial-event.png)

Kullanıcı akar aracıyla sorulara yanıt verilmesi başlamak için bir başlangıç sayfa görünümü veya görselleştirme için başlangıç noktası olarak hizmet vermek için özel olay seçin:
1. Bağlantıya tıklayın "kullanıcıların sonra ne yapacaksınız...?" Başlık veya Düzenle düğmesini tıklatın. 
2. Bir sayfa görünümü veya özel olay "İlk olay" aşağı açılır listeden seçin.
3. "Grafik oluştur" seçeneğini tıklatın.

Görselleştirme "1. adım" sütunun ne kullanıcıların en sık üst alta gelen çoğu için en az-sıklıkta sıralı yalnızca ilk olay sonra yaptığını gösterir. Kullanıcılar bundan sonra ne yaptığını "2. adım" ve sonraki sütunları göster tüm yolları kullanıcılar kullanıcılarınızın siteniz aracılığıyla gittiğinizde bir resim oluşturma.

Varsayılan olarak, kullanıcı akar aracı yalnızca son 24 saat sayfa görünümleri ve sitenizin özel olay rastgele örnekler. Zaman aralığını artırın ve performans ve doğruluk bakiyesi Düzenle menüsünde rastgele örnekleme için değiştirin.

Bazı sayfa görünümleri ve özel olaylar, ilgili değilse gizlemek istediğiniz düğümlerde "X"'i tıklatın. Gizlemek istediğiniz düğümleri seçtikten sonra görselleştirme aşağıdaki "Oluştur grafik" düğmesine tıklayın. Tüm gizli düğümleri görmek için Düzenle düğmesini tıklatın, sonra "olayları dışlanan" kısmına bakın.

Sayfa görünümleri veya özel olaylar eksik ise üzerinde görselleştirme görmeyi beklediğiniz:
* Düzen menüsü "olayları dışlanan" bölümüne bakın.
* "Ayrıntı düzeyi" denetimi Düzenle menüde görselleştirme daha az sıklıkta olayları içermek üzere kullanın.
* Sayfa görünümü ya da özel olay beklediğiniz kullanıcılar tarafından seyrek gönderilirse Düzenle menüsünde görselleştirme zaman aralığını artırmayı deneyin.
* Sayfa emin olun görüntüleyin veya sitenizin kaynak kodundaki Application Insights SDK'sı tarafından toplanacak beklediğiniz özel olay ayarlayın. [Özel olaylar toplama hakkında daha fazla bilgi edinin.](app-insights-api-custom-events-metrics.md)

Daha fazla adım görseldeki görmek istiyorsanız, "Adımları sayısı" kaydırıcıyı Düzen menüsünde kullanın.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Bir sayfa veya özellik ziyaret ettikten sonra kullanıcıların gittikleri ve ne bunlar tıklatın?

![Burada kullanıcılar'ı anlamak için kullanıcı akar kullanın](./media/app-insights-usage-flows/flows-one-step.png)

İlk olay sayfa görünümü ise, görselleştirme ilk sütun ("Adım 1") sayfasını ziyaret hemen sonra kullanıcıların ne yaptığını anlamak için hızlı bir yoludur. Kullanıcı akar görselleştirme yanındaki bir pencerede sitenizi açmayı deneyin. Kullanıcıların "1. adım" sütununda olayların listesini sayfasına nasıl etkileşim, beklentilerinizi karşılaştırın. Genellikle, ekibinizin anlamsız görünüyor sayfasında bir kullanıcı Arabirimi öğesi, en sık kullanılan sayfasında arasında olabilir. Siteniz için tasarım geliştirmeleri için harika bir başlangıç noktası olabilir.

İlk olay özel bir olay ise, ilk sütun bu eylemi gerçekleştirdikten sonra kullanıcıların ne yaptığını gösterir. Sayfa görünümlerle gibi kullanıcılarınızın gözlemlenen davranışını ekibinizin hedefleri ve beklentileri eşleşip eşleşmediğini göz önünde bulundurun. Seçili, ilk olay "Öğesi için ü alışveriş sepetine eklenen" ise, örneğin, "Checkout Git" ve "Satın alma tamamlandı" görselleştirme kısa süre içinde bundan sonra görünüyorsa görmek için arayın. Kullanıcı davranışlarını beklentilerinizi çok farklı ise, kullanıcıların nasıl alma anlamak için görsel öğe kullanın "sitenizin geçerli tasarım gereği yakalanan".

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Kullanıcılar en sitenizden karmaşıklığı yerleri nerede?

Gözcü yüksek yukarı görünmesini "Oturumu sona erdi" düğümler için özellikle başlarında bir akış görselleştirme sütununda. Bu, büyük olasılıkla önceki yol sayfaları ve kullanıcı Arabirimi etkileşimleri izledikten sonra sitenizden churned çok sayıda kullanıcı anlamına gelir. Bazen karmaşası - Örneğin, bir e-ticaret sitesinde bir satın alma tamamladığınızda - bekleniyordu, ancak genellikle karmaşası bir tasarım sorunları, düşük performans veya diğer sorunlar sitenizle geliştirilebilir işaretidir.

Bu "oturum düğümleri yalnızca bu Application Insights kaynağı tarafından toplanan telemetri dayanır sona" göz önünde bulundurun. Application Insights telemetri belirli kullanıcı etkileşimleri almazsa, oturum sona kullanıcı akar aracı diyor sonra kullanıcılar hala bu yolla sitenizle kurduğunda.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Burada kullanıcılar aynı eylemi tekrar tekrar yineleyin yerler var mı?

Sayfa görünümü veya birden çok kullanıcı tarafından görselleştirme sonraki adımlarda arasında yinelenen özel olay arayın. Bu, genellikle kullanıcıların, sitenizde yinelenen Eylemler performans gösterdiğini anlamına gelir. Yineleme bulursanız, sitenizin tasarımını değiştirme veya yineleme azaltmak için yeni işlevsellik ekleme hakkında düşünün. Örneğin, her bir tablo öğesi satırındaki yinelenen eylemler gerçekleştirme kullanıcılar bulursanız Toplu Düzenle işlevselliği ekleme.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="why-do-steps-appear-disconnected"></a>Adımları neden bağlantısı kesilmiş görünüyor?

![Bağlantısı kesilmiş adımlara kullanıcı akışlar](./media/app-insights-usage-flows/flows-disconnected.png)

Bir kullanıcı akar görselleştirme (sütunları) adımlarda bağlantısı kesilirse, kullanıcılar adımları arasında tarafından gerçekleştirilecek yolların hiçbiri gösterilecek sık anlamına gelir. Adımları bağlı görünmesi için daha az sıklıkta olayları üzerinde görselleştirme göstermek için "Ayrıntı düzeyi" kaydırıcıyı Düzen menüsü Ayarla.

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>İsteğe bağlı olarak her zaman bir oturumda görünür veya ilk olay temsil olay bir oturum görünür ilk kez mu?

Görselleştirme üzerindeki ilk olay yalnızca bir kullanıcı bu sayfa görünümü veya özel olay bir oturum sırasında gönderilen ilk kez temsil eder. Kullanıcılar, birden çok kez bir oturumda ilk olay gönderebilir sonra "1. adım" sütunu, yalnızca sonra kullanıcıların nasıl davranacağını gösterir *ilk* ilk olay örneği, tüm örnekleri.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıma genel bakış](app-insights-usage-overview.md)
* [Kullanıcıları, oturumlar ve olaylar](app-insights-usage-segmentation.md)
* [Bekletme](app-insights-usage-retention.md)
* [Uygulamanıza özel olaylar ekleme](app-insights-api-custom-events-metrics.md)