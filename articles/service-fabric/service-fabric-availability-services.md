---
title: "Service Fabric hizmetlerin kullanılabilirliğini | Microsoft Docs"
description: "Hata algılama, yük devretme ve kurtarma Hizmetleri açıklar"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 6c23a56df48434db3b82bce70cbd3a23941a077a
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="availability-of-service-fabric-services"></a>Service Fabric hizmetlerin kullanılabilirliğini
Bu makalede Azure Service Fabric bir hizmetin kullanılabilirliğini nasıl korur genel bir bakış sağlar.

## <a name="availability-of-service-fabric-stateless-services"></a>Service Fabric durum bilgisi olmayan hizmetler kullanılabilirliği
Service Fabric Hizmetleri durum bilgisi olan ya da durum bilgisiz olabilir. Durum bilgisiz hizmeti sahip olmayan bir uygulama hizmetidir bir [yerel durumu](service-fabric-concepts-state.md) , yüksek oranda kullanılabilir veya güvenilir olması gerekir.

Durum bilgisiz hizmet oluşturma gerektirir tanımlayan bir `InstanceCount`. Örnek sayısı, kümede çalışan bir durum bilgisi olmayan hizmetin uygulama mantığı örneklerinin sayısını tanımlar. Örneklerinin sayısını artırarak bir durum bilgisi olmayan hizmetin ölçeklendirme önerilen yoludur.

Bir durum bilgisiz adlı hizmet örneği başarısız olduğunda, uygun bir küme düğümünde yeni bir örneği oluşturulur. Örneğin, bir durum bilgisiz hizmet örneği üzerinde Düğüm1 başarısız olabilir ve Düğüm5'yeniden oluşturulmalıdır.

## <a name="availability-of-service-fabric-stateful-services"></a>Service Fabric durum bilgisi olan hizmetler kullanılabilirliği
Durum bilgisi olan hizmet kendisiyle ilişkili bir durumuna sahip. Service Fabric durum bilgisi olan hizmet çoğaltmaları kümesi olarak modellenir. Her Çoğaltma hizmetinin kodunun çalışan bir örneğidir. Çoğaltma durumu bu hizmet için bir kopyasını da sahiptir. Okuma ve yazma işlemleri adlı bir yinelemede gerçekleştirilen *birincil*. Durumu değişiklikleri yazma işlemleri *çoğaltılan* diğer çoğaltmalar için çoğaltma kümesinde, adlı *etkin ikinciller*ve uygulanır. 

Yalnızca bir birincil çoğaltmaya olabilir, ancak birden fazla etkin ikincil çoğaltma olabilir. Etkin ikincil çoğaltmaların sayısı yapılandırılabilir bir değerdir ve daha yüksek bir sayı çoğaltmalarının daha fazla sayıda eşzamanlı yazılım ve donanım hatalarına dayanabilir.

Birincil çoğaltma devre dışı kalırsa, Service Fabric etkin ikincil çoğaltmaları yeni birincil birini yapar çoğaltma. Bu etkin ikincil çoğaltma durumu güncelleştirilmiş sürümü aracılığıyla zaten *çoğaltma*, ve okuma/yazma işlemleri daha fazla işleme devam edebilirsiniz. Bu işlem olarak bilinir *yeniden yapılandırma* ve daha ayrıntılı açıklanmıştır [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md) makalesi.

Bir birincil veya etkin ikincil olan bir çoğaltma kavramı olarak bilinir *çoğaltma rolü*. Bu çoğaltmalar daha ayrıntılı açıklanır [çoğaltmaları ve örnekleri](service-fabric-concepts-replica-lifecycle.md) makalesi. 

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Service Fabric Hizmetleri ölçeklendirme](service-fabric-concepts-scalability.md)
- [Service Fabric Hizmetleri bölümlendirme](service-fabric-concepts-partitioning.md)
- [Tanımlama ve durumunu yönetme](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

