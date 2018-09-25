---
title: C# ' ta Speech SDK'sı kullanarak konuşma tanıma
titleSuffix: Microsoft Cognitive Services
description: >
  Konuşma tanımayı öğrenmesine (özelleştirilmiş modeli, sürekli bir mikrofondan gelen bir dosyadan veya kesin) Speech SDK'sı için C# kullanarak.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 842452028671ef3db91348886676a95120335d86
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972853"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-c"></a>C# ' ta Speech SDK'sı kullanarak konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-csharp[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!INCLUDE [Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-csharp[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionCustomized)]

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-csharp[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionContinuousWithFile)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu makaledeki örnekleri/csharp/sharedcontent/Konsolu klasör kullandığınız kodunu arayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma amacından anlamayı](how-to-recognize-intents-from-speech-csharp.md)
- [Nasıl konuşma Çevir](how-to-translate-speech-csharp.md)

