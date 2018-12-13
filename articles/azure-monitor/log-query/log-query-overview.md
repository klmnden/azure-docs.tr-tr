---
title: Azure İzleyici'de log Analytics verilerini çözümleme | Microsoft Docs
description: Log Analytics'ten verileri almak için bir günlük araması gerektirir.  Bu makalede, yeni günlük aramaları Log Analytics'te kullanılan açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.openlocfilehash: 26030764544189ae7b075711f0405bf5c0b4ab8f
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53193833"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Azure İzleyici'de log Analytics verilerini çözümleme

Azure İzleyici tarafından toplanan günlük verilerini temel aldığı bir Log Analytics çalışma alanında depolanan [Azure Veri Gezgini](/azure/data-explorer). Çeşitli kaynaklardan telemetri toplar ve kullandığı [sorgu dilini veri Gezgini'nde](/azure/kusto/query) almak ve verileri çözümlemek için.

> [!NOTE]
> Log Analytics daha önce kendi Azure hizmeti olarak kabul. Artık Azure İzleyici bir parçası olarak kabul edilir ve depolama ve analiz sorgu dili kullanarak günlük verilerinin odaklanır. Proaktif olarak size sorunları bildirmekten için Windows ve Linux aracıları için veri toplama, mevcut verileri ve Uyarıları görselleştirmek için görünümleri gibi Log Analytics, ın parçası olarak kabul özellikleri değişmemiştir, ancak artık Azure İzleyici bir parçası olarak kabul edilir.



## <a name="log-queries"></a>Günlük sorguları

Log Analytics bağlantısı herhangi bir veri almak için günlük sorgusu gerektirir.  Olmanıza [Portalı'nda veri çözümleme](../../azure-monitor/log-query/portals.md), [bir uyarı kuralı yapılandırma](../../azure-monitor/platform/alerts-metric.md) bir belirli bir koşula veya verileri alınırken kullanarak bildirilmesini [Log Analytics API](https://dev.loganalytics.io/), bir sorgu, istediğiniz verileri belirtmek için kullanır.  Bu makale, Log Analytics'te günlük sorguları nasıl kullanıldığını açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar.



## <a name="where-log-queries-are-used"></a>Günlük sorguları kullanıldığı

Log Analytics'te sorgu kullanacağını farklı yollar şunlardır:

- **Portalları.** Günlük veri etkileşimli analiz gerçekleştirebilir [Azure portalında](../../azure-monitor/log-query/portals.md).  Bu, sorgunuzu düzenleyin ve çeşitli biçimlerde ve görselleştirmeler sonuçları analiz etmek sağlar.  
- **Uyarı kuralları.** [Uyarı kuralları](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) çalışma alanınızdaki veri sorunları proaktif olarak belirleyin.  Her uyarı kuralı otomatik olarak düzenli aralıklarla çalışan bir günlük araması temel alır.  Sonuçları bir uyarının oluşturulması gerektiğini belirlemek için incelenir.
- **Panolar.** Herhangi bir sorgu sonuçlarını sabitleyebilirsiniz bir [Azure panosuna](../../azure-monitor/platform/dashboards.md) olan izin, günlük ve ölçüm verileri bir araya görselleştirip, isteğe bağlı olarak diğer Azure kullanıcıları ile paylaşın. 
- **Görünümler.**  Kullanıcı panolarla dahil edilecek veri görselleştirmeleri oluşturabilirsiniz [Görünüm Tasarımcısı](../../azure-monitor/platform/view-designer.md).  Günlük sorguları tarafından kullanılan verileri sağlar [kutucukları](../../azure-monitor/platform/view-designer-tiles.md) ve [görselleştirme bölümleri](../../azure-monitor/platform/view-designer-parts.md) her görünümde.  
- **Dışarı aktarın.**  İçeri aktardığınızda verilerin Log Analytics çalışma alanından Excel'e veya [Power BI](../../azure-monitor/platform/powerbi.md), dışarı aktarmak için verileri tanımlamak için bir günlük sorgusu oluşturun.
- **PowerShell.** Bir komut satırı veya kullanan bir Azure Otomasyonu runbook'u bir PowerShell Betiği çalıştırabilirsiniz [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) Log Analytics'ten verileri almak için.  Bu cmdlet, alınacak verileri belirlemek üzere bir sorgu gerektirir.
- **Log Analytics API'si.**  [Log Analytics günlük arama API'si](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) çalışma alanından günlük verilerini almak herhangi bir REST API istemcisi sağlar.  API isteği almak için verileri belirlemek için Log Analytics karşı çalışan bir sorgu içerir.

![Günlük aramaları](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Bir sorgu yazma
Analytics kullanan oturum [Veri Gezgini sorgu dil sürümünü](../../azure-monitor/log-query/get-started-queries.md) almak ve günlük verilerini çeşitli şekillerde analiz etmek için.  Temel sorgular genellikle başlatmak ve gereksinimlerinizi daha karmaşık bir HAL aldıkça daha gelişmiş işlevleri kullanmak için ilerleme durumu.

Bir sorgu temel yapısı bir dikey çizgi karakteriyle ayırarak işleçleri dizi arkasından bir kaynak tablodur `|`.  Verilerin oluşturulup geliştirilmesi ve gelişmiş işlevleri gerçekleştirmek için birden çok işleç araya zincirleyebilirsiniz.

Örneğin, geçtiğimiz gün içinde çoğu hata olayları üst on bilgisayarlarla bulmak istediğinizi varsayalım.

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

Veya belki de bir sinyal son gününe sahip olmayan bilgisayarları bulmak istediğiniz.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

Peki geçen haftadan itibaren her bilgisayar için işlemci kullanımı ile çizgi grafik?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

Bu hızlı örnekleri, sorgu yapısını birlikte çalıştığınız veri türü ne olursa olsun benzer olduğunu görebilirsiniz.  Bu sonuç verileri tek komuttan ardışık düzende sonraki komuta burada gönderilir ayrı adımlara ayırabilirsiniz.

Log Analytics çalışma alanı arasında aboneliğinizde da verileri sorgulayabilirsiniz.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-log-analytics-data-is-organized"></a>Log Analytics verilerini nasıl düzenlenir
Bir sorgu oluşturduğunuzda, aradığınız veri tabloları sahip belirleyerek işe başlayın. Farklı türlerde veri ayrılmış her adanmış tablolara [Log Analytics çalışma alanı](../../azure-monitor/learn/quick-create-workspace.md).  Farklı veri kaynakları için belge oluşturduğu veri türünün adını ve açıklamasını her birinin özelliklerini içerir.  Çok sayıda sorgu yalnızca tek bir tablodan veri gerektirir ancak diğerleri birden çok tablodan veri eklemek için çeşitli seçenekler kullanabilir.

Sırada [Application Insights](../../application-insights/app-insights-overview.md) istekler, özel durumlar, izlemeler ve Log analytics'te bu veri kullanımını gibi uygulama verilerini depolar, diğer günlük verileri farklı bir bölümden depolanır. Bu verilere erişmek için aynı sorgu dilini kullanın, ancak kullanmalısınız [Application Insights konsol](../../application-insights/app-insights-analytics.md) veya [Application Insights REST API](https://dev.applicationinsights.io/) erişmek için. Kullanabileceğiniz [kaynaklar arası sorgular](../../azure-monitor/log-query/cross-workspace-query.md) Application Insights verilerini Log analytics'teki diğer verilerle birleştirmek için.


![Tablolar](media/log-query-overview/queries-tables.png)







## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [oluşturmak ve düzenlemek için kullandığınız portalları günlük aramaları](../../azure-monitor/log-query/portals.md).
- Kullanıma bir [sorgu yazmakla ilgili öğretici](../../azure-monitor/log-query/get-started-queries.md) yeni sorgu diline kullanarak.
