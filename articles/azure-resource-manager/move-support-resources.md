---
title: İşlem desteği Azure kaynak türüne göre Taşı
description: Yeni kaynak grubuna veya aboneliğe taşınabilir Azure kaynak türlerini listeler.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 7/9/2019
ms.author: tomfitz
ms.openlocfilehash: 093c20407cb6210125106189f36566f539de0dcc
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721105"
---
# <a name="move-operation-support-for-resources"></a>İşlem desteği kaynakları Taşı
Bu makalede, bir Azure kaynak türü taşıma işlemini destekleyip desteklemediğini listelenmektedir. Ayrıca, bir kaynak taşırken dikkate alınması gereken özel durumları hakkında bilgi sağlar.

Bir kaynak sağlayıcısı ad alanı için atla:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Gibi Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Backup](#microsoftbackup)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [microsoft.dns](#microsoftdns)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.managedıdentity](#microsoftmanagedidentity)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Sayısı](#microsoftrelay)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SiteRecovery](#microsoftsiterecovery)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft.AAD
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| domainservices | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kiracılar | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| actionrules | Evet | Evet |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Sunucuları | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hizmet | Evet | Evet |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| configurationstores | Evet | Evet |

## <a name="microsoftappservice"></a>Microsoft.AppService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| apiapps | Hayır | Hayır |
| appidentities | Hayır | Hayır |
| Ağ geçitleri | Hayır | Hayır |

> [!IMPORTANT]
> Bkz: [App Service Taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| policyassignments | Hayır | Hayır |

## <a name="microsoftautomation"></a>Gibi Microsoft.Automation
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| automationaccounts | Evet | Evet |
| automationaccounts/yapılandırmalar | Evet | Evet |
| automationaccounts/runbook'ları | Evet | Evet |

> [!IMPORTANT]
> Runbook'ları, Otomasyon hesabı aynı kaynak grubunda mevcut olması gerekir.

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| b2cdirectories | Evet | Evet |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kayıtları | Evet | Evet |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| backupvault | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| batchaccounts | Evet | Evet |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kümeleri | Hayır | Hayır |
| fileservers | Hayır | Hayır |
| İşleri | Hayır | Hayır |
| Çalışma alanları | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| mapapis | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| biztalk | Evet | Evet |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| blockchainmembers | Evet | Evet |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| blueprintassignments | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Redis | Evet | Evet |

> [!IMPORTANT]
> Azure Cache Redis örneği için bir sanal ağ ile yapılandırılmışsa, örneği farklı bir aboneliğe taşınamaz. Bkz: [sanal ağlar taşıma sınırlamaları](./move-limitations/virtual-network-move-limitations.md).

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| profiles | Evet | Evet |
| profilleri/uç noktaları | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| certificateorders | Evet | Evet |

> [!IMPORTANT]
> Bkz: [App Service Taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| domainnames | Evet | Hayır |
| virtualmachines | Evet | Hayır |

> [!IMPORTANT]
> Bkz: [Klasik dağıtım Taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, belirli bir işlem olan Aboneliklerde bu senaryo için taşınabilir.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Hayır | Hayır |
| ReservedIP | Hayır | Hayır |
| virtualnetworks | Hayır | Hayır |

> [!IMPORTANT]
> Bkz: [Klasik dağıtım Taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, belirli bir işlem olan Aboneliklerde bu senaryo için taşınabilir.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Evet | Hayır |

> [!IMPORTANT]
> Bkz: [Klasik dağıtım Taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, belirli bir işlem olan Aboneliklerde bu senaryo için taşınabilir.

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| availabilitysets | Evet | Evet |
| Diskler | Evet | Evet |
| galeriler | Hayır | Hayır |
| galeriler/görüntüleri | Hayır | Hayır |
| galeriler/resimler/sürümleri | Hayır | Hayır |
| hostgroups | Hayır | Hayır |
| hostgroups/ana bilgisayarları | Hayır | Hayır |
| images | Evet | Evet |
| proximityplacementgroups | Hayır | Hayır |
| restorepointcollections | Hayır | Hayır |
| sharedvmimages | Hayır | Hayır |
| sharedvmimages/sürümleri | Hayır | Hayır |
| Anlık görüntüleri | Evet | Evet |
| virtualmachines | Evet | Evet |
| virtualmachines ve uzantıları | Evet | Evet |
| virtualmachinescalesets | Evet | Evet |

> [!IMPORTANT]
> Bkz: [sanal makineleri Taşıma Kılavuzu](./move-limitations/virtual-machines-move-limitations.md).

## <a name="microsoftcontainer"></a>Microsoft.Container
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containergroups | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containergroups | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kayıt defterleri | Evet | Evet |
| kayıt defterleri/buildtasks | Evet | Evet |
| kayıt defterleri/çoğaltmalar | Evet | Evet |
| kayıt defterleri/görevleri | Evet | Evet |
| kayıt defterleri/Web kancaları | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containerservices | Hayır | Hayır |
| managedclusters | Hayır | Hayır |
| openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Evet |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| bağlayıcılar | Evet | Evet |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Hub'ları | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| İşleri | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Çalışma alanları | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| katalogları | Evet | Evet |
| datacatalogs | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| connectionmanagers | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Paketleri | Hayır | Hayır |
| Planları | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datafactories | Evet | Evet |
| fabrikaları | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Hayır | Hayır |
| Hizmetleri/projeleri | Hayır | Hayır |
| yuvaları | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Sunucuları | Evet | Evet |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Sunucuları | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| servergroups | Hayır | Hayır |
| Sunucuları | Evet | Evet |
| serversv2 | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| artifactsources | Evet | Evet |
| Piyasaya çıkarma | Evet | Evet |
| servicetopologies | Evet | Evet |
| servicetopologies/hizmetler | Evet | Evet |
| servicetopologies/services/serviceunits | Evet | Evet |
| adımlar | Evet | Evet |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| elasticpools | Hayır | Hayır |
| elasticpools/iothubtenants | Hayır | Hayır |
| iothubs | Evet | Evet |
| provisioningservices | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Denetleyicileri | Hayır | Hayır |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| labcenters | Hayır | Hayır |
| Laboratuvarları | Evet | Hayır |
| Labs/ortamları | Evet | Evet |
| Labs/servicerunners | Evet | Evet |
| Labs/virtualmachines | Evet | Hayır |
| Zamanlamaları | Evet | Evet |

## <a name="microsoftdns"></a>microsoft.dns
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dnszones | Hayır | Hayır |
| dnszones/a | Hayır | Hayır |
| dnszones/aaaa | Hayır | Hayır |
| dnszones/cname | Hayır | Hayır |
| dnszones/mx | Hayır | Hayır |
| dnszones/ptr | Hayır | Hayır |
| dnszones/srv | Hayır | Hayır |
| dnszones/txt | Hayır | Hayır |
| trafficmanagerprofiles | Hayır | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| databaseaccounts | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| etki alanları | Evet | Evet |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| etki alanları | Evet | Evet |
| konuları | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kümeleri | Evet | Evet |
| Ad alanları | Evet | Evet |

## <a name="microsoftgenomics"></a>Microsoft.Genomics
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hanainstances | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kümeleri | Evet | Evet |

> [!IMPORTANT]
> HDInsight kümeleri, yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz. Ancak, ağ kaynaklarını (örneğin, sanal ağ, NIC veya yük dengeleyici) HDInsight kümesine bağlı abonelikler arasında taşınamaz. Ayrıca, küme için bir sanal makineye ekli NIC yeni bir kaynak grubuna taşınamıyor.
>
> Bir HDInsight kümesi için yeni bir abonelik taşırken, önce diğer kaynakları (örneğin, depolama hesabı) taşıyın. Ardından, HDInsight kümesi, tek başına taşıyın.

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Evet | Evet |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| makineler | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft.HybridData
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datamanagers | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| İşleri | Evet | Evet |

## <a name="microsoftinsights"></a>Microsoft.insights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |
| actiongroups | Evet | Evet |
| activitylogalerts | Hayır | Hayır |
| alertrules | Evet | Evet |
| autoscalesettings | Evet | Evet |
| Bileşenleri | Evet | Evet |
| guestdiagnosticsettings | Hayır | Hayır |
| metricalerts | Hayır | Hayır |
| notificationgroups | Hayır | Hayır |
| notificationrules | Hayır | Hayır |
| scheduledqueryrules | Evet | Evet |
| Web testleri | Evet | Evet |
| Çalışma kitapları | Evet | Evet |

> [!IMPORTANT]
> Yeni bir aboneliğe taşınmasını emin değil aşan [abonelik kotaları](../azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| iotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| checknameavailability | Evet | Evet |
| graph | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hsmpools | Hayır | Hayır |
| kasaları | Evet | Evet |

> [!IMPORTANT]
> Anahtar kasası disk şifrelemesi için kullanılan bir kaynak grubu ile aynı abonelikte veya abonelikler arasında taşınamaz.

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kümeleri | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| labaccounts | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hostingenvironments | Hayır | Hayır |
| integrationaccounts | Evet | Evet |
| integrationserviceenvironments | Hayır | Hayır |
| isolatedenvironments | Hayır | Hayır |
| İş akışları | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| commitmentplans | Evet | Evet |
| veritabanınızdaki | Evet | Hayır |
| Çalışma alanları | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| operationalizationclusters | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |
| hesapları/çalışma alanları | Hayır | Hayır |
| çalışma alanları/hesapları/projeleri | Hayır | Hayır |
| teamaccounts | Hayır | Hayır |
| teamaccounts/çalışma alanları | Hayır | Hayır |
| çalışma alanları/teamaccounts/projeleri | Hayır | Hayır |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hostingaccounts | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Çalışma alanları | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.managedıdentity
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| userassignedıdentities | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| mediaservices | Evet | Evet |
| mediaservices/liveevents | Evet | Evet |
| mediaservices/akış | Evet | Evet |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| assessmentprojects | Hayır | Hayır |
| migrateprojects | Hayır | Hayır |
| Projeleri | Hayır | Hayır |

## <a name="microsoftnetapp"></a>Microsoft.NetApp
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| netappaccounts | Hayır | Hayır |
| netappaccounts/capacitypools | Hayır | Hayır |
| netappaccounts/capacitypools/birimler | Hayır | Hayır |
| netappaccounts/capacitypools/birim/mounttargets | Hayır | Hayır |
| netappaccounts/capacitypools/birim/anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| applicationgateways | Hayır | Hayır |
| applicationgatewaywebapplicationfirewallpolicies | Hayır | Hayır |
| applicationsecuritygroup | Evet | Evet |
| azurefirewalls | Evet | Evet |
| bastionhosts | Hayır | Hayır |
| Bağlantıları | Evet | Evet |
| ddoscustompolicies | Evet | Evet |
| ddosprotectionplans | Hayır | Hayır |
| dnszones | Evet | Evet |
| expressroutecircuits | Hayır | Hayır |
| expressroutecrossconnections | Hayır | Hayır |
| expressroutegateways | Hayır | Hayır |
| expressrouteports | Hayır | Hayır |
| frontdoors | Hayır | Hayır |
| frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
| sonraki | Evet - temel SKU<br>Hayır - standart SKU | Evet - temel SKU<br>Hayır - standart SKU |
| localnetworkgateways | Evet | Evet |
| natgateways | Evet | Evet |
| networkintentpolicies | Evet | Evet |
| networkınterface'lerden bazıları | Evet | Evet |
| networkprofiles | Hayır | Hayır |
| networksecuritygroups | Evet | Evet |
| networkwatchers | Evet | Evet |
| networkwatchers/connectionmonitors | Evet | Evet |
| networkwatchers/merceklerden | Evet | Evet |
| networkwatchers/pingmeshes | Evet | Evet |
| p2svpngateways | Hayır | Hayır |
| privatednszones | Evet | Evet |
| privatednszones/virtualnetworklinks | Evet | Evet |
| privateendpoints | Hayır | Hayır |
| privatelinkservices | Hayır | Hayır |
| publicipaddresses | Evet - temel SKU<br>Hayır - standart SKU | Evet - temel SKU<br>Hayır - standart SKU |
| publicipprefixes | Evet | Evet |
| routefilters | Hayır | Hayır |
| routetables | Evet | Evet |
| securegateways | Evet | Evet |
| serviceendpointpolicies | Evet | Evet |
| trafficmanagerprofiles | Evet | Evet |
| virtualhubs | Hayır | Hayır |
| virtualnetworkgateways | Evet | Evet |
| virtualnetworks | Evet | Evet |
| virtualnetworktaps | Hayır | Hayır |
| virtualwans | Hayır | Hayır |
| vpngateways | Hayır | Hayır |
| vpnsites | Hayır | Hayır |
| webapplicationfirewallpolicies | Evet | Evet |

> [!IMPORTANT]
> Bkz: [sanal ağlar Taşıma Kılavuzu](./move-limitations/virtual-network-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Ad alanları | Evet | Evet |
| ad/notificationhubs | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Çalışma alanları | Evet | Evet |

> [!IMPORTANT]
> Yeni bir aboneliğe taşınmasını emin değil aşan [abonelik kotaları](../azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| managementconfigurations | Evet | Evet |
| çözümler | Evet | Evet |
| Görünümler | Evet | Evet |

## <a name="microsoftpeering"></a>Microsoft.Peering
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| eşlemeler | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Panolar | Evet | Evet |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| rootresources | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| workspacecollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kapasiteleri | Evet | Evet |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kasaları | Evet | Evet |

> [!IMPORTANT]
> Bkz: [kurtarma Hizmetleri Taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftrelay"></a>Microsoft.Relay
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Ad alanları | Evet | Evet |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Hayır |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Akışlar | Evet | Evet |
| eyleminde | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| searchservices | Evet | Evet |

> [!IMPORTANT]
> Tek bir işlemde farklı bölgelerdeki birden çok arama kaynaklar taşınamıyor. Bunun yerine, bunları ayrı işlemlerde taşıyın.

## <a name="microsoftsecurity"></a>Microsoft.Security
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| iotsecuritysolutions | Evet | Evet |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Ağ geçitleri | Hayır | Hayır |
| düğüm | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Ad alanları | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Hayır | Hayır |
| Kümeleri | Evet | Evet |
| containergroups | Hayır | Hayır |
| containergroupsets | Hayır | Hayır |
| edgeclusters | Hayır | Hayır |
| Ağlar | Hayır | Hayır |
| secretstores | Hayır | Hayır |
| volumes | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Evet |
| containergroups | Hayır | Hayır |
| Ağ geçitleri | Evet | Evet |
| Ağlar | Evet | Evet |
| Gizli dizileri | Evet | Evet |
| volumes | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| signalr | Evet | Evet |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Hayır | Hayır |

> [!IMPORTANT]
> Bkz: [kurtarma Hizmetleri Taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| appliancedefinitions | Hayır | Hayır |
| cihazları | Hayır | Hayır |
| applicationdefinitions | Hayır | Hayır |
| uygulamalar | Hayır | Hayır |
| jitrequests | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| instancepools | Evet | Evet |
| managedinstances | Evet | Evet |
| managedinstances/veritabanları | Evet | Evet |
| Sunucuları | Evet | Evet |
| sunucuları/veritabanları | Evet | Evet |
| sunucuları/elasticpools | Evet | Evet |
| virtualclusters | Evet | Evet |

> [!IMPORTANT]
> Bir veritabanı ve sunucu, aynı kaynak grubunda olmalıdır. Bir SQL server taşıdığınızda, tüm veritabanlarını da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Evet | Evet |
| sqlvirtualmachines | Evet | Evet |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dwvm | Hayır | Hayır |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Evet | Evet |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Önbellekler | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Evet | Evet |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Yöneticileri | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| streamingjobs | Evet | Evet |

> [!IMPORTANT]
> Stream Analytics işleri çalıştırırken buna taşınamaz durumu.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Ortamlar | Hayır | Hayır |
| ortamları/eventsources | Hayır | Hayır |
| örnekler | Hayır | Hayır |
| Örnek/ortamları | Hayır | Hayır |
| Örnek/ortam/eventsources | Hayır | Hayır |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| providerregistrations | Hayır | Hayır |
| Kaynakları | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Ortamlar | Evet | Evet |
| ortamları/eventsources | Evet | Evet |
| ortamları/referencedatasets | Evet | Evet |

## <a name="microsofttoken"></a>Microsoft.Token
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| depolar | Hayır | Hayır |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| imagetemplates | Hayır | Hayır |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| account | Evet | Evet |
| hesabı/uzantısı | Evet | Evet |
| hesabı/proje | Evet | Evet |

> [!IMPORTANT]
> Azure DevOps için aboneliğinizi değiştirmek için bkz: [faturalandırma için kullanılan Azure aboneliğini değiştirmeniz](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dedicatedcloudnodes | Evet | Evet |
| dedicatedcloudservices | Evet | Evet |
| virtualmachines | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft.Web
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Sertifikaları | Hayır | Evet |
| connectiongateways | Evet | Evet |
| Bağlantıları | Evet | Evet |
| customapis | Evet | Evet |
| hostingenvironments | Hayır | Hayır |
| serverfarms | Evet | Evet |
| Siteleri | Evet | Evet |
| Site/premieraddons | Evet | Evet |
| Site/Yuvalar | Evet | Evet |

> [!IMPORTANT]
> Bkz: [App Service Taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| deviceservices | Hayır | Hayır |

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft.WindowsVirtualDesktop
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| applicationgroups | Hayır | Hayır |
| hostpools | Hayır | Hayır |
| Çalışma alanları | Hayır | Hayır |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetleri, taşıma işlemi şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

Virgülle ayrılmış değerler dosyası aynı verileri almak için indirme [taşıma desteği resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).
