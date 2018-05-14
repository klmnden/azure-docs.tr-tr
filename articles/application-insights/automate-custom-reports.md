---
title: Azure Application Insights verilerle özel raporlar otomatik hale getirme
description: Azure Application Insights verilerle özel haftalık/günlük/aylık raporlar otomatik hale getirme
services: application-insights
documentationcenter: ''
author: sdash
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: sdash
ms.openlocfilehash: 804e8c7a43d1ab16d11b6075be44599b33b46a3e
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Azure Application Insights verilerle özel raporlar otomatik hale getirme

Dönemsel raporları kendi iş kritik hizmetleri nasıl gittiğini haberdar takım tutmak yardımcı olur. Geliştiriciler, DevOps/SRE ekipleri ve yöneticilerinin ınsights portalında oturum açma herkese gerek kalmadan güvenilir bir şekilde teslim otomatik raporlarla üretken olabilirler. Bu raporlar da aşamalı artar belirlemenize yardımcı olabilecek gecikmeleri içinde herhangi bir tetikleyebilir değil yük veya hata oranları uyarı kuralları.

Her Kuruluş kendi benzersiz raporlama gereksinimlerini gibi sahiptir: 

* Belirli yüzdebirlik toplamalar ölçümleri ya da bir rapordaki özel ölçümleri.
* Günlük, haftalık ve aylık toplamalarını veri için farklı raporları için farklı İzleyici vardır.
* Bölge veya ortam gibi özel öznitelikleri tarafından kesimleme. 
* Bunlar farklı abonelik veya kaynak grupları vb. olabilir olsa bile bazı AI kaynakları tek bir rapor içinde gruplandırın.
* Seçmeli İzleyici gönderilen hassas ölçümleri içeren ayrı raporlar.
* Portal kaynaklarına erişimi olmayabilir Paydaşlar raporlar.

> [!NOTE] 
> Haftalık Application Insights Özet e-posta hiçbir özelleştirme izin verme ve aşağıda listelenen özel seçenekleri lehinde sona erecek. 11 Haziran 2018 son Haftalık Özet e-posta gönderilir. Benzer özel raporlar (aşağıda önerilen sorgu kullanın) almak için aşağıdaki seçeneklerden birini yapılandırın.

## <a name="to-automate-custom-report-emails"></a>Özel rapor e-postaları otomatik hale getirmek için

Yapabilecekleriniz [programlı olarak Application Insights sorgu](https://dev.applicationinsights.io/) bir zamanlamaya göre özel raporlar oluşturmak için veri. Aşağıdaki seçenekler hızla başlamanıza yardımcı olabilir:

* [Microsoft Flow raporlarla otomatik hale getirme](app-insights-automate-with-flow.md)
* [Logic Apps ile raporlar otomatik hale getirme](automate-with-logic-apps.md)
* "Application Insights Zamanlanmış Özet" kullanmak [Azure işlevi](https://azure.microsoft.com/services/functions/) izleme senaryoda şablonu. Bu işlev SendGrid e-posta teslim etmek için kullanır. 

    ![Azure işlevi şablonu](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Haftalık Özet e-posta için örnek sorgu
Aşağıdaki sorgu, birden çok veri kümesi bir Haftalık Özet e-posta için rapor gibi üzerinden katılmak gösterir. Gerektiği gibi özelleştirin ve haftalık rapor otomatik hale getirmek için yukarıda listelenen seçeneklerden birini kullanın.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

  
## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma hakkında daha fazla bilgi [analitik sorguları](app-insights-analytics-using.md).
- Daha fazla bilgi edinmek [programlı olarak Application Insights veri sorgulama](https://dev.applicationinsights.io/)
- [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) hakkında daha fazla bilgi edinin.
- Daha fazla bilgi edinmek [Microsoft Flow](https://ms.flow.microsoft.com).


