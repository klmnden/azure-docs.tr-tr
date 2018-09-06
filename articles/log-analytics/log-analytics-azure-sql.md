---
title: Log analytics'te Azure SQL Analytics çözümünü | Microsoft Docs
description: Azure SQL Analytics çözümünü, Azure SQL veritabanlarınızı yönetmenize yardımcı olur.
services: log-analytics
documentationcenter: ''
author: danimir
manager: carmonm
ms.reviewer: carlrab
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: v-daljep
ms.component: na
ms.openlocfilehash: 82845f475857f9a911febd496e86eb2a60f69c25
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782252"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Azure SQL Analytics (Önizleme) kullanarak Azure SQL veritabanlarını izleme

![Azure SQL Analytics simgesi](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL Analytics, izleme çözümü, uygun ölçekte ve birden çok aboneliğe performans Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekleri izlemek için bir buluttur. Toplar ve performans sorunlarını gidermek için yerleşik zeka sayesinde önemli Azure SQL veritabanı performans ölçümleri görselleştirir.

Topladığınız ölçümleri çözümle birlikte kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz. Çözüm, uygulama yığınının her katmanında sorunları tanımlamanıza yardımcı olur. Log Analytics görünümleri yanı sıra Azure tanılama ölçümleri tek bir Log Analytics çalışma alanında yönetilen örnekler tüm, Azure SQL veritabanları, elastik havuzları ve veritabanlarını ilgili verileri sunmak için kullanır. Log Analytics, toplamanıza, ilişkilendirmenize ve yapılandırılmış ve yapılandırılmamış verileri görselleştirmenize yardımcı olur.

Şu anda bu Önizleme çözüm 200.000 Azure SQL veritabanları ve SQL elastik havuzları 5.000 çalışma alanı başına en fazla destekler.

Azure SQL Analytics çözümünü kullanma uygulamalı bir genel bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL Analytics çözümünü destekleyen tanılama telemetrisi Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekler için akış yalnızca izleme bir buluttur. Log Analytics hizmetine bağlanmak için aracıları kullanmaz gibi çözümü değil veya şirket içi SQL Server Vm'leri izlemeyi destekler, uyumluluk tabloya bakın.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| **[Azure tanılama](log-analytics-azure-storage.md)** | **Evet** | Azure ölçüm ve günlük verileri Log Analytics'e doğrudan Azure tarafından gönderilir. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Log Analytics, verileri bir depolama hesabından okumaz. |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Doğrudan Windows aracıları çözüm tarafından kullanılmaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Doğrudan Linux aracıları çözüm tarafından kullanılmaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | SCOM Aracısı'ndan doğrudan bir bağlantı Log analytics'e çözüm tarafından kullanılmaz. |

## <a name="configuration"></a>Yapılandırma

Azure SQL Analytics çözümünü çalışma alanınıza eklemek için aşağıdaki adımları gerçekleştirin.

1. Çalışma alanınızdan Azure SQL Analytics çözümünü ekleyin [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. Azure portalında **+ kaynak Oluştur**, ardından aramak **Azure SQL Analytics**.  
    ![İzleme + Yönetim](./media/log-analytics-azure-sql/monitoring-management.png)
3. Seçin **Azure SQL Analytics (Önizleme)** listesinden
4. İçinde **Azure SQL Analytics (Önizleme)** alanı tıklayın **Oluştur**.  
    ![Oluşturma](./media/log-analytics-azure-sql/portal-create.png)
5. İçinde **yeni çözüm oluşturma** alanında, yeni ya da çözüme ekleyin ve ardından istediğiniz mevcut bir çalışma alanını seçin **Oluştur**.  
    ![çalışma alanına ekleme](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Azure SQL veritabanları ve elastik havuzlar için akış tanılama telemetrisi yapılandırın

Azure SQL Analytics çözümünü çalışma alanınızda oluşturduktan sonra Azure SQL veritabanı ve/veya elastik havuzları performansını izlemek için gerekir **her yapılandırma** Azure SQL veritabanı ve elastik havuz kaynak istediğiniz Çözüm için kendi tanılama telemetrisi akışını izlemek için.

- Azure SQL veritabanları ve elastik havuzlar için Azure Tanılama'yı etkinleştirin ve [verilerini Log Analytics'e göndermek için bunları yapılandırmanız](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Birden çok Azure aboneliği yapılandırmak için

Birden çok abonelik desteklemek için PowerShell betiğini kullanın. [kaynak ölçümleri günlük kaydı etkinleştirmek için Azure PowerShell kullanarak](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Çalışma alanı kaynak kimliği, başka bir Azure aboneliğinde bir çalışma alanı için bir Azure aboneliğinde kaynaklardan Tanılama verileri Gönder için betiği yürütülürken bir parametresi olarak sağlayın.

**Örnek**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Çözümü kullanma

Çözüm çalışma alanınıza eklediğinizde, çalışma alanınızı bir Azure SQL Analytics kutucuk eklenir ve bu genel bakışta görünür. Kutucuğu, Azure SQL veritabanları ve çözüme bağlı olan Azure SQL elastik havuzları sayısını gösterir.

![Azure SQL Analytics kutucuğuna](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics verileri görüntüleme

Tıklayarak **Azure SQL Analytics** kutucuğu, Azure SQL Analytics panosunu açın. Pano farklı perspektiflerini izlenen tüm veritabanlarının genel bakış içerir. Farklı bakış açıları çalışmak SQL kaynaklarınızın Azure Log Analytics çalışma alanına akışla uygun ölçümleri veya günlükleri etkinleştirmelisiniz.

![Azure SQL Analytics genel bakış](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

![Azure SQL Analytics zaman aşımları](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Her bir perspektif, abonelik, sunucu, elastik havuz ve veritabanı düzeyi özetleri sağlar. Ayrıca, her bir perspektif bir perspektif sağ tarafta raporuna özgü gösterir. Abonelik, sunucu, havuz veya veritabanı listeden seçerek detaya gitme devam eder.

| Perspektif | Açıklama |
| --- | --- |
| Kaynak türüne göre | Bu perspektif izlenen tüm kaynakları sayar. Detaya gitme DTU ve GB ölçümleri özetini sağlar. |
| Insights | Hiyerarşik detaya gitme akıllı Öngörüler sağlar. Akıllı içgörüler hakkında daha fazla bilgi edinin. |
| Hatalar | Hiyerarşik detaya gitme veritabanlarında meydana gelen hatalara SQL sağlar. |
| Zaman Aşımları | Hiyerarşik detaya gitme veritabanlarında gerçekleşen SQL zaman aşımları sağlar. |
| Durdurmalar | Hiyerarşik detaya gitme veritabanlarında gerçekleşen SQL blockings sağlar. |
| Veritabanı beklemeleri | Hiyerarşik detaya gitme SQL bekleme istatistikleri veritabanı düzeyi sağlar. Toplam bekleme süresi ve bekleme türü başına bekleme süresi bir özetini içerir. |
| Sorgu süresi | Sorgu yürütme istatistikleri sorgu süresi, CPU kullanımı, veri GÇ kullanımını, günlük GÇ kullanımını gibi hiyerarşik detaya gitme sağlar. |
| Sorgu beklemeleri | Hiyerarşik detaya gitme sorgu bekleme istatistikleri bekleme kategoriye göre sağlar. |

### <a name="intelligent-insights-report"></a>Akıllı İçgörüler raporu

Azure SQL veritabanı [Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) veritabanı performansınızı bilmeniz neler olduğunu sağlar. Toplanan tüm akıllı İçgörüler görselleştirileceğini ve öngörüleri perspektif erişilebilir.

![Azure SQL Analytics öngörüleri](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastik havuz ve veritabanı raporları

Elastik havuzlar ve veritabanları hem kaynak için belirtilen süre içinde toplanan tüm veriler gösteren kendi özel raporlar vardır.

![Azure SQL Analytics veritabanı](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL Analytics elastik havuz](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu bekler Perspektifler sorgu raporu aracılığıyla herhangi bir sorgu performansını ilişkilendirebilirsiniz. Bu rapor, farklı veritabanları arasında sorgu performansını karşılaştırır ve de yavaş olan olanları karşı seçili sorguyu gerçekleştirmek veritabanları saptamak kolaylaştırır.

![Azure SQL Analytics sorguları](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Verileri analiz etmek ve uyarılar oluşturun

Kolayca [uyarıları oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) ile Azure SQL veritabanı kaynaklardan gelen verileri. İşte bazı yararlı [günlük araması](log-analytics-log-searches.md) ile günlük uyarısı kullanabileceğiniz sorgular:

*Azure SQL veritabanı yüksek CPU*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Bu uyarıyı ayarlama ön gereksinim, izlenen veritabanları akışı tanılama ölçümleri ("Tüm ölçümler" seçeneği) çözüme olmasıdır.
> - MetricName değer cpu_percent dtu_consumption_percent yüksek DTU sonuçları yerine elde etmek için'ile değiştirin.

*Azure SQL veritabanı elastik havuzlar yüksek CPU*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Bu uyarıyı ayarlama ön gereksinim, izlenen veritabanları akışı tanılama ölçümleri ("Tüm ölçümler" seçeneği) çözüme olmasıdır.
> - MetricName değer cpu_percent dtu_consumption_percent yüksek DTU sonuçları yerine elde etmek için'ile değiştirin.

*Ortalamanın üstünde %95 son 1 saat içinde Azure SQL veritabanı depolama*

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - Bu uyarıyı ayarlama ön gereksinim, izlenen veritabanları akışı tanılama ölçümleri ("Tüm ölçümler" seçeneği) çözüme olmasıdır.
> - Bu sorgu bir uyarı kuralı sorgudan koşul bazı veritabanlarında bulunduğunu belirten bir uyarı sonuçlar (> 0 sonuç) varken ateşlenmesine kurulu olmasını gerektirir. Çıktı, yukarıda tanımlanan time_range içinde storage_threshold olan veritabanı kaynakların listesidir.
> - Çıktı, yukarıda tanımlanan time_range içinde storage_threshold olan veritabanı kaynakların listesidir.

*Akıllı Öngörüler uyar*

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - Bu uyarıyı ayarlama ön gereksinim, izlenen veritabanları SQLInsights tanılama günlüğünün akışını çözümü olmasıdır.
> - Bu sorgu, yinelenen sonuçlar önlemek için aynı sıklıkta alert_run_interval olarak çalıştırılmak üzere ayarlanması için bir uyarı kuralı gerektirir. Sonuçlar (> 0 sonuç) bulunduğunda uyarıyı sorgudan ateşlenmesine kural ayarlanmış olması.
> - Koşul, çözüm SQLInsights günlüğünün akışını için yapılandırılmış veritabanları üzerinde oluşup olmadığını denetlemek için zaman aralığını belirtmek için alert_run_interval özelleştirin.
> - Insights kök neden analizi metin çıktısını yakalamak için insights_string özelleştirin. Mevcut ınsights'tan kullanabileceğiniz çözüm kullanıcı arabiriminde görüntülenen metnin budur. Alternatif olarak, tüm öngörü aboneliğinizde oluşturulan metin görmek için aşağıdaki sorguyu kullanabilirsiniz. Insights uyarıları ayarlamak için ayrı dizeleri Hasat için sorgunun çıkışı kullanın.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| distinct rootCauseAnalysis_s
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [günlük aramaları](log-analytics-log-searches.md) ayrıntılı Azure SQL veri görüntülemek için Log analytics'te.
- [Kendi panolarınızı oluşturun](log-analytics-dashboards.md) Azure SQL veri gösteriliyor.
- [Uyarı oluşturma](log-analytics-alerts.md) belirli bir Azure SQL olaylar gerçekleştiğinde.
