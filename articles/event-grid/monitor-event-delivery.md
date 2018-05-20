---
title: Azure olay kılavuz ileti teslimi izleme
description: Azure olay kılavuz iletilerin teslimini izlemek açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: f9719bb1f1563c55537c7ef32278411a2034bd75
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="monitor-event-grid-message-delivery"></a>Olay kılavuz ileti teslimi izleme 

Bu makalede, portal olay teslimler durumunu görmek için kullanmayı açıklar.

Olay kılavuz dayanıklı teslim sağlar. Her ileti en az bir kez her abonelik için sunar. Olayları hemen her abonelik kayıtlı Web kancası için gönderilir. Bir Web kancası ilk teslim girişiminin 60 saniye içinde bir olay alınmasını kabul değil, olay kılavuz olay teslimini yeniden dener.

Olay teslimi ve yeniden denemeleri hakkında bilgi için [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Teslim ölçümleri

Portal, ölçümleri için olay iletileri teslim etme durumunu görüntüler.

Konular için ölçümler şunlardır:

* **Yayımlama başarılı**: olay başarıyla konu başlığına gönderilen ve 2xx yanıtta işlenebilir.
* **Başarısız yayımlama**: olay konu başlığına gönderilen ancak hata koduyla reddetti.
* **Eşleşmeyen**: olay başarıyla konu başlığında yayımlanan, ancak bir olay aboneliği eşleşen değil. Olay bırakıldı.

Abonelikler için ölçümler şunlardır:

* **Teslim başarılı**: olay başarıyla aboneliğin uç noktasına teslim ve 2xx bir yanıt aldı.
* **Teslim başarısız**: olay aboneliğin uç noktasına gönderilen ancak 4xx veya 5xx bir yanıt alındı.
* **Olayların süresi**: olay teslim ve tüm denemeleri gönderildi. Olay bırakıldı.
* **Olayları eşleşen**: konusunda olay Olay aboneliği ile eşleşen.

## <a name="event-subscription-status"></a>Olay aboneliği durumu

Bir olay aboneliği ölçümlerini görmek için arama **olay kılavuz abonelikleri** kullanılabilir hizmetleri ve seçin.

![Olay abonelikleri arayın](./media/monitor-event-delivery/select-event-subscriptions.png)

Olay, abonelik ve konum türüne göre filtreleyin. Seçin **ölçümleri** görüntülemek abonelik için.

![Filtre olay abonelikleri](./media/monitor-event-delivery/filter-events.png)

Olay konu ve abonelik için ölçümleri görüntüleyin.

![Olay metrikleri görüntüleyin](./media/monitor-event-delivery/subscription-metrics.png)

## <a name="custom-event-status"></a>Özel olay durumu

Özel bir konu yayımladıysanız, ölçümler için görüntüleyebilirsiniz. Konu içeren kaynak grubunu seçin ve konu seçin.

![Özel bir konu seçin](./media/monitor-event-delivery/select-custom-topic.png)

Özel olay konu için ölçümleri görüntüleyin.

![Olay metrikleri görüntüleyin](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).
* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
