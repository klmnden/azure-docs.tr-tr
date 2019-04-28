---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 3cc0e521e43f6855397a19fe34fce99da3e20494
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60408319"
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

