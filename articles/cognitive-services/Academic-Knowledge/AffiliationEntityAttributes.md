---
title: Akademik bilgi API'si ilişkisi varlık öznitelikleri | Microsoft Docs
description: Bilişsel hizmetler akademik bilgi API'si ilişkisi varlıkta ile birlikte kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a0ec67cb811ca207b3d038028491da2516028f0b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351352"
---
# <a name="affiliation-entity"></a>İlişki Varlık

<sub> * Aşağıdaki öznitelikleri ilişkisi varlığa özgüdür. (Ty = '5') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AfN     |İlişki normalleştirilmiş adı        |Dize     |Eşittir
DAfN    |İlişki görünen adı       |Dize     |yok
BİLGİ      |İlişki toplam alıntı sayısı           |Int32      |yok  
ECC     |İlişki toplam tahmini alıntı sayısı |Int32      |yok

## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
PC      |İlişki'nın kağıt sayısı