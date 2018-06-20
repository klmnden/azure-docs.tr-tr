---
title: HALUK uygulamalarınızı planlama | Microsoft Docs
description: İlgili uygulamayı hedefleri ve varlıkları anahat ve ardından uygulama planlarınızı dil anlama akıllı Hizmetleri (HALUK içinde) oluşturun.
services: cognitive-services
author: DeniseMak
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 7aec5d5b90ac7145ce9f337ec74c590b4b88c6b1
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266371"
---
# <a name="plan-your-luis-app"></a>HALUK uygulamanızı planlama

İçinde HALUK oluşturmaya başlamadan önce uygulamanızın planlama önemlidir. Anahat veya olası amaçları ve uygulamanızın etki alanına özgü konusunda ilgili varlıklar şeması hazırlayın.  

## <a name="identify-your-domain"></a>Etki alanınızı tanımlayın
HALUK uygulama bir etki alanına özgü konu ortalanır.  Örneğin, kayıt biletleri, uçuşlar, Oteller ve kiralık araba gerçekleştiren bir seyahat uygulama olabilir. Başka bir uygulama kullanan, uygunluk çaba izleme ve hedeflerini ayarlama ile ilgili içerik sağlayabilir. 

> [!TIP]
> HALUK sunar [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) birçok yaygın senaryolar için.
> Önceden oluşturulmuş bir etki alanına bir başlangıç noktası olarak uygulamanız için kullanabileceğiniz olmadığını denetleyin.

## <a name="identify-your-intents"></a>Hedefleri tanımlayın
Düşünün [hedefleri](luis-concept-intent.md) uygulamanızın görev için önemli olan. Bir seyahat uygulamayla bir uçuş kitap ve kullanıcının hedefte hava durumu denetlemek için işlevleri örneği atalım. Bu eylemler için "BookFlight" ve "GetWeather" hedefleri tanımlayabilirsiniz. Daha fazla işlevlere sahip daha karmaşık bir uygulama, daha fazla amacı vardır ve bunları dikkatle çok belirli olmaması amacıyla tanımlamanız gerekir. Örneğin, "BookFlight" ve "BookHotel" ayrı hedefleri olması gerekebilir, ancak "BookInternationalFlight" ve "BookDomesticFlight" çok benzer olabilir.

> [!NOTE]
> Yalnızca en iyi uygulamadır, uygulamanızın işlevleri gerçekleştirmek gerektiği kadar amaçlar. Çok fazla hedefleri tanımlarsanız, utterances doğru sınıflandırmak HALUK için daha zor olur. Çok tanımlarsanız birkaç, bunlar olabilir üst üste seçeceğine şekilde genel.


## <a name="identify-your-entities"></a>Varlıklarınızı tanımlayın
Bir uçuş kitap için hedef, tarih, hava yolu, bilet kategori gibi bazı bilgiler gerekir ve sınıf seyahat. Bunlar olarak ekleyebileceğiniz [varlıklar](luis-concept-entity-types.md) amacına işlemi gerçekleştirmek için önemli olduğundan. 

Uygulamanızda kullanmak için hangi varlıkların karar verirken, farklı türdeki nesneler arasındaki ilişkileri yakalamak için varlık türlerini olduğunu aklınızda bulundurun. [HALUK varlıklarda](luis-concept-entity-types.md) farklı türleri hakkında daha fazla ayrıntı sağlar.

### <a name="simple-entity"></a>Basit varlık
Bir varlığın tek bir kavramı açıklanmaktadır.

![Basit varlık](./media/luis-plan-your-app/simple-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#simple-entity-data) Basit varlık JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Basit varlık deneyin [Hızlı Başlangıç](luis-quickstart-primary-and-secondary-data.md) basit bir varlık kullanma hakkında daha fazla bilgi için.

### <a name="hierarchical-entity"></a>Hiyerarşik varlık
Hiyerarşik bir varlık özel türünde bir **basit** varlık; üst-alt ilişkisi formunda bir kategori ve üyeleri tanımlama.

![hiyerarşik varlık](./media/luis-plan-your-app/hierarchical-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#hierarchical-entity-data) hiyerarşik varlık JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Hiyerarşik varlık deneyin [Hızlı Başlangıç](luis-quickstart-intent-and-hier-entity.md) hiyerarşik bir varlık kullanma hakkında daha fazla bilgi için.

### <a name="composite-entity"></a>Bileşik varlık
Bileşik bir varlık, bir bütün bölümlerini form ait diğer varlıkların yapılır. 

![Bileşik varlık](./media/luis-plan-your-app/composite-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#composite-entity-data) Bileşik varlık JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Bileşik varlık deneyin [öğretici](luis-tutorial-composite-entity.md) Bileşik varlık kullanma hakkında daha fazla bilgi için.

### <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık
HALUK sağlar [önceden oluşturulmuş varlıklar](Pre-builtEntities.md) karşılaşılan ister için `Number`, anahtarların bir bilet sırayla sayısı için kullanabilirsiniz.

![Sayı önceden oluşturulmuş varlık](./media/luis-plan-your-app/number-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#prebuilt-entity-data) normal ifade varlıkları JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. 

### <a name="list-entity"></a>Liste varlık 
Açıkça belirtilmiş bir değer listesi bir liste varlıktır. Her değer bir veya daha fazla anlamlıları oluşur. Seyahat uygulamada havaalanı adlarını göstermek için bir liste varlık oluşturmak isteyebilirsiniz.

![Liste varlık](./media/luis-plan-your-app/list-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#list-entity-data) listesi varlıklar JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Deneyin [Hızlı Başlangıç](luis-quickstart-intent-and-list-entity.md) listesi varlığı kullanma hakkında daha fazla bilgi için.

### <a name="regular-expression-entity"></a>Normal ifade varlık
Bir normal ifade varlık bir regex ifadesi temelinde utterance veri ayıklamak HALUK sağlar.

![Normal ifade varlık](./media/luis-plan-your-app/regex-entity.png)

Bkz: [veri ayıklama](luis-concept-data-extraction.md#regular-expression-entity-data) normal ifade varlıkları JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Deneyin [Hızlı Başlangıç](luis-quickstart-intents-regex-entity.md) bir normal ifade varlık kullanma hakkında daha fazla bilgi için.

## <a name="after-getting-endpoint-utterances"></a>Uç nokta utterances aldıktan sonra
Tahmin geliştirmelerle uygulamak uç nokta utterances uygulamanızı aldıktan sonra planlama [etkin öğrenme](label-suggested-utterances.md), [tümceyi listeleri](luis-concept-feature.md), ve [desenleri](luis-concept-patterns.md). 

### <a name="patternany-entity"></a>Pattern.Any varlık
Patterns.Any bir yer tutucudur yalnızca kullanılan değişken uzunlukta bir [deseninin](luis-concept-patterns.md) burada varlık başlar ve biter işaretlemek için şablon utterance. Şablon utterances uygun [uygun sözdizimi](luis-concept-patterns.md#pattern-syntax) varlıkları ve yoksayılabilir metin tanımlamak için.


## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [ilk dil anlama akıllı Hizmetleri (HALUK) uygulamanızı oluşturma] [ luis-get-started-create-app] HALUK uygulama oluşturmak nasıl hızlı bir kılavuz için.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app