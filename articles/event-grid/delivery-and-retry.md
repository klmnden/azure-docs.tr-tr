---
title: Azure Event Grid teslim ve yeniden deneyin
description: Azure Event Grid olayların nasıl sunar ve teslim edilmeyen iletilerini nasıl işlediğini açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: spelluru
ms.openlocfilehash: b69215a76b332db9b994827705d6bbc3b48af5c8
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465522"
---
# <a name="event-grid-message-delivery-and-retry"></a>Event Grid iletiyi teslim ve yeniden deneyin

Bu makalede, Azure Event Grid olay teslimi onaylanır değil nasıl işlediğini açıklar.

Event Grid, sürekli teslimi sağlar. Bu, her ileti her abonelik için en az bir kez sunar. Olay aboneliğinin kayıtlı uç noktaya hemen gönderilir. Bir uç nokta bir olayın giriş kabul etmez, Event Grid olay teslimini yeniden dener.

Şu anda Event Grid her olay için aboneleri ayrı ayrı gönderir. Abone ile tek bir olay dizisi alır.

## <a name="retry-schedule-and-duration"></a>Yeniden deneme zamanlaması ve süresi

Event Grid olay teslimi için bir üstel geri alma yeniden deneme ilkesi kullanır. Bir uç nokta yanıt vermiyor veya bir hata kodu döndürüyor, Event Grid teslim aşağıdaki zamanlamaya göre yeniden deneme:

1. 10 saniye
2. 30 saniye
3. 1 dakika
4. 5 dakika
5. 10 dakika
6. 30 dakika
7. 1 saat

Event Grid, tüm yeniden deneme adımları küçük rastgele ekler. Bir saat sonra olay teslimi saatte bir kez yeniden denendi.

Varsayılan olarak, Event Grid, 24 saat içinde teslim olmayan tüm olayların süresi dolar. Yapabilecekleriniz [yeniden deneme ilkesi özelleştirme](manage-event-delivery.md) bir olay aboneliği oluştururken. Yaşam süresi (varsayılan değer 30) teslim denemesi ve olay sayısını sağlar (varsayılan değer 1440 dakika).

## <a name="dead-letter-events"></a>Teslim edilemeyen olayları

Event Grid olay teslim edilemiyor, bir depolama hesabına teslim edilmeyen olay gönderebilirsiniz. Bu işlem, ulaşmayan olarak bilinir. Varsayılan olarak, Event Grid hakkında ulaşmayan kapatmaz. Bunu etkinleştirmek için olay abonelik oluştururken teslim edilmeyen olayları barındıracak bir depolama hesabı belirtmeniz gerekir. Olayları teslim çözmek için bu depolama hesabından çekin.

Tüm yeniden deneme girişimlerinin denedi event grid'i edilemeyen konumuna bir olay gönderir. Event Grid 413 (istek varlığı çok büyük) yanıt kodu 400 (Hatalı istek) ya da alırsa, hemen edilemeyen uç noktaya olay gönderir. Olay teslimini hiçbir zaman başarılı olur, bu yanıt kodları belirtin.

Bir olay ve ne zaman teslim edilemeyen konumuna teslim sunmak için son girişimi arasındaki beş dakikalık bir gecikme yoktur. Bu gecikme, çok sayıda Blob Depolama işlemi azaltmak için tasarlanmıştır. Teslim edilemeyen konumu için dört saat kullanılamıyorsa, olay bırakılır.

Teslim edilemeyen konumu ayarlamadan önce bir kapsayıcı ile bir depolama hesabı olması gerekir. Olay aboneliği oluştururken uç noktası için bu kapsayıcı sağlayın. Uç nokta biçimindedir: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Bir olay edilemeyen konumuna gönderildiğinde size bildirilmesini isteyebilirsiniz. Event Grid teslim edilmeyen olaylara yanıt vermesi için kullanılacak [bir olay aboneliği oluşturmak](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) edilemeyen blob depolama için. Her zaman teslim edilmeyen bir olay edilemeyen blob depolama alanınızın alır, Event Grid işleyicinizi bildirir. İşleyici teslim edilmeyen olayları karşılaştırma için almak istediğiniz eylemi ile yanıt verir.

Atılacak Mektubu konumu ayarlama örneği için bkz [kullanılmayan harf ve yeniden deneme ilkelerine](manage-event-delivery.md).

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
- 413 istek varlığı çok büyük
- 414 URI çok uzun
- 429 çok fazla istek
- 500 İç Sunucu Hatası
- 503 Hizmet Kullanılamıyor
- 504 Ağ Geçidi Zaman Aşımı

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimler durumunu görüntülemek için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Olay teslimi seçenekleri özelleştirmek için bkz: [kullanılmayan harf ve yeniden deneme ilkelerine](manage-event-delivery.md).