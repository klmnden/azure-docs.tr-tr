---
title: Azure Service Fabric durum bilgisi olan güvenilir hizmetler tanılama | Microsoft Docs
description: Azure Service Fabric durum bilgisi olan güvenilir hizmetler için tanılama işlevi
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2018
ms.author: dekapur
ms.openlocfilehash: 51895731efd466a314877e963a5fd2c6d868ec02
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110881"
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Durum Bilgisi Olan Reliable Services için tanılama işlevi
Azure Service Fabric durum bilgisi olan güvenilir hizmetler StatefulServiceBase sınıfı yayar [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) hizmetinde hata ayıklama için kullanılan olayları nasıl çalışma zamanı işletim ve sorunu gidermenize yardımcı içine Öngörüler sağlayın.

## <a name="eventsource-events"></a>EventSource olaylarını
EventSource durum bilgisi olan güvenilir hizmetler StatefulServiceBase sınıfı için "Microsoft-ServiceFabric-Services." adıdır Bu olay kaynağı olaylarından görünür [tanılama olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) hizmet yüklenirken pencere [Visual Studio'da hata ayıklaması](service-fabric-debugging-your-application.md).

Araçlar ve toplama ve/veya EventSource olaylarını görüntüleme içinde yardımcı teknolojiler örnekleri [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md)ve [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Olaylar
| Olay adı | Olay kimliği | Düzey | Olay açıklaması |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Bilgilendirici |Hizmet RunAsync görevi başlatıldığında yayılan |
| StatefulRunAsyncCancellation |2 |Bilgilendirici |Hizmet RunAsync görevi iptal ettiğinizde yayılan |
| StatefulRunAsyncCompletion |3 |Bilgilendirici |Hizmet RunAsync görevi tamamlandığında yayılan |
| StatefulRunAsyncSlowCancellation |4 |Uyarı |Hizmet RunAsync görevi iptal tamamlanması çok uzun sürerse yayılan |
| StatefulRunAsyncFailure |5 |Hata |Hizmet RunAsync görevi bir özel durum oluşturduğunda yayılan |

## <a name="interpret-events"></a>Olayları yorumlama
StatefulRunAsyncInvocation, StatefulRunAsyncCompletion ve StatefulRunAsyncCancellation olayların hizmet yazıcı'ne zaman bir hizmeti başlar, iptal eder veya bittikten zamanlama yanı sıra, bir hizmet yaşam döngüsü anlamak için kullanışlıdır. Bu bilgiler aşağıdaki durumlarda kullanışlı olabilir hizmet sorunlarını hata ayıklama veya hizmet yaşam döngüsünü anlayın.

Hizmetiyle ilgili sorunlara belirttiğinden hizmeti yazıcıları Kapat StatefulRunAsyncSlowCancellation ve StatefulRunAsyncFailure olaylara dikkat etmeniz gerekir.

Hizmet RunAsync() görevi aykırı her StatefulRunAsyncFailure yayınlanır. Genellikle, oluşturulan bir özel bir hata veya hizmetinde hata gösterir. Ayrıca, farklı bir düğüme taşınır için özel durum hizmetinin başarısız olmasına neden olur. Bu işlem, pahalı olabilir ve hizmet taşınırken gelen istekleri geciktirebilir. Beklemeleri özel durumun nedeni belirlemek ve mümkünse, bunu azaltmak.

İptal isteği RunAsync görev için dört saniyeden daha uzun sürer her StatefulRunAsyncSlowCancellation yayınlanır. Bir hizmeti iptal tamamlanması çok uzun sürerse, hizmeti başka bir düğümde hızlı bir şekilde yeniden başlatılması yeteneğini etkiler. Bu senaryo hizmetin genel kullanılabilirliğini etkileyebilir.

## <a name="performance-counters"></a>Performans sayaçları
Reliable Services çalışma zamanı aşağıdaki performans sayacı kategorisi tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Service Fabric İşlem Çoğaltıcısı |Azure Service Fabric işlem çoğaltması için özel sayaçlar |
| Service Fabric TStore |Azure Service Fabric TStore özel sayaçlar |

Service Fabric işlem çoğaltması tarafından kullanılan [güvenilir durum Yöneticisi](service-fabric-reliable-services-reliable-collections-internals.md) işlemlerin belirli bir dizi içinden çoğaltmak için [çoğaltmaları](service-fabric-concepts-replica-lifecycle.md).

Service Fabric TStore kullanılan bir bileşen olan [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections-internals.md) depolamak ve almak için kullanılan anahtar-değer çiftleri.

[Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) varsayılan olarak Windows işletim sisteminde kullanılabilir uygulama, toplama ve performans sayacı verilerini görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayaç verileri toplayan ve Azure tablolara karşıya yükleme için başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örneği adları
Çok sayıda güvenilir hizmetler veya güvenilir hizmeti bölümleri olan bir küme, çok sayıda işlem çoğaltması performans sayacı örnekleri sahip olur. Bu ayrıca TStore performans sayaçları için bir durumdur, ancak aynı zamanda güvenilir sözlükler ve güvenilir kullanılan kuyruklar sayısına göre çarpılır. Performans sayacı örneği adları belirli tanımlanmasına yardımcı olabilecek [bölüm](service-fabric-concepts-partitioning.md), hizmet çoğaltma ve performans sayacı örneği ile ilişkili TStore, söz konusu olduğunda durumu sağlayıcısı.

#### <a name="service-fabric-transactional-replicator-category"></a>Service Fabric işlem çoğaltması kategorisi
Kategori için `Service Fabric Transactional Replicator`, sayaç örneği adları aşağıdaki biçimdedir:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionId* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dize gösterimidir. Bölüm kimliği bir GUID olduğundan ve dize gösterimi aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) biçim belirticisi "D" ile.

*ServiceFabricReplicaId* güvenilir bir hizmetin belirli bir çoğaltma ile ilişkili kimliği. Çoğaltma Kimliği benzersizliğini sağlamak ve aynı bölüm için oluşturulan diğer performans sayacı örnekleri ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Çoğaltmaları ve güvenilir Hizmetleri'ndeki rolleri hakkında daha fazla bilgi bulunabilir [burada](service-fabric-concepts-replica-lifecycle.md).

Aşağıdaki sayaç örneği adı altında bir sayaç için tipik `Service Fabric Transactional Replicator` kategorisi:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

Önceki örnekte `00d0126d-3e36-4d68-98da-cc4f7195d85e` Service Fabric bölüm kimliği dize gösterimi ise ve `131652217797162571` çoğaltma kimliğidir.

#### <a name="service-fabric-tstore-category"></a>Service Fabric TStore kategorisi
Kategori için `Service Fabric TStore`, sayaç örneği adları aşağıdaki biçimdedir:

`ServiceFabricPartitionId:ServiceFabricReplicaId:ServiceFabricStateProviderId_PerformanceCounterInstanceDifferentiator`

*ServiceFabricPartitionId* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dize gösterimidir. Bölüm kimliği bir GUID olduğundan ve dize gösterimi aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) biçim belirticisi "D" ile.

*ServiceFabricReplicaId* güvenilir bir hizmetin belirli bir çoğaltma ile ilişkili kimliği. Çoğaltma Kimliği benzersizliğini sağlamak ve aynı bölüm için oluşturulan diğer performans sayacı örnekleri ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Çoğaltmaları ve güvenilir Hizmetleri'ndeki rolleri hakkında daha fazla bilgi bulunabilir [burada](service-fabric-concepts-replica-lifecycle.md).

*ServiceFabricStateProviderId* güvenilir bir hizmet içinde bir durumu sağlayıcısı ile ilişkili kimliği. Durum sağlayıcı kimliği bir TStore birbirinden ayırt etmek için performans sayacı örneği adını dahil edilir.

*PerformanceCounterInstanceDifferentiator* durum sağlayıcısı içinde performans sayacı örneği ile ilişkili bir farklılaştırıcı kimliğidir. Bu fark yatan unsur benzersizliğini sağlamak ve aynı durumu sağlayıcısı tarafından oluşturulan diğer performans sayacı örnekleri ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir.

Aşağıdaki sayaç örneği adı altında bir sayaç için tipik `Service Fabric TStore` kategorisi:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571:142652217797162571_1337`

Önceki örnekte `00d0126d-3e36-4d68-98da-cc4f7195d85e` Service Fabric bölüm kimliği dize gösterimidir `131652217797162571` çoğaltma kimliği `142652217797162571` durumu sağlayıcısı kimliği ve `1337` performans sayacı örneği fark yatan unsur.

### <a name="transactional-replicator-performance-counters"></a>İşlem çoğaltıcı performans sayaçları

Reliable Services çalışma zamanı altında aşağıdaki olaylar yayar `Service Fabric Transactional Replicator` kategorisi

 Sayaç adı | Açıklama |
| --- | --- |
| Saniye Başına Başlatılan İşlemler | Saniye başına oluşturulan yeni yazma işlem sayısı.|
| Saniye Başına İşlemler | Güvenilir derlemeler saniye başına gerçekleştirilen ekleme/güncelleştirme/silme işlemlerinin sayısı.|
| Ort. Flush gecikme süresi (ms) | Saniye başına işlem çoğaltıcı tarafından diske yazılan bayt sayısı |
| Saniye Başına Kısıtlanan İşlem | İşlem sayısı işlem kısıtlama nedeniyle çoğaltıcı tarafından saniyede reddetti. |
| Ort. Hareket ms/kaydetme | Milisaniye cinsinden işlem başına ortalama yürütme gecikmesi |
| Ort. Flush gecikme süresi (ms) | Disk temizleme işlemleri milisaniye cinsinden işlem çoğaltması tarafından başlatılan ortalama süresi |

### <a name="tstore-performance-counters"></a>TStore performans sayaçları

Reliable Services çalışma zamanı altında aşağıdaki olaylar yayar `Service Fabric TStore` kategorisi

 Sayaç adı | Açıklama |
| --- | --- |
| Öğe Sayısı | Depodaki anahtar sayısı.|

## <a name="next-steps"></a>Sonraki adımlar
[PerfView EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
