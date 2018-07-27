---
title: AudioInputStream kavramları
description: AudioInputStream API'nin özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: b3e12fbc616c8d67b557102c6094467e119a23f1
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281914"
---
# <a name="about-the-audio-input-stream-api"></a>API ses giriş akışı

**Ses giriş Stream** API mikrofon ya da giriş dosyası API'lerini kullanarak yerine tanıyıcıları içine ses akışları akış için bir yol sağlar.

## <a name="api-overview"></a>API’ye genel bakış

API kullanan iki bileşenden `AudioInputStream` (ses ham verileri) ve `AudioInputStreamFormat`.

`AudioInputStreamFormat` Ses veri biçimini tanımlar. Standart karşılaştırılabilir `WAVEFORMAT` Windows üzerinde wave dosya yapısı.

  - `FormatTag`

    Ses biçimi. Speech SDK'sı şu anda yalnızca destekler `format 1` (PCM - endian).

  - `Channels`

    Kanal sayısı. Geçerli konuşma hizmeti yalnızca bir kanal (mono) ses malzeme destekler.

  - `SamplesPerSec`

    Örnek hızı. Tipik mikrofon kaydı saniyede 16000 örnekleri vardır.

  - `AvgBytesPerSec`

    İkinci olarak, başına ortalama bayt hesaplanan `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Saniye başına ortalama bayt değişken bit hızlarına dönüştürme kullanan ses akışları için farklı olabilir.

  - `BlockAlign`

    Tek bir çerçeve boyutu olarak hesaplanan `Channels * ceil(wBitsPerSample, 8)`. Doldurma nedeniyle gerçek değeri bu değerden daha yüksek olabilir.

  - `BitsPerSample`

    Örnek başına bit. Tipik bir ses akışı (CD kalite) örnek başına 16 bit kullanır.

`AudioInputStream` Temel sınıf özel akış bağdaştırıcınız tarafından yazılacak. Bu işlevler uygulamak bu bağdaştırıcı sahiptir:

   - `GetFormat()`

     Ses akışı biçimi almak için bu işlev çağrılır. Bir işaretçi AudioInputStreamFormat arabelleğe alır.

   - `Read()`

     Bu işlev, ses akıştan veri almak için çağrılır. Arabellek ses verileri kopyalamak için bir işaretçi bir parametredir. Arabellek boyutu ikinci parametredir. İşlev için arabellek kopyalanan bayt sayısını döndürür. Dönüş değeri `0` akışın sonuna gösterir.

   - `Close()`

     Ses akışı kapatmak için bu işlev çağrılır.

## <a name="usage-examples"></a>Kullanım örnekleri

Genel olarak, aşağıdaki adımları kullanarak ses giriş akışları ilgilidir:

  - Ses akışı biçimini tanımlar. Biçimi SDK ve konuşma hizmeti tarafından desteklenmesi gerekir. Şu anda aşağıdaki yapılandırma desteklenir:

    Bir ses biçimi etiketi (PCM), bir kanal, saniyede 16000 örnekleri 32000 bayt / saniye, iki Blok Hizalama (16 bir örnek için doldurma dahil olmak üzere bit) cihazları, örnek başına 16 bit

  - Kodunuzu yukarıda tanımlanan özelliklerine dair ses ham veriler sağlayabilir emin olun. Ses kaynağından verilerinizi desteklenen biçimler eşleşmiyorsa, ses gerekli biçime dönüştürülebilir olmalıdır.

  - Kendi özel ses giriş akışı sınıfından türetilir `AudioInputStream`. Uygulama `GetFormat()`, `Read()`, ve `Close()` işlemi. Tam işlev imzası dile bağlıdır, ancak kod bu kod örneği için benzer olacaktır:

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

  - Ses giriş akışınız kullanın:

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

  - Bazı dillerde `contosoStream` tanıma tamamlandıktan sonra açıkça silinmelidir. Tam giriş okumadan önce AudioStream serbest bırakılamıyor. Kullanarak bir senaryo `StopContinuousRecognitionAsync` ve `StopContinuousRecognitionAsync` Bu örnekte gösterildiği bir kavram gerektirir:

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

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
