---
title: Azure içerik aracı - .NET kullanarak incelemeler oluşturma | Microsoft Docs
description: .NET için Azure içerik denetleyici SDK'sını kullanarak nasıl oluşturulacağını gözden geçirme
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 6a0ff48f4ea17f9c800f3e6c096df2492699f87a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351839"
---
# <a name="create-reviews-using-net"></a>.NET kullanarak incelemeler oluşturma

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri için .NET için içerik denetleyici SDK'sını kullanarak kullanmaya başlama:
 
- İnsan denetleyiciler için incelemeler kümesi oluşturma
- İnsan denetleyiciler için varolan incelemeler durumunu Al

Genellikle, içerik İnsan gözden geçirilmek üzere zamanlandığı anlarda önce bazı otomatik yönetimini geçer. Bu makalede yalnızca İnsan yönetimini gözden oluşturma kapsar. Daha eksiksiz bir senaryo için bkz [Facebook içerik yönetimini](facebook-post-moderation.md) ve [e-ticaret katalog yönetimini](ecommerce-retail-catalog-moderation.md) öğreticileri.

Bu makalede, Visual Studio ve C# ile bilginiz olduğunu varsayar.

## <a name="sign-up-for-content-moderator-services"></a>İçerik denetleyici Hizmetleri için kaydolun

REST API veya SDK üzerinden içerik denetleyici Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir.
Başvurmak [Hızlı Başlangıç](quick-start.md) anahtarı nasıl edinebilirsiniz öğrenin.

## <a name="create-your-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Yeni bir ekleme **konsol uygulaması (.NET Framework)** çözümünüzü projeye.

   Örnek kodda proje adı **CreateReviews**.

1. Bu proje çözüme yönelik tek başlangıç projesi olarak seçin.

1. Bir başvuru ekleyin **ModeratorHelper** proje oluşturduğunuz derleme [içerik denetleyici istemci Yardımcısı quickstart](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Güncelleştirme program using deyimleri

Değiştirme program using deyimleri kullanıcının.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>İç içerik bilgileri bir gözden geçirme kimliği ile ilişkilendirmek için bir sınıf oluşturun

Aşağıdaki sınıf ekleme **Program** sınıfı.
İç içerik Kimliğinizi öğesi için gözden geçirme Kimliğine ilişkilendirmek için bu sınıf kullanın.

    /// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatma

> [!NOTE]
> İçerik denetleyici hizmeti anahtarınızı ikinci (RPS) hız sınırı başına bir istek varsa ve sınırı aşarsanız, SDK 429 hata kodunu içeren bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS hızı sınırı vardır.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Aşağıdaki sabitleri ekleyin **Program** Program.cs sınıfında.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Statik alanları ve aşağıdaki sabitleri ekleyin **Program** Program.cs sınıfında.

Bu değerler için aboneliği ve takım özgü bilgileri içerecek şekilde güncelleştirin.

> [!NOTE]
> TeamName sabiti oluştururken kullandığınız adına ayarlayın, [içerik denetleyici gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) abonelik. Gelen TeamName almak **kimlik bilgileri** bölümüne **ayarları** (dişli) menüsü.
>
> Takım adınızı değeri **kimliği** alanındaki **API** bölümü.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "{teamname}";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "{callbackUrl}";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Aşağıdaki statik alanları ekleme **Program** Program.cs sınıfında.

Uygulama durumunu izlemek için bu alanları kullanın.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>İletileri günlük dosyasına yazmak için bir yöntem oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Gözden geçirmeler kümesi oluşturmak için bir yöntem oluşturma

Normalde, gelen resim, metin tanımlamak için bazı iş mantığı sahip veya video, gözden geçirilmesi gerekir. Ancak, burada yalnızca bir sabit listesi görüntülerinin kullanın.

**Program** sınıfına aşağıdaki yöntemi ekleyin.

    /// <summary>
    /// Create the reviews using the fixed list of images.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void CreateReviews(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Creating reviews for the following images:", true);

        // Create the structure to hold the request body information.
        List<CreateReviewBodyItem> requestInfo =
            new List<CreateReviewBodyItem>();

        // Create some standard metadata to add to each item.
        List<CreateReviewBodyItemMetadataItem> metadata =
            new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

        // Populate the request body information and the initial cached review information.
        for (int i = 0; i < ImageUrls.Length; i++)
        {
            // Cache the local information with which to create the review.
            var itemInfo = new ReviewItem()
            {
                Type = MediaType,
                ContentId = i.ToString(),
                Url = ImageUrls[i],
                ReviewId = null
            };

            WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

            // Add the item informaton to the request information.
            requestInfo.Add(new CreateReviewBodyItem(
                itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
                CallbackEndpoint, metadata));

            // Cache the review creation information.
            reviewItems.Add(itemInfo);
        }

        var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
            "application/json", TeamName, requestInfo);

        // Update the local cache to associate the created review IDs with
        // the associated content.
        var reviewIds = reviewResponse.Result.Body;
        for (int i = 0; i < reviewIds.Count; i++)
        {
            Program.reviewItems[i].ReviewId = reviewIds[i];
        }

        WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Varolan incelemeler durumunu almak için bir yöntem oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin. 

> [!Note]
> Uygulamada, geri çağırma URL'si ayarlarsınız `CallbackEndpoint` el ile gözden geçirme (aracılığıyla bir HTTP POST isteği) sonuçlarını alacağı URL.
> Gözden geçirmeler bekleyen durumunu denetlemek için bu yöntem değiştirebilir.

    /// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>Gözden geçirmeler kümesi oluşturma ve durumunu denetlemek için kod ekleme

Aşağıdaki kodu ekleyin **ana** yöntemi.

Bu kod tanımlama ve liste yönetme, aynı zamanda ekran görüntüleri için listeyi kullanarak gerçekleştirdiğiniz işlemlerinin birçoğu benzetimini yapar. Günlüğe kaydetme özelliklerini içerik Aracı hizmeti için SDK çağrısı tarafından oluşturulan yanıt nesneleri görmenize olanak sağlar.

    using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        writer = outputWriter;
        using (var client = Clients.NewClient())
        {
            CreateReviews(client);
            GetReviewDetails(client);

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
            Thread.Sleep(latencyDelay * 1000);

            GetReviewDetails(client);
        }

        writer = null;
        outputWriter.Flush();
        outputWriter.Close();
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Programını çalıştırın ve çıktıyı gözden geçirin

Aşağıdaki örnek çıkış bakın:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

İçerik denetleyici içine oturum gözden ile gözden bekleyen görüntü görmek için aracı **sc** etiketi kümesine **doğru**. Ayrıca varsayılan bkz **bir** ve **r** etiketleri ve gözden geçirme aracı içinde tanımlanan herhangi bir özel etiket. 

Kullanım **sonraki** gönderme düğmesi.

![İnsan araburucu için görüntü gözden geçirme](images/moderation-reviews-quickstart-dotnet.PNG)

Devam etmek için herhangi bir tuşa basın.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Günlük dosyasında çıktı aşağıdaki göz atın.

> [!NOTE]
> Çıktı dosyanızda, dizeleri "\{teamname}" ve "\{callbackUrl}" için değerleri yansıtacak `TeamName` ve `CallbackEndpoint` alanlar, sırasıyla.

Kimlikleri gözden geçirme ve görüntünün URL'leri her zaman farklıdır içerik, uygulamayı çalıştırın ve bir gözden geçirme olduğunda tamamlayın, `reviewerResultTags` alan, gözden geçirenin nasıl öğesi etiketli yansıtır.

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="your-callback-url-if-provided-receives-this-response"></a>Geri çağırma URL'si sağladıysa, bu yanıt alır

Aşağıdaki örneğe benzer bir yanıt görürsünüz:

    {
        "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
        "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
        "ModifiedBy": "yourusername",
        "CallBackType": "Review",
        "ContentId": "0",
        "ContentType": "Image",
        "Metadata": {
            "sc": "true"
            },
        "ReviewerResultTags": {
            "a": "False",
            "r": "False",
        }
    }


## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için ve tümleştirme üzerinde başlayın.
