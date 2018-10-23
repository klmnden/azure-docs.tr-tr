---
title: LUIS - Language Understanding veri dönüştürme kavramları
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) Öngörüler önce Konuşma nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 9324f7b4f7bed844f16d17b8960878892be4b165
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638401"
---
# <a name="data-conversion-concepts-in-luis"></a>LUIS, veri dönüştürme kavramları
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

