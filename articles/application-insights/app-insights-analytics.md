---
title: "Analizi - güçlü arama ve sorgu aracının Azure Application Insights | Microsoft Docs"
description: "Analytics, Application Insights, güçlü tanılama arama aracına genel bakış. "
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: 80a9e248ca50c11ef61a5c50c4986c4f8f4ead9d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="analytics-in-application-insights"></a>Application Insights analizleri
Analytics araçtır güçlü arama ve sorgu, [Application Insights](app-insights-overview.md). Kurulum nedenle analytics web aracıdır. Zaten Application Insights uygulamalarınızı biri için yapılandırdıktan sonra uygulamanızın analizi açarak uygulamanızın verileri çözümleyebilir [genel bakış dikey penceresinde](app-insights-dashboards.md).

![Portal.Azure.com açın, Application Insights kaynağınıza açın ve analizi'ı tıklatın.](./media/app-insights-analytics/001.png)

Aynı zamanda [Analytics playground](https://go.microsoft.com/fwlink/?linkid=859557) çok örnek veri ücretsiz tanıtım ortamıyla olduğu.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Sorgu veri analizi
Tipik bir sorgu bir dizi tarafından izlenen bir tablo adı ile başlayan *işleçleri* ayırarak `|`.
Örneğin, şimdi kaç istekleri sırasında son 3 saat farklı ülkelerden alınan uygulamamıza bulun:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

Biz tablo adıyla Başlat *istekleri* ve gerektiği gibi yöneltilen öğeleri ekleyin.  Önce yalnızca son 3 saat kayıtları gözden geçirmek için zaman filtresi tanımlayın.
Biz sonra ülke başına kayıt sayısı (veri sütunu bulunduğunda *client_CountryOrRegion*). Son olarak, pasta grafiği sonuçlarında işleyebilir.
<br>

![Sorgu sonuçları](./media/app-insights-analytics/030.png)

Dil çok çekici özelliklere sahiptir:

* [Filtre](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) ölçümleri ve özel özellikler de dahil olmak üzere herhangi bir sorgu tarafından ham uygulama telemetrinizi.
* [Katılma](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) birden çok tabloları – sayfa görünümleri, bağımlılık çağrıları, özel durumlar ve günlük izlemelerini ile ilişkilendirmek ister.
* Güçlü istatistiksel [toplamalar](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Anında ve güçlü görselleştirmeler.
* [REST API](https://dev.applicationinsights.io/) sorguları program aracılığıyla, örneğin Powershell'den çalıştırmak için kullanabilirsiniz.

[Tam dil başvurusu](https://go.microsoft.com/fwlink/?linkid=856079) desteklenen her komut ayrıntıları ve düzenli olarak güncelleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanmaya başlama [Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587)
* Başlama [sorgu yazma](https://go.microsoft.com/fwlink/?linkid=856078)
* Gözden geçirme [SQL-kullanıcıların kopya sayfası](https://aka.ms/sql-analytics) en yaygın deyimleri çevirisinde için.
* Sürücü Analytics üzerinde test bizim [playground](https://analytics.applicationinsights.io/demo) uygulamanızı veriler Application Insights'a henüz değil gönderiliyorsa.
* Gözcü [tanıtım videosunu](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).