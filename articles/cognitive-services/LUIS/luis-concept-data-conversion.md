---
title: Veri dönüştürme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding (LUIS) Öngörüler önce Konuşma nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 46ad8beb81f42fa19b832c63355488f31da023f8
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259385"
---
# <a name="convert-data-format-of-utterances"></a>Konuşma veri biçimini Dönüştür
LUIS, Bilişsel hizmetler konuşma hizmeti konuşma metin konuşma tahmin önce konuşulan konuşma dönüştürmek için kullanır. 

## <a name="speech-to-intent-conversion-concepts"></a>Konuşma niyetini dönüştürme kavramları
LUIS metinde konuşma dönüştürülmesi konuşulan konuşma bir uç noktasına göndermesi ve LUIS tahmini yanıt almanızı sağlar. Bir tümleştirme işlemidir [konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) LUIS ile hizmet. 

### <a name="key-requirements"></a>Önemli gereksinimler
Oluşturma gerekmez bir **Bing konuşma API'si** Bu tümleştirme için anahtar. A **Language Understanding** Azure portalında oluşturulan anahtarı bu tümleştirme için çalışır. LUIS başlangıç anahtarı kullanmayın, bu tümleştirme için çalışmaz.

### <a name="new-endpoint"></a>Yeni uç nokta 
Bu tümleştirme, yeni bir uç noktası oluşturur ve [fiyatlandırma](luis-boundaries.md#key-limits) modeli. Uç nokta aracılığıyla [Speech SDK'sı](https://github.com/Azure-Samples/cognitive-services-speech-sdk), konuşulan hem de alabilir ve metin konuşma bunları tek bir uç noktası olarak kullanabilirsiniz. 

### <a name="quota-usage"></a>Kota kullanımı
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) bilgi. 

### <a name="data-retention"></a>Veri saklama
Konuşma veya metin ise Speech SDK'sı aracılığıyla uç noktasına bakılmaksızın gönderilen veriler, yalnızca, konuşma modeli geliştirmek için kullanılır. Modelinizi konuşma veya LUIS genel bir kapasitede geliştirmek için kullanılmaz. LUIS uygulaması silindiğinde, saklanan verilere de silinir.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmayı metne dönüştürme kullanın](luis-tutorial-speech-to-intent.md)

