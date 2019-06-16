---
title: Azure tanılama günlükleri, hizmetler ve şemalar desteklenir
description: Azure tanılama günlükleri için desteklenen hizmet ve olay şema anlayın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/11/2018
ms.author: robb
ms.subservice: logs
ms.openlocfilehash: 21eec5ee2fef185a927f6a416732303765e02b1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65789315"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Desteklenen hizmetler, şemalar ve Azure tanılama günlükleri için kategorileri

[Azure İzleyici tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) olan işlemi bu hizmetlerin veya kaynakları tanımlayan Azure Hizmetleri tarafından günlüklerdir. Azure İzleyici kullanılabilir tüm tanılama günlükleri esneklik her hizmet kendi olaylar için benzersiz özellikler yaymak için ortak bir üst düzey şema paylaşın.

Kaynak türü bileşimi (kullanılabilir `resourceId` özelliği) ve `category` bir şema benzersiz olarak tanımlanabilmesi. Bu makalede tanılama günlükleri ve her hizmet için şemaların bağlantılar için üst düzey şema açıklanır.

## <a name="top-level-diagnostic-logs-schema"></a>Şema en üst düzey tanılama günlükleri

| Ad | Gerekli/isteğe bağlı | Açıklama |
|---|---|---|
| time | Gerekli | Olayın zaman damgası (UTC). |
| resourceId | Gerekli | Olay yayılan kaynağının kaynak kimliği. Kiracı Hizmetleri için form /tenants/tenant-id/providers/provider-name budur. |
| tenantId | Kiracı günlükleri için gerekli | Bu olay bağlıdır Active Directory kiracısı Kiracı kimliği. Bu özellik, yalnızca kaynak düzeyi günlüklerde görünmüyor Kiracı düzeyinde günlükleri için kullanılır. |
| operationName | Gerekli | Bu olay tarafından temsil edilen işlemin adı. Bir RBAC işlem olayı temsil ediyorsa, bu RBAC işlemi (örn. addır Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Gerçek belgelenmiş Resource Manager işlemlerini olmasa bile genellikle bir Resource Manager işlem biçiminde modellenmiş (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | İsteğe bağlı | OperationName (örn. bir API kullanarak gerçekleştirdiyseniz, işlemle ilişkili api-version `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Bu işlem için karşılık gelen hiçbir API varsa, sürüm sürümü bu işlem, işlemle ilişkili özellikler gelecekte değiştirilmesini durumunda temsil eder. |
| category | Gerekli | Olay günlüğü kategorisi. Ayrıntı düzeyi, etkinleştirme veya devre dışı belirli bir kaynağa açtığında kategorisidir. Bir olayın özellikleri blob içinde görünen özellikleri belirli günlük kategorisi ve kaynak türü içinde aynıdır. Tipik günlük kategorileri "Denetleme" "işlemsel" "Yürütme" ve "İstek" olan |
| resultType | İsteğe bağlı | Olay durumu. Başlarken, sürüyor, başarılı, başarısız, etkin ve Çözümlenmiş tipik değerler içerir. |
| resultSignature | İsteğe bağlı | Olay alt durumu. Bu işlem için bir REST API çağrısı karşılık geliyorsa, karşılık gelen REST çağrısı HTTP durum kodunu budur. |
| resultDescription | İsteğe bağlı | Bu işlem statik metin açıklamasını örn. "Get. depolama dosyası." |
| durationMs | İsteğe bağlı | Milisaniye cinsinden işlem süresi. |
| callerIpAddress | İsteğe bağlı | Genel olarak kullanılabilir bir IP adresi içeren bir varlıktan gelecektir bir API çağrısına işlemi karşılık geliyorsa arayan IP adresi. |
| correlationId | İsteğe bağlı | Bir dizi ilgili olayları gruplamak için kullanılan bir GUID. Genellikle, iki olay iki farklı durumları ancak aynı operationName (örn. varsa "Başlangıç" ve "Başarılı"), bunlar aynı bağıntı kimliği paylaşıyor. Bu, ayrıca diğer olayları ilişkilerini temsil edebilir. |
| identity | İsteğe bağlı | İşlemi gerçekleştiren uygulama ve kullanıcı kimliğini açıklayan bir JSON blob. Genellikle bu yetkilendirme ve talep içerecektir / active Directory JWT belirteci. |
| Düzey | İsteğe bağlı | Olay önem derecesi. Bilgi, uyarı, hata veya kritik biri olmalıdır. |
| location | İsteğe bağlı | Örneğin olay yayma Kaynak bölgesi. "Doğu ABD" veya "Fransa Güney" |
| properties | İsteğe bağlı | Herhangi bir genişletilmiş bu belirli olayları kategorisine ilgili özellikler. Tüm özel/benzersiz özellikler bu "bölümü B" şeması yerleştirmeniz gerekir. |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>Kaynak tanılama günlükleri için hizmete özel şemalar
Kaynak tanılama günlükleri için şema, kaynak ve günlük kategorisine bağlı olarak değişir. Bu liste, mevcut tanılama günlükleri ve bağlantılar hizmet ve kategoriye özel şema mevcut olduğunda tüm hizmetlerle gösterir.

| Hizmet | Şema ve belgeler |
| --- | --- |
| Azure Active Directory | [Genel Bakış](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [denetim günlüğü şeması](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) ve [oturum açma şeması](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [API Management tanılama günlükleri](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Uygulama Ağ Geçitleri |[Uygulama ağ geçidi için tanılama günlüğünü](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Otomasyonu |[Azure otomasyonu için log analytics](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch tanılama günlüğüne kaydetme](../../batch/batch-diagnostics.md) |
| MySQL için Azure Veritabanı | [Tanılama günlükleri MySQL için Azure veritabanı](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| PostgreSQL için Azure Veritabanı | [Tanılama günlükleri PostgreSQL için Azure veritabanı](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Bilişsel Hizmetler | Şema kullanılabilir değil. |
| Content Delivery Network | [CDN için Azure tanılama günlükleri](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB günlüğe kaydetme](../../cosmos-db/logging.md) |
| Data Factory | [Azure İzleyicisi'ni kullanarak veri Fabrikalarını izleme](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Azure Data Lake Analytics’te tanılama günlüklerine erişim](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Azure Data Lake Store tanılama günlüklerine erişme](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs tanılama günlükleri](../../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | Şema kullanılabilir değil. |
| Azure Güvenlik Duvarı | Şema kullanılabilir değil. |
| IoT Hub | [IOT Hub işlemlerini](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure Anahtar Kasası Günlüğü](../../key-vault/key-vault-logging.md) |
| Load Balancer |[Azure Load Balancer için Log Analytics](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B özel izleme şeması](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Ağ Güvenlik Grupları |[Ağ güvenlik grupları (NSG’ler) için Log Analytics](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Koruması | [Azure DDoS koruma standardını yönetme](../../virtual-network/manage-ddos-protection.md) |
| Power BI ayrılmış | [Azure'da Power BI Embedded için tanılama günlüğüne kaydetme](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Kurtarma Hizmetleri | [Azure yedekleme için veri modeli](../../backup/backup-azure-reports-data-model.md)|
| Ara |[Etkinleştirme ve arama trafiği analizi kullanma](../../search/search-traffic-analytics.md) |
| Service Bus |[Azure Service Bus tanılama günlükleri](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Veritabanı | [Azure SQL veritabanı tanılama günlüğüne kaydetme](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[İş tanılama günlükleri](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Traffic Manager günlüğü şeması](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Sanal Ağlar | Şema kullanılabilir değil. |
| Sanal Ağ Geçitleri | Şema kullanılabilir değil. |

## <a name="supported-log-categories-per-resource-type"></a>Desteklenen kaynak türü başına günlük kategorileri
|Kaynak Türü|Kategori|Kategori görünen adı|
|---|---|---|
|Microsoft.AnalysisServices/servers|Altyapısı|Altyapısı|
|Microsoft.AnalysisServices/servers|Hizmet|Hizmet|
|Microsoft.ApiManagement/service|GatewayLogs|ApiManagement ağ geçidine ilgili Günlükler|
|Microsoft.Automation/automationAccounts|JobLogs|İş günlükleri|
|Microsoft.Automation/automationAccounts|JobStreams|İş akışları|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC düğümü durumu|
|Microsoft.Batch/batchAccounts|ServiceLog|Hizmeti günlükleri|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Örneğin, bant genişliği, çıkış, vb. uç ölçümleri alır.|
|Microsoft.ClassicNetwork/networksecuritygroups|Ağ güvenlik grubu kural akışı olayı|Ağ güvenlik grubu kural akışı olayı|
|Microsoft.CognitiveServices/accounts|Denetim|Denetim Günlükleri|
|Microsoft.CognitiveServices/accounts|RequestResponse|İstek ve yanıt günlükleri|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Kubernetes API sunucusu|
|Microsoft.ContainerService/managedClusters|kube denetleyicisi Yöneticisi|Kubernetes denetleyicisi Yöneticisi|
|Microsoft.ContainerService/managedClusters|Küme otomatik ölçeklendiricinin|Kubernetes kümesi Ölçeklendiriciyi|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Kubernetes Scheduler|
|Microsoft.ContainerService/managedClusters|koruma|Kimlik doğrulama Web kancası|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|İşlem hattı etkinliği çalıştırmaları günlüğü|
|Microsoft.DataFactory/factories|PipelineRuns|Günlük işlem hattı çalıştırmaları|
|Microsoft.DataFactory/factories|TriggerRuns|Günlük tetikleyici çalıştırmaları|
|Microsoft.DataLakeAnalytics/accounts|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeAnalytics/accounts|İstekler|Günlükleri iste|
|Microsoft.DataLakeStore/accounts|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeStore/accounts|İstekler|Günlükleri iste|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|MySQL sunucusu günlükleri|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL sunucusu günlükleri|
|Microsoft.Devices/ıothubs|Bağlantılar|Bağlantılar|
|Microsoft.Devices/ıothubs|DeviceTelemetry|Cihaz Telemetrisi|
|Microsoft.Devices/ıothubs|C2DCommands|C2D komutları|
|Microsoft.Devices/ıothubs|DeviceIdentityOperations|Cihaz kimlik işlemleri|
|Microsoft.Devices/ıothubs|FileUploadOperations|Dosya karşıya yükleme işlemleri|
|Microsoft.Devices/ıothubs|Yollar|Yollar|
|Microsoft.Devices/ıothubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/ıothubs|C2DTwinOperations|C2D İkizi işlemleri|
|Microsoft.Devices/ıothubs|TwinQueries|Çifti sorguları|
|Microsoft.Devices/ıothubs|JobsOperations|İş işlemleri|
|Microsoft.Devices/ıothubs|DirectMethods|Doğrudan yöntemler|
|Microsoft.Devices/ıothubs|E2EDiagnostics|E2E tanılama (Önizleme)|
|Microsoft.Devices/ıothubs|Yapılandırmalar|Yapılandırmalar|
|Microsoft.Devices/provisioningServices|DeviceOperations|Cihaz işlemleri|
|Microsoft.Devices/provisioningServices|ServiceOperations|Hizmet işlemleri|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arşiv günlükleri|
|Microsoft.EventHub/namespaces|OperationalLogs|İşlem günlükleri|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Otomatik ölçek günlükleri|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|Otomatik ölçeklendirme değerlendirmeleri|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|Otomatik ölçek ölçeği eylemleri|
|Microsoft.IoTSpaces/Graph|İzleme|İzleme|
|Microsoft.IoTSpaces/Graph|İşletimsel|İşletimsel|
|Microsoft.IoTSpaces/Graph|Denetim|Denetim|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|Giriş|Giriş|
|Microsoft.IoTSpaces/Graph|Çıkış|Çıkış|
|Microsoft.KeyVault/vaults|AuditEvent|Denetim Günlükleri|
|Microsoft.Logic/workflows|İş akışı WorkflowRuntime|İş akışı çalışma zamanı Tanılama Olayları|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Tümleştirme hesabı izleme olayları|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Ağ güvenlik grubu olayı|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Ağ güvenlik grubu kuralı sayacı|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Yük Dengeleyici uyarı olayları|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Yük Dengeleyici araştırması sistem durumu|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS koruması bildirimleri|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|Akış günlüklerini DDoS riskini azaltma kararları|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|DDoS azaltma raporları|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM koruma uyarıları|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Uygulama ağ geçidi erişim günlüğü|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Uygulama ağ geçidi performans günlüğü|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Application Gateway güvenlik duvarı günlüğü|
|Microsoft.Network/securegateways|AzureFirewallApplicationRule|Azure güvenlik duvarı uygulama kuralı|
|Microsoft.Network/securegateways|AzureFirewallNetworkRule|Azure ağ güvenlik duvarı|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Azure güvenlik duvarı uygulama kuralı|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Azure ağ güvenlik duvarı|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Tünel tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Rota tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|P2S tanılama günlükleri|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Traffic Manager araştırma durumu olay sonuçları|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Eşleme yolu tablo günlükleri|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Frontdoor erişim günlüğü|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Frontdoor Web uygulaması güvenlik duvarı günlüğü|
|Microsoft.PowerBIDedicated/capacities|Altyapısı|Altyapısı|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup raporlama verileri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery işleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery etkinlikleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery çoğaltılan öğeler|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery çoğaltma istatistikleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery kurtarma noktaları|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery çoğaltma verileri yükleme hızı|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery korumalı Disk veri değişim sıklığı|
|Microsoft.Search/searchServices|OperationLogs|İşlem günlükleri|
|Microsoft.ServiceBus/namespaces|OperationalLogs|İşlem günlükleri|
|Microsoft.Sql/servers/databases|SQLInsights|SQL bilgileri|
|Microsoft.Sql/servers/databases|AutomaticTuning|Otomatik ayarlama|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Query Store çalışma zamanı istatistikleri|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Query Store bekleme istatistikleri|
|Microsoft.Sql/servers/databases|Hatalar|Hatalar|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Veritabanı bekleme istatistikleri|
|Microsoft.Sql/servers/databases|Zaman Aşımları|Zaman Aşımları|
|Microsoft.Sql/servers/databases|blokları|blokları|
|Microsoft.Sql/servers/databases|Kilitlenmeler|Kilitlenmeler|
|Microsoft.Sql/servers/databases|Denetim|Denetim Günlükleri|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|
|Microsoft.Sql/servers/databases|DmsWorkers|DMS çalışanları|
|Microsoft.Sql/servers/databases|ExecRequests|Exec istekleri|
|Microsoft.Sql/servers/databases|RequestSteps|İstek adımları|
|Microsoft.Sql/servers/databases|SqlRequests|SQL istekleri|
|Microsoft.Sql/servers/databases|Bekler|Bekler|
|Microsoft.Sql/managedInstances|ResourceUsageStats|Kaynak kullanım istatistikleri|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|
|Microsoft.Sql/managedInstances/databases|SQLInsights|SQL bilgileri|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|Query Store çalışma zamanı istatistikleri|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|Query Store bekleme istatistikleri|
|Microsoft.Sql/managedInstances/databases|Hatalar|Hatalar|
|Microsoft.StreamAnalytics/streamingjobs|Yürütme|Yürütme|
|Microsoft.StreamAnalytics/streamingjobs|Yazma|Yazma|
|Microsoft.Web/Sites|FunctionExecutionLogs|İşlev yürütme günlükleri|
|microsoft.web/sites/slots|FunctionExecutionLogs|İşlev yürütme günlükleri|

## <a name="next-steps"></a>Sonraki Adımlar

* [Tanılama günlükleri hakkında daha fazla bilgi edinin](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Kaynak tanılama günlükleri için Stream **olay hub'ları**](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Log Analytics ile Azure depolama biriminden günlüklerini çözümleme](../../azure-monitor/platform/collect-azure-metrics-logs.md)

