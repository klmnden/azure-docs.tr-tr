---
title: Azure Service Fabric durum bilgisi olan Reliable Services tanılama | Microsoft Docs
description: Azure Service fabric'te durum bilgisi olan Reliable Services için tanılama işlevi
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: dekapur
ms.openlocfilehash: f49176f944aa2abfa1d355ce0bd207d1b544c275
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60772967"
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Durum Bilgisi Olan Reliable Services için tanılama işlevi
Azure Service Fabric durum bilgisi olan Reliable Services StatefulServiceBase sınıfı yayan [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) hizmette hata ayıklamak için kullanılan olayları nasıl çalışma zamanı olan işletim ve gidermeye yardım öngörüleri sağlayın.

## <a name="eventsource-events"></a>EventSource olaylarını
"Microsoft-ServiceFabric-Hizmetler" durum bilgisi olan Reliable Services StatefulServiceBase sınıfının EventSource adıdır Bu olay kaynağındaki etkinlikler görünür [tanılama olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) hizmet yüklenirken pencere [Visual Studio'da hata ayıklaması](service-fabric-debugging-your-application.md).

Örnekleri, toplama ve/veya EventSource olaylarını görüntüleme içinde yardım araçları ve teknolojileri [PerfView](https://www.microsoft.com/download/details.aspx?id=28567), [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md)ve [Microsoft TraceEvent Library](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Events
| Olay adı | Olay Kimliği | Düzey | Olay açıklaması |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Bilgilendirici |Hizmet RunAsync görevi başlatılırken yayılan |
| StatefulRunAsyncCancellation |2 |Bilgilendirici |Hizmet RunAsync görev iptal edildiğinde yayılan |
| StatefulRunAsyncCompletion |3 |Bilgilendirici |Hizmet RunAsync görev tamamlandığında yayılan |
| StatefulRunAsyncSlowCancellation |4 |Uyarı |Hizmet RunAsync görevi iptal tamamlanması çok uzun sürerse yayılan |
| StatefulRunAsyncFailure |5 |Hata |Hizmet RunAsync görevi bir özel durum oluşturduğunda yayılan |

## <a name="interpret-events"></a>Olayları yorumlama
StatefulRunAsyncInvocation StatefulRunAsyncCompletion ve StatefulRunAsyncCancellation olayları, hizmet yazan zamanlama ne zaman bir hizmet başlar, iptal ettiğinde veya tamamlandıktan yanı sıra, bir hizmet yaşam döngüsü anlamak için kullanışlıdır. Bu bilgiler ne zaman yararlı olabilir hizmeti hata ayıklaması veya hizmet yaşam döngüsünü anlayın.

Bunlar hizmet ile ilgili sorunları gösterir çünkü beklemeleri Kapat StatefulRunAsyncSlowCancellation ve StatefulRunAsyncFailure olayları dikkat.

Hizmet RunAsync() görevi bir özel durum oluşturur. her StatefulRunAsyncFailure yayılır. Genellikle, bir özel durum bir hata veya arıza hizmetinde gösterir. Ayrıca, farklı bir düğüme taşınması için özel durumun hizmetin başarısız olmasına neden olur. Bu işlem, pahalı olabilir ve hizmetin taşınırken gelen istekleri erteleyebilirsiniz. Beklemeleri özel durumun nedenini belirlemek ve eğer mümkünse bunu azaltmak.

Her bir iptal isteğine RunAsync görev için dört saniyeden daha uzun sürer StatefulRunAsyncSlowCancellation yayılır. Bir hizmet iptal tamamlanması çok uzun sürerse, hizmetin hızlı bir şekilde başka bir düğümde yeniden olanağı etkiler. Bu senaryo, hizmet genel kullanılabilirliğini etkileyebilir.

## <a name="performance-counters"></a>Performans sayaçları
Reliable Services çalışma zamanı, aşağıdaki performans sayacı kategorileri tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Service Fabric işlem Çoğaltıcısı |Azure Service Fabric işlem Çoğaltıcısı için özel sayaçlar |
| Service Fabric TStore |Belirli bir Azure Service Fabric TStore sayaçları |

Service Fabric işlem Çoğaltıcısı tarafından kullanılan [güvenilir durum Yöneticisi](service-fabric-reliable-services-reliable-collections-internals.md) belirli bir dizi işlem çoğaltmak için [çoğaltmaları](service-fabric-concepts-replica-lifecycle.md).

Service Fabric TStore kullanılan bir bileşendir [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections-internals.md) depolamak ve almak için kullanılan anahtar-değer çiftleri.

[Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) kullanılabilen Windows işletim sistemindeki varsayılan uygulama, toplama ve performans sayacı verilerini görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayacı verilerini toplama ve Azure tablolara yüklemeye başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örneği adları
Çok sayıda güvenilir hizmetler veya güvenilir hizmeti bölümleri olan bir küme, çok sayıda işlem Çoğaltıcısı performans sayacı örneği gerekir. Bu ayrıca TStore performans sayaçları için bir durumdur, ancak ayrıca güvenilir bir sözlük ve güvenilir kullanılan kuyruk sayısı ile çarpılır. Performans sayacı örneği adları belirli tanımlanmasına yardımcı olabilecek [bölüm](service-fabric-concepts-partitioning.md), hizmet yineleme ve durumu sağlayıcısı ile ilişkili performans sayacı örneği TStore, söz konusu olduğunda.

#### <a name="service-fabric-transactional-replicator-category"></a>Service Fabric işlem Çoğaltıcısı kategorisi
Kategori `Service Fabric Transactional Replicator`, şu biçimde sayaç örneği adları şunlardır:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionId* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dizesi gösterimidir. Bölüm kimliği bir GUID olduğundan dize gösterimine aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) "D" biçim belirteci ile.

*ServiceFabricReplicaId* belirli bir güvenilir hizmet çoğaltma ile ilişkili kimliği. Çoğaltma Kimliği kendi benzersiz olmasını sağlamak ve aynı bölüme tarafından oluşturulan diğer performans sayaç örnekleri ile çakışmasını önlemek için performans sayacı örnek adı dahil edilir. Çoğaltmalar ve reliable services özelliğinde kendi rolleri hakkında daha fazla ayrıntı bulunabilir [burada](service-fabric-concepts-replica-lifecycle.md).

Aşağıdaki sayaç örneği adı altında bir sayacın tipik `Service Fabric Transactional Replicator` kategorisi:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

Önceki örnekte `00d0126d-3e36-4d68-98da-cc4f7195d85e` Service Fabric bölüm kimliği, dize gösterimi olduğundan ve `131652217797162571` çoğaltma kimliğidir.

#### <a name="service-fabric-tstore-category"></a>Service Fabric TStore kategorisi
Kategori `Service Fabric TStore`, şu biçimde sayaç örneği adları şunlardır:

`ServiceFabricPartitionId:ServiceFabricReplicaId:ServiceFabricStateProviderId_PerformanceCounterInstanceDifferentiator`

*ServiceFabricPartitionId* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dizesi gösterimidir. Bölüm kimliği bir GUID olduğundan dize gösterimine aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) "D" biçim belirteci ile.

*ServiceFabricReplicaId* belirli bir güvenilir hizmet çoğaltma ile ilişkili kimliği. Çoğaltma Kimliği kendi benzersiz olmasını sağlamak ve aynı bölüme tarafından oluşturulan diğer performans sayaç örnekleri ile çakışmasını önlemek için performans sayacı örnek adı dahil edilir. Çoğaltmalar ve reliable services özelliğinde kendi rolleri hakkında daha fazla ayrıntı bulunabilir [burada](service-fabric-concepts-replica-lifecycle.md).

*ServiceFabricStateProviderId* güvenilir bir hizmet içinde bir durumu sağlayıcısı ile ilişkili kimliği. Durum sağlayıcısı kimliği bir TStore diğerinden ayırt etmek için performans sayacı örnek adı dahil edilir.

*PerformanceCounterInstanceDifferentiator* durumu sağlayıcısı içinde bir performans sayacı örneği ile ilişkili bir ayırt edici kimliğidir. Bu fark yaratan kendi benzersiz olmasını sağlamak ve diğer performans sayacı örneğiyle aynı durum sağlayıcısı tarafından oluşturulan çakışmayı önlemek için performans sayacı örnek adı dahil edilir.

Aşağıdaki sayaç örneği adı altında bir sayacın tipik `Service Fabric TStore` kategorisi:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571:142652217797162571_1337`

Önceki örnekte `00d0126d-3e36-4d68-98da-cc4f7195d85e` Service Fabric bölüm kimliği dizesi gösterimidir `131652217797162571` çoğaltma kimliği `142652217797162571` durumu sağlayıcısı kimliği ve `1337` performans sayacı örneği avantajıdır.

### <a name="transactional-replicator-performance-counters"></a>İşlem Çoğaltıcısı sayaçları

Reliable Services çalışma zamanı altında aşağıdaki olaylar yayan `Service Fabric Transactional Replicator` kategorisi

 Sayaç adı | Açıklama |
| --- | --- |
| Saniye başına başlatılan işlemler başlayın | Saniye başına oluşturulan yeni yazma işlemleri sayısı.|
| Saniye başına başlatılan işlemler | Güvenilir koleksiyonlar saniye başına gerçekleştirilen ekleme/güncelleştirme/silme işlemleri sayısı.|
| Günlük yazılan bayt/sn | Diske saniye başına işlem Çoğaltıcısı tarafından yazılan bayt sayısı |
| Saniye başına Kısıtlanan işlem | İşlemlerin sayısını, işlem kısıtlama nedeniyle çoğaltıcı tarafından saniyede reddetti. |
| Ort. İşlem ms/yürütme | Milisaniye cinsinden işlem başına ortalama yürütme gecikmesi |
| Ort. Temizleme gecikmesi (ms) | Disk temizleme işlemleri milisaniye cinsinden işlem Çoğaltıcısı başlatan ortalama süresi |

### <a name="tstore-performance-counters"></a>TStore sayaçları

Reliable Services çalışma zamanı altında aşağıdaki olaylar yayan `Service Fabric TStore` kategorisi

 Sayaç adı | Açıklama |
| --- | --- |
| Öğe sayısı | Depodaki öğe sayısı.|
| Disk Boyutu | Depo kontrol noktası dosyalarının bayt cinsinden toplam disk boyutu.|
| Denetim noktası dosyası yazma bayt/sn | En son denetim noktası dosyası için saniye başına yazılan bayt sayısı.|
| Disk aktarımı bayt/sn kopyalayın | (Birincil Çoğaltmada) okumak veya (bir ikincil çoğaltma üzerinde), bir kopya depolayıp sırasında saniye başına yazılan disk bayt sayısı.|

## <a name="next-steps"></a>Sonraki adımlar
[EventSource sağlayıcıları Perfview'de Aç](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
