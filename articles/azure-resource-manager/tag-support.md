---
title: Kaynaklar için Azure Resource Manager etiketi desteği
description: Etiketlerin hangi Azure kaynak türlerini destekleyen gösterir. Ayrıntılar için tüm Azure hizmetleri sağlar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 05/10/2019
ms.author: tomfitz
ms.openlocfilehash: 7ef37323fb8150e3a6b52800bfafa2585ae328c2
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523230"
---
# <a name="tag-support-for-azure-resources"></a>Azure kaynakları için etiketi desteği
Bu makalede, bir kaynak türünü destekleyip desteklemediğini açıklar [etiketleri](resource-group-using-tags.md). Etiketli sütun **etiketlerini destekler** kaynak türü etiket için bir özellik olup olmadığını gösterir. Etiketli sütun **Maliyet raporu etiketinde** bu kaynak türü Maliyet raporu etiket başarılı olup olmadığını gösterir.

Virgülle ayrılmış değerler dosyası aynı verileri almak için indirme [etiketi support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| DomainServices | Evet | Evet |
| DomainServices/oucontainer | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftaddons"></a>Microsoft.Addons
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| supportProviders | Hayır |  Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| aadsupportcases | Hayır |  Hayır |
| addsservices | Hayır |  Hayır |
| aracılar | Hayır |  Hayır |
| anonymousapiusers | Hayır |  Hayır |
| yapılandırma | Hayır |  Hayır |
| günlükler | Hayır |  Hayır |
| raporlar | Hayır |  Hayır |
| services | Hayır |  Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Yapılandırmaları | Hayır |  Hayır |
| generateRecommendations | Hayır |  Hayır |
| öneriler | Hayır |  Hayır |
| gizlemeleri | Hayır |  Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| actionRules | Hayır |  Hayır |
| uyarı | Hayır |  Hayır |
| alertsList | Hayır |  Hayır |
| alertsSummary | Hayır |  Hayır |
| alertsSummaryList | Hayır |  Hayır |
| smartDetectorAlertRules | Hayır |  Hayır |
| smartDetectorRuntimeEnvironments | Hayır |  Hayır |
| smartGroups | Hayır |  Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| sunucu | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| reportFeedback | Hayır |  Hayır |
| hizmet | Evet | Evet |
| validateServiceName | Hayır |  Hayır |

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| attestationProviders | Hayır |  Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| classicAdministrators | Hayır |  Hayır |
| denyAssignments | Hayır |  Hayır |
| elevateAccess | Hayır |  Hayır |
| Kilitler | Hayır |  Hayır |
| izinler | Hayır |  Hayır |
| policyAssignments | Hayır |  Hayır |
| policyDefinitions | Hayır |  Hayır |
| policySetDefinitions | Hayır |  Hayır |
| providerOperations | Hayır |  Hayır |
| Rol | Hayır |  Hayır |
| roleDefinitions | Hayır |  Hayır |

## <a name="microsoftautomation"></a>Gibi Microsoft.Automation
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| AutomationAccounts | Evet | Evet |
| automationAccounts/yapılandırmalar | Evet | Evet |
| automationAccounts/işleri | Hayır |  Hayır |
| automationAccounts/runbook'ları | Evet | Evet |
| automationAccounts/softwareUpdateConfigurations | Hayır | Hayır |
| automationAccounts/Web kancaları | Hayır |  Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Ortamlar | Hayır |  Hayır |
| ortamları/hesapları | Hayır |  Hayır |
| hesapları/ortam/ad | Hayır |  Hayır |
| ortamları/hesapları/ad/yapılandırmalar | Hayır |  Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| b2cDirectories | Evet | Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| kayıtları | Evet | Evet |
| kayıtları/customerSubscriptions | Hayır |  Hayır |
| kayıtları/ürünleri | Hayır |  Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| batchAccounts | Evet | Evet |

## <a name="microsoftbilling"></a>Microsoft.Billing
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| billingAccounts | Hayır |  Hayır |
| billingAccounts/billingProfiles | Hayır |  Hayır |
| billingProfiles/billingAccounts/billingSubscriptions | Hayır |  Hayır |
| billingProfiles/billingAccounts/faturalar | Hayır |  Hayır |
| billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır |  Hayır |
| billingProfiles/billingAccounts/operationStatus | Hayır |  Hayır |
| billingProfiles/billingAccounts/paymentMethods | Hayır |  Hayır |
| billingAccounts/billingProfiles/ilkeler | Hayır |  Hayır |
| billingAccounts/billingProfiles/fiyat listesi | Hayır |  Hayır |
| billingProfiles/billingAccounts/ürünleri | Hayır |  Hayır |
| billingProfiles/billingAccounts/işlem | Hayır |  Hayır |
| billingAccounts/billingSubscriptions | Hayır |  Hayır |
| billingAccounts/Departmanlar | Hayır |  Hayır |
| billingAccounts/eligibleOffers | Hayır |  Hayır |
| billingAccounts/enrollmentAccounts | Hayır |  Hayır |
| billingAccounts/faturalar | Hayır |  Hayır |
| billingAccounts/invoiceSections | Hayır |  Hayır |
| invoiceSections/billingAccounts/billingSubscriptions | Hayır |  Hayır |
| billingAccounts/invoiceSections/billingSubscriptions/Aktarım | Hayır |  Hayır |
| invoiceSections/billingAccounts/importRequests | Hayır |  Hayır |
| billingAccounts/invoiceSections/initiateImportRequest | Hayır |  Hayır |
| invoiceSections/billingAccounts/initiateTransfer | Hayır |  Hayır |
| invoiceSections/billingAccounts/operationStatus | Hayır |  Hayır |
| invoiceSections/billingAccounts/ürünleri | Hayır |  Hayır |
| billingAccounts/invoiceSections/Aktarım | Hayır |  Hayır |
| billingAccounts/ürünleri | Hayır |  Hayır |
| billingAccounts/projeler | Hayır |  Hayır |
| billingAccounts/proje/billingSubscriptions | Hayır |  Hayır |
| billingAccounts/projects/importRequests | Hayır |  Hayır |
| billingAccounts/projects/initiateImportRequest | Hayır |  Hayır |
| billingAccounts/proje/operationStatus | Hayır |  Hayır |
| billingAccounts/proje/ürünleri | Hayır |  Hayır |
| billingAccounts/işlem | Hayır |  Hayır |
| billingPeriods | Hayır |  Hayır |
| BillingPermissions | Hayır |  Hayır |
| billingProperty | Hayır |  Hayır |
| BillingRoleAssignments | Hayır |  Hayır |
| BillingRoleDefinitions | Hayır |  Hayır |
| CreateBillingRoleAssignment | Hayır |  Hayır |
| Departmanlar | Hayır |  Hayır |
| enrollmentAccounts | Hayır |  Hayır |
| importRequests | Hayır |  Hayır |
| importRequests/acceptImportRequest | Hayır |  Hayır |
| importRequests/declineImportRequest | Hayır |  Hayır |
| Faturalar | Hayır |  Hayır |
| Aktarımları | Hayır |  Hayır |
| aktarımları/acceptTransfer | Hayır |  Hayır |
| aktarımları/declineTransfer | Hayır |  Hayır |
| aktarımları/operationStatus | Hayır |  Hayır |
| usagePlans | Hayır |  Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| mapApis | Evet | Evet |
| updateCommunicationPreference | Hayır |  Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| BizTalk | Evet | Evet |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| blueprintAssignments | Hayır |  Hayır |
| blueprintAssignments/assignmentOperations | Hayır |  Hayır |
| blueprintAssignments/işlemleri | Hayır |  Hayır |
| şemaları | Hayır |  Hayır |
| Blueprint/yapıları | Hayır |  Hayır |
| Blueprint/sürümleri | Hayır |  Hayır |
| Blueprint/sürümleri/yapıtları | Hayır |  Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| botServices | Evet | Evet |
| botServices kanallara | Hayır |  Hayır |
| botServices/bağlantıları | Hayır |  Hayır |

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Redis | Evet | Evet |
| RedisConfigDefinition | Hayır |  Hayır |

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| appliedReservations | Hayır |  Hayır |
| calculatePrice | Hayır |  Hayır |
| katalogları | Hayır |  Hayır |
| commercialReservationOrders | Hayır |  Hayır |
| reservationOrders | Hayır |  Hayır |
| reservationOrders/calculateRefund | Hayır |  Hayır |
| reservationOrders/merge | Hayır |  Hayır |
| reservationOrders/ayırmalar | Hayır |  Hayır |
| rezervasyonlar/reservationOrders/düzeltme | Hayır |  Hayır |
| reservationOrders/return'e | Hayır |  Hayır |
| reservationOrders/Böl | Hayır |  Hayır |
| reservationOrders/değiştirme | Hayır |  Hayır |
| rezervasyonlar | Hayır |  Hayır |
| kaynaklar | Hayır |  Hayır |
| validateReservationOrder | Hayır |  Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| edgenodes | Hayır |  Hayır |
| Profilleri | Evet | Evet |
| profilleri/uç noktaları | Evet | Evet |
| uç noktalar/profilleri/customdomains | Hayır |  Hayır |
| uç noktalar/profilleri/kaynakları | Hayır |  Hayır |
| validateProbe | Hayır |  Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| certificateOrders | Evet | Evet |
| certificateOrders/sertifikalar | Hayır |  Hayır |
| validateCertificateRegistrationInformation | Hayır |  Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Özellikleri | Hayır |  Hayır |
| domainNames | Hayır |  Hayır |
| domainNames/özellikleri | Hayır |  Hayır |
| domainNames/internalLoadBalancers | Hayır |  Hayır |
| domainNames/serviceCertificates | Hayır |  Hayır |
| domainNames/Yuvalar | Hayır |  Hayır |
| domainNames/yuvaları/roller | Hayır |  Hayır |
| moveSubscriptionResources | Hayır |  Hayır |
| operatingSystemFamilies | Hayır |  Hayır |
| operatingSystems | Hayır |  Hayır |
| quotas | Hayır |  Hayır |
| resourcetypes: | Hayır |  Hayır |
| validateSubscriptionMoveAvailability | Hayır |  Hayır |
| virtualMachines | Hayır |  Hayır |
| virtualMachines/diagnosticSettings | Hayır |  Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| classicInfrastructureResources | Hayır |  Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Özellikleri | Hayır |  Hayır |
| expressRouteCrossConnections | Hayır |  Hayır |
| expressRouteCrossConnections/eşlemeleri | Hayır |  Hayır |
| gatewaySupportedDevices | Hayır |  Hayır |
| networkSecurityGroups | Hayır |  Hayır |
| quotas | Hayır |  Hayır |
| ReservedIP | Hayır |  Hayır |
| virtualNetworks | Hayır |  Hayır |
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Hayır |  Hayır |
| virtualNetworks/virtualNetworkPeerings | Hayır |  Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Özellikleri | Hayır |  Hayır |
| diskler | Hayır |  Hayır |
| görüntüler | Hayır |  Hayır |
| osImages | Hayır |  Hayır |
| osPlatformImages | Hayır |  Hayır |
| publicImages | Hayır |  Hayır |
| quotas | Hayır |  Hayır |
| storageAccounts | Hayır |  Hayır |
| storageAccounts/services | Hayır |  Hayır |
| storageAccounts/services/diagnosticSettings | Hayır |  Hayır |
| storageAccounts/Vmımages | Hayır |  Hayır |
| Vmımages | Hayır |  Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| RateCard | Hayır |  Hayır |
| UsageAggregates | Hayır |  Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| availabilitySets | Evet | Evet |
| diskler | Evet | Evet |
| görüntüler | Evet | Evet |
| restorePointCollections | Evet | Evet |
| restorePointCollections/restorePoints | Hayır |  Hayır |
| sharedVMImages | Evet | Evet |
| sharedVMImages/sürümleri | Evet | Evet |
| anlık görüntüler | Evet | Evet |
| virtualMachines | Evet | Evet |
| virtualMachines/diagnosticSettings | Hayır |  Hayır |
| virtualMachines ve uzantıları | Evet | Evet |
| virtualMachineScaleSets | Evet | Evet |
| virtualMachineScaleSets ve uzantıları | Hayır |  Hayır |
| virtualMachineScaleSets/networkınterface'lerden bazıları | Hayır |  Hayır |
| virtualMachineScaleSets/publicIPAddresses | Hayır |  Hayır |
| virtualMachineScaleSets/virtualMachines | Hayır |  Hayır |
| virtualMachineScaleSets/virtualMachines/networkınterface'lerden bazıları | Hayır |  Hayır |

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| AggregatedCost | Hayır |  Hayır |
| Bakiyeler | Hayır |  Hayır |
| Bütçeler | Hayır |  Hayır |
| Ücretler | Hayır |  Hayır |
| CostTags | Hayır |  Hayır |
| Krediler | Hayır |  Hayır |
| olaylar | Hayır |  Hayır |
| Tahminler | Hayır |  Hayır |
| çok sayıda | Hayır |  Hayır |
| Pazar | Hayır |  Hayır |
| Pricesheets | Hayır |  Hayır |
| ürünler | Hayır |  Hayır |
| ReservationDetails | Hayır |  Hayır |
| ReservationRecommendations | Hayır |  Hayır |
| ReservationSummaries | Hayır |  Hayır |
| ReservationTransactions | Hayır |  Hayır |
| Tags | Hayır |  Hayır |
| Koşullar | Hayır |  Hayır |
| UsageDetails | Hayır |  Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| containerGroups | Evet | Evet |
| serviceAssociationLinks | Hayır |  Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| kayıt defterleri | Evet | Evet |
| kayıt defterleri/yapılar | Hayır |  Hayır |
| kayıt defterleri/yapı/iptal | Hayır |  Hayır |
| kayıt defterleri/yapı/getLogLink | Hayır |  Hayır |
| kayıt defterleri/buildTasks | Evet | Evet |
| kayıt defterleri/buildTasks/adımları | Hayır |  Hayır |
| kayıt defterleri/eventGridFilters | Hayır |  Hayır |
| kayıt defterleri/getBuildSourceUploadUrl | Hayır |  Hayır |
| kayıt defterleri/GetCredentials | Hayır |  Hayır |
| kayıt defterleri/importImage | Hayır |  Hayır |
| kayıt defterleri/queueBuild | Hayır |  Hayır |
| kayıt defterleri/regenerateCredential | Hayır |  Hayır |
| kayıt defterleri/regenerateCredentials | Hayır |  Hayır |
| kayıt defterleri/çoğaltmalar | Evet | Evet |
| kayıt defterleri/çalıştırmaları | Hayır |  Hayır |
| kayıt defterleri/çalıştırmaları/iptal | Hayır |  Hayır |
| kayıt defterleri/scheduleRun | Hayır |  Hayır |
| kayıt defterleri/görevleri | Evet | Evet |
| kayıt defterleri/updatePolicies | Hayır |  Hayır |
| kayıt defterleri/Web kancaları | Evet | Evet |
| Web kancaları/kayıt defterleri/getCallbackConfig | Hayır |  Hayır |
| Web kancaları/kayıt defterleri/ping | Hayır |  Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| containerServices | Evet | Evet |
| managedClusters | Evet | Evet |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| uygulamalar | Evet | Evet |
| updateCommunicationPreference | Hayır |  Hayır |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Uyarılar | Hayır |  Hayır |
| billingAccounts | Hayır |  Hayır |
| Bağlayıcılar | Evet | Evet |
| Bölümler | Hayır |  Hayır |
| Boyutlar | Hayır |  Hayır |
| enrollmentAccounts | Hayır |  Hayır |
| Sorgu | Hayır |  Hayır |
| Kaydolun | Hayır |  Hayır |
| Reportconfigs | Hayır |  Hayır |
| Raporlar | Hayır |  Hayır |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Hub'ları | Evet | Evet |
| hub'ları / authorizationPolicies | Hayır |  Hayır |
| hub'ları / bağlayıcılar | Hayır |  Hayır |
| bağlayıcıların/hubs/eşlemeleri | Hayır |  Hayır |
| hub'ları / etkileşimleri | Hayır |  Hayır |
| hub'ları / KPI | Hayır |  Hayır |
| hub'ları / bağlantıları | Hayır |  Hayır |
| hub'ları / profilleri | Hayır |  Hayır |
| hub'ları / rol | Hayır |  Hayır |
| hub'ları / roller | Hayır |  Hayır |
| hubs/suggestTypeSchema | Hayır |  Hayır |
| hub'ları / görünümler | Hayır |  Hayır |
| hub'ları / widgetTypes | Hayır |  Hayır |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| işler | Evet | Evet |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| DataBoxEdgeDevices | Evet | Evet |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| çalışma alanı | Evet | Hayır |
| çalışma alanları/virtualNetworkPeerings | Hayır |  Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| katalogları | Evet | Evet |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| connectionManagers | Evet | Evet |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| dataFactories | Evet | Hayır |
| dataFactories/diagnosticSettings | Hayır |  Hayır |
| dataFactorySchema | Hayır |  Hayır |
| fabrikaları | Evet | Hayır |
| fabrikaları/integrationRuntimes | Hayır |  Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |
| hesapları/dataLakeStoreAccounts | Hayır |  Hayır |
| hesapları/storageAccounts | Hayır |  Hayır |
| hesapları/storageAccounts/kapsayıcılar | Hayır |  Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |
| hesapları/eventGridFilters | Hayır |  Hayır |
| hesapları/firewallRules | Hayır |  Hayır |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| services | Evet | Evet |
| Hizmetleri/projeleri | Evet | Evet |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| sunucu | Evet | Evet |
| sunucuları/recoverableServers | Hayır |  Hayır |
| sunucuları/virtualNetworkRules | Hayır |  Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| sunucu | Evet | Evet |
| sunucuları/recoverableServers | Hayır |  Hayır |
| sunucuları/virtualNetworkRules | Hayır |  Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| sunucu | Evet | Evet |
| sunucuları/danışmanları | Hayır |  Hayır |
| sunucuları/queryTexts | Hayır |  Hayır |
| sunucuları/recoverableServers | Hayır |  Hayır |
| sunucuları/topQueryStatistics | Hayır |  Hayır |
| sunucuları/virtualNetworkRules | Hayır |  Hayır |
| sunucuları/waitStatistics | Hayır |  Hayır |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| IotHubs | Evet | Evet |
| IotHubs/eventGridFilters | Hayır |  Hayır |
| ProvisioningServices | Evet | Evet |
| Kullanımları | Hayır |  Hayır |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Denetleyicileri | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Laboratuvarları | Evet | Evet |
| Labs/serviceRunners | Evet | Evet |
| Labs/virtualMachines | Evet | Evet |
| Zamanlamaları | Evet | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| databaseAccountNames | Hayır |  Hayır |
| databaseAccounts | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| etki alanları | Evet | Evet |
| etki alanı/domainOwnershipIdentifiers | Hayır |  Hayır |
| generateSsoRequest | Hayır |  Hayır |
| topLevelDomains | Hayır |  Hayır |
| validateDomainRegistrationInformation | Hayır |  Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| lcsprojects | Hayır |  Hayır |
| lcsprojects/clouddeployments | Hayır |  Hayır |
| lcsprojects/bağlayıcılar | Hayır |  Hayır |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| etki alanları | Evet | Hayır |
| etki alanı/konuları | Hayır |  Hayır |
| eventSubscriptions | Hayır |  Hayır |
| extensionTopics | Hayır |  Hayır |
| konuları | Evet | Hayır |
| topicTypes | Hayır |  Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kümeleri | Evet | Hayır |
| ad alanları | Evet | Hayır |
| ad/authorizationrules öğesine | Hayır |  Hayır |
| ad/disasterrecoveryconfigs | Hayır |  Hayır |
| ad/eventhubs | Hayır |  Hayır |
| ad/eventhubs/authorizationrules öğesine | Hayır |  Hayır |
| ad/eventhubs/consumergroups | Hayır |  Hayır |

## <a name="microsoftfeatures"></a>Microsoft.Features
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| SaaS Uygulamaları Geliştirme | Hayır |  Hayır |
| sağlayıcıları | Hayır |  Hayır |

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kaydetme | Hayır |  Hayır |
| galleryitems | Hayır |  Hayır |
| generateartifactaccessuri | Hayır |  Hayır |
| myareas | Hayır |  Hayır |
| myareas/alanları | Hayır |  Hayır |
| myareas/alanlar/alanları | Hayır |  Hayır |
| myareas/alanlar/alanlar/galleryitems | Hayır |  Hayır |
| myareas/alanlar/galleryitems | Hayır |  Hayır |
| myareas/galleryitems | Hayır |  Hayır |
| Kaydolun | Hayır |  Hayır |
| kaynaklar | Hayır |  Hayır |
| retrieveresourcesbyid | Hayır |  Hayır |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| guestConfigurationAssignments | Hayır |  Hayır |
| Yazılım | Hayır |  Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hanaInstances | Evet |  Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kümeleri | Evet | Evet |
| kümeleri/uygulamaları | Hayır |  Hayır |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| işler | Evet | Evet |

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| labelGroups | Hayır |  Hayır |
| labelGroups/etiketleri | Hayır |  Hayır |
| etiketleri/labelGroups/koşulları | Hayır |  Hayır |
| etiketleri/labelGroups/subLabels | Hayır |  Hayır |
| labelGroups/etiketleri/subLabels/koşulları | Hayır |  Hayır |

## <a name="microsoftinsights"></a>Microsoft.insights
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| actiongroups | Evet | Evet |
| activityLogAlerts | Evet | Evet |
| alertrules | Evet | Evet |
| automatedExportSettings | Hayır |  Hayır |
| autoscalesettings | Evet | Evet |
| temel | Hayır |  Hayır |
| calculatebaseline | Hayır |  Hayır |
| Bileşenleri | Evet | Evet |
| bileşenleri/olaylar | Hayır |  Hayır |
| components/pricingPlans | Hayır |  Hayır |
| bileşenleri/sorgu | Hayır |  Hayır |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |
| eventCategories | Hayır |  Hayır |
| eventtypes | Hayır |  Hayır |
| extendedDiagnosticSettings | Hayır |  Hayır |
| logDefinitions | Hayır |  Hayır |
| logprofiles | Hayır |  Hayır |
| günlükler | Hayır |  Hayır |
| metricAlerts | Evet | Evet |
| migrateToNewPricingModel | Hayır |  Hayır |
| myWorkbooks | Hayır |  Hayır |
| sorgu | Hayır |  Hayır |
| rollbackToLegacyPricingModel | Hayır |  Hayır |
| scheduledqueryrules | Evet | Evet |
| vmInsightsOnboardingStatuses | Hayır |  Hayır |
| Web testleri | Evet | Evet |
| çalışma kitapları | Evet | Evet |

## <a name="microsoftintune"></a>Microsoft.Intune
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| IoTApps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Graf | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| deletedVaults | Hayır |  Hayır |
| kasaları | Evet | Evet |
| kasaları/accessPolicies | Hayır |  Hayır |
| Kasalar/parolalar | Hayır |  Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kümeleri | Evet | Evet |
| kümeleri/veritabanları | Hayır |  Hayır |
| veritabanları/kümeleri/dataconnections | Hayır |  Hayır |
| veritabanları/kümeleri/eventhubconnections | Hayır |  Hayır |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| labaccounts | Evet | Evet |
| kullanıcı | Hayır |  Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| günlükler | Hayır |  Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| integrationAccounts | Evet | Evet |
| İş akışları | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| commitmentPlans | Evet | Evet |
| Veritabanınızdaki | Evet | Evet |
| Çalışma Alanı | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |
| hesapları/çalışma alanları | Evet | Evet |
| çalışma alanları/hesapları/projeleri | Evet | Evet |
| teamAccounts | Evet | Evet |
| teamAccounts/çalışma alanları | Evet | Evet |
| çalışma alanları/teamAccounts/projeleri | Evet | Evet |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| çalışma alanı | Evet | Evet |
| çalışma alanları/işlemleri | Hayır |  Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.managedıdentity
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kimlikler | Hayır |  Hayır |
| Userassignedıdentities | Evet | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| getEntities | Hayır |  Hayır |
| managementGroups | Hayır |  Hayır |
| kaynaklar | Hayır |  Hayır |
| startTenantBackfill | Hayır |  Hayır |
| tenantBackfillStatus | Hayır |  Hayır |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |
| hesapları/eventGridFilters | Hayır |  Hayır |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Sunar | Hayır |  Hayır |
| offerTypes | Hayır |  Hayır |
| offerTypes/yayımcıları | Hayır |  Hayır |
| Yayımcılar/offerTypes/teklifler | Hayır |  Hayır |
| offerTypes/yayımcılar/teklif/planları | Hayır |  Hayır |
| offerTypes/yayımcılar/teklif/planları/sözleşmeleri | Hayır |  Hayır |
| offerTypes/yayımcılar/teklif/planları/yapılandırmalar | Hayır |  Hayır |
| offerTypes/publishers/offers/plans/configs/importImage | Hayır |  Hayır |
| privategalleryitems | Hayır |  Hayır |
| ürünler | Hayır |  Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| classicDevServices | Evet | Evet |
| updateCommunicationPreference | Hayır |  Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Anlaşmaları | Hayır |  Hayır |
| offertypes | Hayır |  Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| mediaservices | Evet | Evet |
| mediaservices/accountFilters | Hayır |  Hayır |
| mediaservices/varlıklar | Hayır |  Hayır |
| varlıklar/mediaservices/assetFilters | Hayır |  Hayır |
| mediaservices/contentKeyPolicies | Hayır |  Hayır |
| mediaservices/eventGridFilters | Hayır |  Hayır |
| mediaservices/liveEventOperations | Hayır |  Hayır |
| mediaservices/liveEvents | Evet | Evet |
| liveEvents/mediaservices/liveOutputs | Hayır |  Hayır |
| mediaservices/liveOutputOperations | Hayır |  Hayır |
| mediaservices/streamingEndpointOperations | Hayır |  Hayır |
| mediaservices/akış | Evet | Evet |
| mediaservices/streamingLocators | Hayır |  Hayır |
| mediaservices/streamingPolicies | Hayır |  Hayır |
| mediaservices/dönüştürme | Hayır |  Hayır |
| dönüşümler/mediaservices/işleri | Hayır |  Hayır |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Projeleri | Evet | Evet |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| applicationGateways | Evet | Hayır |
| Applicationsecuritygroup | Evet | Evet |
| azureFirewallFqdnTags | Hayır |  Hayır |
| azureFirewalls | Evet | Hayır |
| bgpServiceCommunities | Hayır |  Hayır |
| bağlantılar | Evet | Evet |
| ddosCustomPolicies | Evet | Evet |
| ddosProtectionPlans | Evet | Evet |
| dnsOperationStatuses | Hayır |  Hayır |
| dnszones | Evet | Evet |
| dnszones/A | Hayır |  Hayır |
| dnszones/AAAA | Hayır |  Hayır |
| dnszones/all | Hayır |  Hayır |
| dnszones/CAA | Hayır |  Hayır |
| dnszones/CNAME | Hayır |  Hayır |
| dnszones/MX | Hayır |  Hayır |
| dnszones/NS | Hayır |  Hayır |
| dnszones/PTR | Hayır |  Hayır |
| dnszones/kayıt kümeleri | Hayır |  Hayır |
| dnszones/SOA | Hayır |  Hayır |
| dnszones/SRV | Hayır |  Hayır |
| dnszones/TXT | Hayır |  Hayır |
| expressRouteCircuits | Evet  | Hayır |
| expressRouteServiceProviders | Hayır |  Hayır |
| frontdoors | Evet | Evet |
| frontdoorWebApplicationFirewallPolicies | Evet | Evet |
| getDnsResourceReference | Hayır |  Hayır |
| interfaceEndpoints | Evet | Evet |
| internalNotify | Hayır |  Hayır |
| Sonraki | Evet | Hayır |
| localNetworkGateways | Evet | Evet |
| natGateways | Evet | Evet |
| networkIntentPolicies | Evet | Evet |
| networkınterface'lerden bazıları | Evet | Evet |
| networkProfiles | Evet | Evet |
| networkSecurityGroups | Evet | Evet |
| networkWatchers | Evet | Hayır |
| networkWatchers/connectionMonitors | Evet | Hayır |
| networkWatchers/merceklerden | Evet | Hayır |
| networkWatchers/pingMeshes | Evet | Hayır |
| privateLinkServices | Evet | Evet |
| publicIPAddresses | Evet | Evet |
| publicIPPrefixes | Evet | Evet |
| routeFilters | Evet | Evet |
| routeTables | Evet | Evet |
| serviceEndpointPolicies | Evet | Evet |
| trafficManagerGeographicHierarchies | Hayır |  Hayır |
| trafficmanagerprofiles | Evet | Evet |
| trafficmanagerprofiles/ısı haritaları | Hayır |  Hayır |
| virtualHubs | Evet | Evet |
| virtualNetworkGateways | Evet | Hayır |
| virtualNetworks | Evet | Evet |
| virtualNetworks/alt ağlar | Hayır |  Hayır |
| virtualNetworkTaps | Evet | Evet |
| virtualWans | Evet | Evet |
| vpnGateways | Evet | Hayır |
| vpnSites | Evet | Evet |
| webApplicationFirewallPolicies | Evet | Evet |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| ad alanları | Evet | Hayır |
| ad/notificationHubs | Evet | Hayır |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| cihazlar | Hayır |  Hayır |
| linkTargets | Hayır |  Hayır |
| storageInsightConfigs | Hayır |  Hayır |
| çalışma alanı | Evet | Evet |
| çalışma alanları/veri kaynakları | Hayır |  Hayır |
| çalışma alanları/linkedServices | Hayır |  Hayır |
| çalışma alanları/sorgu | Hayır |  Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| managementassociations | Hayır |  Hayır |
| managementconfigurations | Evet | Evet |
| çözümler | Evet | Evet |
| görüntüleme | Evet | Evet |

## <a name="microsoftpolicyinsights"></a>Microsoft.policyınsights
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| policyEvents | Hayır |  Hayır |
| policyStates | Hayır |  Hayır |
| policyTrackedResources | Hayır |  Hayır |
| düzeltmeler | Hayır |  Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| konsolları | Hayır |  Hayır |
| Panolar | Evet | Evet |
| kullanıcı ayarlarını | Hayır |  Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| workspaceCollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kapasiteleri | Evet | Evet |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesaplar | Evet | Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| backupProtectedItems | Hayır |  Hayır |
| kasaları | Evet | Evet |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| ad alanları | Evet | Evet |
| ad/authorizationrules öğesine | Hayır |  Hayır |
| ad/hybridconnections | Hayır |  Hayır |
| ad/hybridconnections/authorizationrules öğesine | Hayır |  Hayır |
| ad/wcfrelays | Hayır |  Hayır |
| ad/wcfrelays/authorizationrules öğesine | Hayır |  Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| kaynaklar | Hayır |  Hayır |
| subscriptionsStatus | Hayır |  Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| availabilityStatuses | Hayır |  Hayır |
| childAvailabilityStatuses | Hayır |  Hayır |
| childResources | Hayır |  Hayır |
| olaylar | Hayır |  Hayır |
| impactedResources | Hayır |  Hayır |
| bildirimler | Hayır |  Hayır |

## <a name="microsoftresources"></a>Microsoft.Resources
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| dağıtımlar | Hayır |  Hayır |
| Dağıtımları/işlemleri | Hayır |  Hayır |
| Bağlantılar | Hayır |  Hayır |
| notifyResourceJobs | Hayır |  Hayır |
| sağlayıcıları | Hayır |  Hayır |
| resourceGroups | Hayır |  Hayır |
| kaynaklar | Hayır |  Hayır |
| abonelik | Hayır |  Hayır |
| Abonelikler/sağlayıcıları | Hayır |  Hayır |
| Abonelikler/kaynak grupları | Hayır |  Hayır |
| Abonelikler/resourcegroups/kaynaklar | Hayır |  Hayır |
| Abonelikler/kaynak | Hayır |  Hayır |
| Abonelikler/tagnames | Hayır |  Hayır |
| Abonelikler/tagNames/tagValues | Hayır |  Hayır |
| Kiracılar | Hayır |  Hayır |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| uygulamalar | Evet | Evet |
| saasresources | Hayır |  Hayır |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| akışlar | Evet | Evet |
| eyleminde | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| resourceHealthMetadata | Hayır |  Hayır |
| searchServices | Evet | Evet |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| advancedThreatProtectionSettings | Hayır |  Hayır |
| uyarı | Hayır |  Hayır |
| allowedConnections | Hayır |  Hayır |
| cihazları | Hayır |  Hayır |
| applicationWhitelistings | Hayır |  Hayır |
| AutoProvisioningSettings | Hayır |  Hayır |
| Özellikleri | Hayır |  Hayır |
| dataCollectionAgents | Hayır |  Hayır |
| discoveredSecuritySolutions | Hayır |  Hayır |
| externalSecuritySolutions | Hayır |  Hayır |
| InformationProtectionPolicies | Hayır |  Hayır |
| jitNetworkAccessPolicies | Hayır |  Hayır |
| izleme | Hayır |  Hayır |
| İzleme/kötü amaçlı yazılımdan koruma | Hayır |  Hayır |
| İzleme temel | Hayır |  Hayır |
| İzleme/düzeltme eki | Hayır |  Hayır |
| ilkeler | Hayır |  Hayır |
| fiyatları | Hayır |  Hayır |
| securityContacts | Hayır |  Hayır |
| securitySolutions | Hayır |  Hayır |
| securitySolutionsReferenceData | Hayır |  Hayır |
| securityStatus | Hayır |  Hayır |
| securityStatus/uç noktaları | Hayır |  Hayır |
| securityStatus/alt ağlar | Hayır |  Hayır |
| securityStatus/virtualMachines | Hayır |  Hayır |
| securityStatuses | Hayır |  Hayır |
| securityStatusesSummaries | Hayır |  Hayır |
| ayarlar | Hayır |  Hayır |
| Görevler | Hayır |  Hayır |
| Topolojileri | Hayır |  Hayır |
| workspaceSettings | Hayır |  Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| ad alanları | Evet | Hayır |
| ad/authorizationrules öğesine | Hayır |  Hayır |
| ad/disasterrecoveryconfigs | Hayır |  Hayır |
| ad/eventgridfilters | Hayır |  Hayır |
| ad/kuyruklar | Hayır |  Hayır |
| ad/kuyruk/authorizationrules öğesine | Hayır |  Hayır |
| ad/konuları | Hayır |  Hayır |
| ad alanları/konu/authorizationrules öğesine | Hayır |  Hayır |
| ad/konular/abonelikler | Hayır |  Hayır |
| ad alanları veya konular/abonelikler/kurallara | Hayır |  Hayır |
| premiumMessagingRegions | Hayır |  Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Kümeleri | Evet | Evet |
| kümeleri/uygulamaları | Hayır |  Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| uygulamalar | Evet | Evet |
| Ağ geçitleri | Evet | Evet |
| ağlar | Evet | Evet |
| gizli dizi | Evet | Evet |
| birim | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| SignalR | Evet | Evet |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| applianceDefinitions | Evet | Evet |
| cihazları | Evet | Evet |
| applicationDefinitions | Evet | Evet |
| uygulamalar | Evet | Evet |
| jitRequests | Evet | Evet |

## <a name="microsoftsql"></a>Microsoft.SQL
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| managedInstances | Evet | Evet |
| managedInstances/veritabanları | Evet (aşağıdaki nota bakın) | Evet |
| veritabanları/managedInstances/backupShortTermRetentionPolicies | Hayır | Hayır |
| veritabanları/managedInstances/şemaları/sütunlar/tablolar/sensitivityLabels | Hayır | Hayır |
| veritabanları/managedInstances/vulnerabilityAssessments | Hayır | Hayır |
| managedInstances/veritabanları/vulnerabilityAssessments/kuralları/temelleri | Hayır | Hayır |
| managedInstances/encryptionProtector | Hayır | Hayır |
| managedInstances/anahtarları | Hayır | Hayır |
| restorableDroppedDatabases/managedInstances/backupShortTermRetentionPolicies | Hayır | Hayır |
| managedInstances/vulnerabilityAssessments | Hayır | Hayır |
| sunucu | Evet | Evet |
| sunucuları/yöneticileri | Hayır |  Hayır |
| sunucuları/communicationLinks | Hayır |  Hayır |
| sunucuları/veritabanları | Evet (aşağıdaki nota bakın) | Evet |
| sunucuları/encryptionProtector | Hayır |  Hayır |
| sunucuları/firewallRules | Hayır |  Hayır |
| sunucuları/anahtarları | Hayır |  Hayır |
| sunucuları/restorableDroppedDatabases | Hayır |  Hayır |
| sunucuları/serviceobjectives | Hayır |  Hayır |
| sunucuları/tdeCertificates | Hayır |  Hayır |

> [!NOTE]
> Asıl veritabanı etiketleri desteklemez, ancak diğer veritabanlarını Azure SQL veri ambarı veritabanları dahil olmak üzere, etiketleri destekler. Azure SQL veri ambarı veritabanları, etkin olması gerekir (Duraklatılmadığı) durumu.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| SqlVirtualMachineGroups | Evet | Evet |
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Hayır |  Hayır |
| SqlVirtualMachines | Evet | Evet |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| storageAccounts | Evet | Evet |
| storageAccounts/blobServices | Hayır |  Hayır |
| storageAccounts/fileServices | Hayır |  Hayır |
| storageAccounts/queueServices | Hayır |  Hayır |
| storageAccounts/services | Hayır |  Hayır |
| storageAccounts/tableServices | Hayır |  Hayır |
| Kullanımları | Hayır |  Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| storageSyncServices | Evet | Evet |
| storageSyncServices/registeredServers | Hayır |  Hayır |
| storageSyncServices/syncGroups | Hayır |  Hayır |
| syncGroups/storageSyncServices/cloudEndpoints | Hayır |  Hayır |
| syncGroups/storageSyncServices/serverEndpoints | Hayır |  Hayır |
| storageSyncServices/iş akışları | Hayır |  Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Yöneticileri | Evet | Evet |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| streamingjobs | Evet (aşağıdaki nota bakın) | Evet |
| streamingjobs/diagnosticSettings | Hayır |  Hayır |

> [!NOTE]
> Streamingjobs çalışırken bir etiket ekleyemezsiniz. Bir etiket eklemek için kaynak durdurun.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| CreateSubscription | Hayır |  Hayır |
| SubscriptionDefinitions | Hayır |  Hayır |
| SubscriptionOperations | Hayır |  Hayır |

## <a name="microsoftsupport"></a>Microsoft.support
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| supporttickets | Hayır |  Hayır |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| providerRegistrations | Evet | Evet |
| kaynaklar | Evet | Evet |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Ortamlar | Evet | Hayır |
| ortamları/accessPolicies | Hayır |  Hayır |
| ortamları/eventsources | Evet | Hayır |
| ortamları/referenceDataSets | Evet | Hayır |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| hesap | Evet | Evet |
| hesabı/uzantısı | Evet | Evet |
| hesabı/proje | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft.Web
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| apiManagementAccounts | Hayır |  Hayır |
| apiManagementAccounts/apiAcls | Hayır |  Hayır |
| apiManagementAccounts/API'leri | Hayır |  Hayır |
| API/apiManagementAccounts/apiAcls | Hayır |  Hayır |
| API/apiManagementAccounts/connectionAcls | Hayır |  Hayır |
| API/apiManagementAccounts/bağlantıları | Hayır |  Hayır |
| apiManagementAccounts/API/bağlantı/connectionAcls | Hayır |  Hayır |
| API/apiManagementAccounts/localizedDefinitions | Hayır |  Hayır |
| apiManagementAccounts/connectionAcls | Hayır |  Hayır |
| apiManagementAccounts/bağlantıları | Hayır |  Hayır |
| billingMeters | Hayır |  Hayır |
| sertifika | Evet | Evet |
| connectionGateways | Evet | Evet |
| bağlantılar | Evet | Evet |
| customApis | Evet | Evet |
| deletedSites | Hayır |  Hayır |
| işlevler | Hayır |  Hayır |
| hostingEnvironments | Evet | Hayır |
| hostingEnvironments/multiRolePools | Hayır |  Hayır |
| multiRolePools/hostingEnvironments/örnekleri | Hayır |  Hayır |
| hostingEnvironments/workerPools | Hayır |  Hayır |
| workerPools/hostingEnvironments/örnekleri | Hayır |  Hayır |
| publishingUsers | Hayır |  Hayır |
| öneriler | Hayır |  Hayır |
| resourceHealthMetadata | Hayır |  Hayır |
| Çalışma zamanları | Hayır |  Hayır |
| serverFarms | Evet | Evet |
| serverFarms/çalışanları | Hayır |  Hayır |
| siteler | Evet | Evet |
| Site/domainOwnershipIdentifiers | Hayır |  Hayır |
| Site/hostNameBindings | Hayır |  Hayır |
| Site/örnekleri | Hayır |  Hayır |
| Örnek/Site/uzantıları | Hayır |  Hayır |
| Site/premieraddons | Evet | Evet |
| Site/önerileri | Hayır |  Hayır |
| Site/resourceHealthMetadata | Hayır |  Hayır |
| Site/Yuvalar | Evet | Evet |
| yuvaları/site/hostNameBindings | Hayır |  Hayır |
| yuvaları/site/örnekleri | Hayır |  Hayır |
| Siteler ve yuvaları/örnekleri/uzantıları | Hayır |  Hayır |
| sourceControls | Hayır |  Hayır |
| Doğrulama | Hayır |  Hayır |
| verifyHostingEnvironmentVnet | Hayır |  Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| DeviceServices | Evet | Evet |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Kaynak türü | Etiketleri destekler | Maliyet raporunda etiketi |
| ------------- | ----------- | ----------- |
| Bileşenleri | Hayır |  Hayır |
| componentsSummary | Hayır |  Hayır |
| monitorInstances | Hayır |  Hayır |
| monitorInstancesSummary | Hayır |  Hayır |
| İzleyiciler | Hayır |  Hayır |
| notificationSettings | Hayır |  Hayır |

## <a name="next-steps"></a>Sonraki adımlar
Etiketler kaynaklara öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md).
