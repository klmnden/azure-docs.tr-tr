---
title: Uygulamaları daha hızlı giriş LUIS uygulamalar oluşturmak için önceden oluşturulmuş etki alanları kullanın
titleSuffix: Azure Cognitive Services
description: 'Language Understanding (LUIS), önceden oluşturulmuş hedefleri ve etki alanları için birlikte çalışan bir varlık kümesi veya istemci uygulamaların genel kategorileri şunlardır: önceden oluşturulmuş etki alanları sağlar. Önceden oluşturulmuş etki alanları önceden eğitilmiş ve LUIS uygulamanıza eklemek hazırdır.'
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 1aa7e1bf9c1a584803a60a5061b4ae4cc8664ff4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037380"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>LUIS uygulamalarında önceden oluşturulmuş etki alanları kullanın  

Language Understanding (LUIS) sağlayan *önceden oluşturulmuş etki alanları*, önceden oluşturulmuş kümesi olan [hedefleri](luis-how-to-add-intents.md) ve [varlıkları](luis-concept-entity-types.md) etki alanları veya genel kategorileri için birlikte çalışan istemci uygulamaları. Önceden oluşturulmuş etki alanları önceden eğitilmiş ve LUIS uygulamanıza eklemek hazırdır. Amaç ve varlıkları önceden oluşturulmuş bir etki alanında tamamen özelleştirilebilir bir kez uygulamanıza eklediğiniz - kullanıcılarınız için çalıştıkları için bunları Konuşma ile sisteminizden eğitebilirsiniz. Önceden oluşturulmuş tüm etki alanı özelleştirmesi için bir başlangıç noktası olarak kullanın veya yalnızca birkaç ıntents veya önceden oluşturulmuş bir etki alanı varlıklardan ödünç alın. 

Gözat **önceden oluşturulmuş etki alanları** uygulamanızda kullanabileceğiniz diğer önceden oluşturulmuş etki alanları görmek için sekmesinde. Uygulamanıza eklemek bir etki alanı için bir kutucuğa tıklayarak ya da tıklayın **daha fazla bilgi edinin** kendi amacı ve varlıklar hakkında bilgi edinmek için döşeme içindeki.

> [!TIP]
> Önceden oluşturulmuş etki alanları içindeki tam listesini bulabilirsiniz [önceden oluşturulmuş etki alanları başvuru](./luis-reference-prebuilt-domains.md).

![Önceden oluşturulmuş etki alanını ekleme](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir alan ekleme
İçinde **önceden oluşturulmuş etki alanları** sekmesinde RestaurantReservation etki alanını bulun ve tıklayın **etki alanı Ekle**. Önceden oluşturulmuş etki alanı LUIS uygulamanızı eklendikten sonra açın **hedefleri** RestaurantReservation.Reserve amaca tıklayın. Birçok örnek konuşma zaten alınan sağlanan ve varlıklarla etiketli olduğunu görebilirsiniz.

![RestaurantReservation.Reserve hedefi](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından LUIS uygulamaları tasarlama
Önceden oluşturulmuş etki alanları LUIS uygulamanızı kullanırken, önceden oluşturulmuş tüm etki alanı özelleştirme veya kendi amacı ve varlıkların birkaç ile başlayın.

## <a name="customizing-an-entire-prebuilt-domain"></a>Önceden oluşturulmuş tüm etki alanı özelleştirme
Önceden oluşturulmuş etki alanları, genel olacak şekilde tasarlanmıştır. Bunlar, birçok hedefleri ve ihtiyaçlarınıza bir uygulamayı özelleştirmek için arasından seçim yapabileceği varlıkları içerir. Önceden oluşturulmuş tüm etki alanı özelleştirmesini başlatırsanız, hedefleri ve uygulamanızı kullanın gerekmez varlıkları silin. Bazı ıntents veya varlıkları önceden oluşturulmuş etki alanı zaten sağlayan kümesine ekleyebilirsiniz. Örneğin, kullanıyorsanız **olayları** önceden oluşturulmuş etki alanı bir spor olayı uygulama için aşağıdakileri yapabilirsiniz sports ekiplerin varlık eklemek için. Başladığınızda [konuşma sağlayan](luis-how-to-add-example-utterances.md) LUIS için uygulamanıza özgü olan koşulları içerir. LUIS bunları öğrenen ve önceden oluşturulmuş etki alanının hedefleri ve uygulamanızın ihtiyaçlarını varlıklara belirlenmek. 

> [!TIP]
> Amaç ve varlıkları önceden oluşturulmuş bir etki alanında en iyi birlikte çalışır. Hedefleri ve mümkün olduğunda aynı etki alanındaki varlık birleştirerek daha iyidir.
> * En iyi uygulama, aynı etki alanından ilgili ıntents kullanmaktır. Örneğin, özelleştirme, `MakeReservation` hedefi olarak **yerler** etki alanı, ardından `Cancel` gelen hedefi **yerler** İptalamacayerineetkialanı**Olayları** veya **yardımcı programları** etki alanları.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Önceden oluşturulmuş etki alanına amacını davranışını değiştirme
LUIS uygulamanızı olmasını istediğiniz bir hedefi benzer bir amaç önceden oluşturulmuş bir etki alanı içerir ancak farklı davranır istediğiniz bulabilirsiniz. Örneğin, **yerler** önceden oluşturulmuş etki alanı sağlar bir `MakeReservation` hedefi bir restoran ayırma, ancak yapmak istediğiniz otel rezervasyon yapmak için bu hedefi kullanmak için uygulamanızı. Bu durumda, otel rezervasyon yapma ve bunları etiketleme hakkında LUIS için konuşma sağlayarak bu amaç davranışını değiştirmek kullanarak `MakeReservation` hedefi, böylece LUIS tanımak için eğitilebileceği de `MakeReservation` isteğinde bir otel rezervasyonu için hedefi .

> [!TIP]
> Herhangi bir etki alanı kullanmak için özelleştirebileceğiniz önceden oluşturulmuş hedefleri için yardımcı programlar etki göz atın. Örneğin, ekleyebileceğiniz `Utilities.Repeat` eğitimi ve uygulaması için tanı hangi eylemleri kullanıcı uygulamanızda yineleyin isteyebilirsiniz.


## <a name="next-step"></a>Sonraki adım

Önceden oluşturulmuş bir etki alanı, daha fazla örnek konuşma ekleyerek özelleştirin.

> [!div class="nextstepaction"]
> [Örnek Konuşma ekleme](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Ek kaynaklar

Bkz: [önceden oluşturulmuş etki alanları başvuru](./luis-reference-prebuilt-domains.md) önceden oluşturulmuş etki alanları hakkında daha fazla ayrıntı için.
