---
title: Azure Application Insights kullanıcı, oturum ve olay analizi | Microsoft docs
description: Kullanıcılar, web uygulamanızın demografik analizini.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/24/2018
ms.author: mbullwin; daviste
ms.openlocfilehash: 3ac738d6b3c0f1f3579a9b644a03a01f1509173a
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Application Insights kullanıcıları, oturumlar ve olayları çözümleme

Kişiler bunlar en, burada kullanıcılarınızın bulunur ve hangi tarayıcılar ve işletim sistemleri kullandıkları ilgileniyor hangi sayfaların web uygulamanızı kullandığınızda öğrenin. İş ve kullanım telemetrisi kullanarak çözümlemek [Azure Application Insights](app-insights-overview.md).

![Uygulama Öngörüler kullanıcıları ekran görüntüsü](./media/app-insights-usage-segmentation/0001-users.png)

## <a name="get-started"></a>başlarken

Kullanıcılar, oturumlar veya Application Insights portalında olayları Kanatlar verileri henüz görmüyorsanız, [kullanım araçları ile çalışmaya başlama öğrenin](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Kullanıcıları, oturumlar ve Olayları kesimleme aracı

Üç kullanım Kanatlar aynı aracı dilimlediği telemetri üç perspektiflerden web uygulamanızı kullanın. Filtreleme ve veri bölme farklı sayfalarında ve özelliklerin göreli kullanımı hakkında Öngörüler açığa.

* **Kullanıcılar aracını**: kaç kişinin uygulamanızı ve özelliklerini kullanılır.  Kullanıcıların, tarayıcı tanımlama bilgilerinde depolanan anonim kimlikleri kullanılarak sayılır. Farklı tarayıcılar ya da makineleri kullanan tek bir kişi olarak birden fazla kullanıcı sayılacaktır.
* **Oturumlar aracını**: kullanıcı etkinliği kaç oturumları belirli sayfalarını ve özelliklerini, uygulamanızın eklediniz. Bir oturum, kullanıcı yapılmadıktan yarım saat sürekli kullanımı 24 saat sonra veya sayılır.
* **Olayları aracı**: belirli sayfalarını ve özelliklerini, uygulamanızın ne sıklıkta kullanılır. Sahip olduğunuz sağlanan sayfa görünümü bir tarayıcı, uygulamanızdan bir sayfa yüklediğinde sayılır [onu izlenmiş](app-insights-javascript.md). 

    Özel bir olay, bir şeyin uygulamanızı, genellikle bir düğmeye gibi bir kullanıcı etkileşimi veya bazı görev tamamlandığında bir oluşumu temsil eder. Uygulamanızda için kod ekleme [özel olayları](app-insights-api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Belirli kullanıcıların sorgulama

Farklı kullanıcı grupları, kullanıcıları aracın üstündeki sorgu seçeneklerini ayarlayarak keşfedin:

* Göster: çözümlemek için bir kohort kullanıcıları seçin.
* Kim: özel olaylar seçin ve sayfa görünümleri.
* İşlem sırasında: bir zaman aralığı seçin.
* Tarafından: bir süre veya tarayıcı veya şehir gibi başka bir özellik tarafından veri aralığı nasıl ek Yardım düğmesini seçin.
* Bölme: bir özellik olarak bölme veya kesimine verileri seçin. 
* Filtreler ekleyin: belirli kullanıcılar, oturumlar ve olayları, tarayıcı veya şehir gibi özelliklerine göre sorgu sınırlayın. 
 
## <a name="saving-and-sharing-reports"></a>Kaydetme ve raporları paylaşma 
Bu Application Insights kaynağı paylaşılan Raporlar bölümündeki erişimi olan başka herkesle kullanıcıların raporları, yalnızca, raporlarım bölümünde özel ya da paylaşılan kaydedebilirsiniz.

Kullanıcıları, oturumları veya olayları rapora bir bağlantı paylaşmak için; tıklatın **paylaşımı** araç çubuğunda, ardından bağlantı kopyalayın.

Kullanıcıları, oturumları veya olayları rapordaki verilerin bir kopyasını paylaşmak için; tıklatın **paylaşımı** araç çubuğunda, ardından **Word simgesi** verilerle bir Word belgesi oluşturmak için. Veya tıklatın **Word simgesi** ana grafiğin üstünde.

## <a name="meet-your-users"></a>Kullanıcılarınızın karşılayan

**Kullanıcılarınızın karşılayan** bölüm geçerli sorgu tarafından eşleşen beş örnek kullanıcılar bilgileri gösterir. Dikkate ve toplamalar, yanı sıra kişiler davranışlarını keşfetme kişiler gerçekten uygulamanızı kullanma hakkında bilgiler sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımı deneyimleri etkinleştirmek için göndermeye Başla [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olaylar veya sayfa görünümleri zaten gönderirseniz, kullanıcıların hizmetinizin kullanımını öğrenmek için kullanım araçları keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](app-insights-usage-retention.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Çalışma kitapları](app-insights-usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)