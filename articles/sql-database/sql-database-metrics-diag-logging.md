---
title: Azure SQL veritabanı ölçümleri ve tanılama günlükleri | Microsoft Docs
description: Kaynak kullanımı, bağlantı ve sorgu yürütme istatistikleri depolamak için Azure SQL veritabanını yapılandırma hakkında bilgi edinin.
services: sql-database
documentationcenter: ''
author: veljko-msft
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 03/16/2018
ms.author: vvasic
ms.openlocfilehash: b1ac34c97d94f0b8759cb3e6f229ba0f7a2be7c9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL Database ölçümleri ve tanılama günlükleri 
Azure SQL veritabanı yayma ölçümleri ve tanılama daha kolay izleme günlükleri. SQL Veritabanını kaynak kullanımını, çalışanları, oturumları ve bu Azure kaynaklarından birine yapılan bağlantıları kaydedecek şekilde yapılandırabilirsiniz:

* **Azure depolama**: telemetri küçük bir fiyat için çok büyük miktarda arşivlemek için kullanılır.
* **Azure Event Hubs**: SQL veritabanı telemetri özel izleme çözümü veya etkin işlem hatları ile tümleştirmek için kullanılır.
* **Azure günlük analizi**: Giden kutusu izleme çözümünü raporlama, uyarı ve yetenekleri Azaltıcı için kullanılır.

    ![Mimari](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Ölçümleri ve Tanılama Günlüğü varsayılan olarak etkinleştirilmedi. Etkinleştirme ve ölçümleri ve aşağıdaki yöntemlerden birini kullanarak oturum tanılama yönetebilirsiniz:

- Azure portalına
- PowerShell
- Azure CLI
- Azure monitör REST API'si 
- Azure Resource Manager şablonu

Ölçümleri ve tanılama günlükleri etkinleştirdiğinizde, burada seçilen verileri toplanır Azure kaynak belirtmeniz gerekir. Kullanılabilir seçenekler şunlardır:

- Log Analytics
- Event Hubs
- Depolama 

Yeni bir Azure kaynak sağlama veya var olan bir kaynak seçin. Depolama kaynağı seçtikten sonra hangi verileri toplamak için belirtmeniz gerekir. Kullanılabilir seçenekler şunlardır:

- [Tüm ölçümleri](sql-database-metrics-diag-logging.md#all-metrics): içeren DTU yüzdesi, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma ve yüzde başarılı/başarısız/engellenen Güvenlik Duvarı bağlantılarını, oturumlar yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi ve XTP depolama yüzdesi.
- [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): CPU kullanımı ve sorgu süresi gibi sorgu çalışma zamanı istatistikleri hakkında bilgiler içerir.
- [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): ne sorgularınızı, CPU, günlük ve KİLİTLEME gibi beklediğini bildirir sorgu bekleme İstatistikler hakkında bilgi içerir.
- [Hataları](sql-database-metrics-diag-logging.md#errors-dataset): Bu veritabanında oldu SQL hatalar hakkında bilgi içerir.
- [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): hakkında ne kadar bir veritabanı zaman harcanan farklı bekleme türlerinde bekleyen bilgileri içerir.
- [Zaman aşımları](sql-database-metrics-diag-logging.md#time-outs-dataset): bir veritabanı üzerinde gerçekleşen zaman aşımları hakkında bilgiler içerir.
- [Blockings](sql-database-metrics-diag-logging.md#blockings-dataset): bir veritabanı üzerinde gerçekleşen olayları engelleme hakkında bilgi içerir.
- [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): Akıllı Öngörüler içerir. [Akıllı Insights hakkında daha fazla bilgi](sql-database-intelligent-insights.md).

Olay hub'ları veya depolama hesabı seçerseniz, bir bekletme ilkesi belirtebilirsiniz. Bu ilke seçilen zaman süresinden daha eski olan verileri siler. Günlük analizi belirtirseniz, bekletme ilkesi seçilen fiyatlandırma katmanını bağlıdır. Daha fazla bilgi için bkz: [günlük analizi fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/). 

Günlük kaydını etkinleştirmek ve çeşitli Azure Hizmetleri tarafından desteklenen ölçümleri ve günlük kategorileri anlama hakkında bilgi edinmek için okumanızı öneririz: 

* [Microsoft Azure ölçümlerini genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Azure tanılama günlükleri'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

### <a name="azure-portal"></a>Azure portalına

1. Portalda ölçümleri ve tanılama günlükleri toplamayı etkinleştirmek için SQL veritabanınız veya esnek havuz sayfasına gidin ve ardından **tanılama ayarları**.

   ![Azure portalında etkinleştir](./media/sql-database-metrics-diag-logging/enable-portal.png)

2. Yeni oluşturabilir veya varolan tanılama ayarları hedef ve telemetri seçerek düzenleyebilirsiniz.

   ![Tanılama ayarları](./media/sql-database-metrics-diag-logging/diagnostics-portal.png)

### <a name="powershell"></a>PowerShell

Ölçümleri ve PowerShell kullanarak günlüğü Tanılama'yı etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlüklerinin bir depolama hesabındaki depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı depolama hesabı için kaynak kimliği günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlüklerini bir olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği bu biçiminde bir dize şöyledir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Günlük analizi çalışma alanı için gönderen tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Günlük analizi çalışma alanınız kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıktı seçenekleri etkinleştirmek için bu parametreler birleştirebilirsiniz.

### <a name="to-configure-multiple-azure-resources"></a>Birden çok Azure kaynaklarını yapılandırmak için

Birden çok abonelik desteklemek için PowerShell komut dosyasını kullanın [PowerShell kullanarak etkinleştirmek Azure kaynak ölçümleri günlük](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

Çalışma alanı kaynak kimliği sağlayın &lt;$WSID&gt; (çalışma alanına birden çok kaynaklardan tanılama veri göndermesini etkinleştir-AzureRMDiagnostics.ps1) komut dosyası yürütülürken bir parametre olarak. Çalışma alanı kimliği almak için &lt;$WSID&gt; olan Tanılama verileri göndermek istediğinizi için değiştirmek &lt;subID&gt; abonelik kimliği ile Değiştir &lt;RG_NAME&gt; kaynak grubu adı ve değiştirme &lt;WS_NAME&gt; aşağıdaki komut dosyası çalışma ada sahip.

- Birden çok Azure kaynaklarını yapılandırmak için aşağıdaki komutları kullanın:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>Azure CLI

Ölçümleri ve tanılama Azure CLI kullanarak günlüğü etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlüklerinin bir depolama hesabındaki depolama etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Depolama hesabı depolama hesabı için kaynak kimliği günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlüklerini bir olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Hizmet veri yolu kural kimliği bu biçiminde bir dize şöyledir:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Günlük analizi çalışma alanı için gönderen tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Birden çok çıktı seçenekleri etkinleştirmek için bu parametreler birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

Konusunu okuyun [Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirme](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager şablonu

Konusunu okuyun [Resource Manager şablonu kullanarak kaynak oluşturmada tanılama ayarları etkinleştirmek](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Günlük analizi akışa 
SQL veritabanı ölçümleri ve tanılama günlükleri akışı günlük analizi yerleşik kullanarak **için günlük analizi Gönder** portalında seçeneği. PowerShell cmdlet'leri, Azure CLI veya Azure İzleyici REST API'si aracılığıyla tanılama ayarını kullanarak, günlük analizi de etkinleştirebilirsiniz.

### <a name="installation-overview"></a>Yüklemeye genel bakış

SQL veritabanı Donanma izleme günlük analizi ile basit bir işlemdir. Üç adım gerekli değildir:

1. Günlük analizi kaynağı oluşturun.

2. Kayıt ölçümleri ve tanılama günlüklerini veritabanlarına oluşturduğunuz günlük analizi kaynak yapılandırın.

3. Yükleme **Azure SQL analizi** günlük analizi Galerisi'nde çözüm.

### <a name="create-a-log-analytics-resource"></a>Günlük analizi kaynak oluşturma

1. Seçin **kaynak oluşturma** soldaki menüde.

2. Seçin **izleme + Yönetim**.

3. **Log Analytics**’i seçin.

4. Gerekli ek bilgileri içeren günlük analizi formu doldurun: çalışma alanı adı, abonelik, kaynak grubu, konum ve fiyatlandırma katmanı.

   ![Log Analytics](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Kayıt ölçümleri ve tanılama günlüklerine veritabanlarını yapılandırın

Azure portalı üzerinden nerede veritabanları kendi ölçümleri kaydı yapılandırmak için en kolay yoludur. Portalda, SQL veritabanı kaynağınızın gidip seçin **tanılama ayarları**. 

### <a name="install-the-sql-analytics-solution-from-the-gallery"></a>Galeriden SQL analiz çözümü yükleyin

1. Günlük analizi kaynak oluşturmak ve verilerinizi içine akan sonra SQL analiz çözümü yükleyin. Yan menüde giriş sayfasında seçin **Çözümleri Galerisi**. Galeride seçin **Azure SQL analizi** çözümü ve select **Ekle**.

   ![İzleme çözümü](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Giriş sayfası, **Azure SQL analizi** döşeme görüntülenir. SQL analizi panosunu açmak için bu kutucuğu seçin.

### <a name="use-the-sql-analytics-solution"></a>SQL analiz çözümü kullanın

SQL analizi SQL veritabanı kaynak hiyerarşisi aracılığıyla taşımanızı sağlar hiyerarşik bir Pano ' dir. SQL analiz çözümü kullanmayı öğrenmek için bkz: [SQL analiz çözümü kullanarak İzleyici SQL veritabanı](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Olay hub'ları akışa

SQL veritabanı ölçümleri ve tanılama günlükleri akışı Event Hubs'a yerleşik kullanarak **bir olay hub'ına akış** portalında seçeneği. PowerShell cmdlet'leri, Azure CLI veya Azure İzleyici REST API'si aracılığıyla tanılama ayarını kullanarak Service Bus kural kimliği de etkinleştirebilirsiniz. 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Olay hub ' günlükleri ölçümleri ve tanılama yapmanız gerekenler
Seçili verileri Event Hubs'a akışı sonra Gelişmiş izleme senaryoları etkinleştirmek için bir adım daha yaklaşarak artık demektir. Event Hubs bir olay ardışık düzeni için ön kapı görevi görür. Veriler bir event hub'ına toplandıktan sonra dönüştürülmüş ve tüm gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak depolanır. Olay hub'ları bu olayların tüketilmesinden akışı üretim ayrıştırır. Bu şekilde, olay tüketicileri olayları kendi zamanlamalarında erişebilir. Event Hubs hakkında daha fazla bilgi için bkz:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Akış özelliği kullanabilir birkaç şekilde şunlardır:

* **Power BI sık yolu veri akış tarafından hizmet durumu görüntülemek**. Event Hubs, akış analizi ve Power BI'ı kullanarak Azure hizmetlerinizi ölçümleri ve tanılama verilerinizi yakın gerçek zamanlı Öngörüler kolayca dönüştürebilirsiniz. Akış analizi ve bir çıkış olarak kullanmak üzere Power BI ile işlem verilerini olay hub'ı ayarlamak nasıl genel bakış için bkz [akış analizi ve Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

* **Akış günlükleri üçüncü taraf günlüğe kaydetme ve telemetri akışlara**. Olay hub'ları akış kullanarak farklı üçüncü taraf izleme ve günlük analizi çözümleriyle ölçümleri ve tanılama günlükleri alabilirsiniz. 

* **Bir özel telemetri ve günlüğe kaydetme platformu yapı**. Zaten bir özel olarak geliştirilmiş telemetri platform veya bir derleme dikkate alarak, yüksek düzeyde ölçeklenebilir yayımlama yapısını abonelik Event Hubs, esnek tanılama günlüklerini alma olanak sağlar. Bkz: [bir küresel ölçekli telemetri platform olay hub'ları kullanarak Dan Rosanova'nın kılavuzuna](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Akış depolama alanına

SQL veritabanı ölçümleri ve tanılama günlükleri depolanabilir depolama yerleşik kullanarak **bir depolama hesabı arşive** portalında seçeneği. PowerShell cmdlet'leri, Azure CLI veya Azure İzleyici REST API'si aracılığıyla tanılama ayarını kullanarak depolama daha da etkinleştirebilirsiniz.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Depolama hesabında şema ölçümleri ve tanılama günlükleri

Ölçümleri ve tanılama günlükleri toplamayı ayarladıktan sonra bir depolama kapsayıcısı ilk veri satırı kullanılamadığında, seçtiğiniz depolama hesabı oluşturulur. Bu BLOB'ları yapıdır:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Veya daha basit bir şekilde:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir blob adı tüm ölçümünün olabilir:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Esnek havuz verileri kaydetmek istiyorsanız, blob adı biraz farklıdır:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>İndirme ölçümleri ve depolama biriminden günlükleri

Bilgi edinmek için nasıl [depolama biriminden ölçümleri ve tanılama günlüklerini indirin](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).

## <a name="metrics-and-logs-available"></a>Ölçümleri ve günlük yok

### <a name="all-metrics"></a>Tüm ölçümleri

|**Kaynak**|**Ölçümler**|
|---|---|
|Database|DTU yüzdesi DTU kullanıldığında, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen Güvenlik Duvarı bağlantılarını, oturumlar yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve kilitlenmeleri |
|Esnek havuz|eDTU yüzde eDTU kullanıldığında, eDTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturumlar yüzdesi, çalışanları yüzdesi, depolama, depolama yüzdesi, depolama sınırı, XTP depolama yüzdesi |
|||

### <a name="query-store-runtime-statistics"></a>Query Store çalışma zamanı istatistikleri

|Özellik|Açıklama|
|---|---|
|Tenantıd|Kiracı kimliğinizi|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlük kaydedilirken zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: QueryStoreRuntimeStatistics|
|OperationName|İşlemin adı. Her zaman: QueryStoreRuntimeStatisticsEvent|
|Kaynak|Kaynağın adı.|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait sunucunun adıdır.|
|ElasticPoolName_s|Veritabanı varsa ait esnek havuz adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|query_hash_s|Karma sorgu.|
|query_plan_hash_s|Sorgu planı karması.|
|statement_sql_handle_s|Deyim sql tanıtıcısı.|
|interval_start_time_d|Datetimeoffset aralığının 1900 1 1 sayısı başlatın.|
|interval_end_time_d|Son datetimeoffset 1900 1 1 sayısı aralığı.|
|logical_io_writes_d|Mantıksal g/ç yazmaları toplam sayısı.|
|max_logical_io_writes_d|Mantıksal bir g/ç sayısı üst sınırı yürütme başına yazar.|
|physical_io_reads_d|Fiziksel GÇ Okuma toplam sayısı.|
|max_physical_io_reads_d|Mantıksal bir g/ç sayısı üst sınırı yürütme başına okur.|
|logical_io_reads_d|Mantıksal g/ç okuma toplam sayısı.|
|max_logical_io_reads_d|Mantıksal bir g/ç sayısı üst sınırı yürütme başına okur.|
|execution_type_d|Yürütme türü.|
|count_executions_d|Sorgu yürütmeleri sayısı.|
|cpu_time_d|Mikrosaniye cinsinden sorgu tarafından kullanılan toplam CPU süresi.|
|max_cpu_time_d|En fazla CPU süresi tüketici tarafından mikrosaniye cinsinden tek bir yürütme.|
|dop_d|Paralellik derecesi toplamı.|
|max_dop_d|Maksimum paralellik derecesi tek yürütme için kullanılır.|
|rowcount_d|Döndürülen satırların toplam sayısı.|
|max_rowcount_d|En fazla tek yürütme döndürülen satır sayısı.|
|query_max_used_memory_d|KB kullanılan bellek toplam miktarı.|
|max_query_max_used_memory_d|Max KB tek bir yürütme tarafından kullanılan bellek miktarıdır.|
|duration_d|Mikrosaniye cinsinden toplam yürütme süresi.|
|max_duration_d|Tek bir yürütme maksimum yürütme süresi.|
|num_physical_io_reads_d|Fiziksel okuma toplam sayısı.|
|max_num_physical_io_reads_d|En fazla fiziksel okuma başına yürütme sayısı.|
|log_bytes_used_d|Kullanılan günlük bayt toplam miktarı.|
|max_log_bytes_used_d|En fazla yürütme başına kullanılan günlük bayt miktarı.|
|query_id_d|Query Store sorguda kimliği.|
|plan_id_d|Query Store planında kimliği.|

Daha fazla bilgi edinmek [Query Store çalışma zamanı istatistik verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Query Store bekleme istatistikleri

|Özellik|Açıklama|
|---|---|
|Tenantıd|Kiracı kimliğinizi|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlük kaydedilirken zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: QueryStoreWaitStatistics|
|OperationName|İşlemin adı. Her zaman: QueryStoreWaitStatisticsEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait sunucunun adıdır.|
|ElasticPoolName_s|Veritabanı varsa ait esnek havuz adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|wait_category_s|Bekleme kategorisi.|
|is_parameterizable_s|Sorgunun parametrelenebilir değil.|
|statement_type_s|İfade türü.|
|statement_key_hash_s|Deyim anahtar karması.|
|exec_type_d|Yürütme türü.|
|total_query_wait_time_ms_d|Belirli bekleme kategorisine sorgusunun toplam bekleme süresi.|
|max_query_wait_time_ms_d|Belirli bekleme kategorisine tek tek yürütmesinde sorgusunun en fazla bekleme süresi.|
|query_param_type_d|0|
|query_hash_s|Karma sorgu Deposu'nda sorgu.|
|query_plan_hash_s|Sorgu planı karma sorgu Deposu'nda.|
|statement_sql_handle_s|Sorgu Deposu'nda deyim tanıtıcısı.|
|interval_start_time_d|Datetimeoffset aralığının 1900 1 1 sayısı başlatın.|
|interval_end_time_d|Son datetimeoffset 1900 1 1 sayısı aralığı.|
|count_executions_d|Sorgu yürütmeleri sayısı.|
|query_id_d|Query Store sorguda kimliği.|
|plan_id_d|Query Store planında kimliği.|

Daha fazla bilgi edinmek [Query Store bekleyin istatistik verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Veri kümesi hataları

|Özellik|Açıklama|
|---|---|
|Tenantıd|Kiracı kimliğinizi|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlük kaydedilirken zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: hataları|
|OperationName|İşlemin adı. Her zaman: ErrorEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait sunucunun adıdır.|
|ElasticPoolName_s|Veritabanı varsa ait esnek havuz adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|İleti|Hata iletisi düz metin.|
|user_defined_b|Kullanıcı tanımlı hata bitidir.|
|error_number_d|Hata kodu.|
|Önem Derecesi|Hata önem derecesi.|
|state_d|Hata durumu.|
|query_hash_s|Başarısız sorgu varsa karmasını sorgu.|
|query_plan_hash_s|Başarısız sorgu varsa sorgu planı karması.|

Daha fazla bilgi edinmek [SQL Server hata iletileri](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Veritabanı bekleme istatistikleri veri kümesi

|Özellik|Açıklama|
|---|---|
|Tenantıd|Kiracı kimliğinizi|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlük kaydedilirken zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: DatabaseWaitStatistics|
|OperationName|İşlemin adı. Her zaman: DatabaseWaitStatisticsEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait sunucunun adıdır.|
|ElasticPoolName_s|Veritabanı varsa ait esnek havuz adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|wait_type_s|Bekleme türünün adı.|
|start_utc_date_t [UTC]|Süresi başlangıç zamanı ölçülür.|
|end_utc_date_t [UTC]|Ölçülen dönem bitiş saati.|
|delta_max_wait_time_ms_d|Max beklenen süre yürütme başına|
|delta_signal_wait_time_ms_d|Toplam sinyal bekleme süresi.|
|delta_wait_time_ms_d|Dönemdeki toplam bekleme süresi.|
|delta_waiting_tasks_count_d|Bekleyen görev sayısı.|

Daha fazla bilgi edinmek [veritabanı bekleme istatistikleri](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Zaman aşımları veri kümesi

|Özellik|Açıklama|
|---|---|
|Tenantıd|Kiracı kimliğinizi|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlük kaydedilirken zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: zaman aşımı|
|OperationName|İşlemin adı. Her zaman: TimeoutEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait sunucunun adıdır.|
|ElasticPoolName_s|Veritabanı varsa ait esnek havuz adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|error_state_d|Hata durum kodu.|
|query_hash_s|Karma, varsa sorgu.|
|query_plan_hash_s|Varsa planı karma sorgu.|

### <a name="blockings-dataset"></a>Blockings veri kümesi

|Özellik|Açıklama|
|---|---|
|Tenantıd|Kiracı kimliğinizi|
|SourceSystem|Her zaman: Azure|
|TimeGenerated [UTC]|Günlük kaydedilirken zaman damgası.|
|Tür|Her zaman: AzureDiagnostics|
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. SQL|
|Kategori|Kategori adı. Her zaman: blokları|
|OperationName|İşlemin adı. Her zaman: BlockEvent|
|Kaynak|Kaynağın adı|
|ResourceType|Kaynak türünün adı. Her zaman: Sunucuları/veritabanları|
|SubscriptionId|Abonelik veritabanının ait GUID.|
|ResourceGroup|Veritabanının ait kaynak grubunun adı.|
|LogicalServerName_s|Veritabanının ait sunucunun adıdır.|
|ElasticPoolName_s|Veritabanı varsa ait esnek havuz adı.|
|DatabaseName_s|Veritabanının adı.|
|ResourceId|Kaynak URI'si.|
|lock_mode_s|Sorgu tarafından kullanılan kilit modu.|
|resource_owner_type_s|Kilit sahibi.|
|blocked_process_filtered_s|İşlem raporu XML engellendi.|
|duration_d|Mikrosaniye cinsinden süresi kilit.|

### <a name="intelligent-insights-dataset"></a>Akıllı Öngörüler veri kümesi
Daha fazla bilgi edinmek [akıllı Öngörüler günlük biçimi](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Sonraki adımlar

Günlük kaydını etkinleştirmek ve çeşitli Azure Hizmetleri tarafından desteklenen ölçümleri ve günlük kategorileri anlama hakkında bilgi edinmek için okuyun:

 * [Microsoft Azure ölçümlerini genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Azure tanılama günlükleri'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

Olay hub'ları hakkında bilgi edinmek için okuyun:

* [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Depolama hakkında daha fazla bilgi için bkz: nasıl yapılır [depolama biriminden ölçümleri ve tanılama günlüklerini indirin](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).
