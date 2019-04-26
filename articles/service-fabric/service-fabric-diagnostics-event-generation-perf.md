---
title: Azure Service Fabric performans izleme | Microsoft Docs
description: İzleme ve Tanılama, Azure Service Fabric kümeleri için performans sayaçları hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: ee1608c40801f568b38ace4670b0d5ea7f73003c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392900"
---
# <a name="performance-metrics"></a>Performans ölçümleri

Kümenizi ve bunun yanı sıra içinde çalışan uygulamaların performansının anlaşılması toplanan ölçümler. Service Fabric kümeleri için aşağıdaki performans sayaçlarını toplamayı öneririz.

## <a name="nodes"></a>Düğümler

Kümenizdeki makineleri için daha iyi her makinede yük anlamak ve uygun küme kararları ölçeklendirme yapmak için aşağıdaki performans sayaçlarını toplamayı göz önünde bulundurun.

| Sayaç kategorisi | Sayaç Adı |
| --- | --- |
| Mantıksal Disk | Mantıksal Disk boş alan |
| PhysicalDisk (başına Disk) | Ort. Disk okuma kuyruğu uzunluğu |
| PhysicalDisk (başına Disk) | Ort. Disk yazma kuyruğu uzunluğu |
| PhysicalDisk (başına Disk) | Ort. Disk sn/Okuma |
| PhysicalDisk (başına Disk) | Ort. Disk sn/yazma |
| PhysicalDisk (başına Disk) | Disk Okuma/sn |
| PhysicalDisk (başına Disk) | Disk Okuma Bayt/sn |
| PhysicalDisk (başına Disk) | Disk Yazma/sn |
| PhysicalDisk (başına Disk) | Disk Yazma Bayt/sn |
| Bellek | Kullanılabilir MBayt |
| PagingFile | Kullanım yüzdesi |
| Processor(Total) | % İşlemci zamanı |
| İşlem (hizmet başına) | % İşlemci zamanı |
| İşlem (hizmet başına) | İşlem kimliği |
| İşlem (hizmet başına) | Özel Baytlar |
| İşlem (hizmet başına) | İş Parçacığı Sayısı |
| İşlem (hizmet başına) | Sanal bayt sayısı |
| İşlem (hizmet başına) | Çalışma kümesi |
| İşlem (hizmet başına) | Çalışma kümesi - özel |
| Ağ Interface(all-instances) | Bayt recd |
| Ağ Interface(all-instances) | Gönderilen bayt sayısı |
| Ağ Interface(all-instances) | Toplam bayt |
| Ağ Interface(all-instances) | Çıkış sırası uzunluğu |
| Ağ Interface(all-instances) | Atılan çıkış paketlerinin |
| Ağ Interface(all-instances) | Atılan alınan paketler |
| Ağ Interface(all-instances) | Giden paket hataları |
| Ağ Interface(all-instances) | Hataları alınan paketler |

## <a name="net-applications-and-services"></a>.NET uygulamaları ve Hizmetleri

.NET Hizmetleri kümenize dağıtıyorsanız aşağıdaki sayaçlarını topla. 

| Sayaç kategorisi | Sayaç Adı |
| --- | --- |
| .NET CLR bellek (hizmet başına) | İşlem Kimliği |
| .NET CLR bellek (hizmet başına) | # Toplam kaydedilmiş bayt |
| .NET CLR bellek (hizmet başına) | # Toplam bayt ayrılmış |
| .NET CLR bellek (hizmet başına) | # Tüm yığınlardaki bayt |
| .NET CLR bellek (hizmet başına) | Büyük nesne yığın boyutu |
| .NET CLR bellek (hizmet başına) | # GC işleme |
| .NET CLR bellek (hizmet başına) | # Gen 0 toplamaları sayısı |
| .NET CLR bellek (hizmet başına) | # Gen 1 toplamaları sayısı |
| .NET CLR bellek (hizmet başına) | # Gen 2 toplamaları sayısı |
| .NET CLR bellek (hizmet başına) | Gc'de zaman |

### <a name="service-fabrics-custom-performance-counters"></a>Service Fabric'in özel performans sayaçları

Service Fabric özel performans Sayaçlarınızı önemli miktarda oluşturur. SDK'ın yüklü varsa, kapsamlı bir liste, Windows makinenizde Performans İzleyicisi uygulamanızda görebilirsiniz (Başlat > Performans İzleyicisi). 

Uygulamalarda sayaçları ekleyin dağıttığınız kümenize, Reliable Actors kullanıyorsanız `Service Fabric Actor` ve `Service Fabric Actor Method` kategorileri (bkz [Service Fabric güvenilir aktör tanılama](service-fabric-reliable-actors-diagnostics.md)).

Reliable Services veya uzaktan hizmet iletişimini kullanıyorsanız benzer şekilde sahibiz `Service Fabric Service` ve `Service Fabric Service Method` sayaç kategorileri, bkz: sayaçlarını Topla [ile uzaktan hizmet iletişimini izleme](service-fabric-reliable-serviceremoting-diagnostics.md) ve [güvenilir performans sayaçları Hizmetleri](service-fabric-reliable-services-diagnostics.md#performance-counters). 

Reliable Collections kullanımı, eklemenizi öneririz `Avg. Transaction ms/Commit` gelen `Service Fabric Transactional Replicator` işlem ölçüm başına ortalama yürütme gecikmesi toplanacak.


## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [olay oluşturma platformu düzeyinde](service-fabric-diagnostics-event-generation-infra.md) Service fabric'te
* Performans ölçümlerini aracılığıyla [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md)
