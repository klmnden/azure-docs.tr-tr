---
title: 'Hızlı Başlangıç: El yazısı metinleri - SDK ayıklamak,C#'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Görüntü İşleme Windows C# istemci kitaplığını kullanarak bir görüntüden metin ayıklayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/12/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: d71a566d5c6dc5505b4bd939e294f8428e9a5b93
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312918"
---
# <a name="quickstart-extract-text-using-the-computer-vision-sdk-and-c"></a>Hızlı Başlangıç: Bilgisayar işleme SDK'sını kullanarak metni ayıklayın veC#

Bu hızlı başlangıçta, el yazısı veya yazdırılan metin için bilgisayar işleme SDK'sını kullanarak bir görüntüden ayıklayacaktır C#. İsterseniz, bu kılavuzdaki kod tam bir örnek uygulamadan olarak indirebilirsiniz [Bilişsel hizmetler Csharp görüntü](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision) github deposu.

## <a name="prerequisites"></a>Önkoşullar

* Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).
* [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
* [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) istemci kitaplığı NuGet paketi. Paketi indirmek gerekli değildir. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="create-and-run-the-sample-app"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Görüntü İşleme istemci kitaplığı NuGet paketini yükleyin.
    1. Menüde **Araçlar**’a tıklayın, **NuGet Paket Yöneticisi**’ni ve ardından **Çözüm için NuGet Paketlerini Yönet**’i seçin.
    1. **Gözat** sekmesine tıklayın ve **Arama** kutusuna "Microsoft.Azure.CognitiveServices.Vision.ComputerVision" yazın.
    1. Görüntülendiğinde **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** öğesini seçin ve projenizin adının yanındaki onay kutusuna tıklayıp **Yükle**’ye tıklayın.
1. `Program.cs` öğesini aşağıdaki kodla değiştirin. `RecognizeTextAsync` ve `RecognizeTextInStreamAsync` metotları sırasıyla uzak ve yerel görüntüler için [Metin Tanıma API’sini](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) sarmalar. `GetTextOperationResultAsync` metodu [Metin Tanıma İşlemi Sonuçlarını Alma API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) sarmalar.

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

    using System;
    using System.IO;
    using System.Threading.Tasks;

    namespace ExtractText
    {
        class Program
        {
            // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
            private const string subscriptionKey = "<SubscriptionKey>";

            // For printed text, change to TextRecognitionMode.Printed
            private const TextRecognitionMode textRecognitionMode =
                TextRecognitionMode.Handwritten;

            // localImagePath = @"C:\Documents\LocalImage.jpg"
            private const string localImagePath = @"<LocalImage>";

            private const string remoteImageUrl =
                "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
                "Cursive_Writing_on_Notebook_paper.jpg/" +
                "800px-Cursive_Writing_on_Notebook_paper.jpg";

            private const int numberOfCharsInOperationId = 36;

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
                var t1 = ExtractRemoteTextAsync(computerVision, remoteImageUrl);
                var t2 = ExtractLocalTextAsync(computerVision, localImagePath);

                Task.WhenAll(t1, t2).Wait(5000);
                Console.WriteLine("Press ENTER to exit");
                Console.ReadLine();
            }

            // Recognize text from a remote image
            private static async Task ExtractRemoteTextAsync(
                ComputerVisionClient computerVision, string imageUrl)
            {
                if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
                {
                    Console.WriteLine(
                        "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                    return;
                }

                // Start the async process to recognize the text
                RecognizeTextHeaders textHeaders =
                    await computerVision.RecognizeTextAsync(
                        imageUrl, textRecognitionMode);

                await GetTextAsync(computerVision, textHeaders.OperationLocation);
            }

            // Recognize text from a local image
            private static async Task ExtractLocalTextAsync(
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
                    // Start the async process to recognize the text
                    RecognizeTextInStreamHeaders textHeaders =
                        await computerVision.RecognizeTextInStreamAsync(
                            imageStream, textRecognitionMode);

                    await GetTextAsync(computerVision, textHeaders.OperationLocation);
                }
            }

            // Retrieve the recognized text
            private static async Task GetTextAsync(
                ComputerVisionClient computerVision, string operationLocation)
            {
                // Retrieve the URI where the recognized text will be
                // stored from the Operation-Location header
                string operationId = operationLocation.Substring(
                    operationLocation.Length - numberOfCharsInOperationId);

                Console.WriteLine("\nCalling GetHandwritingRecognitionOperationResultAsync()");
                TextOperationResult result =
                    await computerVision.GetTextOperationResultAsync(operationId);

                // Wait for the operation to complete
                int i = 0;
                int maxRetries = 10;
                while ((result.Status == TextOperationStatusCodes.Running ||
                        result.Status == TextOperationStatusCodes.NotStarted) && i++ < maxRetries)
                {
                    Console.WriteLine(
                        "Server status: {0}, waiting {1} seconds...", result.Status, i);
                    await Task.Delay(1000);

                    result = await computerVision.GetTextOperationResultAsync(operationId);
                }

                // Display the results
                Console.WriteLine();
                var lines = result.RecognitionResult.Lines;
                foreach (Line line in lines)
                {
                    Console.WriteLine(line.Text);
                }
                Console.WriteLine();
            }
        }
    }
    ```

1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `computerVision.Endpoint` değerini abonelik anahtarlarınız ile ilişkili Azure bölgesi ile değiştirin.
1. İsterseniz `textRecognitionMode` değerini `TextRecognitionMode.Printed` olarak ayarlayabilirsiniz.
1. `<LocalImage>` değerini yerel görüntünün yolu ve dosya adıyla değiştirin.
1. İsteğe bağlı olarak, `remoteImageUrl` öğesini farklı bir görüntüye ayarlayın.
1. Programı çalıştırın.


## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt her görüntü için tanınan metin satırlarını yazdırır.

```console
Calling GetHandwritingRecognitionOperationResultAsync()

Calling GetHandwritingRecognitionOperationResultAsync()
Server status: Running, waiting 1 seconds...
Server status: Running, waiting 1 seconds...

dog
The quick brown fox jumps over the lazy
Pack my box with five dozen liquor jugs
```

Bkz: [hızlı başlangıç: El yazısı metinleri - REST, ayıklamak C# ](../QuickStarts/CSharp-hand-text.md#examine-the-response) API çağrısından ham JSON çıktısını gösteren bir örnek.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
