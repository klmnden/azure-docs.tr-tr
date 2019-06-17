---
title: Görüntü incelemeleri .NET - Content Moderator'ı kullanarak oluşturma
titlesuffix: Azure Cognitive Services
description: Bu makalede bilgiler ve kod örnekleri, hızlı bir şekilde yardımcı olmak için Content Moderator SDK'sı ile kullanmaya başlama C# video incelemeleri oluşturmak için.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/19/2019
ms.author: sajagtap
ms.openlocfilehash: c8572e6bc7fdedb68527cad4e7ea4109838089b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64726839"
---
# <a name="create-video-reviews-using-net"></a>Görüntü incelemeleri .NET kullanarak oluşturun

Bu makalede bilgiler ve kod örnekleri, hızlı bir şekilde yardımcı olması için kullanmaya başlama [Content Moderator SDK'sı ile C#](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için:

- Bir video gözden geçirme için İnsan Moderatörler oluşturma
- Çerçeve bir gözden geçirici ekleyin
- Gözden geçirmeyi çerçeveleri Al
- Durumu ve İnceleme ayrıntılarını Al
- Yayımlamayı gözden geçirme

## <a name="prerequisites"></a>Önkoşullar

- Oturum açma veya hesap üzerinde Content Moderator oluşturmak [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) site.
- Bu makalede, sahibi olduğunuzu varsayar [video aracılı (bkz. Hızlı Başlangıç)](video-moderation-api.md) ve yanıt verileri. Çerçeve tabanlı incelemeleri İnsan Moderatörler için oluşturmak için ihtiyacınız.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>API anahtarınızın inceleme oluşturma amacıyla inceleme API'sini çağırabildiğinden emin olun

Önceki adımları tamamladıktan sonra, başlangıcı Azure portaldan yaptıysanız şu anda iki Content Moderator anahtarınız olmalıdır.

SDK örneğinizde Azure tarafından sağlanan API anahtarını kullanmayı planlıyorsanız, uygulamanızın inceleme API’sini çağırmasına ve incelemeler oluşturmasına izin vermek için [inceleme API'siyle Azure anahtarını kullanma](review-tool-user-guide/configure.md#use-your-azure-account-with-the-review-apis) bölümünde anlatılan adımları izleyin.

İnceleme aracı tarafından oluşturulan ücretsiz deneme anahtarını kullanırsanız, inceleme aracı hesabınız anahtarı zaten tanıdığından ek bir adım gerekmez.

### <a name="prepare-your-video-and-the-video-frames-for-review"></a>Videonuzu ve video kareleri gözden geçirmeniz için hazırlama

Bunların URL'lerini gerekir çünkü gözden geçirmek için video ve örnek video kareleri çevrimiçi yayımlanması gerekir.

> [!NOTE]
> Program İnceleme API kullanımını göstermek için rastgele bir yetişkin/müstehcen puanları ile el ile kaydedilen ekran görüntüleri video kullanır. Gerçek bir durumda, kullandığınız [video denetimi çıkış](video-moderation-api.md#run-the-program-and-review-the-output) görüntülerini oluşturma ve puanları atayın. 

İnceleme aracını video oynatıcı Görünümü'nde oynatılır. böylece video için bir akış uç noktası gerekir.

![Tanıtım videosu küçük resmi](images/ams-video-demo-view.PNG)

- Kopyalama **URL** bu [Azure Media Services tanıtım](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) bildirim URL'sini sayfası.

Video kareleri için (görüntüleri) şu görüntüleri kullanın:

![Video çerçeve küçük resmi 1](images/ams-video-frame-thumbnails-1.PNG) | ![Video çerçeve küçük resim 2](images/ams-video-frame-thumbnails-2.PNG) | ![Video çerçeve küçük resmi 3](images/ams-video-frame-thumbnails-3.PNG) |
| :---: | :---: | :---: |
[Çerçeve 1](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG) | [Çerçeve 2](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG) | [Çerçeve 3](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG) |

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturma

1. Çözümünüze yeni bir **Console uygulaması (.NET Framework)** projesi ekleyin.

1. Projeyi adlandırın **VideoReviews**.

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

Aşağıdaki özel özellikleri ad VideoReviews, Program sınıfı ekleyin.

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

Aşağıdaki yöntem tanımını ad alanına VideoReviews, Program sınıfı ekleyin.

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
> **CreateVideoReviews** bir IList döndürür\<dizesi >. Bu dizelerin her biri, video incelemesi için bir kimlik içerir. Bu kimliklerinin GUID'leri ve değeri ile aynı değil **ContentID** özelliği. 

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
> Content Moderator hizmet anahtarınızın saniye başına istek (RPS) hız sınırı vardır ve bu sınırı aşarsanız SDK 429 hata kodu ile bir özel durum oluşturur.
>
> Ücretsiz katman anahtarı bir RPS'lik hız sınırına sahiptir.

## <a name="add-video-frames-to-the-video-review"></a>Video kareleri video gözden geçirici ekleyin

Video İnceleme ile video kareleri eklediğiniz **ContentModeratorClient.Reviews.AddVideoFrameUrl** (video çerçevelerinizi çevrimiçi barındırılmıyorsa) veya **ContentModeratorClient.Reviews.AddVideoFrameStream** () video çerçevelerinizi yerel olarak barındırılıyorsa). Bu hızlı başlangıçta, video kareleri çevrimiçi barındırılan ve bu nedenle kullanır varsayar **AddVideoFrameUrl**. Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd).

**AddVideoFrameUrl** aşağıdaki parametreler gereklidir:
1. "Application/json." olması gereken bir MIME türü içeren bir dize
1. Content Moderator takım adı.
1. Video gözden geçirme kimliği tarafından döndürülen **CreateVideoReviews**.
1. Bir **IList\<VideoFrameBodyItem >** nesne. Her **VideoFrameBodyItem** nesne video çerçeveyi temsil eder.

**VideoFrameBodyItem** aşağıdaki özelliklere sahiptir:
- **Zaman damgası**. Saniye cinsinden zaman video karesi alınmış videoda içeren bir dize.
- **FrameImage**. Video çerçeve URL'si.
- **Meta veri**. Bir IList\<VideoFrameBodyItemMetadataItem >. **VideoFrameBodyItemMetadataItem** yalnızca bir anahtar/değer çiftidir. Geçerli anahtarlar şunlardır:
- **reviewRecommended**. Bir insan tarafından İnceleme video çerçevenin önerilen true.
- **adultScore**. Yetişkinlere yönelik içeriğe önemini video çerçevede fiyatları 1 değeri 0.
- **bir**. Video yetişkinlere yönelik içeriğe içeriyorsa true.
- **racyScore**. Bir değeri 0'dan video çerçevede müstehcen içerik önemini derecelendirir 1.
- **r**. Çerçevenin video müstehcen içerik içeriyorsa true.
- **ReviewerResultTags**. Bir IList\<VideoFrameBodyItemReviewerResultTagsItem >. **VideoFrameBodyItemReviewerResultTagsItem** yalnızca bir anahtar/değer çiftidir. Bir uygulama, video kareleri düzenlemek için bu etiketleri kullanabilirsiniz.

> [!NOTE]
> Bu Hızlı Başlangıç için rastgele değerler oluşturmaktadır **adultScore** ve **racyScore** özellikleri. Bir üretim uygulamasında, bu değerleri elde edebilirsiniz [video denetimi hizmeti](video-moderation-api.md), Azure medya hizmeti olarak dağıtılmış.

Aşağıdaki yöntem tanımlarını ad alanına VideoReviews, Program sınıfı ekleyin.

```csharp
<summary>
/// Create a video frame to add to a video review after the video review is created.
/// </summary>
/// <param name="url">The URL of the video frame image.</param>
/// <returns>The video frame.</returns>
private static VideoFrameBodyItem CreateFrameToAddToReview(string url, string timestamp_seconds)
{
    // We generate random "adult" and "racy" scores for the video frame.
    Random rand = new Random();

    var frame = new VideoFrameBodyItem
    {
        // The timestamp is measured in milliseconds. Convert from seconds.
        Timestamp = (int.Parse(timestamp_seconds) * 1000).ToString(),
        FrameImage = url,

        Metadata = new List<VideoFrameBodyItemMetadataItem>
        {
            new VideoFrameBodyItemMetadataItem("reviewRecommended", "true"),
            new VideoFrameBodyItemMetadataItem("adultScore", rand.NextDouble().ToString()),
            new VideoFrameBodyItemMetadataItem("a", "false"),
            new VideoFrameBodyItemMetadataItem("racyScore", rand.NextDouble().ToString()),
            new VideoFrameBodyItemMetadataItem("r", "false")
        },

        ReviewerResultTags = new List<VideoFrameBodyItemReviewerResultTagsItem>()
        {
            new VideoFrameBodyItemReviewerResultTagsItem("tag1", "value1")
        }
    };

    return frame;
}
```

```csharp
/// <summary>
/// Add a video frame to the indicated video review. For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
/// <param name="url">The URL of the video frame image.</param>
static void AddFrame(ContentModeratorClient client, string review_id, string url, string timestamp_seconds)
{
    Console.WriteLine("Adding a frame to the review with ID {0}.", review_id);

    var frames = new List<VideoFrameBodyItem>()
    {
        CreateFrameToAddToReview(url, timestamp_seconds)
    };
        
    client.Reviews.AddVideoFrameUrl("application/json", TeamName, review_id, frames);

    Thread.Sleep(throttleRate);
```

## <a name="get-video-frames-for-video-review"></a>Video kareleri video gözden geçirmeniz için Al

Video kareleri ile video incelemesi için alabilirsiniz **ContentModeratorClient.Reviews.GetVideoFrames**. **GetVideoFrames** aşağıdaki parametreler gereklidir:
1. Content Moderator takım adı.
1. Video gözden geçirme kimliği tarafından döndürülen **CreateVideoReviews**.
1. Almak için ilk video çerçevesini sıfır tabanlı dizini.
1. Alınacak video çerçeve sayısı.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, Program sınıfı ekleyin.

```csharp
/// <summary>
/// Get the video frames assigned to the indicated video review.  For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7ba43e7151f0b10d45200
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
static void GetFrames(ContentModeratorClient client, string review_id)
{
    Console.WriteLine("Getting frames for the review with ID {0}.", review_id);

    Frames result = client.Reviews.GetVideoFrames(TeamName, review_id, 0);
    Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

    Thread.Sleep(throttleRate);
}
```

## <a name="get-video-review-information"></a>Video gözden geçirme bilgi alın

İle video gözden geçirme hakkında bilgi alın **ContentModeratorClient.Reviews.GetReview**. **GetReview** aşağıdaki parametreler gereklidir:
1. Content Moderator takım adı.
1. Video gözden geçirme kimliği tarafından döndürülen **CreateVideoReviews**.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, Program sınıfı ekleyin.

```csharp
/// <summary>
/// Get the information for the indicated video review. For more information, see the reference API:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
private static void GetReview(ContentModeratorClient client, string review_id)
{
    Console.WriteLine("Getting the status for the review with ID {0}.", review_id);

    var result = client.Reviews.GetReview(ModeratorHelper.Clients.TeamName, review_id);
    Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

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

Ekleme **ana** yöntem tanımını ad alanına VideoReviews, Program sınıfı. Son olarak, Program sınıfına ve VideoReviews ad alanı kapatın.

```csharp
static void Main(string[] args)
{
    using (ContentModeratorClient client = NewClient())
    {
        // Create a review with the content pointing to a streaming endpoint (manifest)
        var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
        string review_id = CreateReview(client, "review1", streamingcontent);

        var frame1_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG";
        var frame2_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG";
        var frame3_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG";

        // Add the frames from 17, 64, and 144 seconds.
        AddFrame(client, review_id, frame1_url, "17");
        AddFrame(client, review_id, frame2_url, "64");
        AddFrame(client, review_id, frame3_url, "144");

        // Get frames information and show
        GetFrames(client, review_id);
        GetReview(client, review_id);

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

```json
Creating a video review.
Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Getting frames for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
{
    "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
    "VideoFrames": [
    {
        "Timestamp": "17000",
        "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_17000.PNG",
        "Metadata": [
        {
            "Key": "reviewRecommended",
            "Value": "true"
        },
        {
            "Key": "adultScore",
            "Value": "0.808312381528463"
        },
        {
            "Key": "a",
            "Value": "false"
        },
        {
            "Key": "racyScore",
            "Value": "0.846378884206702"
        },
        {
            "Key": "r",
            "Value": "false"
        }
        ],
        "ReviewerResultTags": [
        {
            "Key": "tag1",
            "Value": "value1"
        }
    ]
    },
    {
        "Timestamp": "64000",
        "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_64000.PNG",
        "Metadata": [
        {
            "Key": "reviewRecommended",
            "Value": "true"
        },
        {
            "Key": "adultScore",
            "Value": "0.576078300166912"
        },
        {
            "Key": "a",
            "Value": "false"
        },
        {
            "Key": "racyScore",
            "Value": "0.244768953064815"
        },
        {
            "Key": "r",
            "Value": "false"
        }
        ],
        "ReviewerResultTags": [
        {
            "Key": "tag1",
            "Value": "value1"
        }
    ]
    },
    {
        "Timestamp": "144000",
        "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_144000.PNG",
        "Metadata": [
        {
            "Key": "reviewRecommended",
            "Value": "true"
        },
        {
            "Key": "adultScore",
            "Value": "0.664480847150311"
        },
        {
            "Key": "a",
            "Value": "false"
        },
        {
            "Key": "racyScore",
            "Value": "0.933817870418456"
        },
        {
            "Key": "r",
            "Value": "false"
        }
        ],
        "ReviewerResultTags": [
        {
            "Key": "tag1",
            "Value": "value1"
        }
        ]
    }
    ]
}

Getting the status for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
{
    "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
    "SubTeam": "public",
    "Status": "UnPublished",
    "ReviewerResultTags": [],
    "CreatedBy": "testreview6",
    "Metadata": [
    {
        "Key": "FrameCount",
        "Value": "3"
    }
    ],
    "Type": "Video",
    "Content": "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
    "ContentId": "review1",
    "CallbackEndpoint": null
}

Publishing the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Open your Content Moderator Dashboard and select Review > Video to see the review.
Press any key to close the application.
```

## <a name="check-out-your-video-review"></a>Video incelemeniz denetleyin

Son olarak, aracı hesabı gözden geçirin, Content Moderator video incelemesindeki gördüğünüz **gözden**>**Video** ekran.

![İnsan Moderatörler için video gözden geçirme](images/ams-video-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Alma [Content Moderator .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar için .NET için.

Eklemeyi öğrenin [döküm denetimi](video-transcript-moderation-review-tutorial-dotnet.md) video gözden geçirilmesi. 

Ayrıntılı öğretici nasıl geliştirebileceğinizi kullanıma bir [tamamlamak video denetimi çözümü](video-transcript-moderation-review-tutorial-dotnet.md).
