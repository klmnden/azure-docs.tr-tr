---
title: Önceden oluşturulmuş modeller
titleSuffix: Language Understanding - Azure Cognitive Services
description: Önceden oluşturulmuş modelleri, etki alanları, amacı, konuşma ve varlıklar sağlar. Önceden oluşturulmuş bir etki alanı ile uygulamanızı başlatabilir veya ilgili bir etki alanı daha sonra uygulamanıza ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: 5d2ea9d971eff22ddeed4122c9697ca3096697b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60813886"
---
# <a name="prebuilt-domain-intent-and-entity-models"></a>Etki alanı, hedefi ve varlık önceden oluşturulmuş modelleri

Önceden oluşturulmuş modelleri, etki alanları, amacı, konuşma ve varlıklar sağlar. Önceden oluşturulmuş bir etki alanı ile uygulamanızı başlatabilir veya ilgili bir etki alanı daha sonra uygulamanıza ekleyin. 

## <a name="types-of-prebuilt-models"></a>Önceden oluşturulmuş modelleri türü

LUIS sağlar, önceden oluşturulmuş modelleri 3 tür vardır. Her model, herhangi bir zamanda uygulamanıza eklenebilir. 

|Model türü|İçerir|
|--|--|
|Domain|Hedefleri, konuşma, varlıklar|
|Hedefler|Hedefleri, konuşma|
|Varlıklar|Yalnızca varlıklar| 

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanları

Language Understanding (LUIS) sağlayan *önceden oluşturulmuş etki alanları*, önceden oluşturulmuş kümesi olan [hedefleri](luis-how-to-add-intents.md) ve [varlıkları](luis-concept-entity-types.md) etki alanları veya genel kategorileri için birlikte çalışan istemci uygulamaları. 

Önceden oluşturulmuş etki alanları, eğitilmiş ve LUIS uygulamanıza eklemek hazır. Uygulamanıza ekledikten sonra amaç ve varlıkları önceden oluşturulmuş bir etki alanında tamamen özelleştirilebilir. 

Önceden oluşturulmuş tüm etki alanı özelleştirmesini başlatırsanız, hedefleri ve uygulamanızı kullanın gerekmez varlıkları silin. Bazı ıntents veya varlıkları önceden oluşturulmuş etki alanı zaten sağlayan kümesine ekleyebilirsiniz. Örneğin, kullanıyorsanız **olayları** önceden oluşturulmuş etki alanı bir spor olayı uygulama için aşağıdakileri yapabilirsiniz sports ekiplerin varlık eklemek için. Başladığınızda [konuşma sağlayan](luis-how-to-add-example-utterances.md) LUIS için uygulamanıza özgü olan koşulları içerir. LUIS bunları öğrenen ve önceden oluşturulmuş etki alanının hedefleri ve uygulamanızın ihtiyaçlarını varlıklara belirlenmek. 

> [!TIP]
> Amaç ve varlıkları önceden oluşturulmuş bir etki alanında en iyi birlikte çalışır. Hedefleri ve mümkün olduğunda aynı etki alanındaki varlık birleştirerek daha iyidir.
> Yardımcı programları önceden oluşturulmuş etki alanının herhangi bir etki alanı kullanmak için özelleştirebileceğiniz amacı vardır. Örneğin, ekleyebileceğiniz `Utilities.Repeat` eğitimi ve uygulaması için tanı hangi eylemleri kullanıcı uygulamanızda yineleyin isteyebilirsiniz. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Önceden oluşturulmuş etki alanına amacını davranışını değiştirme

LUIS uygulamanızı olmasını istediğiniz bir hedefi benzer bir amaç önceden oluşturulmuş bir etki alanı içerir ancak farklı davranır istediğiniz bulabilirsiniz. Örneğin, **yerler** önceden oluşturulmuş etki alanı sağlar bir `MakeReservation` hedefi bir restoran ayırma, ancak yapmak istediğiniz otel rezervasyon yapmak için bu hedefi kullanmak için uygulamanızı. Bu durumda, otel rezervasyon yapma ve bunları etiketleme hakkında LUIS için konuşma sağlayarak bu amaç davranışını değiştirmek kullanarak `MakeReservation` hedefi, böylece LUIS tanımak için eğitilebileceği de `MakeReservation` isteğinde bir otel rezervasyonu için hedefi .

Önceden oluşturulmuş etki alanları içindeki tam listesini bulabilirsiniz [önceden oluşturulmuş etki alanları başvuru](./luis-reference-prebuilt-domains.md).

## <a name="prebuilt-intents"></a>Önceden oluşturulmuş hedefleri

LUIS, önceden oluşturulmuş hedefleri ve kullanıcıların konuşma sağlar. Tüm etki alanına eklemeden ıntents eklenebilir. Bir hedefi ekleyerek bir hedefi ve kendi Konuşma ekleme işlemidir. Hedefi adı hem de utterance liste değiştirilebilir.  

## <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar

LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık destek LUIS uygulamanızı kültüre göre değişir. LUIS destekler, kültür tarafından desteği dahil olmak üzere önceden oluşturulmuş varlıkların tam listesi için bkz. [önceden oluşturulmuş bir varlık başvurusu](./luis-reference-prebuilt-entities.md).

Önceden oluşturulmuş bir varlık, uygulamanızda eklendiğinde, Öngörüler, yayımlanan uygulamayı dahil edilir. Önceden oluşturulmuş varlıkların önceden eğitilmiş ve **olamaz** değiştirilemiyor. Önceden oluşturulmuş bir varlık nasıl çalıştığını görmek için aşağıdaki adımları izleyin:

> [!NOTE]
> **Builtin.DateTime** kullanım dışı bırakılmıştır. İle değiştirilir [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), tarih ve saat aralığı tanıma yanı sıra, belirsiz tarihler ve saatler geliştirilmiş tanınmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [önceden oluşturulmuş varlıklar ekleme](luis-prebuilt-entities.md) uygulamanıza.
