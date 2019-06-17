---
title: Azure Application Insights ile web uygulamaları için kullanıcı saklama analizi | Microsoft docs
description: Kaç kullanıcının uygulamanızı döndürür?
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/03/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: bda79520dd86cc14161f6f22cd24feb2e35849ab
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60372643"
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Application Insights ile web uygulamaları için kullanıcı saklama analizi

Saklama özelliği [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) kaç kullanıcının uygulamanıza dönün ve ne sıklıkta kullanıcılar belirli görevleri veya hedeflere ulaşmak analiz yardımcı olur. Örneğin, bir oyun site çalıştırırsanız, ödüllü sonra iade kimin numarası ile bir oyun kaybettikten sonra siteye geri dön kullanıcılar sayıda kıyaslayarak. Bu bilgi, hem kullanıcı deneyiminizi ve iş stratejinizin geliştirmeye yardımcı olabilir.

## <a name="get-started"></a>başlarken

Application Insights portalında, elde tutma aracı verileri henüz göremiyorsanız [kullanım araçları ile çalışmaya başlama hakkında bilgi edinin](usage-overview.md).

## <a name="the-retention-tool"></a>Elde tutma aracı

![Elde tutma aracı](./media/usage-retention/retention.png)

1. Araç çubuğunu yeni saklama raporlar oluşturun, mevcut bekletme raporları açın, geçerli bekletme raporu kaydedin veya kaydedilmiş raporları için yapılan değişiklikleri geri al, rapor veri yenileme, e-posta veya doğrudan bağlantı üzerinden raporu paylaşmak ve belgelerine erişmek olarak kaydedin açmasına izin verir. Sayfa. 
2. Varsayılan olarak, bekletme şey tüm kullanıcılar daha sonra geri geldi ve diğer her şey bir dönem boyunca yaptığınız gösterir. Belirli kullanıcı etkinlikleri üzerinde odağını daraltmak için olayları farklı bir birleşimini seçebilirsiniz.
3. Bir veya daha fazla filtre özellikleri ekleyin. Örneğin, belirli bir ülke veya bölgedeki kullanıcılar üzerinde odaklanabilirsiniz. Tıklayın **güncelleştirme** filtreleri ayarladıktan sonra. 
4. Toplam elde tutma grafiğini, seçilen süre kullanıcı saklama bir özetini gösterir. 
5. Izgara #2'de Sorgu Oluşturucu göre tutulan kullanıcı sayısını gösterir. Her satır, dönem gösterilen saat içinde herhangi bir olay gerçekleştiren bir kullanıcıların kohortu temsil eder. Sıradaki her hücre en az bir kez daha sonraki bir dönemde ne kadarının bu kohort döndürülen gösterir. Bazı kullanıcılar birden fazla dönemde döndürebilir. 
6. Görüşler kartları, en çok beş başlatma olaylarını ve daha iyi anlamak, bekletme raporun kullanıcılara vermek için en çok beş döndürülen olayları gösterir. 

![Bekletme fareyle üzerine gelindiğinde](./media/usage-retention/hover.png)

Kullanıcılar hücre ne anlama geldiğini açıklayan analytics düğmesi ve araç ipuçları erişmek için elde tutma aracı hücreleri üzerine getirin. Analiz düğmesine hücreden kullanıcı oluşturmak için önceden doldurulmuş bir sorgu ile analiz aracı kullanıcılara alır. 

## <a name="use-business-events-to-track-retention"></a>İş olayları, bekletme izlemek için kullanabilirsiniz.

En kullanışlı bekletme analiz almak için önemli iş etkinlikleri temsil eden olayları ölçün. 

Örneğin, çok sayıda kullanıcı görüntülediği oyuna olmadan bir sayfa uygulamanızda açılabilir. Daha önce keyfini sonra oyuna kaç kişinin döndürür, yanlış tahmin yalnızca sayfa görünümleri izleme bu nedenle sağlar. Oyuncuların döndürme NET bir görüntüsünü almak için uygulamanızı bir kullanıcının gerçekten yürütüldüğünde bir özel olay göndermesi gerekir.  

Anahtar iş eylemleri temsil eden özel olaylar kodlayın ve bu bekletme çözümleme için iyi bir uygulamadır. Oyun sonucu yakalamak için bir özel olay Application Insights'a göndermek için kod satırı yazmanız gerekir. Web sayfası koduna veya Node.JS yazma, şöyle görünür:

```JavaScript
    appinsights.trackEvent("won game");
```

ASP.NET sunucusu kod:

```csharp
   telemetry.TrackEvent("won game");
```

[Özel olaylar yazma hakkında daha fazla bilgi](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Sonraki adımlar
- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Kullanıcı Akışları](usage-flows.md)
    - [Çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)
    - [Kullanıcı bağlamı Ekle](usage-send-user-context.md)


