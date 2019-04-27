---
title: Konuşma SDK'sı ses giriş akışı kavramları
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'ın ses giriş akışı API özelliklerine genel bakış.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: f34504dddaa21cc162476d1fdb462e7be5c48629
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60697922"
---
# <a name="about-the-speech-sdk-audio-input-stream-api"></a>API Speech SDK'sı ses giriş akışı

Konuşma SDK'ın **ses giriş Stream** API mikrofon ya da giriş dosyası API'lerini kullanarak yerine tanıyıcıları içine ses akışları akış için bir yol sağlar.

Giriş akışları ses kullanarak aşağıdaki adımlar gereklidir:

- Ses akışı biçimini tanımlar. Biçim Speech SDK'sı ve konuşma hizmeti tarafından desteklenmesi gerekir. Şu anda yalnızca aşağıdaki yapılandırma desteklenir:

  PCM biçimi, bir kanal, saniyede 32000 bayt / saniye, 16000 örnekleri Ses örneği iki blok (16 bir örnek için doldurma dahil olmak üzere bit) cihazları, örnek başına 16 bit hizalayın.

  İlgili ses biçimi oluşturmak için SDK'sı kodda şöyle görünür:

  ```
  byte channels = 1;
  byte bitsPerSample = 16;
  int samplesPerSecond = 16000;
  var audioFormat = AudioStreamFormat.GetWaveFormatPCM(samplesPerSecond, bitsPerSample, channels);
  ```

- Kodunuzu ham ses verileri bu belirtimlere göre sağlayabilir emin olun. Ses kaynağından verilerinizi desteklenen biçimler eşleşmiyorsa, ses gerekli biçime dönüştürülebilir olmalıdır.

- Kendi ses giriş akışı sınıfından türetilen oluşturma `PullAudioInputStreamCallback`. Uygulama `Read()` ve `Close()` üyeleri. Tam işlev imzası dile bağlıdır, ancak kod bu kod örneği için benzer olacaktır:

  ```
   public class ContosoAudioStream : PullAudioInputStreamCallback {
      ContosoConfig config;

      public ContosoAudioStream(const ContosoConfig& config) {
          this.config = config;
      }

      public int Read(byte[] buffer, uint size) {
          // returns audio data to the caller.
          // e.g. return read(config.YYY, buffer, size);
      }

      public void Close() {
          // close and cleanup resources.
      }
   };
  ```

- Ses üzerinde temel bir ses yapılandırması oluşturma biçimini ve Giriş akışı. Tanıyıcı oluşturduğunuzda normal konuşma yapılandırmanızı ve ses giriş yapılandırmasını geçirin. Örneğin:

  ```
  var audioConfig = AudioConfig.FromStreamInput(new ContosoAudioStream(config), audioFormat);

  var speechConfig = SpeechConfig.FromSubscription(...);
  var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

  // run stream through recognizer
  var result = await recognizer.RecognizeOnceAsync();

  var text = result.GetText();
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
