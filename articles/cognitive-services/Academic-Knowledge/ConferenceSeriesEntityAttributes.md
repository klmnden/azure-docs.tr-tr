---
title: Konferans serisi varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Konferans serisi varlıkla kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 38b4aa4c899668a68041f042ce6981ddd8c58219
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61340205"
---
# <a name="conference-series-entity"></a>Konferans serisi varlık

<sub> * Şu öznitelikleri konferans serisi varlığa özgüdür. (Ty = '3') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
CN      |Konferans serisi normalleştirilmiş adı      |String     |Eşittir
DCN     |Konferans serisi görünen adı         |String     |Yok
CC      |Konferans serisi toplam alıntı sayısı         |Int32      |Yok  
ECC     |Konferans serisi tahmini toplam alıntı sayısı   |Int32      |Yok
F.FId   |Konferans serisi ile ilişkili çalışma varlık kimliği alanı |Int64  | Eşittir
F.FN    |Konferans serisi ile ilişkili çalışma ad alanı  | Eşittir<br/>StartsWith
