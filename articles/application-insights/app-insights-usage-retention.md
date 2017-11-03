---
title: "Azure Application Insights ile web uygulamaları için kullanıcı bekletme analizi | Microsoft docs"
description: "Kaç kullanıcının uygulamanıza dönüş?"
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: f19e5022d366bc686beed42bacf8a4d6d108887e
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Application Insights ile web uygulamaları için kullanıcı bekletme çözümlemesi

Bekletme özelliği [Azure Application Insights](app-insights-overview.md) , analiz, uygulamanızın nasıl sayıda kullanıcının döneceğini ve ne sıklıkta belirli görevleri veya hedeflerinize ulaşmak yardımcı olur. Örneğin, bir oyun sitesi çalıştırırsanız, kimin kazanan sonra iade numarayla oyun kaybettikten sonra siteye geri dönün kullanıcılar sayıda karşılaştırın. Bu bilgi, kullanıcı deneyiminizi ve iş stratejinizi geliştirmenize yardımcı olabilir.

## <a name="get-started"></a>başlarken

Veri saklama aracında Application Insights portalında henüz görmüyorsanız, [kullanım araçları ile çalışmaya başlama öğrenin](app-insights-usage-overview.md).

## <a name="the-retention-tool"></a>Bekletme aracı

![Elde tutma aracı](./media/app-insights-usage-retention/retention.png)

1. Araç, yeni bekletme raporları oluşturmak, varolan bekletme raporları açmak, geçerli bekletme raporu kaydedin veya kaydedilmiş raporları yapılan değişiklikleri geri, rapor veri yenileme, e-posta veya doğrudan bağlantı raporu paylaşmak ve belgelerine erişmek olarak kaydetmek kullanıcılara Sayfa. 
2. Varsayılan olarak, bekletme herhangi bir şey mi tüm kullanıcılar daha sonra geri geldiği ve belirli bir süre boyunca başka bir şey mi gösterir. Belirli kullanıcı etkinlikleri odaklanmak daraltmak için olayları farklı bileşimini seçebilirsiniz.
3. Bir veya daha fazla filtre özellikleri ekleyin. Örneğin, belirli bir ülke veya bölgedeki kullanıcılar üzerinde odaklanabilirsiniz. Tıklatın **güncelleştirme** filtreleri ayarladıktan sonra. 
4. Genel saklama grafik seçili süre kullanıcı bekletme özetini gösterir. 
5. Izgara Sorgu Oluşturucu #2'göre tutulur kullanıcı sayısını gösterir. Her satır, dönem gösterilen zaman içinde herhangi bir olay gerçekleştiren bir kohort kullanıcıların temsil eder. Sıradaki her hücre en az bir kez daha sonraki bir süre içinde bu kohort kaç döndürülen gösterir. Bazı kullanıcılar, birden fazla dönemde döndürebilir. 
6. Öngörüler kartları üst beş başlatma olaylarını ve daha iyi anlamak kendi saklama rapor kullanıcılara vermek için üst beş döndürülen olayları gösterir. 

![Bekletme fare vurgulu](./media/app-insights-usage-retention/hover.png)

Kullanıcı hücrenin ne anlama geldiğini açıklayan analytics düğmesi ve araç ipuçları erişmek için bekletme aracı hücreleri üzerine getirin. Analytics düğmesi hücreden kullanıcı oluşturmak için önceden girilmiş bir sorgu Analytics aracıyla kullanıcılara alır. 

## <a name="use-business-events-to-track-retention"></a>Bekletme izlemek için iş olaylarını kullanın

En kullanışlı bekletme analiz almak için önemli iş faaliyetlerine temsil olayları ölçün. 

Örneğin, çok sayıda kullanıcı görüntülediği oyuna olmadan bir sayfa uygulamanızda açılabilir. Yalnızca sayfa görünümleri izleme yanlış tahminidir kaç kişinin önceden keyfini sonra oyun dönün, bu nedenle sağlar. Oynatıcıları döndürme NET bir resim almak için uygulamanızı bir kullanıcı gerçekte yürütürken özel bir olay göndermesi gerekir.  

Anahtar iş eylemlerini temsil eden özel olaylar kod ve bunlar bekletme çözümleme için kullanmak için iyi bir uygulamadır. Oyun sonucu yakalamak için özel bir olay Application Insights'a gönderme kod satırı yazmanız gerekir. Web sayfası koduna veya Node.JS yazma, şuna benzer:

```JavaScript
    appinsights.trackEvent("won game");
```

ASP.NET sunucusu kod:

```C#
   telemetry.TrackEvent("won game");
```

[Özel olaylar yazma hakkında daha fazla bilgi](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Sonraki adımlar
- Kullanımı deneyimleri etkinleştirmek için göndermeye Başla [özel olaylar](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olaylar veya sayfa görünümleri zaten gönderirseniz, kullanıcıların hizmetinizin kullanımını öğrenmek için kullanım araçları keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](app-insights-usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Çalışma kitapları](app-insights-usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)


