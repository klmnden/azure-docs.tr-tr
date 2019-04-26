---
title: Kullanıcı Akışları'nda Azure Application Insights ile kullanıcı Gezinti düzenlerini çözümleme | Microsoft docs
description: Kullanıcıların sayfalarını ve web uygulamanızın özelliklerini arasında nasıl gittiğini analiz edin.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 91274fad4e56c69777333c81ea3b32dccdcf64ff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60373309"
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Kullanıcı Akışları'nda Application Insights ile kullanıcı Gezinti düzenlerini çözümleme

![Application Insights kullanıcı akışları aracı](./media/usage-flows/00001-flows.png)

Kullanıcı akışları aracı, kullanıcıların sayfalarını ve özelliklerini sitenizin arasında nasıl gittiğini görselleştirir. Gibi sorulara yanıt vermek için harikadır:

* Kullanıcıların nasıl sitenizin sayfasında uzağa gittiğini?
* Sitenizin sayfasında hangi kullanıcıların tıklayın?
* Kullanıcıların, sitenizde en değişim sıklığı yerleri nerede?
* Burada kullanıcıların tekrar tekrar aynı eylemi yineleyin yer var mı?

Bir ilk sayfa görüntülemesi, özel olay veya belirttiğiniz özel durum kullanıcı akışları Aracı'nı başlatır. Bu ilk olay göz önünde bulundurulduğunda, kullanıcı akışları önce ve daha sonra kullanıcı oturumları sırasında gerçekleşen olayları gösterir. Değişen kalınlığı satırlarını kaç kez kullanıcılar tarafından izlenen her bir yol gösterir. Özel **oturum açtığında** düğümleri gösterme burada sonraki düğümler oturumu başladı. **Oturum sona erdi** düğümleri gösterme kaç kullanıcının hiçbir sayfa görünümleri veya özel olaylar önceki düğümünün sonra kullanıcılar büyük olasılıkla sitenizi sol nerede vurgulama gönderilir.

> [!NOTE]
> Application Insights kaynağınıza, sayfa görünümleri veya kullanıcı akışları aracı kullanmak için özel olaylar içermelidir. [Sayfa görüntülemeleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](../../azure-monitor/app/javascript.md).
>
>

## <a name="start-by-choosing-an-initial-event"></a>İlk olay seçerek başlayın

![İlk olay kullanıcı akışları için seçin](./media/usage-flows/00002-flows-initial-event.png)

Kullanıcı akışları aracı ile ilgili sorularını yanıtlama başlamak için bir ilk sayfa görüntülemesi, özel olay veya görselleştirme için başlangıç noktası olarak hizmet verecek özel durumu seçin:

1. Bağlantıya tıklayın **kullanıcılar sonra ne...?**  başlık veya tıklayın **Düzenle** düğmesi.
2. Bir sayfa görüntülemesi, özel olay veya özel durumdan seçin **ilk olay** açılır.
3. Tıklayın **Oluştur graf**.

Görselleştirmenin "1. adım" sütunu ne kullanıcıların en sık sonra gelen en az sık yukarıdan sıralı yalnızca ilk olayı yaptığını gösterir. Kullanıcılar bundan sonra ne yaptığını "2. adım" ve sonraki sütunları göster tüm yolları kullanıcılar, kullanıcılarınızın siteniz aracılığıyla çıkıldığında bir resim oluşturma.

Kullanıcı akışları aracı, varsayılan olarak, sayfa görüntülemeleri ve özel olay sitenizden yalnızca son 24 saat rastgele örnekler. Zaman aralığını artırın ve Düzen menüsündeki rastgele örnekleme için performans ve doğruluk dengesini değiştirin.

Bazı sayfa görüntülemesi, özel olayları ve özel durumlar, ilgili değilse tıklayın **X** gizlemek istediğiniz düğümleri üzerinde. İstediğiniz gizlemek için düğümlerinin seçtikten sonra **Oluştur graf** görselleştirme aşağıda düğmesi. Tüm gizli düğümleri görmek için tıklayın **Düzenle** düğmesine ve ardından bakmak **olayları hariç** bölümü.

Sayfa görünümleri veya özel olaylar, yoksa görselleştirmeye görmeyi beklediğiniz:

* Denetleme **olayları hariç** konusundaki **Düzenle** menüsü.
* Artı çubuğunda düğmelerini **başkalarının** düğümleri daha az sıklıkta olayları görselleştirmeye dahil edilecek.
* Beklediğiniz özel olay ve sayfa görünümü kullanıcı tarafından seyrek gönderilirse görselleştirmede zaman aralığını artırmayı deneyin **Düzenle** menüsü.
* Emin olun sayfayı görüntülemek, özel olay veya özel durum beklediğiniz sitenizin kaynak kodunda Application Insights SDK tarafından toplanacak ayarlanır. [Özel olay toplama hakkında daha fazla bilgi edinin.](../../azure-monitor/app/api-custom-events-metrics.md)

Daha fazla görselleştirme adımları görmek istiyorsanız, kullanın **önceki adımları** ve **sonraki adımlar** görselleştirmenin açılır listeler.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Bir sayfa ya da özellik ziyaret ettikten sonra kullanıcıların gittikleri ve bunlar ı ne?

![Burada kullanıcılar'ı anlamak için kullanıcı Akışları'ı kullanın](./media/usage-flows/00003-flows-one-step.png)

Sayfa görünümü, ilk olay ise görselleştirmeyi ilk sütuna ("Adım 1") hemen sayfasını ziyaret ederek sonra kullanıcıların ne yaptığını anlamak için hızlı bir yoludur. Kullanıcı akışları görselleştirme yanında bir penceresinde sitenize açmayı deneyin. Kullanıcıların "1. adım" sütununda olayların listesi sayfasına nasıl etkileşim, beklentilerinizi karşılaştırın. Genellikle, ekibinizin Önemsiz görünüyor sayfasında bir kullanıcı Arabirimi öğesi, en çok kullanılan sayfasında arasında olabilir. Bu, Web sitenize tasarım geliştirmeleri için harika bir başlangıç noktası olabilir.

Özel olay, ilk olay ise ilk sütun, bu eylemi gerçekleştirdikten sonra kullanıcıların ne yaptığını gösterir. Sayfa görüntülemeleri gibi ile kullanıcılarınızın gözlemlenen davranışını takımınızın hedefleri ve beklentileri eşleşip eşleşmediğini göz önünde bulundurun. Seçilen ilk olay "Öğesi için ü alışveriş sepetine eklendi" ise, örneğin, "Kullanıma Git" ve "Satın alma tamamlandı" kısa süre sonra görselleştirmede görünüyorsa görmek için bakın. Kullanıcı davranışını beklentilerinizi farklı ise, kullanıcıların nasıl alıyorsanız anlamak için görselleştirme kullanın "sitenizin geçerli tasarım tarafından yakalanan".

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Kullanıcıların, sitenizde en değişim sıklığı yerleri nerede?

İzlemesi **oturumu sona erdi** yüksek yukarı görünen düğümleri erken özellikle bir akışta görselleştirmeyi bir sütunda. Bu, çok sayıda kullanıcı büyük olasılıkla önceki yol sayfaları ve kullanıcı Arabirimi etkileşimleri yararlandıktan sonra sitenizden çoğaltılmaları anlamına gelir. Bazen değişim sıklığı - Örneğin, bir e-ticaret sitesinde satın tamamladığınızda - beklenir, ancak genellikle değişim sıklığı bir tasarım sorunları, kötü performansa veya geliştirilebilir siteniz ile ilgili diğer sorunlar işaretidir.

Göz önünde bulundurun, **oturumu sona erdi** düğümleri yalnızca bu Application Insights kaynağı tarafından toplanan telemetri dayanır. Application Insights telemetri için belirli kullanıcı etkileşimlerine almazsa, kullanıcı akışları aracı oturum sona erdi diyor sonra kullanıcılar yine de bu şekillerde sitenizle kurulabilen.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Burada kullanıcıların tekrar tekrar aynı eylemi yineleyin yer var mı?

Sayfa görünümü veya görselleştirmeyi sonraki adımlarda arasında birden çok kullanıcı tarafından yinelenen özel olay olup olmadığına bakın. Bu, genellikle kullanıcıların, sitenizde yinelenen Eylemler çalıştığını anlamına gelir. Yineleme bulursanız, sitenizin tasarımını değiştirmek veya yineleme azaltmak için yeni işlevsellik ekleme hakkında düşünün. Örneğin, yinelenen Eylemler gerçekleştirmeden bir table öğesinin her bir satırdaki kullanıcılar bulursanız toplu düzenleme işlevselliği ekleme.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>İlk olay temsil ilk kez bir oturumda olay görünür yapar veya isteğe bağlı olarak her zaman bir oturumda görüntülenen?

İlk olay görselleştirme üzerinde yalnızca bir kullanıcı bir oturum sırasında bu sayfa görünümü veya özel olay gönderilen ilk kez temsil eder. Kullanıcılar, birden çok kez oturumda ilk olay gönderebilirsiniz sonra "1. adım" sütunu, yalnızca kullanıcıların'sonra nasıl davranacağını gösterir *ilk* ilk olay örneği, tüm örnekleri.

### <a name="some-of-the-nodes-in-my-visualization-are-too-high-level-for-example-a-node-that-just-says-button-clicked-how-can-i-break-it-down-into-more-detailed-nodes"></a>Bazı düğümler Uygulamam bir görselleştirmede çok üst düzey. Örneğin, yalnızca "Düğmesine tıklama" ifadesini içeren bir düğüm Nasıl ben bunu ayrıntılı düğümlere ayırabilirsiniz?

Kullanım **bölme ölçütü** seçeneklerini **Düzenle** menüsü:

1. İçinde bölmek istediğiniz olayı seçin **olay** menüsü.
2. Bir boyut seçin **boyut** menüsü. Örneğin, "Düğmesine tıkladı" adlı bir olay varsa, "Düğme adı" adlı bir özel özellik deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıma genel bakış](usage-overview.md)
* [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
* [Bekletme](usage-retention.md)
* [Uygulamanıza özel olaylar ekleme](../../azure-monitor/app/api-custom-events-metrics.md)