<properties
    pageTitle="Visual Studio Eğilimlerini Çözümleme | Microsoft Azure"
    description="Visual Studio Application Insights telemetri eğilimlerini çözümleyin, görselleştirin ve keşfedin."
    services="application-insights"
    documentationCenter=".net"
    authors="numberbycolors"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="daviste"/>

# Visual Studio eğilimlerini çözümleme

Application Insights Eğilimleri aracı, uygulamanızın önemli telemetri olaylarının zaman içinde nasıl değiştiğini göstererek, sorunları ve anomalileri hızlıca belirlemenize yardımcı olur. Sizi daha ayrıntılı tanılama bilgilerine bağlayan Eğilimler, uygulamanızın performansını geliştirmenize, özel durumların nedenlerini izlemenize ve özel olaylarınıza ilişkin bilgileri açığa çıkarmanıza yardımcı olabilir.

![Örnek Eğilimler penceresi](./media/app-insights-trends/app-insights-trends-hero-750.png)

> [AZURE.NOTE] Application Insights Eğilimleri Visual Studio 2015 Güncelleştirme 3 ve sonrasında ya da [Geliştirici Analiz Araçları uzantısı](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a) sürüm 5.209 ve sonrasında mevcuttur.

## Application Insights Eğilimleri’ni açma

Application Insights Eğilimleri penceresini aşağıdaki yollardan biriyle açabilirsiniz:

* Application Insights araç çubuğu düğmesinden **Telemetri Eğilimlerini Keşfet**’i seçin.
* Proje kısayol menüsünden **Application Insights > Telemetri Eğilimlerini Keşfet**’i seçin.
* Visual Studio menü çubuğundan **Görünüm > Diğer Pencereler > Application Insights Eğilimleri**’ni seçin.

Kaynak seçmenizi isteyen bir ileti görebilirsiniz. Bu durumda, **Kaynak seç** öğesine tıklayın, bir Azure aboneliği ile oturum açın, ardından telemetri eğilimlerini çözümlemek istediğiniz bir Application Insights kaynağını seçin.

## Eğilim analizi seçme

![Eğilim analizi genel türleri menüsü](./media/app-insights-trends/app-insights-trends-1-750.png)

Her biri son 24 saatin verilerini çözümleyen beş genel eğilim analizinden birini seçerek işe başlayın:

* **Sunucu isteklerinizin performans sorunlarını denetleyin**: Hizmetinize yapılan ve yanıt sürelerine göre gruplandırılan istekler
* **Sunucu isteklerinizdeki hataları çözümleyin**: Hizmetinize yapılan ve HTTP yanıt koduna göre gruplandırılan istekler
* **Uygulamanızdaki özel durumları inceleyin**: Hizmetinizde özel durum türüne göre gruplandırılmış özel durumlar
* **Uygulama bağımlılıklarınızın performansını denetleyin**: Hizmetiniz tarafından çağrılan ve yanıt sürelerine göre gruplandırılan hizmetler
* **Özel olaylarınızı inceleyin**: Hizmetiniz için ayarladığınız ve olay türüne göre gruplandırılan özel olaylar

Önceden oluşturulmuş bu çözümlemeler Eğilimler penceresinin sol üst köşesindeki **Sık kullanılan telemetri analizi türlerini görüntüleyin** düğmesinden kullanılabilir.

## Uygulamanızdaki eğilimleri görselleştirme

Application Insights Eğilimleri, uygulamanızın telemetrisinden bir zaman dizisi görselleştirmesi oluşturur. Her bir zaman dizisi görselleştirmesi bir telemetri türünü, ilgili telemetrinin bir özelliğine göre gruplandırarak ve belirli bir zaman aralığı üzerinde gösterir.

Örneğin, oluşturuldukları ülkeye göre gruplandırılmış son 24 saatteki sunucu isteklerini görüntülemek isteyebilirsiniz. Bu örnekte görselleştirme üzerindeki her baloncuk, bir saat boyunca belirli bir ülke için yapılan sunucu isteklerinin sayısını gösterir.

Hangi telemetri türlerini görüntüleyeceğinizi ayarlamak için pencerenin üstündeki denetimleri kullanın. İlk olarak ilgilendiğiniz telemetri türlerini seçin:

* **Telemetri Türü**: Sunucu istekleri, özel durumlar, bağımlılıklar veya özel olaylar
* **Zaman Aralığı**: Son 30 dakika ile son 3 gün arasında herhangi bir zaman
* **Gruplandırma Ölçütü**: Özel durum türü, sorun kimliği, ülke/bölge ve daha fazlası

Ardından sorguyu çalıştırmak için **Telemetriyi Çözümle**’ye tıklayın.

Görseldeki baloncuklar arasında gezinmek için:

* Bir baloncuğa tıklayarak seçtiğinizde, pencerenin altındaki filtreler güncelleştirilir ve belirli bir süre içinde oluşan olaylar özetlenir.
* Arama aracında gezinmek için bir baloncuğa çift tıklayın ve ilgili süre içinde gerçekleşen tüm telemetri olaylarını tek tek görüntüleyin.
* Baloncuğun görselleştirmedeki seçimini kaldırmak için **Ctrl** tuşuna basıp tıklayın.

> [AZURE.TIP] Eğilimler ve Arama araçları, hizmetinizle ilgili sorunlara neden olan telemetri olaylarını belirlemenize yardımcı olur. Örneğin, müşterileriniz bir öğleden sonra uygulamanızın daha yavaş yanıt verdiğini fark ederse, sorunu analiz etmeye Eğilimler ile başlayabilirsiniz. Son birkaç saat içinde hizmetinize yapılan istekleri, yanıt süresine göre gruplandırılmış halde çözümleyin. Olağan dışı derecede büyük bir yavaş istekler kümesi olup olmadığına bakın. Ardından baloncuğa çift tıklayarak bu istek olaylarına göre filtrelenen Arama aracına gidin. Arama aracında bu isteklerin içeriklerini keşfedebilir ve sorunu çözmek için ilgili kodu bulabilirsiniz.

## Belirli eğilimler için filtre uygulayın

Pencerenin altındaki filtre denetimleri ile daha özel eğilimleri bulun. Bir filtre uygulamak için adına tıklayın. Telemetrinizin belirli bir boyutunda gizlenmiş olabilecek eğilimleri bulmak için farklı filtreler arasında hızlıca geçiş yapabilirsiniz. Bir boyutta Özel Durum Türü boyutu gibi bir filtre uygularsanız, diğer boyutlardaki filtreler grileştirilmiş görünse bile tıklanabilir durumda kalır. Bir filtrenin uygulamasını kaldırmak için filtreye yeniden tıklayın. Aynı boyutta birden fazla filtreyi seçmek için **Ctrl** tuşuna basıp tıklayın.

![Eğilim filtreleri](./media/app-insights-trends/TrendsFiltering-750.png)

Birden fazla filtre uygulamak isterseniz ne olur?

1. İlk filtreyi uygulayın.
2. Birinci filtrenizin yön adının yanındaki **Seçili filtreleri uygula ve yeniden sorgula** düğmesine tıklayın. Bunun yapılması, telemetrinizi özellikle birinci filtreyle eşleşen olaylar için yeniden sorgular.
3. İkinci bir filtre uygulayın.
4. Telemetrinizin belirli alt kümelerindeki eğilimleri bulmak için işlemi tekrarlayın. Örneğin, adı "GET Home/Index" olan, Almanya’dan gelen ve 500 durum kodu alan sunucu istekleri için sorgulayabilirsiniz.

Bu filtrelerden birini kaldırmak için yöne ilişkin **Seçili filtreleri kaldır ve yeniden sorgula** düğmesine tıklayın.

![Birden fazla filtre](./media/app-insights-trends/TrendsFiltering2-750.png)

## Anormallikleri bulma

Eğilimler aracı, aynı zaman dizisindeki diğer baloncuklara kıyasla anormal olan olayların baloncuklarını vurgulayabilir. **Görünüm Türü** açılan menüsünde **Zaman aralığındaki sayımlar (anomalileri vurgula)** veya **Zaman aralığındaki yüzdeler (anomalileri vurgula)** seçeneğini belirleyin. Kırmızı baloncuklar anormaldir.

Anomaliler sayıları/yüzdeleri, son iki zaman dilimi (örneğin, son 24 saati görüntülüyorsanız 48 saat) boyunca gerçekleşen sayıların/yüzdelerin standart sapmasının 2.1 katını aşan baloncuklar olarak tanımlanır.

![Renkli noktalar anomalileri gösterir](./media/app-insights-trends/TrendsAnomalies-750.png)

> [AZURE.TIP] Anomalilerin vurgulanması işlemi, özellikle diğer durumlarda benzer boyutlarda görünebilecek küçük baloncukların, zaman dizisindeki aykırı değerlerini bulmak için yararlı olabilir.  

## <a name="next"></a>Sonraki adımlar


[Visual Studio’da Application Insights ile çalışma](app-insights-visual-studio.md): Telemetri arayın, CodeLens’te verileri görün ve Application Insights’ı yapılandırın; üstelik tüm bunları Visual Studio içinde yapın.

[Daha fazla veri ekleme](app-insights-asp-net-more.md): Kullanımı, kullanılabilirliği, bağımlılıkları ve özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın.

[Application Insights portalıyla çalışma](app-insights-dashboards.md): Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma.



<!--HONumber=Aug16_HO4-->


