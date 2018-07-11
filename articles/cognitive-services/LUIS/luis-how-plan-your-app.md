---
title: LUIS uygulamalarınızı planlama | Microsoft Docs
description: İlgili uygulamayı amaç ve varlıkları anahat ve uygulama planlarınızı Language Understanding Intelligent Services (LUIS içinde) oluşturun.
services: cognitive-services
author: DeniseMak
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 2ce202bbb1479db18fb88cfef4d510ae4cb39a78
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952109"
---
# <a name="plan-your-luis-app"></a>LUIS uygulamanızı planlama

LUIS oluşturmaya başlamadan önce uygulamanızı planlama önemlidir. Anahat veya olası hedefleri ve uygulamanızın etki alanına özel konuya ilgili varlıkların şema hazırlayın.  

## <a name="identify-your-domain"></a>Etki alanınızı tanımlayın
Bir LUIS uygulaması bir etki alanına özgü konu ortalanır.  Örneğin, kayıt biletleri, uçuşlar, Oteller ve kiralama otomobiller gerçekleştiren bir seyahat uygulaması olabilir. Başka bir uygulama, uygulama, uygunluk çalışmalarını izlemeyi ve hedeflerini ayarlama ilgili içerik sağlayabilir. 

> [!TIP]
> LUIS sunar [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) birçok yaygın senaryo için.
> Önceden oluşturulmuş bir etki alanı bir başlangıç noktası olarak uygulamanız için kullanıp kullanamayacağını denetleyin.

## <a name="identify-your-intents"></a>Hedefleri tanımlayın
Düşünün [hedefleri](luis-concept-intent.md) uygulamanızın görev için önemli. Bir kitap ve kullanıcının hedefteki hava durumunu denetlemek için işlevleri ile bir seyahat uygulaması örneği ele alalım. Bu eylemler için "BookFlight" ve "GetWeather" ıntents tanımlayabilirsiniz. Daha karmaşık bir daha fazla işlev uygulaması, daha fazla amacı sahiptir ve bunları özenle çok belirli olmaması için tanımlamanız gerekir. Örneğin, "BookFlight" ve "BookHotel" ayrı ıntents olması gerekebilir, ancak "BookInternationalFlight" ve "BookDomesticFlight" çok benzer olabilir.

> [!NOTE]
> Yalnızca kullanmak için en iyi bir uygulamadır uygulamanızın işlevleri gerçekleştirmek gerektiği kadar amacı. Çok fazla ıntents tanımlarsanız, konuşma doğru sınıflandırmak LUIS için daha zor hale gelir. Çok tanımlarsanız birkaç, bunlar olabilir çakıştığı için farklı şekilde genel.


## <a name="identify-your-entities"></a>Varlıklarınızı tanımlayın
Bir kitap için hedef, tarih, havayolu, bilet kategorisi gibi bazı bilgiler gerekiyor ve seyahat sınıfı. Bu dosyaların ekleyebilirsiniz [varlıkları](luis-concept-entity-types.md) bir işlemi gerçekleştirmek için önemli olduğundan. 

Uygulamanızda kullanmak için hangi varlıkları belirlerken, farklı nesne türleri arasındaki ilişkileri yakalamak için varlıkların olduğunu aklınızda bulundurun. [LUIS varlıklarda](luis-concept-entity-types.md) farklı türleri hakkında daha fazla ayrıntı sağlar.

### <a name="simple-entity"></a>Basit varlık
Bir varlığın tek bir kavramı açıklanmaktadır.

![varlığın](./media/luis-plan-your-app/simple-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#simple-entity-data) JSON sorgu yanıtı uç noktasından Basit varlık ayıklama hakkında daha fazla bilgi edinmek için. Varlığın deneyin [hızlı](luis-quickstart-primary-and-secondary-data.md) tek bir varlığın kullanma hakkında daha fazla bilgi için.

### <a name="hierarchical-entity"></a>Hiyerarşik varlık
Özel bir tür hiyerarşik bir varlıktır bir **basit** varlık; üst-alt ilişkisi biçiminde bir kategori ve üyelerini tanımlama.

![hiyerarşik varlık](./media/luis-plan-your-app/hierarchical-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#hierarchical-entity-data) JSON sorgu yanıtı uç noktasından hiyerarşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Hiyerarşik varlık deneyin [hızlı](luis-quickstart-intent-and-hier-entity.md) hiyerarşik bir varlık kullanma hakkında daha fazla bilgi için.

### <a name="composite-entity"></a>Bileşik varlık
Bileşik bir varlık, bir bütün bölümleri form ait diğer varlıkların yapılır. 

![Bileşik varlık](./media/luis-plan-your-app/composite-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#composite-entity-data) JSON sorgu yanıtı uç noktasından Bileşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Bileşik varlık deneyin [öğretici](luis-tutorial-composite-entity.md) bileşik bir varlık kullanma hakkında daha fazla bilgi için.

### <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık
LUIS sağlar [önceden oluşturulmuş varlıklarla](luis-prebuilt-entities.md) ister genel türleri için `Number`, anahtarların bir bilet sırada sayısı için kullanabilirsiniz.

![Sayı önceden oluşturulmuş varlık](./media/luis-plan-your-app/number-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#prebuilt-entity-data) normal ifade varlıkları JSON sorgu yanıtı uç noktasından ayıklama hakkında daha fazla bilgi edinmek için. 

### <a name="list-entity"></a>Liste varlığı 
Açıkça belirtilen bir liste değerlerinin bir listesini varlıktır. Her değer bir veya birden çok eş anlamlılar oluşur. Bir seyahat uygulamasında havaalanı adları göstermek için bir liste varlığı seçebilirsiniz.

![Liste varlığı](./media/luis-plan-your-app/list-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#list-entity-data) JSON sorgu yanıtı uç noktasından listesi varlık ayıklama hakkında daha fazla bilgi edinmek için. Deneyin [hızlı](luis-quickstart-intent-and-list-entity.md) liste varlığı kullanma hakkında daha fazla bilgi için.

### <a name="regular-expression-entity"></a>Normal ifade varlık
Bir normal ifade varlık, bir normal ifade ifadeye göre bir utterance verileri ayıklamak LUIS sağlar.

![Normal ifade varlık](./media/luis-plan-your-app/regex-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#regular-expression-entity-data) normal ifade varlıkları JSON sorgu yanıtı uç noktasından ayıklama hakkında daha fazla bilgi edinmek için. Deneyin [hızlı](luis-quickstart-intents-regex-entity.md) bir normal ifade varlık kullanma hakkında daha fazla bilgi için.

## <a name="after-getting-endpoint-utterances"></a>Konuşma uç noktası aldıktan sonra
Tahmin geliştirmelerle uygulamak uygulamanızı konuşma uç noktası aldıktan sonra plan [etkin olarak öğrenmeye](luis-how-to-review-endoint-utt.md), [tümcecik listeleri](luis-concept-feature.md), ve [desenleri](luis-concept-patterns.md). 

### <a name="patternany-entity"></a>Pattern.Any varlık
Patterns.Any bir yer tutucudur kullanılan yalnızca değişken uzunluklu bir [deseninin](luis-concept-patterns.md) varlık burada başlar ve biter işaretlemek için şablon utterance. Şablon konuşma uygun [doğru sözdizimi](luis-concept-patterns.md#pattern-syntax) varlıkları ve Ignorable metin tanımlamak için.


## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [Language Understanding Intelligent Services (LUIS) ilk uygulamanızı oluşturma](luis-get-started-create-app.md) LUIS uygulaması oluşturma Hızlı Kılavuz.