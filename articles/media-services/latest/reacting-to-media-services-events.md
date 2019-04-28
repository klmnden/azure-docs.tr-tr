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
ms.date: 03/12/2019
ms.author: juliako
ms.openlocfilehash: cb5d6474a0c830933c712e1008015b5220617c96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996219"
---
# <a name="handling-event-grid-events"></a>Event Grid olaylarını işleme

Media Services olaylarını uygulamaları modern sunucusuz mimarileri kullanarak farklı olaylara (örneğin, iş durum değişiklik olayı) yanıt verin. Bunu karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerek kalmadan yapar. Bunun yerine, olayların gönderilmesini [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gibi olay işleyicilerine [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi Web kancası ve ne için yalnızca ödeme kullanın. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Event Grid fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/).

Kullanılabilirlik için Media Services olayları Event Grid'e bağlı [kullanılabilirlik](../../event-grid/overview.md) ve Event Grid gibi diğer bölgelerde kullanıma sunulacaktır.  

## <a name="media-services-events-and-schemas"></a>Media Services olaylarını ve şemalar

Event grid'i kullanır [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için. Media Services olaylarını verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.Media" ile başladığından bir Media Services olayı belirleyebilirsiniz.

Daha fazla bilgi için [Media Services olay şemaları](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Olayları kullanan uygulamalar

Media Services olaylarını işleme uygulamaları birkaç önerilen uygulamaları izlemelisiniz:

* Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılması gibi belirli bir kaynaktan gelen olayları olduğu varsayılır değil ancak beklediğiniz depolama hesabından geldiğinden emin olmak için iletisinin konu denetlemek için önemlidir.
* Benzer şekilde, eventType tek bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsaymayın olup olmadığını denetleyin.
* Alanları anlamadığınız yoksayın.  Bu yöntem, dayanıklı, gelecekte eklenebilir yeni özellikler kalmasına yardımcı olur.
* Belirli bir olaya olayları sınırlandırmak için "subject" önek ve sonek eşleşmeleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[İş durumu olayları alma](job-state-events-cli-how-to.md)
