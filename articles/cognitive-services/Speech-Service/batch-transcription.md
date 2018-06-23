---
title: Azure Batch Transcription API | Azure Microsoft Docs
description: Örnekler
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 0e30b26806121fa9b10118d79dd0287745d176f8
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322033"
---
# <a name="batch-transcription"></a>Toplu transcription

Toplu transcription ses büyük miktarda kullanım örnekleri için idealdir. Ses dosyaları gösterin ve zaman uyumsuz modda transcriptions geri almak Geliştirici sağlar.

## <a name="batch-transcription-api"></a>Toplu transcription API

Toplu transcription API yukarıdaki senaryoyu mümkün kılar. Zaman uyumsuz konuşma metin transcription yanı sıra ek özellikler sunar.

> [!NOTE]
> Toplu transcription API genellikle ses saatlik binlerce birikmesini çağrı merkezleri için idealdir. API yangın & unut felsefesi ses kayıtlarını büyük miktarda transcribe kolay hale getirir.

### <a name="supported-formats"></a>Desteklenen biçimler

Toplu transcription API Merkezi ile ilgili tüm çevrimdışı çağrısı senaryoları için de facto olur ve tüm ilişkili biçimleri için destek sağlayan amaçlar. Şu anda desteklenen biçimler:

Ad| Kanal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
WAV |   Mono   |
WAV |  Stereo  |

Stereo ses akışları için sırasında transcription sol ve sağ kanal toplu transcription bölün. Sonuç ile iki JSON dosyaları her tek bir kanaldan oluşturulur. Zaman damgaları utterance başına sıralı son dökümü oluşturmak geliştiricinin etkinleştirin. Aşağıdaki JSON örnek bir kanal çıktısını gösterir.

    ```
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
> Toplu transcription API transcriptions, durum ve ilişkili sonuçları isteyen bir REST hizmeti kullanıyor. .NET temel alır ve dış bağımlılıkları yok. Sonraki bölümde, nasıl kullanıldığı açıklanmaktadır.

## <a name="authorization-token"></a>Yetkilendirme belirteci

Birleşik konuşma hizmetinin tüm özellikleri ile bir abonelik anahtarı oluşturmak kullanıcı gerektiği [Azure portal](https://portal.azure.com). Ayrıca, bir API anahtarı konuşma Portalı'ndan elde edilmesi gerekir. Bir API anahtarı oluşturmak için adımlar:

1. Oturum https://customspeech.ai.

2. Tıklayın.

3. Seçeneğini tıklatın `Generate API Key`.

    ![Karşıya yükleme görünümü](media/stt/Subscriptions.jpg)

4. Kopyalayın ve bu anahtar örnek istemci kodu yapıştırın.

> [!NOTE]
> Ardından özel bir model kullanmayı planlıyorsanız, bu model Kimliğini çok gerekir. Bu uç noktası Ayrıntılar görünümü ancak bu modelin ayrıntılarını tıklattığınızda, alabilirsiniz model kimliği bulma uç noktası kimliği ya da dağıtım olmadığını unutmayın

## <a name="sample-code"></a>Örnek kod

API'si kullanan oldukça kolaydır. Aşağıdaki örnek kod bir abonelik anahtarı ve bir API anahtarı ile özelleştirilmiş olması gerekir.

```cs
   static async Task TranscribeAsync()
        { 
            // Creating an Batch transcription API Client
            var client = 
                await CrisClient.CreateApiV1ClientAsync(
                    "<your msa>", // MSA email
                    "<your api key>", // API key
                    "stt.speech.microsoft.com",
                    443).ConfigureAwait(false);
            
            var newLocation = 
                await client.PostTranscriptionAsync(
                    "<selected locale i.e. en-us>", // Locale 
                    "<your subscripition key>", // Subscription Key
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
> Yukarıdaki kod parçacığında belirtilen abonelik Azure portalında oluşturduğunuz Speech(Preview) kaynak anahtarından anahtardır. Özel konuşma hizmet kaynaktan alınan anahtarlar çalışmaz.


Ses nakil ve transcription durumunu almak için zaman uyumsuz Kurulum dikkat edin. Oluşturulan istemci NET Http istemci olur. Var. bir `PostTranscriptions` ses dosyası ayrıntılarını göndermek için yöntem ve bir `GetTranscriptions` sonuçları almak için yöntem. `PostTranscriptions` bir işleyici döner ve `GetTranscriptions` yöntemi transcription durumu elde etmek için bir tanıtıcı oluşturmak için bu tanıtıcısı kullanıyor.

Geçerli örnek kodu, hiçbir özel modelleri belirtmiyor. Hizmet, dosyaları çoğaltmaya için temel modelleri kullanır. Kullanıcı modelleri belirtin isterse bir modelIDs kullanım ve dil modeli için aynı yöntemine geçirebilirsiniz. 

Bir taban çizgisi kullanmak istediğiniz değil, bir model kimliği için kullanım ve dil modelleri geçmesi gerekir.

> [!NOTE]
> Taban çizgisi için temel modelleri uç noktalarına bildirmek transcription kullanıcı yok. Kullanıcının özel modelleri kullanmak isterse kendisinin uç noktaları kimlikleri olarak sağlamak üzere olurdu [örnek](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Kullanıcı bir temel dil modeliyle akustik temel kullanmak isterse sonra kendisinin yalnızca özel modelinin uç noktası kimliği bildirmeniz gerekir Dahili sistemimizde (Akustik veya dil olması) iş ortağı temel model şekil ve fullfill transcription isteği kullanın.

### <a name="supported-storage"></a>Desteklenen depolama

Şu anda desteklenen tek depolama Azure blob ' dir.

## <a name="downloading-the-sample"></a>Örnek indirme

Burada gösterilen örnek açıktır [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Ses dosyası artı 2-3 dakika yükünü süresi için eşit bir zaman aralığı, genellikle bir ses transcription gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
