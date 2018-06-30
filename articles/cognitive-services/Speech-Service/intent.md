---
title: Örnek hedefi tanıma için | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Burada amaç tanıma için bir örnek verilmiştir.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 1c9c1e2d54ccb200ef009be3566f6da9ced01175
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111176"
---
# <a name="sample-for-intent-recognition"></a>Örnek hedefi tanıma

> [!NOTE]
> Bu örnek ve diğerleri karşıdan yüklemek yönergeleri için bkz: [konuşma SDK'sı için örnek](samples.md).

> [!NOTE]
> Lütfen abonelik anahtarı edinin. Bilişsel hizmeti konuşma SDK'sı tarafından desteklenen diğer hizmetler aksine hedefi tanıma hizmetlerinin belirli abonelik anahtarı gerektirir. [Burada](https://www.luis.ai) hedefi tanıma teknolojisi hakkında ek bilgi gibi bir abonelik anahtarı alma hakkında bilgi bulabilirsiniz. Kendi abonelik anahtarı değiştirmek [aboneliğinizin bölge](regions.md)ve hedefi örnekleri uygun yerlerde modelinizde AppID.

> [!NOTE]
> Aşağıdaki tüm örnekler için aşağıdaki üst düzey bildirimleri yerinde olmalıdır:
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="intent-recognition-using-microphone"></a>Mikrofon kullanarak hedefi tanıma

Aşağıdaki kod parçacığında mikrofon girişi varsayılan dilde amaçtan tanımak nasıl gösterir (`en-US`).

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

- - -

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Belirtilen bir dilde mikrofon kullanarak hedefi tanıma

Aşağıdaki kod parçacığında, belirli bir dilde mikrofon girişi amaçtan bu durumda Almanca tanımak gösterilmektedir (`de-de`).

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

- - -

## <a name="intent-recognition-from-a-file"></a>Bir dosyadan hedefi tanıma

Aşağıdaki kod parçacığını bir ses dosyası varsayılan dilde amaçtan tanıdığı (`en-US`), tek kanallı (mono) WAV desteklenen biçimidir / 16 KHz örnekleme oranını ile PCM.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-cpp[Intent Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithFile)]

- - -

## <a name="intent-recognition-using-events"></a>Hedefi tanıma olaylarını kullanma

Kod parçacığında, sürekli bir biçimde hedefi tanımak gösterilmiştir. Bu kod Ara sonuçların gibi ek bilgilere erişim sağlar. 

[!code-cpp[Intent Recognition Using Events](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Örnek kaynak kodu

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Tanıma](./speech-to-text-sample.md)

- [Çeviri](./translation.md)
