---
title: Konferans serisi varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Konferans serisi varlıkla kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f9f28afd7005d7a61aa0d2f4dba69ca598034b52
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900757"
---
# <a name="conference-series-entity"></a>Konferans serisi varlık

<sub> * Şu öznitelikleri konferans serisi varlığa özgüdür. (Ty = '3') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
CN =      |Konferans serisi normalleştirilmiş adı      |Dize     |Eşittir
DCN     |Konferans serisi görünen adı         |Dize     |yok
BİLGİ      |Konferans serisi toplam alıntı sayısı         |Int32      |yok  
ECC     |Konferans serisi tahmini toplam alıntı sayısı   |Int32      |yok
F.FId   |Konferans serisi ile ilişkili çalışma varlık kimliği alanı |Int64  | Eşittir
F.FN    |Konferans serisi ile ilişkili çalışma ad alanı  | Eşittir<br/>StartsWith