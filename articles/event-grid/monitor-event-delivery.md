---
title: Azure Event Grid ileti teslimi izleme
description: Azure Event Grid iletileri dağıtımını izlemek açıklar.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: spelluru
ms.openlocfilehash: fdd18b833794c25cb90188ba8bc418d4785492ba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60824176"
---
# <a name="monitor-event-grid-message-delivery"></a>Event Grid ileti teslimi izleme 

Bu makalede, olay teslimler durumunu görmek için portalı kullanmayı açıklar.

Event Grid, sürekli teslimi sağlar. Bu, her ileti her abonelik için en az bir kez sunar. Her bir aboneliğe kayıtlı Web kancası için olaylar hemen gönderilir. Bir Web kancası olay alınmasını 60 saniye içinde ilk teslim denemesi kabul değil, Event Grid olay teslimini yeniden dener.

Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Teslim ölçümleri

Portal, ölçümleri için olay iletileri teslim etme durumunu görüntüler.

Konular için ölçümler şunlardır:

* **Yayımlama başarılı**: Olay başarıyla konu başlığına gönderilen ve 2xx yanıtıyla işlenir.
* **Yayımlama başarısız oldu**: Olay konu başlığına gönderilen, ancak bir hata koduyla reddetti.
* **Eşleşmeyen**: Olay başarıyla konu başlığında yayımlanan ancak olay aboneliği karşılaştırılamıyor. Olay atlandı.

Abonelikler için ölçümler şunlardır:

* **Teslimat başarılı**: Olay başarıyla aboneliğin uç noktasına teslim ve 2xx bir yanıt aldı.
* **Teslim başarısız**: Olay aboneliğinin uç noktaya gönderdi, ancak 4xx veya 5xx bir yanıt aldı.
* **Olayların süresi**: Olay teslim edilmemiş ve tüm yeniden deneme girişimleri gönderildi. Olay atlandı.
* **Eşleşen olaylar**: Olay konusunda olay aboneliği tarafından eşleştirildi.

## <a name="event-subscription-status"></a>Olay aboneliği durumu

İçin bir olay aboneliği ölçümleri görmek için ya da abonelik türü veya abonelikleri için belirli bir kaynağa göre arama yapabilirsiniz.

Olay abonelik türüne göre aramak için seçin **tüm hizmetleri**.

![Tüm hizmetleri seçin](./media/monitor-event-delivery/all-services.png)

Arama **event grid'i** seçip **Event Grid abonelikleri** kullanılabilir seçeneklerden.

![Olay abonelikleri arayın](./media/monitor-event-delivery/search-and-select.png)

Olay, abonelik ve konum türüne göre filtreleyin. Seçin **ölçümleri** görüntülemek abonelik için.

![Olay abonelikleri Filtrele](./media/monitor-event-delivery/filter-events.png)

Olay konu ve abonelik için ölçümleri görüntüleyin.

![Olay ölçümlerini görüntüleme](./media/monitor-event-delivery/subscription-metrics.png)

Belirli bir kaynak için ölçüler bulmak için bu kaynak seçin. Ardından, **olayları**.

![Bir kaynak için olayları seçin](./media/monitor-event-delivery/select-events.png)

Bu kaynak için abonelikler için ölçümleri görürsünüz.

## <a name="custom-event-status"></a>Özel olay durumu

Özel bir konu yayımladıysanız, bunun için ölçümleri görüntüleyebilir. Konu için kaynak grubunu seçin ve konuyu seçin.

![Özel konu seçme](./media/monitor-event-delivery/select-custom-topic.png)

Özel olay konu ölçümleri görüntüleyin.

![Olay ölçümlerini görüntüleme](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
