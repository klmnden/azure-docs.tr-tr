---
title: "Azure Application Insights kullanıcı, oturum ve olay analizi | Microsoft docs"
description: "Kullanıcılar, web uygulamanızın demografik analizini."
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
ms.openlocfilehash: 04efb82addd0f307c68c73e28e46b602e5bc194a
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Application Insights kullanıcıları, oturumlar ve olayları çözümleme

Kişilerin web uygulamanızı kullandığınızda, bunlar en, kullanıcılarınızın bulunduğu ilgileniyor hangi sayfaların, hangi tarayıcılar ve işletim sistemleri bunlar kullandığını öğrenin. İş ve kullanım telemetrisi kullanarak çözümlemek [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>başlarken

Kullanıcılar, oturumlar veya Application Insights portalında olayları Kanatlar verileri henüz görmüyorsanız, [kullanım araçları ile çalışmaya başlama öğrenin](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Kullanıcıları, oturumlar ve Olayları kesimleme aracı

Üç kullanım Kanatlar aynı aracı dilimlediği telemetri üç perspektiflerden web uygulamanızı kullanın. Filtreleme ve veri bölme farklı sayfalarında ve özelliklerin göreli kullanımı hakkında Öngörüler açığa.

* **Kullanıcılar aracını**: kaç kişinin uygulamanızı ve özelliklerini kullanılır.  Kullanıcıların, tarayıcı tanımlama bilgilerinde depolanan anonim kimlikleri kullanılarak sayılır. Farklı tarayıcılar ya da makineleri kullanan tek bir kişi olarak birden fazla kullanıcı sayılacaktır.
* **Oturumlar aracını**: kullanıcı etkinliği kaç oturumları belirli sayfalarını ve özelliklerini, uygulamanızın eklediniz. Bir oturum kullanım sürekli 24 h veya kullanıcı yapılmadıktan yarım saat sonra sayılır.
* **Olayları aracı**: belirli sayfalarını ve özelliklerini, uygulamanızın ne sıklıkta kullanılır. Sahip olduğunuz sağlanan sayfa görünümü bir tarayıcı, uygulamanızdan bir sayfa yüklediğinde sayılır [onu izlenmiş](app-insights-javascript.md). 

    Özel bir olay, bir şeyin uygulamanızı, genellikle bir düğmeye gibi bir kullanıcı etkileşimi veya bazı görev tamamlandığında bir oluşumu temsil eder. Uygulamanızda için kod ekleme [özel olayları](app-insights-api-custom-events-metrics.md#trackevent).

![Kullanım aracı](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Belirli kullanıcıların sorgulama 

Farklı kullanıcı grupları, kullanıcıları aracın üstündeki sorgu seçeneklerini ayarlayarak keşfedin: 

* Kim: özel olaylar seçin ve sayfa görünümleri. 
* İşlem sırasında: bir zaman aralığı seçin. 
* Tarafından: bir süre veya tarayıcı veya şehir gibi başka bir özellik tarafından veri aralığı nasıl ek Yardım düğmesini seçin. 
* Bölme: bir özellik olarak bölme veya kesimine verileri seçin. 
* Filtreler ekleyin: belirli kullanıcılar, oturumlar ve olayları, tarayıcı veya şehir gibi özelliklerine göre sorgu sınırlayın. 
 
## <a name="saving-and-sharing-reports"></a>Kaydetme ve raporları paylaşma 
Bu Application Insights kaynağı paylaşılan Raporlar bölümündeki erişimi olan başka herkesle kullanıcıların raporları, yalnızca, raporlarım bölümünde özel ya da paylaşılan kaydedebilirsiniz.  
 
Raporu kaydetme veya özelliklerini düzenlerken, "Geçerli göreli zaman bazı sabit süreyi geri dönerseniz sürekli yenilenen verileri bir raporu olacak şekilde aralığı" seçin.  
 
"Geçerli mutlak zaman sabit bir veri kümesiyle bir raporu kaydetmek için aralığı" seçin. Application Insights verileri yalnızca 90 90 günden itibaren mutlak zaman aralığı olan bir raporu geçmişse kaydedildiği şekilde gün süreyle depolanır unutmayın, rapor boş görünür. 
 
## <a name="example-instances"></a>Örnek örnekleri

Örnek örnekleri bölümüne sayıda bireysel kullanıcıları, oturumları veya geçerli sorgu tarafından eşleşen olaylar hakkında bilgi gösterir. Dikkate ve toplamalar, yanı sıra kişiler davranışlarını keşfetme kişiler gerçekten uygulamanızı kullanma hakkında bilgiler sağlar. 
 
## <a name="insights"></a>Insights 

Öngörüler kenar ortak özellikleri paylaşır kullanıcılar büyük kümelerini gösterir. Bu kümeleri kişiler uygulamanızı kullanma şaşırtıcı eğilimleri ortaya çıkarmaya. Örneğin, tüm uygulamanızı kullanımı % 40 tek bir özelliğini kullanarak kişilerden geliyorsa.  


## <a name="next-steps"></a>Sonraki adımlar
- Kullanımı deneyimleri etkinleştirmek için göndermeye Başla [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olaylar veya sayfa görünümleri zaten gönderirseniz, kullanıcıların hizmetinizin kullanımını öğrenmek için kullanım araçları keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](app-insights-usage-retention.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Çalışma kitapları](app-insights-usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)

