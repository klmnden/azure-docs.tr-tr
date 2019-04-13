---
title: Video deşifre metni incelemeleri .NET - Content Moderator'ı kullanarak oluşturma
titlesuffix: Azure Cognitive Services
description: .NET için Content Moderator SDK'sını kullanarak video deşifre metni incelemeleri oluşturma
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/19/2019
ms.author: sajagtap
ms.openlocfilehash: a3d362f08765cc80b65659b406a2fac3af71f167
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59524506"
---
# <a name="create-video-transcript-reviews-using-net"></a>Video deşifre metni incelemeleri .NET kullanarak oluşturun

Bu makalede bilgiler ve kod örnekleri, hızlı bir şekilde yardımcı olması için kullanmaya başlama [Content Moderator SDK'sı ile C#](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için:

- Bir video gözden geçirme için İnsan Moderatörler oluşturma
- Yönetilen döküm gözden geçirici ekleyin
- Yayımlamayı gözden geçirme

## <a name="prerequisites"></a>Önkoşullar

- Oturum açma veya hesap üzerinde Content Moderator oluşturmak [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) , zaten yapmadıysanız site.
- Bu makalede, sahibi olduğunuzu varsayar [video aracılı](video-moderation-api.md) ve [video gözden oluşturulan](video-reviews-quickstart-dotnet.md) İnsan karar alma için gözden geçirme Aracı'nda. Şimdi gözden geçirme Aracı'nda yönetilen video dökümleri eklemek istediğiniz.

## <a name="ensure-your-api-key-can-call-the-review-api-job-creation"></a>API anahtarınızı (iş oluşturma) Gözden geçirme API çağrısı emin olun.

Önceki adımları tamamladıktan sonra, başlangıcı Azure portaldan yaptıysanız şu anda iki Content Moderator anahtarınız olmalıdır.

SDK örneğinizde Azure tarafından sağlanan API anahtarını kullanmayı planlıyorsanız, uygulamanızın inceleme API’sini çağırmasına ve incelemeler oluşturmasına izin vermek için [inceleme API'siyle Azure anahtarını kullanma](./review-tool-user-guide/configure.md#use-your-azure-account-with-the-review-apis) bölümünde anlatılan adımları izleyin.

İnceleme aracı tarafından oluşturulan ücretsiz deneme anahtarını kullanırsanız, inceleme aracı hesabınız anahtarı zaten tanıdığından ek bir adım gerekmez.

## <a name="prepare-your-video-for-review"></a>Videonuzu gözden geçirmeniz için hazırlama

Transkripti video bir gözden geçirici ekleyin. Video çevrimiçi yayımlanması gerekir. Kendi akış uç noktasını ihtiyacınız vardır. Akış uç noktasını videoyu oynatmak gözden geçirme aracı video oynatıcı sağlar.

![Tanıtım videosu küçük resmi](images/ams-video-demo-view.PNG)

- Kopyalama **URL** bu [Azure Media Services tanıtım](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) bildirim URL'sini sayfası.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturma

1. Çözümünüze yeni bir **Console uygulaması (.NET Framework)** projesi ekleyin.

1. Projeyi adlandırın **VideoTranscriptReviews**.

1. Bu projeyi çözümün tek başlatma projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

TermLists projesi için aşağıdaki NuGet paketlerini yükleyin.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Programı deyimler kullanarak güncelleştirme

Değiştirme program gibi deyimleri kullanarak.


```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
```

### <a name="add-private-properties"></a>Özel özellikler ekleme

Aşağıdaki özel özellikleri ad VideoTranscriptReviews, Program sınıfı ekleyin.

Belirtilen yerlerde, bu özellikler için örnek değerleri değiştirin.

```csharp
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
        /// the Content Moderator web site. Your team name is the Id associated 
        /// with your subscription.</remarks>
        private const string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

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
```

### <a name="create-content-moderator-client-object"></a>Content Moderator istemci nesnesi oluşturma

Aşağıdaki yöntem tanımını ad alanına VideoTranscriptReviews, Program sınıfı ekleyin.

```csharp
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
        Endpoint = AzureBaseURL
    };
}
```

## <a name="create-a-video-review"></a>Bir video gözden geçirmesi oluşturma

İle video bir gözden geçirme oluşturmak **ContentModeratorClient.Reviews.CreateVideoReviews**. Daha fazla bilgi için bkz. [API başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** aşağıdaki parametreler gereklidir:
1. "Application/json." olması gereken bir MIME türü içeren bir dize 
1. Content Moderator takım adı.
1. Bir **IList\<CreateVideoReviewsBodyItem >** nesne. Her **CreateVideoReviewsBodyItem** nesnesini gösteren bir video gözden geçirin. Bu hızlı başlangıçta bir kerede bir inceleme oluşturur.

**CreateVideoReviewsBodyItem** birçok özelliğe sahiptir. En azından aşağıdaki özellikleri ayarlayın:
- **İçerik**. Gözden geçirilmesi video URL'si.
- **ContentID**. Video incelemeye atamak için bir kimlik.
- **Durum**. "Yayımlanmamış." değerine ayarlayın Bunu ayarlamazsanız "Bekliyor", video gözden yayımlanan anlamına gelir ve insan tarafından İnceleme bekleyen varsayar. Bir video incelemesi yayımlandıktan sonra artık yakalayın, bir döküm veya transkript denetimi sonucu için ekleyebilirsiniz.

> [!NOTE]
> **CreateVideoReviews** bir IList döndürür<string>. Bu dizelerin her biri, video incelemesi için bir kimlik içerir. Bu kimliklerinin GUID'leri ve değeri ile aynı değil **ContentID** özelliği.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, Program sınıfı ekleyin.

```csharp
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
```

> [!NOTE]
> Content Moderator hizmet anahtarınızın saniyede istek sayısı (RPS) hız sınırı vardır. Sınırı aşarsanız, SDK 429 hata koduyla bir özel durum oluşturulur.
>
> Ücretsiz katman anahtarı bir RPS'lik hız sınırına sahiptir.

## <a name="add-transcript-to-video-review"></a>Transkript video gözden geçirici ekleyin

Video İnceleme ile bir döküm eklediğiniz **ContentModeratorClient.Reviews.AddVideoTranscript**. **AddVideoTranscript** aşağıdaki parametreler gereklidir:
1. Content Moderator takım kimliği
1. Video gözden geçirme kimliği tarafından döndürülen **CreateVideoReviews**.
1. A **Stream** transkripti içeren nesne.

Transkripti WebVTT biçiminde olması gerekir. Daha fazla bilgi için [WebVTT: Web Video metin biçimi izler](https://www.w3.org/TR/webvtt1/).

> [!NOTE]
> Program VTT biçiminde örnek dökümü kullanır. Azure Media Indexer hizmeti için kullandığınız bir gerçek çözümde [bir döküm oluşturma](https://docs.microsoft.com/azure/media-services/media-services-index-content) bir video.

Aşağıdaki yöntem tanımını ad alanına VideotranscriptReviews, Program sınıfı ekleyin.

```csharp
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
```

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Transkript denetimi sonucu video gözden geçirici ekleyin

Video İnceleme için bir döküm eklemenin yanı sıra, bu döküm yönetme sonucunu da ekleyin. İle bunu **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**. Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranscriptModerationResult** aşağıdaki parametreler gereklidir:
1. "Application/json." olması gereken bir MIME türü içeren bir dize 
1. Content Moderator takım adı.
1. Video gözden geçirme kimliği tarafından döndürülen **CreateVideoReviews**.
1. Bir IList\<TranscriptModerationBodyItem >. A **TranscriptModerationBodyItem** aşağıdaki özelliklere sahiptir:
1. **Koşulları**. Bir IList\<TranscriptModerationBodyItemTermsItem >. A **TranscriptModerationBodyItemTermsItem** aşağıdaki özelliklere sahiptir:
1. **Dizin**. Terim sıfır tabanlı dizini.
1. **Terim**. Terimini içeren bir dize.
1. **Zaman damgası**. Saniye cinsinden zaman içinde bulunan ve koşulları kimse içeren bir dize.

Transkripti WebVTT biçiminde olması gerekir. Daha fazla bilgi için [WebVTT: Web Video metin biçimi izler](https://www.w3.org/TR/webvtt1/).

Aşağıdaki yöntem tanımını ad alanına VideoTranscriptReviews, Program sınıfı ekleyin. Bu yöntem için bir döküm gönderen **ContentModeratorClient.TextModeration.ScreenText** yöntemi. Ayrıca bir IList sonucu çevirir\<TranscriptModerationBodyItem > ve gönderildiği **AddVideoTranscriptModerationResult**.

```csharp
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
```

## <a name="publish-video-review"></a>Yayımlamayı video gözden geçirme

Bir video incelemesiyle yayımlama **ContentModeratorClient.Reviews.PublishVideoReview**. **PublishVideoReview** aşağıdaki parametreler gereklidir:
1. Content Moderator takım adı.
1. Video gözden geçirme kimliği tarafından döndürülen **CreateVideoReviews**.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, Program sınıfı ekleyin.

```csharp
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
```

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme

Ekleme **ana** yöntem tanımını ad alanına VideoTranscriptReviews, Program sınıfı. Son olarak, Program sınıfına ve VideoTranscriptReviews ad alanı kapatın.

> [!NOTE]
> Program VTT biçiminde örnek dökümü kullanır. Azure Media Indexer hizmeti için kullandığınız bir gerçek çözümde [bir döküm oluşturma](https://docs.microsoft.com/azure/media-services/media-services-index-content) bir video.

```csharp
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
        Console.ReadKey();
    }
}
```

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

Uygulamayı çalıştırdığınızda, aşağıdaki satırları bir çıktı görmeniz gerekir:

```console
Creating a video review.
Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Open your Content Moderator Dashboard and select Review > Video to see the review.
Press any key to close the application.
```

## <a name="navigate-to-your-video-transcript-review"></a>Video deşifre metni gözden geçirmeniz için gidin

Content Moderator İnceleme aracında video deşifre metni İnceleme Git **gözden**>**Video**>**döküm** ekran.

Aşağıdaki özellikleri görürsünüz:
- Transkript eklediğiniz iki satırlık
- Bulunan ve metin denetimi hizmeti tarafından vurgulanan küfür terimi
- Video itibaren zaman damgası başlar transkripsiyonu metni seçme

![İnsan Moderatörler için video deşifre metni İnceleme](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Alma [Content Moderator .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar için .NET için.

Nasıl oluşturacağınızı öğrenin [video incelemeleri](video-reviews-quickstart-dotnet.md) gözden geçirme Aracı'nda.

Ayrıntılı öğretici nasıl geliştirebileceğinizi kullanıma bir [tamamlamak video denetimi çözümü](video-transcript-moderation-review-tutorial-dotnet.md).
