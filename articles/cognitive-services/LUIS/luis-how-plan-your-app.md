---
title: Uygulamanızı planlama
titleSuffix: Language Understanding - Azure Cognitive Services
description: İlgili uygulamayı amaç ve varlıkları anahat ve uygulama planlarınızı Language Understanding Intelligent Services (LUIS içinde) oluşturun.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: diberry
ms.openlocfilehash: e14b9f2930ed9c170f31bd654829efe3b5a99446
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091316"
---
# <a name="plan-your-luis-app"></a>LUIS uygulamanızı planlama

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

### <a name="simple-entity"></a>Basit varlık
Bir varlığın tek bir kavramı açıklanmaktadır.

![varlığın](./media/luis-plan-your-app/simple-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#simple-entity-data) JSON sorgu yanıtı uç noktasından Basit varlık ayıklama hakkında daha fazla bilgi edinmek için. Bu deneyin [hızlı](luis-quickstart-primary-and-secondary-data.md) tek bir varlığın kullanma hakkında daha fazla bilgi için.

### <a name="hierarchical-entity"></a>Hiyerarşik varlık
Özel bir tür hiyerarşik bir varlıktır bir **basit** varlık; üst-alt ilişkisi biçiminde bir kategori ve üyelerini tanımlama. İlişki, içinde utterance bağlamından tarafından belirlenir. Alt öğeleri hiyerarşik bir varlığın de basit varlıklardır.

![hiyerarşik varlık](./media/luis-plan-your-app/hierarchical-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#hierarchical-entity-data) JSON sorgu yanıtı uç noktasından hiyerarşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Bu deneyin [hızlı](luis-quickstart-intent-and-hier-entity.md) hiyerarşik bir varlık kullanma hakkında daha fazla bilgi için.

### <a name="composite-entity"></a>Bileşik varlık
Bileşik bir varlık, bir bütün bölümleri form ait diğer varlıkların yapılır. Bileşik bir varlık, çeşitli varlık türleri içerir.

![Bileşik varlık](./media/luis-plan-your-app/composite-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#composite-entity-data) JSON sorgu yanıtı uç noktasından Bileşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Bu deneyin [öğretici](luis-tutorial-composite-entity.md) bileşik bir varlık kullanma hakkında daha fazla bilgi için.

### <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık
LUIS sağlar [önceden oluşturulmuş varlıklarla](luis-prebuilt-entities.md) sayısı, veri, e-posta adresi ve URL gibi ortak veri türleri için. Bir bilet sırada bilet sayısı için önceden oluşturulmuş varlık numarası kullanabilirsiniz.

![Sayı önceden oluşturulmuş varlık](./media/luis-plan-your-app/number-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#prebuilt-entity-data) JSON sorgu yanıtı uç noktasından oluşturulmuş varlık ayıklama hakkında daha fazla bilgi edinmek için. 

### <a name="list-entity"></a>Liste varlığı 
Açıkça belirtilen bir liste değerlerinin bir listesini varlıktır. Her değer bir veya birden çok eş anlamlılar oluşur. Bir seyahat uygulamasında havaalanı adları göstermek için bir liste varlığı seçebilirsiniz.

![Liste varlığı](./media/luis-plan-your-app/list-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#list-entity-data) JSON sorgu yanıtı uç noktasından listesi varlık ayıklama hakkında daha fazla bilgi edinmek için. Bu deneyin [hızlı](luis-quickstart-intent-and-list-entity.md) liste varlığı kullanma hakkında daha fazla bilgi için.

### <a name="regular-expression-entity"></a>Normal ifade varlığı
Bir normal ifade varlık, bir normal ifadeye göre bir utterance iyi biçimlendirilmiş verileri ayıklamak LUIS sağlar.

![Normal ifade varlığı](./media/luis-plan-your-app/regex-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#regular-expression-entity-data) normal ifade varlıkları JSON sorgu yanıtı uç noktasından ayıklama hakkında daha fazla bilgi edinmek için. Deneyin [hızlı](luis-quickstart-intents-regex-entity.md) bir normal ifade varlık kullanma hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanız, yayımlanan eğitildi ve konuşma uç noktası alır sonra tahmin geliştirmelerle uygulamak planlama [etkin olarak öğrenmeye](luis-how-to-review-endoint-utt.md), [tümcecik listeleri](luis-concept-feature.md), ve [desenleri](luis-concept-patterns.md). 


* Bkz: [Language Understanding Intelligent Services (LUIS) ilk uygulamanızı oluşturma](luis-get-started-create-app.md) LUIS uygulaması oluşturma Hızlı Kılavuz.
