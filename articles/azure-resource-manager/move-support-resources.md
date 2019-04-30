---
title: İşlem desteği Azure kaynak türüne göre Taşı
description: Yeni kaynak grubuna veya aboneliğe taşınabilir Azure kaynak türlerini listeler.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 03/22/2019
ms.author: tomfitz
ms.openlocfilehash: d44b1bf778c7ec9551e2fd30f67083f8dded22d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729299"
---
# <a name="move-operation-support-for-resources"></a>İşlem desteği kaynakları Taşı
Bu makalede, bir Azure kaynak türü taşıma işlemini destekleyip desteklemediğini listelenmektedir. Bir kaynak türü taşıma işlemi desteklemesine rağmen kaynak taşınmasını engellemek koşulları olabilir. Taşıma işlemlerini etkileyen koşullar hakkında daha fazla ayrıntı için bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

Virgülle ayrılmış değerler dosyası aynı verileri almak için indirme [taşıma desteği resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| domainservices | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Kiracılar | Hayır | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| sunucu | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| hizmet | Evet | Evet |

## <a name="microsoftappservice"></a>Microsoft.AppService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| apiapps | Hayır | Hayır |
| appidentities | Hayır | Hayır |
| Ağ geçitleri | Hayır | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| policyassignments | Hayır | Hayır |

## <a name="microsoftautomation"></a>Gibi Microsoft.Automation
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| automationaccounts | Evet | Evet |
| automationaccounts/yapılandırmalar | Evet | Evet |
| automationaccounts/runbook'ları | Evet | Evet |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| b2cdirectories | Evet | Evet |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| kayıtları | Evet | Evet |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| backupvault | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| batchaccounts | Evet | Evet |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Kümeleri | Hayır | Hayır |
| fileservers | Hayır | Hayır |
| işler | Hayır | Hayır |
| çalışma alanı | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| mapapis | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| biztalk | Evet | Evet |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| blockchainmembers | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| blueprintassignments | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Redis | Evet | Evet |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Profilleri | Evet | Evet |
| profilleri/uç noktaları | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| certificateorders | Evet | Evet |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| domainnames | Evet | Hayır |
| virtualmachines | Evet | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Hayır | Hayır |
| ReservedIP | Hayır | Hayır |
| virtualnetworks | Hayır | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| storageaccounts | Evet | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| availabilitysets | Evet | Evet |
| diskler | Evet | Evet |
| galeriler | Hayır | Hayır |
| galeriler/görüntüleri | Hayır | Hayır |
| galeriler/resimler/sürümleri | Hayır | Hayır |
| images | Evet | Evet |
| proximityplacementgroups | Hayır | Hayır |
| restorepointcollections | Hayır | Hayır |
| sharedvmimages | Hayır | Hayır |
| sharedvmimages/sürümleri | Hayır | Hayır |
| anlık görüntüler | Evet | Evet |
| virtualmachines | Evet | Evet |
| virtualmachines ve uzantıları | Evet | Evet |
| virtualmachinescalesets | Evet | Evet |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| containergroups | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| containergroups | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| kayıt defterleri | Evet | Evet |
| kayıt defterleri/buildtasks | Evet | Evet |
| kayıt defterleri/çoğaltmalar | Hayır | Hayır |
| kayıt defterleri/görevleri | Evet | Evet |
| kayıt defterleri/Web kancaları | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| containerservices | Hayır | Hayır |
| managedclusters | Hayır | Hayır |
| openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Evet |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| bağlayıcılar | Evet | Evet |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Hub'ları | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| işler | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| çalışma alanı | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| katalogları | Evet | Evet |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| connectionmanagers | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| paketler | Hayır | Hayır |
| planlar | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| datafactories | Evet | Evet |
| fabrikaları | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| services | Hayır | Hayır |
| Hizmetleri/projeleri | Hayır | Hayır |
| yuvaları | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| sunucu | Evet | Evet |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| sunucu | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| servergroups | Hayır | Hayır |
| sunucu | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| artifactsources | Hayır | Hayır |
| Piyasaya çıkarma | Hayır | Hayır |
| servicetopologies | Hayır | Hayır |
| servicetopologies/hizmetler | Hayır | Hayır |
| servicetopologies/services/serviceunits | Hayır | Hayır |
| adımlar | Hayır | Hayır |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| elasticpools | Hayır | Hayır |
| elasticpools/iothubtenants | Hayır | Hayır |
| iothubs | Evet | Evet |
| provisioningservices | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Denetleyicileri | Hayır | Hayır |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| labcenters | Hayır | Hayır |
| Laboratuvarları | Evet | Hayır |
| Labs/servicerunners | Evet | Evet |
| Labs/virtualmachines | Evet | Hayır |
| Zamanlamaları | Hayır | Hayır |

## <a name="microsoftdns"></a>microsoft.dns
| Kaynak türü | Kaynak grubu | Abonelik |
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
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| databaseaccounts | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| etki alanları | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| etki alanları | Evet | Evet |
| konuları | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Kümeleri | Evet | Evet |
| ad alanları | Evet | Evet |

## <a name="microsoftgenomics"></a>Microsoft.Genomics
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| hanainstances | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Kümeleri | Evet | Evet |

## <a name="microsofthybriddata"></a>Microsoft.HybridData
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| datamanagers | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| işler | Evet | Evet |

## <a name="microsoftinsights"></a>Microsoft.insights
| Kaynak türü | Kaynak grubu | Abonelik |
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
| scheduledqueryrules | Hayır | Hayır |
| Web testleri | Evet | Evet |
| çalışma kitapları | Evet | Evet |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| iotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| checknameavailability | Evet | Evet |
| Graf | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| hsmpools | Hayır | Hayır |
| kasaları | Evet | Evet |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Kümeleri | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| labaccounts | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| hostingenvironments | Hayır | Hayır |
| integrationaccounts | Evet | Evet |
| integrationserviceenvironments | Hayır | Hayır |
| isolatedenvironments | Hayır | Hayır |
| İş akışları | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| commitmentplans | Evet | Evet |
| veritabanınızdaki | Evet | Hayır |
| çalışma alanı | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| operationalizationclusters | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |
| hesapları/çalışma alanları | Evet | Evet |
| çalışma alanları/hesapları/projeleri | Evet | Evet |
| teamaccounts | Evet | Evet |
| teamaccounts/çalışma alanları | Evet | Evet |
| çalışma alanları/teamaccounts/projeleri | Evet | Evet |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| hostingaccounts | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| çalışma alanı | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.managedıdentity
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| userassignedıdentities | Evet | Evet |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| mediaservices | Evet | Evet |
| mediaservices/liveevents | Evet | Evet |
| mediaservices/akış | Evet | Evet |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| assessmentprojects | Hayır | Hayır |
| migrateprojects | Hayır | Hayır |
| Projeleri | Hayır | Hayır |

## <a name="microsoftnetapp"></a>Microsoft.NetApp
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| netappaccounts | Hayır | Hayır |
| netappaccounts/capacitypools | Hayır | Hayır |
| netappaccounts/capacitypools/birimler | Hayır | Hayır |
| netappaccounts/capacitypools/birim/mounttargets | Hayır | Hayır |
| netappaccounts/capacitypools/birim/anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| applicationgateways | Hayır | Hayır |
| applicationsecuritygroup | Evet | Evet |
| azurefirewalls | Evet | Evet |
| bastionhosts | Hayır | Hayır |
| bağlantılar | Evet | Evet |
| ddoscustompolicies | Evet | Evet |
| ddosprotectionplans | Hayır | Hayır |
| dnszones | Evet | Evet |
| expressroutecircuits | Hayır | Hayır |
| expressroutecrossconnections | Hayır | Hayır |
| expressroutegateways | Hayır | Hayır |
| expressrouteports | Hayır | Hayır |
| frontdoors | Evet | Evet |
| frontdoorwebapplicationfirewallpolicies | Evet | Evet |
| interfaceendpoints | Hayır | Hayır |
| sonraki | Evet | Evet |
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
| privatelinkservices | Hayır | Hayır |
| publicipaddresses | Evet | Evet |
| publicipprefixes | Evet | Evet |
| routefilters | Hayır | Hayır |
| routetables | Evet | Evet |
| securegateways | Hayır | Hayır |
| serviceendpointpolicies | Evet | Evet |
| trafficmanagerprofiles | Evet | Evet |
| virtualhubs | Hayır | Hayır |
| virtualnetworkgateways | Evet | Evet |
| virtualnetworks | Evet | Evet |
| virtualnetworktaps | Hayır | Hayır |
| virtualwans | Hayır | Hayır |
| vpngateways | Hayır | Hayır |
| vpnsites | Evet | Evet |
| webapplicationfirewallpolicies | Evet | Evet |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| ad alanları | Evet | Evet |
| ad/notificationhubs | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| çalışma alanı | Evet | Evet |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| managementconfigurations | Evet | Evet |
| çözümler | Evet | Evet |
| görüntüleme | Evet | Evet |

## <a name="microsoftportal"></a>Microsoft.Portal
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Panolar | Evet | Evet |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| rootresources | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| workspacecollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Kapasiteleri | Evet | Evet |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| kasaları | Evet | Evet |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| ad alanları | Evet | Evet |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Hayır |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| akışlar | Evet | Evet |
| eyleminde | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| searchservices | Evet | Evet |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Ağ geçitleri | Hayır | Hayır |
| düğüm | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| ad alanları | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| uygulamalar | Hayır | Hayır |
| Kümeleri | Evet | Evet |
| containergroups | Hayır | Hayır |
| containergroupsets | Hayır | Hayır |
| edgeclusters | Hayır | Hayır |
| ağlar | Hayır | Hayır |
| secretstores | Hayır | Hayır |
| volumes | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Evet |
| containergroups | Hayır | Hayır |
| Ağ geçitleri | Evet | Evet |
| ağlar | Evet | Evet |
| gizli dizi | Evet | Evet |
| volumes | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| signalr | Evet | Evet |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| appliancedefinitions | Hayır | Hayır |
| cihazları | Hayır | Hayır |
| applicationdefinitions | Hayır | Hayır |
| uygulamalar | Hayır | Hayır |
| jitrequests | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| managedinstances | Evet | Evet |
| managedinstances/veritabanları | Evet | Evet |
| sunucu | Evet | Evet |
| sunucuları/veritabanları | Evet | Evet |
| sunucuları/elasticpools | Evet | Evet |
| sunucuları/jobaccounts | Hayır | Hayır |
| sunucuları/jobagents | Hayır | Hayır |
| virtualclusters | Evet | Evet |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Evet | Evet |
| sqlvirtualmachines | Evet | Evet |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| dwvm | Hayır | Hayır |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| storageaccounts | Evet | Evet |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| storagesyncservices | Evet | Evet |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| storagesyncservices | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Yöneticileri | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| streamingjobs | Evet | Evet |

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Ortamlar | Hayır | Hayır |
| ortamları/eventsources | Hayır | Hayır |
| örnekler | Hayır | Hayır |
| Örnek/ortamları | Hayır | Hayır |
| Örnek/ortam/eventsources | Hayır | Hayır |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| providerregistrations | Hayır | Hayır |
| kaynaklar | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| Ortamlar | Evet | Evet |
| ortamları/eventsources | Evet | Evet |
| ortamları/referencedatasets | Evet | Evet |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| imagetemplates | Hayır | Hayır |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| account | Evet | Evet |
| hesabı/uzantısı | Evet | Evet |
| hesabı/proje | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft.Web
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| sertifika | Hayır | Evet |
| connectiongateways | Evet | Evet |
| bağlantılar | Evet | Evet |
| customapis | Evet | Evet |
| hostingenvironments | Hayır | Hayır |
| serverfarms | Evet | Evet |
| siteler | Evet | Evet |
| Site/premieraddons | Evet | Evet |
| Site/Yuvalar | Evet | Evet |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | ----------- | ---------- |
| deviceservices | Evet | Evet |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetleri, taşıma işlemi şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
