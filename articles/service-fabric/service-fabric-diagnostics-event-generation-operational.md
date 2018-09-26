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
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: da4a779bca806fe6aa392db96eafc6c20f8ddcf6
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182170"
---
# <a name="list-of-service-fabric-events"></a>Service Fabric olayları listesi 

Service Fabric kümenizi durumunu bildirmek üzere küme olayları birincil bir dizi kullanıma sunan [Service Fabric olayları](service-fabric-diagnostics-events.md). Bunlar düğümlerinizi ve kümenizin Service Fabric tarafından gerçekleştirilen eylemler veya tarafından küme sahibi/işleci yönetim kararlara dayanır. Bu olaylar sorgulayarak erişilebilir [Eventstore'a](service-fabric-diagnostics-eventstore.md) kümenizdeki ya da işlevsel kanal aracılığıyla. Service Fabric olayları Olay Görüntüleyicisi'nde gördüğünüz Windows makinelerde işlevsel kanal ayrıca olay günlüğüne - ölçekledikçe. 

>[!NOTE]
>Sürümleri < 6.2 kümeleri için Service Fabric olayları listesi için lütfen aşağıdaki bölüme bakın. 

Bunların eşleneceğine varlık sıralama ölçütü bir platformda kullanılabilir tüm olayların bir listesi sunulmaktadır.

## <a name="cluster-events"></a>Küme olayları

**Küme yükseltme olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStarted | Bir küme yükseltmesi başlatıldı | CM | Bilgilendirici | 1 |
| 29628 | ClusterUpgradeCompleted | Bir küme yükseltmesi tamamlandı| CM | Bilgilendirici | 1 |
| 29629 | ClusterUpgradeRollbackStarted | Bir küme yükseltmesi için geri alma başlatıldı | CM | Bilgilendirici | 1 |
| 29630 | ClusterUpgradeRollbackCompleted | Bir küme yükseltmesi geri alma tamamlandı | CM | Bilgilendirici | 1 |
| 29631 | ClusterUpgradeDomainCompleted | Bir küme yükseltmesi sırasında bir etki alanı yükseltme tamamlandı | CM | Bilgilendirici | 1 |

## <a name="node-events"></a>Düğüm olayları

**Düğüm yaşam döngüsü olayları** 

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 18602 | NodeDeactivateCompleted | Bir düğüm devre dışı bırakma tamamlandı | FM | Bilgilendirici | 1 |
| 18603 | NodeUp | Kümenin bir düğümü başlatıldı algıladı. | FM | Bilgilendirici | 1 |
| 18604 | NodeDown | Kümenin bir düğümü kapatıldı algıladı. |  FM | Bilgilendirici | 1 |
| 18605 | NodeAddedToCluster | Kümeye yeni bir düğüm eklenmiştir. | FM | Bilgilendirici | 1 |
| 18606 | NodeRemovedFromCluster | Kümeden bir düğümü kaldırıldı | FM | Bilgilendirici | 1 |
| 18607 | NodeDeactivateStarted | Bir düğüm devre dışı bırakma başlatıldı | FM | Bilgilendirici | 1 |
| 25620 | NodeOpening | Bir düğüm başlatılıyor. İlk düğüm yaşam döngüsü aşaması | FabricNode | Bilgilendirici | 1 |
| 25621 | NodeOpenSucceeded | Bir düğüm başarıyla başlatıldı | FabricNode | Bilgilendirici | 1 |
| 25622 | NodeOpenFailed | Bir düğüm başlatılamadı | FabricNode | Bilgilendirici | 1 |
| 25623 | NodeClosing | Bir düğümü kapatılıyor. Son aşama düğüm yaşam döngüsünün başlangıcı | FabricNode | Bilgilendirici | 1 |
| 25624 | NodeClosed | Bir düğüm başarıyla kapatıldı | FabricNode | Bilgilendirici | 1 |
| 25625 | NodeAborting | Bir düğüm ungracefully kapanmaya başlıyor | FabricNode | Bilgilendirici | 1 |
| 25626 | NodeAborted | Bir düğüm ungracefully kapatıldı | FabricNode | Bilgilendirici | 1 |

## <a name="application-events"></a>Uygulama olayları

**Uygulama yaşam döngüsü olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 29620 | ApplicationCreated | Yeni bir uygulama oluşturuldu | CM | Bilgilendirici | 1 |
| 29625 | ApplicationDeleted | Var olan bir uygulamayı silindi | CM | Bilgilendirici | 1 |
| 23083 | ApplicationProcessExited | Bir uygulamadaki bir işlemden çıkıldı | Barındırma | Bilgilendirici | 1 |

**Uygulama yükseltme olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 29621 | ApplicationUpgradeStarted | Uygulama yükseltmesi başlatıldı | CM | Bilgilendirici | 1 |
| 29622 | ApplicationUpgradeCompleted | Uygulama yükseltme tamamlandı | CM | Bilgilendirici | 1 |
| 29623 | ApplicationUpgradeRollbackStarted | Uygulama yükseltmesi için geri alma başlatıldı |CM | Bilgilendirici | 1 |
| 29624 | ApplicationUpgradeRollbackCompleted | Uygulama yükseltmeyi geri alma tamamlandı | CM | Bilgilendirici | 1 |
| 29626 | ApplicationUpgradeDomainCompleted | Uygulama yükseltme sırasında bir etki alanı yükseltme tamamlandı | CM | Bilgilendirici | 1 |

## <a name="service-events"></a>Hizmet olayları

**Hizmet yaşam döngüsü olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | Yeni bir hizmet oluşturuldu | FM | Bilgilendirici | 1 |
| 18658 | ServiceDeleted | Var olan bir hizmeti silindi | FM | Bilgilendirici | 1 |

## <a name="partition-events"></a>Bölüm olayları

**Bölüm taşıma olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Bölümü yeniden yapılandırma işlemi tamamlandı | RA | Bilgilendirici | 1 |

## <a name="container-events"></a>Kapsayıcı olayları

**Kapsayıcı yaşam döngüsü olayları** 

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Bir kapsayıcı başlatıldı | Barındırma | Bilgilendirici | 1 |
| 23075 | ContainerDeactivated | Bir kapsayıcı durduruldu | Barındırma | Bilgilendirici | 1 |
| 23082 | ContainerExited | Bir kapsayıcı çıkıldı - UnexpectedTermination bayrağı denetleyin | Barındırma | Bilgilendirici | 1 |

## <a name="health-reports"></a>Sistem durumu raporlarının sayısı

**Küme sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Yeni bir küme sistem durumu raporu kullanılabilir | HM | Bilgilendirici | 1 |
| 54437 | ClusterHealthReportExpired | Var olan bir küme sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Düğüm durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Yeni bir düğüm durumu raporu kullanılabilir | HM | Bilgilendirici | 1 |
| 54432 | NodeHealthReportExpired | Var olan bir düğüm durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Uygulama sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Yeni bir uygulama sistem durumu raporu oluşturuldu. Dağıtılmamış uygulamalar için budur. | HM | Bilgilendirici | 1 |
| 54426 | DeployedApplicationNewHealthReport | Yeni bir dağıtılmış uygulama sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Yeni bir dağıtılmış hizmet sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54434 | ApplicationHealthReportExpired | Var olan bir uygulama sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Var olan bir dağıtılmış uygulama sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Mevcut bir dağıtılan hizmet sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Hizmet durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Yeni bir hizmet sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54433 | ServiceHealthReportExpired | Mevcut bir hizmet sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Bölüm sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Yeni bir bölüm sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54431 | PartitionHealthReportExpired | Var olan bir bölüm sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Çoğaltma sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Bir durum bilgisi olan çoğaltma sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54430 | StatelessInstanceNewHealthReport | Yeni bir durum bilgisi olmayan örnek sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Var olan bir durum bilgisi olan çoğaltma sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Var olan bir durum bilgisi olmayan örnek sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

## <a name="chaos-testing-events"></a>Kaos test olayları 

**Kaos oturum olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Test oturumu bir Chaos başlatıldı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50023 | ChaosStopped | Test oturumu bir Chaos durduruldu | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos düğüm olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Test oturumu bir Chaos bir parçası olarak yeniden başlatmak için zamanlanmış bir düğüm | Test Edilebilirlik | Bilgilendirici | 1 |
| 50087 | ChaosNodeRestartCompleted | Bir düğüm testi oturumunun bir Chaos bir parçası olarak yeniden başlatmayı tamamladı | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos uygulama olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Bir Chaos testi oturumu sırasında bir kod paketi yeniden zamanlandı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Bir Chaos testi oturumu sırasında bir kod paketi yeniden başlatma tamamlandı | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos bölüm olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Testi oturumunun bir Chaos bir parçası olarak taşımak için zamanlanmış bir birincil bölüm | Test Edilebilirlik | Bilgilendirici | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | İkincil bir bölüm testi oturumunun bir Chaos bir parçası olarak taşımak için zamanlanmış | Test Edilebilirlik | Bilgilendirici | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | Birincil bölüm geçiş daha derin bir analizini kullanılabilir | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos çoğaltma olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Test oturumu bir Chaos bir parçası olarak bir çoğaltma yeniden zamanlandı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Test oturumu bir Chaos bir parçası olarak bir yineleme kaldırma işlemi zamanlandı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Test oturumu bir Chaos bir parçası olarak bir yineleme kaldırma tamamlandı | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="other-events"></a>Diğer olayları

**Bağıntı olaylarını**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Bir bağıntı detacted kaldırıldı | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="events-prior-to-version-62"></a>Sürüm 6.2 önce olayları

Olayları 6.2 sürümü önce Service Fabric tarafından sağlanan kapsamlı bir listesi aşağıda verilmiştir.

| EventID | Ad | Kaynak (görev) | Düzey |
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

* Genel hakkında daha fazla bilgi [olay oluşturma platformu düzeyinde](service-fabric-diagnostics-event-generation-infra.md) Service fabric'te
* Değiştirme, [Azure tanılama](service-fabric-diagnostics-event-aggregation-wad.md) daha fazla günlükleri toplamak için yapılandırma
* [Application ınsights'ı ayarlama](service-fabric-diagnostics-event-analysis-appinsights.md) günlükleri kanal, işlemsel görmek için
