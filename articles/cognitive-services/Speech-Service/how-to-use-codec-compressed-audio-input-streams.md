---
title: Stream codec Speech SDK'sı - konuşma Hizmetleri ile ilgili ses sıkıştırılmış
titleSuffix: Azure Cognitive Services
description: Sıkıştırılmış Speech SDK'sı ile Azure konuşma Hizmetleri ses akışı yapmayı öğrenin. C++ için kullanılabilir C#ve Java Linux için.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: amishu
ms.openlocfilehash: d23190dc8f7980cb8a94ba295f45ae67fc7d4678
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605085"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Codec bileşenini kullanarak sıkıştırılmış Speech SDK'sı ile ses girişi

Konuşma SDK'ın **sıkıştırılmış ses giriş Stream** API PullStream veya PushStream kullanarak konuşma hizmeti için sıkıştırılmış bir ses akışı için bir yol sağlar.

> [!IMPORTANT]
> Sıkıştırılmış bir ses akışı yalnızca desteklenen için C++, C#ve Java Linux'ta (Ubuntu 16.04, 18.04 Ubuntu, Debian 9).
> Konuşma SDK'sı sürüm 1.4.0 veya üzeri bir sürüm gereklidir.

WAV/PCM için ana hat konuşma belgelerine bakın.  WAV/PCM dışında aşağıdaki sıkıştırılmış codec giriş biçimleri desteklenir:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Codec kullanmanın önkoşulları sıkıştırılmış ses girişi

Sıkıştırılmış ses girişi için Linux Speech SDK'sı ile kullanmak için bu ek bağımlılıklar'ı yükleyin:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Örnek kod codec bileşenini kullanarak sıkıştırılmış ses girişi

Bir konuşma Hizmetleri ses biçimde sıkıştırılmış akış için oluşturma `PullAudioInputStream` veya `PushAudioInputStream`. Ardından, oluşturun bir `AudioConfig` akış sıkıştırma biçimi, akış sınıfın bir örneğinden belirtme.

Adlı bir giriş akışı sınıf olduğunu varsayalım `myPushStream` ve geçerli/OGG kullanma. Kodunuz şöyle görünebilir:

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

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
