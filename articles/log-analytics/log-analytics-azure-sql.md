---
title: Günlük analizi Azure SQL analizi çözümde | Microsoft Docs
description: Azure SQL analiz çözümü Azure SQL veritabanlarınızın yönetmenize yardımcı olur
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: f57a47677f752a644975a25fa746d78bced5d766
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133121"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Azure SQL Azure SQL analizi (Önizleme) kullanarak veritabanlarını izleme

![Azure SQL analizi simgesi](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL analizi izleme birden çok esnek havuzlar ve abonelikler arasında ölçekte Azure SQL veritabanlarının performansını izleme çözümü bir buluttur. Toplar ve performans üstte sorun giderme için yerleşik destek ile önemli Azure SQL veritabanı performans ölçümleri visualizes. 

Çözümle topladığınız ölçümleri kullanarak özel izleme kurallarını ve uyarıları oluşturabilirsiniz. Çözüm, uygulama yığınının her katmanda sorunları belirlemenize yardımcı olur. Tüm Azure SQL veritabanları ve esnek havuzlar tek bir günlük analizi çalışma alanındaki hakkındaki verileri sunmak için günlük analizi görünümleri yanı sıra Azure tanılama ölçümleri kullanır. Günlük analizi toplamak, bağıntılı ve yapılandırılmış ve yapılandırılmamış verileri görselleştirmek için yardımcı olur.

Şu anda en fazla 150.000 Azure SQL veritabanları ve çalışma alanı başına 5.000 SQL esnek havuzlar Bu önizleme çözümünü destekler.

Gömülü video Azure SQL analiz çözümü kullanarak uygulamalı bir genel bakış ve tipik kullanım senaryoları için bkz:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL analizi çözüm destekleyen tanılama telemetri Azure SQL veritabanları ve esnek havuzlar için akış izleme bir buluttur. Bu aracıları günlük analizi hizmetine bağlanmak için kullanmaz gibi çözüm Windows, Linux bağlanabilirlik desteklemez veya SCOM kaynakları uyumluluk tabloda konusuna bakın.

| Bağlı Kaynak | Destek | Açıklama |
| --- | --- | --- |
| **[Azure tanılama](log-analytics-azure-storage.md)** | **Evet** | Ölçüm ve günlük verilerini Azure günlük analizi için doğrudan Azure tarafından gönderilir. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Günlük analizi depolama hesabından veri okuma değil. |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Doğrudan Windows aracıları çözümü tarafından kullanılmaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Doğrudan Linux aracılarını çözümü tarafından kullanılmaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | Günlük analizi SCOM Aracısı'nı arasında doğrudan bağlantı çözümü tarafından kullanılmaz. |

## <a name="configuration"></a>Yapılandırma

Azure SQL analiz çözümü, çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin.

1. Azure SQL analiz çözümü alanınızdan ekleyin [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. Azure portalında tıklatın **+ kaynak oluşturma**, ardından aramak **Azure SQL analizi**.  
    ![İzleme + Yönetim](./media/log-analytics-azure-sql/monitoring-management.png)
3. Seçin **Azure SQL analizi (Önizleme)** listeden
4. İçinde **Azure SQL analizi (Önizleme)** alanında tıklatın **oluşturma**.  
    ![Oluşturma](./media/log-analytics-azure-sql/portal-create.png)
5. İçinde **yeni çözüm oluşturmak** alan, yeni oluşturun veya ekleyin ve ardından istediğiniz var olan bir çalışma seçin **oluşturma**.  
    ![Çalışma alanıma Ekle](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Azure SQL veritabanları ve esnek havuzlar akış tanılama telemetri için yapılandırın

Azure SQL analiz çözümü çalışma alanınızda oluşturduktan sonra Azure SQL veritabanı ve/veya esnek havuzlar performansını izlemek için gerekir **her yapılandırın** Azure SQL veritabanı ve esnek havuz kaynak istediğiniz Çözüm için kendi tanılama telemetri akışı izlemek için.

- Azure SQL veritabanları ve esnek havuzlar için Azure Tanılama'yı etkinleştirmek ve [için günlük analizi verilerini gönderecek şekilde yapılandırın](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Birden çok Azure aboneliklerini yapılandırmak için

Birden çok abonelik desteklemek için PowerShell komut dosyasını kullanın [PowerShell kullanarak etkinleştirmek Azure kaynak ölçümleri günlük](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Çalışma alanı kaynak kimliği, bir Azure aboneliği kaynaklarında tanılama verilerini başka bir Azure aboneliğine çalışma göndermek için komut dosyası yürütülürken bir parametre olarak sağlayın.

**Örnek**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Çözümü kullanma

Çözüm, çalışma alanına eklediğinizde, Azure SQL analizi döşeme alanınıza eklenir ve genel bakış bölümünde görüntülenir. Döşeme Azure SQL veritabanı ve çözüm bağlandığı Azure SQL esnek havuzlar sayısını gösterir.

![Azure SQL analizi döşeme](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL analizi verileri görüntüleme

Tıklayın **Azure SQL analizi** döşeme Azure SQL analizi panosunu açın. Pano farklı perspektiflerini izlenen tüm veritabanları bakış içerir. İş farklı perspektiflerini için Azure günlük analizi çalışma alanına akışını SQL kaynaklarınız uygun ölçümleri veya günlükleri etkinleştirmelisiniz.

![Azure SQL analizi genel bakış](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Döşeme birini seçerek, belirli bir perspektife ayrıntıya rapor açılır. Perspektif seçildikten sonra ayrıntıya rapor açılır.

![Azure SQL analizi zaman aşımları](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Her bir perspektif abonelik, sunucu, esnek havuz ve veritabanı düzeyi özetleri sağlar. Ayrıca, her bir perspektif bir perspektif sağ tarafta raporu belirli gösterir. Abonelik, sunucu, havuzu veya veritabanı listeden seçerek ayrıntıya devam eder.

| Perspektifi | Açıklama |
| --- | --- |
| Kaynak türüne göre | İzlenen tüm kaynakları sayar perspektif. Ayrıntıya DTU ve GB ölçümleri özetini sağlar. |
| Insights | Hiyerarşik ayrıntıya akıllı fikir sağlar. Akıllı ınsights hakkında daha fazla bilgi edinin. |
| Hatalar | Veritabanlarına oldu SQL hataları içine hiyerarşik ayrıntıya sağlar. |
| Zaman Aşımları | Veritabanlarına oldu SQL zaman aşımları içine hiyerarşik ayrıntıya sağlar. |
| Durdurmalar | Veritabanlarına oldu SQL blockings içine hiyerarşik ayrıntıya sağlar. |
| Veritabanı beklemeleri | SQL bekleme istatistikleri veritabanı düzeyinde içine hiyerarşik ayrıntıya sağlar. Toplam bekleme süresi ve bekleme tür başına bekleme süresi özetlerini içerir. |
| Sorgu süresi | Sorgu süresi, CPU kullanımı, veri g/ç kullanımı, günlük GÇ kullanım gibi sorgu yürütme istatistikleri içine hiyerarşik ayrıntıya sağlar. |
| Sorgu beklemeleri | Hiyerarşik ayrıntıya bekleme kategoriye göre sorgu bekleme istatistikler sağlar. |

### <a name="intelligent-insights-report"></a>Akıllı Öngörüler raporu

Azure SQL veritabanı [akıllı Öngörüler](../sql-database/sql-database-intelligent-insights.md) veritabanı performansınızı bilmeniz olanlar sağlar. Tüm akıllı toplanan Öngörüler görselleştirilen ve Öngörüler perspektif erişilebilir.

![Azure SQL analizi Öngörüler](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Esnek havuz ve veritabanı raporları

Esnek havuzlar ve veritabanları kaynak için belirtilen süre içinde toplanan tüm verileri göster kendi özel raporlara sahip.

![Azure SQL Analizi veritabanı](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL analizi esnek havuzu](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu bekler Perspektifler aracılığıyla, sorgu rapor aracılığıyla herhangi bir sorgu performansını ilişkilendirebilirsiniz. Bu rapor, farklı veritabanlarındaki sorgu performansını karşılaştırır ve iyi yavaş olanları karşı seçili sorgulaması veritabanları sabitleme kolay hale getirir.

![Azure SQL analitik sorguları](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Verileri çözümlemek ve uyarı oluşturma

Kolayca [uyarı oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) ile Azure SQL veritabanı kaynaklardan gelen veriler. İşte bazı yararlı [günlük arama](log-analytics-log-searches.md) kullanabileceğiniz sorgular olan günlük uyarı kullanın:



*Azure SQL veritabanı yüksek DTU*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

*Azure SQL Database esnek havuzunu üzerinde yüksek DTU*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```



## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [günlük aramaları](log-analytics-log-searches.md) ayrıntılı Azure SQL veri görüntülemek için günlük analizi içinde.
- [Kendi panolar oluşturun](log-analytics-dashboards.md) Azure SQL verilerini göstererek.
- [Uyarı oluşturma](log-analytics-alerts.md) belirli Azure SQL olayları olduğunda.
