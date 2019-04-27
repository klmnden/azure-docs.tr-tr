---
title: Yazar varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si Yazar varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: d4b33c06ab023023aadf403cf0ef0b08c2bafc5f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609753"
---
# <a name="author-entity"></a>Yazar varlık
<sub> * Şu öznitelikleri Yazar varlığa özgüdür. (Ty = '1') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AuN     |Normalleştirilmiş yazar adı                 |String     |Eşittir
DAuN    |Yazar görünen adı                    |String     |yok
BİLGİ      |Yazar Alıntısı toplam sayısı            |Int32      |yok  
ECC     |Yazar toplam tahmini alıntı sayısı  |Int32      |yok
E       |Genişletilmiş meta verileri ("Genişletilmiş Meta öznitelikleri" Tablo bakın)  |String     |yok  


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
LKA. AFN     | Yazar ile ilişkili bağlantı'nın görünen adı  
LKA.AfId        | Yazar ile ilişkili bağlantı'nın varlık kimliği
