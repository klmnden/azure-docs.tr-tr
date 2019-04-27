---
title: Konferans örneği varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si konferans örneği varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 183a307159adb5dfdb248eb0cf4862462a626db6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60498755"
---
# <a name="conference-instance-entity"></a>Konferans örneği varlık

<sub> * Şu öznitelikleri konferans örneği varlığa özgüdür. (Ty = '4') </sub>

Ad    |Açıklama                            |Tür       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
CIN     |Konferans örneği normalleştirilmiş adı ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |String     |Eşittir
DCN     |Konferans örneği görünen ad ({ConferenceSeriesName}: {ConferenceInstanceYear})       |String     |yok
CIL'İ     |Konferans örneği konumu    |String     |Eşittir<br/>StartsWith
CISD    |Konferans örneği başlangıç tarihi  |Tarih       |Eşittir<br/>IsBetween
CIED    |Konferans örneği bitiş tarihi    |Tarih       |Eşittir<br/>IsBetween
CIARD   |Soyut kayıt son tarihi konferans örneği  |Tarih       |Eşittir<br/>IsBetween
CISDD   |Gönderim konferans örneği son tarihi     |Tarih       |Eşittir<br/>IsBetween
CIFVD   |Son sürüm konferans örneği son tarihi  |Tarih       |Eşittir<br/>IsBetween
CINDD   |Konferans örneği bildirim tarihi   |Tarih       |Eşittir<br/>IsBetween
CD. T    |Konferans örneği olay başlığı   |Tarih       |Eşittir<br/>IsBetween
CD. D    |Konferans örneği olay tarihi    |Tarih       |Eşittir<br/>IsBetween
PCS.CN  |Örneğin konferans serisi adı |String     |Eşittir
PCS.CId |Konferans serisi kimliği örneği |Int64    |Eşittir
BİLGİ      |Konferans örneği alıntı toplam sayısı           |Int32      |yok  
ECC     |Konferans örneği toplam tahmini alıntı sayısı |Int32      |yok


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
FN      | Konferans örneği tam adı
