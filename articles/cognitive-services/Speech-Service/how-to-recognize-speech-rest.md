---
title: REST API kullanarak konuşma tanıma
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler konuşma hizmeti Konuşmayı metne dönüştürme API'si kullanmayı öğrenin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 1d8ae196884d800f441943609ac0189260a0ffcf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865745"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>REST API kullanarak konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

REST API, bir HTTP POST isteği kullanarak kısa konuşma tanıma için kullanılabilir.

REST API'si tarafından desteklenen bir dil kullanmıyorsanız, konuşma tanıma en basit yolu, [SDK](speech-sdk.md). İstek gövdesindeki tüm utterance geçirin ve hizmet uç noktası için bir HTTP POST isteği olun. Tanınan metni içeren bir yanıt alırsınız.

> [!NOTE]
> Konuşma 15 saniye veya REST API'yi kullandığınızda, daha az sınırlıdır.
> Kullanıma [Speech SDK'sı](how-to-recognize-speech-csharp.md) uzun konuşma tanıma için.

Daha fazla bilgi için **Konuşmayı metne dönüştürme** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text-api) makalesi. API iş başında görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [REST API'sine genel bakış](rest-apis.md).
