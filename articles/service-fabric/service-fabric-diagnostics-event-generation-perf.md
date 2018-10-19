---
title: Azure Service Fabric performans izleme | Microsoft Docs
description: İzleme ve Tanılama, Azure Service Fabric kümeleri için performans sayaçları hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 1e6ea5d6ae321a0443631ec928912611a68346c6
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408022"
---
# <a name="performance-metrics"></a>Performans ölçümleri

Kümenizi ve bunun yanı sıra içinde çalışan uygulamaların performansının anlaşılması toplanan ölçümler. Service Fabric kümeleri için aşağıdaki performans sayaçlarını toplamayı öneririz.

## <a name="nodes"></a>Düğümler

Kümenizdeki makineleri için daha iyi her makinede yük anlamak ve uygun küme kararları ölçeklendirme yapmak için aşağıdaki performans sayaçlarını toplamayı göz önünde bulundurun.

| Sayaç kategorisi | Sayaç Adı |
| --- | --- |
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
| .NET CLR bellek (hizmet başına) | # Gen 0 toplamaları sayısı |
| .NET CLR bellek (hizmet başına) | # Gen 1 toplamaları sayısı |
| .NET CLR bellek (hizmet başına) | # Gen 2 toplamaları sayısı |
| .NET CLR bellek (hizmet başına) | Gc'de zaman |

### <a name="service-fabrics-custom-performance-counters"></a>Service Fabric'in özel performans sayaçları

Service Fabric özel performans Sayaçlarınızı önemli miktarda oluşturur. SDK'ın yüklü varsa, kapsamlı bir liste, Windows makinenizde Performans İzleyicisi uygulamanızda görebilirsiniz (Başlat > Performans İzleyicisi). 

Uygulamalarda Reliable Actors kullanıyorsanız, kümenize dağıtıyorsanız, gelen countes Ekle `Service Fabric Actor` ve `Service Fabric Actor Method` kategorileri (bkz [Service Fabric güvenilir aktör tanılama](service-fabric-reliable-actors-diagnostics.md)).

Reliable Services kullanıyorsanız, benzer şekilde sahibiz `Service Fabric Service` ve `Service Fabric Service Method` toplamanız gerekir sayaç kategorileri sayaçları öğesinden. 

Reliable Collections kullanımı, eklemenizi öneririz `Avg. Transaction ms/Commit` gelen `Service Fabric Transactional Replicator` işlem ölçüm başına ortalama yürütme gecikmesi toplanacak.


## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [olay oluşturma platformu düzeyinde](service-fabric-diagnostics-event-generation-infra.md) Service fabric'te
* Performans ölçümlerini aracılığıyla [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md)
