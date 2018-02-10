---
title: "Günlük analizi Azure SQL analizi çözümde | Microsoft Docs"
description: "Azure SQL analiz çözümü Azure SQL veritabanlarınızın yönetmenize yardımcı olur."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte;banders
ms.openlocfilehash: 2a363f663677eb7078b7ae06fde374cdbe083fd5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Azure SQL veritabanı günlük analizi Azure SQL analizi (Önizleme) kullanarak izleme

![Azure SQL analizi simgesi](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure günlük analizi Azure SQL analizi çözümde toplar ve önemli SQL Azure performans ölçümleri visualizes. Çözümle topladığınız ölçümleri kullanarak özel izleme kurallarını ve uyarıları oluşturabilirsiniz. Azure SQL veritabanı izleyebilirsiniz ve esnek havuz ölçümler arasında birden çok Azure abonelikleri ve esnek havuzları ve bunları görselleştirin. Çözüm Ayrıca, uygulama yığınının her katmanda sorunlarını belirlemenize yardımcı olur.  Kullandığı [Azure tanılama ölçümleri](log-analytics-azure-storage.md) tüm Azure SQL veritabanları ve esnek havuzlar tek bir günlük analizi çalışma alanındaki hakkındaki verileri sunmak için günlük analizi görünümleri ile birlikte.

Şu anda en fazla 150.000 Azure SQL veritabanları ve çalışma alanı başına 5.000 SQL esnek havuzlar Bu önizleme çözümünü destekler.

Günlük analizi için bulunan diğerleri gibi Azure SQL analiz çözümü izlemenize ve Azure kaynaklarınızı durumu hakkında bildirim almak yardımcı olur; bu durumda, Azure SQL veritabanı. Microsoft Azure SQL veritabanı Azure bulutta çalışan uygulamalar için tanıdık SQL Server gibi özellikleri sağlayan bir ölçeklenebilir ilişkisel veritabanı hizmetidir. Günlük analizi toplamak, bağıntılı ve yapılandırılmış ve yapılandırılmamış verileri görselleştirmek için yardımcı olur.

Gömülü video Azure SQL analiz çözümü kullanarak uygulamalı bir genel bakış ve tipik kullanım senaryoları için bkz:
          
> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL analiz çözümü aracılarını günlük analizi hizmetine bağlanmak için kullanmaz.

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
| --- | --- | --- |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Doğrudan Windows aracıları çözümü tarafından kullanılmaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Doğrudan Linux aracılarını çözümü tarafından kullanılmaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | Günlük analizi SCOM Aracısı'nı arasında doğrudan bağlantı çözümü tarafından kullanılmaz. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Günlük analizi depolama hesabından veri okuma değil. |
| [Azure Tanılama](log-analytics-azure-storage.md) | Evet | Azure ölçüm ve günlük verileri için günlük analizi doğrudan Azure tarafından gönderilir. |

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Yoksa, için bir tane oluşturabilirsiniz [ücretsiz](https://azure.microsoft.com/free/).
- Günlük analizi çalışma alanı. Mevcut bir kullanabilir veya yapabilecekleriniz [yeni bir tane oluşturun](log-analytics-quick-create-workspace.md) Bu çözüm kullanmaya başlamadan önce.
- Azure SQL veritabanları ve esnek havuzlar için Azure Tanılama'yı etkinleştirmek ve [için günlük analizi verilerini gönderecek şekilde yapılandırın](../sql-database/sql-database-metrics-diag-logging.md).

## <a name="configuration"></a>Yapılandırma

Azure SQL analiz çözümü, çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin.

1. Azure SQL analiz çözümü alanınızdan ekleyin [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. Azure portalında tıklatın **yeni** (+ simgesi), ardından kaynak listesinde seçin **izleme + Yönetim**.  
    ![İzleme + Yönetim](./media/log-analytics-azure-sql/monitoring-management.png)
3. İçinde **izleme + Yönetim** listesine **tümünü görmek**.
4. İçinde **önerilen** tıklatın **daha fazla** ve ardından yeni liste bulmak **Azure SQL analizi (Önizleme)** ve seçin.  
    ![Azure SQL analiz çözümü](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. İçinde **Azure SQL analizi (Önizleme)** dikey penceresinde tıklatın **oluşturma**.  
    ![Oluşturma](./media/log-analytics-azure-sql/portal-create.png)
6. İçinde **yeni çözüm oluşturmak** dikey penceresinde, çözüme eklemek istediğiniz çalışma alanını seçin ve ardından **oluşturma**.  
    ![Çalışma alanıma Ekle](./media/log-analytics-azure-sql/add-to-workspace.png)


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

>[!NOTE]
> Azure SQL analizi en son sürümünü almak için günlük analizi yükseltin.
>

Çözüm, çalışma alanına eklediğinizde, Azure SQL analizi döşeme alanınıza eklenir ve genel bakış bölümünde görüntülenir. Döşeme Azure SQL veritabanı ve çözüm bağlandığı Azure SQL esnek havuzlar sayısını gösterir.

![Azure SQL analizi döşeme](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL analizi verileri görüntüleme

Tıklayın **Azure SQL analizi** döşeme Azure SQL analizi panosunu açın. Pano farklı perspektiflerini izlenen tüm veritabanları bakış içerir. İş farklı perspektiflerini için Azure günlük analizi çalışma alanına akışını SQL kaynaklarınız uygun ölçümleri veya günlükleri etkinleştirmelisiniz. 

![Azure SQL analizi genel bakış](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Döşeme birini seçerek, belirli bir perspektife ayrıntıya rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

![Azure SQL analizi zaman aşımları](./media/log-analytics-azure-sql/azure-sql-sol-timeouts.png)

Her bir perspektif abonelik, sunucu, esnek havuz ve veritabanı düzeyi özetleri sağlar. Ayrıca, her bir perspektif sağ tarafta perspektif belirli rapor gösterir. Abonelik, sunucu, havuzu veya veritabanı listeden seçerek aşağı ayrıntıya devam eder.

| Perspektifi | Açıklama |
| --- | --- |
| Kaynak türüne göre | İzlenen tüm kaynakları sayar perspektif. Ayrıntıya DTU ve GB ölçümleri özetini sağlar. |
| Insights | Hiyerarşik ayrıntıya akıllı fikir sağlar. Akıllı ınsights hakkında daha fazla bilgi edinin. |
| Hatalar | Veritabanlarına oldu SQL hataları içine hiyerarşik ayrıntıya sağlar. |
| Zaman aşımları | Veritabanlarına oldu SQL zaman aşımları içine hiyerarşik ayrıntıya sağlar. |
| Blockings | Veritabanlarına oldu SQL blockings içine hiyerarşik ayrıntıya sağlar. |
| Veritabanı bekler | SQL bekleme istatistikleri veritabanı düzeyinde içine hiyerarşik ayrıntıya sağlar. Toplam bekleme süresi ve bekleme tür başına bekleme süresi özetlerini içerir. |
| Sorgu süresi | Sorgu süresi, CPU kullanımı, veri g/ç kullanımı, günlük GÇ kullanım gibi sorgu yürütme istatistikleri içine hiyerarşik ayrıntıya sağlar. |
| Sorgu bekler | Hiyerarşik ayrıntıya bekleme kategoriye göre sorgu bekleme istatistikler sağlar. |

### <a name="intelligent-insights-report"></a>Akıllı Öngörüler raporu

Azure SQL veritabanı [akıllı Öngörüler](../sql-database/sql-database-intelligent-insights.md) veritabanı performansınızı bilmeniz olanlar sağlar. Tüm akıllı toplanan Öngörüler görselleştirilen ve Öngörüler perspektif erişilebilir.

![Azure SQL analizi Öngörüler](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Esnek havuz ve veritabanı raporları

Esnek havuzlar ve veritabanları kaynak için belirtilen süre içinde toplanan tüm verileri göster kendi özel raporlara sahip.

![Azure SQL Analizi veritabanı](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL analizi esnek havuzu](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu bekler perspektif aracılığıyla, sorgu rapor aracılığıyla herhangi bir sorgu performansını ilişkilendirebilirsiniz. Bu rapor, farklı veritabanlarındaki sorgu performansını karşılaştırır ve iyi yavaş olanları karşı seçili sorgulaması veritabanları sabitleme kolay hale getirir.

![Azure SQL analitik sorguları](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Verileri çözümlemek ve uyarı oluşturma

Uyarılar, Azure SQL veritabanı kaynaklardan gelen veriler ile kolayca oluşturabilirsiniz. İşte birkaç faydalı [günlük arama](log-analytics-log-searches.md) uyarmak için kullanabileceğiniz sorgular:

[!INCLUDE[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Azure SQL veritabanı yüksek DTU*

```
AzureMetrics | where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" | summarize avg(Maximum) by ResourceId
```

*Azure SQL Database esnek havuzunu üzerinde yüksek DTU*

```
AzureMetrics | where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" | summarize avg(Maximum) by ResourceId
```

Bu uyarı tabanlı sorgular, Azure SQL veritabanı ve esnek havuzlar için belirli eşiklere uyarmak için kullanabilirsiniz. Günlük analizi çalışma alanınız için bir uyarı yapılandırmak için:

#### <a name="to-configure-an-alert-for-your-workspace"></a>Çalışma alanınız için bir uyarı yapılandırmak için

1. Git [OMS portalı](http://mms.microsoft.com/) ve oturum açın.
2. Çözüm için yapılandırdığınız çalışma alanını açın.
3. Genel bakış sayfasında, tıklatın **Azure SQL analizi (Önizleme)** döşeme.
4. Örnek sorgular birini çalıştırın.
5. Günlük aramada tıklatın **uyarı**.  
![Aramada uyarısı oluştur](./media/log-analytics-azure-sql/create-alert01.png)
6. Üzerinde **uyarı kuralı Ekle** sayfasında, uygun özellikleri ve istediğiniz ve ardından özel eşikler yapılandırmak **kaydetmek**.  
![Uyarı kuralı Ekle](./media/log-analytics-azure-sql/create-alert02.png)

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [günlük aramaları](log-analytics-log-searches.md) ayrıntılı Azure SQL veri görüntülemek için günlük analizi içinde.
- [Kendi panolar oluşturun](log-analytics-dashboards.md) Azure SQL verilerini göstererek.
- [Uyarı oluşturma](log-analytics-alerts.md) belirli Azure SQL olayları olduğunda.
