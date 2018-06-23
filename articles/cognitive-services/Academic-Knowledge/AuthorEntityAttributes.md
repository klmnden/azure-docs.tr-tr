---
title: Akademik bilgi API'si varlık öznitelikleri Yazar | Microsoft Docs
description: Bilişsel hizmetler akademik bilgi API'si Yazar varlıkta ile birlikte kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: afcc3257c49522a4631c4aae0e0c2b88373f9af1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351353"
---
# <a name="author-entity"></a>Yazar varlık
<sub> * Aşağıdaki öznitelikleri Yazar varlığa özgüdür. (Ty = '1') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AuN     |Normalleştirilmiş yazar adı                 |Dize     |Eşittir
DAuN    |Yazar görünen adı                    |Dize     |yok
BİLGİ      |Yazar toplam alıntı sayısı            |Int32      |yok  
ECC     |Yazar toplam tahmini alıntı sayısı  |Int32      |yok
E       |Genişletilmiş meta veriler (bkz. "Genişletilmiş Meta öznitelikleri" tablo)  |Dize     |yok  


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
LKA. AFN     | yazarın ilişkili ilişkisi'nın görünen adı  
LKA. AfId        | yazarın ilişkili ilişkisi'nın varlık kimliği