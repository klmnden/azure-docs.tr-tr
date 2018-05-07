---
title: Azure Service Fabric olay listesi | Microsoft Docs
description: Olayları izlemenize yardımcı olması için Azure Service Fabric tarafından sağlanan kapsamlı bir listesi kümeleri.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: d397dcb1ecdc25dbd66ab3d6f2f010bc29f87c6c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="list-of-service-fabric-events"></a>Service Fabric olayların listesi 

Service Fabric kümesi olarak durumunu bildirmek için küme olayları birincil kümesi sunan [Service Fabric olayları](service-fabric-diagnostics-events.md). Bunlar, düğümleri ve kümenizi Service Fabric tarafından gerçekleştirilen eylemler veya bir küme sahibi/işleciyle yönetim kararlara dayanır. Bu olaylar sorgulayarak erişilebilir [EventStore](service-fabric-diagnostics-eventstore.md) kümenizdeki veya işletimsel kanal üzerinden. Service Fabric olayları Olay Görüntüleyicisi'nde görebilmeleri Windows makinelerde işletimsel kanal ayrıca olay günlüğüne - sayfaya. 

>[!NOTE]
>Sürümler < 6.2 kümeleri için Service Fabric olaylar listesi için lütfen aşağıdaki bölüme bakın. 

Bunlar için eşleme varlığı göre sıralanmış bir platformda kullanılabilir tüm olaylar listesi aşağıdadır.

## <a name="cluster-events"></a>Küme olayları

**Küme yükseltme olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStartOperational | CM | Bilgilendirici | 1 |
| 29628 | ClusterUpgradeCompleteOperational | CM | Bilgilendirici | 1 |
| 29629 | ClusterUpgradeRollbackStartOperational | CM | Bilgilendirici | 1 |
| 29630 | ClusterUpgradeRollbackCompleteOperational | CM | Bilgilendirici | 1 |
| 29631 | ClusterUpgradeDomainCompleteOperational | CM | Bilgilendirici | 1 |

**Sistem Durumu raporu olayları küme**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 54428 | ProcessClusterReportOperational | HM | Bilgilendirici | 1 |
| 54437 | ExpiredClusterEventOperational | HM | Bilgilendirici | 1 |

**Chaos hizmeti olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 50021 | ChaosStartedEvent | Test Edilebilirlik | Bilgilendirici | 1 |
| 50023 | ChaosStoppedEvent | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="node-events"></a>Düğüm olayları

**Düğüm yaşam döngüsü olayları** 

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 18602 | DeactivateNodeCompletedOperational | FM | Bilgilendirici | 1 |
| 18603 | NodeUpOperational | FM | Bilgilendirici | 1 |
| 18604 | NodeDownOperational | FM | Bilgilendirici | 1 |
| 18605 | NodeAddedOperational | FM | Bilgilendirici | 1 |
| 18606 | NodeRemovedOperational | FM | Bilgilendirici | 1 |
| 18607 | DeactivateNodeStartOperational | FM | Bilgilendirici | 1 |
| 25620 | NodeOpening | FabricNode | Bilgilendirici | 1 |
| 25621 | NodeOpenedSuccess | FabricNode | Bilgilendirici | 1 |
| 25622 | NodeOpenedFailed | FabricNode | Bilgilendirici | 1 |
| 25623 | NodeClosing | FabricNode | Bilgilendirici | 1 |
| 25624 | NodeClosed | FabricNode | Bilgilendirici | 1 |
| 25625 | NodeAborting | FabricNode | Bilgilendirici | 1 |
| 25626 | NodeAborted | FabricNode | Bilgilendirici | 1 |

**Düğüm sistem durumu raporu olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 54423 | ProcessNodeReportOperational | HM | Bilgilendirici | 1 |
| 54432 | ExpiredNodeEventOperational | HM | Bilgilendirici | 1 |

**Chaos düğümü olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 50033 | ChaosRestartNodeFaultScheduledEvent | Test Edilebilirlik | Bilgilendirici | 1 |
| 50087 | ChaosRestartNodeFaultCompletedEvent | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="application-events"></a>Uygulama olayları

**Uygulama yaşam döngüsü olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 29620 | ApplicationCreatedOperational | CM | Bilgilendirici | 1 |
| 29625 | ApplicationDeletedOperational | CM | Bilgilendirici | 1 |
| 23083 | ProcessExitedOperational | Barındırma | Bilgilendirici | 1 |

**Uygulama yükseltme olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 29621 | ApplicationUpgradeStartOperational | CM | Bilgilendirici | 1 |
| 29622 | ApplicationUpgradeCompleteOperational | CM | Bilgilendirici | 1 |
| 29623 | ApplicationUpgradeRollbackStartOperational | CM | Bilgilendirici | 1 |
| 29624 | ApplicationUpgradeRollbackCompleteOperational | CM | Bilgilendirici | 1 |
| 29626 | ApplicationUpgradeDomainCompleteOperational | CM | Bilgilendirici | 1 |

**Uygulama sistem durumu raporu olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 54425 | ProcessApplicationReportOperational | HM | Bilgilendirici | 1 |
| 54426 | ProcessDeployedApplicationReportOperational | HM | Bilgilendirici | 1 |
| 54427 | ProcessDeployedServicePackageReportOperational | HM | Bilgilendirici | 1 |
| 54434 | ExpiredApplicationEventOperational | HM | Bilgilendirici | 1 |
| 54435 | ExpiredDeployedApplicationEventOperational | HM | Bilgilendirici | 1 |
| 54436 | ExpiredDeployedServicePackageEventOperational | HM | Bilgilendirici | 1 |

**Chaos uygulama olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 50053 | ChaosRestartCodePackageFaultScheduledEvent | Test Edilebilirlik | Bilgilendirici | 1 |
| 50101 | ChaosRestartCodePackageFaultCompletedEvent | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="service-events"></a>Hizmeti olayları

**Hizmet yaşam döngüsü olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 18602 | ServiceCreatedOperational | FM | Bilgilendirici | 1 |
| 18658 | ServiceDeletedOperational | FM | Bilgilendirici | 1 |

**Hizmet sistem durumu raporu olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 54424 | ProcessServiceReportOperational | HM | Bilgilendirici | 1 |
| 54433 | ExpiredServiceEventOperational | HM | Bilgilendirici | 1 |

## <a name="partition-events"></a>Bölüm olayları

**Bölüm taşıma olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 18940 | ReconfigurationCompleted | RA | Bilgilendirici | 1 |

**Bölüm sistem durumu raporu olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 54422 | ProcessPartitionReportOperational | HM | Bilgilendirici | 1 |
| 54431 | ExpiredPartitionEventOperational | HM | Bilgilendirici | 1 |

**Chaos bölüm olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 50069 | ChaosMovePrimaryFaultScheduledEvent | Test Edilebilirlik | Bilgilendirici | 1 |
| 50077 | ChaosMoveSecondaryFaultScheduledEvent | Test Edilebilirlik | Bilgilendirici | 1 |

**Bölüm çözümleme olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 65003 | PrimaryMoveAnalysisEvent | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="replica-events"></a>Çoğaltma olayları

**Çoğaltma sistem durumu raporu olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 54429 | ProcessStatefulReplicaReportOperational | HM | Bilgilendirici | 1 |
| 54430 | ProcessStatelessInstanceReportOperational | HM | Bilgilendirici | 1 |
| 54438 | ExpiredStatefulReplicaEventOperational | HM | Bilgilendirici | 1 |
| 54439 | ExpiredStatelessInstanceEventOperational | HM | Bilgilendirici | 1 |

**Chaos çoğaltma olayları**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 50047 | ChaosRestartReplicaFaultScheduledEvent | Test Edilebilirlik | Bilgilendirici | 1 |
| 50051 | ChaosRemoveReplicaFaultScheduledEvent | Test Edilebilirlik | Bilgilendirici | 1 |
| 50093 | ChaosRemoveReplicaFaultCompletedEvent | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="container-events"></a>Kapsayıcı olayları

**Kapsayıcı yaşam döngüsü olayları** 

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 23074 | ContainerActivatedOperational | Barındırma | Bilgilendirici | 1 |
| 23075 | ContainerDeactivatedOperational | Barındırma | Bilgilendirici | 1 |
| 23082 | ContainerExitedOperational | Barındırma | Bilgilendirici | 1 |

## <a name="other-events"></a>Diğer olaylar

**Bağıntı olaylarını**

| Olay Kimliği | Ad | Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- |
| 65011 | CorrelationOperational | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="events-prior-to-version-62"></a>Sürüm 6.2 önce olayları

Sürüm 6.2 önce Service Fabric tarafından sağlanan olayları kapsamlı bir listesi aşağıda verilmiştir.

| Olay Kimliği | Ad | Kaynak (görev) | Düzey |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Bilgilendirici |
| 25621 | NodeOpenedSuccess | FabricNode | Bilgilendirici |
| 25622 | NodeOpenedFailed | FabricNode | Bilgilendirici |
| 25623 | NodeClosing | FabricNode | Bilgilendirici |
| 25624 | NodeClosed | FabricNode | Bilgilendirici |
| 25625 | NodeAborting | FabricNode | Bilgilendirici |
| 25626 | NodeAborted | FabricNode | Bilgilendirici |
| 29627 | ClusterUpgradeStart | CM | Bilgilendirici |
| 29628 | ClusterUpgradeComplete | CM | Bilgilendirici |
| 29629 | ClusterUpgradeRollback | CM | Bilgilendirici |
| 29630 | ClusterUpgradeRollbackComplete | CM | Bilgilendirici |
| 29631 | ClusterUpgradeDomainComplete | CM | Bilgilendirici |
| 23074 | ContainerActivated | Barındırma | Bilgilendirici |
| 23075 | ContainerDeactivated | Barındırma | Bilgilendirici |
| 29620 | ApplicationCreated | CM | Bilgilendirici |
| 29621 | ApplicationUpgradeStart | CM | Bilgilendirici |
| 29622 | ApplicationUpgradeComplete | CM | Bilgilendirici |
| 29623 | ApplicationUpgradeRollback | CM | Bilgilendirici |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Bilgilendirici |
| 29625 | ApplicationDeleted | CM | Bilgilendirici |
| 29626 | ApplicationUpgradeDomainComplete | CM | Bilgilendirici |
| 18566 | ServiceCreated | FM | Bilgilendirici |
| 18567 | ServiceDeleted | FM | Bilgilendirici |

## <a name="next-steps"></a>Sonraki adımlar

* Genel hakkında daha fazla bilgi [olay oluşturma platform düzeyinde](service-fabric-diagnostics-event-generation-infra.md) Service Fabric içinde
* Değiştirme, [Azure tanılama](service-fabric-diagnostics-event-aggregation-wad.md) daha fazla günlükleri toplamak için yapılandırma
* [Application Insights'ı ayarlama](service-fabric-diagnostics-event-analysis-appinsights.md) günlükleri kanal, işlemsel görmek için
