---
title: Analizi - güçlü arama ve sorgu aracının Azure Application Insights | Microsoft Docs
description: 'Analytics, Application Insights, güçlü tanılama arama aracına genel bakış. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 170cd76c72e8aeb5de48c711ae4637a0244742fb
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294209"
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

* [Filtre](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) ölçümleri ve özel özellikler de dahil olmak üzere herhangi bir sorgu tarafından ham uygulama telemetrinizi.
* [Katılma](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/join-operator) birden çok tabloları – sayfa görünümleri, bağımlılık çağrıları, özel durumlar ve günlük izlemelerini ile ilişkilendirmek ister.
* Güçlü istatistiksel [toplamalar](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions).
* Anında ve güçlü görselleştirmeler.
* [REST API](https://dev.applicationinsights.io/) sorguları program aracılığıyla, örneğin Powershell'den çalıştırmak için kullanabilirsiniz.

[Tam dil başvurusu](https://go.microsoft.com/fwlink/?linkid=856079) desteklenen her komut ayrıntıları ve düzenli olarak güncelleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanmaya başlama [Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587)
* Başlama [sorgu yazma](https://go.microsoft.com/fwlink/?linkid=856078)
* Gözden geçirme [SQL-kullanıcıların kopya sayfası](https://aka.ms/sql-analytics) en yaygın deyimleri çevirisinde için.
* Sürücü Analytics üzerinde test bizim [playground](https://analytics.applicationinsights.io/demo) uygulamanızı veriler Application Insights'a henüz değil gönderiliyorsa.
* Gözcü [tanıtım videosunu](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).