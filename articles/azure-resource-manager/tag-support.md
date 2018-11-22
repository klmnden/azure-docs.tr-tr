---
title: Kaynaklar için Azure Resource Manager etiketi desteği
description: Etiketlerin hangi Azure kaynak türlerini destekleyen gösterir. Ayrıntılar için tüm Azure hizmetleri sağlar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: a4bb423dc5eddde0fd2d2b9b4f263ab39dbd801f
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284991"
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
| services | Hayır | 
| addsservices | Hayır | 
| yapılandırma | Hayır | 
| aracılar | Hayır | 
| aadsupportcases | Hayır | 
| Raporları | Hayır | 
| servicehealthmetrics | Hayır | 
| günlükler | Hayır | 
| anonymousapiusers | Hayır | 

## <a name="analysis-services"></a>Analysis Services
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 

## <a name="api-hubs"></a>API hub'ları
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| apiManagementAccounts | Hayır | 
| apiManagementAccounts/connectionProviders | Hayır | 
| apiManagementAccounts/bağlantıları | Hayır | 
| apiManagementAccounts/connectionAcls | Hayır | 
| apiManagementAccounts/connectionProviderAcls | Hayır | 
| apiManagementAccounts/API'leri | Hayır | 

## <a name="api-management"></a>API Management
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| hizmet | Evet | 

## <a name="automation"></a>Otomasyon
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| AutomationAccounts | Evet | 
| automationAccounts/runbook'ları | Evet | 
| automationAccounts/yapılandırmalar | Evet | 
| automationAccounts/Web kancaları | Hayır | 
| automationAccounts/softwareUpdateConfigurations | Hayır | 
| automationAccounts/işleri | Hayır | 

## <a name="batch"></a>Batch
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| batchAccounts | Evet | 

## <a name="batch-ai"></a>Batch AI
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Kümeleri | Evet | 
| işler | Evet | 
| fileservers | Evet | 
| çalışma alanı | Evet | 
| çalışma alanları/kümeleri | Hayır | 
| çalışma alanları/fileservers | Hayır | 
| çalışma alanları/denemeleri | Hayır | 
| çalışma alanları/denemeleri/işleri | Hayır | 

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
| Profilleri | Evet | 
| profilleri/uç noktaları | Evet | 
| uç noktalar/profilleri/kaynakları | Hayır | 
| uç noktalar/profilleri/customdomains | Hayır | 
| validateProbe | Hayır | 
| edgenodes | Hayır | 

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
| virtualNetworks/virtualNetworkPeerings | Hayır | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Hayır | 

## <a name="classic-storage"></a>Klasik depolama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| storageAccounts/services | Hayır | 
| storageAccounts/services/diagnosticSettings | Hayır | 

## <a name="compute"></a>İşlem
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| availabilitySets | Evet | 
| virtualMachines | Evet | 
| virtualMachines ve uzantıları | Evet | 
| virtualMachineScaleSets | Evet | 
| virtualMachineScaleSets ve uzantıları | Hayır | 
| virtualMachineScaleSets/virtualMachines | Hayır | 
| virtualMachineScaleSets/networkınterface'lerden bazıları | Hayır | 
| virtualMachineScaleSets/virtualMachines/networkınterface'lerden bazıları | Hayır | 
| virtualMachineScaleSets/publicIPAddresses | Hayır | 
| restorePointCollections | Evet | 
| restorePointCollections/restorePoints | Hayır | 
| virtualMachines/diagnosticSettings | Hayır | 
| virtualMachines/metricDefinitions | Hayır | 
| sharedVMImages | Evet | 
| sharedVMImages/sürümleri | Evet | 
| diskler | Evet | 
| anlık görüntüler | Evet | 
| images | Evet | 

## <a name="container"></a>Kapsayıcı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| containerGroups | Evet | 

## <a name="container-instance"></a>Kapsayıcı Örneği
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| containerGroups | Evet | 
| serviceAssociationLinks | Hayır | 

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
| fabrikaları | Evet | 
| fabrikaları/integrationRuntimes | Hayır | 
| dataFactories/diagnosticSettings | Hayır | 
| dataFactories/metricDefinitions | Hayır | 
| dataFactorySchema | Hayır | 

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
| Zamanlamaları | Evet | 
| Labs/virtualMachines | Evet | 
| Labs/serviceRunners | Evet | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| lcsprojects | Hayır | 
| lcsprojects/bağlayıcılar | Hayır | 
| lcsprojects/clouddeployments | Hayır | 

## <a name="event-grid"></a>Event Grid
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| eventSubscriptions | Hayır | 
| konuları | Evet | 
| etki alanları | Evet | 
| etki alanı/konuları | Hayır | 
| topicTypes | Hayır | 
| extensionTopics | Hayır | 

## <a name="event-hub"></a>Olay Hub'ı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Ad alanları | Evet | 
| Kümeleri | Evet | 

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
| Bileşenleri | Evet | 
| bileşenleri/sorgu | Hayır | 
| bileşenleri/ölçümleri | Hayır | 
| bileşenleri/olaylar | Hayır | 
| Web testleri | Evet | 
| sorgu | Hayır | 
| scheduledqueryrules | Evet | 
| bileşenleri/pricingPlans | Hayır | 
| migrateToNewPricingModel | Hayır | 
| rollbackToLegacyPricingModel | Hayır | 
| automatedExportSettings | Hayır | 
| Çalışma kitapları | Evet | 
| myWorkbooks | Hayır | 
| günlükler | Hayır | 

## <a name="key-vault"></a>Key Vault
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| kasaları | Evet | 
| Kasalar/parolalar | Hayır | 
| kasaları/accessPolicies | Hayır | 
| deletedVaults | Hayır | 

## <a name="log-analytics"></a>Log Analytics
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| günlükler | Hayır | 

## <a name="logic"></a>Mantık
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| İş akışları | Evet | 
| integrationAccounts | Evet | 

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

## <a name="mariadb"></a>MariaDB
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 

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
| mediaservices/varlıklar | Hayır | 
| mediaservices/contentKeyPolicies | Hayır | 
| mediaservices/streamingLocators | Hayır | 
| mediaservices/streamingPolicies | Hayır | 
| mediaservices/eventGridFilters | Hayır | 
| mediaservices/dönüştürme | Hayır | 
| dönüşümler/mediaservices/işleri | Hayır | 
| mediaservices/akış | Evet | 
| mediaservices/liveEvents | Evet | 
| liveEvents/mediaservices/liveOutputs | Hayır | 
| mediaservices/streamingEndpointOperations | Hayır | 
| mediaservices/liveEventOperations | Hayır | 
| mediaservices/liveOutputOperations | Hayır | 
| varlıklar/mediaservices/assetFilters | Hayır | 
| mediaservices/accountFilters | Hayır | 

## <a name="mysql"></a>MySQL
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 

## <a name="network"></a>Ağ
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| virtualNetworks | Evet | 
| publicIPAddresses | Evet | 
| networkınterface'lerden bazıları | Evet | 
| interfaceEndpoints | Evet | 
| Sonraki | Evet | 
| networkSecurityGroups | Evet | 
| Applicationsecuritygroup | Evet | 
| serviceEndpointPolicies | Evet | 
| networkIntentPolicies | Evet | 
| routeTables | Evet | 
| publicIPPrefixes | Evet | 
| networkWatchers | Evet | 
| networkWatchers/connectionMonitors | Evet | 
| networkWatchers/merceklerden | Evet | 
| networkWatchers/pingMeshes | Evet | 
| virtualNetworkGateways | Evet | 
| localNetworkGateways | Evet | 
| bağlantılar | Evet | 
| applicationGateways | Evet | 
| expressRouteCircuits | Evet | 
| routeFilters | Evet | 
| virtualWans | Evet | 
| vpnSites | Evet | 
| virtualHubs | Evet | 
| vpnGateways | Evet | 
| azureFirewalls | Evet | 
| virtualNetworkTaps | Evet | 
| privateLinkServices | Evet | 
| ddosProtectionPlans | Evet | 
| networkProfiles | Evet | 
| frontdoors | Evet | 
| frontdoorWebApplicationFirewallPolicies | Evet | 
| webApplicationFirewallPolicies | Evet | 

## <a name="notification-hubs"></a>Notification Hubs
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Ad alanları | Evet | 
| ad/notificationHubs | Evet | 

## <a name="portal"></a>Portal
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Panolar | Evet | 

## <a name="portal-sdk"></a>Portal SDK'sı
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| rootResources | Evet | 

## <a name="postgresql"></a>PostgreSQL
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 
| sunucuları/topQueryStatistics | Hayır | 
| sunucuları/queryTexts | Hayır | 
| sunucuları/waitStatistics | Hayır | 
| sunucuları/danışmanları | Hayır | 

## <a name="power-bi"></a>Power BI
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| workspaceCollections | Evet | 

## <a name="recovery-services"></a>Kurtarma Hizmetleri
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| kasaları | Evet | 
| backupProtectedItems | Hayır | 

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
| searchServices | Evet | 
| resourceHealthMetadata | Hayır | 

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

## <a name="sql-virtual-machine"></a>SQL sanal makinesi
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| DWVM | Evet | 

## <a name="storage"></a>Depolama
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| storageAccounts | Evet | 
| storageAccounts/blobServices | Hayır | 
| storageAccounts/tableServices | Hayır | 
| storageAccounts/queueServices | Hayır | 
| storageAccounts/fileServices | Hayır | 
| storageAccounts/services | Hayır | 
| storageAccounts/services/metricDefinitions | Hayır | 

## <a name="storage-sync"></a>Depolama eşitleme
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| storageSyncServices | Evet | 
| storageSyncServices/syncGroups | Hayır | 
| syncGroups/storageSyncServices/cloudEndpoints | Hayır | 
| syncGroups/storageSyncServices/serverEndpoints | Hayır | 
| storageSyncServices/registeredServers | Hayır | 
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
| hesabı/proje | Evet | 
| hesabı/uzantısı | Evet | 
| account | Evet | 
| hesabı/proje | Evet | 
| hesabı/uzantısı | Evet | 

## <a name="web"></a>Web
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| Site/örnekleri | Hayır | 
| yuvaları/site/örnekleri | Hayır | 
| Örnek/Site/uzantıları | Hayır | 
| Siteler ve yuvaları/örnekleri/uzantıları | Hayır | 
| publishingUsers | Hayır | 
| Doğrulama | Hayır | 
| sourceControls | Hayır | 
| Site/hostNameBindings | Hayır | 
| Site/domainOwnershipIdentifiers | Hayır | 
| yuvaları/site/hostNameBindings | Hayır | 
| sertifika | Evet | 
| serverFarms | Evet | 
| serverFarms/çalışanları | Hayır | 
| siteler | Evet | 
| Site/Yuvalar | Evet | 
| Site/ölçümleri | Hayır | 
| yuvaları/site/ölçümleri | Hayır | 
| Site/premieraddons | Evet | 
| hostingEnvironments | Evet | 
| hostingEnvironments/multiRolePools | Hayır | 
| hostingEnvironments/workerPools | Hayır | 
| hostingEnvironments/ölçümleri | Hayır | 
| işlevler | Hayır | 
| deletedSites | Hayır | 
| apiManagementAccounts | Hayır | 
| apiManagementAccounts/bağlantıları | Hayır | 
| apiManagementAccounts/connectionAcls | Hayır | 
| apiManagementAccounts/API/bağlantı/connectionAcls | Hayır | 
| API/apiManagementAccounts/connectionAcls | Hayır | 
| apiManagementAccounts/apiAcls | Hayır | 
| API/apiManagementAccounts/apiAcls | Hayır | 
| apiManagementAccounts/API'leri | Hayır | 
| API/apiManagementAccounts/localizedDefinitions | Hayır | 
| API/apiManagementAccounts/bağlantıları | Hayır | 
| bağlantılar | Evet | 
| customApis | Evet | 
| connectionGateways | Evet | 
| billingMeters | Hayır | 
| verifyHostingEnvironmentVnet | Hayır | 

## <a name="xrm"></a>XRM
| Kaynak türü | Etiketleri destekler |
| ------------- | ----------- |
| organizations | Hayır | 

## <a name="next-steps"></a>Sonraki adımlar
Etiketler kaynaklara öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md).
