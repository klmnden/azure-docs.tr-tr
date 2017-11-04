---
title: "Azure Service Fabric işlem kanal | Microsoft Docs"
description: "İşletimsel kanal Azure Service Fabric kümelerde oluşturulan günlükleri kapsamlı bir listesi."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: 934719868ab9968db352db2b440014d35dbc0274
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="operational-channel"></a>İşletimsel kanal 

İşletimsel kanal düğümleriniz ve kümenizi Service Fabric tarafından gerçekleştirilen üst düzey Eylemler günlüklerinden oluşur. "Tanılama" bir küme için etkinleştirildiğinde, Azure Tanılama Aracı kümenizde dağıtılır ve işletimsel kanal günlüklerinden okumak için yapılandırılmış varsayılan olarak açıktır. Yapılandırma hakkında daha fazla okuma [Azure Tanılama Aracı](service-fabric-diagnostics-event-aggregation-wad.md) daha fazla günlükleri veya performans sayaçları seçmek için küme Tanılama yapılandırmasını değiştirmek için. 

## <a name="operational-channel-logs"></a>İşletimsel kanal günlükleri 

İşletimsel kanalda Service Fabric tarafından sağlanan günlükleri kapsamlı bir listesi aşağıda verilmiştir. 

| Olay Kimliği | Ad | Kaynak (görev) | Düzey |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Bilgilendirme |
| 25621 | NodeOpenedSuccess | FabricNode | Bilgilendirme |
| 25622 | NodeOpenedFailed | FabricNode | Bilgilendirme |
| 25623 | NodeClosing | FabricNode | Bilgilendirme |
| 25624 | NodeClosed | FabricNode | Bilgilendirme |
| 25625 | NodeAborting | FabricNode | Bilgilendirme |
| 25626 | NodeAborted | FabricNode | Bilgilendirme |
| 29627 | ClusterUpgradeStart | CM | Bilgilendirme |
| 29628 | ClusterUpgradeComplete | CM | Bilgilendirme |
| 29629 | ClusterUpgradeRollback | CM | Bilgilendirme |
| 29630 | ClusterUpgradeRollbackComplete | CM | Bilgilendirme |
| 29631 | ClusterUpgradeDomainComplete | CM | Bilgilendirme |
| 23074 | ContainerActivated | Barındırma | Bilgilendirme |
| 23075 | ContainerDeactivated | Barındırma | Bilgilendirme |
| 29620 | ApplicationCreated | CM | Bilgilendirme |
| 29621 | ApplicationUpgradeStart | CM | Bilgilendirme |
| 29622 | ApplicationUpgradeComplete | CM | Bilgilendirme |
| 29623 | ApplicationUpgradeRollback | CM | Bilgilendirme |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Bilgilendirme |
| 29625 | ApplicationDeleted | CM | Bilgilendirme |
| 29626 | ApplicationUpgradeDomainComplete | CM | Bilgilendirme |
| 18566 | ServiceCreated | FM | Bilgilendirme |
| 18567 | ServiceDeleted | FM | Bilgilendirme |

## <a name="next-steps"></a>Sonraki adımlar

* Genel hakkında daha fazla bilgi [olay oluşturma platform düzeyinde](service-fabric-diagnostics-event-generation-infra.md) Service Fabric içinde
* Değiştirme, [Azure tanılama](service-fabric-diagnostics-event-aggregation-wad.md) daha fazla günlükleri toplamak için yapılandırma
* [Application Insights'ı ayarlama](service-fabric-diagnostics-event-analysis-appinsights.md) günlükleri kanal, işlemsel görmek için
