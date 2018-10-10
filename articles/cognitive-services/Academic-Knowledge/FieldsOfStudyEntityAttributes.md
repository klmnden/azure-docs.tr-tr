---
title: Alan, örnek olay incelemesini varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si alan, örnek olay incelemesini varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 862fd6d506d5f1ca6f7f532f80f53a29200f33db
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900436"
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
FP. FN   |Örnek olay incelemesini ad alanının üst             |Dize     |Eşittir
FP. FID  |Üst araştırma Kimliği alanı               |Int64      |Eşittir
FC. FN   |Örnek olay incelemesini adının alt alanı              |Dize     |Eşittir
FC. FID  |Alt alan çalışma kimliği                |Int64      |Eşittir