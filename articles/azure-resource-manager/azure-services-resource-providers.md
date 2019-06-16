---
title: Azure Hizmetleri ile Azure Resource Manager kaynak sağlayıcıları
description: Azure Resource Manager için tüm kaynak sağlayıcısı ad alanlarını listeler ve bu ad alanı için Azure hizmet gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/25/2019
ms.author: tomfitz
ms.openlocfilehash: 54493efdc0bffcbb4654b65676554f6707716968
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65235580"
---
# <a name="resource-providers-for-azure-services"></a>Azure Hizmetleri için kaynak sağlayıcıları

Bu makalede, kaynak sağlayıcı ad alanları için Azure hizmetleri nasıl eşleştiği gösterilir.

## <a name="match-resource-provider-to-service"></a>Hizmet için eşleşen kaynak sağlayıcısı

| Kaynak sağlayıcısı ad alanı | Azure hizmeti |
| --------------------------- | ------------- |
| Microsoft.AAD | [Azure Active Directory etki alanı Hizmetleri](../active-directory-domain-services/index.yml) |
| microsoft.aadiam | [Azure Active Directory](/azure/active-directory/) |
| Microsoft.Addons | çekirdek |
| Microsoft.ADHybridHealthService | [Azure Active Directory](/azure/active-directory/) |
| Microsoft.Advisor | [Azure Danışmanı](../advisor/index.yml) |
| Microsoft.AlertsManagement | [Azure İzleyici](../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](/azure/analysis-services/) |
| Microsoft.ApiManagement | [API Yönetimi](../api-management/index.yml) |
| Microsoft.AppConfiguration | çekirdek |
| Microsoft.Authorization | [Azure Resource Manager](index.yml) |
| Gibi Microsoft.Automation | [Otomasyon](../automation/index.yml) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../active-directory-b2c/index.yml) |
| Microsoft.AzureStack | [Azure Stack](/azure-stack/user/) |
| Microsoft.Batch | [Batch](../batch/index.yml) |
| Microsoft.Billing | [Billing](/azure/billing/) |
| Microsoft.BingMaps | [Bing Haritalar](https://docs.microsoft.com/BingMaps/#pivot=main&panel=BingMapsAPI) |
| Microsoft.BizTalkServices | [BizTalk Hizmetleri](../logic-apps/logic-apps-move-from-mabs.md) |
| Microsoft.Blockchain | [Azure Blockchain hizmeti](/azure/blockchain/workbench/) |
| Microsoft.Blueprint | [Azure şemaları](/azure/governance/blueprints/) |
| Microsoft.BotService | [Azure Bot hizmeti](/azure/bot-service/) |
| Microsoft.Cache | [Redis için Azure Önbelleği](/azure/azure-cache-for-redis/) |
| Microsoft.Capacity | çekirdek |
| Microsoft.Cdn | [İçerik teslim ağı](../cdn/index.yml) |
| Microsoft.CertificateRegistration | [App Service sertifikaları](../app-service/web-sites-purchase-ssl-web-site.md) |
| Microsoft.ClassicCompute | Klasik dağıtım modeli sanal makine |
| Microsoft.ClassicInfrastructureMigrate | Klasik dağıtım modeline geçiş |
| Microsoft.ClassicNetwork | Klasik dağıtım modeli sanal ağı |
| Microsoft.ClassicStorage | Klasik dağıtım modeli depolama |
| Microsoft.ClassicSubscription | Klasik dağıtım modeli |
| Microsoft.CognitiveServices | [Bilişsel hizmetler](/azure/cognitive-services/) |
| Microsoft.Commerce | çekirdek |
| Microsoft.Compute | [Sanal makineler](/azure/virtual-machines/) |
| Microsoft.Consumption | [Maliyet Yönetimi](/azure/cost-management/) |
| Microsoft.ContainerInstance | [Container Instances](/azure/container-instances/) |
| Microsoft.ContainerRegistry | [Kapsayıcı kayıt defteri](/azure/container-registry/) |
| Microsoft.ContainerService | [Azure Kubernetes Service'i (AKS)](/azure/aks/) |
| Microsoft.ContentModerator | [Azure Content Moderator](../cognitive-services/content-moderator/index.yml) |
| Microsoft.CostManagement | [Maliyet Yönetimi](/azure/cost-management/) |
| Microsoft.CustomerInsights | Customer Insights |
| Microsoft.CustomerLockbox | Microsoft Azure müşteri kasa |
| Microsoft.DataBox | [Azure Data Box '](/azure/databox-family/) |
| Microsoft.DataBoxEdge | [Azure Data Box Edge](../databox-online/data-box-edge-overview.md) |
| Microsoft.Databricks | [Azure Databricks](/azure/azure-databricks/) |
| Microsoft.DataCatalog | [Veri Kataloğu](/azure/data-catalog/) |
| Microsoft.DataFactory | [Veri Fabrikası](/azure/data-factory/) |
| Microsoft.DataLakeAnalytics | [Data Lake Analytics](/azure/data-lake-analytics/) |
| Microsoft.DataLakeStore | [Azure Data Lake Store](../storage/blobs/data-lake-storage-introduction.md) |
| Microsoft.DataMigration | [Azure veritabanı geçiş hizmeti](/azure/dms/) |
| Microsoft.DBforMariaDB | [MariaDB için Azure veritabanı](/azure/mariadb/) |
| Microsoft.DBforMySQL | [MySQL için Azure veritabanı](/azure/mysql/) |
| Microsoft.DBforPostgreSQL | [PostgreSQL için Azure veritabanı](/azure/postgresql/) |
| Microsoft.DeploymentManager | [Azure Dağıtım Yöneticisi](deployment-manager-overview.md) |
| Microsoft.Devices | [IOT hub'ı](/azure/iot-hub/) |
| Microsoft.DevSpaces | [Azure geliştirme alanları](/azure/dev-spaces/) |
| Microsoft.DevTestLab | [Azure Lab Services'i](../lab-services/index.yml) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../cosmos-db/index.yml) |
| Microsoft.DomainRegistration | [App Service](/azure/app-service/) |
| Microsoft.EnterpriseKnowledgeGraph | Kurumsal bilgi grafiği |
| Microsoft.EventGrid | [Olay Kılavuzu](/azure/event-grid/) |
| Microsoft.EventHub | [Event Hubs](../event-hubs/index.yml) |
| Microsoft.Features | [Azure Resource Manager](index.yml) |
| Microsoft.Genomics | [Microsoft Genomiks](/azure/genomics/) |
| Microsoft.GuestConfiguration | [Azure İlkesi](../governance/policy/index.yml) |
| Microsoft.HanaOnAzure | [Azure'da SAP HANA](../virtual-machines/workloads/sap/hana-overview-architecture.md) |
| Microsoft.HardwareSecurityModules | [Azure ayrılmış HSM](../dedicated-hsm/index.yml) |
| Microsoft.HDInsight | [HDInsight](../hdinsight/index.yml) |
| Microsoft.HealthcareApis | [Azure API FHIR için](../healthcare-apis/index.yml) |
| Microsoft.ImportExport | [Azure içeri/dışarı aktarma](../storage/common/storage-import-export-service.md) |
| Microsoft.insights | [Azure İzleyici](../azure-monitor/index.yml) |
| Microsoft.Intune | [Intune](/intune/) |
| Microsoft.IoTCentral | [IOT Central](/azure/iot-central/) |
| Microsoft.IoTSpaces | [Azure dijital çiftleri](../digital-twins/index.yml) |
| Microsoft.KeyVault | [Anahtar Kasası](../key-vault/index.yml) |
| Microsoft.Kusto | [Azure Veri Gezgini](../data-explorer/index.yml) |
| Microsoft.LabServices | [Azure Lab Services'i](../lab-services/index.yml) |
| Microsoft.LocationBasedServices | [Azure haritalar](../azure-maps/index.yml) |
| Microsoft.LocationServices | çekirdek |
| Microsoft.LogAnalytics | [Azure İzleyici](../azure-monitor/index.yml) |
| Microsoft.Logic | [Logic Apps](../logic-apps/index.yml) |
| Microsoft.MachineLearning | [Machine Learning Studio'da](../machine-learning/studio/index.yml) |
| Microsoft.MachineLearningCompute | [Machine Learning hizmeti](../machine-learning/service/index.yml) |
| Microsoft.MachineLearningModelManagement | [Machine Learning hizmeti](../machine-learning/service/index.yml) |
| Microsoft.MachineLearningServices | [Machine Learning hizmeti](../machine-learning/service/index.yml) |
| Microsoft.managedıdentity | [Azure kaynakları için yönetilen kimlikler](../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft.ManagedLab | [Azure Lab Services'i](../lab-services/index.yml) |
| Microsoft.Management | [Yönetim grupları](/azure/governance/management-groups/) |
| Microsoft.Maps | [Azure haritalar](../azure-maps/index.yml) |
| Microsoft.Marketplace | çekirdek |
| Microsoft.MarketplaceApps | çekirdek |
| Microsoft.MarketplaceOrdering | çekirdek |
| Microsoft.Media | [Media Services](../media-services/index.yml) |
| Microsoft.Migrate | [Azure geçişi](../migrate/migrate-overview.md) |
| Microsoft.MixedReality | [Azure uzamsal yer işaretleri](/azure/spatial-anchors/) |
| Microsoft.NetApp | [Azure NetApp dosyaları](../azure-netapp-files/index.yml) |
| Microsoft.Network | [Sanal Ağ](../virtual-network/index.yml)<br />[Yük Dengeleyici](../load-balancer/index.yml)<br />[Application Gateway](../application-gateway/index.yml)<br />[Azure DNS](../dns/index.yml)<br />[ExpressRoute](../expressroute/index.yml)<br />[VPN Gateway](../vpn-gateway/index.yml)<br />[Traffic Manager](../traffic-manager/index.yml)<br />[Ağ İzleyicisi](../network-watcher/index.yml)<br />[Azure güvenlik duvarı](../firewall/index.yml)<br />[Azure ön kapısı hizmeti](../frontdoor/index.yml) |
| Microsoft.NotificationHubs | [Notification Hubs](../notification-hubs/index.yml) |
| Microsoft.OffAzure | [Azure geçişi](../migrate/migrate-overview.md) |
| Microsoft.OperationalInsights | [Azure İzleyici](../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure İzleyici](../azure-monitor/index.yml) |
| Microsoft.policyınsights | [Azure İlkesi](../governance/policy/index.yml) |
| Microsoft.Portal | [Azure portal](/azure/azure-portal/) |
| Microsoft.PowerBI | [Power BI](/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/azure/power-bi-embedded/) |
| Microsoft.RecoveryServices | [Site Recovery](../site-recovery/index.yml) |
| Microsoft.Relay | [Azure geçişi](../service-bus-relay/relay-what-is-it.md) |
| Microsoft.ResourceHealth | çekirdek |
| Microsoft.Resources | [Azure Resource Manager](index.yml) |
| Microsoft.SaaS | çekirdek |
| Microsoft.Scheduler | [Scheduler](/azure/scheduler/) |
| Microsoft.Search | [Azure arama](../search/index.yml) |
| Microsoft.Security | [Güvenlik Merkezi](../security-center/index.yml) |
| Microsoft.ServiceBus | [Service Bus](/azure/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../service-fabric/index.yml) |
| Microsoft.ServiceFabricMesh | [Service Fabric Mesh](../service-fabric-mesh/index.yml) |
| Microsoft.SignalRService | [Azure SignalR hizmeti](../azure-signalr/index.yml) |
| Microsoft.SiteRecovery | [Site Recovery](../site-recovery/index.yml) |
| Microsoft.Solutions | [Azure yönetilen uygulamalar](../managed-applications/index.yml) |
| Microsoft.Sql | [Azure SQL Veritabanı](../sql-database/index.yml) |
| Microsoft.SqlVirtualMachine | [Azure Sanal Makinelerde SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) |
| Microsoft.Storage | [Depolama](../storage/index.yml) |
| Microsoft.StorageSync | [Depolama](../storage/index.yml) |
| Microsoft.StorSimple | [StorSimple](/azure/storsimple/) |
| Microsoft.StreamAnalytics | [Akış Analizi](../stream-analytics/index.yml) |
| Microsoft.Subscription | çekirdek |
| Microsoft.support | çekirdek |
| Microsoft.TimeSeriesInsights | [Time Series Insights](../time-series-insights/index.yml) |
| microsoft.visualstudio | [Azure DevOps](/azure/devops/?view=azure-devops) |
| Microsoft.Web | [App Service](../app-service/index.yml)<br />[İşlevler](../azure-functions/index.yml) |
| Microsoft.WindowsDefenderATP | [Windows Defender Advanced Threat Protection](/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection) |
| Microsoft.WindowsIoT | [Windows 10 IoT Core Hizmetleri](https://docs.microsoft.com/windows-hardware/manufacture/iot/iotcoreservicesoverview) |
| Microsoft.WorkloadMonitor | [Azure İzleyici](../azure-monitor/index.yml) |

## <a name="next-steps"></a>Sonraki adımlar

Kaynak sağlayıcıları hakkında daha fazla bilgi için bkz: [Azure kaynak sağlayıcıları ve türleri](resource-manager-supported-services.md)
