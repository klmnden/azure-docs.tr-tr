---
title: Azure Event Grid teslim ve yeniden deneyin
description: Azure Event Grid olayların nasıl sunar ve teslim edilmeyen iletilerini nasıl işlediğini açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: spelluru
ms.openlocfilehash: 0945b06f78ac34500f0b16a4a419cff12d1a4734
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812916"
---
# <a name="event-grid-message-delivery-and-retry"></a>Event Grid iletiyi teslim ve yeniden deneyin

Bu makalede, Azure Event Grid olay teslimi onaylanır değil nasıl işlediğini açıklar.

Event Grid, sürekli teslimi sağlar. Bu, her ileti her abonelik için en az bir kez sunar. Olay aboneliğinin kayıtlı uç noktaya hemen gönderilir. Bir uç nokta bir olayın giriş kabul etmez, Event Grid olay teslimini yeniden dener.

Şu anda Event Grid her olay için aboneleri ayrı ayrı gönderir. Abone ile tek bir olay dizisi alır.

## <a name="retry-schedule-and-duration"></a>Yeniden deneme zamanlaması ve süresi

Event Grid, bir iletiyi teslim sonra 30 saniye için bir yanıt bekler. Uç nokta yanıtlamamışsa 30 saniye sonra yeniden deneme için ileti kuyruğa alınır. Event Grid olay teslimi için bir üstel geri alma yeniden deneme ilkesi kullanır. Event Grid teslim en iyi çaba ilkesine göre aşağıdaki zamanlamaya göre yeniden deneme sayısı:

- 10 saniye
- 30 saniye
- 1 dakika
- 5 dakika
- 10 dakika
- 30 dakika
- 1 saat
- Saatlik 24 saate kadar

Uç nokta 3 dakika içinde yanıt verirse, Event Grid olayı en iyi çaba ilkesine göre yeniden deneme kuyruğu'ndan kaldırmayı dener ancak yinelenenler hala alınan.

Event Grid, tüm yeniden deneme adımları küçük rastgele ekler ve bir uç nokta için uzun bir süre kapalı tutarlı bir şekilde sağlam değil veya kısası gibi görünüyor, belirli bir yeniden deneme desteklemedikleri atlayabilirsiniz.

Olay süresi belirleyici davranışını ayarlayın ve en yüksek teslimat çalışır [aboneliği yeniden deneme ilkeleri](manage-event-delivery.md).

Varsayılan olarak, Event Grid, 24 saat içinde teslim olmayan tüm olayların süresi dolar. Yapabilecekleriniz [yeniden deneme ilkesi özelleştirme](manage-event-delivery.md) bir olay aboneliği oluştururken. Yaşam süresi (varsayılan değer 30) teslim denemesi ve olay sayısını sağlar (varsayılan değer 1440 dakika).

## <a name="delayed-delivery"></a>Gecikmeli teslim

Bir uç nokta teslim hataları deneyimleri gibi Event Grid teslim ve yeniden deneme olayların Bu uç noktaya gecikme başlar. Bir uç noktasına yayımlanan ilk on olaylar başarısız olursa, örneğin, Event Grid uç nokta sorun yaşıyor ve tüm sonraki yeniden denemeler geciktirir varsayar *ve yeni* teslimler için birkaç saat kadar bazı durumlarda biraz zaman - .

Event Grid sistem yanı sıra, iyi durumda olmayan uç noktaları korumak için Gecikmeli teslim işlevsel amacı olan. Geri alma ve teslim sağlıksız uç gecikme, Event Grid'ın yeniden deneme ilkesi ve birim özellikleri kolayca bir sistem sık zora sokar.

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

Event Grid göz önünde bulundurur **yalnızca** başarılı teslim olarak aşağıdaki HTTP yanıt kodları. Tüm diğer durum kodları başarısız teslimler olarak kabul edilir ve yeniden denenecek veya deadlettered uygun şekilde. Başarılı durum kodu aldıktan sonra Event Grid teslim tam olarak değerlendirir.

- 200 TAMAM
- 201 oluşturuldu
- 202 kabul edildi
- 203 onaylı olmayan bilgiler
- 204 No Content

### <a name="failure-codes"></a>Hata kodları

Yukarıdaki kümesi (200-204) değil, diğer tüm kodlar hata olarak kabul edilir ve yeniden denenecek. Bazı belirli yeniden deneme ilkeleri bunları aşağıda ana hatlarıyla belirtilen bağlı olan, diğer tüm standart üstel geri alma modelini izler. Event Grid'ın mimarisi son derece paralel yapısı nedeniyle, yeniden deneme davranışı belirleyici olduğunu aklınızda tutmanız önemlidir. 

| Durum kodu | Yeniden deneme davranışı |
| ------------|----------------|
| 400 Hatalı istek | 5 dakika veya daha sonra yeniden deneyin (teslim edilemeyen iletiler, hemen teslim edilemeyen iletiler kurulum) |
| 401 Yetkisiz | 5 dakika veya daha sonra yeniden deneyin. |
| 403 Yasak | 5 dakika veya daha sonra yeniden deneyin. |
| 404 Bulunamadı | 5 dakika veya daha sonra yeniden deneyin. |
| 408 İstek Zaman Aşımı | 2 dakika sonra yeniden deneyin veya daha fazla bilgi |
| 413 istek varlığı çok büyük | 10 saniye veya daha sonra yeniden deneyin (teslim edilemeyen iletiler, hemen teslim edilemeyen iletiler kurulum) |
| 503 Hizmet Kullanılamıyor | 30 saniye sonra yeniden deneyin veya daha fazla bilgi |
| Diğerlerinin tümü | 10 saniye sonra yeniden deneyin veya daha fazla bilgi |


## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimler durumunu görüntülemek için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Olay teslimi seçenekleri özelleştirmek için bkz: [kullanılmayan harf ve yeniden deneme ilkelerine](manage-event-delivery.md).
