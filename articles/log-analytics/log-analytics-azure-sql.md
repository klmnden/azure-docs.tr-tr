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
ms.openlocfilehash: b7a7e2787128c74cd7d016c01b751d15628fb4b2
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182000"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Azure SQL Analytics (Önizleme) kullanarak Azure SQL veritabanı izleme

![Azure SQL Analytics simgesi](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL Analytics, izleme çözümü, uygun ölçekte ve birden çok aboneliğe performans Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekleri izlemek için bir buluttur. Toplar ve performans sorunlarını gidermek için yerleşik zeka sayesinde önemli Azure SQL veritabanı performans ölçümleri görselleştirir.

Topladığınız ölçümleri çözümle birlikte kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz. Çözüm, uygulama yığınının her katmanında sorunları tanımlamanıza yardımcı olur. Log Analytics görünümleri yanı sıra Azure tanılama ölçümleri tek bir Log Analytics çalışma alanında yönetilen örnekler tüm, Azure SQL veritabanları, elastik havuzları ve veritabanlarını ilgili verileri sunmak için kullanır. Log Analytics, toplamanıza, ilişkilendirmenize ve yapılandırılmış ve yapılandırılmamış verileri görselleştirmenize yardımcı olur.

Azure SQL Analytics çözümünü kullanma uygulamalı bir genel bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL Analytics çözümünü destekleyen tanılama telemetrisi Azure SQL veritabanı, yönetilen örnek veritabanları ve elastik havuzlar için akış yalnızca izleme bir buluttur.

Çözüm, Log Analytics hizmetine bağlanmak için aracıları kullanmaz gibi çözüm barındırılan SQL Server şirket içi veya sanal makineleri izleme desteği değil, uyumluluk tabloya bakın.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| **[Azure tanılama](log-analytics-azure-storage.md)** | **Evet** | Azure ölçüm ve günlük verileri Log Analytics'e doğrudan Azure tarafından gönderilir. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Log Analytics, verileri bir depolama hesabından okumaz. |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Doğrudan Windows aracıları çözüm tarafından kullanılmaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Doğrudan Linux aracıları çözüm tarafından kullanılmaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | SCOM Aracısı'ndan doğrudan bir bağlantı Log analytics'e çözüm tarafından kullanılmaz. |

## <a name="configuration"></a>Yapılandırma

Azure SQL Analytics çözümünü Azure panonuza eklemek için aşağıdaki adımları gerçekleştirin.

1. Çalışma alanınızdan Azure SQL Analytics çözümünü ekleyin [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. Azure portalında **+ kaynak Oluştur**, ardından aramak **Azure SQL Analytics**.  
    ![İzleme + Yönetim](./media/log-analytics-azure-sql/monitoring-management.png)
3. Seçin **Azure SQL Analytics (Önizleme)** listesinden
4. İçinde **Azure SQL Analytics (Önizleme)** alanı tıklayın **Oluştur**.  
    ![Oluşturma](./media/log-analytics-azure-sql/portal-create.png)
5. İçinde **yeni çözüm oluşturma** alanında, yeni ya da çözüme ekleyin ve ardından istediğiniz mevcut bir çalışma alanını seçin **Oluştur**.

    ![çalışma alanına ekleme](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekler akışı tanılama telemetrisi için yapılandırma

Azure SQL Analytics çözümünü çalışma alanınızda oluşturduktan sonra Azure SQL veritabanı yönetilen örnek veritabanları ve elastik havuzlar performansını izlemek için gerekir **her yapılandırma** istediğiniz bu kaynaklar Çözüm için kendi tanılama telemetrisi akışını izleyin.

- Azure SQL veritabanı, yönetilen örnek veritabanları ve elastik havuzlar için Azure tanılamayı etkinleştirerek [Azure SQL Analytics için tanılama telemetrisi akışını](../sql-database/sql-database-metrics-diag-logging.md).

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

Çözüm çalışma alanınıza eklediğinizde, çalışma alanınızı bir Azure SQL Analytics kutucuk eklenir ve bu genel bakışta görünür. Kutucuk, çözüm tanılama telemetrisini alan yönetilen örneğe Azure SQL veritabanları, elastik havuzlar, yönetilen örnekler ve veritabanlarının sayısını gösterir.

![Azure SQL Analytics kutucuğuna](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

Çözüm--bir Azure SQL veritabanları ve elastik havuzlar ve diğer görünüm yönetilen örneği, veritabanlarını ve yönetilen örnekleri izlemek için izleme için iki ayrı görünümler sağlar.

Azure SQL veritabanları ve elastik havuzlar için Azure SQL Analytics izleme panosunu görüntülemek için kutucuğun üst kısmında tıklayın. Azure SQL yönetilen örneği ve yönetilen örnek veritabanları için izleme Panosu Analytics görüntülemek için kutucuğun alt bölümünü üzerinde tıklayın.

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics verileri görüntüleme

Pano farklı perspektiflerini izlenen tüm veritabanlarının genel bakış içerir. Farklı bakış açıları çalışmak SQL kaynaklarınızın Azure Log Analytics çalışma alanına akışla uygun ölçümleri veya günlükleri etkinleştirmelisiniz.

Bazı ölçümler veya günlüklerini Azure Log Analytics'e aktarılır değil, çözümdeki kutucukları izleme bilgilerini doldurulmaz olduğunu lütfen unutmayın.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Azure SQL veritabanı ve elastik havuz görüntüleyin

Azure SQL veritabanı için Azure SQL Analytics kutucuğuna ve elastik havuzlar seçili sonra izleme Panosu gösterilmektedir.

![Azure SQL Analytics genel bakış](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

![Azure SQL Analytics zaman aşımları](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Bu görünümde her bir perspektif, abonelik, sunucu, elastik havuz ve veritabanı düzeyi özetleri sağlar. Ayrıca, her bir perspektif bir perspektif sağ tarafta raporuna özgü gösterir. Abonelik, sunucu, havuz veya veritabanı listeden seçerek detaya gitme devam eder.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>Yönetilen örnek ve yönetilen örnek veritabanları görüntüleyin

Yönetilen örnek için Azure SQL Analytics kutucuğuna ve yönetilen Instnace veritabanları seçili sonra izleme Panosu gösterilmektedir.

![Azure SQL Analytics genel bakış](./media/log-analytics-azure-sql/azure-sql-sol-overview-mi.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

Yönetilen örnek görünümü seçerek, yönetilen örneği kullanımı, içerdiği veritabanları ve telemetri örneğinde yürütülen sorguları ayrıntıları gösterir.

![Azure SQL Analytics zaman aşımları](./media/log-analytics-azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="perspectives"></a>Perspektifler

Aşağıdaki tabloda iki Pano, bir Azure SQL veritabanı ve elastik havuzlar için ve yönetilen örneği için başka bir sürümü için desteklenen Perspektifler özetlenmektedir.

| Perspektif | Açıklama | SQL veritabanı ve elastik havuzlar desteği | Yönetilen örnek destek |
| --- | ------- | ----- | ----- |
| Kaynak türüne göre | Bu perspektif izlenen tüm kaynakları sayar. | Evet | Evet | 
| Insights | Hiyerarşik detaya gitme performans akıllı Öngörüler sağlar. | Evet | Evet |
| Hatalar | Hiyerarşik detaya gitme veritabanlarında meydana gelen hatalara SQL sağlar. | Evet | Evet |
| Zaman Aşımları | Hiyerarşik detaya gitme veritabanlarında gerçekleşen SQL zaman aşımları sağlar. | Evet | Hayır |
| Durdurmalar | Hiyerarşik detaya gitme veritabanlarında gerçekleşen SQL blockings sağlar. | Evet | Hayır |
| Veritabanı beklemeleri | Hiyerarşik detaya gitme SQL bekleme istatistikleri veritabanı düzeyi sağlar. Toplam bekleme süresi ve bekleme türü başına bekleme süresi bir özetini içerir. |Evet | Evet |
| Sorgu süresi | Sorgu yürütme istatistikleri sorgu süresi, CPU kullanımı, veri GÇ kullanımını, günlük GÇ kullanımını gibi hiyerarşik detaya gitme sağlar. | Evet | Evet |
| Sorgu beklemeleri | Hiyerarşik detaya gitme sorgu bekleme istatistikleri bekleme kategoriye göre sağlar. | Evet | Evet |

### <a name="intelligent-insights-report"></a>Akıllı İçgörüler raporu

Azure SQL veritabanı [Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) ile Azure SQL veritabanları ve yönetilen örnek veritabanlarının performansını neler olduğunu bildiğiniz sağlar. Toplanan tüm akıllı İçgörüler görselleştirileceğini ve öngörüleri perspektif erişilebilir.

![Azure SQL Analytics öngörüleri](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastik havuz ve veritabanı raporları

SQL veritabanlarının ve elastik havuzlar kaynak için belirtilen süre içinde toplanan tüm verileri göstermek belirli kendi raporlarını vardır.

![Azure SQL Analytics veritabanı](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL esnek havuzu](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu bekler Perspektifler sorgu raporu aracılığıyla herhangi bir sorgu performansını ilişkilendirebilirsiniz. Bu rapor, farklı veritabanları arasında sorgu performansını karşılaştırır ve de yavaş olan olanları karşı seçili sorguyu gerçekleştirmek veritabanları saptamak kolaylaştırır.

![Azure SQL Analytics sorguları](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Verileri analiz etmek ve uyarılar oluşturun

### <a name="creating-alerts-for-azure-sql-database"></a>Uyarılar için Azure SQL veritabanı oluşturma

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

### <a name="creating-alerts-for-managed-instance"></a>Yönetilen örnek için uyarıları oluşturma

* % 90'yönetilen Örnek Depolama

```
let storage_percentage_treshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_treshold
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın öncesi gereksinimdir izlenen yönetilen örneği olduğunu çözümü etkin ResourceUsageStats günlük akış.
> - Bu sorgu, bir uyarı kuralı koşul yönetilen örneği'nde var olduğunu belirten sorgudan gelen sonuçlar (> 0 sonuç) varsa bir uyarı ateşlenmesine kurulu olmasını gerektirir. Çıkış depolama yüzdesi tüketim yönetilen örneği ' dir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [günlük aramaları](log-analytics-log-searches.md) ayrıntılı Azure SQL veri görüntülemek için Log analytics'te.
- [Kendi panolarınızı oluşturun](log-analytics-dashboards.md) Azure SQL veri gösteriliyor.
- [Uyarı oluşturma](log-analytics-alerts.md) belirli bir Azure SQL olaylar gerçekleştiğinde.
