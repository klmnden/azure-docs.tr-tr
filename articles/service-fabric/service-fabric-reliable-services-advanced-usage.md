---
title: "Güvenilir hizmetler kullanımını Gelişmiş | Microsoft Docs"
description: "Service Fabric'ın Reliable Services hizmetlerinizi eklenen esneklik için Gelişmiş kullanımı hakkında bilgi edinin."
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider
ms.assetid: f2942871-863d-47c3-b14a-7cdad9a742c7
ms.service: Service-Fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: 48504f258b13a7ff5f4c91db2d9de09269e92424
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Programlama modeli güvenilir hizmetler kullanımını Gelişmiş
Azure Service Fabric, yazma ve durum bilgisiz ve durum bilgisi olan güvenilir hizmetler yönetme basitleştirir. Bu kılavuz, Gelişmiş kullanımları hizmetlerinizi üzerinde daha fazla denetim ve esneklik sağlamak için güvenilir hizmetleri hakkında alınmaktadır. Bu kılavuzu okumadan önce ile öğrenmeniz [Reliable Services programlama modeli](service-fabric-reliable-services-introduction.md).

Durum bilgisi olan ve durum bilgisi olmayan hizmetler için kullanıcı kodu iki birincil giriş noktası vardır:

* `RunAsync(C#) / runAsync(Java)` Hizmet kodunuz için genel amaçlı giriş noktası niteliğindedir.
* `CreateServiceReplicaListeners(C#)` ve `CreateServiceInstanceListeners(C#) / createServiceInstanceListeners(Java)` istemci istekleri için iletişim dinleyicileri açmak içindir.

Çoğu hizmetler için bu iki giriş noktaları yeterlidir. Nadir durumlarda bir hizmetin yaşam döngüsü hakkında daha fazla denetime gerektiğinde ek yaşam döngüsü olayları kullanılabilir.

## <a name="stateless-service-instance-lifecycle"></a>Durum bilgisiz hizmet örneği yaşam döngüsü
Bir durum bilgisi olmayan hizmetin yaşam döngüsü, çok basit bir işlemdir. Durum bilgisiz hizmeti yalnızca açık, kapalı, durduruldu veya kaldırılabilir. `RunAsync` bir hizmet örneği açılır ve bir hizmet örneği durduruldu veya kapatıldığında iptal durum bilgisi olmayan bir hizmet olarak yürütülür.

Ancak `RunAsync` neredeyse tüm durumlarda, açık kapatın ve durum bilgisiz hizmet durdurma olayları kullanılabilir ayrıca içinde yeterli olması gerekir:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken) - C# / CompletableFuture<String> onOpenAsync(CancellationToken) - Java` Durum bilgisiz hizmet örneği hakkında kullanılacak olduğunda OnOpenAsync adı verilir. Genişletilmiş hizmeti başlatma görevleri şu anda başlatılabilir.
* `Task OnCloseAsync(CancellationToken) - C# / CompletableFuture onCloseAsync(CancellationToken) - Java` Durum bilgisiz hizmet örneği düzgün biçimde kapatılması geçerken OnCloseAsync adı verilir. Bu hizmetin kod yükseltilmesi, hizmet örneği, Yük Dengeleme nedeniyle taşındığı ya da geçici bir hata algılandığında ortaya çıkabilir. OnCloseAsync güvenli bir şekilde tüm kaynakları kapatmak, tüm arka plan işlemi durdurmak, dış durumu kaydetmeyi bitirmek veya varolan bağlantılar kapatmak için kullanılabilir.
* `void OnAbort() - C# / void onAbort() - Java` Durum bilgisiz hizmet örneği zorla kapatılıyor OnAbort denir. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric güvenilir bir şekilde iç hataları nedeniyle hizmet örneğinin yaşam döngüsü yönetemediğinde olarak adlandırılır.

## <a name="stateful-service-replica-lifecycle"></a>Durum bilgisi olan hizmet çoğaltma yaşam döngüsü

Bir durum bilgisi olan hizmet yinelemenin yaşam döngüsü, bir durum bilgisiz hizmet örneği çok daha karmaşıktır. Ayrıca açmak, kapatmak ve olayları durdurmak için yaşam süresi boyunca rol değişiklikleri bir durum bilgisi olan hizmet çoğaltma uğradığında. Bir durum bilgisi olan hizmet çoğaltma rolü değiştiğinde `OnChangeRoleAsync` olay tetiklenir:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)` Durum bilgisi olan hizmet çoğaltma rolü, örneğin birincil veya ikincil değiştirilirken OnChangeRoleAsync adı verilir. Birincil çoğaltmalara yazma durumu verilmiştir (oluşturun ve güvenilir koleksiyonlarına yazma izni verilir). İkincil çoğaltmaları (yalnızca var olan güvenilir koleksiyonlarından okuyabilir) okuma durumu atanır. Durum bilgisi olan hizmet çoğu çalışmasında birincil kopyada gerçekleştirilir. İkincil çoğaltmaların salt okunur doğrulama, rapor oluşturma, veri araştırma veya diğer salt okunur işleri gerçekleştirebilirsiniz.

Durum bilgisi olan bir hizmet olarak yalnızca birincil çoğaltma durumuna yazma erişimi olduğundan ve bunun sonucunda genellikle hizmet asıl işi zaman gerçekleştiriyor. `RunAsync` Bir durum bilgisi olan hizmet yönteminde yalnızca durum bilgisi olan hizmet çoğaltma birincil olduğunda gerçekleştirilir. `RunAsync` Birincil kopyanın rol birincil çıktığınızda yanı sıra kapatın ve durdurma olayları sırasında değiştiğinde yöntemi iptal edilir.

Kullanarak `OnChangeRoleAsync` olay çoğaltma rolü de rol değişikliği yanıtlar bağlı olarak iş gerçekleştirmenizi sağlar.

Durum bilgisi olan hizmet ayrıca kullanım örnekleri ve aynı semantiği ile durumsuz bir hizmet olarak aynı dört yaşam döngüsü olayları sağlar:

```csharp
* Task OnOpenAsync(IStatefulServicePartition, CancellationToken)
* Task OnCloseAsync(CancellationToken)
* void OnAbort()
```

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric ilgili daha gelişmiş konular için aşağıdaki makalelere bakın:

* [Durum bilgisi olan güvenilir hizmetler yapılandırma](service-fabric-reliable-services-configuration.md)
* [Service Fabric sistem durumu giriş](service-fabric-health-introduction.md)
* [Sorun giderme için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Service Fabric kümesi Resource Manager ile Hizmetleri'ni yapılandırma](service-fabric-cluster-resource-manager-configure-services.md)
