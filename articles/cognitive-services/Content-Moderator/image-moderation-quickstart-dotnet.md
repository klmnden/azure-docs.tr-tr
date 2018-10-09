---
title: 'Hızlı Başlangıç: .NET kullanarak görüntü denetleme - Content Moderator'
titlesuffix: Azure Cognitive Services
description: .NET için Content Moderator SDK'sını kullanarak görüntü denetleme
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: d89d9b8a2e3b00155e82cc28105007ab39fc549c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226173"
---
# <a name="quickstart-moderate-images-using-net"></a>Hızlı Başlangıç: .NET kullanarak görüntü denetleme

Bu makalede, aşağıdaki amaçlarla [.NET için Content Moderator SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)'nı kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanmaktadır: 

- Bir görüntüyü yetişkinlere yönelik veya müstehcen içerik için denetleme
- Bir görüntüdeki metni algılama ve ayıklama
- Bir görüntüdeki yüzleri algılama

Bu makale, Visual Studio ve C# hakkında bilgi sahibi olduğunuzu varsayar.

## <a name="sign-up-for-content-moderator-services"></a>Content Moderator hizmetleri için kaydolma

REST API veya SDK aracılığıyla Content Moderator hizmetlerini kullanabilmeniz için önce bir API anahtarı ve API hesabınızın bölgesi gerekir.
Bunları edinmek için, Content Moderator'a kaydolmayı öğreten [Hızlı Başlangıç](quick-start.md)'a başvurun.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturma

1. Çözümünüze yeni bir **Console uygulaması (.NET Framework)** projesi ekleyin.

   Örnek kodda projeyi **ImageModeration** olarak adlandırın.

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

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatma

Aşağıdaki statik alanları Program.cs dosyasındaki **Program** sınıfına ekleyin.

    ///<summary>
    ///The name of the file that contains the image URLs to evaluate.
    ///</summary>
    ///<remarks>You will need to create an input file and update 
    ///this path accordingly. Paths are relative to the execution directory.
    ///</remarks>
    private static string ImageUrlFile = "ImageFiles.txt";

    ///<summary>
    ///The name of the file to contain the output from the evaluation.
    ///</summary>
    ///<remarks>Paths are relative to the execution directory.
    ///</remarks>
    private static string OutputFile = "ModerationOutput.json";


> [!NOTE]
> Örnek, bu hızlı başlangıcın çıktısını üretmek için aşağıdaki görüntüleri kullanır.
> - https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
> - https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png

## <a name="store-the-analysis-results"></a>Analiz sonuçlarını depolama

Aşağıdaki sınıfı **Program** sınıfına ekleyin. Gözden geçirilen görüntülerin denetim sonuçlarını kaydetmek için bu sınıfın bir örneğini kullanın.

    /// <summary>
    /// Contains the image moderation results for an image, 
    /// including text and face detection results.
    /// </summary>
    public class EvaluationData
    {
        /// <summary>
        /// The URL of the evaluated image.
        /// </summary>
        public string ImageUrl;

        /// <summary>
        /// The image moderation results.
        /// </summary>
        public Evaluate ImageModeration;

        /// <summary>
        /// The text detection results.
        /// </summary>
        public OCR TextDetection;

        /// <summary>
        /// The face detection results;
        /// </summary>
        public FoundFaces FaceDetection;
    }

## <a name="evaluate-an-individual-image"></a>Tek bir görüntüyü değerlendirme

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, tek bir görüntüyü değerlendirir ve değerlendirme sonuçlarını döndürür.

> [!NOTE]
> Content Moderator hizmet anahtarınızın saniye başına istek (RPS) hız sınırı vardır ve bu sınırı aşarsanız SDK 429 hata kodu ile bir özel durum oluşturur. 
>
> Ücretsiz katman anahtarı bir RPS'lik hız sınırına sahiptir.


    /// <summary>
    /// Evaluates an image using the Image Moderation APIs.
    /// </summary>
    /// <param name="client">The Content Moderator API wrapper to use.</param>
    /// <param name="imageUrl">The URL of the image to evaluate.</param>
    /// <returns>Aggregated image moderation results for the image.</returns>
    /// <remarks>This method throttles calls to the API.
    /// Your Content Moderator service key will have a requests per second (RPS)
    /// rate limit, and the SDK will throw an exception with a 429 error code 
    /// if you exceed that limit. A free tier key has a 1 RPS rate limit.
    /// </remarks>
    private static EvaluationData EvaluateImage(
    ContentModeratorClient client, string imageUrl)
    {
        var url = new ImageUrl("URL", imageUrl.Trim());

        var imageData = new EvaluationData();

        imageData.ImageUrl = url.Value;

        // Evaluate for adult and racy content.
        imageData.ImageModeration =
            client.ImageModeration.EvaluateUrlInput("application/json", url, true);
        Thread.Sleep(1000);

        // Detect and extract text.
        imageData.TextDetection =
            client.ImageModeration.OCRUrlInput("eng", "application/json", url, true);
        Thread.Sleep(1000);

        // Detect faces.
        imageData.FaceDetection =
            client.ImageModeration.FindFacesUrlInput("application/json", url, true);
        Thread.Sleep(1000);

        return imageData;
    }

**EvaluateUrlInput** yöntemi, Görüntü Denetim REST API'si için bir sarmalayıcıdır.
Döndürülen değer, API çağrısından döndürülen nesneyi içerir.

**OCRUrlInput** yöntemi, Görüntü OCR REST API'si için bir sarmalayıcıdır.
Döndürülen değer, API çağrısından döndürülen nesneyi içerir.

**FindFacesUrlInput** yöntemi, Görüntüde Yüzler Bulma REST API'si için bir sarmalayıcıdır.
Döndürülen değer, API çağrısından döndürülen nesneyi içerir.

## <a name="process-the-image-urls-in-your-code"></a>Görüntü URL'lerini kodunuzda işleme

Aşağıdaki kodu **Main** yöntemine ekleyin.

    // Create an object to store the image moderation results.
    List<EvaluationData> evaluationData = new List<EvaluationData>();

    // Create an instance of the Content Moderator API wrapper.
    using (var client = Clients.NewClient())
    {
        // Read image URLs from the input file and evaluate each one.
        using (StreamReader inputReader = new StreamReader(ImageUrlFile))
        {
            while (!inputReader.EndOfStream)
            {
                string line = inputReader.ReadLine().Trim();
                if (line != String.Empty)
                {
                    EvaluationData imageData = EvaluateImage(client, line);
                    evaluationData.Add(imageData);
                }
            }
        }
    }

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        outputWriter.WriteLine(JsonConvert.SerializeObject(
            evaluationData, Formatting.Indented));

        outputWriter.Flush();
        outputWriter.Close();
    }

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

Aşağıdaki JSON nesnesi program çıktısını içerir.

> [!NOTE]
> `isImageAdultClassified`, belirli durumlarda açık saçık veya yetişkinlere yönelik olarak değerlendirilebilecek görüntülerin mevcut olma olasılığını gösterir.
> `isImageRacyClassified`, belirli durumlarda davetkar veya yetişkinlere yönelik olarak değerlendirilebilecek görüntülerin mevcut olma olasılığını gösterir.
>

    [
    {
    "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
    "ImageModeration": {
      "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
      "result": false,
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "116"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "273405"
        }
      ],
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      }
    },
    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "metadata": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "12"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "273405"
        }
      ],
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
      "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },
    "FaceDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
      "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
      "result": false,
      "count": 0,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "11"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "273405"
        }
      ],
      "faces": []
    }
    },
    {
    "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
    "ImageModeration": {
      "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
      "result": false,
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
      "adultClassificationScore": 0.0035635426174849272,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.021369094029068947,
      "isImageRacyClassified": false,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "109"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "2278902"
        }
      ],
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      }
    },
    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "metadata": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "46"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "2278902"
        }
      ],
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
      "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
      "language": "eng",
      "text": "",
      "candidates": []
    },
    "FaceDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
      "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
      "result": true,
      "count": 6,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "60"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "2278902"
        }
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        },
        {
          "bottom": 575,
          "left": 594,
          "right": 773,
          "top": 396
        },
        {
          "bottom": 563,
          "left": 812,
          "right": 955,
          "top": 420
        },
        {
          "bottom": 611,
          "left": 972,
          "right": 1151,
          "top": 432
        },
        {
          "bottom": 510,
          "left": 1232,
          "right": 1456,
          "top": 286
        }
      ]
    }
    }
    ]


## <a name="next-steps---get-the-source-code"></a>Sonraki adımlar - kaynak kodunu alma

Bu ve diğer .NET için Content Moderator hızlı başlangıçları için [Content Moderator .NET SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümünü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) alın ve tümleştirmeniz üzerinde çalışmaya başlayın.
