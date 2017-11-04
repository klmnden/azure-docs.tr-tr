---
title: "Azure Application ınsights'ta ölçümleri keşfederken | Microsoft Docs"
description: "Ölçüm Gezgini grafiklerde yorumlama ve ölçüm Gezgini dikey pencereleri özelleştirme."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: mbullwin
ms.openlocfilehash: 01b45323b74b54da157f4e9f1af783759c121be1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Application ınsights'ta ölçümleri keşfederken
Ölçümlerini [Application Insights] [ start] ölçülen değerleri ve telemetri uygulamanızdan gönderilen olaylar sayısı. Performans sorunlarını algılamak ve eğilimleri, uygulamanızın nasıl kullanıldığını içinde izleme yardımcı olur. Çok çeşitli standart ölçümleri yoktur ve kendi özel ölçümleri ve olayları de oluşturabilirsiniz.

Ölçümleri ve olay sayısını, SUM'ları, ortalama veya sayıları gibi toplanmış değerler grafiklerinde görüntülenir.

Grafikler dizi örnek aşağıda verilmiştir:

![](./media/app-insights-metrics-explorer/01-overview.png)

Application Insights portalında her yerde ölçümler grafiklerde bulun. Çoğu durumda bunlar özelleştirilebilir ve daha fazla grafik dikey penceresine ekleyebilirsiniz. Genel Bakış dikey penceresinden aracılığıyla ("Sunucuları" gibi başlıkları olan) daha ayrıntılı grafiklere veya tıklatın **ölçüm Gezgini** özel grafikler oluşturduğunuz yeni bir dikey penceresini açın.

## <a name="time-range"></a>Zaman aralığı
Grafikler veya tüm dikey kılavuzları kapsadığı zaman aralığını değiştirebilirsiniz.

![Azure portalında, uygulamanızın genel bakış dikey penceresini açın](./media/app-insights-metrics-explorer/03-range.png)

Henüz görünen kurmadı bazı veriler bekliyorsunuz, Yenile'yi tıklatın. Grafikler kendilerini aralıklarla yenileme ancak aralıkları büyük zaman aralıkları için daha uzun. Bir grafik üzerine analiz ardışık düzeninden gelen veriler için biraz zaman alabilir.

Bir grafik parçasına yakınlaştırma üzerine sürükleyin:

![Bir grafik parçası arasında sürükleyin.](./media/app-insights-metrics-explorer/12-drag.png)

Geri yüklemek için geri Yakınlaştır düğmesini tıklatın.

## <a name="granularity-and-point-values"></a>Ayrıntı düzeyi ve noktası değerleri
Fare bu noktada ölçümleri değerlerini görüntülemek için grafik üzerine gelin.

![Fare grafiğinin üzerine getirin](./media/app-insights-metrics-explorer/02-focus.png)

Belirli bir noktada ölçüm değerini, önceki örnekleme aralığı içinde toplanır.

Örnekleme aralığı veya "ayrıntı düzeyi" dikey pencerenin en üstünde gösterilir.

![Bir dikey pencere üstbilgisi.](./media/app-insights-metrics-explorer/11-grain.png)

Zaman aralığı dikey ayrıntı düzeyi ayarlayabilirsiniz:

![Bir dikey pencere üstbilgisi.](./media/app-insights-metrics-explorer/grain.png)

Kullanılabilir ayrıntı düzeyi zaman aralığına bağlıdır. Açık ayrıntı düzeyi "Otomatik" ayrıntı zaman aralığına ilişkin alternatifleri ' dir.


## <a name="editing-charts-and-grids"></a>Grafikler ve Izgaralar düzenleme
Yeni bir grafik dikey penceresine eklemek için:

![Ölçümleri Explorer'da eklemek grafik seçin](./media/app-insights-metrics-explorer/04-add.png)

Seçin **Düzenle** gösterdiklerini düzenlemek için mevcut veya yeni bir grafik:

![Bir veya daha çok ölçümü seçin](./media/app-insights-metrics-explorer/08-select.png)

Kısıtlamaları birlikte görüntülenebilir birleşimleri hakkında olsa grafikte, birden fazla ölçüm görüntüleyebilirsiniz. Bir ölçüm seçtiğiniz hemen bazı diğer devre dışı.

Kodlu [özel ölçümleri] [ track] uygulamanıza (TrackMetric ve TrackEvent çağrıları) bunlar burada listelenir.

## <a name="segment-your-data"></a>Verilerinizi segmentlere
Bir ölçüm özelliği tarafından - Örneğin, sayfa görünümleri farklı işletim sistemleri ile istemcilerde Karşılaştırılacak bölebilirsiniz.

Bir grafik veya kılavuz, gruplandırılması geçin ve göre gruplamak için bir özellik seçin:

![Gruplandırma'kümesi seçip bir özellik içinde Group By'ı seçin](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Gruplandırma kullandığınızda, alan ve çubuk grafik türleri Yığılmış bir görünüm sağlar. Toplama yöntemi toplam olduğu bu uygundur. Ancak, toplama türü ortalama bulunduğu çizgi veya kılavuz görüntü türlerini seçin.
>
>

Kodlu [özel ölçümleri] [ track] uygulamanıza ve özellik değerlerini içerir, özellik listesinde seçmek kullanabileceksiniz.

Grafik bölümlenmiş veri için çok küçük mi? Kendi yüksekliğini ayarlayın:

![Kaydırıcı Ayarla](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Toplama türleri
Gösterge varsayılan tarafında genellikle grafik dönemi boyunca toplanan değerini gösterir. Grafik üzerine getirirseniz, o noktada değerini gösterir.

Her veri noktası grafiği bir toplama önceki örnekleme aralığında veya "ayrıntı düzeyi" alınan veri değerlerinin ' dir. Ayrıntı düzeyi dikey pencerenin üst kısmında gösterilen ve grafik genel ölçeğini ile değişir.

Ölçümleri farklı şekilde toplanabilir:

* **Count** örnekleme aralığında alınan olayların sayısı. İstekleri gibi olaylar için kullanılır. Grafiğin yüksekliği Çeşitlemeler olaylar meydana geldiği oran Çeşitlemeler gösterir. Ancak, örnekleme aralığı değiştirdiğinizde sayısal değer değiştiğine dikkat edin.
* **Sum** örnekleme aralığı ya da grafik süre alınan tüm veri noktalarının değerlerini ekler.
* **Ortalama** aralığı içinde alınan veri noktası sayısı toplam böler.
* **Benzersiz** sayıları, kullanıcıları ve hesaplarını sayısı için kullanılır. Örnekleme aralığı içinde ya da grafik süre boyunca şekil bu süre içinde görülen farklı kullanıcı sayısını gösterir.
* **%**-Her bir toplama yüzdesi sürümleri yalnızca bölümlenmiş grafiklerle kullanılır. Toplam her zaman % 100 ekler ve grafik toplam'in farklı bileşenleri göreli payını gösterir.

    ![Yüzdesi toplama](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Toplama türünü değiştirme

![Grafik düzenleyin ve ardından toplama seçin](./media/app-insights-metrics-explorer/05-aggregation.png)

Her ölçümü için varsayılan yöntemi yeni bir grafik oluşturduğunuzda veya olduğunda tüm ölçüm seçili gösterilir:

![Varsayılan değerleri görmek için tüm ölçümleri seçimini kaldırın](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>PIN y ekseni 
Varsayılan olarak bir grafik sıfır değerleri Zamanlayıcının görsel gösterimi vermek için veri aralığında en yüksek değerleri kasa başlayarak Y ekseni değerleri gösterir. Ancak bazı durumlarda Zamanlayıcının'den fazla küçük değişiklikler değerleri görsel olarak incelemek ilginç olabilir. Y ekseni bu gibi özelleştirmeler y ekseni minimum veya maksimum değeri istediğiniz yerde sabitlemek için düzenleme özelliği aralık.
Y ekseni aralığını ayarları getirmek için "Gelişmiş" onay kutusunu tıklayın

![Gelişmiş Ayarlar'ı tıklatın, özel aralık seçin ve min en yüksek değerleri belirtin](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Verilerinizi filtre
Yalnızca seçili bir özellik değeri Seti ölçümlerini görmek için:

![Filtre'yi tıklatın, bir özelliği'ni genişletin ve bazı değerleri kontrol edin](./media/app-insights-metrics-explorer/19-filter.png)

Belirli bir özellik için herhangi bir değer seçmezseniz, tümünü seçme ile aynı olduğu: söz konusu özellik üzerinde hiçbir filtre yoktur.

Her özellik değerini yanında olayların sayısına dikkat edin. Bir özellik değerini seçtiğinizde, diğer özellik değerleri yanında sayıları ayarlanır.

Bir dikey penceresinde tüm grafikler için filtre uygulayın. Farklı istiyorsanız, farklı grafiklere uygulanan filtreler oluşturun ve farklı ölçümleri Kanatlar kaydedin. İsterseniz, böylece bunları diğer gördüğünüz farklı Kanatlar grafiklere Pano sabitleyebilirsiniz.

### <a name="remove-bot-and-web-test-traffic"></a>Bot ve web testi trafiği Kaldır
Filtreyi kullanmak **gerçek veya yapay trafiği** ve denetleme **gerçek**.

Göre filtre uygulayabilirsiniz **yapay trafik kaynak**.

### <a name="to-add-properties-to-the-filter-list"></a>Özellikler filtre listesine eklemek için
Telemetri kendi seçme kategorisine filtre ister misiniz? Örneğin, kullanıcılarınız farklı kategorilere yukarı bölmek olabilir ve verilerinizi kategorilerine göre segmentlere ayırmak istiyor musunuz.

[Kendi özellik oluşturmak](app-insights-api-custom-events-metrics.md#properties). Bunu kümesinde bir [Telemetri Başlatıcı](app-insights-api-custom-events-metrics.md#defaults) farklı SDK modülleri tarafından gönderilen standart telemetriyi de dahil olmak üzere tüm telemetri - görünmesini sağlamak için.

## <a name="edit-the-chart-type"></a>Grafik türünü Düzenle
Kılavuzları ve grafiklerinizi arasında geçiş yapabilirsiniz dikkat edin:

![Kılavuz veya grafiği seçin, sonra bir grafik türü seçin](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Ölçümleri dikey pencerenizin Kaydet
Bazı grafiklerde oluşturduğunuz zaman, sık kullanılan olarak kaydedebilirsiniz. Kurumsal bir hesap kullanırsanız, diğer ekip üyeleri ile paylaşmak seçebilirsiniz.

![Sık kullanılan seçin](./media/app-insights-metrics-explorer/21-favorite-save.png)

Dikey yeniden görmek için **genel bakış dikey penceresine gidin** ve Sık Kullanılanlar açın:

![Genel Bakış dikey penceresinde, Sık Kullanılanlar seçin](./media/app-insights-metrics-explorer/22-favorite-get.png)

Kaydettiğinizde göreli zaman aralığını seçerseniz, dikey pencerenin en son Ölçümleriyle güncelleştirilir. Mutlak zaman aralığını seçerseniz, her zaman aynı veri gösterir.

## <a name="reset-the-blade"></a>Dikey Sıfırla
Yalnızca bir dikey pencerede düzenleyebilir, ancak daha sonra geri kümesi kaydedildi özgün almak istersiniz, Sıfırla'yı tıklatın.

![Ölçüm Gezgini üstündeki düğmeleri](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Canlı ölçümleri akış

Telemetrinizi hakkında daha fazla anlık görünümü için açık [canlı akış](app-insights-live-stream.md). Çoğu ölçümleri toplama işlemi nedeniyle görünmesi birkaç dakika sürebilir. Bunun aksine, Canlı ölçümleri düşük gecikme süresi için en iyi duruma getirilir. 

## <a name="set-alerts"></a>Uyarı ayarlama
Alışılmadık değerlerin herhangi bir ölçümü, e-postayla bildirilmesini bir uyarı ekleyin. Hesap Yöneticileri veya belirli e-posta adresleri e-posta göndermek için ya da seçebilirsiniz.

![Uyarı kuralları, ekleme uyarı ölçümleri Explorer'da seçin](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Uyarılar hakkında daha fazla bilgi][alerts].


## <a name="continuous-export"></a>Sürekli Dışarı Aktarma
Böylece dışarıdan işleyebilmesi için sürekli olarak verilen verileri istiyorsanız kullanmayı [sürekli verme](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Verilerinizin daha zengin görünümlerini isterseniz [dışarı aktarmak için Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analiz
[Analytics](app-insights-analytics.md) güçlü sorgu dili kullanarak telemetrinizi çözümlemek için daha ayrıntılı bir yoludur. Birleştirmek ölçümleri sonuçlarından işlem veya ayrıntılı incelenmesi uygulamanızın son performansını gerçekleştirmek istiyorsanız, bunu kullanın. 

Bir ölçüm grafikten eşdeğer Analytics sorgu doğrudan almak için Analytics simgeye tıklayın.

## <a name="troubleshooting"></a>Sorun giderme
*Herhangi bir veri my grafikte bakın yok.*

* Dikey penceresinde tüm grafikler için filtre uygulayın. Bir grafik odaklanan olsa da, diğer tüm veriler dışlar bir filtre ayarlanmış alamadık, emin olun.

    Farklı grafiklerin farklı filtreler ayarlamak istiyorsanız içinde farklı dikey pencereler oluşturma gibi ayrı Sık Kullanılanlar kaydedin. İsterseniz, böylece bunları diğer görebilirsiniz bunları panoya sabitleyebilirsiniz.
* Ölçüm üzerinde tanımlı değil bir özelliği bir grafik grupla, ardından olacaktır hiçbir şey grafik. 'Group by' temizlemeyi deneyin veya farklı gruplandırma özelliği seçin.
* Performans verilerini (CPU, g/ç hızı vb.) Java web Hizmetleri, Windows Masaüstü uygulamaları, kullanılabilir [IIS, uygulama ve hizmetlere Durum İzleyicisi yüklerseniz web](app-insights-monitor-performance-live-website-now.md), ve [Azure Cloud Services](app-insights-azure.md). Azure Web siteleri için kullanılabilir değil.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Application Insights ile kullanım izleme](app-insights-web-track-usage.md)
* [Tanılama aramayı kullanma](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
