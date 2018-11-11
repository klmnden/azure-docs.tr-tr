---
title: Rolleri desen tabanlı varlıklarda nasıl kullanıldığını anlama
titleSuffix: Azure Cognitive Services
description: Adlandırılmış ve bağlamsal subtypes yalnızca desenlerinde kullanılan bir varlığın rolleridir. Örneğin, utterance satın alma New York'tan bilet Londra, New York hem Londra şehirler olan ancak tümcedeki her farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8a92852a2721bd391ddf7c3cf3489b820c4a1400
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277626"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Varlık desenleri bağlamsal subtypes rolleridir
Adlandırılmış ve bağlamsal subtypes yalnızca kullanılan bir varlığın rolleridir [desenleri](luis-concept-patterns.md).

Örneğin utterance içinde `buy a ticket from New York to London`, New York hem London olan şehirleri ancak her bir tümce içinde farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir. 

Rolleri, bu fark için bir ad verin:

|Varlık|Rol|Amaç|
|--|--|--|
|Konum|kaynak|Burada gelen düzlemi bırakır.|
|Konum|Hedef|Uçağın nerede gölünüzdeki|
|Önceden oluşturulmuş datetimeV2|-|Bitiş tarihi|
|Önceden oluşturulmuş datetimeV2|başlangıç|Başlangıç tarihi|

## <a name="how-are-roles-used-in-patterns"></a>Rolleri, modelleri nasıl kullanılır?
Bir desenin şablon utterance içinde rolleri içinde utterance kullanılır: 

|Varlık rolleriyle deseni|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Rol sözdizimi desenleri
Varlık ve rol parantez içinde içine alınmış `{}`. Varlık ve rol virgül ile ayrılır. 


[!INCLUDE[H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="roles-with-prebuilt-entities"></a>Önceden oluşturulmuş varlıklarla rolleri

Roller, farklı bir utterance içinde önceden oluşturulmuş varlık örneklerini anlam katmak için önceden oluşturulmuş varlıklarla kullanın. 

### <a name="roles-with-datetimev2"></a>DatetimeV2 rolleriyle

Önceden oluşturulmuş varlığı datetimeV2, çok çeşitli çeşitli tarihler ve saatler konuşma anlama, harika bir iş yapar. Tarihleri ve tarih aralıklarını önceden oluşturulmuş varlığın varsayılan anlama'dan farklı şekilde belirtmek isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* Eklemeyi öğrenin [rolleri](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
