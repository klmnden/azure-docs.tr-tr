---
title: Aktörlerde tanılama ve izleme | Microsoft Docs
description: Bu makalede tanılama ve performans izleme olayları ve performans sayaçları tarafından yayılan dahil olmak üzere Service Fabric Reliable Actors çalışma zamanı özellikleri açıklar.
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: chackdan
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/26/2017
ms.author: abhisram
ms.openlocfilehash: 5f573db887b3acc2c4a668a8c19c7f8e3cb25019
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670753"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Reliable Actors için tanılama ve performans izlemesi
Reliable Actors çalışma zamanı yayan [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) olayları ve [performans sayaçları](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Bunlar, çalışma zamanının nasıl çalıştığını içine ayrıntılı bilgiler sağlar ve performans izleme ve sorun giderme ile yardımcı.

## <a name="eventsource-events"></a>EventSource olaylarını
Reliable Actors çalışma zamanı için EventSource sağlayıcı adı "Microsoft ServiceFabric aktörler" ' dir. Bu olay kaynağındaki etkinlikler görünür [tanılama olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) actor uygulaması yüklenirken pencere [Visual Studio'da hata ayıklaması](service-fabric-debugging-your-application.md).

Örnekleri, toplama ve/veya EventSource olaylarını görüntüleme içinde yardım araçları ve teknolojileri [PerfView](https://www.microsoft.com/download/details.aspx?id=28567), [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md), [semantik günlük](https://msdn.microsoft.com/library/dn774980.aspx)ve [ Microsoft Announcing Kitaplığı](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Anahtar Sözcükler
Reliable Actors EventSource için ait tüm olayları bir veya daha fazla anahtar sözcükleri ile ilişkilendirilir. Bu, toplanan olayları filtreleme sağlar. Aşağıdaki anahtar sözcüğü bitleri tanımlanır.

| Bit | Açıklama |
| --- | --- |
| 0x1 |Fabric aktörleri çalışma zamanı işlemi özetleyen önemli olaylar ayarlayın. |
| 0x2 |Aktör yöntem çağrılarını açıklayan olaylar kümesi. Daha fazla bilgi için [aktörler tanıtım konuda](service-fabric-reliable-actors-introduction.md). |
| 0x4 |Aktör durumuyla ilgili olaylar kümesi. Daha fazla bilgi için üzerinde konusuna [aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Aktör içinde sırayla oynadıkları eşzamanlılık ilgili olaylar kümesi. Daha fazla bilgi için üzerinde konusuna [eşzamanlılık](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Performans sayaçları
Reliable Actors çalışma zamanı, aşağıdaki performans sayacı kategorileri tanımlar.

| Kategori | Açıklama |
| --- | --- |
| Service Fabric Aktörü |Aktör durumu kaydedilirken geçen Azure Service Fabric aktörleri için örneğin zaman özel sayaçlar |
| Service Fabric Aktör Metodu |Sayaçları belirli Service Fabric aktör tarafından uygulanan yöntemleri, örneğin ne sıklıkta bir aktör yöntemi çağrılır |

Yukarıdaki kategorilerden her biri bir veya daha fazla sayaca sahiptir.

[Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) kullanılabilen Windows işletim sistemindeki varsayılan uygulama, toplama ve performans sayacı verilerini görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayacı verilerini toplama ve Azure tablolara yüklemeye başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örneği adları
Çok sayıda aktör Hizmetleri ya da aktör hizmeti bölümleri olan bir küme, çok sayıda aktör performans sayacı örneği gerekir. Performans sayacı örneği adları belirli tanımlanmasına yardımcı olabilecek [bölüm](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) ve aktör metodu (varsa) performans sayacı örneği ile ilişkili.

#### <a name="service-fabric-actor-category"></a>Service Fabric aktör kategorisi
Kategori `Service Fabric Actor`, şu biçimde sayaç örneği adları şunlardır:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dizesi gösterimidir. Bölüm kimliği bir GUID olduğundan dize gösterimine aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) "D" biçim belirteci ile yöntemi.

*ActorRuntimeInternalID* kendi iç kullanım için Fabric aktörleri çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı, dize temsilidir. Bu, benzersiz olmasını sağlamak ve diğer performans sayacı örneği adlarla çakışma olmasını önlemek için performans sayacı örnek adı dahil edilir. Kullanıcılar bu performans sayacı örneği adı kısmı yorumlamak çalışmamanız gerekir.

Aşağıdaki örneğidir ait bir sayacın sayaç örneği adı `Service Fabric Actor` kategorisi:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Yukarıdaki örnekte `2740af29-78aa-44bc-a20b-7e60fb783264` Service Fabric bölüm kimliği, dize gösterimi olduğundan ve `635650083799324046` olan çalışma zamanı iç için oluşturulan 64-bit kimliği kullanın.

#### <a name="service-fabric-actor-method-category"></a>Service Fabric aktör metodu kategorisi
Kategori `Service Fabric Actor Method`, şu biçimde sayaç örneği adları şunlardır:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* performans sayacı örneği ile ilişkili aktör yöntemin adı. Yöntem adı biçimi, Windows performans sayacı örneği adları en fazla uzunluk kısıtlamaları adıyla okunabilirliğini dengeleyen Fabric aktörleri çalışma zamanında mantığa göre belirlenir.

*ActorsRuntimeMethodId* kendi iç kullanım için Fabric aktörleri çalışma zamanı tarafından oluşturulan bir 32 bit tamsayı, dize temsilidir. Bu, benzersiz olmasını sağlamak ve diğer performans sayacı örneği adlarla çakışma olmasını önlemek için performans sayacı örnek adı dahil edilir. Kullanıcılar bu performans sayacı örneği adı kısmı yorumlamak çalışmamanız gerekir.

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dizesi gösterimidir. Bölüm kimliği bir GUID olduğundan dize gösterimine aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) "D" biçim belirteci ile yöntemi.

*ActorRuntimeInternalID* kendi iç kullanım için Fabric aktörleri çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı, dize temsilidir. Bu, benzersiz olmasını sağlamak ve diğer performans sayacı örneği adlarla çakışma olmasını önlemek için performans sayacı örnek adı dahil edilir. Kullanıcılar bu performans sayacı örneği adı kısmı yorumlamak çalışmamanız gerekir.

Aşağıdaki örneğidir ait bir sayacın sayaç örneği adı `Service Fabric Actor Method` kategorisi:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Yukarıdaki örnekte `ivoicemailboxactor.leavemessageasync` yöntem adı `2` çalışma zamanının iç kullanım için oluşturulan 32-bit kimliği `89383d32-e57e-4a9b-a6ad-57c6792aa521` Service Fabric bölüm kimliği, dize gösterimi olduğundan ve `635650083804480486` için oluşturulan 64-bit kimliği çalışma zamanının iç kullanın.

## <a name="list-of-events-and-performance-counters"></a>Olaylar ve performans sayaçları listesi
### <a name="actor-method-events-and-performance-counters"></a>Aktör yöntemi olaylar ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayan [aktör yöntemleri](service-fabric-reliable-actors-introduction.md).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Ayrıntılı |0x2 |Yaklaşık bir aktör yöntemini çağırmak için aktör çalışma zamanıdır. |
| ActorMethodStop |8 |Ayrıntılı |0x2 |Aktör yöntemi yürütülmesi tamamlandı. Diğer bir deyişle, aktör yöntemin zaman uyumsuz çağrı çalışma zamanının döndürdü ve aktör yöntemi tarafından döndürülen görev tamamlandı. |
| ActorMethodThrewException |9 |Uyarı |0x3 |Bir özel durum, çalışma zamanının aktör yöntemi zaman uyumsuz çağrı sırasında veya görevin yürütülmesi sırasında aktör yöntem tarafından döndürülen bir aktör yönteminin yürütülmesi sırasında oluşturuldu. Bu olay araştırma gerektiren aktör kodda hata çeşit gösterir. |

Reliable Actors çalışma zamanı aktör yöntemlerin çalıştırmayla ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Aktör Metodu |Çağrı/Sn |Aktör hizmeti metodunu saniye başına çağrılma sayısı |
| Service Fabric Aktör Metodu |Çağrı başına ortalama süre (milisaniye) |Aktör hizmeti metodunu yürütmek için harcanan süre (milisaniye) |
| Service Fabric Aktör Metodu |Saniye Başına Oluşturulan Özel Durum |Aktör yöntemi hizmeti sayısını saniye başına özel durum oluşturdu |

### <a name="concurrency-events-and-performance-counters"></a>Eşzamanlılık olayları ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayan [eşzamanlılık](service-fabric-reliable-actors-introduction.md#concurrency).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Ayrıntılı |0x8 |Bu olay, her yeni bir aktör sırayla başlangıcında yazılır. Bu sırayla oynadıkları eşzamanlılık zorlayan aktör başına kilit almayı bekleyen aktör çağrısı sayısı bekleyen sayısını içerir. |

Reliable Actors çalışma zamanı için eşzamanlılık ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Aktörü |Aktör kilidi için bekleyen aktör çağrısı sayısı |Eşzamanlılık sırayla oynadıkları zorlayan aktör başına kilit almayı bekleyen aktör çağrısı sayısı bekleyen sayısı |
| Service Fabric Aktörü |Kilit bekleme işlemi başına ortalama süre (milisaniye) |(Milisaniye cinsinden) eşzamanlılık sırayla oynadıkları zorlayan aktör başına kilit için harcanan süre |
| Service Fabric Aktörü |Aktör kilidinin ortalama tutulma süresi (milisaniye) |Kendisi için aktör başına kilit Tutulma süresi (milisaniye cinsinden) |

### <a name="actor-state-management-events-and-performance-counters"></a>Aktör durumu yönetimi olayları ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayan [aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Ayrıntılı |0x4 |Aktörler çalışma zamanı, yaklaşık aktör durumunun tasarruf etmektir. |
| ActorSaveStateStop |11 |Ayrıntılı |0x4 |Aktörler çalışma zamanı aktör durumu kaydedilirken tamamlandığı anlamına gelir. |

Reliable Actors çalışma zamanı aktör durumu yönetimi ile ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Aktörü |Durum kaydetme işlemi başına ortalama süre (milisaniye) |Aktör durumu kaydedilirken geçen süre (milisaniye) |
| Service Fabric Aktörü |Durum yükleme işlemi başına ortalama süre (milisaniye) |Aktör durumunun yüklenmesi için geçen süre (milisaniye) |

### <a name="events-related-to-actor-replicas"></a>Aktör çoğaltmalarının ilgili olayları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayan [aktör çoğaltmalarının](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Bilgilendirici |0x1 |Aktör çoğaltma rolü birincil siteden değiştirildi. Bu aktörler için bu bölümü içinde bu çoğaltma oluşturulacak anlamına gelir. |
| ReplicaChangeRoleFromPrimary |2 |Bilgilendirici |0x1 |Aktör çoğaltma rolünün birincil olmayan için değiştirildi. Bu aktörler Bu bölüm için artık bu çoğaltma içinde oluşturulacak anlamına gelir. Yeni İstek içinde bu çoğaltma daha önce oluşturulmuş aktörler verilecektir. Tüm Süren istekleri tamamlandıktan sonra aktörleri yok edilir. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktör etkinleştirme ve devre dışı bırakma olaylar ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayan [aktör etkinleştirme ve devre dışı bırakma](service-fabric-reliable-actors-lifecycle.md).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Bilgilendirici |0x1 |Aktörün etkinleştirildi. |
| ActorDeactivated |6 |Bilgilendirici |0x1 |Aktörün devre dışı bırakıldı. |

Reliable Actors çalışma zamanı aktör etkinleştirme ve devre dışı bırakma ile ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Aktörü |Ortalama OnActivateAsync süresi (milisaniye) |OnActivateAsync metodunun yürütülmesi için geçen süre (milisaniye) |

### <a name="actor-request-processing-performance-counters"></a>Aktör isteğinin işlenmesi performans sayaçları
Bir istemci bir aktör proxy nesnesi aracılığıyla bir yöntemi çağırdığında, ağ üzerinden aktör hizmetine gönderilen bir istek iletisi ile sonuçlanır. Hizmet isteği iletiyi işler ve istemcisine geri yanıt gönderir. Reliable Actors çalışma zamanı aktör isteğinin işlemiyle ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Aktörü |Beklemedeki istek sayısı |Hizmette işlenmekte olan istek sayısı |
| Service Fabric Aktörü |İstek başına ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmeti tarafından bir isteği işlemek için harcanan süre |
| Service Fabric Aktörü |İsteğin seri durumdan çıkarılması için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmeti alındığında aktör istek iletisi seri durumdan çıkarmak için harcanan süre |
| Service Fabric Aktörü |Yanıtın serileştirilmesi için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmetine aktör yanıtının yanıtı istemciye gönderilmeden önce seri hale getirmek için harcanan süre |

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Actors, Service Fabric platform kullanma](service-fabric-reliable-actors-platform.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [EventSource sağlayıcıları Perfview'de Aç](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
