---
title: Azure Service Fabric olay Store | Microsoft Docs
description: Azure Service Fabric'in Eventstore'a hakkında bilgi edinin
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: cd58e24a51b153d6bf217f7d32a82e882183ed73
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290680"
---
# <a name="eventstore-service-overview"></a>Eventstore'a hizmetine genel bakış

## <a name="overview"></a>Genel Bakış

6.2 sürümünde sunulan Eventstore'a zaman küme veya iş yüklerini belirli bir noktada durumunu öğrenmek bir yol sağlayan Service fabric'te izleme bir seçenek hizmetidir. Eventstore'a hizmet, bir dizi API'si (REST uç noktaları aracılığıyla veya istemci kitaplığı aracılığıyla erişilebilir) aracılığıyla Service Fabric olayları gösterir. Bu EventStore API'leri yardımcı olmak için kullanılmalıdır ve doğrudan herhangi bir varlık, kümenizdeki ilgili tanılama verilerini almak için küme sorgu için izin ver:

* Geliştirme veya test sorunları tanılayın ve burada izleme bir işlem hattı kullanıyor olabilir
* Yönetim eylemleri kümenizde sürüp doğru kümeniz tarafından işlenen onaylayın

Eventstore'a içinde kullanılabilir olayların tam listesi için bkz [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>Service Fabric 6.2 sürümü itibarıyla. EventStore API'leri şu anda yalnızca Azure üzerinde çalışan Windows kümeleri için Önizleme. Bu işlev, tek başına kümeler yanı sıra Linux taşıma üzerinde çalışıyoruz.

Eventstore'a hizmet, her varlık ve kümenizde varlık türü için kullanılabilir olan olaylar için sorgulanabilir. Başka bir deyişle, olayları aşağıdaki düzeyleri için sorgulama yapabilirsiniz;
* Küme: tüm küme düzeyinde olaylar
* Düğüm: tüm düğüm düzeyinde olaylar
* Düğüm: bir düğüme göre belirli olayları `nodeName`
* Uygulamalar: tüm uygulama düzeyinde olaylar
* Uygulama: tek bir uygulama için belirli olayları
* Hizmetleri: tüm kümelerinizi Hizmetleri'nde olaylardan
* Hizmet: belirli bir hizmet sağlayıcısından olayları
* Bölümler: tüm bölümleri olaylardan
* Bölüm: belirli bir bölüm olayları
* Çoğaltmaları: tüm çoğaltmaların olaylar / örnekleri
* Çoğaltma: belirli bir çoğaltma olaylardan / örnek


Eventstore'a hizmeti, ayrıca, kümenizdeki olayları ilişkilendirmenize özelliğine sahiptir. Aynı anda birbirine etkilemiş olabilecek farklı varlıklardan yazılmış olayları bakarak, Eventstore'a hizmet bu olaylar, etkinlikler, kümenizdeki nedenleri tanımlamaya yardımcı olmak için bağlantı kuramıyor. Uygulamalarınızdan biri uyarılmış hiçbir değişiklik yapmadan sistem durumu ortaya çıkarsa gibi Eventstore'a ayrıca diğer olayları göz platform tarafından sunulan ve bununla ilişkilendirin mı bir `NodeDown` olay. Bu, daha hızlı hata algılama ile yardımcı olur ve kök neden analizi.

Biz Eventstore'a hızlı analiz ve kümenizin nasıl çalıştığını bir anlık görüntü fikir almak için kullanılması önerilir ve olarak şeyler varsa bekleniyor.

Eventstore'a hizmeti kullanmaya başlamak için bkz. [küme olayları için sorgu EventStore API'leri](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Sonraki adımlar
* İzleme ve tanılama Service fabric'te - genel bakış [Service Fabric için izleme ve tanılama](service-fabric-diagnostics-overview.md)
* Kümenizi izleme hakkında daha fazla bilgi- [platform ve küme izleme](service-fabric-diagnostics-event-generation-infra.md).