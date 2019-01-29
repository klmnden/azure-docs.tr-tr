---
title: Alan, örnek olay incelemesini varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si alan, örnek olay incelemesini varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 793b35d9c6412c40a87f3f91fcd772476d57584f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154576"
---
# <a name="field-of-study-entity"></a>Örnek olay incelemesini varlık alanı

<sub> * Şu öznitelikleri örnek olay incelemesini varlığının alanıyla özgüdür. (Ty = '6') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
FN      |Örnek olay incelemesini normalleştirilmiş ad alanı         |Dize     |Eşittir
DFN     |Örnek olay incelemesini görünen ad alanı            |Dize     |yok
BİLGİ      |Örnek olay incelemesini toplam alıntı sayısı alanı    |Int32      |yok  
ECC     |Alanın toplam tahmini alıntı sayısı|Int32      |yok
FL      |Örnek olay incelemesini hiyerarşi alanlarında düzeyi     |Int32      |Eşittir <br/>IsBetween
FP.FN   |Örnek olay incelemesini ad alanının üst             |Dize     |Eşittir
FP.FId  |Üst araştırma Kimliği alanı               |Int64      |Eşittir
FC.FN   |Örnek olay incelemesini adının alt alanı              |Dize     |Eşittir
FC.FId  |Alt alan çalışma kimliği                |Int64      |Eşittir
