---
title: Amaç tanıma için örnek | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Amaç tanıma için bir örnek aşağıda verilmiştir.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 3e9afc990d6bfa73eb045e7ed76dfd194df309c6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213217"
---
# <a name="sample-for-intent-recognition"></a>Örnek amaç tanıma

Lütfen bir abonelik anahtarı edinin. Amaç tanıma Hizmetleri Bilişsel hizmet konuşma SDK'sı tarafından desteklenen diğer hizmetleri aksine, belirli bir abonelik anahtarı gerektirir. [Burada](https://www.luis.ai) niyeti tanıma teknolojisi hakkında ek bilgi yanı sıra, bir abonelik anahtarı alma hakkında bilgi bulabilirsiniz. Kendi dil anlama abonelik anahtarını değiştirin [aboneliğinizin bölgesi](regions.md)ve hedefi modelinizin örneklerdeki uygun yerlerde AppID.

## <a name="top-level-declarations"></a>Üst düzey bildirimleri

Aşağıdaki tüm örnekler için aşağıdaki üst düzey bildirimleri koşulların karşılanması:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#toplevel)]

## <a name="intent-recognition-using-microphone"></a>Mikrofon üzerinden niyeti tanıma

Aşağıdaki kod parçacığında mikrofon girişinin varsayılan dilde amacı tanımayı gösterilmektedir (`en-US`).

[!code-csharp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

[!code-java[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithMicrophone)]

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Mikrofon belirtilen dillerden niyeti tanıma

Aşağıdaki kod parçacığında, belirtilen bir dile mikrofon girişinde amacından Almanca bu durumda tanımaya gösterilmektedir (`de-de`).

[!code-csharp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithLanguage)]

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

[!code-java[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithLanguage)]

## <a name="intent-recognition-from-a-file-using-events"></a>Olayları kullanarak bir dosyadan niyeti tanıma

Kod parçacığının varsayılan dilde amacı tanımayı gösterilmektedir (`en-US`) sürekli bir şekilde. Bu kod, Ara sonuçlar gibi ek bilgilere erişim sağlar. Giriş, bir ses dosyasından alınır, desteklenen bir biçim tek kanallı (tekli) WAV / 16 KHz örnekleme oranını ile PCM.

[!code-csharp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

[!code-cpp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentContinuousRecognitionWithFile)]

[!code-java[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Tanıma](./speech-to-text-sample.md)

- [Çeviri](./translation.md)
