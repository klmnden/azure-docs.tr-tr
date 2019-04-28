---
title: Batch Transkripsiyonu - konuşma hizmetlerini kullanma
titlesuffix: Azure Cognitive Services
description: Batch transkripsiyonu, depolama, Azure BLOB'ları gibi ses büyük bir miktarını konuşmaların istiyorsanız idealdir. Adanmış REST API'sini kullanarak bir paylaşılan erişim imzası (SAS) URI ses dosyalarının üzerine gelin ve döküm zaman uyumsuz olarak alır.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 2/20/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: b389d86fe4d23e3f4ee1c66e4270a74351098129
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61059614"
---
# <a name="why-use-batch-transcription"></a>Batch transkripsiyonu neden kullanmalısınız?

Batch transkripsiyonu, depolama, Azure BLOB'ları gibi ses büyük bir miktarını konuşmaların istiyorsanız idealdir. Adanmış REST API'sini kullanarak bir paylaşılan erişim imzası (SAS) URI ses dosyalarının üzerine gelin ve döküm zaman uyumsuz olarak alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscription-key"></a>Abonelik Anahtarı

Konuşma hizmeti tüm özellikleri ile bir abonelik anahtarı oluştururken [Azure portalında](https://portal.azure.com) izleyerek bizim [Başlarken Kılavuzu](get-started.md). Bizim temel modellerinden döküm almak planlıyorsanız, bir anahtar oluşturmak tek yapmanız gereken bir işlemdir.

>[!NOTE]
> Konuşma Hizmetleri standart aboneliği (S0), batch transkripsiyonu kullanmak için gereklidir. Ücretsiz Abonelik anahtarları (F0) işe yaramaz. Ek bilgi için bkz: [fiyatlandırma ve limitler](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Özel modelleri

Akustik veya dil modellerini özelleştirerek planlıyorsanız, adımları [akustik model özelleştirme](how-to-customize-acoustic-models.md) ve [dil modellerini özelleştirme](how-to-customize-language-model.md). Batch transkripsiyonu içinde oluşturulmuş modelleri kullanmak için model kimlikleri gerekir. Bu kimliği uç noktası Ayrıntıları görünümünde bulma uç noktası kimliği değil, model kimliği modelleri ayrıntılarını seçtiğinizde, geri alabilirsiniz.

## <a name="the-batch-transcription-api"></a>Batch tanıma API'si

Batch tanıma API'si, ek özellikleri ile birlikte zaman uyumsuz konuşma metin tanıma sunar. Bu yöntemleri gösteren bir REST API'si değil:

1. Toplu işlem isteği oluşturma
1. Sorgu durumu
1. Döküm indiriliyor

> [!NOTE]
> Batch tanıma API'si, genellikle saatlik ses binlerce accumulate çağrı merkezleri için idealdir. Bu, büyük hacimli ses kayıtlarını özelliği kolaylaştırır.

### <a name="supported-formats"></a>Desteklenen biçimler

Batch tanıma API'si, aşağıdaki biçimlerde destekler:

| Biçimlendir | Codec bileşeni | Bit hızı | Örnek hızı |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bit | 8 veya 16 kHz, mono, stereo |
| MP3 | PCM | 16-bit | 8 veya 16 kHz, mono, stereo |
| OGG | GEÇERLİ | 16-bit | 8 veya 16 kHz, mono, stereo |

Stereo ses akışları için Batch transkripsiyonu API sol ve sağ kanal döküm sırasında böler. Sonuç ile iki JSON dosyaları her tek bir kanaldan oluşturulur. Zaman damgaları utterance başına bir sıralı son döküm oluşturmak Geliştirici etkinleştirin. Bu örnek istek küfür filtresi, noktalama işaretleri ve sözcük düzeyi zaman damgaları özelliklerini içerir. 

### <a name="configuration"></a>Yapılandırma

Yapılandırma parametreleri JSON olarak sağlanır:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": ["<optional acoustic model ID>, <optional language model ID>"],
  "locale": "<local to us, for example en-US>",
  "name": "<user define name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True"
  }
}
```

> [!NOTE]
> Döküm, durum ve ilişkili sonuçları istenirken bir REST hizmeti ve Batch tanıma API'sini kullanır. Herhangi bir dilde API'den kullanabilirsiniz. Sonraki bölümde, API'yi nasıl kullanıldığını açıklar.

### <a name="configuration-properties"></a>Yapılandırma özellikleri

| Parametre | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Tanıma sonuçları küfür nasıl ele alınacağını belirtir. Kabul edilen değerler `none` , devre dışı bırakır küfür filtresi `masked` yıldız işareti ile küfür değiştirir `removed` sonuç, tüm küfür kaldırır veya `tags` "küfür" etiketleri ekler. Varsayılan ayar `masked`. | İsteğe bağlı |
| `PunctuationMode` | Noktalama işaretleri tanıma sonuçları nasıl ele alınacağını belirtir. Değerler kabul `none` , devre dışı bırakır, noktalama `dictated` açık noktalama gelir `automatic` noktalama işaretleri ile uğraşmak kod çözücü olanak tanıyan veya `dictatedandautomatic` dikte noktalama işaretleri veya otomatik olduğu anlamına gelir. | İsteğe bağlı |
 | `AddWordLevelTimestamps` | Word düzeyi zaman damgası çıkışı eklenip eklenmeyeceğini belirtir. Kabul edilen değerler `true` word düzeyi zaman damgaları sağlar ve `false` (devre dışı bırakmak için varsayılan değer). | İsteğe bağlı |

### <a name="storage"></a>Depolama

Batch transkripsiyonu destekler [Azure Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) ses ve depolamaya yazma döküm okumak için.

## <a name="webhooks"></a>Web Kancaları 

Döküm durumu için yoklama değil en yüksek performanslı olabilir ya da en iyi kullanıcı deneyimi sağlamak. Durumunu yoklamak için uzun süre çalışan döküm görevleri tamamladıktan sonra istemciyi bilgilendirir geri çağırmaları kaydedebilirsiniz.

Daha fazla ayrıntı için [Web kancaları](webhooks.md).

## <a name="sample-code"></a>Örnek kod

Tam örnek kullanılabilir [GitHub örnek deposundan](https://aka.ms/csspeech/samples) içinde `samples/batch` alt.

Örnek kod, abonelik bilgilerinizi hizmeti bölge, konuşmaların ve durumda özel bir dil ve akustik model kullanmak istediğiniz kimlik modeli için SAS ses dosyasına işaret eden URI ile özelleştirmeniz gerekir. 

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Örnek kod, istemci kurulum ve döküm isteği gönderin. Durum bilgileri ve yazdırma transkripsiyonu ilerleme ayrıntılarını ardından yoklama yapar.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Önceki çağrıları ile ilgili tüm ayrıntılar için bkz. bizim [Swagger belgesinin](https://westus.cris.ai/swagger/ui/index). Burada gösterilen tam örnek için Git [GitHub](https://aka.ms/csspeech/samples) içinde `samples/batch` alt.

Ses gönderme ve döküm durumu almak için zaman uyumsuz Kurulum not alın. Oluşturduğunuz .NET HTTP istemci istemcisidir. Var. bir `PostTranscriptions` ses dosyası ayrıntılarını göndermek için yöntem ve bir `GetTranscriptions` sonuçları almak için yöntemi. `PostTranscriptions` bir tanıtıcı döndürür ve `GetTranscriptions` transkripsiyonu durumu almak için bir tanıtıcı oluşturmak için kullanır.

Geçerli örnek kod, özel bir model belirtmez. Hizmet, dosya veya dosyalar fotoğrafını için temel modelleri kullanır. Modelleri belirtmek için model kimliklerini akustik ve dil modeli için aynı yönteme geçirebilirsiniz.

> [!NOTE]
> Temel döküm için temel modelleri kimliği bildirmeniz gerekmez. Eşleşen bir akustik model, yalnızca bir dil modeli kimliği (ve hiçbir akustik model kimliği) belirtirseniz, otomatik olarak seçilir. Eşleşen bir dil modeli, yalnızca bir akustik model kimliği belirtmezseniz, otomatik olarak seçilir.

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki örnekte bulabilirsiniz `samples/batch` dizininde [GitHub örnek deposundan](https://aka.ms/csspeech/samples).

> [!NOTE]
> Döküm toplu bir en iyi çaba ilkesine göre zamanlanır, hiçbir zaman tahmin için bir iş çalışır duruma ne zaman değişir. Bir kez çalışır durumda gerçek transkripsiyonu ses gerçek zamanlı daha hızlı işlenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
