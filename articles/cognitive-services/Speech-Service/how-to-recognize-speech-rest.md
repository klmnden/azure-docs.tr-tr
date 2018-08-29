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
ms.openlocfilehash: eafec2dd262098bc4b7e485293818b79debe3d27
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126855"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>REST API kullanarak konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

REST API, bir HTTP POST isteği kullanarak kısa konuşma tanıma için kullanılabilir.

REST API'si tarafından desteklenen bir dil kullanmıyorsanız, konuşma tanıma en basit yolu, [SDK](speech-sdk.md). İstek gövdesindeki tüm utterance geçirin ve hizmet uç noktası için bir HTTP POST isteği olun. Tanınan metni içeren bir yanıt alırsınız.

> [!NOTE]
> Konuşma 15 saniye veya REST API'yi kullandığınızda, daha az sınırlıdır.
> Kullanıma [Speech SDK'sı](how-to-recognize-speech-csharp.md) uzun konuşma tanıma için.

Daha fazla bilgi için **Konuşmayı metne dönüştürme** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text) makalesi. API iş başında görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [REST API'sine genel bakış](rest-apis.md).
