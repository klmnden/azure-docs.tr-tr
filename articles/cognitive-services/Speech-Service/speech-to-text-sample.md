---
title: Konuşma metin için örnek | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Burada, konuşma metin için bir örnek verilmiştir.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 9e0b82cc9a034f1e25ef0cc656e3770a166a6ad7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355661"
---
# <a name="sample-for-speech-to-text"></a>Konuşma metin için örnek

> [!NOTE]
> Bu örnek ve diğerleri karşıdan yüklemek yönergeleri için bkz: [konuşma SDK'sı için örnek](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Aşağıdaki tüm örnekler için aşağıdaki üst düzey bildirimleri yerinde olmalıdır:
>
> [!code-csharp[](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#toplevel)]
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="speech-recognition-using-the-microphone"></a>Konuşma tanıma mikrofon kullanma

Aşağıdaki kod parçacığında, varsayılan dilde Mikrofon konuşma girişten tanımak gösterilmektedir (`en-US`).

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

- - -

## <a name="speech-recognition-from-a-file"></a>Bir dosyadan konuşma tanıma

Aşağıdaki kod parçacığını bir ses dosyası varsayılan dilde konuşma girişten tanıdığı (`en-US`), tek kanallı (mono) WAV desteklenen biçimidir / 16 KHz örnekleme oranını ile PCM.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-csharp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs?name=recognitionFromFile)]

[!code-cpp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp?name=SpeechRecognitionWithFile)]

- - -

## <a name="speech-recognition-using-a-customized-model"></a>Özelleştirilmiş bir modeli kullanılarak konuşma tanıma

[Özel konuşma hizmet (CRI)](https://www.cris.ai/) Microsoft'un konuşma metin altyapısı, uygulamanız için özelleştirmesini sağlar. Aşağıdaki kod parçacığında CRI model kullanarak mikrofon gelen Konuşma tanıması gösterilmektedir; CRI abonelik anahtarınızı ve çalıştırmadan önce dağıtım kimlik bilgileriniz kendi doldurun.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

- - -

## <a name="continuous-speech-recognition"></a>Sürekli konuşma tanıma

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionContinuous)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Örnek kaynak kodu

En son sürümünü örnekleri ve hatta daha gelişmiş örnekleri olan ayrılmış bir [GitHub deposunu](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Sonraki adımlar

- [Hedefi tanıma](./intent.md)

- [Çeviri](./translation.md)
