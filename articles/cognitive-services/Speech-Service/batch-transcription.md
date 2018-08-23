---
title: Azure Batch tanıma API'si
description: Örnekler
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 5af829ca076b39758973c28a44d918b9ba5782b1
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42351259"
---
# <a name="batch-transcription"></a>Toplu iş transkripsiyonu

Batch tanıma, ses büyük miktarlarda varsa idealdir. Ses dosyalarının olduğu noktaya ve döküm zaman uyumsuz modda geri dönebilirsiniz.

## <a name="batch-transcription-api"></a>Batch tanıma API'si

Batch tanıma API'si, metin tanıma, ek özellikleri ile birlikte zaman uyumsuz konuşma sunar.

> [!NOTE]
> Batch tanıma API'si, genellikle saatlik ses binlerce accumulate çağrı merkezleri için idealdir. API, büyük hacimli ses kayıtlarını özelliği kolaylaştıran bir "Başlat ve unut" felsefemiz tarafından yönlendirilir.

### <a name="supported-formats"></a>Desteklenen biçimler

Batch tanıma API'si aşağıdaki biçimlerde destekler:

Ad| Kanal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
WAV |   Mono   |
WAV |  Stereo  |

Stereo ses akışları için Batch döküm sırasında transkripsiyonu sol ve sağ kanal böler. Sonuç ile iki JSON dosyaları her tek bir kanaldan oluşturulur. Zaman damgaları utterance başına bir sıralı son döküm oluşturmak Geliştirici etkinleştirin. Aşağıdaki JSON örneği, bir kanal çıktısını gösterir.

```json
       {
        "recordingsUrl": "https://mystorage.blob.core.windows.net/cris-e2e-datasets/TranscriptionsDataset/small_sentence.wav?st=2018-04-19T15:56:00Z&se=2040-04-21T15:56:00Z&sp=rl&sv=2017-04-17&sr=b&sig=DtvXbMYquDWQ2OkhAenGuyZI%2BYgaa3cyvdQoHKIBGdQ%3D",
        "resultsUrls": {
            "channel_0": "https://mystorage.blob.core.windows.net/bestor-87a0286f-304c-4636-b6bd-b3a96166df28/TranscriptionData/24265e4c-e459-4384-b572-5e3e7795221f?sv=2017-04-17&sr=b&sig=IY2qd%2Fkgtz2PwRe2C88BphH4Hv%2F1VCb1UVJ33xsw%2BEY%3D&se=2018-04-23T14:48:24Z&sp=r"
        },
        "statusMessage": "None.",
        "id": "0bb95356-ff06-469d-acc7-81f9144a269a",
        "createdDateTime": "2018-04-20T14:11:57.167",
        "lastActionDateTime": "2018-04-20T14:12:54.643",
        "status": "Succeeded",
        "locale": "en-US"
    },
```

> [!NOTE]
> Batch tanıma API'si, döküm, durum ve ilişkili sonuçları istenirken bir REST hizmeti kullanıyor. Herhangi bir dilde API'den kullanabilirsiniz. Sonraki bölümde, nasıl kullanıldığı açıklanır.

## <a name="authorization-token"></a>Yetkilendirme belirteci

Birleşik konuşma hizmeti tüm özellikleri ile bir abonelik anahtarı oluştururken [Azure portalında](https://portal.azure.com). Ayrıca, bir API anahtarı konuşma portaldan edindiğiniz: 

1. Oturum [özel konuşma](https://customspeech.ai).

2. **Abonelikler**'i seçin.

3. Seçin **API anahtarı oluşturma**.

    ![Özel konuşma abonelikler ekran sayfası](media/stt/Subscriptions.jpg)

4. Kopyalayın ve bu anahtarın aşağıdaki örnekte istemci kodu yapıştırın.

> [!NOTE]
> Özel bir model kullanmayı planlıyorsanız, bu model Kimliğini çok gerekir. Bu uç noktası Ayrıntıları görünümünde bulduğunuz dağıtım veya uç noktası kimliği olmadığını unutmayın. Bu modelin ayrıntılarını seçtiğinizde, alabileceğiniz model kimliği var.

## <a name="sample-code"></a>Örnek kod

Aşağıdaki örnek kod bir abonelik anahtarı ve bir API anahtarı ile özelleştirin. Bu, bir taşıyıcı belirteç edinme sağlar.

```cs
    public static async Task<CrisClient> CreateApiV1ClientAsync(string username, string key, string hostName, int port)
        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;

            var tokenProviderPath = "/oauth/ctoken";
            var clientToken = await CreateClientTokenAsync(client, hostName, port, tokenProviderPath, username, key).ConfigureAwait(false);
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", clientToken.AccessToken);

            return new CrisClient(client);
        }
```

Belirteç edindikten sonra transkripsiyonu isteyen bir ses dosyasına işaret eden SAS URI'sini belirtmeniz gerekir. Kodun geri kalanını durumu yinelenir ve sonuçları görüntüler.

```cs
   static async Task TranscribeAsync()
        { 
            // Creating a Batch transcription API Client
            var client = 
                await CrisClient.CreateApiV1ClientAsync(
                    "<your msa>", // MSA email
                    "<your api key>", // API key
                    "stt.speech.microsoft.com",
                    443).ConfigureAwait(false);
            
            var newLocation = 
                await client.PostTranscriptionAsync(
                    "<selected locale i.e. en-us>", // Locale 
                    "<your subscription key>", // Subscription Key
                    new Uri("<SAS URI to your file>")).ConfigureAwait(false);

            var transcription = await client.GetTranscriptionAsync(newLocation).ConfigureAwait(false);

            while (true)
            {
                transcription = await client.GetTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                if (transcription.Status == "Failed" || transcription.Status == "Succeeded")
                {
                    Console.WriteLine("Transcription complete!");

                    if (transcription.Status == "Succeeded")
                    {
                        var resultsUri = transcription.ResultsUrls["channel_0"];

                        WebClient webClient = new WebClient();

                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(resultsUri, filename);

                        var results = File.ReadAllText(filename);
                        Console.WriteLine(results);
                    }

                    await client.DeleteTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                    break;
                }
                else
                {
                    Console.WriteLine("Transcription status: " + transcription.Status);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.ReadLine();
        }
```

> [!NOTE]
> Önceki kodda, Azure portalında oluşturduğunuz Speech(Preview) kaynak abonelik anahtarını arasındadır. Özel konuşma hizmeti kaynaktan alınan tuşu çalışmıyor.

Ses gönderme ve döküm durumu almak için zaman uyumsuz Kurulum dikkat edin. Oluşturulan istemci, bir .NET Http istemcidir. Var. bir `PostTranscriptions` ses dosyası ayrıntıları, gönderme yöntemi ve bir `GetTranscriptions` sonuçlarını almak için yöntemi. `PostTranscriptions` bir tanıtıcı döndürür ve `GetTranscriptions` transkripsiyonu durumunu almak için bir tanıtıcı oluşturmak için bu tutamacı kullanır.

Geçerli örnek kod, herhangi bir özel modelleri belirtmiyor. Hizmet, dosya veya dosyalar fotoğrafını için temel modelleri kullanır. Modelleri belirtmek için model kimliklerini akustik ve dil modeli için aynı yönteme geçirebilirsiniz. 

Taban çizgisi kullanmak istemiyorsanız, hem akustik ve dil modelleri için model kimliklerini geçmesi gerekir.

> [!NOTE]
> Taban çizgisi döküm için temel modelleri uç noktalarına bildirmeniz gerekmez. Özel modelleri kullanmak istiyorsanız, uç noktaları kimlikleri olarak sağladığınız [örnek](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Temel dil modeli ile bir akustik temel kullanmak istiyorsanız, yalnızca özel modelin uç noktası kimliği bildirmeniz gerekir Microsoft (Akustik veya dil olması) iş ortağı temel model algılar ve, döküm isteği yerine getirmek için kullanır.

### <a name="supported-storage"></a>Desteklenen depolama

Şu anda desteklenen tek depolama, Azure Blob depolama alanıdır.

## <a name="downloading-the-sample"></a>Örneği indirme

Burada gösterilen örnek açıktır [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Genellikle, bir ses tanıma, ses dosyası yanı sıra, 2-3 dakika yükü süresi için eşit bir zaman aralığı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
