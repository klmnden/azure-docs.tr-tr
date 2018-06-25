---
title: Azure içerik aracı - .NET kullanarak video dökümü incelemeler oluşturma | Microsoft Docs
description: .NET için Azure içerik denetleyici SDK'sını kullanarak video dökümü oluşturma gözden geçirme
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/19/2018
ms.author: sajagtap
ms.openlocfilehash: 3286da6e38f0fba02386d877a835fb694ed0fdec
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "35352450"
---
# <a name="create-video-transcript-reviews-using-net"></a>.NET kullanarak video dökümü incelemeler oluşturma

Bu makalede bilgiler sağlanmaktadır ve hızlı bir şekilde yardımcı olmak için kod örnekleri içerik denetleyici SDK'sı ile C# kullanmaya başlama:

- Video İnceleme İnsan denetleyiciler için oluşturma
- Aracılı dökümü incelemeye ekleme
- Gözden geçirme yayımlama

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sahibi olduğunuzu varsayar [video aracılı](video-moderation-api.md) ve [video gözden geçirme oluşturulan](video-reviews-quickstart-dotnet.md) İnsan kararlar için İnceleme aracı içinde. Şimdi gözden geçirme aracında aracılı video dökümleri eklemek istediğiniz.

Bu makalede ayrıca Visual Studio ve C# ile bilginiz olduğunu varsayar.

### <a name="sign-up-for-content-moderator-services"></a>İçerik denetleyici Hizmetleri için kaydolun

REST API veya SDK üzerinden içerik denetleyici Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir.

İçerik denetleyici panosunda abonelik anahtarınızı bulmak **ayarları** > **kimlik bilgileri** > **API**  >   **Deneme Ocp-Apim-Subscription-Key**. Daha fazla bilgi için bkz: [genel bakış](overview.md).

### <a name="prepare-your-video-for-review"></a>Videonuzu gözden geçirme için hazırlama

Bir video incelemeye dökümü ekleyin. Video çevrimiçi yayımlanması gerekir. Akış uç gerekir. Akış uç çalmasına İnceleme aracı video oynatıcı sağlar.

![Video demo küçük resim](images/ams-video-demo-view.PNG)

- Kopya **URL** bu [Azure Media Services gösteri](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) bildirim URL'si için sayfa.

## <a name="create-your-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Yeni bir ekleme **konsol uygulaması (.NET Framework)** çözümünüzü projeye.

1. Proje adı **VideoTranscriptReviews**.

1. Bu proje çözüme yönelik tek başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

TermLists proje için aşağıdaki NuGet paketlerini yükleyin.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Güncelleştirme program using deyimleri

Değiştirme program deyimleri aşağıdaki gibi kullanarak kullanıcının.

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;
    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;


### <a name="add-private-properties"></a>Özel Özellikler ekleme

Aşağıdaki özel özellikleri ad alanına VideoTranscriptReviews, sınıf Program ekleyin.

Belirtilen yerlerde, bu özellikler için örnek değerleri değiştirin.


    namespace VideoReviews
    {
        class Program
        {
            // NOTE: Replace this example location with the location for your Content Moderator account.
            /// <summary>
            /// The region/location for your Content Moderator account, 
            /// for example, westus.
            /// </summary>
            private static readonly string AzureRegion = "YOUR CONTENT MODERATOR REGION";

            // NOTE: Replace this example key with a valid subscription key.
            /// <summary>
            /// Your Content Moderator subscription key.
            /// </summary>
            private static readonly string CMSubscriptionKey = "YOUR CONTENT MODERATOR KEY";

            // NOTE: Replace this example team name with your Content Moderator team name.
            /// <summary>
            /// The name of the team to assign the job to.
            /// </summary>
            /// <remarks>This must be the team name you used to create your 
            /// Content Moderator account. You can retrieve your team name from
            /// the Conent Moderator web site. Your team name is the Id associated 
            /// with your subscription.</remarks>
            public static readonly string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

            /// <summary>
            /// The base URL fragment for Content Moderator calls.
            /// </summary>
            private static readonly string AzureBaseURL =
                $"{AzureRegion}.api.cognitive.microsoft.com";

            /// <summary>
            /// The minimum amount of time, in milliseconds, to wait between calls
            /// to the Content Moderator APIs.
            /// </summary>
            private const int throttleRate = 2000;


### <a name="create-content-moderator-client-object"></a>İçerik denetleyici istemci nesnesi oluşturun

Aşağıdaki yöntem tanımını ad alanına VideoTranscriptReviews, sınıf Program ekleyin.

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        return new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey))
        {
            BaseUrl = AzureBaseURL
        };
    }

## <a name="create-a-video-review"></a>Video İnceleme oluşturma

Bir video gözden geçirmesine oluşturmak **ContentModeratorClient.Reviews.CreateVideoReviews**. Daha fazla bilgi için bkz: [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** aşağıdaki gerekli parametreleri:
1. "Application/json." olması gereken bir MIME türü içeren bir dize 
1. İçerik denetleyici takım adı.
1. Bir **IList<CreateVideoReviewsBodyItem>**  nesnesi. Her **CreateVideoReviewsBodyItem** nesnesi, bir video gözden geçirme temsil eder. Bu Hızlı Başlangıç, aynı anda bir gözden geçirme oluşturur.

**CreateVideoReviewsBodyItem** çeşitli özelliklere sahiptir. En azından aşağıdaki özellikleri ayarlayın:
- **İçerik**. Gözden geçirilecek video URL'si.
- **ContentID**. Video incelemeye atamak için bir kimliği.
- **Durum**. "Yayımlanmamış." değerine ayarlayın Bunu ayarlamazsanız "Bekliyor", video gözden geçirme yayımlanan anlamına gelir ve İnsan gözden geçirme bekleyen varsayar. Video İnceleme yayımlandığında, artık video çerçeveleri, bir dökümü veya dökümü yönetimini sonuç kendisine ekleyebilirsiniz.

> [!NOTE]
> **CreateVideoReviews** IList döndürür<string>. Bu dizelerin her biri bir video gözden geçirme için bir kimlik içeriyor. Bu kimlikleri guıd'lerdir ve değeri aynı olmayan **ContentID** özelliği. 

Aşağıdaki yöntem tanımını ad alanına VideoReviews, sınıf Program ekleyin.

    /// <summary>
    /// Create a video review. For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="id">The ID to assign to the video review.</param>
    /// <param name="content">The URL of the video to review.</param>
    /// <returns>The ID of the video review.</returns>
    private static string CreateReview(ContentModeratorClient client, string id, string content)
    {
        Console.WriteLine("Creating a video review.");

        List<CreateVideoReviewsBodyItem> body = new List<CreateVideoReviewsBodyItem>() {
            new CreateVideoReviewsBodyItem
            {
                Content = content,
                ContentId = id,
                /* Note: to create a published review, set the Status to "Pending".
                However, you cannot add video frames or a transcript to a published review. */
                Status = "Unpublished",
            }
        };

        var result = client.Reviews.CreateVideoReviews("application/json", TeamName, body);

        Thread.Sleep(throttleRate);

        // We created only one review.
        return result[0];
    }

> [!NOTE]
> İçerik denetleyici hizmeti anahtarınızı ikinci (RPS) hız sınırı başına bir istek var. Sınırı aşarsanız, SDK 429 hata kodunu içeren bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS hızı sınırı vardır.

## <a name="add-transcript-to-video-review"></a>Video incelemeye dökümü Ekle

Bir video gözden geçirme ile bir dökümü eklemek **ContentModeratorClient.Reviews.AddVideoTranscript**. **AddVideoTranscript** aşağıdaki gerekli parametreleri:
1. İçerik denetleyici ekibinizin kimliği
1. Video İnceleme Kimliği tarafından döndürülen **CreateVideoReviews**.
1. A **akış** dökümü içeren nesne.

Dökümü WebVTT biçiminde olması gerekir. Daha fazla bilgi için bkz: [WebVTT: Web Video metin parçaları biçimi](https://www.w3.org/TR/webvtt1/).

> [!NOTE]
> Program bir örnek dökümü VTT biçimde kullanır. Azure Media Indexer hizmete kullandığınız gerçek çözümde [bir dökümü oluşturmak](https://docs.microsoft.com/azure/media-services/media-services-index-content) bir video.

Aşağıdaki yöntem tanımını ad alanına VideotranscriptReviews, sınıf Program ekleyin.

    /// <summary>
    /// Add a transcript to the indicated video review.
    /// The transcript must be in the WebVTT format.
    /// For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b8b2e7151f0b10d451fe
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="transcript">The video transcript.</param>
    static void AddTranscript(ContentModeratorClient client, string review_id, string transcript)
    {
        Console.WriteLine("Adding a transcript to the review with ID {0}.", review_id);
        client.Reviews.AddVideoTranscript(TeamName, review_id, new MemoryStream(System.Text.Encoding.UTF8.GetBytes(transcript)));
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Video incelemeye dökümü yönetimini sonuç Ekle

Bir video gözden geçirme için bir dökümü eklemeden yanı sıra bu dökümü yönetme sonucunu de ekleyin. Bunu ile **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**. Daha fazla bilgi için bkz: [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranscriptModerationResult** aşağıdaki gerekli parametreleri:
1. "Application/json." olması gereken bir MIME türü içeren bir dize 
1. İçerik denetleyici takım adı.
1. Video İnceleme Kimliği tarafından döndürülen **CreateVideoReviews**.
1. IList<TranscriptModerationBodyItem>. A **TranscriptModerationBodyItem** aşağıdaki özelliklere sahiptir:
- **Koşulları**. IList<TranscriptModerationBodyItemTermsItem>. A **TranscriptModerationBodyItemTermsItem** aşağıdaki özelliklere sahiptir:
- **Dizin**. Terim sıfır tabanlı dizini.
- **Terim**. Terim içeren bir dize.
- **Zaman damgası**. Saniye cinsinden süre içinde bulunan ve koşulları dökümü içeren bir dize.

Dökümü WebVTT biçiminde olması gerekir. Daha fazla bilgi için bkz: [WebVTT: Web Video metin parçaları biçimi](https://www.w3.org/TR/webvtt1/).

Aşağıdaki yöntem tanımını ad alanına VideoTranscriptReviews, sınıf Program ekleyin. Bu yöntem bir dökümü gönderen **ContentModeratorClient.TextModeration.ScreenText** yöntemi. Ayrıca bir IList sonucu çevirir<TranscriptModerationBodyItem>ve gönderildiği **AddVideoTranscriptModerationResult**.

    /// <summary>
    /// Add the results of moderating a video transcript to the indicated video review.
    /// For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="transcript">The video transcript.</param>
    static void AddTranscriptModerationResult(ContentModeratorClient client, string review_id, string transcript)
    {
        Console.WriteLine("Adding a transcript moderation result to the review with ID {0}.", review_id);

        // Screen the transcript using the Text Moderation API. For more information, see:
        // https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f
        Screen screen = client.TextModeration.ScreenText("eng", "text/plain", transcript);

        // Map the term list returned by ScreenText into a term list we can pass to AddVideoTranscriptModerationResult.
        List<TranscriptModerationBodyItemTermsItem> terms = new List<TranscriptModerationBodyItemTermsItem>();
        if (null != screen.Terms)
        {
            foreach (var term in screen.Terms)
            {
                if (term.Index.HasValue)
                {
                    terms.Add(new TranscriptModerationBodyItemTermsItem(term.Index.Value, term.Term));
                }
            }
        }

        List<TranscriptModerationBodyItem> body = new List<TranscriptModerationBodyItem>()
        {
            new TranscriptModerationBodyItem()
            {
                Timestamp = "0",
                Terms = terms
            }
        };

        client.Reviews.AddVideoTranscriptModerationResult("application/json", TeamName, review_id, body);

        Thread.Sleep(throttleRate);
    }


## <a name="publish-video-review"></a>Video gözden geçirme yayımlama

Bir video gözden geçirme ile yayımlama **ContentModeratorClient.Reviews.PublishVideoReview**. **PublishVideoReview** aşağıdaki gerekli parametreleri:
1. İçerik denetleyici takım adı.
1. Video İnceleme Kimliği tarafından döndürülen **CreateVideoReviews**.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, sınıf Program ekleyin.

    /// <summary>
    /// Publish the indicated video review. For more information, see the reference API:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7bb29e7151f0b10d45201
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    private static void PublishReview(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Publishing the review with ID {0}.", review_id);
        client.Reviews.PublishVideoReview(TeamName, review_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Tüm bir araya getirme

Ekleme **ana** yöntem tanımını ad alanına VideoTranscriptReviews, Program sınıfı. Son olarak, Program sınıfı ve VideoTranscriptReviews ad kapatın.

> [!NOTE]
> Program bir örnek dökümü VTT biçimde kullanır. Azure Media Indexer hizmete kullandığınız gerçek çözümde [bir dökümü oluşturmak](https://docs.microsoft.com/azure/media-services/media-services-index-content) bir video. 

    static void Main(string[] args)
    {
        using (ContentModeratorClient client = NewClient())
        {
            // Create a review with the content pointing to a streaming endpoint (manifest)
            var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
            string review_id = CreateReview(client, "review1", streamingcontent);

            var transcript = @"WEBVTT

            01:01.000 --> 02:02.000
            First line with a crap word in a transcript.

            02:03.000 --> 02:25.000
            This is another line in the transcript.
            ";

            AddTranscript(client, review_id, transcript);

            AddTranscriptModerationResult(client, review_id, transcript);

            // Publish the review
            PublishReview(client, review_id);

            Console.WriteLine("Open your Content Moderator Dashboard and select Review > Video to see the review.");
            Console.WriteLine("Press any key to close the application.");
            Console.Read();
        }
    }

## <a name="run-the-program-and-review-the-output"></a>Programını çalıştırın ve çıktıyı gözden geçirin

Uygulamayı çalıştırdığınızda, aşağıdaki satırları bir çıktı görmeniz gerekir:

    Creating a video review.
    Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Open your Content Moderator Dashboard and select Review > Video to see the review.
    Press any key to close the application.

## <a name="navigate-to-your-video-transcript-review"></a>Video dökümü incelemeniz gidin

İçerik denetleyici gözden geçirme aracı video dökümü incelemede Git **gözden**>**Video**>**dökümü** ekran.

Aşağıdaki özellikler bakın:
- Eklediğiniz dökümü iki satırlık
- Uygunsuz metin terim bulundu ve metin denetleme hizmeti tarafından vurgulanmış
- Video zaman damgası başlatır transcription metni seçme

![İnsan araburucu için video dökümü gözden geçirme](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl oluşturulacağını öğrenin [video incelemeleri](video-reviews-quickstart-dotnet.md) gözden geçirme aracında.

Geliştirme konusunda ayrıntılı öğretici kullanıma bir [tamamlamak video yönetimini çözüm](video-transcript-moderation-review-tutorial-dotnet.md).

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için.
