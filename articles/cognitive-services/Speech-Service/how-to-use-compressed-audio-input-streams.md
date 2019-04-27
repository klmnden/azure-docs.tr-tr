---
title: Sıkıştırılmış Stream sesle Speech SDK'sı - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Sıkıştırılmış Speech SDK'sı ile Azure konuşma Hizmetleri ses akışı yapmayı öğrenin. C++ için kullanılabilir C#ve Java Linux için.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: amishu
ms.openlocfilehash: 2066dc3e20ab9fc92b23fd071728ea6a920d3324
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60697693"
---
# <a name="stream-compressed-audio-with-the-speech-sdk"></a>Sıkıştırılmış Stream ses Speech SDK'sı ile

Konuşma SDK'ın **sıkıştırılmış ses giriş Stream** API PullStream veya PushStream kullanarak konuşma hizmeti için sıkıştırılmış bir ses akışı için bir yol sağlar.

> [!IMPORTANT]
> Sıkıştırılmış bir ses akışı C++ için yalnızca desteklenir C#ve Java Linux'ta (Ubuntu 16.04 veya Ubuntu 18.04).
> Destek, MP3 ve geçerli/OGG ile sınırlıdır.

## <a name="prerequisites"></a>Önkoşullar

Sıkıştırılmış ses girişi için Linux Speech SDK'sı ile kullanmak için bu bağımlılıkları yüklemeniz gerekir:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="streaming-compressed-audio"></a>Sıkıştırılmış bir ses akışı

Bir konuşma Hizmetleri ses biçimde sıkıştırılmış akış için oluşturma `PullAudioInputStream` veya `PushAudioInputStream`. Ardından, oluşturun bir `AudioConfig` akış sıkıştırma biçimi, akış sınıfın bir örneğinden belirtme.

Adlı bir giriş akışı sınıf olduğunu varsayalım `myPushStream` ve geçerli/OGG kullanma. Bu kod aşağıdaki gibi görünebilir. 

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
