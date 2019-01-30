---
title: Varlıklar için roller
titleSuffix: Azure Cognitive Services
description: Adlandırılmış ve bağlamsal subtypes yalnızca desenlerinde kullanılan bir varlığın rolleridir. Örneğin utterance içinde `buy a ticket from New York to London`, New York hem London olan şehirleri ancak her bir tümce içinde farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: diberry
ms.openlocfilehash: f5768012a03629190a65dd86d004dc842d99912e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215955"
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


[!INCLUDE [H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="example-role-for-entities"></a>Varlıklar için rol örneği

Yalnızca bir varlık içindeki bir utterance bağlamsal öğrenilen yerleşimini rolüdür. Utterance birden fazla varlık türü olduğunda en etkili. Ayırt etmek için herhangi bir varlık türü için kolay örneği olan bir konum gelen ve giden. Çok sayıda farklı varlık türleri, konumu temsil edilebilir. 

Bir örnek kullanım örneği bir çalışan bir bölümden diğerine her departman bir listedeki bir öğe olduğu aktarıyor. Örneğin: 

`Move [PersonName] from [Department:from] to [Department:to]`. 

Döndürülen tahmin, her iki bölüm varlıkları JSON yanıtta döndürülecek ve her rol adı yer alır. 

## <a name="roles-with-prebuilt-entities"></a>Önceden oluşturulmuş varlıklarla rolleri

Roller, farklı bir utterance içinde önceden oluşturulmuş varlık örneklerini anlam katmak için önceden oluşturulmuş varlıklarla kullanın. 

### <a name="roles-with-datetimev2"></a>DatetimeV2 rolleriyle

Önceden oluşturulmuş varlığı datetimeV2, çok çeşitli çeşitli tarihler ve saatler konuşma anlama, harika bir iş yapar. Tarihleri ve tarih aralıklarını önceden oluşturulmuş varlığın varsayılan anlama'dan farklı şekilde belirtmek isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* Eklemeyi öğrenin [rolleri](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
