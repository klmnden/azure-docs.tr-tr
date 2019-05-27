---
title: Varlıklar için roller
titleSuffix: Azure Cognitive Services
description: Adlandırılmış ve bağlamsal subtypes yalnızca desenlerinde kullanılan bir varlığın rolleridir. Örneğin utterance içinde `buy a ticket from New York to London`, New York hem London olan şehirleri ancak her bir tümce içinde farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 318e71b68bbabeeef34c75a412f9fdd5b6db754a
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073021"
---
# <a name="entity-roles-for-contextual-subtypes"></a>Bağlamsal alt türleri için varlık rolleri

Rolleri subtypes adlı için varlıklar sağlar. Bir rol ile herhangi bir önceden oluşturulmuş veya özel varlık türü kullanılan ve örnek konuşma ve desenler kullanılır. 

<a name="example-role-for-entities"></a>
<a name="roles-with-prebuilt-entities"></a>

## <a name="machine-learned-entity-example-of-roles"></a>Rolleri makine öğrenilen varlık örneği

Utterance içinde "öğesinden bir bilet satın alma **New York** için **Londra**, New York hem London olan şehirleri ancak her bir tümce içinde farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir. 

```
buy a ticket from New York to London
```

Rolleri, bu fark için bir ad verin:

|Varlık türü|Varlık adı|Rol|Amaç|
|--|--|--|--|
|Basit|Location|kaynak|Burada gelen düzlemi bırakır.|
|Basit|Location|Hedef|Uçağın nerede gölünüzdeki|

## <a name="non-machine-learned-entity-example-of-roles"></a>Varlık olmayan makine öğrendiniz rol örneği

Utterance içinde "9 8 Toplantı zamanlama", her iki numarayı bir zaman belirtmek ancak her zaman içinde utterance farklı bir anlama sahiptir. Rolleri, farklar için bir ad belirtin. 

```
Schedule the meeting from 8 to 9
```

|Varlık türü|Rol adı|Değer|
|--|--|--|
|Önceden oluşturulmuş datetimeV2|StartTime|8|
|Önceden oluşturulmuş datetimeV2|EndTime|9|

## <a name="are-multiple-entities-in-an-utterance-the-same-thing-as-roles"></a>Bir utterance içinde birden çok varlık rol olarak aynı şey midir? 

Birden çok varlık içinde bir utterance bulunabilir ve rolleri kullanarak olmadan ayıklanır. Cümlenin bağlamı ile gösteriyorsa varlık sürümü bir değere sahip ve ardından bir rol kullanılmalıdır. 

### <a name="dont-use-roles-for-duplicates-without-meaning"></a>Rolleri anlamı olmadan çoğaltmaları kullanmayın

Utterance konumlarının listesini içeriyorsa `I want to travel to Seattle, Cairo, and London.`, burada her öğenin ek bir anlamı yok listesini budur. 

### <a name="use-roles-if-duplicates-indicate-meaning"></a>Çoğaltmaları anlama gösteriyorsa rolleri kullanın

Utterance konumlarla, yani listesi içeriyorsa `I want to travel from Seattle, with a layover in Londen, landing in Cairo.`, bu kaynak, layover ve hedef anlamını rolleri ile yakalanması gerekir.

### <a name="roles-can-indicate-order"></a>Rolleri sırası belirtebilirsiniz.

Utterance ayıklamak için istiyordu sırasını göstermek için değiştirdiyseniz `I want to first start with Seattle, second London, then third Cairo`, birkaç farklı şekilde ayıklayabilirsiniz. Rolü göstermek belirteçleri etiketleyebilirsiniz `first start with`, `second`, `third`. Önceden oluşturulmuş varlık da kullanabileceğinizi **sıra** ve **GeographyV2** önceden oluşturulmuş varlığında siparişi ve yerde fikrini yakalamak için bileşik bir varlık. 

## <a name="how-are-roles-used-in-example-utterances"></a>Rolleri, örnek konuşma içinde nasıl kullanılır?

Bir varlığın bir rolü vardır ve varlık içinde bir örnek utterance işaretlenmiş varlık seçerek veya varlık ve rol seçme seçeneğiniz vardır. 

Aşağıdaki örnek konuşma varlıklarını ve rolleri kullanın:

|Belirteç görüntüle|Varlığı görüntüle|
|--|--|
|Hakkında daha çok ilginizi çeken ben **Seattle**|{Location} hakkında daha fazla bilgi ilgileniyorum|
|Bir bilet Seattle'dan New York'ta satın alın.|{Konumu: Origin} öğesinden {konumu: hedef} bilet satın alma|

## <a name="how-are-roles-used-in-patterns"></a>Rolleri, modelleri nasıl kullanılır?
Bir desenin şablon utterance içinde rolleri içinde utterance kullanılır: 

|Varlık rolleriyle deseni|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Rol sözdizimi desenleri
Varlık ve rol parantez içinde içine alınmış `{}`. Varlık ve rol virgül ile ayrılır. 

## <a name="entity-roles-versus-collaborator-roles"></a>Varlık rolleri ortak çalışan rolleri ile karşılaştırması

LUIS uygulaması için veri modeli varlık rolleri uygulayabilirsiniz. [Ortak çalışan](luis-concept-collaborator.md) rolleri yazma erişimi düzeyleri için geçerlidir. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Kullanım bir [uygulamalı öğretici](tutorial-entity-roles.md) varlık rolleri makine öğrenilen varlıklar ile kullanma
* Eklemeyi öğrenin [rolleri](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
