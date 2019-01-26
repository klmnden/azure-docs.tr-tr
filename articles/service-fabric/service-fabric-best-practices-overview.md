---
title: Azure Service Fabric uygulama ve küme en iyi uygulamalar | Microsoft Docs
description: Service Fabric kümeleri ve uygulamaları yönetmek için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 06240ac08a12b67e95b4cb9b9a33fcca32de45a8
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54914116"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Azure Service Fabric uygulama ve küme için en iyi yöntemler

Azure Service Fabric uygulama ve kümelerin başarılı bir şekilde yönetmek için üretim ortamınızın güvenilirliği iyileştirmek için almanızı öneririz işlemler vardır; Bu belgede tanımlanan işlemleri ve birini bizim [Azure örnekleri Service Fabric kümesi şablonları](https://github.com/Azure-Samples/service-fabric-cluster-templates) üretim çözümünüzü tasarlama başlamak ya da bu uygulamaları eklemek, varolan bir şablonu değiştirmek için.

## <a name="security"></a>Güvenlik 

* [En iyi güvenlik uygulamaları](service-fabric-best-practices-security.md)

## <a name="networking"></a>Ağ

* [Ağ için en iyi uygulamalar](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>İşlem planlama ve ölçekleme

* [İşlem ölçeklendirme için en iyi yöntemler](service-fabric-best-practices-capacity-scaling.md)
* [İşlem kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Kod olarak altyapı

* [Kod olarak altyapı uygulamak için en iyi yöntemler](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

* [Küme izleme ve tanılama için en iyi uygulamalar](service-fabric-best-practices-monitoring.md)

## <a name="checklist"></a>Denetim listesi

Yukarıdaki bölümlerde hepsini tamamladıktan sonra tüm üretim hazırlık listesindeki en iyi yöntemler tümleştirdiyseniz emin olun:
* [Azure Service Fabric üretim hazırlık denetim listesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Bir küme sanal makineleri veya Linux çalıştıran bilgisayarlara oluşturun: [Bir Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* Sorun giderme: [Service Fabric sorun giderme kılavuzu](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)