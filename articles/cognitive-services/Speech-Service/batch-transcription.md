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
ms.openlocfilehash: f21973855ceb3a257627c147490ac50465c54020
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281948"
---
# <a name="batch-transcription"></a>Toplu iş transkripsiyonu

Batch tanıma, ses büyük miktarda kullanım örnekleri için idealdir. Bu ses dosyalarının olduğu noktaya ve döküm zaman uyumsuz modda dönmek Geliştirici sağlar.

## <a name="batch-transcription-api"></a>Batch tanıma API'si

Batch tanıma API'si, yukarıdaki senaryoyu mümkün kılar. Zaman uyumsuz konuşma metin tanıma birlikte ek özellikler sunar.

> [!NOTE]
> Batch tanıma API'si, genellikle saatlik ses binlerce accumulate çağrı merkezleri için idealdir. API'nin yangın & unut felsefesi ses kayıtlarını büyük hacimli konuşmaların daha kolay hale getirir.

### <a name="supported-formats"></a>Desteklenen biçimler

Çevrimdışı çağrı senaryoları ile ilgili center de kaybolmayacaktır olur ve tüm ilgili biçimleri için destek sunmak için Batch transkripsiyonu API amaçlar. Şu anda desteklenen biçimler:

Ad| Kanal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
WAV |   Mono   |
WAV |  Stereo  |

Stereo ses akışları için Batch döküm sırasında transkripsiyonu sol ve sağ kanal bölecektir. Sonuç ile iki JSON dosyaları her tek bir kanaldan oluşturulur. Zaman damgaları utterance başına bir sıralı son döküm oluşturmak Geliştirici etkinleştirin. Aşağıdaki JSON örneği, bir kanal çıktısını gösterir.

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
> Batch tanıma API'si, döküm, durum ve ilişkili sonuçları istenirken bir REST hizmeti kullanıyor. API, herhangi bir dilde kullanılabilir. Sonraki bölümde, nasıl kullanıldığı açıklanır.

## <a name="authorization-token"></a>Yetkilendirme belirteci

Birleşik konuşma hizmeti tüm özellikleri ile bir abonelik anahtarı oluşturmak kullanıcı gereksinimleriniz değiştikçe [Azure portalında](https://portal.azure.com). Ayrıca, bir API anahtarı konuşma tanıma Portalı'ndan elde edilmesi gerekir. Bir API anahtarı oluşturmak için bu adımları:

1. Oturum https://customspeech.ai.

2. Abonelikler'e tıklayın.

3. Seçeneğe tıklayın `Generate API Key`.

    ![Karşıya yükleme görüntüle](media/stt/Subscriptions.jpg)

4. Kopyalayın ve bu anahtarın aşağıdaki örnekte istemci kodu yapıştırın.

> [!NOTE]
> Ardından, özel bir model kullanmayı planlıyorsanız, bu modeli kimliği çok gerekir. Bu uç noktası ayrıntıları görüntüle ancak bu modelin ayrıntıları tıkladığınızda almak model Kimliğini bulma uç noktası kimliği ya da dağıtım olmadığına dikkat edin

## <a name="sample-code"></a>Örnek kod

API'si kullanan oldukça oldukça kolaydır. Bir abonelik anahtarı ve buna karşılık olarak aşağıdaki kod parçacığı bir taşıyıcı belirteç almak Geliştirici sağlayan bir API anahtarı ile özelleştirilmesi gereken aşağıdaki örnek kodları gösterir:

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

Belirteç alındıktan sonra Geliştirici transkripsiyonu isteyen bir ses dosyasına işaret eden SAS URI'sini belirtin gerekir. Kodun geri kalanını yalnızca durum yinelenir ve sonuçları görüntüler.

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
> Yukarıdaki kod parçacığında geçen abonelik anahtarı, Azure portalında oluşturduğunuz Speech(Preview) kaynaktan anahtardır. Özel konuşma hizmeti kaynaktan alınan anahtarlar çalışmaz.


Ses gönderme ve döküm durumu almak için zaman uyumsuz Kurulum dikkat edin. Oluşturulan istemci, bir .NET Http istemcidir. Var. bir `PostTranscriptions` ses dosyası ayrıntıları, gönderme yöntemi ve bir `GetTranscriptions` sonuçlarını almak için yöntemi. `PostTranscriptions` bir tanıtıcı döndürür ve `GetTranscriptions` yöntemi transkripsiyonu durumunu almak için bir tanıtıcı oluşturmak için bu tutamacı kullanıyor.

Geçerli örnek kod, herhangi bir özel modelleri belirtmiyor. Hizmet, dosyaları fotoğrafını için temel modelleri kullanır. Kullanıcı modelleri belirtin isterse bir modelIDs akustik ve dil modeli için aynı yönteme geçirebilirsiniz. 

Bir taban çizgisi değil kullanıyorsanız, bir model kimliği için hem akustik ve dil modellerini geçmesi gerekir.

> [!NOTE]
> Şunun için taban çizgisi temel modelleri uç noktalarına bildirmek döküm kullanıcı yok. Kullanıcı özel modelleri kullanmak istiyorsa yaptığı uç noktaları kimlikleri sağlamanız gerekir [örnek](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Kullanıcı bir akustik temeli ile temel bir dil modeli kullanmak istiyorsa ardından kendisi yalnızca özel modelin uç noktası kimliği bildirmeniz gerekir Dahili olarak sistemimiz (Akustik veya dil olması) iş ortağı temel modelinin ölçeğini şekil ve, döküm isteği yerine getirmek için kullanın.

### <a name="supported-storage"></a>Desteklenen depolama

Şu anda desteklenen tek Azure blob depolamadır.

## <a name="downloading-the-sample"></a>Örneği indirme

Burada görüntülenen örnek açıktır [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Genellikle bir ses tanıma, ses dosyası artı 2-3 dakika yükü süre eşit bir zaman aralığı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
