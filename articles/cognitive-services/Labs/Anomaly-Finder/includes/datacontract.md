---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 9280790f6692096a0b3909c9d1dfab2e94a8c0d7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904532"
---
İle [Anomali Bulucu API](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), zaman serisi verilerini JSON biçiminde API uç noktasına yükleyin ve sonra sonucu API yanıtı okuyun. Zaman serisi verilerini karşıya yükleme, her veri noktası içerir:  
* Zaman damgası - veri noktası için zaman damgası. Örneğin, bir UTC tarih saat dizesi kullandığı emin olun "2017-08-01T00:00:00Z"
* Değer - söz konusu veri noktasının ölçümü

Sonuçları oluşur:
* Süre - anormallikleri için API kullanan dönemsellik
* WarningText - olası uyarı bilgileri
* ExpectedValue - öğrenme tahmin edilen değer tabanlı modeli
* IsAnomaly - sonucu veri noktaları anomalileri olup olmadığı veya her iki yönde (ani artışlar veya Düşüşler) içinde değil
* IsAnomaly_Neg - veri noktaları (dıps) olumsuz yönde anomalileri olup üzerinde sonucu
* IsAnomaly_Pos - veri noktaları (ani) olumlu yönde anomalileri olup üzerinde sonucu
* Veri noktası üst sınırını hala normal olarak düşünülebilir ExpectedValue ve UpperMargin UpperMargin - belirler
* LowerMargin - (ExpectedValue - LowerMargin), alt sınır veri noktası hala normal olarak düşünüldüğü olduğunu belirler

Veri anlaşması ayrıntılarını bulunabilir [burada](../apiref.md).

