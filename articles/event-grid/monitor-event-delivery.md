---
title: "Azure olay kılavuz ileti teslimi izleme"
description: "Azure olay kılavuz iletilerin teslimini izlemek açıklar."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/10/2018
ms.author: tomfitz
ms.openlocfilehash: 1552174589e91c370c3b85a9af7b5cc1106cbb90
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
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
