---
title: AudioInputStream kavramları | Microsoft Docs
description: AudioInputStream API özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
manager: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: 528356473c4221a815fa68cbec3426866c4cbd23
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356293"
---
# <a name="about-the-audio-input-stream-api"></a>API hakkında ses giriş akışı

**Ses giriş akışı** API mikrofon veya giriş dosyası API'leri kullanmak yerine tanıyıcıları içine ses akışları akışı için bir yol sağlar.

## <a name="api-overview"></a>API’ye genel bakış

API iki bileşenleri kullanan `AudioInputStream` (ses ham veriler) ve `AudioInputStreamFormat`.

`AudioInputStreamFormat` Ses verilerin biçimini tanımlar. Standart karşılaştırılabilir `WAVEFORMAT` wave dosyaları Windows yapısı.

  - `FormatTag`

    Ses biçimi. Konuşma SDK'sı şu anda yalnızca destekler `format 1` (PCM - little endian).

  - `Channels`

    Kanal sayısı. Geçerli konuşma hizmeti, yalnızca bir kanal (mono) ses malzeme destekler.

  - `SamplesPerSec`

    Örnek hızı. Tipik mikrofon kaydı saniyede 16000 örnek sahiptir.

  - `AvgBytesPerSec`

    İkinci olarak, başına ortalama bayt hesaplanan `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Saniye başına ortalama bayt değişken bit kullanan ses akışları için farklı olabilir.

  - `BlockAlign`

    Tek bir çerçeve boyutunu hesaplanan olarak `Channels * ceil(wBitsPerSample, 8)`. Doldurma nedeniyle gerçek değeri bu değerden daha yüksek olabilir.

  - `BitsPerSample`

    Örnek başına bit. Tipik bir ses akışını örnek (CD kalite) başına 16 bit kullanır.

`AudioInputStream` Temel sınıf özel akış bağdaştırıcınız tarafından geçersiz. Bu işlevler uygulamak bu bağdaştırıcı sahiptir:

   - `GetFormat()`

     Bu işlev, ses akışı biçimi almak için çağrılır. Bir işaretçi AudioInputStreamFormat arabelleğe alır.

   - `Read()`

     Bu işlev, ses akışından veri almak için çağrılır. Ses verileri kopyalamak için arabellek için bir işaretçi bir parametredir. İkinci parametre arabellek boyutu ' dir. İşlev arabelleğe kopyalanan bayt sayısını döndürür. Dönüş değeri `0` akışın sonuna gösterir.

   - `Close()`

     Bu işlev, ses akışı kapatmak için çağrılır.

## <a name="usage-examples"></a>Kullanım örnekleri

Genel olarak, aşağıdaki adımları ses giriş akışları kullanırken oynayan:

  - Ses akışı biçimi tanımlayın. Biçimi SDK ve konuşma hizmeti tarafından desteklenmesi gerekir. Şu anda aşağıdaki yapılandırma desteklenir:

    Bir ses biçimi etiketi (PCM), bir kanalı, saniye başına 16000 örnekleri, saniyede 32000 bayt iki Blok Hizalama (16 bir örnek için doldurmayı dahil olmak üzere bit), örnek başına 16 bit

  - Kodunuzu ham ses verileri yukarıda tanımlanan belirtimlerin konusunda sağlayabilir emin olun. Ses kaynağı verilerinizi desteklenen biçimler eşleşmiyorsa, ses gerekli biçime dönüştürülebilir olmalıdır.

  - Özel ses giriş akışı sınıfından türetilen `AudioInputStream`. Uygulama `GetFormat()`, `Read()`, ve `Close()` işlemi. Tam işlev imzası dile bağlı olmakla birlikte, kod bu kod örneği şuna benzer::

    ```
     public class ContosoAudioStream : AudioInputStream {
        ContosoConfig config;

        public ContosoAudioStream(const ContosoConfig& config) {
            this.config = config;
        }

        public void GetFormat(AudioInputStreamFormat& format) {
            // returns format data to the caller.
            // e.g. format.FormatTag = config.XXX;
            // ...
        }

        public size_t Read(byte *buffer, size_t size) {
            // returns audio data to the caller.
            // e.g. return read(config.YYY, buffer, size);
        }

        public void Close() {
            // close and cleanup resources.
        }
     };
    ```

  - Ses giriş akışınızı kullanın:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    var result = await recognizer.RecognizeAsync();

    var text = result.GetText();

    // In some languages you need to delete the stream explicitly.
    // delete contosoStream;
    ```

  - Bazı dillerde `contosoStream` tanıma tamamlandıktan sonra açıkça silinmelidir. Tam giriş okumadan önce AudioStream serbest bırakılamıyor. Senaryo kullanarak `StopContinuousRecognitionAsync` ve `StopContinuousRecognitionAsync` Bu örnekte gösterilen bir kavram gerektirir:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    await recognizer.StartContinuousRecognitionAsync();

    // ERROR: do not delete the contosoStream before ending recognition!
    // delete contosoStream;

    await recognizer.StopContinuousRecognitionAsync();

    // OK: Safe to delete the contosoStream.
    // delete contosoStream;
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
