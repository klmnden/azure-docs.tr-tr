---
title: C++ için Speech SDK'sı kullanarak konuşma tanıma
titleSuffix: Microsoft Cognitive Services
description: >
  Konuşma tanımayı öğrenmesine (özelleştirilmiş modeli, sürekli bir mikrofondan gelen bir dosyadan veya kesin) için C++ Speech SDK'sı kullanarak.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 698f2738294c159239203dd77bd64dd9b0586be2
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43110964"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-c"></a>C++ için Speech SDK'sı kullanarak konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!include[Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!include[Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#toplevel)]

[!include[Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-cpp[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

[!include[Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-cpp[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

[!include[Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-cpp[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu makaledeki örnekleri/cpp/windows/Konsolu klasöründe kullandığınız kodunu arayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl amaçlardan tutun konuşma tanıma](how-to-recognize-intents-from-speech-cpp.md)
- [Nasıl konuşma Çevir](how-to-translate-speech-cpp.md)

