---
title: Örnek konuşma metin
titleSuffix: Microsoft Cognitive Services
description: Konuşma metni için bir örnek aşağıda verilmiştir.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 167fc57bf902f020f50c7ef8a8248cce9b882bab
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281897"
---
# <a name="sample-for-speech-to-text"></a>Örnek konuşma metin

[!include[Get a Subscription Key](../../../includes/cognitive-services-speech-service-get-subscription-key.md)]

## <a name="top-level-declarations"></a>Üst düzey bildirimleri

Aşağıdaki tüm örnekler için aşağıdaki üst düzey bildirimleri koşulların karşılanması:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#toplevel)]

## <a name="speech-recognition-using-the-microphone"></a>Mikrofon kullanarak konuşma tanıma

Aşağıdaki kod parçacığında, mikrofon varsayılan dilde konuşma girişten tanımak gösterilmektedir (`en-US`).

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

[!code-java[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionWithMicrophone)]

## <a name="speech-recognition-using-a-customized-model"></a>Özelleştirilmiş bir modeli kullanarak konuşma tanıma

[Özel konuşma hizmeti (CRI)](https://www.cris.ai/) uygulamanız için Microsoft'un Konuşmayı metne altyapısı özelleştirmesini sağlar. Aşağıdaki kod parçacığında, bir mikrofondan CRI modelinizi kullanarak konuşma tanıma gösterilmektedir; CRI abonelik anahtarınız ve çalıştırmadan önce dağıtım kimlik bilgileriniz kendi doldurun.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

[!code-java[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionCustomized)]

## <a name="continuous-speech-recognition-from-a-file"></a>Bir dosyadan sürekli konuşma tanıma

Aşağıdaki kod parçacığı bir ses dosyası varsayılan dilde konuşma girişten sürekli olarak tanır (`en-US`), tek kanallı (tekli) WAV desteklenen biçimidir / 16 KHz örnekleme oranını ile PCM.

[!include[Sample Audio](../../../includes/cognitive-services-speech-service-sample-audio.md)]

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionContinuousWithFile)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionWithFile)]

[!code-java[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionContinuousWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Amaç Tanıma](./intent.md)

- [Çeviri](./translation.md)
