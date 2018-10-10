---
title: Akademik bilgi API'si ilişkisi varlık öznitelikleri
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si ilişkisi varlığı ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 177fe9da8bbe821a69eae02d89a225e5d4009331
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900487"
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