---
title: Rolleri desen tabanlı varlıklarda nasıl kullanıldığını anlama
titleSuffix: Azure Cognitive Services
description: Adlandırılmış ve bağlamsal subtypes yalnızca desenlerinde kullanılan bir varlığın rolleridir. Örneğin, utterance satın alma New York'tan bilet Londra, New York hem Londra şehirler olan ancak tümcedeki her farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: aabd3a22498e0e33993d715e7a5882dde7aacf37
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632492"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Varlık desenleri bağlamsal subtypes rolleridir
Adlandırılmış ve bağlamsal subtypes yalnızca kullanılan bir varlığın rolleridir [desenleri](luis-concept-patterns.md).

Örneğin utterance içinde `buy a ticket from New York to London`, New York hem London olan şehirleri ancak her bir tümce içinde farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir. 

Rolleri, bu fark için bir ad verin:

|Varlık|Rol|Amaç|
|--|--|--|
|Konum|kaynak|Burada gelen düzlemi bırakır.|
|Konum|Hedef|Uçağın nerede gölünüzdeki|

## <a name="how-are-roles-used-in-patterns"></a>Rolleri, modelleri nasıl kullanılır?
Bir desenin şablon utterance içinde rolleri içinde utterance kullanılır: 

|Varlık rolleriyle deseni|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Rol sözdizimi desenleri
Varlık ve rol parantez içinde içine alınmış `{}`. Varlık ve rol virgül ile ayrılır. 

## <a name="roles-versus-hierarchical-entities"></a>Hiyerarşik varlıkları ve rolleri
Hiyerarşik varlıkları aynı bağlamsal bilgi roller olarak ancak yalnızca içinde konuşma sağlayan **hedefleri**. Benzer şekilde, hiyerarşik varlıklar olarak ancak yalnızca aynı bağlamsal bilgi roller sağlar **desenleri**.

|Bağlamsal öğrenme|Kullanılan|
|--|--|
|hiyerarşik varlıklar|Hedefleri|
|roles|Desenleri|

## <a name="next-steps"></a>Sonraki adımlar

* Eklemeyi öğrenin [rolleri](luis-how-to-add-entities.md#add-role-to-pattern-based-entity)
