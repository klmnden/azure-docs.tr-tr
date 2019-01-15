---
title: "Hızlı Başlangıç: .NET - Content Moderator'ı kullanarak incelemeleri oluşturma"
titlesuffix: Azure Cognitive Services
description: .NET için Azure Content Moderator SDK'sını kullanarak incelemeler oluşturma.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 8a5c11df1b6353a100dd5f1cf388308b9c048932
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258897"
---
# <a name="quickstart-create-reviews-using-net"></a>Hızlı Başlangıç: .NET kullanarak incelemeleri oluşturma

Bu makalede, aşağıdaki amaçlarla [.NET için Content Moderator SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)'nı kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanmaktadır:
 
- İnsan moderatörler için bir inceleme kümesi oluşturma
- İnsan moderatörler için mevcut incelemelerin durumunu alma

Genellikle, içerik insan incelemesi için zamanlanmadan önce otomatik moderasyondan geçer. Bu makale yalnızca insan moderasyonu için inceleme oluşturma konusunu ele alır. Daha eksiksiz bir senaryo için bkz [Facebook içerik denetleme](facebook-post-moderation.md) ve [Orta e-ticaret ürün görüntüleri](ecommerce-retail-catalog-moderation.md) öğreticiler.

Bu makale, Visual Studio ve C# hakkında bilgi sahibi olduğunuzu varsayar.

## <a name="sign-up-for-content-moderator"></a>Content Moderator için kaydolma

REST API veya SDK aracılığıyla Content Moderator hizmetlerini kullanabilmeniz için önce bir abonelik anahtarınız olması gerekir. Content Moderator'a abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergelerini izleyin.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Önceki adımda yapmadıysanız bir inceleme araç hesabına kaydolun

Content Moderator’ı Azure portaldan aldıysanız, [inceleme aracı hesabına da kaydolun](https://contentmoderator.cognitive.microsoft.com/) ve bir inceleme takımı oluşturun. Bir İşi başlatmak ve inceleme aracındaki incelemeleri görüntülemek üzere inceleme API'sini çağırmak için ekip kimliği ve inceleme aracı gerekir.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>API anahtarınızın inceleme oluşturma amacıyla inceleme API'sini çağırabildiğinden emin olun

Önceki adımları tamamladıktan sonra, başlangıcı Azure portaldan yaptıysanız şu anda iki Content Moderator anahtarınız olmalıdır. 

SDK örneğinizde Azure tarafından sağlanan API anahtarını kullanmayı planlıyorsanız, uygulamanızın inceleme API’sini çağırmasına ve incelemeler oluşturmasına izin vermek için [inceleme API'siyle Azure anahtarını kullanma](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) bölümünde anlatılan adımları izleyin.

İnceleme aracı tarafından oluşturulan ücretsiz deneme anahtarını kullanırsanız, inceleme aracı hesabınız anahtarı zaten tanıyordur ve bu nedenle ek bir adım gerekli değildir.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Çözümünüze yeni bir **Console uygulaması (.NET Framework)** projesi ekleyin.

   Örnek kodda, projeyi **CreateReviews** olarak adlandırın.

1. Bu projeyi çözümün tek başlatma projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Programı deyimler kullanarak güncelleştirme

Programı deyimler kullanarak değiştirin.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

Aboneliğiniz için bir Content Moderator istemcisi oluşturmak üzere aşağıdaki kodu ekleyin.

> [!IMPORTANT]
> **AzureRegion** ve **CMSubscriptionKey** alanlarını bölge tanımlayıcınız ve abonelik anahtarınız ile değiştirin.


    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>İç içerik bilgilerini bir inceleme kimliği ile ilişkilendirmek için sınıf oluşturma

Aşağıdaki sınıfı **Program** sınıfına ekleyin.
İnceleme kimliğini öğenin iç içerik kimliği ile ilişkilendirmek için bu sınıfı kullanın.

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
> Content Moderator hizmet anahtarınızın saniye başına istek (RPS) hız limiti vardır ve sınırı aşarsanız SDK 429 hata kodu ile bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS’lik hız sınırına sahiptir.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Aşağıdaki sabitleri Program.cs dosyasında **Program** sınıfına ekleyin.
    
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
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Aşağıdaki sabitleri ve statik alanları Program.cs dosyasında **Program** sınıfına ekleyin.

Bu değerleri aboneliğinize ve takımınıza özel bilgiler içerecek şekilde güncelleştirin.

> [!NOTE]
> TeamName sabitini, [Content Moderator inceleme aracı](https://contentmoderator.cognitive.microsoft.com/) aboneliğinizi oluştururken kullandığınız ada ayarlayın. TeamName değerini **Ayarlar** (dişli) menüsündeki **Kimlik Bilgileri** bölümünden alın.
>
> Takım adınız **API** bölümündeki **Id** alanının değeridir.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Content Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "YOUR REVIEW TEAM ID";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "YOUR API ENDPOINT";

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

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Aşağıdaki statik alanları Program.cs dosyasında **Program** sınıfına ekleyin.

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

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Günlük dosyasına iletileri yazmak için yöntem oluşturma

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

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Bir inceleme kümesi oluşturmak için yöntem oluşturma

Normalde, incelenmesi gereken gelen görüntü, metin veya videoları tanımlamak için bir iş mantığınız vardır. Ancak burada yalnızca görüntülerin sabit listesini kullanmanız yeterlidir.

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

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Mevcut incelemelerin durumunu almak için yöntem oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin. 

> [!Note]
> Uygulamada, `CallbackEndpoint` geri arama URL’sini el ile incelemenin sonuçlarını alacak (bir HTTP POST isteği aracılığıyla) URL’ye ayarlarsınız.
> Bekleyen incelemelerin durumunu denetlemek için bu yöntemi değiştirebilirsiniz.

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

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>İnceleme kümesi oluşturmak ve durumunu denetlemek için kod ekleme

Aşağıdaki kodu **Main** yöntemine ekleyin.

Bu kod, listeyi tanımlayıp yönetmek ve listeyi kullanarak görüntüleri taramak için gerçekleştirdiğiniz işlemlerin birçoğunun benzetimini yapar. Günlüğe kaydetme özellikleri, Content Moderator hizmetine yapılan SDK çağrıları tarafından oluşturulan yanıt nesnelerini görmenize olanak tanır.

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

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

Aşağıdaki örnek çıktıyı görürsünüz:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Bekleyen görüntü incelemesini **sc** etiketi **true** olarak ayarlanmış halde görmek için Content Moderator inceleme aracında oturum açın. Ayrıca varsayılan **a** ile **r** etiketlerini ve inceleme aracı içinde tanımlamış olabileceğiniz tüm özel etiketleri görürsünüz. 

Göndermek için **İleri** düğmesini kullanın.

![İnsan moderatörler için görüntü incelemesi](images/moderation-reviews-quickstart-dotnet.PNG)

Ardından, devam etmek için herhangi bir tuşa basın.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Günlük dosyasında aşağıdaki çıktıya göz atın.

> [!NOTE]
> Çıktı dosyanızda "\{teamname}" ve "\{callbackUrl}" dizeleri sırasıyla `TeamName` ve `CallbackEndpoint` alanlarının değerlerini yansıtır.

İnceleme kimlikleri ve görüntü içerik URL’leri, uygulamayı her çalıştırdığınızda farklıdır ve bir inceleme tamamlandığında `reviewerResultTags` alanı inceleyenin öğeyi nasıl etiketlediğini yansıtır.

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

## <a name="your-callback-url-if-provided-receives-this-response"></a>Belirtilmişse geri arama Url’niz bu yanıtı alır

Şu örneğe benzer bir yanıt alırsınız:

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

Bu ve diğer .NET için Content Moderator hızlı başlangıçları için [Content Moderator .NET SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümünü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) alın ve tümleştirmeniz üzerinde çalışmaya başlayın.
