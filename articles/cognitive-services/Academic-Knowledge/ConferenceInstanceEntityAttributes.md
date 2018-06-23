---
title: Akademik bilgi API'si konferans örneği varlık öznitelikleri | Microsoft Docs
description: Bilişsel hizmetler akademik bilgi API'si konferans örneği varlıkta ile birlikte kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ef2bca4346a4666905f3dfb7bd448720f3b0ef8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351395"
---
# <a name="conference-instance-entity"></a>Konferans örneği varlık

<sub> * Aşağıdaki öznitelikleri konferans örneği varlığa özgüdür. (Ty = '4') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
CIN     |Konferans örnek normalleştirilmiş adı ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Dize     |Eşittir
DCN     |Konferans örneği görünen adı ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Dize     |yok
CIL'İ     |Konferans örneğinin konumu    |Dize     |Eşittir<br/>StartsWith
CISD    |Konferans örneğinin başlangıç tarihi  |Tarih       |Eşittir<br/>IsBetween
CIED    |Bitiş tarihi konferans örneği    |Tarih       |Eşittir<br/>IsBetween
CIARD   |Soyut kayıt konferans örneğinin son tarih  |Tarih       |Eşittir<br/>IsBetween
CISDD   |Gönderme konferans örneğinin son tarih     |Tarih       |Eşittir<br/>IsBetween
CIFVD   |Son sürümü konferans örneğinin son tarih  |Tarih       |Eşittir<br/>IsBetween
CINDD   |Bildirim tarih konferans örneği   |Tarih       |Eşittir<br/>IsBetween
CD. T    |Konferans örnek olay başlığı   |Tarih       |Eşittir<br/>IsBetween
CD. D    |Konferans örneği olayın tarihi    |Tarih       |Eşittir<br/>IsBetween
BİLGİSAYARLAR. CN =  |Örneğinin konferans seri adı |Dize     |Eşittir
BİLGİSAYARLAR. CID |Örneğin konferans serisi kimliği |Int64    |Eşittir
BİLGİ      |Konferans örnek toplam alıntı sayısı           |Int32      |yok  
ECC     |Konferans örnek toplam tahmini alıntı sayısı |Int32      |yok


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
FN      | Tam ad konferans örneği