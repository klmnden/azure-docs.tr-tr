---
title: REST API kullanarak konuşma tanıma
description: Bilişsel hizmetler konuşma hizmeti Konuşmayı metne dönüştürme API'si kullanmayı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 54cdfdeabe8b43b079ab0c2ec6280894f217fe12
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43099950"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>REST API kullanarak konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

REST API, bir HTTP POST isteği kullanarak kısa konuşma tanıma için kullanılabilir.

REST API'si tarafından desteklenen bir dil kullanmıyorsanız, konuşma tanıma en basit yolu, [SDK](speech-sdk.md). İstek gövdesindeki tüm utterance geçirin ve hizmet uç noktası için bir HTTP POST isteği olun. Tanınan metni içeren bir yanıt alırsınız.

> [!NOTE]
> Konuşma 15 saniye veya REST API'yi kullandığınızda, daha az sınırlıdır.
> Kullanıma [Speech SDK'sı](how-to-recognize-speech-csharp.md) uzun konuşma tanıma için.

Daha fazla bilgi için **Konuşmayı metne dönüştürme** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text) makalesi. API iş başında görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [REST API'sine genel bakış](rest-apis.md).
