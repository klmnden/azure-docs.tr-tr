---
title: Azure Content Moderator - .NET kullanarak başlangıç denetimi işleri | Microsoft Docs
description: .NET için Azure Content Moderator SDK'sını kullanarak denetimi işleri başlatma
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: 3761552f81bd733f9c93fab40db07ef6f5a6a7f6
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181610"
---
# <a name="start-moderation-jobs-using-net"></a>.NET kullanarak denetimi işleri Başlat

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri, kullanmaya başlama [Content Moderator SDK'sı .NET için](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için:
 
- Tarama ve gözden geçirmeler için İnsan Moderatörler oluşturmak için bir denetimi işi başlatma
- Bekleyen durumunu alın
- İzleme ve gözden geçirmeyi son durumunu alın
- Geri çağırma URL'si için sonucu Gönder

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

## <a name="define-a-custom-moderation-workflow"></a>Özel denetimi iş akışı tanımlama

Bir denetimi işi kullanır ve API kullanarak içeriğinizi tarar bir **iş akışı** incelemeleri oluşturup belirlemek için.
İnceleme aracını varsayılan iş akışı şimdi içerirken [özel bir iş akışını tanımlayan](Review-Tool-User-Guide/Workflows.md) Bu Hızlı Başlangıç için.

Kodunuzda denetimi işini başlatır'de iş akışı adını kullanın.

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

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatmak

Aşağıdaki sabitler ve statik alanları ekleme **Program** Program.cs sınıfında.

> [!NOTE]
> TeamName sabiti Content Moderator aboneliğinizi oluştururken kullandığınız ada ayarlayın. Gelen TeamName almak [Content Moderator web sitesi](https://westus.contentmoderator.cognitive.microsoft.com/).
> Oturum açtıktan sonra seçin **kimlik bilgilerini** gelen **ayarları** (dişli) menüsü.
>
> Takım adınızı değeri **kimliği** alanındaki **API** bölümü.


    /// <summary>
    /// The moderation job will use this workflow that you defined earlier.
    /// See the quickstart article to learn how to setup custom workflows.
    /// </summary>
    private const string WorkflowName = "OCR";
    
    /// <summary>
    /// The name of the team to assign the job to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Content Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "***";

    /// <summary>
    /// The URL of the image to create a review job for.
    /// </summary>
    private const string ImageUrl =
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png";

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Otomatik-Orta için kod ekleyin, bir gözden geçirme oluşturmak ve iş ayrıntılarını Al

> [!Note]
> Uygulamada, geri çağırma URL'sini ayarlayın **CallbackEndpoint** URL'sine (aracılığıyla bir HTTP POST isteği) el ile İnceleme sonuçlarını alır.

Başlamak için aşağıdaki kodu ekleyerek **ana** yöntemi.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the name of the workflow defined in the online review tool.
            // See the quickstart article to learn more.
            var jobResult = client.Reviews.CreateJobWithHttpMessagesAsync(
                    TeamName, "image", "contentID", WorkflowName, "application/json", content, CallbackEndpoint);

            // Record the job ID.
            var jobId = jobResult.Result.Body.JobIdProperty;

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                jobResult.Result.Body, Formatting.Indented));

            Thread.Sleep(2000);
            writer.WriteLine();

            writer.WriteLine("Get review job status.");
            var jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
                    TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                    jobDetails.Result.Body, Formatting.Indented));

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propagate.");
            Thread.Sleep(latencyDelay * 1000);

            writer.WriteLine("Get review details.");
            jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
            TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
            jobDetails.Result.Body, Formatting.Indented));
        }
        writer.Flush();
        writer.Close();
    }

> [!NOTE]
> Content Moderator hizmeti anahtarınızı ikinci (RP'ler) hız sınırı başına bir istek var. Sınırı aşarsanız, SDK'sı 429 hata koduna sahip özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS oranı sınırı vardır.

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırın ve çıktıyı gözden geçirin

Konsolunda aşağıdaki örnek çıktıyı görürsünüz:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Content Moderator oturum gözden bekleyen görüntüyü görmek için aracı gözden geçirin.

Kullanım **sonraki** gönderme düğmesi.

![İnsan Moderatörler için resim incelemesi](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Örnek çıktı günlük dosyasına bakın

> [!NOTE]
> Çıkış dosyanızda, dizeleri **Teamname**, **ContentID**, **CallBackEndpoint**, ve **Workflowıd** kullandığınız değerleri yansıtır daha önce.

    Create moderation job for an image.
    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

    Get review details.
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": [
        {
            "Ts": "2018-01-07T00:38:26.7714671",
            "Msg": "Successfully got hasText response from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:26.4181346",
            "Msg": "Getting hasText from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:25.5122828",
            "Msg": "Starting Execution - Try 1"
        }
        ]
    }


## <a name="your-callback-url-if-provided-receives-this-response"></a>Geri çağırma URL'nizi sağlanırsa, bu yanıt alır.

Aşağıdaki örnekte olduğu gibi bir yanıt görürsünüz:

> [!NOTE]
> Dizeleri, geri çağırma yanıt **ContentID** ve **Workflowıd** daha önce kullanılan değerleri yansıtır.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe",
        "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
        "WorkFlowId": "OCR",
        "Status": "Complete",
        "ContentType": "Image",
        "CallBackType": "Job",
        "ContentId": "contentID",
        "Metadata": {
            "hastext": "True",
            "ocrtext": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
            "imagename": "contentID"
        }
    }


## <a name="next-steps"></a>Sonraki adımlar

Alma [Content Moderator .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar, .NET için ve tümleştirmenizi üzerinde çalışmaya başlayın.
