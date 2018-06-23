---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: e37d3ef5b6f65ad31bc19f9f8c15350014d1c9ad
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353356"
---
İle [Anomali Bulucu API](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), JSON biçiminde zaman serisi veri API uç noktasına karşıya yükleyin ve ardından sonucu API yanıtından okuyun. Zaman serisi verilerini karşıya yükleyebilir, her veri noktası içerir:  
* Zaman damgası - veri noktası için zaman damgası. Örneğin, bir UTC tarih saat dizesi kullandığı emin olun "2017-08-01T00:00:00Z"
* Değer - o veri noktası ölçümü

Sonuçları oluşur:
* Dönem - API'si anormallikleri algılamak için kullanılan dönemsellik
* WarningText - olası uyarı bilgileri
* ExpectedValue - tahmin edilen bir değer öğrenme tabanlı modeli
* IsAnomaly - sonucun veri noktalarını bozukluklar olup alan veya her iki yönde (ani veya dıps)
* IsAnomaly_Neg - veri noktaları (dıps) olumsuz yönde bozukluklar olup sonucu
* IsAnomaly_Pos - veri noktaları (ani) pozitif yönde bozukluklar olup sonucu
* Veri noktası üst sınır hala normal olarak zorlayıcı UpperMargin - ExpectedValue ve UpperMargin toplamı belirler
* LowerMargin - (ExpectedValue - LowerMargin) alt sınır veri noktası hala normal olarak düşünüldüğü olduğunu belirler

Veri sözleşmesi ayrıntılarını bulunabilir [burada](../apiref.md).

