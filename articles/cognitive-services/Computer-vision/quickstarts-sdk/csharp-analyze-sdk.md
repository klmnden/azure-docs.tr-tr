---
title: 'Hızlı Başlangıç: Görüntü - SDK çözümlemek,C#'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Görüntü İşleme Windows C# istemci kitaplığını kullanarak bir görüntüyü analiz edeceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a88f1d0b2290bc4f5f52827960ea3bed94053c62
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60782030"
---
# <a name="quickstart-analyze-an-image-using-the-computer-vision-sdk-and-c"></a>Hızlı Başlangıç: Bilgisayar işleme SDK'sını kullanarak bir resmi çözümleme veC#

Bu hızlı başlangıçta, hem yerel hem de uzak bir görüntü için görüntü işleme istemci kitaplığı kullanarak görsel özellikleri ayıklamak için analiz eder C#. İsterseniz, bu kılavuzdaki kod tam bir örnek uygulamadan olarak indirebilirsiniz [Bilişsel hizmetler Csharp görüntü](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision) github deposu.

## <a name="prerequisites"></a>Önkoşullar

* Bir görüntü işleme abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.
* [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
* [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) istemci kitaplığı NuGet paketi. Paketi indirmek gerekli değildir. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="create-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Görüntü İşleme istemci kitaplığı NuGet paketini yükleyin.
    1. Menüde **Araçlar**’a tıklayın, **NuGet Paket Yöneticisi**’ni ve ardından **Çözüm için NuGet Paketlerini Yönet**’i seçin.
    1. **Gözat** sekmesine tıklayın ve **Arama** kutusuna "Microsoft.Azure.CognitiveServices.Vision.ComputerVision" yazın.
    1. Görüntülendiğinde **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** öğesini seçin ve projenizin adının yanındaki onay kutusuna tıklayıp **Yükle**’ye tıklayın.
1. Öğesinin içeriğini değiştirin *Program.cs* aşağıdaki kod ile. `AnalyzeImageAsync` Ve `AnalyzeImageInStreamAsync` yöntemleri kaydırma [görüntü REST API'analiz](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) uzak ve yerel görüntüler için sırasıyla.

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading.Tasks;

    namespace ImageAnalyze
    {
        class Program
        {
            // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
            private const string subscriptionKey = "<SubscriptionKey>";

            // localImagePath = @"C:\Documents\LocalImage.jpg"
            private const string localImagePath = @"<LocalImage>";

            private const string remoteImageUrl =
                "https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

            // Specify the features to return
            private static readonly List<VisualFeatureTypes> features =
                new List<VisualFeatureTypes>()
            {
                VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
                VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
                VisualFeatureTypes.Tags
            };

            static void Main(string[] args)
            {
                ComputerVisionClient computerVision = new ComputerVisionClient(
                    new ApiKeyServiceClientCredentials(subscriptionKey),
                    new System.Net.Http.DelegatingHandler[] { });

                // You must use the same region as you used to get your subscription
                // keys. For example, if you got your subscription keys from westus,
                // replace "westcentralus" with "westus".
                //
                // Free trial subscription keys are generated in the "westus"
                // region. If you use a free trial subscription key, you shouldn't
                // need to change the region.

                // Specify the Azure region
                computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

                Console.WriteLine("Images being analyzed ...");
                var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
                var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

                Task.WhenAll(t1, t2).Wait(5000);
                Console.WriteLine("Press ENTER to exit");
                Console.ReadLine();
            }

            // Analyze a remote image
            private static async Task AnalyzeRemoteAsync(
                ComputerVisionClient computerVision, string imageUrl)
            {
                if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
                {
                    Console.WriteLine(
                        "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                    return;
                }

                ImageAnalysis analysis =
                    await computerVision.AnalyzeImageAsync(imageUrl, features);
                DisplayResults(analysis, imageUrl);
            }

            // Analyze a local image
            private static async Task AnalyzeLocalAsync(
                ComputerVisionClient computerVision, string imagePath)
            {
                if (!File.Exists(imagePath))
                {
                    Console.WriteLine(
                        "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                    return;
                }

                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                        imageStream, features);
                    DisplayResults(analysis, imagePath);
                }
            }

            // Display the most relevant caption for the image
            private static void DisplayResults(ImageAnalysis analysis, string imageUri)
            {
                Console.WriteLine(imageUri);
                if (analysis.Description.Captions.Count != 0)
                {
                    Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
                }
                else
                {
                    Console.WriteLine("No description generated.");
                }
            }
        }
    }
    ```

1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `computerVision.Endpoint` değerini abonelik anahtarlarınız ile ilişkili Azure bölgesi ile değiştirin.
1. `<LocalImage>` değerini yerel görüntünün yolu ve dosya adıyla değiştirin.
1. İsteğe bağlı olarak, `remoteImageUrl` için farklı bir resim URL'si.
1. Programı çalıştırın.

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıtta her görüntü için en ilgili resim yazısı görüntülenecektir. Değiştirebileceğiniz `DisplayResults` farklı çıkış yöntemi görüntü veri. Bkz: [AnalyzeLocalAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions.analyzeimageinstreamasync?view=azure-dotnet) daha fazla bilgi için yöntemi.

Bkz: [API'si hızlı başlangıç: Yerel bir görüntü ile analiz C# ](../QuickStarts/CSharp-analyze.md#examine-the-response) ham JSON örneği için çıktı.

```console
https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
