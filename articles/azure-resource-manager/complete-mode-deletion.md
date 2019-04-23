---
title: Kaynak türüne göre Azure Resource Manager tam modda silme
description: Kaynak türleri tam modda silme işlemini Azure Resource Manager şablonlarında nasıl işleneceğini gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 02/13/2019
ms.author: tomfitz
ms.openlocfilehash: fded37fee844a01f4d51518f2ca56dcf575704b2
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60150158"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Tam modda dağıtımlar için Azure kaynakları silme
Bu makalede, kaynak türleri silmeyi tam modunda dağıtılmış olan olmayan bir şablon olduğunda nasıl işleneceğini açıklar.

Kaynak türleri ile işaretlenen `Yes` türü ile tam modda şablonda dağıtılan değil, silinir. 

Kaynak türleri ile işaretlenen `No` otomatik olarak silinmez üst kaynak silinirse değil, şablon olduğunda; ancak, silinen. Davranış tam bir açıklaması için bkz. [Azure Resource Manager dağıtım modları](deployment-modes.md).

Virgülle ayrılmış değerler dosyası aynı verileri almak için indirme [tamamlamak-modu-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| DomainServices | Evet | 
| DomainServices/oucontainer | Hayır | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| diagnosticSettings | Hayır | 
| diagnosticSettingsCategories | Hayır | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| supportProviders | Hayır | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| aadsupportcases | Hayır | 
| addsservices | Hayır | 
| aracılar | Hayır | 
| anonymousapiusers | Hayır | 
| yapılandırma | Hayır | 
| günlükler | Hayır | 
| raporlar | Hayır | 
| services | Hayır | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Yapılandırmaları | Hayır | 
| generateRecommendations | Hayır | 
| öneriler | Hayır | 
| gizlemeleri | Hayır | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| actionRules | Hayır | 
| alerts | Hayır | 
| alertsList | Hayır | 
| alertsSummary | Hayır | 
| alertsSummaryList | Hayır | 
| smartDetectorAlertRules | Hayır | 
| smartDetectorRuntimeEnvironments | Hayır | 
| smartGroups | Hayır | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| sunucu | Evet | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| reportFeedback | Hayır | 
| hizmet | Evet | 
| validateServiceName | Hayır | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| attestationProviders | Hayır | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| classicAdministrators | Hayır | 
| denyAssignments | Hayır | 
| elevateAccess | Hayır | 
| Kilitler | Hayır | 
| izinler | Hayır | 
| policyAssignments | Hayır | 
| policyDefinitions | Hayır | 
| policySetDefinitions | Hayır | 
| providerOperations | Hayır | 
| Rol | Hayır | 
| roleDefinitions | Hayır | 

## <a name="microsoftautomation"></a>Gibi Microsoft.Automation
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| AutomationAccounts | Evet | 
| automationAccounts/yapılandırmalar | Evet | 
| automationAccounts/işleri | Hayır | 
| automationAccounts/runbook'ları | Evet | 
| automationAccounts/softwareUpdateConfigurations | Hayır | 
| automationAccounts/Web kancaları | Hayır | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Ortamlar | Hayır | 
| ortamları/hesapları | Hayır | 
| hesapları/ortam/ad | Hayır | 
| ortamları/hesapları/ad/yapılandırmalar | Hayır | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| b2cDirectories | Evet | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| kayıtları | Evet | 
| kayıtları/customerSubscriptions | Hayır | 
| kayıtları/ürünleri | Hayır | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| batchAccounts | Evet | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| billingAccounts | Hayır | 
| billingAccounts/billingProfiles | Hayır | 
| billingProfiles/billingAccounts/billingSubscriptions | Hayır | 
| billingProfiles/billingAccounts/faturalar | Hayır | 
| billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır | 
| billingProfiles/billingAccounts/operationStatus | Hayır | 
| billingProfiles/billingAccounts/paymentMethods | Hayır | 
| billingAccounts/billingProfiles/ilkeler | Hayır | 
| billingAccounts/billingProfiles/fiyat listesi | Hayır | 
| billingProfiles/billingAccounts/ürünleri | Hayır | 
| billingProfiles/billingAccounts/işlem | Hayır | 
| billingAccounts/billingSubscriptions | Hayır | 
| billingAccounts/Departmanlar | Hayır | 
| billingAccounts/eligibleOffers | Hayır | 
| billingAccounts/enrollmentAccounts | Hayır | 
| billingAccounts/faturalar | Hayır | 
| billingAccounts/invoiceSections | Hayır | 
| invoiceSections/billingAccounts/billingSubscriptions | Hayır | 
| billingAccounts/invoiceSections/billingSubscriptions/Aktarım | Hayır | 
| invoiceSections/billingAccounts/importRequests | Hayır | 
| billingAccounts/invoiceSections/initiateImportRequest | Hayır | 
| invoiceSections/billingAccounts/initiateTransfer | Hayır | 
| invoiceSections/billingAccounts/operationStatus | Hayır | 
| invoiceSections/billingAccounts/ürünleri | Hayır | 
| billingAccounts/invoiceSections/Aktarım | Hayır | 
| billingAccounts/ürünleri | Hayır | 
| billingAccounts/projeler | Hayır | 
| billingAccounts/proje/billingSubscriptions | Hayır | 
| billingAccounts/projects/importRequests | Hayır | 
| billingAccounts/projects/initiateImportRequest | Hayır | 
| billingAccounts/proje/operationStatus | Hayır | 
| billingAccounts/proje/ürünleri | Hayır | 
| billingAccounts/işlem | Hayır | 
| billingPeriods | Hayır | 
| BillingPermissions | Hayır | 
| billingProperty | Hayır | 
| BillingRoleAssignments | Hayır | 
| BillingRoleDefinitions | Hayır | 
| CreateBillingRoleAssignment | Hayır | 
| Departmanlar | Hayır | 
| enrollmentAccounts | Hayır | 
| importRequests | Hayır | 
| importRequests/acceptImportRequest | Hayır | 
| importRequests/declineImportRequest | Hayır | 
| Faturalar | Hayır | 
| Aktarımları | Hayır | 
| aktarımları/acceptTransfer | Hayır | 
| aktarımları/declineTransfer | Hayır | 
| aktarımları/operationStatus | Hayır | 
| usagePlans | Hayır | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| mapApis | Evet | 
| updateCommunicationPreference | Hayır | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| BizTalk | Evet | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| blueprintAssignments | Hayır | 
| blueprintAssignments/assignmentOperations | Hayır | 
| blueprintAssignments/işlemleri | Hayır | 
| şemaları | Hayır | 
| Blueprint/yapıları | Hayır | 
| Blueprint/sürümleri | Hayır | 
| Blueprint/sürümleri/yapıtları | Hayır | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| botServices | Evet | 
| botServices kanallara | Hayır | 
| botServices/bağlantıları | Hayır | 

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Redis | Evet | 
| RedisConfigDefinition | Hayır | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| appliedReservations | Hayır | 
| calculatePrice | Hayır | 
| katalogları | Hayır | 
| commercialReservationOrders | Hayır | 
| reservationOrders | Hayır | 
| reservationOrders/calculateRefund | Hayır | 
| reservationOrders/merge | Hayır | 
| reservationOrders/ayırmalar | Hayır | 
| rezervasyonlar/reservationOrders/düzeltme | Hayır | 
| reservationOrders/return'e | Hayır | 
| reservationOrders/Böl | Hayır | 
| reservationOrders/değiştirme | Hayır | 
| rezervasyonlar | Hayır | 
| kaynaklar | Hayır | 
| validateReservationOrder | Hayır | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| edgenodes | Hayır | 
| Profilleri | Evet | 
| profilleri/uç noktaları | Evet | 
| uç noktalar/profilleri/customdomains | Hayır | 
| uç noktalar/profilleri/kaynakları | Hayır | 
| validateProbe | Hayır | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| certificateOrders | Evet | 
| certificateOrders/sertifikalar | Hayır | 
| validateCertificateRegistrationInformation | Hayır | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Özellikleri | Hayır | 
| domainNames | Hayır | 
| domainNames/özellikleri | Hayır | 
| domainNames/internalLoadBalancers | Hayır | 
| domainNames/serviceCertificates | Hayır | 
| domainNames/Yuvalar | Hayır | 
| domainNames/yuvaları/roller | Hayır | 
| moveSubscriptionResources | Hayır | 
| operatingSystemFamilies | Hayır | 
| operatingSystems | Hayır | 
| quotas | Hayır | 
| resourcetypes: | Hayır | 
| validateSubscriptionMoveAvailability | Hayır | 
| virtualMachines | Hayır | 
| virtualMachines/diagnosticSettings | Hayır | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| classicInfrastructureResources | Hayır | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Özellikleri | Hayır | 
| expressRouteCrossConnections | Hayır | 
| expressRouteCrossConnections/eşlemeleri | Hayır | 
| gatewaySupportedDevices | Hayır | 
| networkSecurityGroups | Hayır | 
| quotas | Hayır | 
| ReservedIP | Hayır | 
| virtualNetworks | Hayır | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Hayır | 
| virtualNetworks/virtualNetworkPeerings | Hayır | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Özellikleri | Hayır | 
| diskler | Hayır | 
| images | Hayır | 
| osImages | Hayır | 
| osPlatformImages | Hayır | 
| publicImages | Hayır | 
| quotas | Hayır | 
| storageAccounts | Hayır | 
| storageAccounts/services | Hayır | 
| storageAccounts/services/diagnosticSettings | Hayır | 
| storageAccounts/Vmımages | Hayır | 
| Vmımages | Hayır | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| RateCard | Hayır | 
| UsageAggregates | Hayır | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Tam modda silme |
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
| virtualMachineScaleSets | Evet | 
| virtualMachineScaleSets ve uzantıları | Hayır | 
| virtualMachineScaleSets/networkınterface'lerden bazıları | Hayır | 
| virtualMachineScaleSets/publicIPAddresses | Hayır | 
| virtualMachineScaleSets/virtualMachines | Hayır | 
| virtualMachineScaleSets/virtualMachines/networkınterface'lerden bazıları | Hayır | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| AggregatedCost | Hayır | 
| Bakiyeler | Hayır | 
| Bütçeler | Hayır | 
| Ücretler | Hayır | 
| CostTags | Hayır | 
| Krediler | Hayır | 
| etkinlikler | Hayır | 
| Tahminler | Hayır | 
| çok sayıda | Hayır | 
| Pazar | Hayır | 
| Pricesheets | Hayır | 
| ürünler | Hayır | 
| ReservationDetails | Hayır | 
| ReservationRecommendations | Hayır | 
| ReservationSummaries | Hayır | 
| ReservationTransactions | Hayır | 
| Etiketler | Hayır | 
| Koşullar | Hayır | 
| UsageDetails | Hayır | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| containerGroups | Evet | 
| serviceAssociationLinks | Hayır | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| kayıt defterleri | Evet | 
| kayıt defterleri/yapılar | Hayır | 
| kayıt defterleri/yapı/iptal | Hayır | 
| kayıt defterleri/yapı/getLogLink | Hayır | 
| kayıt defterleri/buildTasks | Evet | 
| kayıt defterleri/buildTasks/adımları | Hayır | 
| kayıt defterleri/eventGridFilters | Hayır | 
| kayıt defterleri/getBuildSourceUploadUrl | Hayır | 
| kayıt defterleri/GetCredentials | Hayır | 
| kayıt defterleri/importImage | Hayır | 
| kayıt defterleri/queueBuild | Hayır | 
| kayıt defterleri/regenerateCredential | Hayır | 
| kayıt defterleri/regenerateCredentials | Hayır | 
| kayıt defterleri/çoğaltmalar | Evet | 
| kayıt defterleri/çalıştırmaları | Hayır | 
| kayıt defterleri/çalıştırmaları/iptal | Hayır | 
| kayıt defterleri/scheduleRun | Hayır | 
| kayıt defterleri/görevleri | Evet | 
| kayıt defterleri/updatePolicies | Hayır | 
| kayıt defterleri/Web kancaları | Evet | 
| Web kancaları/kayıt defterleri/getCallbackConfig | Hayır | 
| Web kancaları/kayıt defterleri/ping | Hayır | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| containerServices | Evet | 
| managedClusters | Evet | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| uygulamalar | Evet | 
| updateCommunicationPreference | Hayır | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Uyarılar | Hayır | 
| billingAccounts | Hayır | 
| Bağlayıcılar | Evet | 
| Bölümler | Hayır | 
| Boyutlar | Hayır | 
| enrollmentAccounts | Hayır | 
| Sorgu | Hayır | 
| Kaydolun | Hayır | 
| Reportconfigs | Hayır | 
| Reports | Hayır | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Hub'ları | Evet | 
| hub'ları / authorizationPolicies | Hayır | 
| hub'ları / bağlayıcılar | Hayır | 
| bağlayıcıların/hubs/eşlemeleri | Hayır | 
| hub'ları / etkileşimleri | Hayır | 
| hub'ları / KPI | Hayır | 
| hub'ları / bağlantıları | Hayır | 
| hub'ları / profilleri | Hayır | 
| hub'ları / rol | Hayır | 
| hub'ları / roller | Hayır | 
| hubs/suggestTypeSchema | Hayır | 
| hub'ları / görünümler | Hayır | 
| hub'ları / widgetTypes | Hayır | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| işler | Evet | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| DataBoxEdgeDevices | Evet | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| çalışma alanı | Evet | 
| çalışma alanları/virtualNetworkPeerings | Hayır | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| katalogları | Evet | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| connectionManagers | Evet | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| dataFactories | Evet | 
| dataFactories/diagnosticSettings | Hayır | 
| dataFactorySchema | Hayır | 
| fabrikaları | Evet | 
| fabrikaları/integrationRuntimes | Hayır | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 
| hesapları/dataLakeStoreAccounts | Hayır | 
| hesapları/storageAccounts | Hayır | 
| hesapları/storageAccounts/kapsayıcılar | Hayır | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 
| hesapları/eventGridFilters | Hayır | 
| hesapları/firewallRules | Hayır | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| services | Evet | 
| Hizmetleri/projeleri | Evet | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| sunucu | Evet | 
| sunucuları/danışmanları | Hayır | 
| sunucuları/queryTexts | Hayır | 
| sunucuları/recoverableServers | Hayır | 
| sunucuları/topQueryStatistics | Hayır | 
| sunucuları/virtualNetworkRules | Hayır | 
| sunucuları/waitStatistics | Hayır | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| IotHubs | Evet | 
| IotHubs/eventGridFilters | Hayır | 
| ProvisioningServices | Evet | 
| Kullanımları | Hayır | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Denetleyicileri | Evet | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Laboratuvarları | Evet | 
| Labs/serviceRunners | Evet | 
| Labs/virtualMachines | Evet | 
| Zamanlamaları | Evet | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| databaseAccountNames | Hayır | 
| databaseAccounts | Evet | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| etki alanları | Evet | 
| etki alanı/domainOwnershipIdentifiers | Hayır | 
| generateSsoRequest | Hayır | 
| topLevelDomains | Hayır | 
| validateDomainRegistrationInformation | Hayır | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| lcsprojects | Hayır | 
| lcsprojects/clouddeployments | Hayır | 
| lcsprojects/bağlayıcılar | Hayır | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| etki alanları | Evet | 
| etki alanı/konuları | Hayır | 
| eventSubscriptions | Hayır | 
| extensionTopics | Hayır | 
| konuları | Evet | 
| topicTypes | Hayır | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kümeleri | Evet | 
| ad alanları | Evet | 
| ad/authorizationrules öğesine | Hayır | 
| ad/disasterrecoveryconfigs | Hayır | 
| ad/eventhubs | Hayır | 
| ad/eventhubs/authorizationrules öğesine | Hayır | 
| ad/eventhubs/consumergroups | Hayır | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| SaaS Uygulamaları Geliştirme | Hayır | 
| sağlayıcıları | Hayır | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kaydetme | Hayır | 
| galleryitems | Hayır | 
| generateartifactaccessuri | Hayır | 
| myareas | Hayır | 
| myareas/alanları | Hayır | 
| myareas/alanlar/alanları | Hayır | 
| myareas/alanlar/alanlar/galleryitems | Hayır | 
| myareas/alanlar/galleryitems | Hayır | 
| myareas/galleryitems | Hayır | 
| Kaydolun | Hayır | 
| kaynaklar | Hayır | 
| retrieveresourcesbyid | Hayır | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| guestConfigurationAssignments | Hayır | 
| Yazılım | Hayır | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| hanaInstances | Evet | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kümeleri | Hayır | 
| kümeleri/uygulamaları | Hayır | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| işler | Evet | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| labelGroups | Hayır | 
| labelGroups/etiketleri | Hayır | 
| etiketleri/labelGroups/koşulları | Hayır | 
| etiketleri/labelGroups/subLabels | Hayır | 
| labelGroups/etiketleri/subLabels/koşulları | Hayır | 

## <a name="microsoftinsights"></a>Microsoft.insights
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| actiongroups | Evet | 
| activityLogAlerts | Evet | 
| alertrules | Evet | 
| automatedExportSettings | Hayır | 
| autoscalesettings | Evet | 
| temel | Hayır | 
| calculatebaseline | Hayır | 
| Bileşenleri | Evet | 
| bileşenleri/olaylar | Hayır | 
| components/pricingPlans | Hayır | 
| bileşenleri/sorgu | Hayır | 
| diagnosticSettings | Hayır | 
| diagnosticSettingsCategories | Hayır | 
| eventCategories | Hayır | 
| eventtypes | Hayır | 
| extendedDiagnosticSettings | Hayır | 
| logDefinitions | Hayır | 
| logprofiles | Hayır | 
| günlükler | Hayır | 
| metricAlerts | Evet |
| migrateToNewPricingModel | Hayır | 
| myWorkbooks | Hayır | 
| sorgu | Hayır | 
| rollbackToLegacyPricingModel | Hayır | 
| scheduledqueryrules | Evet | 
| vmInsightsOnboardingStatuses | Hayır | 
| Web testleri | Evet | 
| çalışma kitapları | Evet | 

## <a name="microsoftintune"></a>Microsoft.Intune
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| diagnosticSettings | Hayır | 
| diagnosticSettingsCategories | Hayır | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| IoTApps | Evet | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Graf | Evet | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| deletedVaults | Hayır | 
| kasaları | Evet | 
| kasaları/accessPolicies | Hayır | 
| Kasalar/parolalar | Hayır | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kümeleri | Evet | 
| kümeleri/veritabanları | Hayır | 
| veritabanları/kümeleri/dataconnections | Hayır | 
| veritabanları/kümeleri/eventhubconnections | Hayır | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| labaccounts | Evet | 
| kullanıcılar | Hayır | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| günlükler | Hayır | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| integrationAccounts | Evet | 
| İş akışları | Evet | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| commitmentPlans | Evet | 
| Veritabanınızdaki | Evet | 
| Çalışma Alanları | Evet | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 
| hesapları/çalışma alanları | Evet | 
| çalışma alanları/hesapları/projeleri | Evet | 
| teamAccounts | Evet | 
| teamAccounts/çalışma alanları | Evet | 
| çalışma alanları/teamAccounts/projeleri | Evet | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| çalışma alanı | Evet | 
| çalışma alanları/işlemleri | Hayır | 

## <a name="microsoftmanagedidentity"></a>Microsoft.managedıdentity
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kimlikler | Hayır | 
| Userassignedıdentities | Evet | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| getEntities | Hayır | 
| managementGroups | Hayır | 
| kaynaklar | Hayır | 
| startTenantBackfill | Hayır | 
| tenantBackfillStatus | Hayır | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 
| hesapları/eventGridFilters | Hayır | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Sunar | Hayır | 
| offerTypes | Hayır | 
| offerTypes/yayımcıları | Hayır | 
| Yayımcılar/offerTypes/teklifler | Hayır | 
| offerTypes/yayımcılar/teklif/planları | Hayır | 
| offerTypes/yayımcılar/teklif/planları/sözleşmeleri | Hayır | 
| offerTypes/yayımcılar/teklif/planları/yapılandırmalar | Hayır | 
| offerTypes/publishers/offers/plans/configs/importImage | Hayır | 
| privategalleryitems | Hayır | 
| ürünler | Hayır | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| classicDevServices | Evet | 
| updateCommunicationPreference | Hayır | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Anlaşmaları | Hayır | 
| offertypes | Hayır | 

## <a name="microsoftmedia"></a>Microsoft.Media
| Kaynak türü | Tam modda silme |
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
| mediaservices/streamingEndpointOperations | Hayır | 
| mediaservices/akış | Evet | 
| mediaservices/streamingLocators | Hayır | 
| mediaservices/streamingPolicies | Hayır | 
| mediaservices/dönüştürme | Hayır | 
| dönüşümler/mediaservices/işleri | Hayır | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Projeleri | Evet | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| applicationGateways | Evet | 
| Applicationsecuritygroup | Evet | 
| azureFirewallFqdnTags | Hayır | 
| azureFirewalls | Evet | 
| bgpServiceCommunities | Hayır | 
| bağlantılar | Evet | 
| ddosCustomPolicies | Evet | 
| ddosProtectionPlans | Evet | 
| dnsOperationStatuses | Hayır | 
| dnszones | Evet | 
| dnszones/A | Hayır | 
| dnszones/AAAA | Hayır | 
| dnszones/all | Hayır | 
| dnszones/CAA | Hayır | 
| dnszones/CNAME | Hayır | 
| dnszones/MX | Hayır | 
| dnszones/NS | Hayır | 
| dnszones/PTR | Hayır | 
| dnszones/kayıt kümeleri | Hayır | 
| dnszones/SOA | Hayır | 
| dnszones/SRV | Hayır | 
| dnszones/TXT | Hayır | 
| expressRouteCircuits | Evet | 
| expressRouteServiceProviders | Hayır | 
| frontdoors | Evet | 
| frontdoorWebApplicationFirewallPolicies | Evet | 
| getDnsResourceReference | Hayır | 
| interfaceEndpoints | Evet | 
| internalNotify | Hayır | 
| Sonraki | Evet | 
| localNetworkGateways | Evet | 
| natGateways | Evet | 
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
| trafficManagerGeographicHierarchies | Hayır | 
| trafficmanagerprofiles | Evet | 
| trafficmanagerprofiles/ısı haritaları | Hayır | 
| virtualHubs | Evet | 
| virtualNetworkGateways | Evet | 
| virtualNetworks | Evet | 
| virtualNetworkTaps | Evet | 
| virtualWans | Evet | 
| vpnGateways | Evet | 
| vpnSites | Evet | 
| webApplicationFirewallPolicies | Evet | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| ad alanları | Evet | 
| ad/notificationHubs | Evet | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| cihazlar | Hayır | 
| linkTargets | Hayır | 
| storageInsightConfigs | Hayır | 
| çalışma alanı | Evet | 
| çalışma alanları/veri kaynakları | Hayır | 
| çalışma alanları/linkedServices | Hayır | 
| çalışma alanları/sorgu | Hayır | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| managementassociations | Hayır | 
| managementconfigurations | Evet | 
| çözümler | Evet | 
| görüntüleme | Evet | 

## <a name="microsoftpolicyinsights"></a>Microsoft.policyınsights
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| policyEvents | Hayır | 
| policyStates | Hayır | 
| policyTrackedResources | Hayır | 
| düzeltmeler | Hayır | 

## <a name="microsoftportal"></a>Microsoft.Portal
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| konsolları | Hayır | 
| Panolar | Evet | 
| kullanıcı ayarlarını | Hayır | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| workspaceCollections | Evet | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kapasiteleri | Evet | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| accounts | Evet | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| backupProtectedItems | Hayır | 
| kasaları | Evet | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| ad alanları | Evet | 
| ad/authorizationrules öğesine | Hayır | 
| ad/hybridconnections | Hayır | 
| ad/hybridconnections/authorizationrules öğesine | Hayır | 
| ad/wcfrelays | Hayır | 
| ad/wcfrelays/authorizationrules öğesine | Hayır | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| kaynaklar | Hayır | 
| subscriptionsStatus | Hayır | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| availabilityStatuses | Hayır | 
| childAvailabilityStatuses | Hayır | 
| childResources | Hayır | 
| etkinlikler | Hayır | 
| impactedResources | Hayır | 
| bildirimler | Hayır | 

## <a name="microsoftresources"></a>Microsoft.Resources
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| dağıtımlar | Hayır | 
| Dağıtımları/işlemleri | Hayır | 
| Bağlantılar | Hayır | 
| notifyResourceJobs | Hayır | 
| sağlayıcıları | Hayır | 
| resourceGroups | Hayır | 
| kaynaklar | Hayır | 
| abonelik | Hayır | 
| Abonelikler/sağlayıcıları | Hayır | 
| Abonelikler/kaynak grupları | Hayır | 
| Abonelikler/resourcegroups/kaynaklar | Hayır | 
| Abonelikler/kaynak | Hayır | 
| Abonelikler/tagnames | Hayır | 
| Abonelikler/tagNames/tagValues | Hayır | 
| Kiracılar | Hayır | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| uygulamalar | Evet | 
| saasresources | Hayır | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| akışlar | Evet | 
| eyleminde | Evet | 

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| resourceHealthMetadata | Hayır | 
| searchServices | Evet | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| advancedThreatProtectionSettings | Hayır | 
| alerts | Hayır | 
| allowedConnections | Hayır | 
| cihazları | Hayır | 
| applicationWhitelistings | Hayır | 
| AutoProvisioningSettings | Hayır | 
| Özellikleri | Hayır | 
| dataCollectionAgents | Hayır | 
| discoveredSecuritySolutions | Hayır | 
| externalSecuritySolutions | Hayır | 
| InformationProtectionPolicies | Hayır | 
| jitNetworkAccessPolicies | Hayır | 
| izleme | Hayır | 
| İzleme/kötü amaçlı yazılımdan koruma | Hayır | 
| İzleme temel | Hayır | 
| İzleme/düzeltme eki | Hayır | 
| ilkeler | Hayır | 
| fiyatları | Hayır | 
| securityContacts | Hayır | 
| securitySolutions | Hayır | 
| securitySolutionsReferenceData | Hayır | 
| securityStatus | Hayır | 
| securityStatus/uç noktaları | Hayır | 
| securityStatus/alt ağlar | Hayır | 
| securityStatus/virtualMachines | Hayır | 
| securityStatuses | Hayır | 
| securityStatusesSummaries | Hayır | 
| ayarlar | Hayır | 
| Görevler | Hayır | 
| Topolojileri | Hayır | 
| workspaceSettings | Hayır | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| diagnosticSettings | Hayır | 
| diagnosticSettingsCategories | Hayır | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| ad alanları | Evet | 
| ad/authorizationrules öğesine | Hayır | 
| ad/disasterrecoveryconfigs | Hayır | 
| ad/eventgridfilters | Hayır | 
| ad/kuyruklar | Hayır | 
| ad/kuyruk/authorizationrules öğesine | Hayır | 
| ad/konuları | Hayır | 
| ad alanları/konu/authorizationrules öğesine | Hayır | 
| ad/konular/abonelikler | Hayır | 
| ad alanları veya konular/abonelikler/kurallara | Hayır | 
| premiumMessagingRegions | Hayır | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Kümeleri | Evet | 
| kümeleri/uygulamaları | Hayır | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| uygulamalar | Evet | 
| Ağ geçitleri | Evet | 
| ağlar | Evet | 
| gizli dizi | Evet | 
| volumes | Evet | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| SignalR | Evet | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| applianceDefinitions | Evet | 
| cihazları | Evet | 
| applicationDefinitions | Evet | 
| uygulamalar | Evet | 
| jitRequests | Evet | 

## <a name="microsoftsql"></a>Microsoft.SQL
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| managedInstances | Evet |
| managedInstances/veritabanları | Evet (aşağıdaki nota bakın) |
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
| sunucuları/communicationLinks | Hayır | 
| sunucuları/veritabanları | Evet (aşağıdaki nota bakın) | 
| sunucuları/encryptionProtector | Hayır | 
| sunucuları/firewallRules | Hayır | 
| sunucuları/anahtarları | Hayır | 
| sunucuları/restorableDroppedDatabases | Hayır | 
| sunucuları/serviceobjectives | Hayır | 
| sunucuları/tdeCertificates | Hayır | 

> [!NOTE]
> Asıl veritabanı etiketleri desteklemez, ancak etiketleri veri ambarı veritabanları dahil olmak üzere, diğer veritabanlarını destekler.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| SqlVirtualMachineGroups | Evet | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Hayır | 
| SqlVirtualMachines | Evet | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| storageAccounts | Evet | 
| storageAccounts/blobServices | Hayır | 
| storageAccounts/fileServices | Hayır | 
| storageAccounts/queueServices | Hayır | 
| storageAccounts/services | Hayır | 
| storageAccounts/tableServices | Hayır | 
| Kullanımları | Hayır | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| storageSyncServices | Evet | 
| storageSyncServices/registeredServers | Hayır | 
| storageSyncServices/syncGroups | Hayır | 
| syncGroups/storageSyncServices/cloudEndpoints | Hayır | 
| syncGroups/storageSyncServices/serverEndpoints | Hayır | 
| storageSyncServices/iş akışları | Hayır | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Yöneticileri | Evet | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| streamingjobs | Evet (aşağıdaki nota bakın) | 
| streamingjobs/diagnosticSettings | Hayır | 

> [!NOTE]
> Streamingjobs çalışırken bir etiket ekleyemezsiniz. Bir etiket eklemek için kaynak durdurun.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| CreateSubscription | Hayır | 
| SubscriptionDefinitions | Hayır | 
| SubscriptionOperations | Hayır | 

## <a name="microsoftsupport"></a>Microsoft.support
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| supporttickets | Hayır | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| providerRegistrations | Evet | 
| kaynaklar | Evet | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Ortamlar | Evet | 
| ortamları/accessPolicies | Hayır | 
| ortamları/eventsources | Evet | 
| ortamları/referenceDataSets | Evet | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| account | Evet | 
| hesabı/uzantısı | Evet | 
| hesabı/proje | Evet | 

## <a name="microsoftweb"></a>Microsoft.Web
| Kaynak türü | Tam modda silme |
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
| hostingEnvironments/multiRolePools | Hayır | 
| multiRolePools/hostingEnvironments/örnekleri | Hayır | 
| hostingEnvironments/workerPools | Hayır | 
| workerPools/hostingEnvironments/örnekleri | Hayır | 
| publishingUsers | Hayır | 
| öneriler | Hayır | 
| resourceHealthMetadata | Hayır | 
| Çalışma zamanları | Hayır | 
| serverFarms | Evet | 
| serverFarms/çalışanları | Hayır | 
| siteler | Evet | 
| Site/domainOwnershipIdentifiers | Hayır | 
| Site/hostNameBindings | Hayır | 
| Site/örnekleri | Hayır | 
| Örnek/Site/uzantıları | Hayır | 
| Site/premieraddons | Evet | 
| Site/önerileri | Hayır | 
| Site/resourceHealthMetadata | Hayır | 
| Site/Yuvalar | Evet | 
| yuvaları/site/hostNameBindings | Hayır | 
| yuvaları/site/örnekleri | Hayır | 
| Siteler ve yuvaları/örnekleri/uzantıları | Hayır | 
| sourceControls | Hayır | 
| Doğrulama | Hayır | 
| verifyHostingEnvironmentVnet | Hayır | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| diagnosticSettings | Hayır | 
| diagnosticSettingsCategories | Hayır | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| DeviceServices | Evet | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Kaynak türü | Tam modda silme |
| ------------- | ----------- |
| Bileşenleri | Hayır | 
| componentsSummary | Hayır | 
| monitorInstances | Hayır | 
| monitorInstancesSummary | Hayır | 
| İzleyiciler | Hayır | 
| notificationSettings | Hayır | 

## <a name="next-steps"></a>Sonraki adımlar
Etiketler kaynaklara öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md).
