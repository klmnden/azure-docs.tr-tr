---
title: Azure Content Moderator - .NET kullanarak incelemeleri oluştur | Microsoft Docs
description: .NET için Azure Content Moderator SDK'sını kullanarak nasıl oluşturulacağını gözden geçirmeleri
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: 32e18273ad92f6b415b2a0219fd8b0520fe707f3
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716154"
---
# <a name="create-reviews-using-net"></a>.NET kullanarak incelemeleri oluşturma

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri, kullanmaya başlama [Content Moderator SDK'sı .NET için](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için:
 
- İnsan Moderatörler için gözden geçirmeleri kümesi oluşturma
- İnsan Moderatörler için mevcut incelemeleri durumunu alın

Genellikle, içerik insan tarafından İnceleme için planlanan önce bazı otomatik denetimden geçer. Bu makalede, insan tarafından denetim için gözden geçirmesi oluşturma yalnızca kapsar. Daha eksiksiz bir senaryo için bkz [Facebook içerik denetleme](facebook-post-moderation.md) ve [Eticaret katalog denetimi](ecommerce-retail-catalog-moderation.md) öğreticiler.

Bu makalede, zaten Visual Studio ve C# ile ilgili bilgi sahibi olduğunuz varsayılır.

## <a name="sign-up-for-content-moderator"></a>Content Moderator için kaydolun

Content Moderator Hizmetleri REST API veya SDK aracılığıyla kullanabilmeniz için önce bir abonelik anahtarı gerekir.
Başvurmak [hızlı](quick-start.md) anahtarı nasıl edinebilirsiniz öğrenin.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Önceki adımda tamamlanmamış olursa bir gözden geçirme aracı hesabı için kaydolun

Content Moderator, Azure portalından da aldığınız varsa [gözden geçirme aracı hesabı için kaydolun](https://contentmoderator.cognitive.microsoft.com/) ve bir gözden geçirme ekibi oluşturun. Takım Kimliği ve bir iş başlatabilir ve gözden geçirmeleri gözden geçirme Aracı'nda görüntülemek için gözden geçirme API'sini çağırmak için gözden geçirme aracı ihtiyacınız var.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Gözden geçirme oluşturmak için API anahtarınızı gözden geçirme API çağrısı emin olun.

Azure Portalı'ndan başlattıysanız önceki adımları tamamladıktan sonra iki Content Moderator anahtarlarla bitirebilirsiniz. 

SDK'sı örneğinizi Azure tarafından sağlanan API anahtarı kullanmayı planlıyorsanız, belirtilen adımları izleyin [gözden geçirme API kullanarak Azure anahtarla](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) bölümünde uygulamanız gözden geçirme API çağrısı ve gözden geçirmeler oluşturmak izin vermek için.

Gözden geçirme aracı tarafından oluşturulan ücretsiz deneme sürümü anahtarı kullanırsanız, gözden geçirme aracı hesabınızı anahtarı hakkında zaten bilir ve bu nedenle, ek adımlar gereklidir.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Yeni bir **konsol uygulaması (.NET Framework)** çözümünüze bir proje.

   Örnek kodda, projeyi adlandırın **CreateReviews**.

1. Bu proje, çözüm için tek bir başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Deyimleri kullanarak program güncelleştirme

Değiştirme deyimleri kullanarak program.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisi oluşturma

Aboneliğiniz için bir Content Moderator istemcisi oluşturmak için aşağıdaki kodu ekleyin.

> [!IMPORTANT]
> Güncelleştirme **AzureRegion** ve **CMSubscriptionKey** bölge tanımlayıcısı ve abonelik anahtarınızın değerlerini.


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

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>İç içerik bilgilerini gözden geçirme kimliği ile ilişkilendirmek için bir sınıf oluşturun

Aşağıdaki sınıfa eklemek **Program** sınıfı.
Bu sınıf, öğe için dahili içerik kimliğinizin gözden geçirme kimliği ilişkilendirmek için kullanın.

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

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatmak

> [!NOTE]
> Content Moderator hizmet anahtarınız bir istek başına ikinci (RP'ler) hız sınırı vardır ve sınırını aşarsanız, SDK'sı, 429 hata koduna sahip bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS oranı sınırı vardır.

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

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Aşağıdaki sabitler ve statik alanları ekleme **Program** Program.cs sınıfında.

Aboneliğiniz ve takım için belirli bilgiler içerecek şekilde bu değerleri güncelleştirin.

> [!NOTE]
> TeamName sabiti oluştururken kullandığınız ada ayarlayın, [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) abonelik. Gelen TeamName almak **kimlik bilgilerini** konusundaki **ayarları** (dişli) menüsü.
>
> Takım adınızı değeri **kimliği** alanındaki **API** bölümü.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "YOUR REVIEW TEAM ID";

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

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Aşağıdaki statik alanları ekleme **Program** Program.cs sınıfında.

Bu alanlar, uygulama durumunu izlemek için kullanın.

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

Normalde, gelen resim, metin tanımlamaya yönelik bazı iş mantığına sahip olmak veya video, gözden geçirilmesi gerekiyor. Bununla birlikte, burada yalnızca görüntülerin sabit listesini kullanın.

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

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Mevcut incelemeleri durumunu almak için bir yöntem oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin. 

> [!Note]
> Uygulamada, geri çağırma URL'si ayarlamalı `CallbackEndpoint` URL'sine (aracılığıyla bir HTTP POST isteği) el ile İnceleme sonuçları alırsınız.
> Bu yöntem, incelemeleri bekleyen durumunu denetlemek için değiştirebilir.

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

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>İncelemeleri kümesi oluşturma ve onun durumunu denetlemek için kod ekleyin

Aşağıdaki kodu ekleyin **ana** yöntemi.

Bu kod tanımlama ve listeyi yönetmek, ek olarak ekran görüntüleri listesine kullanarak gerçekleştirdiğiniz işlemlerinin birçoğu benzetimini yapar. Günlüğe kaydetme özelliklerini Content Moderator hizmet SDK çağrıları tarafından oluşturulan yanıt nesnelerinin görmenize olanak sağlar.

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

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırın ve çıktıyı gözden geçirin

Aşağıdaki örnek çıktı görürsünüz:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Content Moderator oturum gözden ile gözden geçirin. bekleyen görüntüyü görmek için aracı **sc** etiket kümesine **true**. Ayrıca varsayılan gördüğünüz **bir** ve **r** etiketleri ve gözden geçirme aracı içinde tanımlanan herhangi bir özel etiket. 

Kullanım **sonraki** gönderme düğmesi.

![İnsan Moderatörler için resim incelemesi](images/moderation-reviews-quickstart-dotnet.PNG)

Devam etmek için herhangi bir tuşa basın.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Aşağıdaki çıktıyı günlük dosyasına göz atın.

> [!NOTE]
> Dizeleri, çıkış dosyasında "\{teamname}" ve "\{callbackUrl}" için değerleri yansıtmasına `TeamName` ve `CallbackEndpoint` alanlar, sırasıyla.

Gözden geçirme kimliği URL'leri her zaman farklıdır içerik görüntüsü, uygulamayı çalıştırın ve bir gözden geçirme olduğunda tamamlandı, `reviewerResultTags` alanı nasıl Gözden Geçiren öğeyi etiketli yansıtır.

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

## <a name="your-callback-url-if-provided-receives-this-response"></a>Sağlanırsa, bu yanıt, geri çağırma URL'sini alır

Aşağıdaki örnekte olduğu gibi bir yanıt görürsünüz:

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

Alma [Content Moderator .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar, .NET için ve tümleştirmenizi üzerinde çalışmaya başlayın.
