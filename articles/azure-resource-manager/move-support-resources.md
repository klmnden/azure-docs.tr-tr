---
title: İşlem desteği Azure kaynak türüne göre Taşı | Microsoft Docs
description: Yeni kaynak grubuna veya aboneliğe taşınabilir Azure kaynak türlerini listeler.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/28/2018
ms.author: tomfitz
ms.openlocfilehash: 6f1869b83f46f97d0c54eb874a8879521a43b1e2
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53387072"
---
# <a name="move-operation-support-for-resources"></a>İşlem desteği kaynakları Taşı

Bu makalede, bir Azure kaynak türü taşıma işlemini destekleyip desteklemediğini listelenmektedir. Bir kaynak türü taşıma işlemi desteklemesine rağmen kaynak taşınmasını engellemek koşulları olabilir. Taşıma işlemlerini etkileyen koşullar hakkında daha fazla ayrıntı için bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

## <a name="find-resource-provider-and-resource-type"></a>Kaynak sağlayıcısı ve kaynak türü bulma

Bir kaynak taşınıp taşınamayacağını belirlemek için kaynak türü ve kaynak sağlayıcısı bulmanız gerekir.

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
```

Azure CLI için şunu kullanın:

```azurecli-interactive
az resource list -g demogroup --query '[].{name:name, resourceType:type}' --output table
```

Kaynak türü şu biçimde döndürülür `<resource-provider>/<resource-type-name>`. Bunu, değer `Microsoft.OperationalInsights/workspaces` kaynak sağlayıcısı olan **Microsoft.OperationalInsights** ve kaynak türünün adı **çalışma alanları**.

Kaynak sağlayıcısı hem de kaynak türünü bulduktan sonra kaynak türü taşıma işlemini destekleyip desteklemediğini belirlemek için bu makaledeki tabloları kullanın.

## <a name="microsoftaad"></a>Microsoft.AAD

| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | --------------- | ----------- |
| domainservices | Hayır | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| sunucu | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | --------------- | ----------- |
| hizmet | Evet | Evet |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | --------------- | ----------- |
| policyassignments | Hayır | Hayır |

## <a name="microsoftautomation"></a>Gibi Microsoft.Automation
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| automationaccounts | Evet | Evet |
| automationaccounts/yapılandırmalar | Evet | Evet |
| automationaccounts/runbook'ları | Evet | Evet |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| b2cdirectories | Evet | Evet |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| kayıtları | Evet | Evet |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| backupvault | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| batchaccounts | Evet | Evet |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| mapapis | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| biztalk | Evet | Evet |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| blueprintassignments | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Redis | Evet | Evet |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Profilleri | Evet | Evet |
| profilleri/uç noktaları | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| certificateorders | Evet | Evet |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| domainnames | Evet | Hayır |
| virtualmachines | Evet | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| networksecuritygroups | Hayır | Hayır |
| ReservedIP | Hayır | Hayır |
| virtualnetworks | Hayır | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| storageaccounts | Evet | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| availabilitysets | Evet | Evet |
| diskler | Evet | Evet |
| galeriler | Hayır | Hayır |
| galeriler/görüntüleri | Hayır | Hayır |
| galeriler/resimler/sürümleri | Hayır | Hayır |
| images | Evet | Evet |
| restorepointcollections | Hayır | Hayır |
| sharedvmimages | Hayır | Hayır |
| sharedvmimages/sürümleri | Hayır | Hayır |
| anlık görüntüler | Evet | Evet |
| virtualmachines | Evet | Evet |
| virtualmachines ve uzantıları | Evet | Evet |
| virtualmachinescalesets | Evet | Evet |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| containergroups | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| containergroups | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| kayıt defterleri | Evet | Evet |
| kayıt defterleri/buildtasks | Evet | Evet |
| kayıt defterleri/çoğaltmalar | Hayır | Hayır |
| kayıt defterleri/görevleri | Evet | Evet |
| kayıt defterleri/Web kancaları | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| containerservices | Hayır | Hayır |
| managedclusters | Hayır | Hayır |
| openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| uygulamalar | Evet | Evet |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| bağlayıcılar | Evet | Evet |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Hub'ları | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| işler | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| databoxedgedevices | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| çalışma alanı | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| katalogları | Evet | Evet |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| datafactories | Evet | Evet |
| fabrikaları | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| services | Hayır | Hayır |
| Hizmetleri/projeleri | Hayır | Hayır |
| yuvaları | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| sunucu | Hayır | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| sunucu | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| servergroups | Hayır | Hayır |
| sunucu | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| artifactsources | Hayır | Hayır |
| Piyasaya çıkarma | Hayır | Hayır |
| servicetopologies | Hayır | Hayır |
| servicetopologies/hizmetler | Hayır | Hayır |
| servicetopologies/services/serviceunits | Hayır | Hayır |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| iothubs | Evet | Evet |
| provisioningservices | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| labcenters | Hayır | Hayır |
| Laboratuvarları | Evet | Hayır |
| Labs/servicerunners | Evet | Evet |
| Labs/virtualmachines | Evet | Hayır |
| Zamanlamaları | Hayır | Hayır |

## <a name="microsoftdns"></a>Microsoft.DNS
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
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
| ------------- | -------------- | ------------ |
| databaseaccounts | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| etki alanları | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| konuları | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Kümeleri | Evet | Evet |
| Ad alanları | Evet | Evet |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| hanainstances | Evet | Evet |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| dedicatedhsms | Hayır | Hayır |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Kümeleri | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| işler | Evet | Evet |

## <a name="microsoftinsights"></a>Microsoft.insights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| actiongroups | Evet | Evet |
| activitylogalerts | Hayır | Hayır |
| alertrules | Evet | Evet |
| autoscalesettings | Evet | Evet |
| Bileşenleri | Evet | Evet |
| metricalerts | Hayır | Hayır |
| scheduledqueryrules | Evet | Evet |
| Web testleri | Evet | Evet |
| Çalışma kitapları | Evet | Evet |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| iotapps | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| kasaları | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| labaccounts | Evet | Evet |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| integrationaccounts | Evet | Evet |
| İş akışları | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| commitmentplans | Evet | Evet |
| veritabanınızdaki | Evet | Hayır |
| çalışma alanı | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| operationalizationclusters | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |
| hesapları/çalışma alanları | Evet | Evet |
| çalışma alanları/hesapları/projeleri | Evet | Evet |
| teamaccounts | Evet | Evet |
| teamaccounts/çalışma alanları | Evet | Evet |
| çalışma alanları/teamaccounts/projeleri | Evet | Evet |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| çalışma alanı | Evet | Evet |

## <a name="microsoftmanagedidentity"></a>Microsoft.managedıdentity
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| userassignedıdentities | Evet | Evet |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| accounts | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| mediaservices | Evet | Evet |
| mediaservices/liveevents | Evet | Evet |
| mediaservices/akış | Evet | Evet |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Projeleri | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| applicationgateways | Hayır | Hayır |
| applicationsecuritygroup | Evet | Evet |
| azurefirewalls | Evet | Evet |
| bağlantılar | Evet | Evet |
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
| networkintentpolicies | Evet | Evet |
| networkınterface'lerden bazıları | Evet | Evet |
| networkprofiles | Hayır | Hayır |
| networksecuritygroups | Evet | Evet |
| networkwatchers | Evet | Evet |
| networkwatchers/connectionmonitors | Evet | Evet |
| networkwatchers/merceklerden | Evet | Evet |
| networkwatchers/pingmeshes | Evet | Evet |
| publicipaddresses | Evet | Evet |
| publicipprefixes | Evet | Evet |
| routefilters | Hayır | Hayır |
| routetables | Evet | Evet |
| serviceendpointpolicies | Evet | Evet |
| trafficmanagerprofiles | Evet | Evet |
| virtualhubs | Evet | Evet |
| virtualnetworkgateways | Evet | Evet |
| virtualnetworks | Evet | Evet |
| virtualnetworktaps | Hayır | Hayır |
| virtualwans | Evet | Evet |
| vpngateways | Evet | Evet |
| vpnsites | Evet | Evet |
| webapplicationfirewallpolicies | Evet | Evet |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Ad alanları | Evet | Evet |
| ad/notificationhubs | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| çalışma alanı | Evet | Evet |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| managementconfigurations | Evet | Evet |
| çözümler | Evet | Evet |
| görüntüleme | Evet | Evet |

## <a name="microsoftportal"></a>Microsoft.Portal
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Panolar | Evet | Evet |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| workspacecollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Kapasiteleri | Evet | Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| kasaları | Evet | Evet |

## <a name="microsoftrelay"></a>Sayısı
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Ad alanları | Evet | Evet |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| uygulamalar | Evet | Hayır |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| akışlar | Evet | Evet |
| eyleminde | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| searchservices | Evet | Evet |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Ad alanları | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Kümeleri | Evet | Evet |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| uygulamalar | Evet | Evet |
| ağlar | Evet | Evet |
| volumes | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| signalr | Evet | Evet |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| siterecoveryvault | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| appliancedefinitions | Hayır | Hayır |
| cihazları | Hayır | Hayır |
| applicationdefinitions | Hayır | Hayır |
| uygulamalar | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| managedinstances | Evet | Evet |
| managedinstances/veritabanları | Evet | Evet |
| sunucu | Evet | Evet |
| sunucuları/veritabanları | Evet | Evet |
| sunucuları/elasticpools | Evet | Evet |
| virtualclusters | Evet | Evet |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| storageaccounts | Evet | Evet |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| storagesyncservices | Evet | Evet |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Yöneticileri | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| streamingjobs | Evet | Evet |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| Ortamlar | Evet | Evet |
| ortamları/eventsources | Evet | Evet |
| ortamları/referencedatasets | Evet | Evet |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| account | Evet | Evet |
| hesabı/uzantısı | Evet | Evet |
| hesabı/proje | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft.Web
| Kaynak türü | Kaynak grubu | Abonelik |
| ------------- | -------------- | ------------ |
| sertifika | Hayır | Evet |
| classicmobileservices | Hayır | Hayır |
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
| ------------- | -------------- | ------------ |
| deviceservices | Evet | Evet |


## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetleri, taşıma işlemi şu anda desteklenmiyor. Bu kaynak sağlayıcıları şunlardır:

* 84codes. CloudAMQP
* AppDynamics.APM
* Aspera.Transfers
* Auth0.cloud
* Citrix.Cloud
* Citrix.Services
* CloudSimple.PrivateCloudIaaS
* Cloudyn.Analytics
* Conexlink.MyCloudIT
* Crypteron.DataSecurity
* Dynatrace.DynatraceSaaS
* Dynatrace.Ruxit
* LiveArena.Broadcast
* Lombiq.DotNest
* Mailjet.Email
* Myget.PackageManagement
* NewRelic.APM
* nuubit.nextgencdn
* Paraleap.CloudMonix
* Pokitdok.Platform
* RavenHq.Db
* Raygun.CrashReporting
* RevAPM.MobileCDN
* Sendgrid.Email
* Sparkpost.Basic
* stackify.retrace
* SuccessBricks.ClearDB
* TrendMicro.DeepSecurity
* U2uconsult.TheIdentityHub


## <a name="next-steps"></a>Sonraki adımlar

* Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
