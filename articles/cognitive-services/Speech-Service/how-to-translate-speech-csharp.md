---
title: Konuşma Speech SDK'sı için C# kullanarak çevir
titleSuffix: Azure Cognitive Services
description: Konuşma Speech SDK'sı için C# kullanarak çevirme işlemi gösterilmektedir.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: b99b120056350977df0bc671abd29c2d76c7222d
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466707"
---
# <a name="translate-speech-with-the-cognitive-services-speech-sdk-for-c"></a>Konuşma Bilişsel hizmetler konuşma SDK'sı ile C# ' ta Çevir

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

[!code-csharp[Translation from a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#TranslationWithMicrophoneAsync)]

[!INCLUDE [Introduction to using a file](../../../includes/cognitive-services-speech-service-how-to-translate-speech-file.md)]

[!code-csharp[Translation from file input](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#TranslationWithFileAsync)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu makaledeki örnekleri/csharp/sharedcontent/Konsolu klasör kullandığınız kodunu arayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşmayı algılama](how-to-recognize-speech-csharp.md)
- [Nasıl amaçlardan tutun konuşma tanıma](how-to-recognize-intents-from-speech-csharp.md)
