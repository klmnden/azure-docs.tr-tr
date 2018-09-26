---
title: Azure Application ınsights'da Uygulama Haritası | Microsoft Docs
description: Uygulama Haritası ile karmaşık bir uygulama topolojisini izleyin
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 9b39eef5accec4764f61ab31dd894d368242ee3d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094659"
---
# <a name="application-map-triage-distributed-applications"></a>Uygulama Haritası: Dağıtılmış uygulamalar Önceliklendirme

Uygulama Haritası, nokta performans sorunlarını veya hata noktalarına dağıtılmış uygulamanızı tüm bileşenler genelinde yardımcı olur. Harita üzerinde her düğüm, bir uygulama bileşeni veya bağımlılıkları temsil eder; KPI durum ve durum uyarır. Aracılığıyla herhangi bir bileşene Application Insights olayları gibi daha ayrıntılı tanılama için tıklayabilirsiniz. Uygulamanızı Azure hizmetlerini kullanıyorsa, aracılığıyla SQL veritabanı Danışmanı önerilerini gibi Azure tanılama tıklatabilirsiniz.

## <a name="what-is-a-component"></a>Bir bileşen nedir?

Bağımsız bir şekilde dağıtılabilen dağıtılmış mikro uygulamanızın parçalarını bileşenlerdir. Geliştiriciler ve işlem ekipleri bu uygulama bileşenleri tarafından oluşturulan telemetri erişimi veya kod düzeyinde görünürlüğe sahip. 

* Bileşenleri "gözlemlenen" dış bağımlılıkları SQL gibi farklı takım/kuruluşunuz olmayabilir EventHub vb. erişim (kod veya telemetri).
* Bileşenleri rol/sunucu/kapsayıcı örnekleri herhangi bir sayıda çalışır.
* Bileşenleri ayrı bir Application Insights izleme anahtarı (abonelikler farklı olsa bile) olması ya da farklı roller için tek bir Application Insights izleme anahtarı raporlama gerçekleştirebilirsiniz. Önizleme eşlemesi deneyimi bileşenlerini nasıl ayarladıktan bakılmaksızın gösterir.

## <a name="composite-application-map"></a>Bileşik Uygulama Eşlemesi

Tam uygulama topolojisinin birden fazla seviyede ilgili uygulama bileşenleri arasında görebilirsiniz. Bileşenler, farklı Application Insgihts kaynakları veya tek bir kaynaktaki farklı roller olabilir. Uygulama Haritası Application Insights SDK'yi içeren sunucular arasında yapılan aşağıdaki HTTP bağımlılık çağrıları tarafından bileşenlerini bulur. 

Bu deneyim, bileşenlerin aşamalı bulma ile başlar. Uygulama Haritası'ı ilk yüklediğinizde, bir sorgu kümesi bu bileşenle ilgili bileşenler bulmak için tetiklenir. Bulundukları anda sol üst köşesinde bir düğme, uygulamanızda bileşen sayısı ile güncelleştirir. 

"Güncelleştirme eşleme bileşenlerini" a tıklandığında, harita noktasında kadar bulunan tüm bileşenleri ile yenilenir.

Tüm bileşenleri tek bir Application Insights kaynağı içindeki rol varsa, bu bulma adım gerekli değildir. Bu tür bir uygulama için ilk yükleme işlemi, tüm bileşenleri sahip olur.

![Uygulama Haritası ekran görüntüsü](media/app-insights-app-map/001.png)

Bu deneyim ile anahtar hedeflerinden bileşenleri yüzlerce karmaşık topolojiler görselleştirmek sağlamaktır.

Öngörüleri görmek ve performans ve söz konusu bileşen için hata önceliklendirme deneyimi için herhangi bir bileşeni tıklayın.

![Açılır öğesi](media/app-insights-app-map/application-map-001.png)

### <a name="investigate-failures"></a>Hataları araştır

Seçin **hataları Araştır** hataları bölmesinde başlatmak için.

![Ekran görüntüsü araştırma hataları düğmesi](media/app-insights-app-map/investigate-failures.png)

![Hataları deneyiminin ekran görüntüsü](media/app-insights-app-map/failures.png)

### <a name="investigate-performance"></a>Performansı araştır

Performans sorunları seçme sorunlarını gidermek için **performansını araştırın**

![Ekran görüntüsü araştırmak performans düğmesi](media/app-insights-app-map/investigate-performance.png)

![Performans deneyimi ekran görüntüsü](media/app-insights-app-map/performance.png)

### <a name="go-to-details"></a>Ayrıntılara git

Seçin **ayrıntıları Git** çağrı yığın düzeyi için gerçekleştirilen görünümleri sunan uçtan uca işlem deneyimi keşfedin.

![Git ayrıntıları düğmesinin Ekran görüntüsü](media/app-insights-app-map/go-to-details.png)

![Uçtan uca işlem ayrıntıları ekran görüntüsü](media/app-insights-app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Analytics'de görüntüle

Sorgulamak ve uygulamaların veri başka tıklatın araştırmak için **analytics'te görüntüle**.

![Analytics düğmesinde görünümünün ekran görüntüsü](media/app-insights-app-map/view-in-analytics.png)

![Analytics deneyiminin ekran görüntüsü](media/app-insights-app-map/analytics.png)

### <a name="alerts"></a>Uyarılar

Etkin uyarıları ve Uyarıları tiggered olması neden temel kurallarını görüntülemek için seçin **uyarılar**.

![Uyarılar düğmesinin Ekran görüntüsü](media/app-insights-app-map/alerts.png)

![Analytics deneyiminin ekran görüntüsü](media/app-insights-app-map/alerts-view.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Geri Bildirim
Lütfen portalı geri bildirim seçeneği aracılığıyla geri bildirim sağlayın.

![MapLink 1 görüntüsü](./media/app-insights-app-map/13.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal](https://portal.azure.com)
