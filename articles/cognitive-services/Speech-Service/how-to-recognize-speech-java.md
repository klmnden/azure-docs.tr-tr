---
title: Java için Speech SDK'sı kullanarak konuşma tanıma
titleSuffix: Microsoft Cognitive Services
description: >
  Konuşma tanımayı öğrenmesine (özelleştirilmiş modeli, sürekli bir mikrofondan gelen bir dosyadan veya kesin) Speech SDK'sı için Java kullanarak.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: e44bbd4a620e65f1d17ffe09a7130e624e6337bb
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43109699"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-java"></a>Java için Speech SDK'sı kullanarak konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!include[Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!include[Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#toplevel)]

[!include[Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-java[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionWithMicrophone)]

[!include[Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-java[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionCustomized)]

[!include[Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-java[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionContinuousWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu makaledeki örnekleri/java/jre/konsol klasöründe kullandığınız kodunu arayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl amaçlardan tutun konuşma tanıma](how-to-recognize-intents-from-speech-java.md)
- [Nasıl konuşma Çevir](how-to-translate-speech-java.md)

