---
title: Yazar varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si Yazar varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 48758ac9ec8c993bbdb490229ae20fcce1fb0a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175188"
---
# <a name="author-entity"></a>Yazar varlık
<sub> * Şu öznitelikleri Yazar varlığa özgüdür. (Ty = '1') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AuN     |Normalleştirilmiş yazar adı                 |Dize     |Eşittir
DAuN    |Yazar görünen adı                    |Dize     |yok
BİLGİ      |Yazar Alıntısı toplam sayısı            |Int32      |yok  
ECC     |Yazar toplam tahmini alıntı sayısı  |Int32      |yok
E       |Genişletilmiş meta verileri ("Genişletilmiş Meta öznitelikleri" Tablo bakın)  |Dize     |yok  


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
LKA. AFN     | Yazar ile ilişkili bağlantı'nın görünen adı  
LKA.AfId        | Yazar ile ilişkili bağlantı'nın varlık kimliği
