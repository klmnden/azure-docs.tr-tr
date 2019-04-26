---
title: Azure Application Insights kullanım etkisi | Microsoft docs
description: Farklı özellikleri potansiyel olarak analiz dönüştürme oranlarını uygulamalarınızı bölümlerinin etkiler.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/08/2019
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 8efab173f464b67c0c88c87ee28ea7fa19980501
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60373241"
---
# <a name="impact-analysis-with-application-insights"></a>Application Insights ile Etkisi Analizi

Etkisi, nasıl yükleme süreleri ve diğer özellikleri dönüştürme oranlarını uygulamanızın çeşitli bölümleri için etkilemek analiz eder. Daha hassas şekilde yerleştirmek için bulduğu nasıl **herhangi bir boyuta** , bir **sayfa görünümü**, **özel olay**, veya **isteği** kullanımını etkileyen bir farklı **sayfa görünümü** veya **özel olay**. 

![Etkisi aracı](./media/usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Hala emin değil etki yapar?

Bağımsız değişkenleri biriyle takımınızdaki kullanıcıların etmelerini olmadığını yavaşlık sitenizin bazı yönlerini içinde nasıl etkilediğini hakkında sonlandırma için ultimate araç etkisini düşünün yollarından biri gibidir. Kullanıcıların belirli bir miktarda yavaşlık tolere, ancak etkisi, iyileştirme ve kullanıcı dönüştürme en üst düzeye çıkarmak için performans dengelemek en iyi nasıl Öngörüler sunar.

Ancak, performans çözümleme yalnızca etkisi'nın özellikleri alt kümesidir. Etkisi özel olaylar ve boyutları desteklediğinden, kullanıcının tarayıcı seçim dönüştürme farklı oranlarda nasıl bağıntılı mı gibi soru yanıtlama yalnızca birkaç tıklamayla ulaşabiliyoruz ' dir.

![Tarayıcı ekran dönüştürme](./media/usage-impact/0004-browsers.png)

> [!NOTE]
> Application Insights kaynağınıza, sayfa görünümleri veya etkisi aracı kullanmak için özel olaylar içermelidir. [Sayfa görüntülemeleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](../../azure-monitor/app/javascript.md). Ayrıca bu yana bağıntı, analiz, örnek boyutu etkilenebileceğini önemlidir.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Sayfa yükleme süresi, kaç kişinin sayfamda dönüştürme etkiliyor?

Etkisi aracıyla soruyu yanıtlayarak başlamak için bir ilk sayfa görüntülemesi, özel olay veya istek'i seçin.

![Etkisi aracı](./media/usage-impact/0002-dropdown.png)

1. Bir sayfa görünümünden seçim **sayfa görünümü için** açılır.
2. Bırakın **analiz nasıl kendi** varsayılan seçimi açılan **süresi** (Bu bağlamda **süresi** için bir diğer addır **sayfa yükleme süresi**.)
3. İçin **kullanımını etkiler** açılır listesinde, özel bir olay seçin. Bu olay, 1. adımda seçtiğiniz sayfa görünümü kullanıcı Arabirimi öğesinde karşılık gelmelidir.

![Sonuç görüntüsü](./media/usage-impact/0003-results.png)

Bu örnekte **ürün sayfası** yükleme süresi için dönüştürme oranını artırır **satın alma tıklandığında ürün** arıza. Yukarıdaki dağıtım bağlı olarak, bir en iyi sayfa yükleme süresi 3.5 saniye olası bir %55 dönüştürme oranı elde etmek için hedeflenecek. 3.5 saniye aşağıda yükleme süresini azaltmak için daha fazla performans geliştirmeleri ile ek dönüştürme avantajları şu anda bağıntılı mı.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Ne sayfa görünümleri izleme veya özel yollarla yükleme süreleri?

Etkisi, standart ve özel özellikler ve ölçümler destekler. İstediğiniz kullanın. Süre yerine daha belirgin almak için birincil ve ikincil olayları filtrelerini kullanın.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Kullanıcılar farklı ülke veya bölge farklı hızlarda dönüştürme?

1. Bir sayfa görünümünden seçim **sayfa görünümü için** açılır.
2. "Ülke veya bölge" seçin **analiz nasıl kendi** açılan kutusu
3. İçin **kullanımını etkiler** sayfa görünümü kullanıcı Arabirimi öğesine karşılık gelen bir özel olay, 1. adımda seçtiğiniz açılır penceresinde seçin.

Bu durumda, sonuçları, artık ilk örnekte olduğu gibi bir sürekli x ekseni modele uyar. Bunun yerine, bir görselleştirme için bölümlenmiş bir huniye benzer sunulur. Sıralama ölçütü **kullanım** ülkeye göre özel olay dönüştürme çeşitlemesi görüntülemek için.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Bu dönüşüm oranlarını etkisi Aracı'nı nasıl hesaplar?

Etkisi aracı dayanan altyapı öğeleri, [Pearson bağıntı katsayısı](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Sonuçları -1 ve negatif bir doğrusal bağıntı ve pozitif doğrusal bir bağıntı gösteren 1 -1 ile 1 arasında hesaplanır.

Etki analizi nasıl çalıştığına ilişkin temel dökümü aşağıdaki gibidir:

İzin _A_ ana sayfa görünümü/özel olay/ilk açılan menüde seçtiğiniz istek =. (**Sayfa görünümü için**).

İzin _B_ seçtiğiniz ikincil sayfa görünümü/özel olay = (**kullanımını etkiler**).

Etkisi, kullanıcıların seçili zaman aralığındaki tüm oturumlara örneğe arar. Her oturum için her örneği için görünüyor _A_.

Oturumları sonra iki farklı tür bozuk, _subsessions_ iki koşul birini temel alan:

- İle biten bir oturumun dönüştürülmüş ilişkilendirmesinde alt oturumu oluşan bir _B_ olay ve tümünü kapsayan _A_ öncesinde meydana gelen olayları _B_.
- Bir Dönüştürülmeyen ilişkilendirmesinde alt oturumu gerçekleşir, tüm _A_kullanıcının ortaya bir terminal _B_.

Etkisi sonuçta nasıl hesaplandığını olup olmadığını biz boyut veya ölçü tarafından analiz göre değişir. Tüm ölçümler için _A_kullanıcının ilişkilendirmesinde alt oturumu içinde ortalaması alınır. Ancak her değeri boyutları için _bir_ katkıda _1/N_ için atanan değer _B_ burada _N_ sayısıdır_A_kullanıcının ilişkilendirmesinde alt oturumu içinde.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](usage-retention.md)
    - [Kullanıcı Akışları](usage-flows.md)
    - [Çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](usage-send-user-context.md)