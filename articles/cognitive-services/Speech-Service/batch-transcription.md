---
title: Batch Transkripsiyonu - konuşma hizmetlerini kullanma
titlesuffix: Azure Cognitive Services
description: Batch transkripsiyonu, depolama, Azure BLOB'ları gibi ses büyük bir miktarını konuşmaların istiyorsanız idealdir. Adanmış REST API'sini kullanarak bir paylaşılan erişim imzası (SAS) URI ses dosyalarının üzerine gelin ve döküm zaman uyumsuz olarak alır.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: bf89180ea98473d2da3495286396a12c6f25288f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228670"
---
# <a name="why-use-batch-transcription"></a>Batch transkripsiyonu neden kullanmalısınız?

Batch transkripsiyonu, depolama, Azure BLOB'ları gibi ses büyük bir miktarını konuşmaların istiyorsanız idealdir. Adanmış REST API'sini kullanarak bir paylaşılan erişim imzası (SAS) URI ses dosyalarının üzerine gelin ve döküm zaman uyumsuz olarak alır.

>[!NOTE]
> Konuşma Hizmetleri standart aboneliği (S0), batch transkripsiyonu kullanmak için gereklidir. Ücretsiz Abonelik anahtarları (F0) işe yaramaz. Ek bilgi için bkz: [fiyatlandırma ve limitler](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

## <a name="the-batch-transcription-api"></a>Batch tanıma API'si

Batch tanıma API'si, ek özellikleri ile birlikte zaman uyumsuz konuşma metin tanıma sunar. Bu yöntemleri gösteren bir REST API'si değil:

1. Toplu işlem isteği oluşturma
1. Sorgu durumu
1. Döküm indiriliyor

> [!NOTE]
> Batch tanıma API'si, genellikle saatlik ses binlerce accumulate çağrı merkezleri için idealdir. API, büyük hacimli ses kayıtlarını özelliği kolaylaştıran bir "Başlat ve unut" felsefemiz tarafından yönlendirilir.

### <a name="supported-formats"></a>Desteklenen biçimler

Batch tanıma API'si, aşağıdaki biçimlerde destekler:

| Biçimlendir | Codec bileşeni | Bit hızı | Örnek hızı |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bit | 8 veya 16 kHz, mono, stereo |
| MP3 | PCM | 16-bit | 8 veya 16 kHz, mono, stereo |
| OGG | GEÇERLİ | 16-bit | 8 veya 16 kHz, mono, stereo |

> [!NOTE]
> Batch tanıma API'si (katman ödeme) bir S0 anahtarı gerektirir. Ücretsiz (f0) anahtar ile çalışmaz.

Stereo ses akışları için Batch transkripsiyonu API sol ve sağ kanal döküm sırasında böler. Sonuç ile iki JSON dosyaları her tek bir kanaldan oluşturulur. Zaman damgaları utterance başına bir sıralı son döküm oluşturmak Geliştirici etkinleştirin. Aşağıdaki JSON örneği kanal çıkış, includuing küfür filtresini ve noktalama işaretleri modeli ayarlama özellikleri gösterir.

```json
{
  "recordingsUrl": "https://contoso.com/mystoragelocation",
  "models": [],
  "locale": "en-US",
  "name": "Transcription using locale en-US",
  "description": "An optional description of the transcription.",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic"
  },
```

> [!NOTE]
> Döküm, durum ve ilişkili sonuçları istenirken bir REST hizmeti ve Batch tanıma API'sini kullanır. Herhangi bir dilde API'den kullanabilirsiniz. Sonraki bölümde, API'yi nasıl kullanıldığını açıklar.

### <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreleri REST isteğinin sorgu dizesinde eklenebilir.

| Parametre | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Tanıma sonuçları küfür nasıl ele alınacağını belirtir. Kabul edilen değerler `none` , devre dışı bırakır küfür filtresi `masked` yıldız işareti ile küfür değiştirir `removed` sonuç, tüm küfür kaldırır veya `tags` "küfür" etiketleri ekler. Varsayılan ayar `masked`. | İsteğe bağlı |
| `PunctuationMode` | Noktalama işaretleri tanıma sonuçları nasıl ele alınacağını belirtir. Değerler kabul `none` , devre dışı bırakır, noktalama `dictated` açık noktalama gelir `automatic` noktalama işaretleri ile uğraşmak kod çözücü olanak tanıyan veya `dictatedandautomatic` dikte noktalama işaretleri veya otomatik olduğu anlamına gelir. | İsteğe bağlı |


## <a name="authorization-token"></a>Yetkilendirme belirteci

Konuşma hizmeti tüm özellikleri ile bir abonelik anahtarı oluştururken [Azure portalında](https://portal.azure.com) izleyerek bizim [Başlarken Kılavuzu](get-started.md). Bizim temel modellerinden döküm almak planlıyorsanız, bir anahtar oluşturmak tek yapmanız gereken bir işlemdir.

Abonelik anahtarı özelleştirme ve özel bir model kullanmak planlıyorsanız, aşağıdakileri yaparak özel konuşma tanıma Portalı'na ekleyin:

1. Oturum [özel konuşma](https://customspeech.ai).

2. Sağ üst kısımdaki seçin **abonelikleri**.

3. Seçin **mevcut aboneliğe bağlanma**.

4. Açılır pencerede abonelik anahtarını ve bir diğer ad ekleyin.

    ![Abonelik Ekle penceresi](media/stt/Subscriptions.jpg)

5. Kopyalayın ve bu anahtarın aşağıdaki örnekte istemci kodu yapıştırın.

> [!NOTE]
> Özel bir model kullanmayı planlıyorsanız, bu model Kimliğini çok gerekir. Bu kimliği uç noktası Ayrıntıları görünümünde bulma uç noktası kimliği değil. Bu modelin ayrıntılarını seçtiğinizde, alabileceğiniz model kimliği var.

## <a name="sample-code"></a>Örnek kod

Aşağıdaki örnek kod bir abonelik anahtarı ve bir API anahtarı ile özelleştirin. Bu eylem, bir taşıyıcı belirteç almak üzere sağlar.

```cs
     public static CrisClient CreateApiV2Client(string key, string hostName, int port)

        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            return new CrisClient(client);
        }
```

Belirteci aldıktan sonra transkripsiyonu gerektiren ses dosyasına işaret eden SAS URI'sini belirtin. Kodun geri kalanını durumu yinelenir ve sonuçları görüntüler. İlk başta, anahtar, bölge, model ve SA aşağıdaki kod parçacığında gösterildiği gibi ayarlayabilirsiniz. Ardından, istemci ve POST isteğinin örneği.

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;

            // SAS URI
            private const string RecordingsBlobUri = "SAS URI pointing to the file in Azure Blob Storage";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("guid of the acoustic adaptation model");
            private static Guid AdaptedLanguageId = new Guid("guid of the language model");

            // Creating a Batch Transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);

            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

İstek yaptığınız, sorgu ve tanıma sonuçları, aşağıdaki kod parçacığında gösterildiği gibi indirin:

```cs

            // get all transcriptions for the user
            transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            // for each transcription in the list we check the status
            foreach (var transcription in transcriptions)
            {
                switch(transcription.Status)
                {
                    case "Failed":
                    case "Succeeded":

                            // we check to see if it was one of the transcriptions we created from this client.
                        if (!createdTranscriptions.Contains(transcription.Id))
                        {
                            // not created from here, continue
                            continue;
                        }

                        completed++;

                        // if the transcription was successful, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succeeded. Results: ");
                            Console.WriteLine(results);
                        }

                    break;
                    case "Running":
                    running++;
                     break;
                    case "NotStarted":
                    notStarted++;
                    break;

                    }
                }
            }
        }
```

Önceki çağrıları ile ilgili tüm ayrıntılar için bkz. bizim [swagger belgesinin](https://westus.cris.ai/swagger/ui/index). Burada gösterilen tam örnek için Git [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Önceki kodda, Azure portalında oluşturduğunuz konuşma kaynak abonelik anahtarını arasındadır. Özel konuşma hizmeti kaynak Al tuşu çalışmıyor.

Ses gönderme ve döküm durumu almak için zaman uyumsuz Kurulum not alın. Oluşturduğunuz .NET HTTP istemci istemcisidir. Var. bir `PostTranscriptions` ses dosyası ayrıntılarını göndermek için yöntem ve bir `GetTranscriptions` sonuçları almak için yöntemi. `PostTranscriptions` bir tanıtıcı döndürür ve `GetTranscriptions` transkripsiyonu durumu almak için bir tanıtıcı oluşturmak için kullanır.

Geçerli örnek kod, özel bir model belirtmez. Hizmet, dosya veya dosyalar fotoğrafını için temel modelleri kullanır. Modelleri belirtmek için model kimliklerini akustik ve dil modeli için aynı yönteme geçirebilirsiniz.

Taban çizgisi kullanmak istemiyorsanız, hem akustik ve dil modelleri için model kimliklerini geçirin.

> [!NOTE]
> Taban çizgisi döküm için temel modelleri uç noktalarına bildirmeniz gerekmez. Özel modelleri kullanmak istiyorsanız, uç noktaları kimlikleri olarak sağladığınız [örnek](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Temel dil modeli ile bir akustik temel kullanmak istiyorsanız, yalnızca özel modelin uç noktası kimliği bildirmenize gerek Microsoft iş ortağı temel model algılar&mdash;olmadığını akustik veya dil&mdash;ve döküm isteği yerine getirmek için kullanır.

### <a name="supported-storage"></a>Desteklenen depolama

Şu anda desteklenen tek depolama, Azure Blob depolama alanıdır.

## <a name="download-the-sample"></a>Örneği indirme

Örnek bu makalede bulabileceğiniz [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Bir ses tanıma, ses dosyası yanı sıra, iki için üç dakikalık yükü süresi için eşit bir zaman aralığı normalde gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
