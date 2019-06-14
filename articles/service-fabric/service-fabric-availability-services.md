---
title: Service Fabric hizmetlerinin kullanılabilirliği | Microsoft Docs
description: Hata algılama, yük devretme ve kurtarma Hizmetleri açıklanmaktadır
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: dd10af0d3c8a57168a27a039286ea0ec4c1dad02
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60310953"
---
# <a name="availability-of-service-fabric-services"></a>Service Fabric hizmetlerinin kullanılabilirliği
Bu makalede, Azure Service Fabric hizmet kullanılabilirliğini nasıl korur genel bir bakış sağlar.

## <a name="availability-of-service-fabric-stateless-services"></a>Service Fabric durum bilgisi olmayan hizmetler kullanılabilirliği
Service Fabric Hizmetleri, durum bilgisi olan veya durum bilgisi olmayan olabilir. Durum bilgisi olmayan hizmet sahip olmayan bir uygulama hizmetidir bir [yerel durumu](service-fabric-concepts-state.md) , yüksek oranda kullanılabilir veya güvenilir olması gerekir.

Durum bilgisi olmayan hizmet oluşturma gerektirir tanımlayan bir `InstanceCount`. Örnek sayısı, kümede çalışan uygulama mantığı durum bilgisi olmayan hizmetin örnek sayısını tanımlar. Örneği sayısının artırılması, bir durum bilgisi olmayan hizmetin ölçeğini ölçeklendirmenin önerilen bir yoldur.

Bir durum bilgisi olmayan adlı-hizmet örneğini başarısız olduğunda, uygun bir küme düğümünde yeni bir örneği oluşturulur. Örneğin, bir durum bilgisi olmayan hizmet örneği üzerinde Düğüm1 başarısız olabilir ve Düğüm5 yeniden oluşturulmalıdır.

## <a name="availability-of-service-fabric-stateful-services"></a>Service Fabric durum bilgisi olan hizmetlerin kullanılabilirliği
Durum bilgisi olan hizmet kendisiyle ilişkili bir duruma sahip. Service Fabric durum bilgisi olan hizmet çoğaltmalar bir dizi modellenir. Her yineleme, hizmetin kod çalışan bir örneğidir. Çoğaltma, hizmet durumunun bir kopyasını da vardır. Okuma ve yazma işlemleri olarak adlandırılan bir çoğaltma üzerinde gerçekleştirilen *birincil*. Durumu değişiklikleri yazma işlemleri *çoğaltılan* diğer çoğaltma için çoğaltma kümesindeki adlı *etkin ikincil veritabanı*ve uygulanır. 

Yalnızca bir birincil çoğaltma olabilir, ancak birden fazla etkin ikincil çoğaltma olabilir. Etkin ikincil çoğaltmaların sayısı yapılandırılabilir ve daha yüksek bir yineleme sayısı daha fazla sayıda eş zamanlı yazılım ve donanım hatalarına dayanabileceğinden.

Birincil çoğaltma kalırsa, Service Fabric yeni birincil etkin ikincil çoğaltmalardan birine sağlar çoğaltma. Bu etkin ikincil çoğaltma durumu, güncelleştirilmiş sürümünü aracılığıyla zaten *çoğaltma*, ve daha fazla okuma/yazma işlemleri işleme devam edebilirsiniz. Bu işlem olarak bilinir *yeniden yapılandırma* ve daha ayrıntılı açıklanmıştır [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md) makalesi.

Bir birincil veya etkin ikincil olan bir çoğaltma kavramı olarak bilinen *çoğaltma rolü*. Bu çoğaltmaların daha ayrıntılı açıklanır [çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md) makalesi. 

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Service Fabric Hizmetleri ölçeklendirme](service-fabric-concepts-scalability.md)
- [Service Fabric hizmetlerini bölümleme](service-fabric-concepts-partitioning.md)
- [Durum tanımlama ve yönetme](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

