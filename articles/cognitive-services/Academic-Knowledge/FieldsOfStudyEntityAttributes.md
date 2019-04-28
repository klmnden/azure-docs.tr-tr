---
title: Alan, örnek olay incelemesini varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si alan, örnek olay incelemesini varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: e9d6badf76efd03c0520a728af7b3e47b25f200a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61339594"
---
# <a name="field-of-study-entity"></a>Örnek olay incelemesini varlık alanı

<sub> * Şu öznitelikleri örnek olay incelemesini varlığının alanıyla özgüdür. (Ty = '6') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
FN      |Örnek olay incelemesini normalleştirilmiş ad alanı         |String     |Eşittir
DFN     |Örnek olay incelemesini görünen ad alanı            |String     |yok
BİLGİ      |Örnek olay incelemesini toplam alıntı sayısı alanı    |Int32      |yok  
ECC     |Alanın toplam tahmini alıntı sayısı|Int32      |yok
FL      |Örnek olay incelemesini hiyerarşi alanlarında düzeyi     |Int32      |Eşittir <br/>IsBetween
FP.FN   |Örnek olay incelemesini ad alanının üst             |String     |Eşittir
FP.FId  |Üst araştırma Kimliği alanı               |Int64      |Eşittir
FC.FN   |Örnek olay incelemesini adının alt alanı              |String     |Eşittir
FC.FId  |Alt alan çalışma kimliği                |Int64      |Eşittir
