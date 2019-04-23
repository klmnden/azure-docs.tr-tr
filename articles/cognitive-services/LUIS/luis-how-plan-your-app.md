---
title: Uygulamanızı planlama
titleSuffix: Language Understanding - Azure Cognitive Services
description: İlgili uygulamayı amaç ve varlıkları anahat ve uygulama planlarınızı Language Understanding Intelligent Services (LUIS içinde) oluşturun.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 9d54cff81f39f41b60800e9b33f3b4da1a735d85
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795005"
---
# <a name="plan-your-luis-app-with-subject-domain-intents-and-entities"></a>Konu etki alanı, amaçlarını ve varlıkları LUIS uygulamanızı planlama

Uygulamanızı planlama önemlidir. Olası hedefleri ve uygulamanız için uygun olan varlıklar dahil olmak üzere, etki alanı tanımlayın.  

## <a name="identify-your-domain"></a>Etki alanınızı tanımlayın

Bir LUIS uygulaması bir etki alanına özgü konu ortalanır.  Örneğin, kayıt biletleri, uçuşlar, Oteller ve kiralama otomobiller gerçekleştiren bir seyahat uygulaması olabilir. Başka bir uygulama, uygulama, uygunluk çalışmalarını izlemeyi ve hedeflerini ayarlama ilgili içerik sağlayabilir. Etki alanını tanıtan sözcük ve tümcecikleri etki alanınız için önemli olan bulmanıza yardımcı olur.

> [!TIP]
> LUIS sunar [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) birçok yaygın senaryo için.
> Önceden oluşturulmuş bir etki alanı bir başlangıç noktası olarak uygulamanız için kullanıp kullanamayacağını denetleyin.

## <a name="identify-your-intents"></a>Hedefleri tanımlayın

Düşünün [hedefleri](luis-concept-intent.md) uygulamanızın görev için önemli. Bir kitap ve kullanıcının hedefteki hava durumunu denetlemek için işlevleri ile bir seyahat uygulaması örneği ele alalım. Bu eylemler için "BookFlight" ve "GetWeather" ıntents tanımlayabilirsiniz. Daha karmaşık bir daha fazla işlev uygulaması, daha fazla amacı sahiptir ve bunları özenle çok belirli olmaması için tanımlamanız gerekir. Örneğin, "BookFlight" ve "BookHotel" ayrı ıntents olması gerekebilir, ancak "BookInternationalFlight" ve "BookDomesticFlight" çok benzer olabilir.

> [!NOTE]
> Yalnızca kullanmak için en iyi bir uygulamadır uygulamanızın işlevleri gerçekleştirmek gerektiği kadar amacı. Çok fazla ıntents tanımlarsanız, konuşma doğru sınıflandırmak LUIS için daha zor hale gelir. Çok tanımlarsanız birkaç, bunlar olabilir çakıştığı için farklı şekilde genel.

## <a name="create-example-utterances-for-each-intent"></a>Her bir amaç için örnek konuşma oluşturma

Intents belirledikten sonra her amaç için 10 veya 15 örnek konuşma oluşturun. Başlangıç değil bu sayıdan daha az olması veya her amaç için birçok konuşma oluşturun. Her utterance önceki utterance farklı olmalıdır. Konuşma iyi çeşitli genel sözcük sayısı, sözcük seçimi, fiil şimdiki ve noktalama işaretleri içerir. 

## <a name="identify-your-entities"></a>Varlıklarınızı tanımlayın

Ayıklanan istediğiniz varlıkları örnek konuşma belirleyin. Bir kitap için hedef, tarih, havayolu, bilet kategorisi gibi bazı bilgiler gerekiyor ve seyahat sınıfı. Bu veri türleri için varlıklar oluşturun ve ardından işaretleyin [varlıkları](luis-concept-entity-types.md) örnek konuşma içinde bir işlemi gerçekleştirmek için önemli olduğundan. 

Uygulamanızda kullanmak için hangi varlıkları belirlerken, farklı nesne türleri arasındaki ilişkileri yakalamak için varlıkların olduğunu aklınızda bulundurun. [LUIS varlıklarda](luis-concept-entity-types.md) farklı türleri hakkında daha fazla ayrıntı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanız, yayımlanan eğitildi ve konuşma uç noktası alır sonra tahmin geliştirmelerle uygulamak planlama [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md), [tümcecik listeleri](luis-concept-feature.md), ve [desenleri](luis-concept-patterns.md). 


* Bkz: [Language Understanding Intelligent Services (LUIS) ilk uygulamanızı oluşturma](luis-get-started-create-app.md) LUIS uygulaması oluşturma Hızlı Kılavuz.
