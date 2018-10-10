---
title: Yazar varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si Yazar varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 62e6da6d558a0494fb83115b1e307081099399d4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900698"
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
LKA. AfId        | Yazar ile ilişkili bağlantı'nın varlık kimliği