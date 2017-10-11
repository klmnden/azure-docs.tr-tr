---
title: "Azure Application Insights web uygulaması performans değişikliklerin tanılama akıllı | Microsoft Docs"
description: "Otomatik tanılama ani veya performans telemetrisini adımlarda web uygulamanızdan."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 5e53bc714d89bf6204681349e7890e0b8fbc7046
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a>Uygulama telemetrinizi ani değişiklikler tanılama

*Bu özelliğin önizlemede değil.*

Web uygulamanızın performansı veya tek bir tıklatmayla kullanımı ani değişiklikler tanılamak! Her zaman grafik oluşturduğunuzda akıllı tanılama özelliği kullanılabilir [Analytics](app-insights-analytics.md) içinde [Application Insights](app-insights-overview.md). Bir depo veya bir DIP gibi sonuçlarınızı eğilimi olağan dışı bir değişiklik olduğunda akıllı tanılama bir desen boyutları ve değişiklik açıklayabilir ilgili değerleri tanımlar. Bu sorunu hızlı tanılamanıza yardımcı olur. 

Bu örnekte, akıllı tanılama değişiklikle ilişkili özellik değerlerinin bir desen belirledi ve sonuçları ile ve bu deseni olmadan arasındaki farkı vurgular:

![Örnek analytics Tanılama sonucu](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a>Veri değişikliklerini tanılama

1.  Analizleri bir sorgu çalıştırın ve zaman grafik olarak işleme. 
2.  Varsa herhangi bir vurgulanan yoğun noktasını'ı tıklatın.
 
    ![yoğun noktası](./media/app-insights-analytics-diagnostics/peak.png)

    Tanılama bir desen keşfetmek için birkaç saniye sürer.

3. Tanılama sonuçları sekmesi, veri süreksizlik açıklayabilir bir desen gösterir.

    ![Sonuç](./media/app-insights-analytics-diagnostics/result.png)
 
    Metin kaydırma ile ilişkilendirmek için görüntülenen boyut değerleri gösterir. Bu örnekte, belirli bir istek ve belirli tarayıcı sürümü ile ilişkili.

    Filtre true ve false grafik de iki bileşenden dikkat edin. False bileşen değişmeden eğilimi gösterir. Diğer bir deyişle, biz tanılama belirledi sorunlu birleşimlerini bıraksanız telemetri sonuçlarında değişiklik yoktur. Bunun aksine, sonuçları bu bileşimi içinde çarpıcı bir değişiklik araştırma vurgulanmış alanı içinde gösterir. Bu tanılama değişikliği açıklayan özellikleri bileşimini buldu gösterir.

4.  Desen karmaşıksa üzerine gelerek gerek **Tümünü Göster** boyutları görmek için.

    ![tümünü göster](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  Tanılama hakkında bilgilendirmek için önemli hiçbir desen bulur durumda ' yok ' sunulduğunu sunulur. Bu noktada, sorgunuzu değişebilir. Örneğin, zaman aralığı ve daha fazla çözümleme için Analytics sorgu binning daraltmak ve olası sonuçları daha iyi.

Web sitenizin belirli bir sayfa üzerinde belirli bir tarayıcı bir sorun olduğunu bilen sayesinde artık doğrudan sorun sayfasına gidin ve en son değişiklikler araştırın.

## <a name="try-the-demo"></a>Demoyu deneyin

[Bir örnek görmek için burayı tıklatın](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) örnek verileri.

## <a name="how-it-works"></a>Nasıl çalışır?

Akıllı tanılama dayalı bir Gelişmiş Denetimsiz makine öğrenme algoritmasını kullanan [DiffPatterns](app-insights-analytics-reference.md) işlemi. Veri değişikliği açıklayabilir için aday desenleri görünüyor. Her adayı ölçüm üzerindeki etkisini analizleri yaparken ve en iyi değişikliğe karşılık gelen deseni gösterir.

## <a name="no-diagnostic-points"></a>Tanılama noktası yok?

Aşağıdaki ölçütler sağlandığında akıllı tanılama yalnızca çalışır:

 * Akıllı tanılama ayarını açık. Analytics'te ayarlar simgesinin altında arayın.
 * Analytics ayarlarında akıllı tanılama seçenek seçilidir. 
 * Zaman ekseni: grafiğin x ekseni türde olmalıdır `datetime`.
 * Çizgi veya alan grafiği: Tanılama yalnızca bu tür grafik çalışır. Kullanım `| render timechart` veya `| render areachart` sorgunuzu; sonunda veya satır ya da alan grafiği açılan seçicisini seçin.
 * Süreksizlik: Verileri önemli süreksizlik koyulmalıdır.
 * Analiz etmek için yeterli noktaları.
 * Birden fazla sorgu yan tümcesinde özetler.
 * Özetleme yan tümcesi önce adı tanımını içeren hiçbir proje yan tümcesi.

 
 ## <a name="related-articles"></a>İlgili makaleler

 * [Analytics Öğreticisi](app-insights-analytics-tour.md)
 * [Akıllı algılama](app-insights-proactive-diagnostics.md) performans sorunları otomatik olarak sizi uyarır.