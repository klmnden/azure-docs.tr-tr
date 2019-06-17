---
title: Günlük varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si, Bilişsel Hizmetler günlük varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ffb159dc684b4b6663dcb966706d4745ab88a403
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61337814"
---
# <a name="journal-entity"></a>Günlük varlık

<sub> * Şu öznitelikleri günlük varlığa özgüdür. (Ty = '2') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
DJN     |Normalleştirilmiş günlük adı                |String     |Yok
JN      |Günlük görünen adı                   |String     |Eşittir
CC      |Günlük toplam alıntı sayısı           |Int32      |Yok  
ECC     |Günlük toplam tahmini alıntı sayısı |Int32      |Yok
