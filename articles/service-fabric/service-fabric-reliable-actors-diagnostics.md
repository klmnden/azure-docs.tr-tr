---
title: "Aktör tanılama ve izleme | Microsoft Docs"
description: "Bu makalede tanılama ve performans izleme olayları ve performans sayaçları tarafından gösterilen dahil olmak üzere Service Fabric Reliable Actors çalışma zamanında özelliklerini açıklar."
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: timlt
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/26/2017
ms.author: abhisram
ms.openlocfilehash: 5fbef8a3fb32f4bc47856ef6c6b459ae389dd541
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Reliable Actors için tanılama ve performans izlemesi
Reliable Actors çalışma zamanı yayar [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) olayları ve [performans sayaçları](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Bu çalışma zamanı nasıl çalıştığını içine bilgiler ve sorun giderme ve performans izleme ile Yardım.

## <a name="eventsource-events"></a>EventSource olaylarını
EventSource sağlayıcısı Reliable Actors çalışma zamanı için "Microsoft-ServiceFabric-aktörler" adıdır. Bu olay kaynağı olaylarından görünür [tanılama olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) aktör uygulama yüklenirken pencere [Visual Studio'da hata ayıklaması](service-fabric-debugging-your-application.md).

Araçlar ve toplama ve/veya EventSource olaylarını görüntüleme içinde yardımcı teknolojiler örnekleri [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md), [semantik günlük](https://msdn.microsoft.com/library/dn774980.aspx)ve [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Anahtar sözcükler
Güvenilir aktörler EventSource ait tüm olayları bir veya daha fazla ile ilişkili anahtar sözcükler. Bu, toplanan olayları filtreleme sağlar. Aşağıdaki anahtar sözcüğü BITS tanımlanır.

| bit | Açıklama |
| --- | --- |
| 0x1 |Fabric aktör çalışma zamanı işlemi özetleyen önemli olaylar ayarlayın. |
| 0x2 |Aktör yöntem çağrılarını açıklayan olaylar kümesi. Daha fazla bilgi için bkz: [aktörler giriş konuda](service-fabric-reliable-actors-introduction.md). |
| 0x4 |Aktör durumuyla ilgili olayları kümesi. Daha fazla bilgi için üzerinde konusuna [aktör durum yönetimi](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Aktör eşzamanlılık Aç tabanlı ilgili olayları kümesi. Daha fazla bilgi için üzerinde konusuna [eşzamanlılık](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Performans sayaçları
Reliable Actors çalışma zamanı aşağıdaki performans sayacı kategorisi tanımlar.

| Kategori | Açıklama |
| --- | --- |
| Service Fabric aktör |Aktör durumunu kaydetmek için harcanan Azure Service Fabric aktör için örneğin zaman özel sayaçlar |
| Service Fabric aktör metodu |Sayaçları belirli Service Fabric aktör tarafından uygulanan yöntemleri, örneğin ne sıklıkta bir aktör yöntemi çağrılır |

Yukarıdaki kategorilerin her biri bir veya daha fazla sayaca sahiptir.

[Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) varsayılan olarak Windows işletim sisteminde kullanılabilir uygulama, toplama ve performans sayacı verilerini görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayaç verileri toplayan ve Azure tablolara karşıya yükleme için başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örneği adları
Çok sayıda aktör Hizmetleri veya aktör hizmeti bölümleri olan bir küme, çok sayıda aktör performans sayacı örnekleri sahip olur. Performans sayacı örneği adları belirli tanımlanmasına yardımcı olabilecek [bölüm](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) ve aktör yöntemi (varsa) performans sayacı örneği ile ilişkili.

#### <a name="service-fabric-actor-category"></a>Service Fabric aktör kategorisi
Kategori için `Service Fabric Actor`, sayaç örneği adları aşağıdaki biçimdedir:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dize gösterimidir. Bölüm kimliği bir GUID olduğundan ve dize gösterimi aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) biçim belirticisi "D" yöntemiyle.

*ActorRuntimeInternalID* iç kullanımı için doku aktörler çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı dize gösterimidir. Bu benzersizliğini sağlamak ve diğer performans sayacı örneği adları ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Kullanıcılar, bu performans sayacı örneği adı kısmını yorumlamaya deneyin.

Aşağıdaki bir sayaç örneği adı ait bir sayacın örneğidir `Service Fabric Actor` kategorisi:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Yukarıdaki örnekte `2740af29-78aa-44bc-a20b-7e60fb783264` Service Fabric bölüm kimliği dize gösterimi ise ve `635650083799324046` çalışma zamanı iç için oluşturan 64-bit kimliği kullanın.

#### <a name="service-fabric-actor-method-category"></a>Service Fabric aktör metodu kategorisi
Kategori için `Service Fabric Actor Method`, sayaç örneği adları aşağıdaki biçimdedir:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* performans sayacı örneği ile ilişkili aktör yöntemin adı. Yöntem adı biçimi, Windows performans sayacı örneği adları en fazla uzunluk kısıtlamalar adıyla okunabilirliğini dengeleyen Fabric aktör çalışma zamanında bazı mantığı göre belirlenir.

*ActorsRuntimeMethodId* iç kullanımı için doku aktörler çalışma zamanı tarafından oluşturulan bir 32 bit tamsayı dize gösterimidir. Bu benzersizliğini sağlamak ve diğer performans sayacı örneği adları ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Kullanıcılar, bu performans sayacı örneği adı kısmını yorumlamaya deneyin.

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dize gösterimidir. Bölüm kimliği bir GUID olduğundan ve dize gösterimi aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) biçim belirticisi "D" yöntemiyle.

*ActorRuntimeInternalID* iç kullanımı için doku aktörler çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı dize gösterimidir. Bu benzersizliğini sağlamak ve diğer performans sayacı örneği adları ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Kullanıcılar, bu performans sayacı örneği adı kısmını yorumlamaya deneyin.

Aşağıdaki bir sayaç örneği adı ait bir sayacın örneğidir `Service Fabric Actor Method` kategorisi:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Yukarıdaki örnekte `ivoicemailboxactor.leavemessageasync` yöntem adı `2` zamanının iç kullanım için oluşturulan 32-bit kimliği `89383d32-e57e-4a9b-a6ad-57c6792aa521` Service Fabric bölüm kimliği dize gösterimi ise ve `635650083804480486` çalışma zamanı iç için oluşturulan 64-bit Kimliğini kullanın.

## <a name="list-of-events-and-performance-counters"></a>Olaylar ve performans sayaçları listesi
### <a name="actor-method-events-and-performance-counters"></a>Aktör yöntemi olaylar ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayar [aktör yöntemleri](service-fabric-reliable-actors-introduction.md).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Ayrıntılı |0x2 |Aktör çalışma zamanı aktör yöntemi hakkında çağırmaktır. |
| ActorMethodStop |8 |Ayrıntılı |0x2 |Aktör yöntemi yürütülmesi tamamlandı. Diğer bir deyişle, aktör yöntemi zaman uyumsuz çağrı zamanının döndürdü ve aktör yöntemi tarafından döndürülen görev tamamlandı. |
| ActorMethodThrewException |9 |Uyarı |0x3 |Aktör yöntemi yürütülmesi sırasında aktör yöntemine zamanının zaman uyumsuz çağrı sırasında veya görevin yürütülmesi sırasında aktör yöntemi tarafından döndürülen özel durum oluştu. Bu olay araştırma gerektiren aktör kodda hata çeşit gösterir. |

Reliable Actors çalışma zamanı aktör yöntemleri yürütülmesi ile ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric aktör metodu |Çağrıları/sn |Aktör hizmeti metodunu saniye başına çağrılma sayısı |
| Service Fabric aktör metodu |Çağrı başına ortalama milisaniye |Aktör hizmeti metodunu yürütmek için harcanan süre (milisaniye) |
| Service Fabric aktör metodu |Oluşturulan özel durum/sn |Aktör yöntemi hizmeti sayısını saniye başına özel durum oluşturdu |

### <a name="concurrency-events-and-performance-counters"></a>Eşzamanlılık olaylar ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayar [eşzamanlılık](service-fabric-reliable-actors-introduction.md#concurrency).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Ayrıntılı |0x8 |Bu olay, her yeni bir aktör sırayla başlangıcında yazılır. Bırakma tabanlı eşzamanlılık zorlar aktör başına kilit almayı bekleyen aktör çağrısı sayısı bekleyen sayısını içerir. |

Reliable Actors çalışma zamanı için eşzamanlılık ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric aktör |Aktör kilidi için bekleyen aktör çağrısı sayısı |Bırakma tabanlı eşzamanlılık zorlar aktör başına kilit almayı bekleyen aktör çağrısı sayısı bekleyen sayısı |
| Service Fabric aktör |Kilit bekleme işlemi başına ortalama süre (milisaniye) |(Milisaniye cinsinden) Aç tabanlı eşzamanlılık zorlar aktör başına kilit almak için geçen süre |
| Service Fabric aktör |Aktör kilidinin ortalama tutulma süresi (milisaniye) |Aktör başına kilit tutulduğu süre (milisaniye cinsinden) |

### <a name="actor-state-management-events-and-performance-counters"></a>Aktör durumunu yönetim olaylar ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayar [aktör durum yönetimi](service-fabric-reliable-actors-state-management.md).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Ayrıntılı |0x4 |Aktör çalışma zamanı hakkında aktör durumunu kaydetmek için ' dir. |
| ActorSaveStateStop |11 |Ayrıntılı |0x4 |Aktörler çalışma zamanı aktör durumu kaydetmeyi tamamladı. |

Reliable Actors çalışma zamanı aktör durumunu yönetimiyle ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric aktör |Durum kaydetme işlemi başına ortalama süre (milisaniye) |Aktör durumu kaydedilirken geçen süre (milisaniye) |
| Service Fabric aktör |Durum yükleme işlemi başına ortalama süre (milisaniye) |Aktör durumunun yüklenmesi için geçen süre (milisaniye) |

### <a name="events-related-to-actor-replicas"></a>Aktör çoğaltmalar için ilgili olayları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayar [aktör çoğaltmaları](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Bilgilendirme |0x1 |Aktör çoğaltma rolü için birincil değiştirildi. Bu, bu bölüm aktörler Bu çoğaltma içinde oluşturulacak anlamına gelir. |
| ReplicaChangeRoleFromPrimary |2 |Bilgilendirme |0x1 |Aktör çoğaltma rolü birincil olmayan için değiştirildi. Bu, bu bölüm aktörler artık bu çoğaltma içinde oluşturulacak anlamına gelir. Yeni İstek zaten bu çoğaltma içinde oluşturulan aktörler için teslim edilecek. Tüm Süren istekleri tamamlandıktan sonra aktörler yok. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktör etkinleştirme ve devre dışı bırakma olaylar ve performans sayaçları
Reliable Actors çalışma zamanı ile ilgili aşağıdaki olaylar yayar [aktör etkinleştirme ve devre dışı bırakma](service-fabric-reliable-actors-lifecycle.md).

| Olay adı | Olay Kimliği | Düzey | Anahtar sözcüğü | Açıklama |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Bilgilendirme |0x1 |Bir oyuncu etkinleştirildi. |
| ActorDeactivated |6 |Bilgilendirme |0x1 |Bir oyuncu devre dışı bırakıldı. |

Reliable Actors çalışma zamanı aktör etkinleştirme ve devre dışı bırakma işlemi ile ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric aktör |Ortalama OnActivateAsync milisaniye |OnActivateAsync metodunun yürütülmesi için geçen süre (milisaniye) |

### <a name="actor-request-processing-performance-counters"></a>Aktör isteği işleme performans sayaçları
Bir istemci bir aktör proxy nesnesi yöntemiyle çalıştırdığında, ağ üzerinden aktör hizmete gönderilen bir istek iletisi sonuçlanır. Hizmet İsteği iletisini işler ve istemcisine geri yanıt gönderir. Reliable Actors çalışma zamanı aktör isteği işlemiyle ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric aktör |Beklemedeki istek sayısı |Hizmetinde işlenmekte olan istek sayısı |
| Service Fabric aktör |İstek başına ortalama milisaniye |Bir isteği işlemek için (milisaniye cinsinden) hizmeti tarafından harcanan süre |
| Service Fabric aktör |İsteğin seri durumdan çıkarılması için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmeti alındığında aktör istek iletisi serisini kaldırmak için harcanan süre |
| Service Fabric aktör |Yanıtın serileştirilmesi için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) yanıt istemciye gönderilmeden önce konuşması aktör yanıtının serileştirmek için harcanan süre |

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Actors Service Fabric platformundan kullanma](service-fabric-reliable-actors-platform.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Örnek kod](https://github.com/Azure/servicefabric-samples)
* [PerfView EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
