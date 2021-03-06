---
title: Azure SQL veritabanı ölçümleri ve tanılama günlüğünü | Microsoft Docs
description: Kaynak kullanımı ve sorgu yürütme istatistikleri hakkında bilgi depolamak için Azure SQL veritabanı'nda tanılamayı etkinleştirme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 05/21/2019
ms.openlocfilehash: 6824a7151a0c007d6fe4ba021f274886a3cf0dcb
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447816"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL veritabanı ölçümleri ve tanılama günlükleri

Bu konu başlığında, günlüğün tanılama telemetrisi, Azure SQL veritabanı için Azure portalı, PowerShell, Azure CLI, Azure İzleyici REST API ve Azure Resource Manager şablonu aracılığıyla nasıl yapılandırılacağını öğreneceksiniz. Bu tanılama, kaynak kullanımı ve sorgu yürütme istatistikleri ölçmek için kullanılabilir.

Tek veritabanları ve elastik havuza alınmış veritabanlarını bir yönetilen örnek uygulamasındaki performans izlemeyi kolaylaştırmak için ölçümleri ve tanılama günlüklerini akışla örnek veritabanları. Bir veritabanının kaynak kullanımını, çalışanları ve oturumları ve aşağıdaki Azure kaynakları birine bağlantı aktarmaya yapılandırabilirsiniz:

- **Azure SQL Analytics**: performans raporları, uyarılar ve azaltıcı öneriler içeren, Azure SQL veritabanı akıllı izleme alınamıyor.
- **Azure Event Hubs**: SQL veritabanı telemetrisini özel izleme çözümleri veya yoğun işlem hatlarıyla tümleştirmek için.
- **Azure depolama**: telemetri fiyatı çok daha düşük maliyetlerle arşivlemek için.

    ![Mimari](./media/sql-database-metrics-diag-logging/architecture.png)

Çeşitli Azure Hizmetleri tarafından desteklenen Ölçümler ve günlük kategorileri hakkında daha fazla bilgi için bkz:

- [Microsoft azure'da ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md)

Bu makalede, Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekleri için tanılama telemetrisi etkinleştirmenize yardımcı olan yönergeler sağlar. Veritabanı tanılama telemetrisi görüntüleme için bir izleme aracı olarak Azure SQL Analytics yapılandırma anlamanıza da yardımcı olabilir.

## <a name="enable-logging-of-diagnostics-telemetry"></a>Telemetrinin tanılama günlüğünü etkinleştirme

Etkinleştirin ve aşağıdaki yöntemlerden birini kullanarak, ölçümleri ve tanılama telemetrisi günlüğünü yönetme:

- Azure portal
- PowerShell
- Azure CLI
- Azure İzleyici REST API
- Azure Resource Manager şablonu

Ölçümler ve günlüğe kaydetme tanılama etkinleştirdiğinizde, tanılama telemetrisini toplamaya yönelik Azure resource hedef belirtmeniz gerekir. Mevcut seçenekler şunlardır:

- Azure SQL Analytics
- Azure Event Hubs
- Azure Storage

Yeni bir Azure kaynak sağlayın veya mevcut bir kaynağı seçin. Kullanarak bir kaynak seçin sonra **tanılama ayarları** seçeneğinde, veri toplamak için belirtin.

## <a name="supported-diagnostic-logging-for-azure-sql-databases-and-instance-databases"></a>Desteklenen Azure SQL veritabanı ve örnek veritabanları için tanılama günlüğüne kaydetme

SQL veritabanlarında günlüğe kaydetme tanılama ve ölçümler enable - bunlar varsayılan olarak etkin değildir.

Aşağıdaki tanılama telemetrisi toplamak için Azure SQL veritabanı ve örnek veritabanları ayarlayın:

| Veritabanları için telemetri izleme | Tek veritabanı ve havuza veritabanı desteği | Örnek veritabanı desteği |
| :------------------- | ----- | ----- |
| [Temel ölçümler](#basic-metrics): DTU/CPU yüzdesi, DTU/CPU sınırı, fiziksel içeren veri okuma yüzdesi, günlük yazma ve yüzde başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi ve XTP depolama yüzdesi. | Evet | Hayır |
| [QueryStoreRuntimeStatistics](#query-store-runtime-statistics): CPU kullanımı gibi çalışma zamanı istatistikleri sorgu ve sorgu süresi istatistikleri hakkında bilgi içerir. | Evet | Evet |
| [QueryStoreWaitStatistics](#query-store-wait-statistics): Sorgu bekleme istatistikleri (ne sorgularınızın beklenen) hakkında bilgi içeren gibi CPU, günlük ve KİLİTLEME olan. | Evet | Evet |
| [Hataları](#errors-dataset): Bir veritabanında SQL hatalar hakkında bilgi içerir. | Evet | Evet |
| [DatabaseWaitStatistics](#database-wait-statistics-dataset): Ne kadar süre ilgili farklı bekleme türleri üzerinde bekleyen bir veritabanı harcanan bilgiler içerir. | Evet | Hayır |
| [Zaman aşımları](#time-outs-dataset): Bir veritabanı zaman aşımları hakkındaki bilgileri içerir. | Evet | Hayır |
| [Blokları](#blockings-dataset): Bir veritabanı etkinlikleri engelleme hakkında bilgi içerir. | Evet | Hayır |
| [Kilitlenmeler](#deadlocks-dataset): Bir veritabanında kilitlenme olaylar hakkında bilgi içerir. | Evet | Hayır |
| [AutomaticTuning](#automatic-tuning-dataset): Veritabanı otomatik ayarlama önerileri hakkında bilgi içerir. | Evet | Hayır |
| [SQLInsights](#intelligent-insights-dataset): Bir veritabanı için performans akıllı Öngörüler içerir. Daha fazla bilgi için bkz. [Intelligent Insights](sql-database-intelligent-insights.md). | Evet | Evet |

> [!IMPORTANT]
> Elastik havuzlar ve yönetilen örnekleri kendi ayrı tanılama telemetrisi içerdikleri veritabanlarından var. Bu, aşağıda açıklandığı gibi tanılama telemetrisi bu kaynakların her biri, için ayrı olarak yapılandırıldığı şekilde dikkat edin önemlidir.

> [!NOTE]
> Güvenlik denetimi ve SQLSecurityAuditEvents günlüklerini veritabanı tanılama ayarlarını (ekran üzerinde gösteriliyor olsa da) etkinleştirilemez. Denetim günlüğü akışını etkinleştirmek için bkz: [veritabanınız için denetimi ayarlamanız](sql-database-auditing.md#subheading-2), ve [Azure İzleyici günlüklerine ve Azure Event Hubs'a günlükler denetim](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242).

## <a name="azure-portal"></a>Azure portal

Kullanabileceğiniz **tanılama ayarları** menüsünü her, havuza alınmış, tek veya tanılama telemetrisi akış yapılandırmak için Azure portalında veritabanı örneği. Ayrıca, telemetriyi Tanıla de ayrı olarak veritabanı kapsayıcılar için yapılandırılabilir: elastik havuzları ve yönetilen örnekleri. Tanılama telemetrisi akışını aşağıdaki hedefleri ayarlayabilirsiniz: Azure depolama, Azure Event Hubs ve Azure İzleyici günlükleri.

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Elastik havuzlar için tanılama telemetrisi akışını yapılandırın

   ![Elastik havuz simgesi](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

Aşağıdaki tanılama telemetrisi toplamak için bir elastik havuz kaynağını ayarlayabilirsiniz:

| Resource | Telemetri izleme |
| :------------------- | ------------------- |
| **Elastik havuz** | [Temel ölçümler](sql-database-metrics-diag-logging.md#basic-metrics) içeren eDTU/CPU yüzdesi, eDTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, depolama sınırına ve XTP depolama yüzdesi. |

Elastik havuzlar ve elastik havuzlardaki veritabanları için tanılama telemetrisi akışını yapılandırmak için ayrı ayrı yapılandırmanız gerekecektir **hem** biri:

- Bir elastik havuz için tanılama telemetrisi akışını etkinleştirmek **ve**
- Elastik havuzdaki her veritabanı için tanılama telemetrisi akışını etkinleştir

Elastik havuz veritabanı bir tek veritabanı telemetrisini ayrı kendi telemetri ile kapsayıcıdır olmasıdır.

Bir elastik havuz kaynak için tanılama telemetrisi akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Git **elastik havuz** Azure portalında kaynak.
1. Seçin **tanılama ayarları**.
1. Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için.

   ![Elastik havuzlar için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

1. Kendi başvuru için bir ayar adı girin.
1. Tanılama veri akışı için bir hedef kaynak seçin: **Depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**.
1. Log analytics için seçin **yapılandırma** ve seçerek yeni bir çalışma alanı oluşturma **+ oluştur yeni çalışma alanı**, ya da mevcut bir çalışma alanını seçin.
1. Elastik havuz tanılama telemetrisi için onay kutusunu seçin: **Temel** ölçümleri.
   ![Elastik havuzlar için tanılamayı yapılandırmak](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)
1. **Kaydet**’i seçin.
1. Ayrıca, sonraki bölümde açıklanan adımları uygulayarak izlemek istediğiniz elastik havuz içindeki her bir veritabanı için tanılama telemetrisi akışını yapılandırın.

> [!IMPORTANT]
> Elastik havuzlar için tanılama telemetrisi yapılandırmaya ek olarak, ayrıca tanılama telemetrisi her veritabanı için elastik havuzda yapılandırmak aşağıda belirtildiği gibi gerekir.

### <a name="configure-streaming-of-diagnostics-telemetry-for-single-database-or-database-in-elastic-pool"></a>Tek veritabanı veya elastik havuzdaki veritabanı için tanılama telemetrisi akışını yapılandırın

   ![SQL veritabanı simgesi](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

Tek veya havuza alınmış veritabanları için tanılama telemetrisi akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure Git **SQL veritabanı** kaynak.
1. Seçin **tanılama ayarları**.
1. Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için.
   - Akış tanılama telemetrisi en fazla üç Paralel bağlantılar oluşturabilirsiniz.
   - Seçin **+ tanılama ayarı ekleme** , birden fazla kaynak için Tanılama verileri paralel akış yapılandırmak için.

   ![Tek için tanılamayı etkinleştirin, havuza alınmış veya örnek veritabanları](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)
1. Kendi başvuru için bir ayar adı girin.
1. Tanılama veri akışı için bir hedef kaynak seçin: **Depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**.
1. Standart, olay tabanlı izleme deneyimi için veritabanı tanılama günlük telemetrisi için aşağıdaki onay kutularını işaretleyin: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics**, **hataları** , **DatabaseWaitStatistics**, **zaman aşımları**, **blokları**, ve **kilitlenmeleri**.
1. Bir Gelişmiş, bir dakika-tabanlı izleme deneyimi için onay kutusunu seçin **temel** ölçümleri.
   ![Tek tanılamayı yapılandırmak, havuza alınmış veya örnek veritabanları](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)
1. **Kaydet**’i seçin.
1. İzlemek istediğiniz her veritabanı için bu adımları yineleyin.

> [!NOTE]
> Güvenlik denetimi ve SQLSecurityAuditEvents günlüklerini (ekranda gösterilen rağmen) veritabanı tanılama ayarları etkinleştirilemez. Denetim günlüğü akışını etkinleştirmek için bkz: [veritabanınız için denetimi ayarlamanız](sql-database-auditing.md#subheading-2), ve [Azure İzleyici günlüklerine ve Azure Event Hubs'a günlükler denetim](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242).
> [!TIP]
> İzlemek istediğiniz her Azure SQL veritabanı bu adımları yineleyin.

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instances"></a>Yönetilen örnek için tanılama telemetrisi akışını yapılandırın

   ![Yönetilen örnek simgesi](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

Aşağıdaki tanılama telemetrisi toplamak için bir yönetilen örnek kaynağı ayarlayabilirsiniz:

| Resource | Telemetri izleme |
| :------------------- | ------------------- |
| **Yönetilen örnek** | [ResourceUsageStats](#resource-usage-stats-for-managed-instance) sanal çekirdek sayısı, ortalama CPU yüzdesi, g/ç istekleri, bayt okunan/yazılan, ayrılmış depolama alanı içerir ve kullanılan depolama alanı. |

Yönetilen örnek ve örnek veritabanları için tanılama telemetrisi akışını yapılandırmak için ayrı ayrı yapılandırmanız gerekecektir **hem** biri:

- Yönetilen örnek için tanılama telemetrisi akışını etkinleştirmek **ve**
- Her bir örnek veritabanı için tanılama telemetrisi akışını etkinleştir

Yönetilen örnek veritabanı ile bir bağımsız örnek veritabanı telemetrisini ayrı kendi telemetri kapsayıcıdır olmasıdır.

Yönetilen örnek kaynak için tanılama telemetrisi akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Git **yönetilen örnek** Azure portalında kaynak.
1. Seçin **tanılama ayarları**.
1. Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için.

   ![Yönetilen örnek için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

1. Kendi başvuru için bir ayar adı girin.
1. Tanılama veri akışı için bir hedef kaynak seçin: **Depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**.
1. Log analytics için seçin **yapılandırma** ve seçerek yeni bir çalışma alanı oluşturma **+ oluştur yeni çalışma alanı**, veya varolan bir çalışma alanını kullanın.
1. Tanılama telemetrisi örneği için onay kutusunu seçin: **ResourceUsageStats**.
   ![Yönetilen örnek için tanılamayı yapılandırmak](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)
1. **Kaydet**’i seçin.
1. Ayrıca, sonraki bölümde açıklanan adımları izleyerek izlemek istediğiniz yönetilen örnek içindeki her bir örnek veritabanı için tanılama telemetrisi akışını yapılandırın.

> [!IMPORTANT]
> Yönetilen örnek için tanılama telemetrisi yapılandırmaya ek olarak, ayrıca her bir örnek veritabanı için tanılama telemetrisi yapılandırmak aşağıda belirtildiği gibi gerekir.

### <a name="configure-streaming-of-diagnostics-telemetry-for-instance-databases"></a>Tanılama telemetrisi örneğin veritabanlarına akış yapılandırma

   ![Örnek veritabanı yönetilen örneği simgesi](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

Tanılama telemetrisi örneğin veritabanlarına akışını etkinleştirmek için bu adımları izleyin:

1. Git **örnek veritabanı** yönetilen örnek içinde kaynak.
1. Seçin **tanılama ayarları**.
1. Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için.
   - En çok üç (3) paralel bağlantı stream tanılama telemetrisi için oluşturabilirsiniz.
   - Seçin **+ tanılama ayarı ekleme** , birden fazla kaynak için Tanılama verileri paralel akış yapılandırmak için.

   ![Veritabanı örneği için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

1. Kendi başvuru için bir ayar adı girin.
1. Tanılama veri akışı için bir hedef kaynak seçin: **Depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**.
1. Veritabanı tanılama telemetrisi için onay kutularını seçin: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics** ve **hataları**.
   ![Tanılama örneği için veritabanları Yapılandır](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)
1. **Kaydet**’i seçin.
1. İzlemek istediğiniz her bir örnek veritabanı için bu adımları yineleyin.

> [!TIP]
> İzlemek istediğiniz her bir örnek veritabanı için bu adımları yineleyin.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

PowerShell kullanarak, ölçümleri ve tanılama günlük kaydını etkinleştirebilirsiniz.

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı kimliği hedef depolama hesabı için kaynak kimliğidir.

- Tanılama günlükleri Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Log Analytics çalışma alanınızın kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="to-configure-multiple-azure-resources"></a>Birden çok Azure kaynaklarını yapılandırmak için

Birden çok abonelik desteklemek için PowerShell betiğini kullanın. [kaynak ölçümleri günlük kaydı etkinleştirmek için Azure PowerShell kullanarak](https://blogs.technet.microsoft.com/msoms/20../../enable-azure-resource-metrics-logging-using-powershell/).

Çalışma alanı kaynak Kimliğini sağlamanız \<$WSID\> betiği yürütülürken, bir parametre olarak `Enable-AzureRMDiagnostics.ps1` çalışma alanına birden çok kaynaklardan Tanılama verileri göndermek.

- Çalışma alanı kimliği almak için \<$WSID\> hedefini tanılama verilerinizi, aşağıdaki betiği kullanın:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

   Değiştirin \<Subıd\> abonelik kimliği ile \<RG_NAME\> kaynak grubu adı ile ve \<WS_NAME\> çalışma alanı adı ile.

### <a name="azure-cli"></a>Azure CLI

Azure CLI kullanarak, ölçümleri ve tanılama günlük kaydını etkinleştirebilirsiniz.

> [!NOTE]
> Tanılama Günlüğü etkinleştirmek için betikleri Azure CLI Sürüm 1.0 için desteklenir. CLI v2.0 şu anda desteklenmeyen olduğunu lütfen unutmayın.

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Depolama hesabı kimliği hedef depolama hesabı için kaynak kimliğidir.

- Tanılama günlükleri Olay hub'ına akış etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Tanılama günlükleri Log Analytics çalışma alanına gönderme etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

Konusunu okuyun [Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings).

### <a name="resource-manager-template"></a>Resource Manager şablonu

Konusunu okuyun [Resource Manager şablonu kullanarak kaynak oluşturma sırasında tanılama ayarlarını etkinleştirme](../azure-monitor/platform/diagnostic-logs-stream-template.md).

## <a name="stream-into-azure-sql-analytics"></a>Azure SQL Analytics içine Stream

Azure SQL Analytics, Azure SQL veritabanları, elastik havuzlar ve uygun ölçekte ve birden çok aboneliğe yönetilen örnekleri performansını izleyen bir bulut çözümüdür. Performans sorunlarını gidermek için yerleşik zekaya sahiptir ve toplamanıza ve Azure SQL veritabanı performans ölçümleri görselleştirmenize yardımcı olabilir.

![Azure SQL Analytics genel bakış](../azure-monitor/insights/media/azure-sql/azure-sql-sol-overview.png)

SQL veritabanı ölçümleri ve tanılama günlükleri akışa Azure SQL Analytics içinde yerleşik kullanarak **Log Analytics'e gönderme** portalında tanılama ayarlar sekmesindeki seçeneği. Ayrıca, PowerShell cmdlet'lerini, Azure CLI veya Azure İzleyici REST API aracılığıyla bir tanılama ayarını kullanarak log analytics'e de etkinleştirebilirsiniz.

### <a name="installation-overview"></a>Yüklemeye genel bakış

Azure SQL Analytics ile bir SQL veritabanı fleet izleyebilirsiniz. Aşağıdaki adımları gerçekleştirin:

1. Azure Marketi'nden bir Azure SQL Analytics çözümünü oluşturun.
2. Çözümde bir izleme çalışma alanı oluşturun.
3. Akış tanılama telemetrisi veritabanlarına çalışma alanına yapılandırın.

Elastik havuzlar kullandığınız ya da yönetilen örnekler, ayrıca tanılama telemetrisi bu kaynaklardan akış yapılandırmanız gerekir.

### <a name="create-azure-sql-analytics-resource"></a>Azure SQL Analytics kaynak oluştur

1. Azure SQL Analytics, Azure Market'te arayın ve seçin.

   ![Azure SQL Analytics için portalda da arayabilirsiniz.](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)

2. Seçin **Oluştur** çözümün genel bakış ekranında.

3. Azure SQL Analytics formunu gerekli olan ek bilgilerle doldurun: çalışma alanı adı, abonelik, kaynak grubu, konum ve fiyatlandırma katmanı.

   ![Azure SQL Analytics portalında yapılandırma](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Seçin **Tamam** onaylayın ve ardından **Oluştur**.

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Veritabanlarını kayıt ölçümleri ve tanılama günlükleri için yapılandırma

Burada ölçümleri veritabanları kaydı yapılandırmak için en kolay yolu, Azure portalı kullanmaktır. SQL veritabanı kaynağınızın Azure portal ve seçin için daha önce açıklandığı gibi Git **tanılama ayarları**.

Elastik havuzlar kullandığınız ya da yönetilen örnekler, ayrıca çalışma alanına akışı tanılama telemetrisi etkinleştirmek için bu kaynakları tanılama ayarları yapılandırmanız gerekir.

### <a name="use-the-sql-analytics-solution"></a>SQL Analytics çözümünü kullanın

SQL veritabanı kaynaklarınızı görüntülemek için SQL Analytics hiyerarşik bir pano olarak kullanabilirsiniz. SQL Analytics çözümünü kullanma konusunda bilgi almak için bkz: [SQL Analytics çözümünü kullanarak SQL veritabanını izleme](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Event Hubs'a akış sağlama

Yerleşik kullanarak SQL veritabanı ölçümleri ve tanılama günlüklerinin Event Hubs'a düzenleyebilir **Stream olay hub'ına** seçeneği Azure portalında. PowerShell cmdlet'lerini, Azure CLI veya Azure İzleyici REST API aracılığıyla bir tanılama ayarını kullanarak, Service Bus kural kimliği de etkinleştirebilirsiniz.

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Olay hub'ları, ölçümleri ve tanılama özellikli yapmanız gerekenler günlüğe kaydeder

Seçili verileri, Event Hubs'a akış sonra Gelişmiş izleme senaryoları etkinleştirmek için bir adım daha yaklaştınız olursunuz. Event Hubs bir olay ardışık düzeni için ön kapı görevi görür. Veri bir olay hub'ına toplandıktan sonra dönüştürülmüş ve gerçek zamanlı analiz sağlayıcısı veya bir depolama bağdaştırıcısı kullanılarak depolanır. Olay hub'ları akışı üretimlerini bu olayların üretim ayırır. Bu şekilde, olay tüketicileri olaylara kendi zamanlamalarında erişebilir. Event Hubs hakkında daha fazla bilgi için bkz:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Event Hubs, akış ölçümleri kullanabilirsiniz:

- **Hot yol verilerini Power bı'a akış tarafından hizmet durumu görüntüleme**. Event Hubs, Stream Analytics ve Power BI'ı kullanarak Azure hizmetlerinizi ölçümleri ve tanılama verilerinizi neredeyse gerçek zamanlı Öngörüler kolayca dönüştürebilirsiniz. Verileri işlemek, çıktı olarak kullanmak üzere Power BI ve Stream Analytics ile nasıl bir olay hub'ı ayarladınız genel bakış için bkz. [Stream Analytics ve Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

- **Üçüncü taraf günlüğe kaydetme ve telemetri akışlarını günlüklerine Stream**. Event Hubs akış kullanarak çeşitli üçüncü taraf izleme ve günlük analizi çözümleriyle, ölçümleri ve tanılama günlüklerini alabilirsiniz.

- **Özel telemetri ve günlüğe kaydetme platform derleme**. Yapmak zaten ürettikleri telemetri platform veya bir oluşturma dikkate? Yüksek düzeyde ölçeklenebilir Yayımla-doğasını abone ol olay hub'ları esnek bir şekilde tanılama günlükleri alma olanak tanır. Bkz: [Dan Rosanova'nın küresel ölçekli bir telemetri platform Event Hubs kullanarak Kılavuzu](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Stream depolamaya

SQL veritabanı ölçümleri depolayabilir ve tanılama günlüklerini Azure Depolama'da yerleşik kullanarak **bir depolama hesabında arşivle** seçeneği Azure portalında. Ayrıca, depolama PowerShell cmdlet'lerini, Azure CLI veya Azure İzleyici REST API aracılığıyla bir tanılama ayarını kullanarak etkinleştirebilirsiniz.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Depolama hesabında şema ölçümleri ve tanılama günlükleri

Ölçümleri ve tanılama günlükleri toplamayı ayarlama sonra ilk veri satırı kullanılamadığında, seçilen depolama hesabında bir depolama kapsayıcısı oluşturulur. Blobları yapıdır:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Veya daha basit:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir blob adı için temel ölçümleri olabilir:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Elastik havuz verileri depolamak için blob adı şuna benzer:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

## <a name="data-retention-policy-and-pricing"></a>Veri bekletme ilkesi ve fiyatlandırma

Olay hub'ları veya bir depolama hesabı seçerseniz, bekletme ilkesi belirtebilirsiniz. Bu ilke, seçilen zaman süresinden daha eski olan verileri siler. Log Analytics belirtirseniz, bekletme ilkesi seçili fiyatlandırma katmanına bağlıdır. Bu durumda, veri alımı, sağlanan ücretsiz birimleri her ay ücretsiz çeşitli veritabanları izlemeyi etkinleştirebilirsiniz. Herhangi bir tanılama telemetrisi ücretsiz birimleri aşan tüketiminin ücrete neden olabilir. Etkin veritabanları daha ağır iş yükleri ile boştaki veritabanlarının daha fazla veri alma dikkat edin. Daha fazla bilgi için [Log analytics fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/).

Azure SQL Analytics kullanıyorsanız, veri alımı tüketiminiz çözümdeki seçerek izleyebilirsiniz **OMS çalışma alanı** Azure SQL Analytics ve ardından Gezinti menüsünde **kullanım** ve **tahmini maliyetleri**.

## <a name="metrics-and-logs-available"></a>Ölçümleri ve günlük yok

Azure SQL veritabanı için kullanılabilen telemetri izleme, elastik havuzları ve yönetilen örnek belgelenen aşağıda. Kendi özel analiz ve uygulama geliştirme kullanmak için SQL Analytics içinde toplanan izleme telemetri kullanılabilir [Azure İzleyici günlük sorguları](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) dili.

## <a name="basic-metrics"></a>Temel ölçümler

Kaynak tarafından temel ölçümler hakkında ayrıntılar için aşağıdaki tablolara bakın.

> [!NOTE]
> Temel ölçümler seçeneği, daha önce tüm ölçümler olarak biliniyordu. İzlenen ölçümler değişiklik yoktu ve yalnızca adlandırma için yaptığınız değişikliği oluştu. Bu değişiklik, gelecekte ek ölçüm kategorisi tanıtılması için izin verecek şekilde başlatıldı.

### <a name="basic-metrics-for-elastic-pools"></a>Elastik havuzlar için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Elastik havuz|eDTU yüzdesi eDTU kullanıldığında, eDTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, depolama sınırı, XTP depolama yüzdesi |

### <a name="basic-metrics-for-azure-sql-databases"></a>Azure SQL veritabanları için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Azure SQL veritabanı|DTU yüzdesi DTU kullanıldığında, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve kilitlenmeler |

## <a name="basic-logs"></a>Temel günlükleri

Tüm günlükler için kullanılabilen telemetri ayrıntıları aşağıdaki tabloda belirtilmiştir. Lütfen [tanılama günlüğüne kaydetme desteklenen](#supported-diagnostic-logging-for-azure-sql-databases-and-instance-databases) havuza, hangi günlüklerin belirli veritabanı flavor için - Azure SQL tek desteklenen anlamak veya veritabanı örneği.

### <a name="resource-usage-stats-for-managed-instance"></a>Yönetilen örnek için kaynak kullanım İstatistiği

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: ResourceUsageStats |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: MANAGEDINSTANCES |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Yönetilen örnek adı |
|ResourceId|Kaynak URI'si |
|SKU_s|Yönetilen örnek Ürün SKU'su |
|virtual_core_count_s|Kullanılabilir sanal çekirdek sayısı |
|avg_cpu_percent_s|Ortalama CPU yüzdesi |
|reserved_storage_mb_s|Yönetilen örnek ayrılmış depolama kapasitesi |
|storage_space_used_mb_s|Yönetilen örneğinde kullanılan depolama |
|io_requests_s|IOPS sayısı |
|io_bytes_read_s|Okunan bayt IOPS |
|io_bytes_written_s|Yazılan bayt IOPS |

### <a name="query-store-runtime-statistics"></a>Query Store çalışma zamanı istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: QueryStoreRuntimeStatistics |
|OperationName|İşlemin adı. Her zaman: QueryStoreRuntimeStatisticsEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|query_hash_s|Sorgu karması |
|query_plan_hash_s|Sorgu planı karması |
|statement_sql_handle_s|Deyim sql tanıtıcısı |
|interval_start_time_d|Aralığın datetimeoffset işareti 1900-1-1'den başlatın |
|interval_end_time_d|Son datetimeoffset 1900-1-1'den tıklarının sayısını zaman aralığı |
|logical_io_writes_d|Mantıksal GÇ Yazma toplam sayısı |
|max_logical_io_writes_d|Yürütme en fazla mantıksal bir g/ç sayısı Yazar |
|physical_io_reads_d|Toplam fiziksel GÇ okuma sayısı |
|max_physical_io_reads_d|Yürütme en fazla mantıksal bir g/ç sayısı okur |
|logical_io_reads_d|Mantıksal bir g/ç okuma toplam sayısı |
|max_logical_io_reads_d|Yürütme en fazla mantıksal bir g/ç sayısı okur |
|execution_type_d|Yürütme türü |
|count_executions_d|Sorgu yürütme sayısı |
|cpu_time_d|Mikrosaniye cinsinden sorgu tarafından kullanılan toplam CPU süresi |
|max_cpu_time_d|Tek bir yürütme mikrosaniye cinsinden tarafından en fazla CPU süresi tüketici |
|dop_d|Paralellik derecesi toplamı |
|max_dop_d|En fazla tek yürütme için kullanılan paralellik derecesi |
|rowcount_d|Döndürülen satır sayısı |
|max_rowcount_d|En fazla tek yürütme döndürülen satır sayısı |
|query_max_used_memory_d|Toplam KB kullanılan bellek miktarı |
|max_query_max_used_memory_d|En çok tek bir yürütme KB tarafından kullanılan bellek miktarı |
|duration_d|Mikrosaniye cinsinden toplam yürütme süresi |
|max_duration_d|En fazla yürütme zamanı, tek bir yürütme |
|num_physical_io_reads_d|Toplam fiziksel okuma sayısı |
|max_num_physical_io_reads_d|En fazla yürütme başına fiziksel okuma sayısı |
|log_bytes_used_d|Kullanılan günlük baytı toplam miktarı |
|max_log_bytes_used_d|En fazla yürütme kullanılan günlük bayt miktarı |
|query_id_d|Query Store sorgu kimliği |
|plan_id_d|Query Store planında kimliği |

Daha fazla bilgi edinin [Query Store çalışma zamanı istatistik verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Query Store bekleme istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: QueryStoreWaitStatistics |
|OperationName|İşlemin adı. Her zaman: QueryStoreWaitStatisticsEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|wait_category_s|Bekleme kategorisi |
|is_parameterizable_s|Sorgu parametrelenebilir mi |
|statement_type_s|Deyiminin türü |
|statement_key_hash_s|Deyimi anahtar karması |
|exec_type_d|Çalıştırma türü |
|total_query_wait_time_ms_d|Belirli bir bekleme kategoriye sorgunun toplam bekleme süresi |
|max_query_wait_time_ms_d|En fazla bekleme zamanı sorgunun belirli bekleme kategorisi üzerinde tek tek yürütme |
|query_param_type_d|0 |
|query_hash_s|Query Store içinde sorgu karması |
|query_plan_hash_s|Query Store içinde sorgu planı karması |
|statement_sql_handle_s|Query Store içinde deyim tanıtıcısı |
|interval_start_time_d|Aralığın datetimeoffset işareti 1900-1-1'den başlatın |
|interval_end_time_d|Son datetimeoffset 1900-1-1'den tıklarının sayısını zaman aralığı |
|count_executions_d|Sorgu yürütme sayısı |
|query_id_d|Query Store sorgu kimliği |
|plan_id_d|Query Store planında kimliği |

Daha fazla bilgi edinin [Query Store bekleme istatistikleri veri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Hataları veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQ |
|Category|Kategori adı. Her zaman: Hatalar |
|OperationName|İşlemin adı. Her zaman: ErrorEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|`Message`|Hata iletisi düz metin |
|user_defined_b|Hata kullanıcı tanımlı bit |
|error_number_d|Hata kodu |
|Severity|Hata önem derecesi |
|state_d|Hata durumu |
|query_hash_s|Sorgu karması varsa başarısız sorgu |
|query_plan_hash_s|Sorgu planı karma varsa başarısız sorgu |

Daha fazla bilgi edinin [SQL Server hata iletileri](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Veritabanı bekleme istatistikleri veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: DatabaseWaitStatistics |
|OperationName|İşlemin adı. Her zaman: DatabaseWaitStatisticsEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|wait_type_s|Bekleme türü adı |
|start_utc_date_t [UTC]|Ölçülen dönem başlangıç zamanı |
|end_utc_date_t [UTC]|Ölçülen dönemi bitiş saati |
|delta_max_wait_time_ms_d|En fazla beklenen süre yürütme başına |
|delta_signal_wait_time_ms_d|Toplam bekleme süresi sinyalleri |
|delta_wait_time_ms_d|Dönemi içindeki toplam bekleme süresi |
|delta_waiting_tasks_count_d|Bekleyen görev sayısı |

Daha fazla bilgi edinin [bekleme istatistikleri veritabanı](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Zaman aşımlarını veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: Zaman Aşımları |
|OperationName|İşlemin adı. Her zaman: TimeoutEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|error_state_d|Hata durum kodu |
|query_hash_s|Varsa, karma sorgu |
|query_plan_hash_s|Sorgu planı karma varsa |

### <a name="blockings-dataset"></a>Blockings veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: blokları |
|OperationName|İşlemin adı. Her zaman: BlockEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|lock_mode_s|Sorgu tarafından kullanılan kilit modu |
|resource_owner_type_s|Kilit sahibi |
|blocked_process_filtered_s|İşlem raporu XML engellendi |
|duration_d|Mikrosaniye cinsinden kilit süresi |

### <a name="deadlocks-dataset"></a>Kilitlenmeler veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC] |Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: Kilitlenmeler |
|OperationName|İşlemin adı. Her zaman: DeadlockEvent |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|deadlock_xml_s|Kilitlenme rapor XML |

### <a name="automatic-tuning-dataset"></a>Otomatik ayarlama veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı Kimliğiniz |
|SourceSystem|Her zaman: Azure |
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT.SQL |
|Category|Kategori adı. Her zaman: AutomaticTuning |
|Resource|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULARI/VERİTABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID'si |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı sunucusunun adı |
|LogicalDatabaseName_s|Veritabanının adı |
|ElasticPoolName_s|Varsa veritabanı için elastik havuz adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI'si |
|RecommendationHash_s|Otomatik ayarlama önerileri benzersiz karma |
|OptionName_s|Otomatik ayarlama işlemi |
|Schema_s|Veritabanı şeması |
|Table_s|Etkilenen tablo |
|IndexName_s|Dizin adı |
|IndexColumns_s|Sütun adı |
|IncludedColumns_s|Dahil edilen sütunlar |
|EstimatedImpact_s|Otomatik ayarlama önerileri JSON etkisini tahmini |
|Event_s|Otomatik ayarlama olay türü |
|Timestamp_t|Son güncelleştirilen zaman damgası |

### <a name="intelligent-insights-dataset"></a>Intelligent Insights veri kümesi

Daha fazla bilgi edinin [Intelligent Insights günlük biçimi](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Sonraki adımlar

Günlüğe kaydetmeyi etkinleştirmek için ölçümleri anlamak ve kategoriler çeşitli Azure Hizmetleri tarafından desteklenen günlük öğrenmek için bkz:

- [Microsoft azure'da ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md)

Event Hubs hakkında bilgi edinmek için:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
