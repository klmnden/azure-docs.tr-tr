---
title: Azure Media Services olaylara tepki verme | Microsoft Docs
description: Media Services olaylarına abone olmak için Azure Event grid'i kullanın.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/19/2018
ms.author: juliako
ms.openlocfilehash: 143fec2ddb168b0fff0e419fa5767e9718637241
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465546"
---
# <a name="reacting-to-media-services-events"></a>Media Services olaylara tepki verme

Media Services olaylarını uygulamaları modern sunucusuz mimarileri kullanarak farklı olaylara (örneğin, iş durum değişiklik olayı) yanıt verin. Bunu karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerek kalmadan yapar. Bunun yerine, olayların gönderilmesini [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gibi olay işleyicilerine [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi Web kancası ve ne için yalnızca ödeme kullanın. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Event Grid fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/).

Kullanılabilirlik için Media Services olayları Event Grid'e bağlı [kullanılabilirlik](../../event-grid/overview.md) ve Event Grid gibi diğer bölgelerde kullanıma sunulacaktır.  

## <a name="available-media-services-events"></a>Kullanılabilir Media Services olayları

Event grid'i kullanır [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için.  Şu anda, Media Services olay abonelikleri aşağıdaki olaylar içerebilir:  

|Olay Adı|Açıklama|
|----------|-----------|
| Microsoft.Media.JobStateChange| Bir işin değişiklikleri durumu değiştiğinde harekete geçirilen. |
| Microsoft.Media.LiveEventConnectionRejected | Kodlayıcı'nın bağlantı girişimini reddetti. |
| Microsoft.Media.LiveEventEncoderConnected | Kodlayıcı Canlı etkinlik ile bağlantı kurar. |
| Microsoft.Media.LiveEventEncoderDisconnected | Kodlayıcı bağlantısını keser. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Medya sunucusu çok geç veya çakışan bir zaman damgası olduğundan veri öbeği düşene (yeni veri öbeğin zaman damgası olan önceki verileri öbek, son saatten daha az). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Medya sunucusu, her parça için ilk veri öbeği stream ya da bağlantı alır. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Ses medya sunucusu algılar ve video akışları eşitlenmiş halde değil. Kullanıcı deneyimi olmayan etkilenebilir, çünkü bir uyarı olarak kullanın. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Medya sunucusu herhangi bir dış kodlayıcıdan gelen iki video akışları eşitlenmemiş algılar. Kullanıcı deneyimi olmayan etkilenebilir, çünkü bir uyarı olarak kullanın. |
| Microsoft.Media.LiveEventIngestHeartbeat | Canlı etkinlik çalıştırıldığında, her parça için her 20 saniyede yayımladı. Sağlar sistem durumu özetini alın. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Medya sunucusu süreksizlik gelen izde algılar. |

## <a name="event-schema"></a>Olay Şeması

Media Services olaylarını verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir.  EventType özelliği "Microsoft.Media" ile başladığından bir Media Services olayı belirleyebilirsiniz.

Daha fazla bilgi için [Media Services olay şemaları](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Olayları kullanan uygulamalar

Media Services olaylarını işleme uygulamaları birkaç önerilen uygulamaları izlemelisiniz:

* Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılması gibi belirli bir kaynaktan gelen olayları olduğu varsayılır değil ancak beklediğiniz depolama hesabından geldiğinden emin olmak için iletisinin konu denetlemek için önemlidir.
* Benzer şekilde, eventType tek bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsaymayın olup olmadığını denetleyin.
* Alanları anlamadığınız yoksayın.  Bu yöntem, dayanıklı, gelecekte eklenebilir yeni özellikler kalmasına yardımcı olur.
* Belirli bir olaya olayları sınırlandırmak için "subject" önek ve sonek eşleşmeleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[İş durumu olayları alma](job-state-events-cli-how-to.md)
