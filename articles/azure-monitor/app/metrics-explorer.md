---
title: Azure Application ınsights ölçümleri keşfederken | Microsoft Docs
description: Ölçüm Gezgini grafikleri yorumlama ve ölçüm Gezgini dikey pencereleri özelleştirme.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: mbullwin
ms.openlocfilehash: 5c659ca2f40d47450227d16963499a6b27c9e313
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60700896"
---
# <a name="exploring-metrics-in-application-insights"></a>Application ınsights'ta ölçümleri keşfederken
Ölçümlerde [Application Insights] [ start] ölçülen değerleri ve telemetriyi uygulamanızdan gönderilen olayların sayısı. Performans sorunları tespit edin ve eğilimler, uygulamanızın nasıl kullanıldığını izleyin yardımcı olur. Çok çeşitli standart ölçüm vardır ve kendi özel Ölçümler ve olaylar da oluşturabilirsiniz.

> [!NOTE]
> Bu makalede, şu anda kullanım dışıdır ve sonunda kullanımdan kaldırılacak Klasik ölçüm Gezgini deneyimi açıklanır. Açıklanan yeni deneyimini kullanıma öneririz [bu makalede](../platform/metrics-charts.md).

Ölçüm ve olay sayılarını, toplanan değerlere toplamlar, ortalamalar veya sayısı gibi grafiklerdeki görüntülenir.

Örnek bir grafik aşağıda verilmiştir:

![](./media/metrics-explorer/01-overview.png)

Her yerde ölçüm grafikleri Application Insights portalında bulabilirsiniz. Çoğu durumda, özelleştirilebilir ve dikey pencereyi daha fazla grafikleri ekleyebilirsiniz. Genel Bakış dikey penceresinden aracılığıyla ("Sunucuları" gibi başlıkları olan) daha ayrıntılı grafiklere veya tıklatın **ölçüm Gezgini** grafiklerini oluşturabileceğiniz yeni bir dikey penceresini açın.

## <a name="time-range"></a>Zaman aralığı
Grafikleri ya da tüm dikey penceresinde kılavuzlarda kapsadığı zaman aralığını değiştirebilirsiniz.

![Genel Bakış dikey penceresinde, uygulamanızın Azure portalında açın](./media/metrics-explorer/03-range.png)

Henüz görünen henüz bazı veri bekliyorsanız, Yenile'ye tıklayın. Grafikler kendilerini aralıklarla yeniler, ancak aralıklar daha büyük zaman aralıkları için daha uzun. Bu, bir grafik üzerine analiz işlem hattı üzerinden gelen veriler için biraz zaman alabilir.

Grafik bir parçası yakınlaştırmak için üzerine sürükleyin:

![Bir grafik parçası sürükleyin.](./media/metrics-explorer/12-drag.png)

Geri yüklemek için yakınlaştırma geri düğmesini tıklatın.

## <a name="granularity-and-point-values"></a>Ayrıntı düzeyi ve nokta değerleri
Fare bu noktada ölçümlerinin değerlerini görüntülemek için grafik üzerine gelin.

![Fare bir grafik üzerine gelin.](./media/metrics-explorer/02-focus.png)

Belirli bir noktada ölçüm değerini, önceki örnekleme aralığı içinde toplanır.

Örnekleme aralığı veya "ayrıntı düzeyi" dikey pencerenin en üstünde gösterilir.

![Bir dikey pencere üst bilgisine.](./media/metrics-explorer/11-grain.png)

Ayrıntı düzeyi zaman aralığını dikey penceresinde ayarlayabilirsiniz:

![Bir dikey pencere üst bilgisine.](./media/metrics-explorer/grain.png)

Kullanılabilir ayrıntı düzeyi seçtiğiniz zaman aralığına bağlıdır. Açık ayrıntı düzeylerinde alternatifleri zaman aralığı için "Otomatik" ayrıntı düzeyi var.


## <a name="editing-charts-and-grids"></a>Düzenleme grafikleri ve Kılavuzlar
Dikey penceresine yeni bir grafik eklemek için:

![Ölçüm Gezgini'nde grafiği ekleme seçin](./media/metrics-explorer/04-add.png)

Seçin **Düzenle** gösterdiklerini düzenlemek için mevcut veya yeni bir grafik:

![Bir veya daha fazla ölçüm seçin](./media/metrics-explorer/08-select.png)

Kısıtlamaları birlikte görüntülenebilir birleşimler hakkında olsa, birden fazla ölçüm bir grafikte görüntüleyebilirsiniz. Bir ölçüm seçin hemen sonra başkalarının devre dışı bırakılmıştır.

Varsa, kodlanmış [özel ölçümler] [ track] uygulamanıza (TrackMetric ve TrackEvent çağrıları), burada listelenir.

## <a name="segment-your-data"></a>Verilerinizi segmentlere ayırın.
Size bir ölçüm özelliğiyle - Örneğin, sayfa görüntülemeleri istemcilerde farklı işletim sistemleri ile Karşılaştırılacak bölebilirsiniz.

Grafiğiniz veya kılavuzunuz seçin, gruplandırma'yı geçin ve gruplamak için bir özellik seçin:

![Gruplandırma'Group By içinde bir özellik kümesi seçin sonra'ı seçin](./media/metrics-explorer/15-segment.png)

> [!NOTE]
> Gruplandırma kullandığınızda, alan ve çubuk grafik türleri Yığılmış bir görünüm sağlar. Toplama yöntemi toplamı olduğu bu uygundur. Ancak, toplama türü ortalama bulunduğu satırı veya geniş görüntü türlerini seçin.
>
>

Varsa, kodlanmış [özel ölçümler] [ track] uygulamanıza ve özellik değerlerini içerirler, özellik listesinde seçmek mümkün olacaktır.

Grafik parçalı veriler için çok küçük mi? Yükseklik ayarlayın:

![Kaydırıcıyı ayarladığınız](./media/metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Toplama türleri
Gösterge tarafındaki varsayılan olarak, genellikle grafik dönem boyunca toplu değere gösterir. Grafik üzerine gelin, bu noktada değerini gösterir.

Her veri noktası Grafiği'nde yer alan önceki örnekleme aralığı ya da "ayrıntı düzeyi" alınan veri değerlerini toplama ' dir. Ayrıntı düzeyi, dikey pencerenin en üstünde gösterilen ve grafik genel zaman ile ölçeği değişir.

Ölçümleri farklı şekilde toplanabilir:

* **Sayısı** örnekleme aralığı içinde alınan olayların sayısı. İstekleri gibi olayları için kullanılır. Grafik yüksekliği çeşitleri, olaylar meydana geldiği oran farklılığı gösterir. Ancak, örnekleme aralığı değiştirdiğinizde, sayısal değer değiştiğine dikkat edin.
* **Sum** örnekleme aralığı veya grafiğin dönemi üzerinden alınan tüm veri noktaları değerlerini toplar.
* **Ortalama** aralığı içinde alınan veri noktası sayısı toplamı böler.
* **Benzersiz** sayıları sayar, kullanıcılar ve hesaplar için kullanılır. Örnekleme aralığı üzerinden ya da grafik dönemdeki şekilde bu süre içinde görülen farklı kullanıcı sayısı gösterilmektedir.
* **%** -Her bir toplama yüzdesi sürümleri yalnızca segmentli grafiklerle kullanılır. Toplam % 100 her zaman ekler ve toplam farklı bileşenlerin göreli katkı grafik gösterir.

    ![Yüzde toplama](./media/metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Toplama türünü değiştirebilirsiniz

![Grafiği düzenlemek ve toplama seçin](./media/metrics-explorer/05-aggregation.png)

Her ölçümü için varsayılan yöntemi yeni bir grafik oluşturduğunuzda veya tüm ölçümleri ne zaman seçimi gösterilmektedir:

![Varsayılan değerleri görmek için tüm ölçümlerin seçimini kaldırın](./media/metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>PIN y ekseni 
Varsayılan olarak bir grafik veri aralıktaki değerleri kuantum görsel bir temsilini sağlamak için en yüksek değerleri kasa sıfırdan başlayan Y ekseni değer gösterilir. Ancak bazı durumlarda kuantum fazlasını görsel olarak küçük değişiklikler değerlerini incelemek ilginç olabilir. Y ekseni bu gibi özelleştirmeler için y ekseni minimum veya maksimum değeri istediğiniz yerde sabitlemek için düzenleme özelliği aralık.
Y ekseni aralığı ayarları getirmek için "Gelişmiş ayarları" onay kutusunu tıklayın

![Gelişmiş Ayarlar'ı tıklatın, özel aralık seçin ve min max değerleri belirtin](./media/metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Verilerinizi Filtrele
Seçili özellik değerleri kümesi için yalnızca ölçümleri görmek için:

![Filtreye, bir özelliği'ni genişletin ve bazı değerleri kontrol edin](./media/metrics-explorer/19-filter.png)

Belirli bir özellik için herhangi bir değer seçmezseniz, tümünü seçme ile aynı olduğu: söz konusu özellik üzerinde filtre yoktur.

Her özellik değerini yanı sıra olay sayısını dikkat edin. Bir özellik değerlerini seçtiğinizde, diğer özellik değerlerini yanı sıra sayıları ayarlanır.

Bir dikey pencere tüm grafikler için filtre uygulayın. Farklı istiyorsanız farklı grafikler için uygulanan filtreler oluşturun ve farklı ölçümleri dikey pencereleri kaydedin. İsterseniz, bunları birbirine yanı sıra görebilirsiniz, böylece farklı dikey pencereleri bir grafikten panoya sabitleyebilirsiniz.

### <a name="remove-bot-and-web-test-traffic"></a>Trafik için robot ve web testi Kaldır
Filtreyi kullanın **gerçek veya yapay trafik** ve **gerçek**.

Göre de filtre uygulayabilirsiniz **yapay trafik kaynağı**.

### <a name="to-add-properties-to-the-filter-list"></a>Özellikler filtre listesine eklemek için
Kendi seçtiğiniz bir kategorinin telemetri filtreleme ister misiniz? Örneğin, belki de, kullanıcılar farklı kategorilere ayırmak ve verilerinizi kategorilerine göre segmentlere ayırmak istediğiniz.

[Kendi özellik Oluştur](../../azure-monitor/app/api-custom-events-metrics.md#properties). Ayarlamanız bir [Telemetri başlatıcısını](../../azure-monitor/app/api-custom-events-metrics.md#defaults) farklı SDK modülleri tarafından gönderilen standart telemetri dahil olmak üzere tüm telemetriyi - görünmesini sağlamak için.

## <a name="edit-the-chart-type"></a>Grafik türünü Düzenle
Graflar ve Kılavuzlar arasında geçiş yapabilirsiniz dikkat edin:

![Kılavuz veya graph'ı seçin ve ardından bir grafik türü seçin](./media/metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Ölçümler dikey pencerenize Kaydet
Bazı grafikler oluşturdunuz, sık kullanılan olarak kaydedebilirsiniz. Bir kuruluş hesabı kullanıyorsanız, diğer ekip üyeleriyle paylaşmak seçebilirsiniz.

![Sık kullanılan seçin](./media/metrics-explorer/21-favorite-save.png)

Dikey pencereyi yeniden görmek için **genel bakış dikey penceresine gidin** ve Sık Kullanılanlar açın:

![Sık Kullanılanlar genel bakış dikey penceresinde](./media/metrics-explorer/22-favorite-get.png)

Kaydettiğinizde göreli zaman aralığı seçtiyseniz, dikey pencerenin en son ölçümleri ile güncelleştirilecektir. Mutlak zaman aralığı seçtiyseniz, bu her zaman aynı verileri gösterir.

## <a name="reset-the-blade"></a>Sıfırlama dikey penceresi
Yalnızca bir dikey pencere düzenleyebilir, ancak ardından kümesi kaydedildi özgün geri almak istiyor musunuz, Sıfırla'ya tıklayın.

![Ölçüm Gezgini'nde üstündeki düğmeleri](./media/metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Canlı ölçüm akışı

Telemetrinizin çok daha yakın bir görünüm için açık [Canlı Stream](live-stream.md). Çoğu ölçümün, toplama işlemi nedeniyle görünmesi birkaç dakika sürebilir. Aksine, Canlı ölçümleri, düşük gecikme süresi için iyileştirilmiştir. 

## <a name="set-alerts"></a>Uyarı ayarlama
Ölçümlerden herhangi birinin alışılmadık değerlerin e-postayla gönderilecek bir uyarı ekleyin. Hesap Yöneticileri veya belirli bir e-posta adreslerine e-posta göndermek ya da seçebilirsiniz.

![Ölçüm Gezgini'nde uyarı kuralları, uyarı Ekle öğesini seçin.](./media/metrics-explorer/appinsights-413setMetricAlert.png)

[Uyarılar hakkında daha fazla bilgi][alerts].


## <a name="continuous-export"></a>Sürekli Dışarı Aktarma
Harici olarak işleyebilir, böylece sürekli olarak dışarı aktarılan verileri istiyorsanız kullanmayı [sürekli dışarı aktarma](../../azure-monitor/app/export-telemetry.md).

### <a name="power-bi"></a>Power BI
Verilerinizi daha kapsamlı görünümlerini istiyorsanız [dışarı aktarmak için Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analiz
[Analytics](../../azure-monitor/app/analytics.md) güçlü bir sorgu dili kullanarak telemetrinizi analiz etmek için daha çok yönlü bir yoludur. Birleştirme veya ölçümleri sonuçlarını işlem ya da uygulamanızın en son performans ayrıntılı inceleme yapmak istiyorsanız, bunu kullanın. 

Bir ölçüm grafiği doğrudan eşdeğer Analytics sorgusu almak için Analytics simgesine tıklayabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
*Mychart üzerinde herhangi bir veri görmüyorum.*

* Dikey penceresinde tüm grafikler için filtre uygulayın. Bir grafikte odaklandığınız olsa da başka bir tüm verileri bırakan bir filtre ayarlanmış alamadık, emin olun.

    Farklı filtreler farklı grafikler üzerinde ayarlamak istiyorsanız bunları farklı dikey pencerelerinde oluşturma gibi ayrı sık kaydedin. İsterseniz, bunları birbirine yanı sıra görebilirsiniz, böylece bunları panoya sabitleyebilirsiniz.
* Ölçüme göre tanımlanmamış özelliği bir grafiği gruplarsanız, ardından olacaktır hiçbir şey grafiği. 'Gruplandırma ölçütü' temizlemeyi deneyin veya farklı gruplandırma özelliğini seçin.
* Performans verileri (CPU, GÇ oranı vb.) Java web Hizmetleri, Windows Masaüstü uygulamaları için kullanılabilir [Durum İzleyicisi'ni yükleyin, web uygulamaları ve Hizmetleri'IIS](../../azure-monitor/app/monitor-performance-live-website-now.md), ve [Azure Cloud Services](../../azure-monitor/app/app-insights-overview.md). Azure Web siteleri için kullanılabilir değildir.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Application Insights ile kullanımı izleme](../../azure-monitor/app/usage-overview.md)
* [Tanılama aramayı kullanma](../../azure-monitor/app/diagnostic-search.md)

<!--Link references-->

[alerts]: ../../azure-monitor/app/alerts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
