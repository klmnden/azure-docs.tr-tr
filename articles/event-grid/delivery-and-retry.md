---
title: Azure Event Grid teslim ve yeniden deneyin
description: Azure Event Grid olayların nasıl sunar ve teslim edilmeyen iletilerini nasıl işlediğini açıklar.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: tomfitz
ms.openlocfilehash: b34386a7b416d6f7d8b008a9cb5ef142948a370f
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005404"
---
# <a name="event-grid-message-delivery-and-retry"></a>Event Grid iletiyi teslim ve yeniden deneyin 

Bu makalede, Azure Event Grid olay teslimi onaylanır değil nasıl işlediğini açıklar.

Event Grid, sürekli teslimi sağlar. Bu, her ileti her abonelik için en az bir kez sunar. Her bir aboneliğe kayıtlı Web kancası için olaylar hemen gönderilir. Bir Web kancası olay alınmasını 60 saniye içinde ilk teslim denemesi kabul değil, Event Grid olay teslimini yeniden dener. 

Şu anda Event Grid her olay için aboneleri ayrı ayrı gönderir. Abone ile tek bir olay dizisi alır.

## <a name="message-delivery-status"></a>İleti teslim durumu

Event Grid, olayları alındığını onaylamak için HTTP yanıt kodları kullanır. 

### <a name="success-codes"></a>Başarılı kodları

Aşağıdaki HTTP yanıt kodları, bir olay için Web kancası başarıyla teslim olduğunu gösterir. Event Grid teslim tam olarak değerlendirir.

- 200 TAMAM
- 202 kabul edildi

### <a name="failure-codes"></a>Hata kodları

Aşağıdaki HTTP yanıt kodları olay teslim denemesi başarısız olduğunu gösterir. 

- 400 Hatalı istek
- 401 Yetkisiz
- 404 Bulunamadı
- 408 istek zaman aşımı
- 414 URI çok uzun
- 429 çok fazla istek
- 500 İç Sunucu Hatası
- 503 Hizmet Kullanılamıyor
- 504 Ağ Geçidi Zaman Aşımı

Event Grid alırsa, uç nokta belirten bir hata geçici olarak kullanılamıyor veya gelecekteki bir istek başarılı olabilir, olay göndermek yeniden çalışır. Event Grid teslim hiçbir zaman başarılı olur belirten bir hata alırsa ve [edilemeyen uç nokta yapılandırıldı](manage-event-delivery.md), olay edilemeyen uç noktasına gönderir. 

## <a name="retry-intervals-and-duration"></a>Yeniden deneme aralıkları ve süresi

Event Grid olay teslimi için bir üstel geri alma yeniden deneme ilkesi kullanır. Event Grid, Web kancası yanıt vermiyor veya bir hata kodu döndürüyor, teslim aşağıdaki zamanlamaya göre yeniden deneme:

1. 10 saniye
2. 30 saniye
3. 1 dakika
4. 5 dakika
5. 10 dakika
6. 30 dakika
7. 1 saat

Event Grid, tüm yeniden deneme aralıkları için küçük bir rastgele seçim ekler. Bir saat sonra olay teslimi saatte bir kez yeniden denendi.

Varsayılan olarak, Event Grid, 24 saat içinde teslim olmayan tüm olayların süresi dolar. Yapabilecekleriniz [yeniden deneme ilkesi özelleştirme](manage-event-delivery.md) bir olay aboneliği oluştururken. Yaşam süresi (varsayılan değer 30) teslim denemesi ve olay sayısını sağlar (varsayılan değer 1440 dakika).

## <a name="dead-letter-events"></a>Teslim edilemeyen olayları

Event Grid olay teslim edilemiyor, bir depolama hesabına teslim edilmeyen olay gönderebilirsiniz. Bu işlem, ulaşmayan olarak bilinir. Teslim edilmeyen olayları görmek için bunları edilemeyen konumdan çekebilirsiniz. Daha fazla bilgi için [kullanılmayan harf ve yeniden deneme ilkelerine](manage-event-delivery.md).

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimler durumunu görüntülemek için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Olay teslimi seçenekleri özelleştirmek için bkz: [yönetme Event Grid teslim ayarları](manage-event-delivery.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).