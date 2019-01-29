---
title: Konferans serisi varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Konferans serisi varlıkla kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ff71b489cce01d8d6ea29e09905d7d3ac8429e34
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155618"
---
# <a name="conference-series-entity"></a>Konferans serisi varlık

<sub> * Şu öznitelikleri konferans serisi varlığa özgüdür. (Ty = '3') </sub>

Name    |Açıklama                            |Type       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
CN      |Konferans serisi normalleştirilmiş adı      |Dize     |Eşittir
DCN     |Konferans serisi görünen adı         |Dize     |yok
BİLGİ      |Konferans serisi toplam alıntı sayısı         |Int32      |yok  
ECC     |Konferans serisi tahmini toplam alıntı sayısı   |Int32      |yok
F.FId   |Konferans serisi ile ilişkili çalışma varlık kimliği alanı |Int64  | Eşittir
F.FN    |Konferans serisi ile ilişkili çalışma ad alanı  | Eşittir<br/>StartsWith
