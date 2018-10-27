---
title: Azure Application Insights kullanıcı, oturum ve olay analizi | Microsoft docs
description: Web uygulamanızın kullanıcılarının demografik analizi.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 6fd8adab93f5741afe6d3eab0c50ca50a327fbff
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140341"
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Application Insights kullanıcılar, oturumlar ve olaylar analizi

Web uygulamanız, bunlar en, burada, kullanıcılarınızın bulunduğu ve hangi tarayıcılar ve işletim sistemleri kullandıkları ilginizi çeken hangi sayfaların kullandıklarında öğrenin. Kullanarak iş ve kullanım telemetrisini çözümlemek [Azure Application Insights](app-insights-overview.md).

![Application Insights kullanıcılarının ekran görüntüsü](./media/app-insights-usage-segmentation/0001-users.png)

## <a name="get-started"></a>başlarken

Kullanıcılar, oturumlar veya olaylar dikey pencereleri Application Insights portalında verileri henüz göremiyorsanız [kullanım araçları ile çalışmaya başlama hakkında bilgi edinin](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Kullanıcılar, oturumlar ve olaylar Segment aracı

Kullanım dikey pencerelerinde üçünün de aynı aracı dilimlediği telemetri üç perspektiflerden web uygulamanızdan kullanın. Filtreleme ve verileri bölme farklı sayfalarını ve özelliklerini göreli kullanımıyla ilgili öngörüleri ortaya çıkarabilirsiniz.

* **Kullanıcılar aracı**: kaç kişinin uygulamanızı ve özellikleri kullanılır.  Kullanıcıların, tarayıcı tanımlama bilgilerinde depolanan anonim kimlikleri kullanarak sayılır. Farklı tarayıcılar veya makineleri kullanarak tek bir kişi olarak birden fazla kullanıcı olarak sayılır.
* **Oturumlar aracına**: kaç oturum, kullanıcı etkinliğinin belirli sayfalarını ve özelliklerini uygulamanızın olarak eklenir. Oturum, kullanıcı yapılmadığında yarım saat sürekli kullanıma 24 saat sonra veya sayılır.
* **Olaylar aracına**: belirli sayfalarını ve özelliklerini uygulamanızın ne sıklıkla kullanılır. Sahip olduğunuz sağlanan bir sayfa görünümü bir tarayıcı, uygulamanızdan Sayfa yüklediğinde sayılır [, izleme eklenmiş](app-insights-javascript.md). 

    Özel olay şeyin uygulamanızı, genellikle bir düğmeyi gibi bir kullanıcı etkileşimi veya bazı görevi tamamlanırken bir oluşumu temsil eder. Uygulamanıza kod eklemek [özel olaylar oluşturma](app-insights-api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Belirli kullanıcılar sorgulama

Farklı kullanıcı grupları, kullanıcılar aracı üst kısmındaki sorgu seçeneklerini ayarlayarak keşfedin:

* Göster: Analiz etmek için kullanıcıların kohortu seçin.
* Şunu kullanan: özel olaylar'ı seçin ve sayfa görüntüleme.
* İşlem sırasında: bir zaman aralığı seçin.
* Tarafından: bir süre veya tarayıcı veya şehir gibi başka bir özellik tarafından veri demetine nasıl ek Yardım düğmesini seçin.
* Bölme ölçütü: bir özellik olarak bölünmüş veya segment için verileri seçin. 
* Filtreleri ekleyin: Sorgu belirli kullanıcılar, oturumlar veya tarayıcı veya şehir gibi özelliklerini dayalı olarak olayları sınırı. 
 
## <a name="saving-and-sharing-reports"></a>Kaydetme ve rapor paylaşma 
Kullanıcıların raporları yalnızca raporlarım bölümünde, özel veya paylaşılan diğer paylaşılan Raporlar bölümünde bu Application Insights kaynağına erişimi olan herkes ile kaydedebilirsiniz.

Kullanıcılar, oturumlar veya olaylar bir raporun bir bağlantısını paylaşmak için; tıklayın **paylaşımı** araç çubuğunda, ardından bağlantıyı kopyalayın.

Kullanıcılar, oturumlar veya olaylar rapordaki verilerin bir kopyasını paylaşmak için; tıklayın **paylaşımı** araç çubuğunda, ardından **Word simgesi** verilerle bir Word belgesi oluşturmak için. Veya tıklayın **Word simgesi** ana grafiğin üstünde.

## <a name="meet-your-users"></a>Kullanıcılarınızla buluşun

**Kullanıcılarınızın karşılamak** bölümü geçerli sorgu tarafından eşleşen yaklaşık beş örnek kullanıcı bilgileri gösterir. Düşünüyor ve davranışları kişilerin, toplamlar, ek olarak keşfetmeye insanların gerçekte uygulamanızı kullanma hakkında bilgiler sağlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](app-insights-usage-retention.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Çalışma kitapları](app-insights-usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)