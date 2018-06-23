---
title: Akademik bilgi API'si konferans serisi varlık öznitelikleri | Microsoft Docs
description: Bilişsel hizmetler konferans serisi varlıkta ile birlikte kullanabileceğiniz öznitelikler hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 332736c927bdaa00334546f626a6eabb8e11d3b5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351364"
---
# <a name="conference-series-entity"></a>Konferans serisi varlık

<sub> * Aşağıdaki öznitelikleri konferans serisi varlığa özgüdür. (Ty = '3') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
CN =      |Konferans seri normalleştirilmiş adı      |Dize     |Eşittir
DCN     |Konferans serisi görünen adı         |Dize     |yok
BİLGİ      |Konferans serisi toplam alıntı sayısı         |Int32      |yok  
ECC     |Konferans serisi tahmini alıntı toplam sayısı   |Int32      |yok
F.FId   |Konferans seriyle ilişkili araştırma varlık kimliği alanı |Int64  | Eşittir
F.FN    |Konferans seriyle ilişkili olay incelemesi ad alanı  | Eşittir<br/>StartsWith