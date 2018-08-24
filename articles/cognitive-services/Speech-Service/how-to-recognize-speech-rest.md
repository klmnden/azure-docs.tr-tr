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
ms.openlocfilehash: 24fa3882a65bf6605444a139ad5d4ee42800a8ef
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42744938"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>REST API kullanarak konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

REST API kullanarak bir HTTP POST isteği kısa konuşma tanıma için kullanılabilir.

REST API'si tarafından desteklenen bir dil kullanıyorsanız, konuşma tanıma en basit yolu, [SDK](speech-sdk.md).
Bir HTTP POST isteği için hizmet uç noktasını, isteğin gövdesindeki tüm utterance geçirin yapabilirsiniz; ve tanınan metin içeren bir yanıt almanız.

> [!NOTE]
> REST API kullanırken, konuşma 15 saniye veya daha az sınırlı olmalıdır.
> Kullanıma [Speech SDK'sı](how-to-recognize-speech-csharp.md) uzun konuşma tanıma için.

Daha fazla bilgi için **Konuşmayı metne dönüştürme** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text). Nasıl çalıştığını görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- [REST API'sine genel bakış konusuna bakın.](rest-apis.md)
