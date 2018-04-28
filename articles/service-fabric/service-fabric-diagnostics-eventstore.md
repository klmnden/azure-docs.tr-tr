---
title: Azure Service Fabric olay deposuna | Microsoft Docs
description: Azure Service Fabric'ın EventStore hakkında bilgi edinin
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 7e401a310ce9a5f59473353227a9ce36767aac3c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="eventstore-service-overview"></a>EventStore hizmetine genel bakış

>[!NOTE]
>Service Fabric sürüm 6.2 itibariyle. EventStore apı'leridir şu anda yalnızca Azure üzerinde çalışan Windows kümeleri için önizlemede. Bu işlevsellik, tek başına kümelerinin yanı sıra Linux taşıma üzerinde çalışıyoruz.

## <a name="overview"></a>Genel Bakış

Sürüm 6.2 çıkan, EventStore zamanında küme veya belirli bir noktada iş yükleri durumunu anlamak bir yol sunar Service Fabric izleme seçeneğinde hizmetidir. EventStore hizmeti çağrıları yapabileceğiniz API'leri aracılığıyla Service Fabric olayları gösterir. Bu EventStore API'leri ve yardımcı olmak için kullanılması gereken doğrudan, kümedeki herhangi bir varlığa tanılama verilerini almak için küme sorgu için izin ver:
* Geliştirme veya test sorunları tanılamak veya burada izleme ardışık düzen kullanıyor olabilir
* Kümenizde kaplayan yönetim eylemleri doğru kümeniz tarafından işlenen onaylayın

EventStore kullanılabilir olayları tam listesini görmek için bkz: [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md).

EventStore hizmeti, her bir varlık ve kümenizdeki varlık türü için kullanılabilir olan olaylar için sorgulanabilir. Bu, aşağıdaki düzeyleri olaylarına sorgulayabilirsiniz anlamına gelir;
* Küme: tüm düzeyi olayları küme
* Düğüm: tüm düğüm düzeyi olayları
* Düğüm: bir düğüme göre belirli olaylar `nodeName`
* Uygulamalar: tüm uygulama düzeyi olayları
* Uygulama: tek bir uygulama için belirli olayları
* Hizmetleri: tüm hizmetlerin kümelerinizi olayları
* Hizmeti: belirli bir hizmet olaylarından
* Bölümler: tüm bölümleri olayları
* Bölümü: belirli bir bölüm olayları
* Çoğaltmaları: tüm çoğaltmaların olaylar / örnekleri
* Çoğaltma: olayları belirli bir çoğaltma / örnek


EventStore hizmeti, aynı zamanda kümenizdeki olayları ilişkilendirmenize olanak silebilir. Aynı anda birbirine etkilenmiş farklı varlıklardan yazılmış olayları bakarak, EventStore hizmet kümenizdeki etkinlikleri nedenleri tanımlamaya yardımcı olması için bu olayları bağlamak kullanabilirsiniz. Uygulamalarınızdan birini uyarılmış hiçbir değişiklik yapmadan sağlıksız olmasını olursa, örneğin, EventStore ayrıca diğer olayları göz platform tarafından kullanıma sunulan ve bu ile ilişkilendirmek olur bir `NodeDown` olay. Bu daha hızlı hatası algılama yardımcı olur ve analiz kök neden olur.

EventStore hizmeti kullanmaya başlamak için bkz: [küme olayları için sorgu EventStore API'leri](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Sonraki adımlar
* İzleme ve Tanılama, Service Fabric - genel bakış [izleme ve tanılama Service Fabric için](service-fabric-diagnostics-overview.md)
* Kümenizi izleme hakkında daha fazla bilgi- [küme ve platform izleme](service-fabric-diagnostics-event-generation-infra.md).