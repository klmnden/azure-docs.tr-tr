---
title: Azure olay kılavuz teslim ve yeniden deneyin
description: Azure olay kılavuz olayları nasıl sunar ve teslim edilmeyen iletilerini nasıl işlediğini açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/17/2018
ms.author: tomfitz
ms.openlocfilehash: 017cb5850788bd230c4a4ba256997f2776c07bec
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Olay kılavuz ileti teslimi ve yeniden deneyin 

Bu makalede, Azure olay kılavuz teslim edilmedi olduğunda olayları nasıl işlediğini açıklar.

Olay kılavuz dayanıklı teslim sağlar. Her ileti en az bir kez her abonelik için sunar. Olayları hemen her abonelik kayıtlı Web kancası için gönderilir. Bir Web kancası ilk teslim girişiminin 60 saniye içinde bir olay alınmasını kabul değil, olay kılavuz olay teslimini yeniden dener. 

Şu anda, olay kılavuz her olay abonelere ayrı ayrı gönderir. Abone olan tek bir olay bir dizi alır.

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
- 500 İç Sunucu Hatası
- 503 Hizmet Kullanılamıyor
- 504 Ağ Geçidi Zaman Aşımı

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

Olay kılavuz, küçük rasgele tüm yeniden deneme aralıkları ekler. Bir saat sonra saatte bir olay teslimi denenir.

## <a name="retry-duration"></a>Süre yeniden deneyin

Azure olay kılavuz 24 saat içinde teslim edilmedi tüm olayları süresi dolar.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimler durumunu görüntülemek için bkz: [İzleyicisi olay kılavuz ileti teslimi](monitor-event-delivery.md).
* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).