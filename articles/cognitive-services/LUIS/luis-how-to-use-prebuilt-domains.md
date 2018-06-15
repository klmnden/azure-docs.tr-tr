---
title: Önceden oluşturulmuş etki alanlarının Azure HALUK uygulamalardaki | Microsoft Docs
description: Dil anlama akıllı hizmet (HALUK) uygulamaları önceden oluşturulmuş etki alanlarında kullanmayı öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 72ca0def8528adae5e46a02fa5bfccd14a3b6ab4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355775"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Önceden oluşturulmuş alanlarında HALUK uygulamaları kullanın  

Dil anlama (HALUK) sağlar *önceden oluşturulmuş etki alanları*, önceden oluşturulmuş kümesi olan [hedefleri](luis-how-to-add-intents.md) ve [varlıklar](luis-concept-entity-types.md) etki alanları veya ortak kategorileri için birlikte çalışan istemci uygulamaları. Önceden oluşturulmuş etki alanları önceden eğitilmiş ve HALUK uygulamanıza eklemek hazırdır. Hedefleri ve varlıkları önceden oluşturulmuş bir etki alanında tam olarak özelleştirilebilir sonra uygulamanıza eklediğiniz - kullanıcılarınız için çalışması için bunları utterances ile sisteminizden eğitim yapabilirsiniz. Önceden oluşturulmuş tüm etki alanı özelleştirmesi için bir başlangıç noktası olarak kullanın ya da yalnızca birkaç hedefleri veya önceden oluşturulmuş bir etki alanından varlıklar ödünç. 

Gözat **önceden oluşturulmuş etki alanları** uygulamanızda kullanabileceğiniz önceden oluşturulmuş diğer etki alanları görmek için sekmesini. Uygulamanıza eklemek bir etki alanı için kutucuğa tıklayın veya tıklayın **daha fazla bilgi edinin** kendi döşemesinin kendi hedefleri ve varlıkları hakkında bilgi edinin.

> [!TIP]
> Önceden oluşturulmuş alanlarında tam bir listesini bulabilirsiniz [önceden oluşturulmuş etki alanları başvuru](./luis-reference-prebuilt-domains.md).

![Önceden oluşturulmuş bir etki alanı ekleme](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir etki alanı ekleme
İçinde **önceden oluşturulmuş etki alanları** sekmesinde, RestaurantReservation etki alanını bulun ve tıklatın **etki alanı Ekle**. Önceden oluşturulmuş etki alanı HALUK uygulamanıza eklendikten sonra açmak **hedefleri** ve RestaurantReservation.Reserve amacına'ı tıklatın. Birçok örnek utterances daha önce sağlanan ve varlık etiketli olduğunu görebilirsiniz.

![RestaurantReservation.Reserve amacı](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından HALUK uygulamalar tasarlama
Önceden oluşturulmuş bir etki alanı HALUK uygulamanızda kullanırken, önceden oluşturulmuş tüm etki alanı özelleştirme veya kendi amaçları ve varlıkları bazılarını ile başlayın.

## <a name="customizing-an-entire-prebuilt-domain"></a>Önceden oluşturulmuş tüm etki alanı özelleştirme
Önceden oluşturulmuş etki alanları, genel olacak şekilde tasarlanmıştır. Bunlar, birçok amaçları ve gereksinimlerinizi uygulamayı özelleştirmek için seçebileceğiniz varlıklar, içerir. Önceden oluşturulmuş tüm etki alanı özelleştirme başlatırsanız, amaçları ve uygulamanızı kullanmanıza gerek olmayan varlıklar silin. Önceden oluşturulmuş etki alanı zaten sağlar kümeye bazı hedefleri veya varlıklar de ekleyebilirsiniz. Örneğin, kullanıyorsanız **olayları** önceden oluşturulmuş etki alanı Spor olay uygulama için şunları yapabilirsiniz Spor takımlar için varlık eklemek için. Başlattığınızda [utterances sağlama](luis-how-to-add-example-utterances.md) HALUK için uygulamanızın belirli terimleri içerir. HALUK bunları tanıyabilmesi öğrenir ve önceden oluşturulmuş etki alanının amaçları ve uygulamanızın gereksinimlerine varlıklara belirlenmek. 

> [!TIP]
> Hedefleri ve varlıkları önceden oluşturulmuş bir etki alanındaki en iyi birlikte çalışır. Hedefleri ve mümkün olduğunda aynı etki alanından varlıkları birleştirmek iyidir.
> * Aynı etki alanından ilgili hedefleri kullanmak en iyi bir uygulamadır. Örneğin, özelleştirme, `MakeReservation` içinde hedefi **yerler** etki alanı, ardından `Cancel` gelen hedefi **yerler** İptalhedefiyerineetkialanı**Olayları** veya **yardımcı programları** etki alanları.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Önceden oluşturulmuş bir etki alanı hedefi davranışını değiştirme
HALUK uygulamanızda edilmesini istediğiniz amacına benzer amacına önceden oluşturulmuş bir etki alanı içerir ancak farklı davranır istediğiniz bulabilirsiniz. Örneğin, **yerlerde** önceden oluşturulmuş bir etki alanı sağlar bir `MakeReservation` bir restoran ayırma, ancak sağlama amacı otel rezervasyon yapmak için bu hedefi kullanmak için uygulamanızı istiyor. Bu durumda, bu amacı davranışını utterances otel rezervasyonları yapma ve bunları etiketleme hakkında HALUK sağlayarak değiştirebileceğiniz kullanarak `MakeReservation` hedefi, böylece HALUK tanımak için retrained `MakeReservation` bir otel kitap için bir istek hedefi .

> [!TIP]
> Yardımcı programlar etki alanı için herhangi bir etki alanında kullanmak için özelleştirebileceğiniz önceden oluşturulmuş hedefleri göz atın. Örneğin, ekleyebileceğiniz `Utilities.Repeat` eğitimi ve uygulama tanıması hangi eylemleri kullanıcı uygulamanızda yineleyin isteyebilirsiniz.


## <a name="next-step"></a>Sonraki adım

Önceden oluşturulmuş bir etki alanına daha fazla örnek utterances ekleyerek özelleştirin.

> [!div class="nextstepaction"]
> [Örnek utterances Ekle](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Ek kaynaklar

Bkz: [önceden oluşturulmuş etki alanları başvuru](./luis-reference-prebuilt-domains.md) önceden oluşturulmuş etki alanları hakkında daha fazla ayrıntı için.
