---
title: REST API kullanarak konuşma tanıma
description: Konuşmayı metne dönüştürme konuşma hizmeti kullanmayı öğrenin
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 15db2d9b872c76d70fd531af07fb55c701e86494
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331481"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>REST API kullanarak konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

REST API kullanarak bir HTTP POST isteği kısa konuşma tanıma için kullanılabilir.

REST API, SDK tarafından desteklenen bir dil kullanıyorsanız, konuşma tanıma en basit yoludur.
Hizmet uç noktası için bir HTTP POST isteği isteğin gövdesindeki tüm utterance geçirme olun.
Tanınan metni içeren bir yanıt alırsınız.

> [!NOTE]
> REST API kullanırken, konuşma 15 saniye veya daha az sınırlı olmalıdır.
> Kullanıma [Speech SDK'sı](how-to-recognize-speech-csharp.md) uzun konuşma tanıma için.

Daha fazla bilgi için **Konuşmayı metne dönüştürme** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text). Nasıl çalıştığını görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- [REST API'sine genel bakış konusuna bakın.](rest-apis.md)
