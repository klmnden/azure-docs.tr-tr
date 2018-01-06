---
title: "Azure olay kılavuz teslim ve yeniden deneyin"
description: "Azure olay kılavuz olayları nasıl sunar ve teslim edilmeyen iletilerini nasıl işlediğini açıklar."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: darosa
ms.openlocfilehash: 4eacb37d6e19b4b69d604aa84fd404479dead1ea
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Olay kılavuz ileti teslimi ve yeniden deneyin 

Bu makalede, Azure olay kılavuz teslim edilmedi olduğunda olayları nasıl işlediğini açıklar.

Olay kılavuz dayanıklı teslim sağlar. Her ileti en az bir kez her abonelik için sunar. Olayları hemen her abonelik kayıtlı Web kancası için gönderilir. Bir Web kancası ilk teslim girişiminin 60 saniye içinde bir olay alınmasını kabul değil, olay kılavuz olay teslimini yeniden dener.

## <a name="message-delivery-status"></a>İleti teslimat durumu

Olay kılavuz HTTP yanıt kodları olayları alınmasını onaylamak için kullanır. 

### <a name="success-codes"></a>Başarı kodları

Aşağıdaki HTTP yanıt kodları, bir olay, Web kancası başarıyla teslim olduğunu gösterir. Olay kılavuz teslim tam olarak değerlendirir.

- 200 TAMAM
- 202 kabul edilen

### <a name="failure-codes"></a>Hata kodları

Aşağıdaki HTTP yanıt kodları, bir olay teslim girişimi başarısız olduğunu gösterir. Olay kılavuz olay göndermek yeniden çalışır. 

- 400 Hatalı istek
- 401 Yetkisiz
- 404 Bulunamadı
- 408 isteği zaman aşımı
- 414 URI çok uzun
- 500 İç sunucu hatası
- 503 Hizmet kullanılamıyor
- 504 ağ geçidi zaman aşımı

Diğer bir yanıt kodu veya bir yanıt eksikliği hata gösterir. Olay kılavuz teslim yeniden dener. 

## <a name="retry-intervals"></a>Aralıkları yeniden deneyin

Olay kılavuz üstel geri alma yeniden deneme ilkesi olay teslimi için kullanır. Web kancası yanıt vermiyor veya bir hata kodu döndürüyor, olay kılavuz şu zamanlamaya göre teslim yeniden deneme:

1. 10 saniye
2. 30 saniye
3. 1 dakika
4. 5 dakika
5. 10 dakika
6. 30 dakika
7. 1 saat

Olay kılavuz, küçük rasgele tüm yeniden deneme aralıkları ekler.

## <a name="retry-duration"></a>Süre yeniden deneyin

Önizleme sırasında Azure olay kılavuz iki saat içinde teslim edilmedi tüm olayları süresi dolar.

## <a name="monitoring"></a>İzleme

Olay teslimler durumunu görmek için portalı kullanabilirsiniz.

Bir olay aboneliği ölçümlerini görmek için arama **olay abonelikleri** kullanılabilir hizmetleri ve seçin.

![Olay abonelikleri arayın](./media/delivery-and-retry/select-event-subscriptions.png)

Olay, abonelik ve konum türüne göre filtreleyin. Seçin **ölçümleri** görüntülemek abonelik için.

![Filtre olay abonelikleri](./media/delivery-and-retry/filter-events.png)

Olay konu ve abonelik için ölçümleri görüntüleyin.

![Olay metrikleri görüntüleyin](./media/delivery-and-retry/subscription-metrics.png)

Özel bir konu yayımladıysanız, ölçümler için görüntüleyebilirsiniz. Konu içeren kaynak grubunu seçin ve konu seçin.

![Özel bir konu seçin](./media/delivery-and-retry/select-custom-topic.png)

Özel olay konu için ölçümleri görüntüleyin.

![Olay metrikleri görüntüleyin](./media/delivery-and-retry/custom-topic-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).