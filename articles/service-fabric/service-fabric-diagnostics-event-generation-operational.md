---
title: Azure Service Fabric olay listesi | Microsoft Docs
description: Olayları izlemenize yardımcı olması için Azure Service Fabric tarafından sağlanan kapsamlı bir listesi kümeleri.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: 7a4cccf774d89229810c1668f38e4e2ef99fa79d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60393048"
---
# <a name="list-of-service-fabric-events"></a>Service Fabric olayları listesi 

Service Fabric kümenizi durumunu bildirmek üzere küme olayları birincil bir dizi kullanıma sunan [Service Fabric olayları](service-fabric-diagnostics-events.md). Bunlar düğümlerinizi ve kümenizin Service Fabric tarafından gerçekleştirilen eylemler veya tarafından küme sahibi/işleci yönetim kararlara dayanır. Bu olayları, yapılandırma dahil olmak üzere bir sayı yapılandırarak erişilebilir [Azure İzleyici ile kümenizi günlükleri](service-fabric-diagnostics-oms-setup.md), veya sorgulama [Eventstore'a](service-fabric-diagnostics-eventstore.md). Service Fabric olayları Olay Görüntüleyicisi'nde gördüğünüz Windows makinelerde bu olayları olay günlüğüne - beslenir. 

Bu olayların özelliklerinden bazıları şunlardır
* Her olay küme içindeki belirli bir varlığa bağlıdır örneğin uygulama, hizmet, düğüm, çoğaltma.
* Her olay, bir dizi ortak alanları içerir: Eventınstanceıd, EventName ve kategorisi.
* Her olay, olay ile ilişkili varlığa geri tie alanları içerir. Örneğin, ApplicationCreated olay oluşturulan uygulama adını tanımlayan alanları gerekir.
* Olayları bunlar kullanılabileceği bir şekilde yapılandırılmış yapmak için Araçlar çeşitli daha fazla analiz. Ayrıca, ilgili olay ayrıntılarını, uzun bir dize yerine ayrı özellik olarak tanımlanır. 
* Olayları olarak yazılır Service fabric'te farklı alt sistemleri, aşağıdaki Source(Task) tarafından tanımlanır. Daha fazla bilgi, şirket içinde bu alt sistemlerin kullanılabilir [Service Fabric mimarisi](service-fabric-architecture.md) ve [Service Fabric teknik genel bakış](service-fabric-technical-overview.md).

Varlık tarafından düzenlenen bu Service Fabric olaylarının bir listesi aşağıda verilmiştir.

## <a name="cluster-events"></a>Küme olayları

**Küme yükseltme olayları**

Küme yükseltme hakkında daha fazla ayrıntı bulunabilir [burada](service-fabric-cluster-upgrade-windows-server.md).

| EventID | Ad | Kategori | Açıklama |Kaynak (görev) | Düzey | 
| --- | --- | --- | --- | --- | --- | 
| 29627 | ClusterUpgradeStarted | Yükseltme | Bir küme yükseltmesi başlatıldı | CM | Bilgilendirici |
| 29628 | ClusterUpgradeCompleted | Yükseltme | Bir küme yükseltmesi tamamlandı | CM | Bilgilendirici | 
| 29629 | ClusterUpgradeRollbackStarted | Yükseltme | Bir küme yükseltmesi için geri alma başlatıldı  | CM | Uyarı | 
| 29630 | ClusterUpgradeRollbackCompleted | Yükseltme | Bir küme yükseltmesi geri alma tamamlandı | CM | Uyarı | 
| 29631 | ClusterUpgradeDomainCompleted | Yükseltme | Bir yükseltme etki alanını bir küme yükseltmesi sırasında yükseltmeyi tamamladı. | CM | Bilgilendirici | 

## <a name="node-events"></a>Düğüm olayları

**Düğüm yaşam döngüsü olayları** 

| EventID | Ad | Kategori | Açıklama |Kaynak (görev) | Düzey |
| --- | --- | ---| --- | --- | --- | 
| 18602 | NodeDeactivateCompleted | StateTransition | Bir düğüm devre dışı bırakma tamamlandı | FM | Bilgilendirici | 
| 18603 | NodeUp | StateTransition | Kümenin bir düğümü başlatıldı algıladı. | FM | Bilgilendirici | 
| 18604 | NodeDown | StateTransition | Kümenin bir düğümü kapatıldı algıladı. Bir düğümü yeniden başlatma sırasında NodeUp olay tarafından izlenen bir NodeDown olayı görürsünüz. |  FM | Hata | 
| 18605 | NodeAddedToCluster | StateTransition |  Yeni bir düğüm kümeye eklenen ve Service Fabric uygulamaları bu düğüme dağıtabilirsiniz. | FM | Bilgilendirici | 
| 18606 | NodeRemovedFromCluster | StateTransition |  Bir düğümün kümeden kaldırılmış olabilir. Service Fabric uygulamaları bu düğüm için artık dağıtır | FM | Bilgilendirici | 
| 18607 | NodeDeactivateStarted | StateTransition |  Bir düğüm devre dışı bırakma başlatıldı | FM | Bilgilendirici | 
| 25621 | NodeOpenSucceeded | StateTransition |  Bir düğüm başarıyla başlatıldı | FabricNode | Bilgilendirici | 
| 25622 | NodeOpenFailed | StateTransition |  Bir düğüm başlatmak ve halka katılmak başarısız oldu | FabricNode | Hata | 
| 25624 | NodeClosed | StateTransition |  Bir düğüm başarıyla kapatıldı | FabricNode | Bilgilendirici | 
| 25626 | NodeAborted | StateTransition |  Bir düğüm ungracefully kapatıldı | FabricNode | Hata | 

## <a name="application-events"></a>Uygulama olayları

**Uygulama yaşam döngüsü olayları**

| EventID | Ad | Kategori | Açıklama |Kaynak (görev) | Düzey | 
| --- | --- | --- | --- | --- | --- | 
| 29620 | ApplicationCreated | Yaşam döngüsü | Yeni bir uygulama oluşturuldu | CM | Bilgilendirici | 
| 29625 | ApplicationDeleted | Yaşam döngüsü | Var olan bir uygulamayı silindi | CM | Bilgilendirici | 
| 23083 | ApplicationProcessExited | Yaşam döngüsü | Bir uygulamadaki bir işlemden çıkıldı | Barındırma | Bilgilendirici | 

**Uygulama yükseltme olayları**

Uygulama yükseltmeleri hakkında daha fazla ayrıntı bulunabilir [burada](service-fabric-application-upgrade.md).

| EventID | Ad | Kategori | Açıklama |Kaynak (görev) | Düzey | 
| --- | --- | ---| --- | --- | --- | 
| 29621 | ApplicationUpgradeStarted | Yükseltme | Uygulama yükseltmesi başlatıldı | CM | Bilgilendirici | 
| 29622 | ApplicationUpgradeCompleted | Yükseltme | Uygulama yükseltme tamamlandı | CM | Bilgilendirici | 
| 29623 | ApplicationUpgradeRollbackStarted | Yükseltme | Uygulama yükseltmesi için geri alma başlatıldı |CM | Uyarı | 
| 29624 | ApplicationUpgradeRollbackCompleted | Yükseltme | Uygulama yükseltmeyi geri alma tamamlandı | CM | Uyarı | 
| 29626 | ApplicationUpgradeDomainCompleted | Yükseltme | Bir yükseltme etki alanını bir uygulama yükseltmesi sırasında yükseltmeyi tamamladı. | CM | Bilgilendirici | 

## <a name="service-events"></a>Hizmet olayları

**Hizmet yaşam döngüsü olayları**

| EventID | Ad | Kategori | Açıklama |Kaynak (görev) | Düzey | 
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | Yaşam döngüsü | Yeni bir hizmet oluşturuldu | FM | Bilgilendirici | 
| 18658 | ServiceDeleted | Yaşam döngüsü | Var olan bir hizmeti silindi | FM | Bilgilendirici | 

## <a name="partition-events"></a>Bölüm olayları

**Bölüm taşıma olayları**

| EventID | Ad | Kategori | Açıklama |Kaynak (görev) | Düzey | 
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Yaşam döngüsü | Bölümü yeniden yapılandırma işlemi tamamlandı | RA | Bilgilendirici | 

## <a name="container-events"></a>Kapsayıcı olayları

**Kapsayıcı yaşam döngüsü olayları** 

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Bir kapsayıcı başlatıldı | Barındırma | Bilgilendirici | 1 |
| 23075 | ContainerDeactivated | Bir kapsayıcı durduruldu | Barındırma | Bilgilendirici | 1 |
| 23082 | ContainerExited | Bir kapsayıcı çıkıldı - UnexpectedTermination bayrağı denetleyin | Barındırma | Bilgilendirici | 1 |

## <a name="health-reports"></a>Sistem durumu raporlarının sayısı

[Service Fabric sistem durumu modeli](service-fabric-health-introduction.md) bir zengin, esnek ve Genişletilebilir bir sistem durumu değerlendirmesi sağlar ve raporlama. Service Fabric sürüm 6.2 başlayarak, sistem durumu verileri geçmiş kayıtlarını sistem durumu sağlamak için Platform olayları olarak yazılır. Sistem durumu olayların hacmine düşük tutmak için biz yalnızca aşağıdaki Service Fabric olayları gibi yazın:

* Tüm `Error` veya `Warning` sistem durumu raporlarının sayısı
* `Ok` geçiş sırasında sistem durumu raporu
* Olduğunda bir `Error` veya `Warning` sistem durumu olayı süresi dolar. Bu, ne kadar varlık sağlıksız olduğunu belirlemek için kullanılabilir

**Küme sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Yeni bir küme sistem durumu raporu kullanılabilir | HM | Bilgilendirici | 1 |
| 54437 | ClusterHealthReportExpired | Var olan bir küme sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Düğüm durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Yeni bir düğüm durumu raporu kullanılabilir | HM | Bilgilendirici | 1 |
| 54432 | NodeHealthReportExpired | Var olan bir düğüm durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Uygulama sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Yeni bir uygulama sistem durumu raporu oluşturuldu. Dağıtılmamış uygulamalar için budur. | HM | Bilgilendirici | 1 |
| 54426 | DeployedApplicationNewHealthReport | Yeni bir dağıtılmış uygulama sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Yeni bir dağıtılmış hizmet sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54434 | ApplicationHealthReportExpired | Var olan bir uygulama sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Var olan bir dağıtılmış uygulama sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Mevcut bir dağıtılan hizmet sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Hizmet durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Yeni bir hizmet sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54433 | ServiceHealthReportExpired | Mevcut bir hizmet sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Bölüm sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Yeni bir bölüm sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54431 | PartitionHealthReportExpired | Var olan bir bölüm sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

**Çoğaltma sistem durumu raporu olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Bir durum bilgisi olan çoğaltma sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54430 | StatelessInstanceNewHealthReport | Yeni bir durum bilgisi olmayan örnek sistem durumu raporu oluşturuldu | HM | Bilgilendirici | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Var olan bir durum bilgisi olan çoğaltma sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Var olan bir durum bilgisi olmayan örnek sistem durumu raporu süresi doldu | HM | Bilgilendirici | 1 |

## <a name="chaos-testing-events"></a>Kaos test olayları 

**Kaos oturum olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Test oturumu bir Chaos başlatıldı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50023 | ChaosStopped | Test oturumu bir Chaos durduruldu | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos düğüm olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Test oturumu bir Chaos bir parçası olarak yeniden başlatmak için zamanlanmış bir düğüm | Test Edilebilirlik | Bilgilendirici | 1 |
| 50087 | ChaosNodeRestartCompleted | Bir düğüm testi oturumunun bir Chaos bir parçası olarak yeniden başlatmayı tamamladı | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos uygulama olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Bir Chaos testi oturumu sırasında bir kod paketi yeniden zamanlandı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Bir Chaos testi oturumu sırasında bir kod paketi yeniden başlatma tamamlandı | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos bölüm olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Testi oturumunun bir Chaos bir parçası olarak taşımak için zamanlanmış bir birincil bölüm | Test Edilebilirlik | Bilgilendirici | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | İkincil bir bölüm testi oturumunun bir Chaos bir parçası olarak taşımak için zamanlanmış | Test Edilebilirlik | Bilgilendirici | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | Birincil bölüm geçiş daha derin bir analizini kullanılabilir | Test Edilebilirlik | Bilgilendirici | 1 |

**Kaos çoğaltma olayları**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Test oturumu bir Chaos bir parçası olarak bir çoğaltma yeniden zamanlandı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Test oturumu bir Chaos bir parçası olarak bir yineleme kaldırma işlemi zamanlandı | Test Edilebilirlik | Bilgilendirici | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Test oturumu bir Chaos bir parçası olarak bir yineleme kaldırma tamamlandı | Test Edilebilirlik | Bilgilendirici | 1 |

## <a name="other-events"></a>Diğer olayları

**Bağıntı olaylarını**

| EventID | Ad | Açıklama |Kaynak (görev) | Düzey | Version |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Bir ilişki algılandı | Test Edilebilirlik | Bilgilendirici | 1 |

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

* Genel Bakış [Service fabric'te tanılama](service-fabric-diagnostics-overview.md)
* Eventstore'a içinde hakkında daha fazla bilgi [Service Fabric Eventstore'a genel bakış](service-fabric-diagnostics-eventstore.md)
* Değiştirme, [Azure tanılama](service-fabric-diagnostics-event-aggregation-wad.md) daha fazla günlükleri toplamak için yapılandırma
* [Application ınsights'ı ayarlama](service-fabric-diagnostics-event-analysis-appinsights.md) günlükleri kanal, işlemsel görmek için
