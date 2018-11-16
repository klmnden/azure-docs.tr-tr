---
title: REST API kullanarak konuşma tanıma
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler konuşma hizmeti Konuşmayı metne dönüştürme API'si kullanmayı öğrenin.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: ceb8e929520becd2bf097fefc9c6fe8959b1bf85
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51709823"
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
