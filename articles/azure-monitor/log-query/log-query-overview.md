---
title: Azure İzleyici'de günlük verilerini analiz edin | Microsoft Docs
description: Azure İzleyici'den günlük verilerini almak için günlük sorgusu gerektirir.  Bu makalede, Azure İzleyici'de kullanılan sorguları nasıl yeni bir günlük açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: bwren
ms.openlocfilehash: 6ed3a98282221d5ac148e88b6646bfaa4da768be
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446439"
---
# <a name="analyze-log-data-in-azure-monitor"></a>Azure İzleyici'de günlük verilerini çözümleme

Azure İzleyici tarafından toplanan günlük verilerini temel aldığı bir Log Analytics çalışma alanında depolanan [Azure Veri Gezgini](/azure/data-explorer). Çeşitli kaynaklardan telemetri toplar ve kullandığı [Kusto sorgu dili](/azure/kusto/query) almak ve verileri çözümlemek için Veri Gezgini tarafından kullanılır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a name="log-queries"></a>Günlük sorguları

Azure İzleyici'deki herhangi bir günlük veri almak için günlük sorgusu gerektirir.  Olmanıza [Portalı'nda veri çözümleme](portals.md), [bir uyarı kuralı yapılandırma](../platform/alerts-metric.md) bir belirli bir koşula veya verileri alınırken kullanarak bildirilmesini [Azure İzleyici günlüklerine API](https://dev.loganalytics.io/) , istediğiniz verileri belirtmek için bir sorgu kullanın.  Bu makalede, Azure İzleyici'de günlük sorguları nasıl kullanıldığını açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar.



## <a name="where-log-queries-are-used"></a>Günlük sorguları kullanıldığı


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyicisi'nde sorgular kullanacağını farklı yollar şunlardır:


- **Portalı.** Günlük veri etkileşimli analiz gerçekleştirebilir [Azure portalında](portals.md).  Bu, sorgunuzu düzenleyin ve çeşitli biçimlerde ve görselleştirmeler sonuçları analiz etmek sağlar.  
- **Uyarı kuralları.** [Uyarı kuralları](../platform/alerts-overview.md) çalışma alanınızdaki veri sorunları proaktif olarak belirleyin.  Her uyarı kuralı otomatik olarak düzenli aralıklarla çalışan bir günlük araması temel alır.  Sonuçları bir uyarının oluşturulması gerektiğini belirlemek için incelenir.
- **Panolar.** Herhangi bir sorgu sonuçlarını sabitleyebilirsiniz bir [Azure panosuna](../learn/tutorial-logs-dashboards.md) olan izin, günlük ve ölçüm verileri bir araya görselleştirip, isteğe bağlı olarak diğer Azure kullanıcıları ile paylaşın. 
- **Görünümler.**  Kullanıcı panolarla dahil edilecek veri görselleştirmeleri oluşturabilirsiniz [Görünüm Tasarımcısı](../platform/view-designer.md).  Günlük sorguları tarafından kullanılan verileri sağlar [kutucukları](../platform/view-designer-tiles.md) ve [görselleştirme bölümleri](../platform/view-designer-parts.md) her görünümde.  

- **Dışarı aktarın.**  Aktardığınızda günlük verilerini Azure İzleyici'den Excel'e veya [Power BI](../platform/powerbi.md), dışarı aktarmak için verileri tanımlamak için bir günlük sorgusu oluşturun.
- **PowerShell.** Bir komut satırı veya kullanan bir Azure Otomasyonu runbook'u bir PowerShell Betiği çalıştırabilirsiniz [Get-AzOperationalInsightsSearchResults](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults) günlük verilerini Azure İzleyici'den alınamadı.  Bu cmdlet, alınacak verileri belirlemek üzere bir sorgu gerektirir.
- **Azure İzleyici günlüklerine API.**  [Azure İzleyici günlüklerine API](../platform/alerts-overview.md) çalışma alanından günlük verilerini almak herhangi bir REST API istemcisi sağlar.  API isteği almak için verileri belirlemek için Azure İzleyici'karşı çalışan bir sorgu içerir.

![Günlük aramaları](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Bir sorgu yazma
Azure İzleyicisi'ni kullanan [Kusto sorgu dil sürümünü](get-started-queries.md) almak ve günlük verilerini çeşitli şekillerde analiz etmek için.  Temel sorgular genellikle başlatmak ve gereksinimlerinizi daha karmaşık bir HAL aldıkça daha gelişmiş işlevleri kullanmak için ilerleme durumu.

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

## <a name="how-azure-monitor-log-data-is-organized"></a>Azure İzleyici günlük verilerini nasıl düzenlenir
Bir sorgu oluşturduğunuzda, aradığınız veri tabloları sahip belirleyerek işe başlayın. Farklı türlerde veri ayrılmış her adanmış tablolara [Log Analytics çalışma alanı](../learn/quick-create-workspace.md).  Farklı veri kaynakları için belge oluşturduğu veri türünün adını ve açıklamasını her birinin özelliklerini içerir.  Çok sayıda sorgu yalnızca tek bir tablodan veri gerektirir ancak diğerleri birden çok tablodan veri eklemek için çeşitli seçenekler kullanabilir.

Sırada [Application Insights](../app/app-insights-overview.md) istekler, özel durumlar, izlemeler ve Azure İzleyici günlüklerine kullanımı gibi uygulama verilerini depolar, bu verileri bir günlük verileri farklı bir bölümden depolanır. Bu verilere erişmek için aynı sorgu dilini kullanın, ancak kullanmalısınız [Application Insights konsol](../app/analytics.md) veya [Application Insights REST API](https://dev.applicationinsights.io/) erişmek için. Kullanabileceğiniz [kaynaklar arası sorgular](../log-query/cross-workspace-query.md) Application Insights verilerini Azure İzleyici'de diğer günlük verileriyle birleştirilecek.


![Tablolar](media/log-query-overview/queries-tables.png)




## <a name="next-steps"></a>Sonraki adımlar
- Kullanma hakkında bilgi edinin [oluşturmak ve düzenlemek için Log Analytics günlük aramaları](../log-query/portals.md).
- Kullanıma bir [sorgu yazmakla ilgili öğretici](../log-query/get-started-queries.md) yeni sorgu diline kullanarak.
