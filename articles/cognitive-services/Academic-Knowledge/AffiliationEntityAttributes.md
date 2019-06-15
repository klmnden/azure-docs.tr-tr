---
title: Akademik bilgi API'si ilişkisi varlık öznitelikleri
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si ilişkisi varlığı ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 82e6a5b66342e58e62da029d617cbd1d74c28149
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61340528"
---
# <a name="affiliation-entity"></a>Bağlantı varlığı

<sub> * Şu öznitelikleri ilişkisi varlığa özgüdür. (Ty = '5') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AfN     |Normalleştirilmiş ilişkisi adı        |String     |Eşittir
DAfN    |İlişki görünen adı       |String     |Yok
CC      |İlişki alıntı toplam sayısı           |Int32      |Yok  
ECC     |İlişki toplam tahmini alıntı sayısı |Int32      |Yok

## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
PC      |Bağlantı'nın kağıt sayısı
