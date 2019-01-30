---
title: Veri dönüştürme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding (LUIS) Öngörüler önce Konuşma nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: c7495ba3f2dc453463038fd37147b4574536ca92
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215342"
---
# <a name="convert-data-format-of-utterances"></a>Konuşma veri biçimini Dönüştür
LUIS, Bilişsel hizmetler konuşma hizmeti konuşma metin konuşma tahmin önce konuşulan konuşma dönüştürmek için kullanır. 

## <a name="speech-to-intent-conversion-concepts"></a>Konuşma niyetini dönüştürme kavramları
LUIS metinde konuşma dönüştürülmesi konuşulan konuşma bir uç noktasına göndermesi ve LUIS tahmini yanıt almanızı sağlar. Bir tümleştirme işlemidir [konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) LUIS ile hizmet. Konuşmayı amaca dönüştürme ile hakkında daha fazla bilgi bir [öğretici](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Önemli gereksinimler
Oluşturma gerekmez bir **Bing konuşma API'si** Bu tümleştirme için anahtar. A **Language Understanding** Azure portalında oluşturulan anahtarı bu tümleştirme için çalışır. LUIS başlangıç anahtarı kullanmayın.

### <a name="pricing-tier"></a>Fiyatlandırma Katmanı
Bu tümleştirme farklı bir kullanan [fiyatlandırma](luis-boundaries.md#key-limits) fiyatlandırma katmanları normal Language Understanding daha model. 

### <a name="quota-usage"></a>Kota kullanımı
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) bilgi. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmayı metne dönüştürme kullanın](luis-tutorial-speech-to-intent.md)

