---
title: Çeviri için örnek | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Burada, konuşma çeviri için bir örnek verilmiştir.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: a02a89de1d05ec5388a92f14a0fd206e0ee966a6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355619"
---
# <a name="sample-for-translation"></a>Çeviri için örnek

> [!NOTE]
> Bu örnek ve diğerleri karşıdan yüklemek yönergeleri için bkz: [konuşma SDK'sı için örnek](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Aşağıdaki tüm örnekler için aşağıdaki üst düzey bildirimleri yerinde olmalıdır:
>
> [!code-csharp [Using Statements](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#toplevel)]
>
> - - -

## <a name="translation-using-the-microphone"></a>Mikrofon kullanarak çevirisi

Aşağıdaki kod parçacığında, İngilizce konuşma girişten Almanca için çevirin ve ayrıca çevrilmiş metni sesli çıktısını almak gösterilmektedir. Mikrofon kullanır.

[!code-csharp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithMicrophoneAsync)]

- - -

## <a name="translation-using-file-input"></a>Çeviri dosyası girişi kullanma

Aşağıdaki kod parçacığında, Almanca ve Fransızca için İngilizce konuşma girişten Çevir gösterilmektedir.
Bu dosya giriş olarak kullanır.

[!code-csharp[Translation Using File Input](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithFileAsync)]

- - -

## <a name="sample-source-code"></a>Örnek kaynak kodu

En son sürümünü örnekleri ve hatta daha gelişmiş örnekleri olan ayrılmış bir [GitHub deposunu](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma tanıma](./speech-to-text-sample.md)

- [Hedefi tanıma](./intent.md)
