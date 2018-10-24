---
title: Azure SQL veritabanı ölçümleri ve tanılama günlüğünü | Microsoft Docs
description: Kaynak kullanımı, bağlantı ve sorgu yürütme istatistikleri depolamak için Azure SQL veritabanı yapılandırma hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 775883d575a87758f563bd8dae8e5a726cd8ed36
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49959086"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL veritabanı ölçümleri ve tanılama günlükleri 

Azure SQL veritabanı, elastik havuzlar, yönetilen örneği ve yönetilen örneği can ölçümleri ve tanılama günlükleri, performans izlemeyi kolaylaştırmak için yayma veritabanlarında. Bir veritabanı stream kaynak kullanımını, çalışanları ve oturumları ve bu Azure kaynaklarından birine bağlantısını için yapılandırabilirsiniz:

* **Azure SQL Analytics**: tümleşik Azure veritabanı performans akıllı izleme çözümü, raporlama, uyarı ve azaltma özelliklerine sahip olarak kullanılır.
* **Azure Event Hubs**: SQL veritabanı telemetrisini özel izleme çözümünüz veya yoğun işlem hatlarıyla tümleştirmek için kullanılır.
* **Azure depolama**: maliyetlerle çok fiyat çok daha düşük telemetri arşivleme için kullanılır.

    ![Mimari](./media/sql-database-metrics-diag-logging/architecture.png)

Ölçüleri anlama ve çeşitli Azure Hizmetleri tarafından desteklenen kategoriler oturum için okuma dikkate alın:

* [Microsoft azure'da ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Azure tanılama günlüklerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

 Etkinleştirin ve aşağıdaki yöntemlerden birini kullanarak bir veritabanında günlüğe kaydetme, ölçümleri ve tanılama telemetrisini yönetme:

- Azure portal
- PowerShell
- Azure CLI
- Azure İzleyici REST API 
- Azure Resource Manager şablonu

Ölçümleri ve tanılama günlüğünü etkinleştirdiğinizde, seçilen verileri nerede toplanacağını Azure resource hedef belirtmeniz gerekir. Kullanılabilir seçenekler şunlardır:

- SQL analizi
- Event Hubs
- Depolama 

Yeni bir Azure kaynak sağlayın veya mevcut bir kaynağı seçin. Tanılama ayarları seçeneğini kullanarak, bir kaynak seçtikten sonra hangi verileri toplamak için belirtmeniz gerekir. 

## <a name="enable-logging-for-elastic-pools-or-managed-instance"></a>Elastik havuzlar veya yönetilen örneği için günlüğe kaydetmeyi etkinleştirme

Elastik havuzlar ve yönetilen örnekler veritabanı kapsayıcıları olarak varsayılan olarak etkin değil, kendi tanılama telemetrisi var. Bu telemetrinin veritabanı tanılama telemetrisini ayrı unutmayın. Elastik havuzlar için tanılama telemetrisi akışını olmasının nedeni budur ve yönetilen örnek ayrıca veritabanı tanılama telemetrisi, aşağıda daha fazla açıklanmaktadır yapılandırma ile yapılandırılması gerekir. 

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Elastik havuzlar için tanılama telemetrisi akışını yapılandırın

Aşağıdaki tanılama telemetrisi toplama elastik için kullanılabilir kaynak havuzları:

| Kaynak | Telemetri izleme |
| :------------------- | ------------------- |
| **Elastik havuz** | [Tüm ölçümleri](sql-database-metrics-diag-logging.md#all-metrics) içeren eDTU/CPU yüzdesi, eDTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, depolama sınırına ve XTP depolama yüzdesi. |

Tanılama telemetrisi için akışını etkinleştirmek için **elastik havuz kaynak**, şu adımları izleyin:

- Azure portalında esnek havuz kaynağa Git
- Seçin **tanılama ayarları**
- Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için
- Ayar - kendi başvuru adını yazın
- Elastik havuzundan tanılama veri akışı için hangi kaynak için seçin: **depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**
- Log Analytics seçili durumda seçin **yapılandırma** ve seçerek yeni bir çalışma alanı oluşturma **+ oluştur yeni çalışma alanı**, veya varolan bir çalışma alanı seçin
- Elastik havuz tanılama telemetrisi için onay kutusunu işaretleyin **AllMetrics**
- **Kaydet**’e tıklayın

İzlemek istediğiniz her elastik havuz için yukarıdaki adımları yineleyin.

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instance"></a>Yönetilen örnek için tanılama telemetrisi akış yapılandırma

Aşağıdaki tanılama telemetrisi için yönetilen örnek kaynak koleksiyonu için kullanılabilir:

| Kaynak | Telemetri izleme |
| :------------------- | ------------------- |
| **Yönetilen örnek** | [ResourceUsageStats](sql-database-metrics-diag-logging.md#resource-usage-stats) , ayrılmış depolama alanı, kullanılan depolama alanı sanal çekirdek sayısı, ortalama CPU yüzdesi, g/ç istekleri, okunan/yazılan bayt içerir. |

Tanılama telemetrisi için akışını etkinleştirmek için **yönetilen örnek kaynak**, şu adımları izleyin:

- Azure portalında yönetilen örneği kaynağa Git
- Seçin **tanılama ayarları**
- Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için
- Ayar - kendi başvuru adını yazın
- Elastik havuzundan tanılama veri akışı için hangi kaynak için seçin: **depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**
- Log Analytics seçili durumda oluşturun veya mevcut bir çalışma alanını kullanma
- Tanılama telemetrisi örneği için onay kutusunu işaretleyin **ResourceUsageStats**
- **Kaydet**’e tıklayın

Her yönetilen izlemek istediğiniz örneği için yukarıdaki adımları yineleyin.

## <a name="enable-logging-for-azure-sql-database-or-databases-in-managed-instance"></a>Azure SQL veritabanı veya yönetilen örnek veritabanları için günlük kaydını etkinleştirme

Ölçümler ve SQL veritabanı yönetilen örneği veritabanlarını ve oturum Tanılama, varsayılan olarak etkinleştirilmedi.

Aşağıdaki tanılama telemetrisi toplama Azure SQL veritabanları ve yönetilen örnek veritabanları için kullanılabilir:

| Veritabanları için telemetri izleme | Azure SQL veritabanı desteği | Veritabanı yönetilen örneği desteği |
| :------------------- | ------------------- | ------------------- |
| [Tüm ölçümleri](sql-database-metrics-diag-logging.md#all-metrics): içeren DTU/CPU yüzdesi, DTU/CPU limit, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi ve XTP depolama yüzdesi. | Evet | Hayır |
| [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): bilgileri içeren sorgu çalışma zamanı istatistikleri hakkında CPU kullanım içindir ve süresi istatistikleri sorgu. | Evet | Evet |
| [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): ne sorgularınızı, CPU, günlük ve KİLİTLEME gibi beklediğini bildirir sorgu bekleme istatistikleri hakkında bilgiler içerir. | Evet | Evet |
| [Hataları](sql-database-metrics-diag-logging.md#errors-dataset): Bu veritabanı üzerinde gerçekleşen SQL hatalar hakkında bilgi içerir. | Evet | Hayır |
| [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): bir veritabanı hakkında ne kadar zaman harcanan farklı bekleme türleri üzerinde bekleyen bilgileri içerir. | Evet | Hayır |
| [Zaman aşımları](sql-database-metrics-diag-logging.md#time-outs-dataset): bir veritabanı üzerinde gerçekleşen zaman aşımları hakkındaki bilgileri içerir. | Evet | Hayır |
| [Blokları](sql-database-metrics-diag-logging.md#blockings-dataset): bir veritabanı üzerinde gerçekleşen olayları engelleme hakkında bilgi içerir. | Evet | Hayır |
| [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): performans akıllı Öngörüler içerir. [Akıllı İçgörüler hakkında daha fazla bilgi](sql-database-intelligent-insights.md). | Evet | Evet |

### <a name="azure-portal"></a>Azure portal

Azure SQL veritabanı ve yönetilen örnek veritabanları hedeflere Azure depolama için tanılama telemetrisi akış, olay hub'ları veya Log Analytics tanılama ayarları menüsü Azure portalında veritabanlarının her biri için yapılandırılır.

### <a name="configure-streaming-of-diagnostics-telemetry-for-azure-sql-database"></a>Tanılama telemetrisi Azure SQL veritabanı için akış yapılandırma

Tanılama telemetrisi için akışını etkinleştirmek için **Azure SQL veritabanı**, şu adımları izleyin:

- Azure SQL veritabanı kaynağınızın gidin
- Seçin **tanılama ayarları**
- Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için
- En fazla üç (3) paralel bağlantıları akışı tanılama telemetrisi için oluşturulabilir. Birden çok paralel birden fazla kaynak için tanılama veri akışını yapılandırmak için seçin **+ tanılama ayarı ekleme** ek bir ayar oluşturun.

   ![Azure portalında etkinleştirin](./media/sql-database-metrics-diag-logging/enable-portal.png)

- Ayar - kendi başvuru adını yazın
- Tanılama veri akışı için hangi kaynak veritabanından için seçin: **depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**
- Standart izleme deneyimi için veritabanı tanılama günlük telemetrisi için onay kutularını seçin: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics** , **QueryStoreWaitStatistics**, **hataları**, **DatabaseWaitStatistics**, **zaman aşımları**, **blokları** , **Kilitlenmeleri**. Bu telemetrinin, olay tabanlı ve izleme deneyimini standart sunar ' dir.
- Gelişmiş izleme deneyimi için onay kutusunu seçip **AllMetrics**. 1 dakikalık tabanlı telemetri veritabanı tanılama telemetrisi için yukarıda açıklandığı gibi budur. 

   ![Tanılama ayarları](./media/sql-database-metrics-diag-logging/diagnostics-portal.png)

İzlemek istediğiniz her Azure SQL veritabanı yukarıdaki adımları yineleyin.

> [!NOTE]
> Seçeneği gösteriliyor olsa da, Denetim günlüğü veritabanı tanılama ayarlarını, etkinleştirilemez. Denetim günlüğü akışını etkinleştirmek için bkz: [veritabanınız için denetimi ayarlamanız](sql-database-auditing.md#subheading-2)
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-databases-in-managed-instance"></a>Yönetilen örnek veritabanları için tanılama telemetrisi akışını yapılandırın

Tanılama telemetrisi için akışını etkinleştirmek için **yönetilen örnek veritabanları**, şu adımları izleyin:

- Yönetilen örnek veritabanınızdaki gidin
- Seçin **tanılama ayarları**
- Seçin **tanılamayı Aç** önceki ayar yok ya da seçin **ayarını Düzenle** önceki bir ayarı düzenlemek için
- En fazla üç (3) paralel bağlantıları akışı tanılama telemetrisi için oluşturulabilir. Birden çok paralel birden fazla kaynak için tanılama veri akışını yapılandırmak için seçin **+ tanılama ayarı ekleme** ek bir ayar oluşturun.
- Ayar - kendi başvuru adını yazın
- Tanılama veri akışı için hangi kaynak veritabanından için seçin: **depolama hesabında arşivle**, **Stream olay hub'ına**, veya **Log Analytics'e gönderme**
- Veritabanı tanılama telemetrisi için onay kutularını seçin: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics** ve **hataları**

   ![Tanılama ayarları](./media/sql-database-metrics-diag-logging/diagnostics-portal-mi.png)

Yönetilen örnek izlemek istediğiniz her veritabanı için yukarıdaki adımları yineleyin.

### <a name="powershell"></a>PowerShell

Ölçümleri ve tanılama PowerShell kullanarak etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı kimliği kaynak depolama hesabı için günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlükleri Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Log Analytics çalışma alanınızın kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="to-configure-multiple-azure-subscriptions"></a>Birden çok Azure aboneliği yapılandırmak için

Birden çok abonelik desteklemek için PowerShell betiğini kullanın. [kaynak ölçümleri günlük kaydı etkinleştirmek için Azure PowerShell kullanarak](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

Çalışma alanı kaynak Kimliğini sağlamanız &lt;$WSID&gt; (çalışma alanına birden çok kaynaklardan Tanılama verileri Gönder için Enable-AzureRMDiagnostics.ps1) betiği yürütülürken, bir parametre olarak. Çalışma alanı kimliği almak için &lt;$WSID&gt; değiştirin, Tanılama verileri göndermek istediğiniz için &lt;Subıd&gt; abonelik kimliği ile değiştirin &lt;RG_NAME&gt; ile kaynak grubu adı değiştirin &lt;WS_NAME&gt; aşağıdaki betiğindeki çalışma alanı adı ile.

- Birden çok Azure kaynaklarını yapılandırmak için aşağıdaki komutları kullanın:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>Azure CLI

Ölçümleri ve tanılama Azure CLI'yı kullanarak günlük kaydı etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Depolama hesabı kimliği kaynak depolama hesabı için günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlükleri Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

Konusunu okuyun [Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). 

### <a name="resource-manager-template"></a>Resource Manager şablonu

Konusunu okuyun [Resource Manager şablonu kullanarak kaynak oluşturma sırasında tanılama ayarlarını etkinleştirme](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Log Analytics ile Stream 

SQL veritabanı ölçümleri ve tanılama günlükleri akışa Log Analytics'e yerleşik kullanarak **Log Analytics'e gönderme** portalında seçeneği. Log Analytics PowerShell cmdlet'leri, Azure CLI veya Azure İzleyici REST API aracılığıyla bir tanılama ayarını kullanarak da etkinleştirebilirsiniz.

### <a name="installation-overview"></a>Yüklemeye genel bakış

SQL veritabanı fleet izleme Log Analytics ile basit bir işlemdir. Üç adım gereklidir:

1. Log Analytics kaynağını oluşturun.

2. Kayıt ölçümleri ve tanılama günlükleri için veritabanları, oluşturduğunuz Log Analytics kaynağını yapılandırın.

3. Yükleme **Azure SQL Analytics** Azure marketi'ndeki çözüm.

### <a name="create-a-log-analytics-resource"></a>Log Analytics kaynak oluştur

1. Seçin **kaynak Oluştur** sol taraftaki menüden.

2. Seçin **izleme + Yönetim**.

3. **Log Analytics**’i seçin.

4. Log Analytics formunu gerekli olan ek bilgilerle doldurun: çalışma alanı adı, abonelik, kaynak grubu, konum ve fiyatlandırma katmanı.

   ![Log Analytics](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Veritabanlarını kayıt ölçümleri ve tanılama günlükleri için yapılandırma

Azure portalı üzerinden burada veritabanlarını kendi ölçümleri kaydı yapılandırmak için en kolay yoludur. Portalda, SQL veritabanı kaynağınızın gidip seçin **tanılama ayarları**. 

### <a name="install-the-sql-analytics-solution-from-the-gallery"></a>Galeriden SQL Analytics çözümünü yükleme

1. Log Analytics kaynağını oluşturun ve verilerinizi içine akar sonra SQL Analytics çözümünü yükleyin. Giriş sayfasında yan menü seçin **çözüm Galerisi**. Galeride seçin **Azure SQL Analytics** çözümünün yanı sıra, select **Ekle**.

   ![İzleme çözümü](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Giriş sayfanızda **Azure SQL Analytics** kutucuk görünür. SQL Analytics panosunu açmak için bu kutucuğu seçin.

### <a name="use-the-sql-analytics-solution"></a>SQL Analytics çözümünü kullanın

SQL Analytics, SQL veritabanı kaynak hiyerarşisi aracılığıyla taşımanıza izin veren bir hiyerarşik bir panodur. SQL Analytics çözümünü kullanma konusunda bilgi almak için bkz: [SQL Analytics çözümünü kullanarak SQL veritabanını izleme](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Event Hubs'a akış sağlama

SQL veritabanı ölçümleri ve tanılama günlükleri akışa Event Hubs'a yerleşik kullanarak **Stream olay hub'ına** portalında seçeneği. PowerShell cmdlet'lerini, Azure CLI veya Azure İzleyici REST API aracılığıyla bir tanılama ayarını kullanarak, Service Bus kural kimliği de etkinleştirebilirsiniz. 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Olay hub'ları, ölçümleri ve tanılama özellikli yapmanız gerekenler günlüğe kaydeder
Seçili verileri, Event Hubs'a akış sonra Gelişmiş izleme senaryoları etkinleştirmek için bir adım daha yaklaştınız olursunuz. Event Hubs bir olay ardışık düzeni için ön kapı görevi görür. Veri bir olay hub'ına toplandıktan sonra dönüştürülmüş ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işleme/depolama bağdaştırıcısı kullanılarak depolanır. Olay hub'ları akışı üretimlerini bu olayların üretim ayırır. Bu şekilde, olay tüketicileri olaylara kendi zamanlamalarında erişebilir. Event Hubs hakkında daha fazla bilgi için bkz:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Akış özelliği kullanabileceğinize birkaç yolu vardır:

* **Hot yol verilerini Power bı'a akış tarafından hizmet durumu görüntüleme**. Event Hubs, Stream Analytics ve Power BI'ı kullanarak Azure hizmetlerinizi ölçümleri ve tanılama verilerinizi neredeyse gerçek zamanlı Öngörüler kolayca dönüştürebilirsiniz. Verileri işlemek, çıktı olarak kullanmak üzere Power BI ve Stream Analytics ile nasıl bir olay hub'ı ayarladınız genel bakış için bkz. [Stream Analytics ve Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

* **Üçüncü taraf günlüğe kaydetme ve telemetri akışlarını günlüklerine Stream**. Event Hubs akış kullanarak, farklı üçüncü taraf izleme ve günlük analizi çözümleriyle, ölçümleri ve tanılama günlüklerini alabilirsiniz. 

* **Özel telemetri ve günlüğe kaydetme platform derleme**. Zaten bir ürettikleri telemetri platformu varsa veya bir oluşturma dikkate yüksek düzeyde ölçeklenebilir Yayımla-doğasını abone ol olay hub'ları esnek bir şekilde tanılama günlükleri alma olanak tanır. Bkz: [Dan Rosanova'nın küresel ölçekli bir telemetri platform Event Hubs kullanarak Kılavuzu](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Stream depolamaya

SQL veritabanı ölçümleri ve tanılama günlüklerini depolanabilir depolamada yerleşik kullanarak **bir depolama hesabında arşivle** portalında seçeneği. Ayrıca, depolama PowerShell cmdlet'lerini, Azure CLI veya Azure İzleyici REST API aracılığıyla bir tanılama ayarını kullanarak etkinleştirebilirsiniz.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Depolama hesabında şema ölçümleri ve tanılama günlükleri

Ölçümleri ve tanılama günlükleri toplamayı ayarlama sonra ilk veri satırı kullanılamadığında, seçilen depolama hesabında bir depolama kapsayıcısı oluşturulur. Bu bloblarda yapısı şöyledir:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Veya daha basit:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, tüm ölçümler için bir blob adı aşağıdaki gibi olabilir:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Esnek havuz verileri kaydetmek istiyorsanız, blob adı biraz farklıdır:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>Ölçüm ve günlükleri Depolama'dan indirme

Bilgi edinmek için nasıl [depolamadan ölçümleri ve tanılama günlüklerini indirin](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).

## <a name="data-retention-policy-and-pricing"></a>Veri bekletme ilkesi ve fiyatlandırma

Olay hub'ları veya bir depolama hesabı seçerseniz, bekletme ilkesi belirtebilirsiniz. Bu ilke, seçilen zaman süresinden daha eski olan verileri siler. Log Analytics belirtirseniz, bekletme ilkesi seçili fiyatlandırma katmanına bağlıdır. Tanılama telemetrisi yukarıda veri alımı ayrılan her ay ücretsiz birimlerinin tüketimi için geçerlidir. Sağlanan veri alımı ücretsiz birimlerinin ücretsiz çeşitli veritabanları her ay izlemeyi etkinleştirin. Ağır iş yükleri daha etkin veritabanlarıyla boştaki veritabanlarının karşı daha fazla veri içe alma, lütfen unutmayın. Daha fazla bilgi için [Log Analytics fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/). 

Azure SQL Analytics kullanıyorsanız, OMS çalışma alanı Azure SQL Analytics Gezinti menüsünde ve ardından kullanım ve Tahmini maliyetler seçerek kolayca veri alımı tüketiminiz çözümdeki izleyebilirsiniz.

## <a name="metrics-and-logs-available"></a>Ölçümleri ve günlük yok

Lütfen ayrıntılı izleme telemetri içeriğini ölçümlerini ve günlüklerini kullanılabilir Azure SQL veritabanı, elastik havuzlar, yönetilen örneği ve veritabanı yönetilen örneği için bulmak, **özel çözümleme** ve **uygulama geliştirme** kullanarak [SQL Analytics dil](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="all-metrics"></a>Tüm ölçümleri

### <a name="all-metrics-for-elastic-pools"></a>Elastik havuzlar için tüm ölçümleri

|**Kaynak**|**Ölçümler**|
|---|---|
|Elastik havuz|eDTU yüzdesi eDTU kullanıldığında, eDTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, depolama sınırı, XTP depolama yüzdesi |

### <a name="all-metrics-for-azure-sql-database"></a>Azure SQL veritabanı için tüm ölçümleri

|**Kaynak**|**Ölçümler**|
|---|---|
|Azure SQL Database|DTU yüzdesi DTU kullanıldığında, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturumları yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve kilitlenmeler |

## <a name="logs"></a>Günlükler

### <a name="logs-for-managed-instance"></a>Yönetilen örnek için günlükleri

### <a name="resource-usage-stats"></a>Kaynak kullanım İstatistiği

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: ResourceUsageStats|
|Kaynak|Kaynağın adı.|
|ResourceType|Kaynak türünün adı. Her zaman: MANAGEDINSTANCES|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Yönetilen örnek adı.|
|ResourceId|Kaynak URI'si.|
|SKU_s|Yönetilen örnek Ürün SKU'su|
|virtual_core_count_s|Numver, kullanılabilir sanal çekirdekler|
|avg_cpu_percent_s|Ortalama CPU yüzdesi|
|reserved_storage_mb_s|Ayrılmış depolama kapasitesi yönetilen örneği|
|storage_space_used_mb_s|Kullanılan depolama yönetilen örneği|
|io_requests_s|IOPS sayısı|
|io_bytes_read_s|Okunan bayt IOPS|
|io_bytes_written_s|Yazılan bayt IOPS|

### <a name="logs-for-azure-sql-database-and-managed-instance-database"></a>Azure SQL veritabanı ve yönetilen örnek veritabanı için günlükleri

### <a name="query-store-runtime-statistics"></a>Query Store çalışma zamanı istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: QueryStoreRuntimeStatistics|
|OperationName|İşlemin adı. Her zaman: QueryStoreRuntimeStatisticsEvent|
|Kaynak|Kaynağın adı.|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|query_hash_s|Karma sorgu.|
|query_plan_hash_s|Sorgu planı karması.|
|statement_sql_handle_s|Deyim sql tanıtıcısı.|
|interval_start_time_d|Aralığın datetimeoffset işareti 1900-1-1'den başlatın.|
|interval_end_time_d|Son datetimeoffset 1900-1-1'den tıklarının sayısını zaman aralığı.|
|logical_io_writes_d|Mantıksal bir g/ç yazmaları toplam sayısı.|
|max_logical_io_writes_d|Mantıksal bir g/ç sayısı en fazla yürütme yazar.|
|physical_io_reads_d|Fiziksel GÇ Okuma toplam sayısı.|
|max_physical_io_reads_d|Mantıksal bir g/ç sayısı en fazla yürütme okur.|
|logical_io_reads_d|Mantıksal bir g/ç okuma toplam sayısı.|
|max_logical_io_reads_d|Mantıksal bir g/ç sayısı en fazla yürütme okur.|
|execution_type_d|Yürütme türü.|
|count_executions_d|Sorgunun yürütmelerinin sayısı.|
|cpu_time_d|Mikrosaniye cinsinden sorgu tarafından kullanılan toplam CPU zamanı.|
|max_cpu_time_d|Maksimum CPU süresi tüketici tarafından tek bir yürütme mikrosaniye cinsinden.|
|dop_d|Paralellik derecesi toplamı.|
|max_dop_d|Tek yürütme için kullanılan çalıştırılır. paralellik derecesini maks.|
|rowcount_d|Toplam satır sayısı döndürüldü.|
|max_rowcount_d|En fazla tek yürütme döndürülen satır sayısı.|
|query_max_used_memory_d|KB kullanılan bellek miktarı.|
|max_query_max_used_memory_d|Tek bir yürütme KB tarafından kullanılan bellek miktarı en yüksek.|
|duration_d|Mikrosaniye cinsinden toplam yürütme süresi.|
|max_duration_d|En fazla yürütme zamanı tek bir yürütme.|
|num_physical_io_reads_d|Fiziksel okuma toplam sayısı.|
|max_num_physical_io_reads_d|En fazla yürütme başına fiziksel okuma sayısı.|
|log_bytes_used_d|Kullanılan günlük baytı toplam miktarı.|
|max_log_bytes_used_d|En fazla yürütme kullanılan günlük bayt miktarı.|
|query_id_d|Query Store sorgu kimliği.|
|plan_id_d|Query Store planında kimliği.|

Daha fazla bilgi edinin [Query Store çalışma zamanı istatistik verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Query Store bekleme istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: QueryStoreWaitStatistics|
|OperationName|İşlemin adı. Her zaman: QueryStoreWaitStatisticsEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|wait_category_s|Bekleme kategorisi.|
|is_parameterizable_s|Sorgu parametrelenebilir gereklidir.|
|statement_type_s|Deyim türü.|
|statement_key_hash_s|Deyimi anahtar karması.|
|exec_type_d|Yürütme türü.|
|total_query_wait_time_ms_d|Belirli bir bekleme kategoriye sorgunun toplam bekleme süresi.|
|max_query_wait_time_ms_d|Belirli bir bekleme kategorisi üzerinde tek tek yürütme sorgunun maksimum bekleme süresi.|
|query_param_type_d|0|
|query_hash_s|Query Store içinde karma sorgu.|
|query_plan_hash_s|Sorgu planı Query Store karma değeri.|
|statement_sql_handle_s|Query Store içinde deyim tanıtıcı.|
|interval_start_time_d|Aralığın datetimeoffset işareti 1900-1-1'den başlatın.|
|interval_end_time_d|Son datetimeoffset 1900-1-1'den tıklarının sayısını zaman aralığı.|
|count_executions_d|Sorgu yürütme sayısı.|
|query_id_d|Query Store sorgu kimliği.|
|plan_id_d|Query Store planında kimliği.|

Daha fazla bilgi edinin [Query Store bekleme istatistikleri veri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Hataları veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: hataları|
|OperationName|İşlemin adı. Her zaman: ErrorEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|İleti|Düz metin hata iletisi.|
|user_defined_b|Kullanıcı tanımlı hata bitidir.|
|error_number_d|Hata kodu.|
|Severity|Hata önem derecesi.|
|state_d|Hata durumu.|
|query_hash_s|Sorgu karması varsa başarısız sorgu.|
|query_plan_hash_s|Sorgu planı karması varsa başarısız sorgu.|

Daha fazla bilgi edinin [SQL Server hata iletileri](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Veritabanı bekleme istatistikleri veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: DatabaseWaitStatistics|
|OperationName|İşlemin adı. Her zaman: DatabaseWaitStatisticsEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|wait_type_s|Bekleme türünün adı.|
|start_utc_date_t [UTC]|Dönem başlangıç zamanı ölçülür.|
|end_utc_date_t [UTC]|Ölçülen dönemi bitiş saati.|
|delta_max_wait_time_ms_d|En fazla beklenen süre yürütme başına|
|delta_signal_wait_time_ms_d|Sinyal toplam bekleme süresi.|
|delta_wait_time_ms_d|Dönemi içindeki toplam bekleme süresi.|
|delta_waiting_tasks_count_d|Bekleyen Görevler sayısı.|

Daha fazla bilgi edinin [bekleme istatistikleri veritabanı](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Zaman aşımlarını veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: zaman aşımı|
|OperationName|İşlemin adı. Her zaman: TimeoutEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|error_state_d|Hata durum kodu.|
|query_hash_s|Karma değeri varsa sorgulayın.|
|query_plan_hash_s|Sorgu planı karma varsa.|

### <a name="blockings-dataset"></a>Blockings veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: blokları|
|OperationName|İşlemin adı. Her zaman: BlockEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|lock_mode_s|Sorgu tarafından kullanılan kilit modu.|
|resource_owner_type_s|Kilit sahibi.|
|blocked_process_filtered_s|İşlem raporu XML engellendi.|
|duration_d|Mikrosaniye cinsinden süresi kilit.|

### <a name="deadlocks-dataset"></a>Kilitlenmeler veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC] |Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: kilitlenmeler|
|OperationName|İşlemin adı. Her zaman: DeadlockEvent|
|Kaynak|Kaynağın adı.|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı. |
|ResourceId|Kaynak URI'si.|
|deadlock_xml_s|Rapor XML kilitlenme.|

### <a name="automatic-tuning-dataset"></a>Otomatik ayarlama veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlüğe kaydedildiği zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcı adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: AutomaticTuning|
|Kaynak|Kaynağın adı.|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait olduğu sunucu adı.|
|LogicalDatabaseName_s|Veritabanının adı.|
|ElasticPoolName_s|Veritabanı varsa ait elastik havuzunun adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|RecommendationHash_s|Otomatik ayarlama önerileri benzersiz karması.|
|OptionName_s|Otomatik ayarlama işlemi.|
|Schema_s|Veritabanı şeması.|
|Table_s|Etkilenen tablo.|
|IndexName_s|Dizin adı.|
|IndexColumns_s|Sütun adı.|
|IncludedColumns_s|Dahil edilen sütun.|
|EstimatedImpact_s|Otomatik ayarlama önerileri JSON etkisini tahmin.|
|Event_s|Otomatik ayarlama olay türü.|
|Timestamp_t|Son güncelleştirilen zaman damgası.|

### <a name="intelligent-insights-dataset"></a>Intelligent Insights veri kümesi
Daha fazla bilgi edinin [Intelligent Insights günlük biçimi](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Sonraki adımlar

Günlük kaydını etkinleştirmek ve çeşitli Azure Hizmetleri tarafından desteklenen Ölçümler ve günlük kategorileri anlama hakkında bilgi edinmek için:

 * [Microsoft azure'da ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Azure tanılama günlüklerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

Event Hubs hakkında bilgi edinmek için:

* [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Depolama hakkında daha fazla bilgi için bkz. nasıl [ölçümleri ve tanılama günlüklerini Depolama'dan indirme](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).
