---
title: "Azure uygulama Öngörüler kullanım etkisi | Microsoft docs"
description: "Farklı özellikleri potansiyel olarak analiz dönüştürme oranları uygulamalarınızı bölümlerinin etkileyebilir."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/25/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: d76db02647ce878343f60fc84cf063c5b7833438
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="impact-analysis-with-application-insights"></a>Application Insights ile etki analizi

Yükleme süreleri ve diğer özellikleri, uygulamanızın çeşitli bölümlerini dönüştürme oranları nasıl etkileyeceğini etkisi çözümler. Daha hassas bir şekilde koymak için bulduğu nasıl **herhangi bir boyuta** , bir **sayfa görünümü**, **özel olay**, veya **isteği** kullanımını etkileyen bir farklı **sayfa görünümü** veya **özel olay**. 

![Etkisi aracı](./media/app-insights-usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Etkisi yaptığı emin hala değil mi?

Etkisini düşünmek için bir bağımsız değişken birisiyle kullanıcılar etmelerini olup olmadığını nasıl sitenizin bazı yönlerinin yavaşlığı etkileyen hakkında takımınızdaki şekilde ultimate aracı olarak yoludur. Kullanıcıların belirli bir miktarda yavaşlığı tolerans, ancak etkisi en iyi duruma getirme ve kullanıcı dönüştürme en üst düzeye çıkarmak için performans dengelemek en iyi nasıl anlayış sağlar.

Ancak performansını analiz etme yalnızca etkisi'nin özellikleri alt kümesidir. Etkisi özel olayları ve boyutları desteklediğinden, kullanıcı tarayıcı seçim farklı oranlarda dönüştürme nasıl bağıntılı gibi yanıtlama sorular yalnızca birkaç tıklama vardır.

![Tarayıcılar ekran dönüştürme](./media/app-insights-usage-impact/0004-browsers.png)

> [!NOTE]
> Application Insights kaynağınıza, sayfa görünümleri veya etkisi aracı kullanmak için özel olaylar içermesi gerekir. [Sayfa görünümleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](app-insights-javascript.md). Ayrıca bağıntı, çözümleme beri boyutu örnek unutmayın önemlidir.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Sayfa yükleme süresini kaç kişinin sayfamda Dönüştür etkileyen?

Etkisi aracıyla sorulara yanıt verilmesi başlamak için ilk sayfa görünümü, özel bir olay ya da istek'i seçin.

![Etkisi aracı](./media/app-insights-usage-impact/0002-dropdown.png)

1. Bir sayfa görünüm seçin **sayfa görünümü için** açılır.
2. Bırakın **analiz nasıl kendi** varsayılan seçimini açılır **süresi** (Bu bağlamda **süresi** için diğer ad olduğu **sayfa yükleme süresi**.)
3. İçin **kullanımını etkiler** açılan listesinde, özel bir olay seçin. Bu olay, 1. adımda seçtiğiniz sayfa görünümü UI öğede karşılık gelmelidir.

![Sonuçları ekran görüntüsü](./media/app-insights-usage-impact/0003-results.png)

Bu örnekte **ürün sayfası** yükleme süresini artırır dönüştürme oranı **satın alma tıklattınız ürün** arıza. Yukarıdaki dağıtım bağlı olarak, bir en iyi sayfa yükleme süresi 3.5 saniye olası %55 dönüştürme oranı elde etmek için hedeflenen. 3.5 saniye aşağıda yükleme süresini azaltmak için daha fazla performans iyileştirmeleri şu anda ek dönüştürme yararları ile ilişkilendirmek değil.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Ne sayfa görünümleri izleme veya özel yollarla kez yük?

Etkisi, standart ve özel özellikleri ve ölçülerini destekler. İstediğiniz kullanın. Süre yerine daha belirli almak için birincil ve ikincil olaylarına filtreleri kullanın.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Farklı ülke veya bölgelerden kullanıcılardan farklı hızlarda Dönüştür?

1. Bir sayfa görünüm seçin **sayfa görünümü için** açılır.
2. "Ülke veya bölge" seçin **analiz nasıl kendi** açılır
3. İçin **kullanımını etkiler** sayfa görünümü üzerinde bir kullanıcı Arabirimi öğesine karşılık gelen özel bir olay 1. adımda seçtiğiniz açılan listesinde, seçin.

Bu durumda, ilk örnekte olduğu gibi sonuçları artık sürekli x ekseni modele uymayan. Bunun yerine, bölümlenmiş bir huni benzer bir görsel öğe sunulur. Sıralama ölçütü **kullanım** ülkeye göre kendi özel olayınız dönüştürme çeşitlemesi görüntülemek için.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Etkisi aracı bu dönüştürme oranları nasıl hesaplar?

Başlık altında [Pearson bağıntı katsayısı] üzerinde etkisi Aracı'nı kullanır (https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). -1-sıfır bağıntı ve pozitif bağıntı temsil eden 1 temsil eden 1 ile 1 arasında sonuçları hesaplanır.

Etki analizi nasıl çalıştığını temel dökümünü aşağıdaki gibidir:

Let _A_ ana sayfa görünümü/özel olay/ilk açılır listede seçtiğiniz = istek. (**Sayfa görünümü için**).

Let _B_ seçtiğiniz ikincil sayfa görünümü/özel olay = (**kullanımını etkiler**).

Etkisi tüm oturumları bir örnek: kullanıcıların seçili zaman aralığı içinde arar. Her oturum için her oluşumu için görünüyor _A_.

Oturumları sonra iki farklı tür bozuk, _subsessions_ iki koşul birini temel alan:

- İle biten bir oturumun dönüştürülen ilişkilendirmesinde alt oturumu oluşan bir _B_ olay ve tümünü kapsayan _A_ öncesinde meydana gelen olayları _B_.
- Bir Dönüştürülmeyen ilişkilendirmesinde alt oturumu oluşur, tüm _A_kullanıcının ortaya bir terminal _B_.

Etkisi sonuçta nasıl hesaplandığını olup olmadığını biz ölçüm veya boyuta göre Analiz göre değişir. Tüm ölçümünün _A_kullanıcının ilişkilendirmesinde alt oturumu içinde ortalaması alınır. Ancak her değeri boyutları için _A_ katkıda bulunan _1/N_ atanan değer için _B_ nerede _N_ sayısı_A_kullanıcının ilişkilendirmesinde alt oturumu içinde.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımı deneyimleri etkinleştirmek için göndermeye Başla [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olaylar veya sayfa görünümleri zaten gönderirseniz, kullanıcıların hizmetinizin kullanımını öğrenmek için kullanım araçları keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](app-insights-usage-retention.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Çalışma kitapları](app-insights-usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)