---
title: Java için Speech SDK'sı kullanarak konuşma Çevir
titleSuffix: Microsoft Cognitive Services
description: Java için Speech SDK'sı kullanarak konuşma çevirme işlemi gösterilmektedir.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 098410f94d4218bed5718e77feb3c4d82b96b3cb
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887641"
---
# <a name="translate-speech-with-the-cognitive-services-speech-sdk-for-java"></a>Bilişsel hizmetler konuşma SDK'sı için Java konuşma Çevir

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/TranslationSamples.java#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

[!code-java [Translation from a microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/TranslationSamples.java#TranslationWithMicrophoneAsync)]

[!INCLUDE [Introduction to using a file](../../../includes/cognitive-services-speech-service-how-to-translate-speech-file.md)]

[!code-java [Translation from file input](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/TranslationSamples.java#TranslationWithFileAsync)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu makaledeki örnekleri/java/jre/konsol klasöründe kullandığınız kodunu arayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşmayı algılama](how-to-recognize-speech-java.md)
- [Nasıl amaçlardan tutun konuşma tanıma](how-to-recognize-intents-from-speech-java.md)
