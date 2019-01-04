---
title: Kaynaklar için Azure Resource Manager etiketi desteği
description: Etiketlerin hangi Azure kaynak türlerini destekleyen gösterir. Ayrıntılar için tüm Azure hizmetleri sağlar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/02/2019
ms.author: tomfitz
ms.openlocfilehash: 50ea7a2446b5560bd208b2da128fa877068ce452
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000300"
---
# <a name="tag-support-for-azure-resources"></a>Azure kaynakları için etiketi desteği
Bu makalede, bir kaynak türünü destekleyip desteklemediğini açıklar [etiketleme](resource-group-using-tags.md).

## <a name="aad-domain-services"></a>AAD etki alanı Hizmetleri
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| etki alanları | Hayır | 

## <a name="ad-hybrid-health-service"></a>AD karma sistem durumu hizmeti
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| addsservices | Hayır |
| aadsupportcases | Hayır | 
| aracılar | Hayır | 
| anonymousapiusers | Hayır | 
| yapılandırma | Hayır | 
| günlükler | Hayır | 
| raporlar | Hayır | 
| services | Hayır | 
| servicehealthmetrics | Hayır | 

## <a name="aks"></a>AKS
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| managedClusters | Evet | 

## <a name="analysis-services"></a>Analysis Services
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 

## <a name="api-hubs"></a>API hub'ları
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| apiManagementAccounts | Hayır | 
| apiManagementAccounts/API'leri | Hayır | 
| apiManagementAccounts/connectionAcls | Hayır | 
| apiManagementAccounts/connectionProviders | Hayır | 
| apiManagementAccounts/connectionProviderAcls | Hayır | 
| apiManagementAccounts/bağlantıları | Hayır | 

## <a name="api-management"></a>API Management
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| hizmet | Evet | 

## <a name="automation"></a>Otomasyon
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| AutomationAccounts | Evet | 
| automationAccounts/yapılandırmalar | Evet | 
| automationAccounts/işleri | Hayır | 
| automationAccounts/runbook'ları | Evet | 
| automationAccounts/softwareUpdateConfigurations | Hayır | 
| automationAccounts/Web kancaları | Hayır | 

## <a name="azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/yapılandırmalar | Hayır |
| sunucuları/veritabanları | Hayır |
| sunucuları/firewallRules | Hayır |
| sunucuları/recoverableServers | Hayır | 
| sunucuları/securityAlertPolicies | Hayır |
| sunucuları/virtualNetworkRules | Hayır | 

## <a name="azure-database-for-mysql"></a>MySQL için Azure Veritabanı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/yapılandırmalar | Hayır |
| sunucuları/veritabanları | Hayır |
| sunucuları/firewallRules | Hayır |
| sunucuları/recoverableServers | Hayır | 
| sunucuları/securityAlertPolicies | Hayır |
| sunucuları/virtualNetworkRules | Hayır | 

## <a name="azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/danışmanları | Hayır | 
| sunucuları/yapılandırmalar | Hayır |
| sunucuları/veritabanları | Hayır |
| sunucuları/firewallRules | Hayır |
| sunucuları/queryTexts | Hayır | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/securityAlertPolicies | Hayır |
| sunucuları/topQueryStatistics | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 
| sunucuları/waitStatistics | Hayır | 

## <a name="batch"></a>Batch
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| batchAccounts | Evet | 

## <a name="bing-maps"></a>Bing Haritalar
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| mapApis | Evet | 

## <a name="biztalk-services"></a>BizTalk Services
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| BizTalk | Evet | 

## <a name="cache"></a>Önbellek
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Redis | Evet | 

## <a name="cdn"></a>CDN
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| edgenodes | Hayır | 
| Profilleri | Evet | 
| profilleri/uç noktaları | Evet | 
| uç noktalar/profilleri/customdomains | Hayır | 
| uç noktalar/profilleri/kaynakları | Hayır | 
| validateProbe | Hayır | 

## <a name="classic-compute"></a>Klasik İşlem
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| domainNames | Hayır | 
| domainNames/Yuvalar | Hayır | 
| domainNames/yuvaları/roller | Hayır | 
| virtualMachines | Hayır | 
| virtualMachines/diagnosticSettings | Hayır | 
| virtualMachines/metricDefinitions | Hayır | 
| virtualMachines/ölçümleri | Hayır | 

## <a name="classic-infrastructure-migrate"></a>Klasik altyapı geçirme
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| classicInfrastructureResources | Hayır | 

## <a name="classic-network"></a>Klasik ağ
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| virtualNetworks | Hayır | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Hayır | 
| virtualNetworks/virtualNetworkPeerings | Hayır | 

## <a name="classic-storage"></a>Klasik depolama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| storageAccounts/services | Hayır | 
| storageAccounts/services/diagnosticSettings | Hayır | 

## <a name="compute"></a>İşlem
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| availabilitySets | Evet | 
| diskler | Evet | 
| images | Evet | 
| restorePointCollections | Evet | 
| restorePointCollections/restorePoints | Hayır | 
| sharedVMImages | Evet | 
| sharedVMImages/sürümleri | Evet | 
| anlık görüntüler | Evet | 
| virtualMachines | Evet | 
| virtualMachines/diagnosticSettings | Hayır | 
| virtualMachines ve uzantıları | Evet | 
| virtualMachines/metricDefinitions | Hayır | 
| virtualMachineScaleSets | Evet | 
| virtualMachineScaleSets ve uzantıları | Hayır | 
| virtualMachineScaleSets/networkınterface'lerden bazıları | Hayır | 
| virtualMachineScaleSets/publicIPAddresses | Hayır | 
| virtualMachineScaleSets/virtualMachines | Hayır | 
| virtualMachineScaleSets/virtualMachines/networkınterface'lerden bazıları | Hayır | 

## <a name="container"></a>Kapsayıcı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| containerGroups | Evet | 

## <a name="container-instance"></a>Kapsayıcı Örneği
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| containerGroups | Evet | 
| serviceAssociationLinks | Hayır | 

## <a name="container-registry"></a>Container Kayıt Defteri
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| kayıt defterleri | Evet | 
| kayıt defterleri/çoğaltmalar | Evet |
| kayıt defterleri/görevleri | Evet |
| kayıt defterleri/Web kancaları | Evet |

## <a name="container-service"></a>Kapsayıcı Hizmeti
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| containerServices | Evet | 

## <a name="cortana-analytics"></a>Cortana Analytics
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="cosmos-db"></a>Cosmos DB
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| databaseAccounts | Evet | 
| databaseAccountNames | Hayır | 

## <a name="cost-management"></a>Maliyet Yönetimi
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Bağlayıcılar | Evet | 

## <a name="data-box"></a>Data Box
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| işler | Evet | 

## <a name="data-box-edge"></a>Data Box Edge
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| DataBoxEdgeDevices | Evet | 

## <a name="data-catalog"></a>Veri Kataloğu
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| katalogları | Evet | 

## <a name="data-connect"></a>Veri bağlama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| connectionManagers | Evet | 

## <a name="data-factory"></a>Data Factory
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| dataFactories | Evet | 
| dataFactories/diagnosticSettings | Hayır | 
| dataFactories/metricDefinitions | Hayır | 
| dataFactorySchema | Hayır | 
| fabrikaları | Evet | 
| fabrikaları/integrationRuntimes | Hayır | 

## <a name="devices"></a>Cihazlar
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| IotHubs | Evet | 
| IotHubs/eventGridFilters | Hayır | 
| ProvisioningServices | Evet | 

## <a name="devspaces"></a>Devspaces
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Denetleyicileri | Evet | 

## <a name="devtest-lab"></a>Devtest Lab
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Laboratuvarları | Evet | 
| Labs/artifactsources | Evet |
| Labs/maliyetleri | Evet |
| Labs/customimages | Evet |
| Labs/formülleri | Evet |
| Labs/notificationchannels | Evet |
| Labs/policysets/ilkeler | Evet |
| Labs/zamanlamaları | Evet |
| Labs/serviceRunners | Evet | 
| Labs/kullanıcılar | Evet |
| Labs/kullanıcı/diskleri | Evet |
| Labs/kullanıcı/ortamları | Evet |
| Labs/kullanıcı/gizli | Evet |
| Labs/kullanıcı/servicefabrics | Evet |
| Labs/kullanıcı/servicefabrics/zamanlamaları | Evet |
| Labs/virtualMachines | Evet | 
| Labs/virtualmachines/zamanlamaları | Evet |
| Labs/virtualnetworks | Evet |
| Zamanlamaları | Evet | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| lcsprojects | Hayır | 
| lcsprojects/bağlayıcılar | Hayır | 
| lcsprojects/clouddeployments | Hayır | 

## <a name="event-grid"></a>Event Grid
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| etki alanları | Evet | 
| etki alanı/konuları | Hayır | 
| eventSubscriptions | Hayır | 
| extensionTopics | Hayır | 
| konuları | Evet | 
| topicTypes | Hayır | 

## <a name="event-hub"></a>Olay Hub'ı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Kümeleri | Evet | 
| Ad alanları | Evet | 
| ad/authorizationrules öğesine | Hayır |
| ad/disasterRecoveryConfigs | Hayır |
| ad/eventhubs | Hayır |
| ad/eventhubs/authorizationrules öğesine | Hayır |
| ad/eventhubs/consumergroups | Hayır |

## <a name="hana-on-azure"></a>Azure'da Hana
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| hanaInstances | Evet | 

## <a name="hdinsight"></a>HDInsight
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Kümeleri | Evet | 
| kümeleri/uygulamaları | Hayır | 

## <a name="import-export"></a>İçeri ve Dışarı Aktarma
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| işler | Evet | 

## <a name="insights"></a>Insights
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| actionGroups | Evet |
| activityLogAlerts | Evet |
| alertrules | Evet |
| automatedExportSettings | Hayır | 
| Bileşenleri | Evet | 
| bileşenleri/olaylar | Hayır | 
| bileşenleri/ölçümleri | Hayır | 
| bileşenleri/pricingPlans | Hayır | 
| bileşenleri/sorgu | Hayır | 
| günlükler | Hayır | 
| metricAlerts | Evet |
| migrateToNewPricingModel | Hayır | 
| myWorkbooks | Hayır | 
| sorgu | Hayır | 
| rollbackToLegacyPricingModel | Hayır | 
| scheduledqueryrules | Evet | 
| Web testleri | Evet | 
| Çalışma kitapları | Evet | 

## <a name="key-vault"></a>Key Vault
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| deletedVaults | Hayır | 
| kasaları | Evet | 
| kasaları/accessPolicies | Hayır | 
| Kasalar/parolalar | Hayır | 

## <a name="log-analytics"></a>Log Analytics
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| günlükler | Hayır | 

## <a name="logic"></a>Mantık
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| integrationAccounts | Evet | 
| İş akışları | Evet | 

## <a name="machine-learning-services"></a>Machine Learning Services
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| çalışma alanı | Evet | 
| çalışma alanları/işlemleri | Hayır | 

## <a name="managed-identity"></a>Yönetilen Kimlik
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Kimlikler | Hayır | 
| Userassignedıdentities | Evet | 

## <a name="marketplace-apps"></a>Marketplace uygulamaları
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| classicDevServices | Evet | 

## <a name="marketplace-ordering"></a>Market sıralama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Anlaşmaları | Hayır | 
| offertypes | Hayır | 

## <a name="media"></a>Medya
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| mediaservices | Evet | 
| mediaservices/accountFilters | Hayır | 
| mediaservices/varlıklar | Hayır | 
| varlıklar/mediaservices/assetFilters | Hayır | 
| mediaservices/contentKeyPolicies | Hayır | 
| mediaservices/eventGridFilters | Hayır | 
| mediaservices/liveEventOperations | Hayır | 
| mediaservices/liveEvents | Evet | 
| liveEvents/mediaservices/liveOutputs | Hayır | 
| mediaservices/liveOutputOperations | Hayır | 
| mediaservices/akış | Evet | 
| mediaservices/streamingEndpointOperations | Hayır | 
| mediaservices/streamingLocators | Hayır | 
| mediaservices/streamingPolicies | Hayır | 
| mediaservices/dönüştürme | Hayır | 
| dönüşümler/mediaservices/işleri | Hayır | 

## <a name="network"></a>Ağ
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| applicationGateways | Evet | 
| Applicationsecuritygroup | Evet | 
| azureFirewalls | Evet | 
| bağlantılar | Evet | 
| ddosProtectionPlans | Evet | 
| expressRouteCircuits | Evet | 
| frontdoors | Evet | 
| frontdoorWebApplicationFirewallPolicies | Evet | 
| interfaceEndpoints | Evet | 
| Sonraki | Evet | 
| localNetworkGateways | Evet | 
| networkIntentPolicies | Evet | 
| networkınterface'lerden bazıları | Evet | 
| networkProfiles | Evet | 
| networkSecurityGroups | Evet | 
| networkWatchers | Evet | 
| networkWatchers/connectionMonitors | Evet | 
| networkWatchers/merceklerden | Evet | 
| networkWatchers/pingMeshes | Evet | 
| privateLinkServices | Evet | 
| publicIPAddresses | Evet | 
| publicIPPrefixes | Evet | 
| routeFilters | Evet | 
| routeTables | Evet | 
| serviceEndpointPolicies | Evet | 
| virtualHubs | Evet | 
| virtualNetworks | Evet | 
| virtualNetworkGateways | Evet | 
| virtualNetworkTaps | Evet | 
| virtualWans | Evet | 
| vpnGateways | Evet | 
| vpnSites | Evet | 
| webApplicationFirewallPolicies | Evet | 

## <a name="notification-hubs"></a>Notification Hubs
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Ad alanları | Evet | 
| ad/notificationHubs | Evet | 

## <a name="operational-insights"></a>Operasyonel İçgörüler
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| çalışma alanı | Evet |
| çalışma alanları/veri kaynakları | Evet |
| çalışma alanları/linkedServices | Evet |
| çalışma alanları/savedSearches | Hayır |
| çalışma alanları/storageInsightConfigs | Evet |

## <a name="operations-management"></a>Operations Management
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| çözümler | Hayır |

## <a name="portal"></a>Portal
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Panolar | Evet | 

## <a name="portal-sdk"></a>Portal SDK'sı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| rootResources | Evet | 

## <a name="power-bi"></a>Power BI
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| workspaceCollections | Evet | 

## <a name="recovery-services"></a>Kurtarma Hizmetleri
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| backupProtectedItems | Hayır | 
| kasaları | Evet | 

## <a name="relay"></a>Geçiş
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Ad alanları | Evet | 

## <a name="resources"></a>Kaynaklar
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| resourceGroups | Evet | 
| Abonelikler/kaynak grupları | Evet | 

## <a name="scheduler"></a>Scheduler
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| eyleminde | Evet | 
| akışlar | Evet | 

## <a name="search"></a>Arama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| resourceHealthMetadata | Hayır | 
| searchServices | Evet | 

## <a name="security"></a>Güvenlik
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| dataCollectionAgents | Hayır | 

## <a name="service-bus"></a>Service Bus
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Ad alanları | Evet | 
| ad/eventgridfilters | Hayır | 

## <a name="service-fabric"></a>Service Fabric
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Kümeleri | Evet | 
| kümeleri/uygulamaları | Hayır | 

## <a name="service-fabric-mesh"></a>Service Fabric Mesh
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| uygulamalar | Evet | 
| ağlar | Evet | 
| volumes | Evet | 

## <a name="signalr-service"></a>SignalR Service
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| SignalR | Evet | 

## <a name="site-recovery"></a>Site Recovery
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| SiteRecoveryVault | Evet | 

## <a name="solutions"></a>Çözümler
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| uygulamalar | Evet | 
| applicationDefinitions | Evet | 
| jitRequests | Evet | 

## <a name="sql"></a>SQL
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Konum/instanceFailoverGroups | Hayır |
| managedInstances | Evet |
| managedInstances/veritabanları | Evet |
| veritabanları/managedInstances/backupShortTermRetentionPolicies | Hayır |
| veritabanları/managedInstances/şemaları/sütunlar/tablolar/sensitivityLabels | Hayır |
| veritabanları/managedInstances/vulnerabilityAssessments | Hayır |
| managedInstances/veritabanları/vulnerabilityAssessments/kuralları/temelleri | Hayır |
| managedInstances/encryptionProtector | Hayır |
| managedInstances/anahtarları | Hayır |
| restorableDroppedDatabases/managedInstances/backupShortTermRetentionPolicies | Hayır |
| managedInstances/vulnerabilityAssessments | Hayır |
| sunucu | Evet |
| sunucuları/yöneticileri | Hayır |
| sunucuları/danışmanları | Hayır |
| sunucuları/auditingSettings | Hayır |
| sunucuları/backupLongTermRetentionVaults | Hayır |
| sunucuları/communicationLinks | Hayır |
| sunucuları/connectionPolicies | Hayır |
| sunucuları/veritabanları | Evet |
| veritabanları/sunucuları/danışmanları | Hayır |
| veritabanları/sunucuları/auditingSettings | Hayır |
| veritabanları/sunucuları/backupLongTermRetentionPolicies | Hayır |
| veritabanları/sunucuları/backupShortTermRetentionPolicies | Hayır |
| veritabanları/sunucuları/connectionPolicies | Hayır |
| veritabanları/sunucuları/dataMaskingPolicies | Hayır |
| sunucuları veya veritabanları/dataMaskingPolicies/kurallara | Hayır |
| veritabanları/sunucuları/extendedAuditingSettings | Hayır |
| sunucuları ve veritabanları/uzantıları | Hayır |
| veritabanları/sunucuları/geoBackupPolicies | Hayır |
| veritabanları/sunucuları/şemaları/sütunlar/tablolar/sensitivityLabels | Hayır |
| veritabanları/sunucuları/securityAlertPolicies | Hayır |
| veritabanları/sunucuları/syncGroups | Hayır |
| sunucuları/veritabanları/syncGroups/syncMembers | Hayır |
| veritabanları/sunucuları/transparentDataEncryption | Hayır |
| veritabanları/sunucuları/vulnerabilityAssessments | Hayır |
| sunucuları/veritabanları/vulnerabilityAssessments/kuralları/temelleri | Hayır |
| sunucuları/disasterRecoveryConfiguration | Hayır |
| sunucuları/dnsAliases | Hayır |
| sunucuları/elasticPools | Evet |
| sunucuları/encryptionProtector | Hayır |
| sunucuları/extendedAuditingSettings | Hayır |
| sunucuları/failoverGroups | Evet |
| sunucuları/firewallRules | Hayır |
| sunucuları/jobAgents | Evet |
| sunucuları/jobAgents/kimlik bilgileri | Hayır |
| sunucuları/jobAgents/işleri | Hayır |
| sunucuları/jobAgents/iş/yürütme | Hayır |
| sunucuları/jobAgents/iş/adımları | Hayır |
| Hedef sunucuları/jobAgents/grupları | Hayır |
| sunucuları/anahtarları | Hayır |
| sunucuları/securityAlertPolicies | Hayır |
| sunucuları/syncAgents | Hayır |
| sunucuları/virtualNetworkRules | Hayır |
| sunucuları/vulnerabilityAssessments | Hayır |

## <a name="sql-virtual-machine"></a>SQL sanal makinesi
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| DWVM | Evet | 

## <a name="storage"></a>Depolama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| storageAccounts | Evet | 
| storageAccounts/blobServices | Hayır | 
| storageAccounts/fileServices | Hayır | 
| storageAccounts/queueServices | Hayır | 
| storageAccounts/services | Hayır | 
| storageAccounts/services/metricDefinitions | Hayır | 
| storageAccounts/tableServices | Hayır | 

## <a name="storage-sync"></a>Depolama eşitleme
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| storageSyncServices | Evet | 
| storageSyncServices/registeredServers | Hayır | 
| storageSyncServices/syncGroups | Hayır | 
| syncGroups/storageSyncServices/cloudEndpoints | Hayır | 
| syncGroups/storageSyncServices/serverEndpoints | Hayır | 
| storageSyncServices/iş akışları | Hayır | 

## <a name="storsimple"></a>Storsimple
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Yöneticileri | Evet | 

## <a name="stream-analytics"></a>Stream Analytics
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| streamingjobs | Evet | 
| streamingjobs/diagnosticSettings | Hayır | 
| streamingjobs/metricDefinitions | Hayır | 

## <a name="subscription"></a>Abonelik
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| SubscriptionDefinitions | Hayır | 
| SubscriptionOperations | Hayır | 

## <a name="support"></a>Destek
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| supporttickets | Hayır | 

## <a name="visual-studio"></a>Visual Studio
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| account | Evet | 
| hesabı/uzantısı | Evet | 
| hesabı/proje | Evet | 

## <a name="web"></a>Web
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| apiManagementAccounts | Hayır | 
| apiManagementAccounts/apiAcls | Hayır | 
| apiManagementAccounts/API'leri | Hayır | 
| API/apiManagementAccounts/apiAcls | Hayır | 
| API/apiManagementAccounts/connectionAcls | Hayır | 
| API/apiManagementAccounts/bağlantıları | Hayır | 
| apiManagementAccounts/API/bağlantı/connectionAcls | Hayır | 
| API/apiManagementAccounts/localizedDefinitions | Hayır | 
| apiManagementAccounts/connectionAcls | Hayır | 
| apiManagementAccounts/bağlantıları | Hayır | 
| billingMeters | Hayır | 
| sertifika | Evet | 
| connectionGateways | Evet | 
| bağlantılar | Evet | 
| customApis | Evet | 
| deletedSites | Hayır | 
| işlevler | Hayır | 
| hostingEnvironments | Evet | 
| hostingEnvironments/ölçümleri | Hayır | 
| hostingEnvironments/multiRolePools | Hayır | 
| hostingEnvironments/workerPools | Hayır | 
| publishingUsers | Hayır | 
| serverFarms | Evet | 
| serverFarms/çalışanları | Hayır | 
| siteler | Evet | 
| Site/domainOwnershipIdentifiers | Hayır | 
| Site/hostNameBindings | Hayır | 
| Site/örnekleri | Hayır | 
| Örnek/Site/uzantıları | Hayır | 
| Site/ölçümleri | Hayır | 
| Site/premieraddons | Evet | 
| Site/Yuvalar | Evet | 
| yuvaları/site/hostNameBindings | Hayır | 
| yuvaları/site/örnekleri | Hayır | 
| Siteler ve yuvaları/örnekleri/uzantıları | Hayır | 
| yuvaları/site/ölçümleri | Hayır | 
| sourceControls | Hayır | 
| Doğrulama | Hayır | 
| verifyHostingEnvironmentVnet | Hayır | 

## <a name="xrm"></a>XRM
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| organizations | Hayır | 

## <a name="next-steps"></a>Sonraki adımlar
Etiketler kaynaklara öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md).
