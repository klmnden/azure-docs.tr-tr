---
title: Azure Batch tanıma API'si
description: Örnekler
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: c6912b45bc62ce9492e8e33bd1ffd8e7147b9d17
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427796"
---
# <a name="batch-transcription"></a>Toplu iş transkripsiyonu

Batch tanıma, ses büyük miktarda depolama alanında varsa idealdir. Rest API'mizi kullanarak, ses dosyaları SAS URI tarafından üzerine gelin ve döküm zaman uyumsuz olarak alır.

## <a name="batch-transcription-api"></a>Batch tanıma API'si

Batch tanıma API'si, metin tanıma, ek özellikleri ile birlikte zaman uyumsuz konuşma sunar. Bu, bir REST API için yöntemleri gösterme değil:

1. Toplu işlem isteği oluşturma

2. Sorgu Durumu 

3. Trnascriptions indiriliyor

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

Konuşma hizmeti tüm özellikleri ile bir abonelik anahtarı oluştururken [Azure portalında](https://portal.azure.com) aşağıdaki bizim [başlangıç kılavuzunu](get-started.md). Bizim temel modellerinden döküm alma planlıyorsanız, daha sonra tek yapmanız gereken budur. 

Özelleştirme ve özel bir model kullanarak planlıyorsanız bu subscritpion anahtar özel konuşma tanıma Portalı'na aşağıdaki gibi eklemeniz gerekir:

1. Oturum [özel konuşma](https://customspeech.ai).

2. **Abonelikler**'i seçin.

3. Seçin **mevcut aboneliğe bağlanma**.

4. Açılan view abonelik anahtarını ve bir diğer ad ekleyin

    ![Özel konuşma abonelikler ekran sayfası](media/stt/Subscriptions.jpg)

5. Kopyalayın ve bu anahtarın aşağıdaki örnekte istemci kodu yapıştırın.

> [!NOTE]
> Özel bir model kullanmayı planlıyorsanız, bu model Kimliğini çok gerekir. Bu uç noktası Ayrıntıları görünümünde bulma uç noktası kimliği olmadığını unutmayın. Bu modelin ayrıntılarını seçtiğinizde, alabileceğiniz model kimliği var.

## <a name="sample-code"></a>Örnek kod

Aşağıdaki örnek kod bir abonelik anahtarı ve bir API anahtarı ile özelleştirin. Bu, bir taşıyıcı belirteç edinme sağlar.

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

Belirteç edindikten sonra transkripsiyonu isteyen bir ses dosyasına işaret eden SAS URI'sini belirtmeniz gerekir. Kodun geri kalanını durumu yinelenir ve sonuçları görüntüler. Başlangıçta bir anahtar, bölge, kullanılacak modelleri ve SA ayarlarsınız. Aşağıdaki kod parçacığında gösterildiği gibi. Bu istemci ve POST isteğinin bir örneğinin tarafından izlenir. 

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;
    
            // SAS URI 
            private const string RecordingsBlobUri = "some SAS URI";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("some guid");
            private static Guid AdaptedLanguageId = new Guid("some guid");

            // Creating a Batch transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);
            
            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

İstek kullanıcı yapılan göre sorgulayabilir ve tanıma sonuçları kod parçacığı gösterildiği gibi indirin.

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
                            // not creted form here, continue
                            continue;
                        }
                            
                        completed++;
                            
                        // if the transcription was successfull, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succedded. Results: ");
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

Bizim [Swagger belgesinin](https://westus.cris.ai/swagger/ui/index) yukarıdaki çağrılarda tam ayrıntılar sağlar. Tam örnek burada gösterilen açıktır [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Önceki kodda, Azure portalında oluşturduğunuz konuşma kaynak abonelik anahtarını arasındadır. Özel konuşma hizmeti kaynaktan alınan tuşu çalışmıyor.

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
