---
title: Azure Media Services olaylarına tepki | Microsoft Docs
description: Media Services olaylarına abone olmak için Azure olay kılavuzunu kullanın.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 969957d53824bd70440e5529b83bc830bb5d9cc4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788154"
---
# <a name="reacting-to-media-services-events"></a>Media Services olaylarına tepki

Media Services olayları modern sunucusuz mimarileri kullanarak farklı olaylara (örneğin, iş durum değişikliği olayından) tepki vermek uygulamaların olanak tanır. Bunu karmaşık kodu veya verimsiz ve pahalı yoklama Hizmetleri gerek kalmadan yapar. Bunun yerine, olayları gönderilen [Azure olay kılavuz](https://azure.microsoft.com/services/event-grid/) gibi olay işleyicileri için [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi Web kancası ve ne için yalnızca ödeme kullanın. Fiyatlandırma hakkında daha fazla bilgi için bkz: [olay kılavuz fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/).

Kullanılabilirlik Media Services olayları için olay kılavuza bağlı [kullanılabilirlik](../../event-grid/overview.md) ve olay kılavuz yaptığı gibi diğer bölgelerde kullanılabilir hale gelecektir.  

## <a name="available-media-services-events"></a>Kullanılabilir Media Services olayları

Olay kılavuz kullanan [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için.  Şu anda, Media Services olay abonelikleri aşağıdaki olay türü içerebilir:  

|Olay adı|Açıklama|
|----------|-----------|
| Microsoft.Media.JobStateChange| İş değişiklikleri durumunu tetiklenir. |

## <a name="event-schema"></a>Olay şeması

Media Services olayları, verilerde yapılan değişiklikler yanıtlamaları gereken tüm bilgileri içerir.  EventType özelliği "Microsoft.Media" ile başlar, Media Services olay tanımlayabilirsiniz.

Daha fazla bilgi için bkz: [Media Services olay şemaları](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Olayları tüketimi için uygulamalar

Media Services olayları işlemek uygulamaları birkaç önerilen uygulamaları izlemelidir:

* Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılabilir gibi olayları belirli bir kaynaktan varsayalım değil ancak beklediğiniz depolama hesabından geldiğinden emin olmak için ileti konusu denetlemek için önemlidir.
* Benzer şekilde, eventType bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsayar değil olup olmadığını denetleyin.
* Anlamadığınız alanları yoksayın.  Bu yöntem, esnek gelecekte eklenebilecek yeni özelliklere tutmaya yardımcı olur.
* Belirli bir olaya olayları sınırlandırmak için "subject" önek ve sonek eşleşmeleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[İş durumu olaylarını alma](job-state-events-cli-how-to.md)
