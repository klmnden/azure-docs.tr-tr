---
title: Rolleri desen tabanlı varlıklarda - Azure nasıl kullanıldığını anlama | Microsoft Docs
description: Bir rolü alt bağlamsal varlık türü için bir ad vermek için desen tabanlı bir varlıkta nasıl kullanıldığını öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry
ms.openlocfilehash: d2692cdce9da7428bd7b30c4feaf7347792618f5
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222712"
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

```
buy a ticket from {Location:origin} to {Location:destination}
```

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
