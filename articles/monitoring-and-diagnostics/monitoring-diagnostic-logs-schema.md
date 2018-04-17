---
title: Azure tanılama günlükleri desteklenen Hizmetleri ve şemaları | Microsoft Docs
description: Desteklenen hizmetler ve olay şema Azure tanılama günlüklerini anlayın.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2018
ms.author: johnkem
ms.openlocfilehash: 91c3f1507bb4fb64d5395917e8e431951f77e72b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Desteklenen hizmetler, şemalar ve Azure tanılama günlükleri için kategorileri

[Azure kaynak tanılama günlüklerini](monitoring-overview-of-diagnostic-logs.md) olan bu kaynağın işlemi açıklayan Azure kaynaklarınızı tarafından gösterilen günlükleri. Kaynak türü bu günlüklerin belirli. Bu makalede, desteklenen Hizmetleri ve her hizmeti tarafından oluşturulan olaylar için olay şema kümesini alır. Bu makalede ayrıca kaynak türüne göre kullanılabilen günlük kategorilerinin tam bir listesini içerir.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Desteklenen kaynak tanılama günlüklerini Hizmetleri ve şemaları
Kaynağın tanılama günlükleri için şema kaynak ve günlük kategoriye göre değişir.   

| Hizmet | Şema & belgeleri |
| --- | --- |
| Analysis Services | Şema kullanılamaz. |
| API Management | [API Management tanılama günlükleri](../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Application Gatewayler |[Uygulama ağ geçidi için tanılama günlükleri](../application-gateway/application-gateway-diagnostics.md) |
| Azure Otomasyonu |[Azure otomasyonu için günlük analizi](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch Tanılama Günlüğü](../batch/batch-diagnostics.md) |
| Customer Insights | Şema kullanılamaz. |
| Content Delivery Network | Şema kullanılamaz. |
| CosmosDB | [Azure Cosmos DB günlüğe kaydetme](../cosmos-db/logging.md) |
| Data Lake Analytics |[Azure Data Lake Analytics’te tanılama günlüklerine erişim](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Azure Data Lake Store için tanılama günlüklerine erişme](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs tanılama günlükleri](../event-hubs/event-hubs-diagnostic-logs.md) |
| IoT Hub’ı | [IOT hub'ı işlemleri](../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Anahtar Kasası |[Azure Anahtar Kasası Günlüğü](../key-vault/key-vault-logging.md) |
| Load Balancer |[Azure Load Balancer için Log Analytics](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B özel izleme şeması](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Ağ Güvenlik Grupları |[Ağ güvenlik grupları (NSG’ler) için Log Analytics](../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS koruması | Şema kullanılamaz. |
| Kurtarma Hizmetleri | [Azure yedekleme için veri modeli](../backup/backup-azure-reports-data-model.md)|
| Arama |[Etkinleştirme ve arama trafiği Analytics kullanma](../search/search-traffic-analytics.md) |
| Sunucu Yönetimi | Şema kullanılamaz. |
| Service Bus |[Azure Service Bus tanılama günlükleri](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Azure SQL veritabanı Tanılama Günlüğü](../sql-database/sql-database-metrics-diag-logging.md) |
| Akış Analizi |[İş tanılama günlükleri](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Sanal Ağlar | Şema kullanılamaz. |

## <a name="supported-log-categories-per-resource-type"></a>Kaynak türü başına günlük kategoriler desteklenen
|Kaynak Türü|Kategori|Kategori görünen adı|
|---|---|---|
|Microsoft.AnalysisServices/servers|Altyapısı|Altyapısı|
|Microsoft.AnalysisServices/servers|Hizmet|Hizmet|
|Microsoft.ApiManagement/service|GatewayLogs|ApiManagement ağ geçidine ilgili günlükleri|
|Microsoft.Automation/automationAccounts|JobLogs|İş günlükleri|
|Microsoft.Automation/automationAccounts|JobStreams|İş akışları|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC düğüm durumu|
|Microsoft.Batch/batchAccounts|ServiceLog|Hizmet Günlükleri|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Uç noktaya ilişkin ölçümleri (ör. bant genişliği, çıkış) alır.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|Ardışık Düzen etkinlik çalıştırmalarını günlüğü|
|Microsoft.DataFactory/factories|PipelineRuns|Ardışık Düzen günlük çalışır|
|Microsoft.DataFactory/factories|TriggerRuns|Tetikleyici günlük çalıştırır|
|Microsoft.DataLakeAnalytics/accounts|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeAnalytics/accounts|İstekler|İstek günlükleri|
|Microsoft.DataLakeStore/accounts|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeStore/accounts|İstekler|İstek günlükleri|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL sunucu günlükleri|
|Microsoft.Devices/IotHubs|Bağlantılar|Bağlantılar|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Cihaz Telemetrisi|
|Microsoft.Devices/IotHubs|C2DCommands|C2D komutları|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Aygıt Kimliği işlemleri|
|Microsoft.Devices/IotHubs|FileUploadOperations|Dosya karşıya yükleme işlemleri|
|Microsoft.Devices/IotHubs|Yollar|Yollar|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D Twin işlemleri|
|Microsoft.Devices/IotHubs|TwinQueries|Twin sorguları|
|Microsoft.Devices/IotHubs|JobsOperations|İşlerini işlemleri|
|Microsoft.Devices/IotHubs|DirectMethods|Doğrudan yöntemleri|
|Microsoft.Devices/IotHubs|E2EDiagnostics|E2E tanılama (Önizleme)|
|Microsoft.Devices/provisioningServices|DeviceOperations|Aygıt işlemleri|
|Microsoft.Devices/provisioningServices|ServiceOperations|Hizmet işlemleri|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arşiv günlükleri|
|Microsoft.EventHub/namespaces|OperationalLogs|İşlem günlükleri|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Otomatik ölçek günlükleri|
|Microsoft.KeyVault/vaults|AuditEvent|Denetim Günlükleri|
|Microsoft.Logic/workflows|İş akışı WorkflowRuntime|İş akışı çalışma zamanı tanılama olayları|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Tümleştirme Hesabı izleme olayları|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Ağ güvenlik grubu olayı|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Ağ güvenlik grubu kural sayacı|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Yük Dengeleyici uyarı olayları|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Yük Dengeleyici araştırması sistem durumu|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS koruması bildirimleri|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM koruma uyarıları|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Uygulama ağ geçidi erişim günlüğü|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Uygulama ağ geçidi performans günlüğü|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Uygulama ağ geçidi güvenlik duvarı günlüğü|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Tünel tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Rota tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE tanılama günlükleri|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|P2S tanılama günlükleri|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Trafik Yöneticisi araştırma sistem durumu sonuçları olayı|
|Microsoft.Network/expressRouteCircuits|GWMCountersTable|Tablo GWM sayaçları|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Raporlama verilerini azure yedekleme|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery işleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site kurtarma olayları|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery çoğaltılan öğeler|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery çoğaltma istatistikleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery kurtarma noktaları|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery çoğaltma verileri oranı karşıya yükle|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Disk veri Dalgalanmasına Azure Site Recovery korumalı|
|Microsoft.Search/searchServices|OperationLogs|İşletim Günlükleri|
|Microsoft.ServiceBus/namespaces|OperationalLogs|İşlem günlükleri|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı istatistikleri|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Sorgu deposu bekleme istatistikleri|
|Microsoft.Sql/servers/databases|Hatalar|Hatalar|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Veritabanı bekleme istatistikleri|
|Microsoft.Sql/servers/databases|Zaman aşımları|Zaman aşımları|
|Microsoft.Sql/servers/databases|Blokları|Blokları|
|Microsoft.Sql/servers/databases|SQLInsights|SQL Öngörüler|
|Microsoft.Sql/servers/databases|Denetim|Denetim Günlükleri|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|
|Microsoft.StreamAnalytics/streamingjobs|Yürütme|Yürütme|
|Microsoft.StreamAnalytics/streamingjobs|Yazma|Yazma|

## <a name="next-steps"></a>Sonraki Adımlar

* [Tanılama günlükleri hakkında daha fazla bilgi edinin](monitoring-overview-of-diagnostic-logs.md)
* [Akış kaynak tanılama günlüklerine **olay hub'ları**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md)
