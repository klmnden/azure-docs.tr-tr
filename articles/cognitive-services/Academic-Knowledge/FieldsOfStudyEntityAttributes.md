---
title: Akademik bilgi API'si olay incelemesi alan varlık öznitelikleri | Microsoft Docs
description: Bilişsel hizmetler akademik bilgi API'si olay incelemesi alan varlıkta ile birlikte kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: a8176370f5b2f63b7741f7e892ed5a8c775f19c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351359"
---
# <a name="field-of-study-entity"></a>Olay İncelemesi varlığın alan

<sub> * Aşağıdaki öznitelikleri incelemesi varlık alanına özgüdür. (Ty = '6') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
FN      |Olay İncelemesi Normalleştirilmemiş bir ad alanı         |Dize     |Eşittir
DFN     |Olay İncelemesi görünen ad alanı            |Dize     |yok
BİLGİ      |Alan incelemesi toplam alıntı sayısı    |Int32      |yok  
ECC     |Alanın toplam tahmini alıntı sayısı|Int32      |yok
FL      |Olay İncelemesi hiyerarşi alanlarında düzey     |Int32      |Eşittir <br/>IsBetween
FP. FN   |Üst alan incelemesi adı             |Dize     |Eşittir
FP. FID  |Araştırma kimliği üst alanı               |Int64      |Eşittir
FC. FN   |Alt alan incelemesi adı              |Dize     |Eşittir
FC. FID  |Araştırma kimliği alt alanı                |Int64      |Eşittir