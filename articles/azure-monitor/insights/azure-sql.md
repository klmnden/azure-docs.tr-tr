---
title: Azure İzleyici'de Azure SQL Analytics çözümünü | Microsoft Docs
description: Azure SQL Analytics çözümünü, Azure SQL veritabanlarınızı yönetmenize yardımcı olur.
services: log-analytics
ms.service: log-analytics
ms.custom: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: carlrab
manager: craigg
ms.date: 12/17/2018
ms.openlocfilehash: c68c278b2a7afa8287845c452e3bec5380cf05c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60498612"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Azure SQL Analytics (Önizleme) kullanarak Azure SQL veritabanı izleme

![Azure SQL Analytics simgesi](./media/azure-sql/azure-sql-symbol.png)

Azure SQL Analytics, izleme çözümü, uygun ölçekte ve birden çok aboneliği tek bir bölme cam arasında Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekler performansı izleme için Gelişmiş bir buluttur. Toplar ve performans sorunlarını gidermek için yerleşik zeka sayesinde önemli Azure SQL veritabanı performans ölçümleri görselleştirir.

Topladığınız ölçümleri çözümle birlikte kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz. Çözüm, uygulama yığınının her katmanında sorunları tanımlamanıza yardımcı olur. Azure İzleyici görünümleri yanı sıra Azure tanılama ölçümleri tek bir Log Analytics çalışma alanında yönetilen örnekler tüm, Azure SQL veritabanları, elastik havuzları ve veritabanlarını ilgili verileri sunmak için kullanır. Azure İzleyici, toplamanıza, ilişkilendirmenize ve yapılandırılmış ve yapılandırılmamış verileri görselleştirmenize yardımcı olur.

Azure SQL Analytics çözümünü kullanma uygulamalı bir genel bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:

>[!VIDEO https://www.youtube.com/embed/j-NDkN4GIzg]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL Analytics, bulut çözümü, Azure SQL veritabanları için tanılama telemetrisi destekleyen akış izleme yalnızca: tek, havuza alınmış ve yönetilen örnek veritabanları. Çözüm, Azure İzleyici bağlanmak için aracıları kullanmaz gibi çözüm barındırılan SQL Server şirket içi veya sanal makineleri izleme desteği değil, uyumluluk tabloya bakın.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Azure Tanılama](../platform/collect-azure-metrics-logs.md) | **Evet** | Azure İzleyici günlüklerine, Azure ölçüm ve günlük verileri doğrudan Azure tarafından gönderilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure İzleyici, verileri bir depolama hesabından okumaz. |
| [Windows aracıları](../platform/agent-windows.md) | Hayır | Doğrudan Windows aracıları çözüm tarafından kullanılmaz. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Doğrudan Linux aracıları çözüm tarafından kullanılmaz. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Hayır | Azure İzleyici Operations Manager Aracısı'ndan doğrudan bağlantı çözümü tarafından kullanılmaz. |

## <a name="configuration"></a>Yapılandırma
Açıklanan işlemi kullanın [Azure İzleyici'yi ekleyin çözüm galeri'sinden](../../azure-monitor/insights/solutions.md) Azure SQL Analytics (Önizleme) çözümü Log Analytics çalışma alanınıza eklemek için.

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekler akışı tanılama telemetrisi için yapılandırma

Azure SQL Analytics çözümünü çalışma alanınızda oluşturduktan sonra yapmanız **her yapılandırma** çözümü için kendi tanılama telemetrisi akışını izlemek istediğiniz kaynakları. Bu sayfada ayrıntılı yönergeleri izleyin:

- Azure SQL veritabanınızın Azure tanılamayı etkinleştirerek [Azure SQL Analytics için tanılama telemetrisi akışını](../../sql-database/sql-database-metrics-diag-logging.md).

Yukarıdaki sayfayı ayrıca tek bir Azure SQL Analytics çalışma alanı birden çok Azure aboneliklerinde tek bir cam bölmeyle izlemek için destek etkinleştirme hakkında yönergeler sağlar.

## <a name="using-the-solution"></a>Çözümü kullanma

Çözüm çalışma alanınıza eklediğinizde, çalışma alanınızı bir Azure SQL Analytics kutucuk eklenir ve bu genel bakışta görünür. Kutucuk içeriği yüklemek için Özet Görüntüle bağlantıyı seçin.

![Azure SQL Analytics Özet kutucuğu](./media/azure-sql/azure-sql-sol-tile-01.png)

Veriler yüklendikten sonra kutucuğu çözüm tanılama telemetrisini alan yönetilen örneğe Azure SQL veritabanları, elastik havuzlar, yönetilen örnekler ve veritabanlarının sayısını gösterir.

![Azure SQL Analytics kutucuğuna](./media/azure-sql/azure-sql-sol-tile-02.png)

Çözüm--bir Azure SQL veritabanları ve elastik havuzlar ve diğer görünüm yönetilen örneği, veritabanlarını ve yönetilen örnekleri izlemek için izleme için iki ayrı görünümler sağlar.

Azure SQL veritabanları ve elastik havuzlar için Azure SQL Analytics izleme panosunu görüntülemek için kutucuğun üst kısmında tıklayın. Azure SQL yönetilen örneği ve yönetilen örnek veritabanları için izleme Panosu Analytics görüntülemek için kutucuğun alt bölümünü üzerinde tıklayın.

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics verileri görüntüleme

Pano farklı perspektiflerini izlenen tüm veritabanlarının genel bakış içerir. Farklı bakış açıları çalışmak SQL kaynaklarınızın Log Analytics çalışma alanına akışla uygun ölçümleri veya günlükleri etkinleştirmelisiniz.

Azure İzleyici ile bazı ölçümler veya günlüklerini akışla değil, çözümdeki kutucukları izleme bilgilerini doldurulmaz olduğunu unutmayın.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Azure SQL veritabanı ve elastik havuz görüntüleyin

Veritabanını Azure SQL Analytics kutucuğuna seçildikten sonra izleme Panosu gösterilmektedir.

![Azure SQL Analytics genel bakış](./media/azure-sql/azure-sql-sol-overview.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

![Azure SQL Analytics zaman aşımları](./media/azure-sql/azure-sql-sol-metrics.png)

Bu görünümde her bir perspektif, abonelik, sunucu, elastik havuz ve veritabanı düzeyi özetleri sağlar. Ayrıca, her bir perspektif bir perspektif sağ tarafta raporuna özgü gösterir. Abonelik, sunucu, havuz veya veritabanı listeden seçerek detaya gitme devam eder.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>Yönetilen örnek ve yönetilen örnek veritabanları görüntüleyin

Veritabanlarını Azure SQL Analytics kutucuğuna seçildikten sonra izleme Panosu gösterilmektedir.

![Azure SQL Analytics genel bakış](./media/azure-sql/azure-sql-sol-overview-mi.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

Yönetilen örnek görünümü seçerek, yönetilen örneği kullanımı, içerdiği veritabanları ve telemetri örneğinde yürütülen sorguları ayrıntıları gösterir.

![Azure SQL Analytics zaman aşımları](./media/azure-sql/azure-sql-sol-metrics-mi.png)

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

Azure SQL veritabanı [Intelligent Insights](../../sql-database/sql-database-intelligent-insights.md) performans tüm Azure SQL veritabanı ile neler olduğunu bildiğiniz sağlar. Toplanan tüm akıllı İçgörüler görselleştirileceğini ve öngörüleri perspektif erişilebilir.

![Azure SQL Analytics öngörüleri](./media/azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastik havuz ve veritabanı raporları

SQL veritabanlarının ve elastik havuzlar kaynak için belirtilen süre içinde toplanan tüm verileri göstermek belirli kendi raporlarını vardır.

![Azure SQL Analytics veritabanı](./media/azure-sql/azure-sql-sol-database.png)

![Azure SQL esnek havuzu](./media/azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu bekler Perspektifler sorgu raporu aracılığıyla herhangi bir sorgu performansını ilişkilendirebilirsiniz. Bu rapor, farklı veritabanları arasında sorgu performansını karşılaştırır ve de yavaş olan olanları karşı seçili sorguyu gerçekleştirmek veritabanları saptamak kolaylaştırır.

![Azure SQL Analytics sorguları](./media/azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>İzinler

Azure SQL Analytics kullanmak için kullanıcıların azure'daki okuyucu rolünün en az bir izin verilmesi gerekir. Bu rol, ancak değil sorgu metni görmesine olanak veya tüm otomatik ayarlama eylemleri gerçekleştirin. Tam ölçüde çözümünü sağlayan daha esnek azure'da sahibi, katkıda bulunan, SQL DB Katılımcısı veya SQL Server Katılımcısı rolleridir. Yalnızca Azure SQL Analytics kullanmak için gereken belirli izinleri ile ve diğer kaynakları yönetmek için erişim olmaksızın Portalı'nda özel rol oluşturma düşünmek isteyebilirsiniz.

### <a name="creating-a-custom-role-in-portal"></a>Portalda özel rol oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bazı kuruluşların azure'daki katı izin denetimleri zorunlu tanıma, en düşük Azure portalıyla "SQL Analytics izleme operatörü" okuma ve yazma izinleri için gereken özel bir rol oluşturulmasını etkinleştirme aşağıdaki PowerShell Betiği bulma Azure SQL Analytics tam boyutuna kullanın.

"{Subscriptionıd}" Değiştir aşağıdaki betiği, Azure abonelik kimliği ile bir Azure sahibi veya katkıda bulunan rolü olarak günlüğe bu betiği yürütün.

   ```powershell
    Connect-AzAccount
    Select-AzSubscription {SubscriptionId}
    $role = Get-AzRoleDefinition -Name Reader
    $role.Name = "SQL Analytics Monitoring Operator"
    $role.Description = "Lets you monitor database performance with Azure SQL Analytics as a reader. Does not allow change of resources."
    $role.IsCustom = $true
    $role.Actions.Add("Microsoft.SQL/servers/databases/read");
    $role.Actions.Add("Microsoft.SQL/servers/databases/topQueries/queryText/*");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/*");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Resources/deployments/write");
    $role.AssignableScopes = "/subscriptions/{SubscriptionId}"
    New-AzRoleDefinition $role
   ```

Yeni rol oluşturulduktan sonra Azure SQL Analytics kullanmak için özel izinleri vermek için gereken her kullanıcıya bu rolü atayın.

## <a name="analyze-data-and-create-alerts"></a>Verileri analiz etmek ve uyarılar oluşturun

Azure SQL Analytics veri analizi temel [Log Analytics dilini](../log-query/get-started-queries.md) özel sorgulama ve raporlama için. Kullanılabilir veri bulma açıklamasını toplanan özel sorgulayan için veritabanı kaynaktan [ölçümlerini ve günlüklerini kullanılabilir](../../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available).

Koşul sonrasında bir uyarı tetikleyen bir Log Analytics sorgusu karşılanıyor yazmak tabanlı otomatik çözümde uyarı. Log Analytics sorgularını birkaç örneği aşağıda hangi uyarı üzerine çözümde ayarlanabilir bulun.

### <a name="creating-alerts-for-azure-sql-database"></a>Uyarılar için Azure SQL veritabanı oluşturma

Kolayca [uyarıları oluşturma](../platform/alerts-metric.md) ile Azure SQL veritabanı kaynaklardan gelen verileri. İşte bazı yararlı [oturum sorguları](../log-query/log-query-overview.md) ile günlük uyarısı kullanabileceğiniz:

#### <a name="high-cpu-on-azure-sql-database"></a>Azure SQL veritabanı yüksek CPU

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

#### <a name="high-cpu-on-azure-sql-database-elastic-pools"></a>Azure SQL veritabanı elastik havuzlar yüksek CPU

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

#### <a name="azure-sql-database-storage-in-average-above-95-in-the-last-1-hr"></a>Ortalamanın üstünde %95 son 1 saat içinde Azure SQL veritabanı depolama

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

#### <a name="alert-on-intelligent-insights"></a>Akıllı Öngörüler uyar

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

#### <a name="managed-instance-storage-is-above-90"></a>Yönetilen Örnek Depolama % 90'tır.

```
let storage_percentage_threshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_threshold
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gereksinim izlenen yönetilen örneği çözümü etkin ResourceUsageStats günlük akış sahip olur.
> - Bu sorgu, bir uyarı kuralı koşul yönetilen örneği'nde var olduğunu belirten sorgudan gelen sonuçlar (> 0 sonuç) varsa bir uyarı ateşlenmesine kurulu olmasını gerektirir. Çıkış depolama yüzdesi tüketim yönetilen örneği ' dir.

#### <a name="managed-instance-cpu-average-consumption-is-above-95-in-the-last-1-hr"></a>Yönetilen örnek CPU ortalama tüketim %95 son 1 saat içinde olduğu

```
let cpu_percentage_threshold = 95;
let time_threshold = ago(1h);
AzureDiagnostics
| where Category == "ResourceUsageStats" and TimeGenerated > time_threshold
| summarize avg_cpu = max(todouble(avg_cpu_percent_s)) by ResourceId
| where avg_cpu > cpu_percentage_threshold
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gereksinim izlenen yönetilen örneği çözümü etkin ResourceUsageStats günlük akış sahip olur.
> - Bu sorgu, bir uyarı kuralı koşul yönetilen örneği'nde var olduğunu belirten sorgudan gelen sonuçlar (> 0 sonuç) varsa bir uyarı ateşlenmesine kurulu olmasını gerektirir. Çıkış ortalama CPU kullanımı yüzde tüketimini tanımlanan süre içinde yönetilen örneği ' dir.

### <a name="pricing"></a>Fiyatlandırma

Çözümü ücretsiz olsa da, veri alımı ayrılan her ay ücretsiz birimlerinin yukarıda tanılama telemetrisi tüketiminin uygular, bkz: [Log Analytics fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/monitor). Sağlanan veri alımı ücretsiz birimlerinin ücretsiz çeşitli veritabanları her ay izlemeyi etkinleştirin. Daha ağır iş yükleri daha etkin veritabanlarıyla boştaki veritabanlarının karşı daha fazla veri alma olduğunu unutmayın. Veri alımı tüketiminiz çözümünde, OMS çalışma alanı Azure SQL Analytics Gezinti menüsünde ve ardından kullanım ve Tahmini maliyetler seçerek kolayca izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [oturum sorguları](../log-query/log-query-overview.md) ayrıntılı Azure SQL veri görüntülemek için Azure İzleyici'de.
- [Kendi panolarınızı oluşturun](../learn/tutorial-logs-dashboards.md) Azure SQL veri gösteriliyor.
- [Uyarı oluşturma](../platform/alerts-overview.md) belirli bir Azure SQL olaylar gerçekleştiğinde.
