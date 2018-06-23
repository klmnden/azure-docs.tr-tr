---
title: Azure içerik aracı - .NET kullanarak video incelemeler oluşturma | Microsoft Docs
description: .NET için Azure içerik denetleyici SDK'sını kullanarak video oluşturma gözden geçirme
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/18/2018
ms.author: sajagtap
ms.openlocfilehash: cb487314b8695f3676fdb22a9d7e3ec5ca3ed9f2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351875"
---
# <a name="create-video-reviews-using-net"></a>.NET kullanarak video incelemeler oluşturma

Bu makalede bilgiler sağlanmaktadır ve hızlı bir şekilde yardımcı olmak için kod örnekleri içerik denetleyici SDK'sı ile C# kullanmaya başlama:

- Video İnceleme İnsan denetleyiciler için oluşturma
- Bir gözden geçirme çerçeveleri ekleyin
- Gözden geçirme için çerçeveleri Al 
- Gözden ayrıntılarını ve durumunu Al
- Gözden geçirme yayımlama

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sahibi olduğunuzu varsayar [video aracılı (hızlı başlangıç bakın)](video-moderation-api.md) ve yanıt verileri. Bu çerçeve tabanlı incelemeler İnsan denetleyiciler için oluşturmak için gerekir.

Bu makalede ayrıca Visual Studio ve C# ile bilginiz olduğunu varsayar.

### <a name="sign-up-for-content-moderator-services"></a>İçerik denetleyici Hizmetleri için kaydolun

REST API veya SDK üzerinden içerik denetleyici Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir.

İçerik denetleyici panosunda abonelik anahtarınızı bulabilirsiniz **ayarları** > **kimlik bilgileri** > **API**  >  **Deneme Ocp-Apim-Subscription-Key**. Daha fazla bilgi için bkz: [genel bakış](overview.md).

### <a name="prepare-your-video-and-the-video-frames-for-review"></a>Video ve görüntü çerçeveleri gözden geçirme için hazırlama

Bunların URL'lerini gerektiğinden gözden geçirmek için video ve örnek video çerçeveleri çevrimiçi yayımlanması gerekir.

> [!NOTE]
> Program video el ile kaydedilmiş ekran görüntüleri ile rastgele bir yetişkinin/saldırganlardan puanları gözden geçirme API kullanımını göstermek için kullanır. Gerçek dünya durumda kullandığınız [video yönetimini çıktı](video-moderation-api.md#run-the-program-and-review-the-output) görüntüleri oluşturmak ve puanları atamak için. 

Video için İnceleme aracı video oynatıcı görünümünde oynatılır. böylece bir akış uç gerekir.

![Video demo küçük resim](images/ams-video-demo-view.PNG)

- Kopya **URL** bu [Azure Media Services gösteri](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) bildirim URL'si için sayfa.

Video çerçevelerini (görüntüler), aşağıdaki görüntüleri kullanın:

![Video çerçeve küçük resim 1](images/ams-video-frame-thumbnails-1.PNG) | ![Video çerçeve küçük resim 2](images/ams-video-frame-thumbnails-2.PNG) | ![Video çerçeve küçük resim 3](images/ams-video-frame-thumbnails-3.PNG) |
| :---: | :---: | :---: |
[Çerçeve 1](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG) | [Çerçeve 2](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG) | [Çerçeve 3](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG) |

## <a name="create-your-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Yeni bir ekleme **konsol uygulaması (.NET Framework)** çözümünüzü projeye.

1. Proje adı **VideoReviews**.

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

Aşağıdaki özel özellikleri ad alanına VideoReviews, sınıf Program ekleyin.

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

Aşağıdaki yöntem tanımını ad alanına VideoReviews, sınıf Program ekleyin.

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
> İçerik denetleyici hizmeti anahtarınızı ikinci (RPS) hız sınırı başına bir istek varsa ve sınırı aşarsanız, SDK 429 hata kodunu içeren bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS hızı sınırı vardır.

## <a name="add-video-frames-to-the-video-review"></a>Video incelemeye video çerçeveleri ekleyin

Bir video gözden geçirme ile video çerçeveleri eklemek **ContentModeratorClient.Reviews.AddVideoFrameUrl** (video çerçevelerinizi çevrimiçi barındırılıyorsa) veya **ContentModeratorClient.Reviews.AddVideoFrameStream** () video çerçevelerinizi yerel olarak barındırılıyorsa). Bu hızlı başlangıç video çerçevelerinizi çevrimiçi olarak barındırılan ve bu nedenle kullanır varsayar **AddVideoFrameUrl**. Daha fazla bilgi için bkz: [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd).

**AddVideoFrameUrl** aşağıdaki gerekli parametreleri:
1. "Application/json." olması gereken bir MIME türü içeren bir dize
1. İçerik denetleyici takım adı.
1. Video İnceleme Kimliği tarafından döndürülen **CreateVideoReviews**.
1. Bir **IList<VideoFrameBodyItem>**  nesnesi. Her **VideoFrameBodyItem** nesnesi video çerçeveyi temsil eder.

**VideoFrameBodyItem** aşağıdaki özelliklere sahiptir:
- **Zaman damgası**. Saniye cinsinden zaman içinden video karesi alınmış videoda içeren bir dize.
- **FrameImage**. Video çerçeve URL'si.
- **Meta veri**. IList<VideoFrameBodyItemMetadataItem>. **VideoFrameBodyItemMetadataItem** yalnızca bir anahtar/değer çiftidir. Geçerli anahtarlar şunlardır:
- **reviewRecommended**. Video çerçeve İnsan gözden önerilen true.
- **adultScore**. Bir değeri 0'dan yetişkinlere yönelik içeriğe önemini video çerçevede derecelendirir 1.
- **bir**. Video yetişkinlere yönelik içeriğe içeriyorsa true.
- **racyScore**. Bir değeri 0'dan saldırganlardan içerik önemini video çerçevede derecelendirir 1.
- **r**. Video çerçeve saldırganlardan içerik içeriyorsa true.
- **ReviewerResultTags**. IList<VideoFrameBodyItemReviewerResultTagsItem>. **VideoFrameBodyItemReviewerResultTagsItem** yalnızca bir anahtar/değer çiftidir. Bir uygulama bu etiketler video çerçeveleri düzenlemek için kullanabilir.

> [!NOTE]
> Bu Hızlı Başlangıç için rastgele değerler oluşturur **adultScore** ve **racyScore** özellikleri. Bir üretim uygulamasında bu değerleri elde edebilirsiniz [video denetleme hizmet](video-moderation-api.md), Azure medya hizmeti olarak dağıtılmış.

Aşağıdaki yöntem tanımlarını ad alanına VideoReviews, sınıf Program ekleyin.

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
    

## <a name="get-video-frames-for-video-review"></a>Video gözden geçirilmek üzere video çerçeveleri Al

Video çerçeveleri ile video İnceleme için alabileceğiniz **ContentModeratorClient.Reviews.GetVideoFrames**. **GetVideoFrames** aşağıdaki gerekli parametreleri:
1. İçerik denetleyici takım adı.
1. Video İnceleme Kimliği tarafından döndürülen **CreateVideoReviews**.
1. Alınacak ilk video çerçevesini sıfır tabanlı dizini.
1. Alınacak video çerçeve sayısı.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, sınıf Program ekleyin.

    /// <summary>
    /// Get the video frames assigned to the indicated video review.  For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7ba43e7151f0b10d45200
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    static void GetFrames(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Getting frames for the review with ID {0}.", review_id);

        Frames result = client.Reviews.GetVideoFrames(TeamName, review_id, 0, Int32.MaxValue);
        Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="get-video-review-information"></a>Video gözden geçirme bilgi alın

Bir video gözden geçirmesine bilgilerini edinin **ContentModeratorClient.Reviews.GetReview**. **GetReview** aşağıdaki gerekli parametreleri:
1. İçerik denetleyici takım adı.
1. Video İnceleme Kimliği tarafından döndürülen **CreateVideoReviews**.

Aşağıdaki yöntem tanımını ad alanına VideoReviews, sınıf Program ekleyin.

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

Ekleme **ana** yöntem tanımını ad alanına VideoReviews, Program sınıfı. Son olarak, Program sınıfı ve VideoReviews ad kapatın.

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
            Console.Read();
        }
    }

## <a name="run-the-program-and-review-the-output"></a>Programını çalıştırın ve çıktıyı gözden geçirin
Uygulamayı çalıştırdığınızda, aşağıdaki satırları bir çıktı görmeniz gerekir:

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

## <a name="check-out-your-video-review"></a>Video incelemeniz denetleyin

Son olarak, içerik denetleyici video incelemede aracı hesabı gözden bkz **gözden**>**Video** ekran.

![İnsan araburucu için video gözden geçirme](images/ams-video-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Eklemeyi öğrenin [dökümü yönetimini](video-transcript-moderation-review-tutorial-dotnet.md) video gözden geçirilecek. 

Geliştirme konusunda ayrıntılı öğretici kullanıma bir [tamamlamak video yönetimini çözüm](video-transcript-moderation-review-tutorial-dotnet.md).

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için.
