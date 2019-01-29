---
title: Akademik bilgi API'si ilişkisi varlık öznitelikleri
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si ilişkisi varlığı ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 344b26b16f74cd44982e3c93fa69295792daa9a0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190657"
---
# <a name="affiliation-entity"></a>Bağlantı varlığı

<sub> * Şu öznitelikleri ilişkisi varlığa özgüdür. (Ty = '5') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AfN     |Normalleştirilmiş ilişkisi adı        |Dize     |Eşittir
DAfN    |İlişki görünen adı       |Dize     |yok
BİLGİ      |İlişki alıntı toplam sayısı           |Int32      |yok  
ECC     |İlişki toplam tahmini alıntı sayısı |Int32      |yok

## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
PC      |Bağlantı'nın kağıt sayısı
