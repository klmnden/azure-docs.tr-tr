---
title: Rolleri düzeni tabanlı varlıklarda - Azure nasıl kullanıldığını anlama | Microsoft Docs
description: Bir rolü bağlamsal varlık alt türü için bir ad vermek için bir desen tabanlı varlığı nasıl kullanıldığını öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: ab6100e33fb767528b87c6afde4c5ef275fc7c81
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356370"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Bağlamsal alt desenleri varlık rollerinde olan
Rolleri, yalnızca kullanılan bir varlığın adlandırılmış, bağlamsal subtypes [desenleri](luis-concept-patterns.md).

Örneğin, utterance içinde `buy a ticket from New York to London`, New York ve Londra olan Şehir ancak her cümlede farklı bir anlama sahip. New York kaynak Şehir ve Londra hedef şehir. 

Roller, bu farklar için bir ad verin:

|Varlık|Rol|Amaç|
|--|--|--|
|Konum|kaynak|Burada gelen düzlemi bırakır|
|Konum|Hedef|Burada düzlemi adlandırıldığını|

## <a name="how-are-roles-used-in-patterns"></a>Rolleri düzenleri nasıl kullanılır?
Bir deseninin şablonu utterance içinde rolleri içinde utterance kullanılır: 

```
buy a ticket from {Location:origin} to {Location:destination}
```

## <a name="role-syntax-in-patterns"></a>Rol sözdiziminde desenleri
Varlık ve rol parantez içinde çevrelenmiş `{}`. Varlık ve rol virgülle ayrılır. 

## <a name="roles-versus-hierarchical-entities"></a>Rolleri hiyerarşik varlıklar karşılaştırması
Hiyerarşik varlıkları aynı bağlamsal bilgileri rolleri olarak ancak yalnızca utterances içinde sağlayın **hedefleri**. Benzer şekilde, rolleri aynı bağlamsal hiyerarşik varlıklar olarak ancak yalnızca bilgilerini **desenleri**.

|Bağlamsal öğrenme|Kullanılan|
|--|--|
|hiyerarşik varlıklar|Hedefleri|
|roles|Desenler|

## <a name="next-steps"></a>Sonraki adımlar

* Eklemeyi öğrenin [rolleri](luis-how-to-add-entities.md#add-role-to-pattern-based-entity)
